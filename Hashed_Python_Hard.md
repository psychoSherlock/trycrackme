# Solution for [Hashed](https://trycrack.me/chall?ch=44)

Name: Hashed<br>
Language: Python<br>
Difficulty: Hard<br>
Solves: 6<br>

### Analysing the code
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
6. And from the below lines, we can understand that the hashes are split into two parts, first 0 to 4th character and then the 4th to the last character
```python
   part1 = key[:4]
    part2 = key[4:]
    
    hash1 = custom_hash(part1)
    hash2 = custom_hash(part2)
```
7. And then from the remaining lines, both the parts are compared to two hardcoded hashes. Which means that both the parts of the key, when hashed, should become the hardcoded hash for solving the challange.

### The Solution

Ok so from all our above code analysis we can conclude that an example key must be like this: `XXX--YYY`. This is 8 characters long and the 4th and 5th characters are -.
Also when the key is splitted, part1 is actually `XXX-` and part2 is actually `-YYY`.
- This makes it easier to crack because the actually length of the key is 3 characters and the remaining are hardcoded.
- To solve this, all one have to do is to make a list of 3 digit characters with a - infront of it and another one with a - after it.
- Using one of the list, one can just paste the hardcoded hash into a file and use `hashcat` to convert the wordlist into `sha256` and find the correct part1 and part2 and finally join it.
- To make a list of characters use the following python code:
```python
import itertools

# Define the characters, numbers, and symbols you want to include
characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'

# Generate all 3-digit combinations
combinations = itertools.product(characters, repeat=3)

# Create and write the combinations to the file
with open("part1_chars.txt", "w") as file:
    for combo in combinations:
        # Join the combination elements into a string and append a hyphen
        line = '-' + ''.join(combo) + '\n'
        file.write(line)

print("File 'part1_chars.txt' has been generated.")
```
And  another one
```python
import itertools

# Define the characters, numbers, and symbols you want to include
characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'

# Generate all 3-digit combinations
combinations = itertools.product(characters, repeat=3)

# Create and write the combinations to the file
with open("part2_chars.txt", "w") as file:
    for combo in combinations:
        # Join the combination elements into a string and append a hyphen
        line = ''.join(combo) + '-\n'
        file.write(line)

print("File 'part2_chars.txt' has been generated.")
```
- Then paste the two hashes into a file called `hash1.txt` and `hash2.txt`
- Use hashcat or jtr to solve the challange, by running `sudo hashcat -a 0 -m 1400 hash1.txt part1_chars.txt` and `sudo hashcat -a 0 -m 1400 hash2.txt part2_chars.txt`
- Join the parts to get the key

💙 [psychoSherlock](https://www.linkedin.com/in/athul-prakash-nj-564a80226/)
