# FCSC 2022 - À l'aise

## Informations

- Platform: Hackropole
- Event: FCSC 2022
- Category: Cryptography
- Difficulty: Intro
- State: In progress
## Objective

Decrypt the provided message using the Blaise de Vigénère method. Use the key provided.

## Provided informations

- We know the name of the algorithm used for the encryption process: Chiffre de Vigénère
- We know the key used to encrypt the message: "FCSC"
- We have the encrypted message:
```
Gqfltwj emgj clgfv ! Aqltj rjqhjsksg ekxuaqs, ua xtwk
n'feuguvwb gkwp xwj, ujts f'npxkqvjgw nw tjuwcz
ugwygjtfkf qz uw efezg sqk gspwonu. Jgsfwb-aqmu f
Pspygk nj 29 cntnn hqzt dg igtwy fw xtvjg rkkunqf.
```

## Understanding the "Chiffre de Vigénère"

The other encryption algorithm I know of is the Caesar Cipher. It shifts letters by a fixed number of positions in the alphabet.
If the alphabet were stored in a list, with A at position 0, a Caesar Cipher with a shift of 5 would transform A into F: 

```
A [0] -> F [5]
```

Caesar uses one constant shift, meaning the key is the same as the shift. For Vigénère it seems the shift changes according to the key (repeated if the message is longer than the key).

For Caesar Cipher with a shift of 5:
```
Message           -> A B C D E
Key/Shift         -> 5 5 5 5 5
Encrypted Message -> F G H I J
```
If we want to encrypt the message "HELLOWORLD" (we will only use letters for this example) using the Caesar Cipher with a shift of 5:
```Python
alphabet = "abcdefghijklmnopqrstuvwxyz"
def encrypt_caesar(message, key):
	encrypted_message = []
	for char in message:
		if char not in alphabet:
			encrypted_message.append(char)
			continue
		original_position = alphabet.index(char)
		shifted_position = (original_position + key) % 26
		new_char = alphabet[shifted_position]
		encrypted_message.append(new_char)
	return "".join(encrypted_message)
>>> encrypt_caesar("hello world", 5)
'mjqqt btwqi'
```
If we want to decrypt a message using the same method, we just have to go the other way around by changing shifted_position value from "+" to "-"
```Python
alphabet = "abcdefghijklmnopqrstuvwxyz"
def decrypt_caesar(message, key):
	decrypted_message = []
	for char in message:
		if char not in alphabet:
			decrypted_message.append(char)
			continue
		original_position = alphabet.index(char)
		shifted_position = (original_position - key) % 26
		new_char = alphabet[shifted_position]
		decrypted_message.append(new_char)
	return "".join(decrypted_message)
>>> decrypt_caesar("mjqqt btwqi", 5)
'hello world'
```
For Vigénère with a shift of "KEY":
```
Message         -> H E L L O W O R L D
Key             -> K E Y K E Y K E Y K
Shift           ->10 4 24 10 4 24 10 4 24 10
```
As we can see, it is the same underlying mechanism as a Caesar Cipher. The difference is that the shift changes for each letter, according to the key used. Each letter of the key represents a different Caesar shift.
We can use our earlier code: by changing each letter of the key into a number, we can then use those numbers as a shift, and loop through the message

```Python
alphabet = "abcdefghijklmnopqrstuvwxyz"
def decrypt_vigenere(message, key):
	message = message.lower()
    key = key.lower()
	decrypted_message = []
	key_index = 0
	for char in message:
		if char not in alphabet:
			decrypted_message.append(char)
			continue
		key_char = key[key_index]
		shift_amount = alphabet.find(key_char)
		original_position = alphabet.index(char)
		new_position = (original_position - shift_amount) % 26
		new_char = alphabet[new_position]
		decrypted_message.append(new_char)
		key_index = (key_index + 1) % len(key)
	return "".join(decrypted_message)
```