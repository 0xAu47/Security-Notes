# Recap: Digital Footprint

**Room:** TryHackMe, Digital Footprint

**What it covered:** An OSINT (Open-Source Intelligence) challenge room. Starting from an image, I had to find information using publicly available tools and techniques — scanning the image for visual clues and checking the file's metadata (EXIF data) to extract hidden information.

**Why it matters (real attack/defense connection):** Photos carry hidden metadata — GPS coordinates, timestamp, device model — that isn't visible just by looking at the image. If that photo is ever posted publicly, anyone can pull this data out and learn exactly where you were, when, and what device you own. This is a real passive information leak, and it's the same kind of technique used in OSINT reconnaissance, whether by an investigator or an attacker.

**Could I explain it cold?** Yes, with one honest caveat — my first attempt at the investigation order was backwards (see below).

**Load Bearing Idea:** Information about you can leak out even when you never posted it yourself — it can be hiding inside the files you share, like metadata in a photo.

**How I would attack it:** Given a target photo, check the file's metadata first — it's fast and gives hard facts (location, time, device) with no guesswork. Only fall back to manually scanning the image for visual clues (text, landmarks, signs) to search across Google, Maps, or YouTube if the metadata doesn't give enough.

**How I would defend it:** Before posting a photo publicly, strip its metadata first. Some platforms do this automatically, but not all — so it shouldn't be assumed. A photo with no caption at all can still reveal your exact location and device if the metadata is left intact.
