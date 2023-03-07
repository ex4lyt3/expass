# ExPass Password Manager
Version 2.1

## A highly inefficient command-line password manager that should not be used for actual purposes
Scripted in `bash`

Uses openssl to encrypt password files with AES-256

## Questions
**Why is it inefficient?**

Well, it currently uses multiple decryptions/encryptions with openssl per command which could be optimised to reduce the number to one

**Why is it bad?**

Multiple reasons. First, the key generated is not salted which increases the potential of brute force. Second, it is highly inefficient. Third, it is just (less) bad (in v2.1).

**How does it work?**

It is an index-based system. Seperate password entries are stored as a file and its location is marked by an index file. For decryption, the index is first decrypted to get the location of the encrypted entry and it uses the index to decrypt the entry. 
Key verification sort of works like HMAC where the hash of the generated key is compared to a file which contains the correct hash.

**Why did I make this?**

Learn bash (This is my first bash project)

edit: turns out i am dumb with openssl. im pretty sure its more simple to encrypt and decrypt the password entry and index using `openssl enc -aes-256-cbc -pbkdf2 -in password -out password.enc` then `key=$(openssl enc -nosalt -aes-256-cbc -pbkdf2 -k $masterPassword -P | grep "key")
    iv=$(openssl enc -nosalt -aes-256-cbc -pbkdf2 -k $masterPassword -P | grep "iv")
    openssl enc -d -nosalt -aes-256-cbc -pbkdf2 -in $mainDir/data/index.enc -out $mainDir/data/index -base64 -K "${key:4:64}" -iv "${iv:4:32}"` where the former is also encrypted with a salt??? 