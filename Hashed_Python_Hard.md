# Solution for [Hashed](https://trycrack.me/chall?ch=44)

Name: Hashed
Language: Python
Difficulty: Hard
Solves: 6

### Steps to Solve the Challange
1. Copy the code to a file and save it as `hashed.py`
2. Analyse the code. Note out the key points.
3. As per the following lines, we can understand that the `custom_hash` function is actually making use of the `sha256` hash.
```python
def custom_hash(text):
    sha256 = hashlib.sha256()
    sha256.update(text.encode('utf-8'))
    return sha256.hexdigest()
```
4. Also at the beginning of the `verify_key` function, we see that the code actually checks a certain condition
```python
    if len(key) != 8 and (key[3] and key[4] != '-'):
        return False
```
5. So by the above, we can assume that the key is 8 characters long and the 3rd (4th because python counts from 0) and 4th (5th) digits characters should be -
