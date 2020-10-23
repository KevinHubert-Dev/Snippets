# Snippets
Here you can find some of my snippets i used for some situation i needed them.


## FCrackZip + Crunch - Open a password protected zip file
I had the situation were i've received some zip-files which were quite limited in the complexity of their password.
Due to the fact that i lost the password provided by the sender and only remember the used characters i've found the following-solution to get the password-protected file opened without having the password (using bruteforce)
```
# Generate file which contain all combination of possible characters (Possible_Chars ^ Password-Length)
# In my case is was only the upper-case-character A-D and the &-sign at a length of 10
# crunch <min-length> <max-length> <chars>
crunch 10 10 \&ABCD

# Use fcrackzip with the generated wordlist to brute-force all possible characters
fcrackzip -v -u -D -p <path-to-wordlist> <path-to-zip-file>
```

Note: fcrackzip itself contains a option to use generated brute-force words to open the file but i think crunch was easier for my special user-case with only a set of given characters
