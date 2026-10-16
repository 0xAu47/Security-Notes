# Recap: Ninja Skills

**Room:** TryHackMe, Ninja Skills (Challenge Lab)

**Link** https://tryhackme.com/room/ninjaskills

**What it covered:** Deployed a Linux lab machine and worked through 12 oddly-named files scattered across the filesystem (home directory, /mnt, /var, /opt, /etc, /media, and root) to answer six questions about them. For each question I located the relevant file(s) with `find`, then matched the question to the right basic command: `find -group` to filter by group ownership directly, `cat` piped into `grep` to search content for an IP address, `sha1sum` to match a given hash, `wc -l` to match a line count, `ls -ln` to find a file by its numeric owner ID, and `ls -al` to spot the file executable by everyone.

**Why it matters (real attack/defense connection):** This mirrors real investigative work — you rarely get told exactly where a relevant file lives, so you search for it by name, owner, group, or content across the whole disk. `find -group` is how you'd audit "which files belong to this group" during a permissions review. Grepping file content for IP-shaped strings is literally how you'd hunt for hardcoded infrastructure addresses in configs or logs. Hashing a file with `sha1sum` is how you verify its integrity or match it against a known file. And spotting a file that's executable by *everyone* with `ls -al` is one of the simplest, most real checks in a permissions audit — it's a textbook sign of a misconfigured or backdoored system.

**Could I explain it cold?** Yes — this method is genuinely mine, not something I half-remember. I know there's a faster way to search the whole filesystem in one shot instead of locating files individually first, but I haven't practiced that version enough to write it from memory yet. That's next, not now.

**Load Bearing Idea:** Match the right tool to the right kind of question. Ownership and group are metadata — `find`/`ls` read that directly without opening the file. Content (an IP, a line count) means actually reading inside the file — `cat`, `grep`, `wc`. Identity (a hash) means transforming the file into a fingerprint and comparing — `sha1sum`. Each question category has its own correct tool, and knowing which one to reach for is the actual skill here, not memorizing commands.

**How I would attack it:** Locate each of the 12 target files individually by name across the filesystem with `find`, and note their paths. Then go question by question: for ownership, filter directly with `find -group`; for content questions, `cd` to the file and `cat`/`grep` it; for the hash, run `sha1sum` against candidate files until one matches; for line count, `wc -l` the same way; for numeric owner, `ls -ln`; for permissions, `ls -al`.

**How I would defend it:** Same personal angle as before — this isn't about protecting a company's infrastructure, it's a reminder of what a careless setup looks like on my own systems. Don't leave files executable by everyone when they don't need to be. Don't rely on scattering sensitive files across odd folders as if that's "security" — if I can find them with `find`, so can anyone else with access. And periodically hashing important files is a cheap way to confirm nothing's been tampered with.
