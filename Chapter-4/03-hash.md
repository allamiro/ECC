Hash to Finite Field

We use a hash function to turn a pass phrase into a field element. The C code uses
KangarooTwelve (K12). Here we provide a compatible interface but use SHAKE256
as a drop-in XOF. You can replace it with a K12 binding if desired.

Listing 4.1: Hash to finite field

Python
```python
import hashlib


def hash_to_field(data_bytes, modulus, dst=b"Hash_b pring&sig"):
    # XOF: SHAKE256 (replace with K12 if available)
    m = modulus.bit_length()
    if m < 208:
        k = 80
    elif m < 320:
        k = 128
    elif m < 448:
        k = 192
    else:
        k = 256
    b = m + k
    out_len = (b + 7) // 8
    shake = hashlib.shake_256()
    shake.update(data_bytes + dst)
    outp = shake.digest(out_len)
    hsh = int.from_bytes(outp, "little")
    return hsh % modulus
```

MATLAB
```matlab
function hsh = hash_to_field(data, modulus)
    % XOF using SHA-256 counter mode (replace with K12 if available)
    % data: uint8 array
    dst = uint8('Hash_b pring&sig');
    m = floor(log2(modulus)) + 1;
    if m < 208
        k = 80;
    elseif m < 320
        k = 128;
    elseif m < 448
        k = 192;
    else
        k = 256;
    end
    b = m + k;
    out_len = ceil(b / 8);

    outp = uint8([]);
    ctr = uint32(0);
    while numel(outp) < out_len
        ctr_bytes = typecast(ctr, 'uint8');
        block = sha256_bytes([data, dst, ctr_bytes]);
        outp = [outp, block]; %#ok<AGROW>
        ctr = ctr + 1;
    end
    outp = outp(1:out_len);
    hsh = mod(bytes_to_int_le(outp), modulus);
end

function out = sha256_bytes(msg)
    md = java.security.MessageDigest.getInstance('SHA-256');
    md.update(int8(msg));
    out = typecast(md.digest(), 'uint8')';
end

function x = bytes_to_int_le(bytes)
    x = uint64(0);
    for i = numel(bytes):-1:1
        x = bitshift(x, 8) + uint64(bytes(i));
    end
    x = double(x);
end
```

How to run (example)

Python
```python
modulus = 10177
val = hash_to_field(b"hello", modulus)
print(val)
```

MATLAB
```matlab
modulus = 10177;
val = hash_to_field(uint8('hello'), modulus);
disp(val);
```

Expected result
- A deterministic integer in the range $[0, modulus-1]$.
