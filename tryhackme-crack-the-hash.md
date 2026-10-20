# Recap: Crack the Hash

**Room:** TryHackMe — Crack the Hash (Challenge Lab)
Link: (https://tryhackme.com/room/crackthehash)

**What it covered:** Identifying hash types and cracking them. Level 1 was straightforward — unsalted MD5, SHA1, SHA256, and bcrypt hashes solved with an online hash-cracking tool (hashes.com). Level 2 raised the difficulty: hashes had to be identified manually using hashcat's example-hash reference and hashes.com's identifier tool, then cracked offline with `hashcat` against the rockyou wordlist. The last two hashes in Level 2 were salted, which meant figuring out not just the hash type but how the salt and password were combined.

**Why it matters (real attack/defense connection):** Hash cracking is the backbone of a huge amount of real attacks — stolen password databases, dumped `/etc/shadow` files, Windows NTLM hashes pulled from a compromised machine. Whether any of that is actually useful to an attacker almost entirely comes down to what hash algorithm was used and whether it was salted. A weak, fast, unsalted hash (plain MD5 or SHA1) falls to a wordlist in seconds; a slow, salted one (bcrypt, sha512crypt) can hold out for a very long time even against the same wordlist.

**Could I explain it cold?** Not yet — what's fuzzy: the general workflow (file the hash, identify it, pick a hashcat mode, run a dictionary attack) is solid in my head. What I'd still get stuck on is telling hashcat's salted-SHA1 modes (110–160) apart just by looking at a hash — I got the last one right by testing modes in order, not by recognizing it outright.

**Load Bearing Idea:** Cracking a hash is really two separate problems — correctly identifying *what* you're looking at (the algorithm, and if salted, exactly how the salt and password were combined), and then letting hashcat do the actual guessing. Get the identification wrong and no wordlist, however good, will ever crack it. For some salted formats, the only way to find the exact construction is to try the candidate modes in order until one works, since the hash string alone doesn't tell you.

**How I would attack it:** For Level 1, I ran each hash straight through hashes.com's online cracker — no manual identification needed, it auto-detects basic unsalted formats. For Level 2, I located the rockyou wordlist (`find / -name '*.txt' 2>/dev/null | grep rockyou.txt`, which pointed to `/usr/share/wordlists/rockyou.txt`), copied each hash into its own file with `nano`, used the hashcat example-hash page plus the hashes.com identifier to match a mode, and ran `hashcat -m <mode> -a 0 hash.txt /usr/share/wordlists/rockyou.txt`. That gave me `-m 1400` (SHA2-256), `-m 1000` (NTLM), and `-m 1800` (sha512crypt) for the first three. For the last hash, salted SHA1, I added the given salt to the hash file and worked through hashcat's salted-SHA1 modes in order (110, 120, 130, 140, 150) until `-m 160` actually cracked it.

**How I would defend it:** Don't store passwords with fast general-purpose hashes (MD5, SHA1, plain SHA256) at all — use something deliberately slow and built for passwords, like bcrypt, scrypt, or Argon2. Always use a unique, random salt per password, never a shared or guessable one. Enforce a policy that rejects any password appearing in a known breached-password list like rockyou.txt — if it's crackable with a public wordlist, it's not a real password. And treat any leaked hash dump (a breach, a stolen `/etc/shadow`, extracted NTLM hashes) as a live incident, not just stored data, since cracking it is usually just a matter of time and the right wordlist.

---
