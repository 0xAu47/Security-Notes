# Recap: Digital Footprint 

**Room:** TryHackMe, Digital Footprint (Challenge Lab)

**Link** https://tryhackme.com/room/osintchallengeiv

**What it covered:** Four different OSINT angles for digging up information from "leaked" material — recovering hidden EXIF/metadata from a photo, pulling older archived/cached versions of a company site, identifying a location just from what's visible in an image with no metadata at all, and finding identifying info left inside a document file itself.


**Why it matters (real attack/defense connection):** Photos carry hidden metadata — GPS coordinates, timestamp, device model — that isn't visible just by looking at the image. If that photo is ever posted publicly, anyone can pull this data out and learn exactly where you were, when, and what device you own. This is a real passive information leak, and it's the same kind of technique used in OSINT reconnaissance, whether by an investigator or an attacker.

**Could I explain it cold?** Yes, with one honest caveat — my first attempt at the investigation order was backwards (see below).

On my first attempt, I did this backwards — I scanned the image visually first and checked the metadata last — which is exactly how I learned this order matters.

**Load Bearing Idea:** Information about you can leak out even when you never posted it yourself — it can be hiding inside the files you share, like metadata in a photo.

**How I would attack it:** Given a target photo, check the file's metadata first — it's fast and gives hard facts (location, time, device) with no guesswork. Only fall back to manually scanning the image for visual clues (text, landmarks, signs) to search across Google, Maps, or YouTube if the metadata doesn't give enough.

**How I would defend it:** I'm not protecting a company's systems, so for me this is really about protecting my own stuff. Strip EXIF data before posting any photo publicly — location, device, timestamp. Check a document's "Properties" before sharing a Word or PDF file, since it can quietly carry my real name or my computer's username. And remember that even with metadata stripped, what's actually visible in the photo — a landmark, a sign, a reflection — can still give away where I was.
