https://stackoverflow.com/questions/1582894/how-to-send-password-securely-over-http


log
hash,
salt,
https over insecure wifi
rainbow attack



hashing password on client side or server side


incorrect password:
why log password.

Log.println(request.body())
ssl: man in the middle



Good read:
https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/

n = number of db dump records
N = pre computed hashes
rainbow table attack: O(n) to go through all the hashes see if they exists in existing data.

with Salt:
n * N

may crack one, but not all,
bigger N, longer.

Salt is in clear text,  
do not want to make the salts readily accessible to the publi


Brute force (trying every possible candidate).
Dictionaries or wordlists of common passwords
Lists of passwords obtained from other compromised sites.
More sophisticated algorithms such as Markov chains or PRINCE
Patterns or masks (such as “1 capital letter, 6 lowercase letters, 1 number”).
