# Recap: Digital Footprint

**Room:** TryHackMe, Digital Footprint

**What it covered:** Four different OSINT angles for digging up information from "leaked" material — recovering hidden EXIF/metadata from a photo, pulling older archived/cached versions of a company site, identifying a location just from what's visible in an image with no metadata at all, and finding identifying info left inside a document file itself.


**Why it matters (real attack/defense connection):** Photos carry hidden metadata — GPS coordinates, timestamp, device model — that isn't visible just by looking at the image. If that photo is ever posted publicly, anyone can pull this data out and learn exactly where you were, when, and what device you own. This is a real passive information leak, and it's the same kind of technique used in OSINT reconnaissance, whether by an investigator or an attacker.

**Could I explain it cold?** Yes, with one honest caveat — my first attempt at the investigation order was backwards (see below).

On my first attempt, I did this backwards — I scanned the image visually first and checked the metadata last — which is exactly how I learned this order matters.

**Load Bearing Idea:** Information about you can leak out even when you never posted it yourself — it can be hiding inside the files you share, like metadata in a photo.

**How I would attack it:**I wouldn't start with tools or metadata extractors. I'd do what I eventually learned to do in this room — look at the image or document with my own eyes first: visible text, logos, street signs, anything recognizable in the background. Only after I've gotten everything I can from just looking would I open up the file's metadata (EXIF for a photo, document properties for a file) to check for GPS coordinates, timestamps, author names, or software versions — because that's usually where someone who thinks they've already "cleaned" a file still leaks the real info. For the archived site, I'd check what the live site shows first before going to an archive tool to dig up what it used to say.

**How I would defend it:**I'm not protecting a company's systems, so for me this is really about protecting my own stuff. Strip EXIF data before posting any photo publicly — location, device, timestamp. Check a document's "Properties" before sharing a Word or PDF file, since it can quietly carry my real name or my computer's username. And remember that even with metadata stripped, what's actually visible in the photo — a landmark, a sign, a reflection — can still give away where I was.
