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

-------------

## SSH authentication using privat-/public key (ED25519 || RSA)
```
# Generate public-privat-key-pair
ssh-keygen -o -a 200 -t ed25519 -f id_ed25519
# To use RSA instead of ED25519 just replace "-t ed25519" with "-t rsa"

# Copy public key to desired machine (without ssh-copy-id)
cat id_ed25519.pub | ssh user@host "cat >> .ssh/authorized_keys"
# Copy with ssh-copy-id
ssh-copy-id -i ed25519.pub user@host

# No more password required
ssh -i id_ed25519 user@host
```

Configure ssh to automatically use the privat-key:
- Copy private key (in my case `id_ed25519`) to `~/.ssh/`
- Create/edit `~/.ssh/config` file
```
Host <Shortname>
     HostName <IP/DNS>
     User     <user>
     IdentityFile       ~/.ssh/id_ed25519
```


--------------

## Shell-Script Arguments/Options

Parser for -options/arguments passed on start
```
# When no arguments are given - call help-function
if [ $# -eq 0 ]; then
  help
fi

# Iterate through passed arguments
while [ $# -ne 0 ]
do
    arg="$1"
    case "$arg" in
        -install)
            # Whats todo on "-install"?
        ;;
        -start)
            # Whats todo on "-start"
        ;;
        *)
            # Else help
        ;;
    esac
    shift # Go ahead to next argument
done
```

## SSH Tunnel 
Access service on not private port of remote machine through ssh tunnel
```
# Without ssh config host configured
ssh -N user@address -L 8081:localhost:1111

# With ssh cfongi host configured
ssh -N <hostname> -L <localport>:localhost:<remortport>

e.g.
# When "127.0.0.1:12345" is browsed ssh tunnel to "myname"-server and forward to localhost:10100 
ssh -N myname -L 12345:localhost:10100
```

## Access GitHub using SSH-Key URL
Follow steps from: SSH authentication using privat-/public key (ED25519 || RSA)
Enter private key in ssh-key-config of github
Execute the following command to update the origin-url of the affected repositories.

```
git remote set-url origin git@github.com:$USERNAME/$REPONAME.git
```
