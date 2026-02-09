Schnorr Test Example

Listing 5.4: Test message input

What it does
- Reads a file into a byte buffer for signing and verification.

Python
```python
def read_message(path):
    with open(path, "rb") as f:
        return f.read()
```

MATLAB
```matlab
function msg = read_message(path)
    fid = fopen(path, 'rb');
    if fid < 0
        error('file not found');
    end
    msg = fread(fid, Inf, '*uint8')';
    fclose(fid);
end
```

Listing 5.5: Schnorr test

What it does
- Signs a message and verifies it with the public key.

Python
```python
msg = read_message("sign_test.txt")
sig = schnorr_sign(sk, msg, base)
print(schnorr_verify(sig, pk, msg, base))
```

MATLAB
```matlab
msg = read_message('sign_test.txt');
sig = schnorr_sign(sk, msg, base);
disp(schnorr_verify(sig, pk, msg, base));
```

Expected result
- Verification prints `True` when the message is unchanged.
