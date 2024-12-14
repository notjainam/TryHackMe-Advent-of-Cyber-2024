# Advent of Cyber - Day 1: Investigating Malicious Website

## Task Overview
McSkidy, our protagonist, is investigating a suspicious website and a potentially malicious YouTube to MP3 converter that has been circulating in the community. The task involves investigating the website, analyzing suspicious files, and identifying the malicious actor behind it.

### Learning Objectives
- Learn how to investigate malicious link files.
- Understand OPSEC (Operational Security) and OPSEC mistakes.
- Learn how to track and attribute digital identities in cyber investigations.

## Connecting to the Machine
Before proceeding with the investigation, ensure that your virtual machine (VM) is running, and the AttackBox is launched. The VM should be fully loaded in about 3 minutes. If you encounter any issues, try again later due to high demand.

## Investigating the Website
The website in question is a YouTube to MP3 converter being circulated among SOC-mas organizers. The website appears legitimate, but past experiences suggest it could be risky.

### Website Details:
- The website claims to be "Secure" and "Safe," but these claims are likely dubious.
- The "About" page mentions "The Glitch" as the creator, which might be a lead.
- Common risks associated with such sites include malvertising, phishing, and bundled malware.

### Actions Taken:
1. **Accessing the Website:** I visited the URL 
`http://10.10.108.138` on the AttackBox.
2. **Downloading a File:** I entered a YouTube link 
(`https://www.youtube.com/watch?v=dQw4w9WgXcQ`) in the website's converter and downloaded the resulting file, named `download.zip`.
3. **Extracting the File:** I extracted the contents of the `download.zip` file, which contained two files: `song.mp3` and `somg.mp3`.

## Investigating the Files
The downloaded files need to be analyzed to check for any malicious behavior.

### Step 1: Check the File Type of `song.mp3`
I ran the following command to check the file type of `song.mp3`:

```bash
file song.mp3
