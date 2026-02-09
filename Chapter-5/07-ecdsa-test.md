ECDSA Test Example

Listing 5.9: ECDSA signing example

What it does
- Signs a message using the private key.

Python
```python
msg = read_message("sign_test.txt")
sig = ecdsa_sign(sk, msg, base)
```

MATLAB
```matlab
msg = read_message('sign_test.txt');
sig = ecdsa_sign(sk, msg, base);
```

Listing 5.10: ECDSA verify example

What it does
- Verifies the signature using the public key and message.

Python
```python
print(ecdsa_verify(sig, pk, msg, base))
```

MATLAB
```matlab
disp(ecdsa_verify(sig, pk, msg, base));
```

Expected result
- Verification prints `True` when the message is unchanged.
