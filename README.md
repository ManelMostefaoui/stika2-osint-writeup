# Stika2 — OSINT Write-up (IngeHack CTF)

## Overview

* **Challenge:** Stika2
* **Category:** OSINT
* **CTF:** IngeHack CTF
* **Difficulty:** Hard

---

##  Goal

The objective was to track down a fugitive individual known as **“stika”** by following his digital footprint.

The challenge only provided:

* A name: **stika**
* A hint: *“he is around us”*

We had to identify:

* His network
* His activities
* And ultimately locate the **meeting point (flag)**

---

##  Recon & Initial Analysis

Since the description mentioned *“he is around us”*, and the CTF communication platform was **Discord**, I started searching within the Discord server itself.

* Checked members list (especially offline users)
* Found an account named **stika**, but it was empty (no activity, no clues)

I continued investigating and found a suspicious account:

* **Name:** Adrien Solvek
* **Observation:** AI-generated profile picture
* **Bio contained:**

  ```
  Cf9BXvmS
  ```

This looked like an identifier or invite code.

---

##  Hidden Channel Discovery

I tested the string across platforms and found:

```
https://discord.gg/Cf9BXvmS
```

This led to a private Discord channel.

Inside, there was a conversation between **stika** and **Adrien**, mentioning:

> A valuable and expensive painting stolen from a well-known museum in London.

---

## Identifying the Museum

Next step: identify the museum.

I searched for famous museums in **London** and narrowed it down to several candidates.

After checking reviews across platforms, I eventually found a lead on **Tripadvisor**:

* A review left by user: **stika_3457**

The museum was:

=> **Victoria and Albert Museum**

Review content:

> "Wow Adrien, great target. Curious to explore what this museum holds."

This confirmed:

* The museum
* A new username: **stika_3457**

---

##  Tracking the Stolen Painting

Now the goal shifted to finding the stolen painting.

I searched for the username **stika_3457** on platforms where images are shared.

Found a match on:

* **Pinterest**

The account had:

* One post → the stolen painting

In the comments, Adrien wrote:

> “We pulled it off… I’m proud of you, bro. Now there’s just one step left — the deal. All we know about the buyer is his name: Mark Zelmarken. He said he’d leave clues…”

---

##  Finding the Buyer

Next target: **Mark Zelmarken**

After searching across platforms, I found him on:

* **GitHub**

His profile contained:

* A repository named: **secrets**

Inside:

* A password-protected ZIP file

---

## 🔐 Password Discovery (Key Step)

Initial attempts:

* Tried brute force ❌
* Tried guessing ❌

Then the challenge author dropped a hint:

> “Not everything worth sharing needs a full project, sometimes a simple snippet is enough.”

After Searching i found a github feature called gist :
**GitHub Gists**

I explored:

* `gist.github.com/<username>`

And found the password:

```
fr0m_th3_r1v3r_t0_th3_se4,_P4l3st1ne_w1ll_b3_fr33
```

---

## Extracting the Archive

After unlocking the ZIP file, it contained:

* A text file:

  ```
  we will meet at the place in front of this
  ```
* An image file

---

## Image Analysis (EXIF)

I analyzed the image using:

```bash
exiftool pic.png
```

**What this does:**

* Extracts metadata embedded in the image
* Includes GPS, comments, device info, etc.

Output revealed:

```
Comment: northwestern china
```

---

##  Locating the Place

From the image:

* A **university** was visible

So I:

1. Listed regions in **northwestern China**
2. Searched universities in each region

Eventually identified:
**Lanzhou University (Huiyuan Campus)**

---

##  Final Step — Finding the Meeting Point

Problem:

* Google Maps has limited street-level data in China

Solution:

* Used **Baidu Maps**

Steps:

1. Searched for Lanzhou University (Huiyuan)
2. Looked at the area **in front of the location shown in the image**
3. Identified the exact place name

---

##  Flag

The flag was the name of the location found in front of the university using Baidu Maps.

 Key Takeaways

Always scan all members in CTF Discord servers — the target may be hiding in plain sight.
Short strings in bios or profiles can be invite codes, IDs, or passwords — always test them across platforms.
GitHub Gist is an often-overlooked feature that can serve as a hidden dead-drop.
When dealing with locations in China, Google Maps is useless — use Baidu Maps.
exiftool is an essential OSINT tool for extracting hidden metadata from images.
The chain of clues in this challenge spanned 6+ platforms — patience and methodical searching are key.
