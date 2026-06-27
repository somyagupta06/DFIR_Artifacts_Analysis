
# Linux File System Analysis

## 1. Objective


The objective of this exercise is to understand the artifacts , validate attacker abuse techniques and analyze according the ivestigator perspective.

---

## 2. Environment

- Platform: Tryhackme
- Date: 9 June 2026
- Analyst: Somya Gupta 
- VM/AttackBox: Tryhackme Attackbox

---

# Topic 01 — Securing the Environment

## - Concept

I learned that a compromised system should not be trusted during an investigation. If an attacker gets high-level access, they can change system tools, binaries, or libraries to hide their activities. Because of this, investigators should use trusted tools and libraries instead of relying on the tools already present on the system.

---

## - Attacker Perspective

An attacker can modify system tools to hide files, processes, or other evidence. This can make the investigation more difficult and help the attacker avoid detection.

---

## - Investigator Perspective

As an investigator, I should not fully trust the tools available on a compromised machine. Using trusted binaries and libraries helps me collect more reliable information and reduces the risk of being misled by modified tools.

---

# - Practical Validation

## I. Hypothesis

I wanted to understand why investigators use trusted binaries and libraries instead of trusting the tools already present on a compromised machine.

---

## II. Validation 1 - PATH Manipulation

### - Task

I wanted to check how Linux decides which binary to execute when I run a command.

### - What I Did

* First, I checked which `ls` binary was being used by the system.
* Then, I checked the PATH variable.
* I created my own fake `ls` executable inside a custom directory.
* After that, I added my directory at the beginning of the PATH variable.
* Finally, I ran the `ls` command again.

### - What I Observed

Before changing PATH, the original system `ls` was executed.

After adding my directory to PATH, the same `ls` command executed my fake binary instead.

This showed me that Linux follows the PATH search order and runs the first matching executable it finds.

### - Evidence
<img width="1005" height="554" alt="Screenshot 2026-06-10 at 6 10 56 PM" src="https://github.com/user-attachments/assets/fd6d4e70-20cb-4f57-82b1-591c09b42fc8" />
<img width="1000" height="377" alt="Screenshot 2026-06-10 at 6 11 18 PM" src="https://github.com/user-attachments/assets/db6e0b80-8504-4fc7-90b3-e811e6270d0e" />





### - What I Learned

An attacker could place a malicious binary earlier in PATH and trick an investigator into running it instead of the legitimate one.

---

## III. Validation 2 - Environment Variables

### - Task

I wanted to understand the difference between a normal variable and an exported variable.

### - What I Did

* I created a custom variable.
* I opened a child shell.
* I checked whether the child shell could access the variable.
* Then I repeated the same test using `export`.
* I also opened a new terminal session to see whether the change survived.

### - What I Observed

The variable did not automatically appear inside the child shell.

After using export, the child shell inherited the variable.

However, when I opened a new terminal session, the variable was gone.

### - Evidence

Without export on changing terminal - <img width="1014" height="525" alt="Screenshot 2026-06-10 at 6 28 14 PM" src="https://github.com/user-attachments/assets/da681a05-ab7a-462a-9f02-dd7f45762383" />

With export on changing terminal - <img width="1011" height="522" alt="Screenshot 2026-06-10 at 6 28 44 PM" src="https://github.com/user-attachments/assets/36371f23-5a1b-414d-84a9-a9c2b1bcdde7" />

In Child Shell - <img width="1144" height="638" alt="Screenshot 2026-06-10 at 6 31 28 PM" src="https://github.com/user-attachments/assets/6a8177c2-1800-4a49-beab-972a874a93d2" />

Permanently creating a env variable - <img width="717" height="460" alt="Screenshot 2026-06-10 at 6 33 46 PM" src="https://github.com/user-attachments/assets/5cd304ad-f48f-4163-8567-ebd6322503e0" />



### - What I Learned

Export allows variables to be passed to child processes, but it does not make them permanent.

---

## IV. Validation 3 - Binary and Library Investigation

### - Task

I wanted to understand whether Linux binaries work independently or depend on external libraries.

### - What I Did

* I located the original `ls` binary.
* I tried reading its contents directly.
* I extracted readable strings from the binary.
* I searched for library references used by the binary.

### - What I Observed

The binary was not human-readable.

While inspecting it, I found references to shared libraries such as `libc.so` and `libselinux.so`.

This showed that the binary depends on external libraries to work properly.

### - Evidence

Step 1. <img width="367" height="147" alt="Screenshot 2026-06-10 at 6 35 28 PM" src="https://github.com/user-attachments/assets/b2e97854-adbc-4f9b-b6bc-953f28aaa6b1" />

Step 2. <img width="1021" height="528" alt="Screenshot 2026-06-10 at 6 35 50 PM" src="https://github.com/user-attachments/assets/a123060c-c7b4-4b02-88f6-3f9d729780dd" />

Step 3. <img width="418" height="80" alt="Screenshot 2026-06-10 at 6 36 07 PM" src="https://github.com/user-attachments/assets/8a398b04-cfa8-4e3f-814d-5c6219a572c1" />

Step 4. <img width="586" height="588" alt="Screenshot 2026-06-10 at 6 36 28 PM" src="https://github.com/user-attachments/assets/acf7eb11-9e09-42d1-bd3d-a172cb9094a6" />

Step 5. <img width="298" height="121" alt="Screenshot 2026-06-10 at 6 36 46 PM" src="https://github.com/user-attachments/assets/0049e354-a483-44e3-82c1-940e639d10b6" />

Step 6. <img width="1014" height="537" alt="Screenshot 2026-06-10 at 6 37 07 PM" src="https://github.com/user-attachments/assets/8bc7748d-f761-4d39-bff2-2b1080d67d7b" />

Step 7. <img width="978" height="489" alt="Screenshot 2026-06-10 at 6 37 56 PM" src="https://github.com/user-attachments/assets/91ee9a93-bb1d-4031-8fb9-dbdd54a4bd8f" />

### - What I Learned

Even if a binary is legitimate, modifying a library could still affect its behaviour.

This is why investigators use trusted libraries along with trusted binaries.

---

## V. Final Understanding

After completing these tests, I understood why investigators do not blindly trust a compromised machine.

A binary can be replaced.

A PATH variable can be manipulated.

A shared library can be modified.

Using trusted binaries and libraries helps ensure that the information collected during an investigation is reliable.


---

# Topic 02 — Identifying the Foothold


## Concept

A foothold is the initial access gained by an attacker on a system.

It is usually the first malicious file, account, process, or web shell used to enter the environment.

---

## Attacker Perspective

The attacker creates a way to access the target system.

Common footholds include:

- Web shells
- Malicious scripts
- Backdoor accounts
- Exploited applications

The goal is to maintain access and perform further actions.

---

## Investigator Perspective

The investigator tries to find the first malicious artifact used by the attacker.

Things to look for:

- Newly created files
- Suspicious uploads
- Strange permissions
- Unknown accounts
- Unusual processes

Finding the foothold helps build the attack timeline and determine how the compromise started.

---

## Practical Validation

# Upload Hunting & Directory Profiling

## I. Hypothesis

I wanted to understand how investigators identify suspicious files inside upload directories by first establishing what is normal and then looking for anomalies.

---

## II. Validation 1 - Suspicious Upload Hunting

### Task

I wanted to practice identifying a suspicious file hidden among a large number of normal uploaded files.

### What I Did

- Created an uploads directory containing mostly image files.
- Added a few files with different extensions.
- Listed all files inside the directory.
- Looked for files that did not match the expected file type.
- Compared the unusual files against the normal files.

### What I Observed

Most files inside the directory were `.jpg` images.

A few files had different extensions such as:

```text
.pdf
.txt
.sh
```

These files immediately stood out because they did not match the normal upload pattern.

### Evidence

Step 1. - <img width="548" height="331" alt="Screenshot 2026-06-11 at 7 16 12 PM" src="https://github.com/user-attachments/assets/8272c2cf-b664-4fe0-ad74-22c5c70c87be" />

Step 2. - <img width="466" height="118" alt="Screenshot 2026-06-11 at 7 16 31 PM" src="https://github.com/user-attachments/assets/c9dbbb1a-a009-48ed-9a8f-db68367f24c1" />




Step 3. - <img width="611" height="206" alt="Screenshot 2026-06-11 at 7 16 52 PM" src="https://github.com/user-attachments/assets/f2725a69-f3fc-449e-93e8-31ed30f6228e" />


Step 4. - <img width="1079" height="424" alt="Screenshot 2026-06-11 at 7 17 05 PM" src="https://github.com/user-attachments/assets/21827036-1c20-49b3-815e-73649eb4f6c0" />




### What I Learned

An investigator does not start by searching for malware.

The first step is to understand what is normal for a directory and then identify anything that deviates from that baseline.

---

## III. Validation 2 - Attacker Mistakes

### Task

I wanted to simulate an attacker uploading a malicious file and intentionally leaving indicators that could be discovered during an investigation.

### What I Did

- Created a file using a suspicious extension.
- Made the file executable.
- Assigned a SUID permission.
- Modified the timestamp to make it appear much older than the surrounding files.
- Investigated the directory as if I had no prior knowledge of the file.

### What I Observed

The file contained multiple indicators that made it stand out:

- Unusual extension
- Dangerous permissions
- Abnormal timestamp

Each artifact independently raised suspicion.

### Evidence

#### Suspicious Extension

```text
ahguihguial1554.phtml
```

#### SUID Permission

Step 1. - <img width="1074" height="600" alt="Screenshot 2026-06-11 at 7 17 40 PM" src="https://github.com/user-attachments/assets/01b11db8-d572-41cb-8e06-a8fac3e18ac8" />

Step 2. - <img width="800" height="586" alt="Screenshot 2026-06-11 at 7 18 07 PM" src="https://github.com/user-attachments/assets/7260e797-cb36-4f9b-b79a-b1ea8c6f5e3a" />


#### Modified Timestamp

Step 1. - <img width="750" height="174" alt="Screenshot 2026-06-11 at 7 18 44 PM" src="https://github.com/user-attachments/assets/b53cba7f-2b6c-4e7d-b14f-48d07bb2a0a8" />


Step 2. - <img width="641" height="229" alt="Screenshot 2026-06-11 at 7 19 05 PM" src="https://github.com/user-attachments/assets/583d2cca-1eaf-47b6-aacc-f039f8b522be" />


Step 3. - <img width="775" height="584" alt="Screenshot 2026-06-11 at 7 19 21 PM" src="https://github.com/user-attachments/assets/0ead44e5-8221-4bc9-80e1-f97a7c54e3fa" />


### What I Learned

Attackers often leave traces behind.

Unusual extensions, dangerous permissions, and abnormal timestamps can all be valuable indicators during forensic investigations.

---

## IV. Validation 3 - Upload Directory Profiling

### Task

I wanted to create a baseline profile for an upload directory and then identify files that violated that profile.

### What I Did

- Examined all files inside an upload directory.
- Identified common file extensions.
- Reviewed ownership information.
- Checked file permissions.
- Compared file sizes and timestamps.
- Looked for files that did not match the established baseline.

### What I Observed

Most files shared similar characteristics:

- Extension: `.jpeg`
- Owner: `www-data`
- Permission: `644`
- Similar file sizes
- Similar timestamps

One file did not match the profile.

### Evidence

#### Normal Files

```text
Owner: www-data
Extension: .jpeg
Permissions: -rw-r--r--
Size: ~1900 bytes
```

#### Suspicious File

```text
b2c8e1f5.phtml
```

```text
Extension: .phtml
Size: 30 bytes
```
<img width="619" height="598" alt="Screenshot 2026-06-11 at 7 19 46 PM" src="https://github.com/user-attachments/assets/7b10ab3b-d89a-44b8-b4be-6518c651571b" />

### What I Learned

Directory profiling is one of the fastest ways to identify anomalies.

By understanding what should exist inside a directory, suspicious files become much easier to detect.

---
# V. Validation 4 - File Type Identification

### Task

I created different types of files inside a test directory and practiced identifying them using Linux commands.

---

### What I Did

- Created files with different content but same extensions.
- Used `ls` to view the files.
- Used the `file` and `exiftool` command to identify the actual file type.
- Compared file extensions with the real file content.

---

### What I Observed

The file extension does not always tell the real file type.

The `file`  and `exiftool` command checks the file content and metadata instead of trusting the extension.

A file can be renamed to look harmless while actually containing different content.

---

### Evidence

Step 1. - <img width="684" height="534" alt="Screenshot 2026-06-11 at 7 10 56 PM" src="https://github.com/user-attachments/assets/4fb57994-14cc-455e-8f43-af31e723b420" />

Step 2. - <img width="1093" height="500" alt="Screenshot 2026-06-11 at 7 11 26 PM" src="https://github.com/user-attachments/assets/e01893bc-2e0f-43f9-95ac-6ad361538d18" />

Step 3. - <img width="483" height="183" alt="Screenshot 2026-06-11 at 7 11 57 PM" src="https://github.com/user-attachments/assets/6842a912-64d0-4028-b9fb-b91018f44146" />

Step 4. - <img width="402" height="100" alt="Screenshot 2026-06-11 at 7 12 18 PM" src="https://github.com/user-attachments/assets/528b1cdd-155e-4f18-ac35-652d1103c677" />

Step 5. - <img width="1016" height="544" alt="Screenshot 2026-06-11 at 7 13 03 PM" src="https://github.com/user-attachments/assets/90788e1e-af35-4176-b933-9bbccb7f761a" />

Step 6. - <img width="504" height="403" alt="Screenshot 2026-06-11 at 7 13 25 PM" src="https://github.com/user-attachments/assets/1823c0b5-1a1d-4a69-92c5-70bc1509a7aa" />


Step 7. - <img width="388" height="140" alt="Screenshot 2026-06-11 at 7 13 47 PM" src="https://github.com/user-attachments/assets/2371d4f4-6c40-4673-8e6c-f72cc64d0990" />


Step 8. - <img width="888" height="572" alt="Screenshot 2026-06-11 at 7 14 08 PM" src="https://github.com/user-attachments/assets/21aee4ba-68bb-4f05-bd47-dec6d8b71eae" />

Step 9. - <img width="560" height="526" alt="Screenshot 2026-06-11 at 7 14 48 PM" src="https://github.com/user-attachments/assets/3f5ff761-8256-499f-b6c6-540a5e8b1222" />



---

### What I Learned

Attackers can disguise malicious files by changing file extensions.

During an investigation, I should not trust the filename alone and should verify the actual file type using forensic tools and Linux commands.

---

## VI. Conclusion

These exercises demonstrated the importance of establishing a baseline before beginning an investigation.

Instead of searching directly for malware, investigators should first identify:

- Expected file types
- Expected permissions
- Expected ownership
- Expected timestamps

Any deviation from that baseline becomes a strong candidate for further analysis.
---

# Topic 03 — Ownership and Permissions

## Concept

File ownership and permissions help determine who created, modified, or executed a file.

Investigators often analyze ownership and permissions to identify suspicious activity.


## Attacker Perspective

Attackers commonly upload files using compromised accounts such as www-data.

They may also modify permissions to make payloads executable.

Common targets include:

- /tmp
- /var/tmp
- /dev/shm

## Investigator Perspective

Investigators look for:

- Files owned by suspicious users
- Unexpected executable files
- World-writable files
- Recently modified files
- Files located in writable directories

Ownership and permission analysis can quickly reveal attacker-created artifacts.

---

# Practical Validation

## I. Hypothesis

I wanted to understand how investigators can identify suspicious files by analyzing ownership, permissions, special permission bits, and deviations from normal directory behavior.

---

## II. Practical 1 - Ownership Profiling

### Task

I wanted to identify files based on ownership and group ownership.

### What I Did

- Created multiple normal files owned by root.
- Created multiple web files owned by www-data.
- Created multiple suspicious files owned by nobody:nogroup.
- Used file ownership as the hunting criteria.

### What I Observed

Different ownership categories immediately separated files into logical groups.

Files owned by www-data could represent web server activity, while files owned by nobody:nogroup stood out from the normal dataset.

### Evidence

Step 1. - <img width="1166" height="383" alt="Screenshot 2026-06-25 at 8 03 06 PM" src="https://github.com/user-attachments/assets/fe4e9d35-36b2-4a39-9491-75d1e5891fe5" />


Step 2. - <img width="750" height="644" alt="Screenshot 2026-06-25 at 8 03 19 PM" src="https://github.com/user-attachments/assets/e50a4f78-5281-433b-a400-96b6a74f682d" />


Step 3. - <img width="1159" height="370" alt="Screenshot 2026-06-25 at 8 03 35 PM" src="https://github.com/user-attachments/assets/74e155ce-111b-42fb-aae6-dc5f9d968b11" />


Step 4. - <img width="1322" height="460" alt="Screenshot 2026-06-25 at 8 03 52 PM" src="https://github.com/user-attachments/assets/219294b9-405f-4cb7-ae89-211db59561eb" />


Step 5. - <img width="555" height="604" alt="Screenshot 2026-06-25 at 8 04 11 PM" src="https://github.com/user-attachments/assets/6de4c2fc-2246-4da3-8e22-47e8c081f6a4" />


Step 6. - <img width="1321" height="541" alt="Screenshot 2026-06-25 at 8 04 43 PM" src="https://github.com/user-attachments/assets/29119be4-e62b-407e-9155-3e2d57fe8eab" />

Step 7. - <img width="942" height="666" alt="Screenshot 2026-06-25 at 8 05 00 PM" src="https://github.com/user-attachments/assets/3f54e6b8-159d-40c4-8155-9170bf24b59c" />


### What I Learned

Ownership is an important hunting artifact because attackers often create or modify files using compromised service accounts.

---

## III. Practical 2 - Special Permission Hunting

### Task

I wanted to identify files with dangerous permission settings.

### What I Did

- Created normal files with standard permissions.
- Created files containing SUID, SGID, and Sticky Bit permissions.
- Used find commands to locate files with special permission bits.

### What I Observed

Normal files blended together.

Files containing special permissions immediately became visible when filtering based on permission values.

### Evidence

Step 1. - <img width="436" height="634" alt="Screenshot 2026-06-25 at 8 06 51 PM" src="https://github.com/user-attachments/assets/8cb7112e-09b6-4cca-bc48-b640f6a8a082" />

Step 2. - <img width="826" height="554" alt="Screenshot 2026-06-25 at 8 07 14 PM" src="https://github.com/user-attachments/assets/828459eb-6fcb-4c2f-8784-236e6655b329" />

Step 3. - <img width="1003" height="353" alt="Screenshot 2026-06-25 at 8 07 44 PM" src="https://github.com/user-attachments/assets/3d4e3f1b-fea7-4d82-92e3-98df98ff87d2" />



### What I Learned

Special permission bits can introduce security risks and should always be reviewed during investigations.



## V. Practical 3 - Baseline vs Anomaly Detection

### Task

I wanted to learn how investigators identify abnormal files by first understanding what is normal.

### What I Did

- Created 200 normal files.
- Assigned the same owner, group, extension, and permissions.
- Added one abnormal file.
- Modified its owner, permissions, timestamp, and size.

### What I Observed

The abnormal file immediately stood out because it violated multiple baseline characteristics.

The investigation became easier because I knew what normal behavior looked like.

### Evidence

Step 1. - <img width="663" height="365" alt="Screenshot 2026-06-25 at 8 08 14 PM" src="https://github.com/user-attachments/assets/619ef783-6f54-4025-868a-490686e324e0" />

Step 2. - <img width="572" height="551" alt="Screenshot 2026-06-25 at 8 08 35 PM" src="https://github.com/user-attachments/assets/53aea113-f05c-4d54-a2c3-285ed6a0c94f" />

Step 3. - <img width="541" height="594" alt="Screenshot 2026-06-25 at 8 08 54 PM" src="https://github.com/user-attachments/assets/55a594c7-3ae4-405e-976d-c596c27db931" />

### What I Learned

Investigators rarely start by looking for malware.

They first establish a baseline and then identify anything that breaks that baseline.

---

## Overall Learning

Ownership and permissions provide valuable forensic artifacts.

During investigations I can:

- Hunt files owned by compromised accounts.
- Hunt files owned by suspicious groups.
- Identify SUID, SGID, and Sticky Bit files.
- Detect unusual permissions.
- Detect abnormal ownership.
- Build a baseline and locate anomalies quickly.

These techniques help investigators identify suspicious files even when the file name itself appears normal.

---

# Topic 04 — Users and Groups

## Concept

Linux stores information about users, groups, login activity, and privileges. During a forensic investigation, these artifacts help identify unauthorized accounts, privilege escalation, persistence mechanisms, and suspicious user activity. Investigating users and groups also helps reconstruct how an attacker accessed and interacted with the system.

---

## Attacker Perspective

After gaining access, an attacker may create new user accounts, modify existing accounts, assign UID 0, add users to privileged groups such as sudo or shadow, change login shells, or modify sudo privileges to maintain persistent access. These changes allow the attacker to regain access even after the initial vulnerability has been fixed.

---

## Investigator Perspective

An investigator should enumerate all users and groups, review privileged accounts, identify duplicate UID 0 accounts, verify login shell configurations, inspect group memberships, analyze authentication logs, review successful and failed login attempts, and investigate sudo activity. These artifacts help detect persistence, privilege escalation, and reconstruct the attack timeline.

---

# Practical Validation

## I. Hypothesis

I wanted to understand how investigators can identify unauthorized or suspicious user accounts by analyzing user IDs (UIDs), login shells, and account information stored in the Linux authentication database.

---

## II. Practical 1 - User Enumeration & UID 0 Backdoor Detection

### Task

I wanted to investigate Linux user accounts and determine whether an attacker had created a hidden backdoor account with root privileges.

### What I Did

* Enumerated all user accounts from the system.
* Reviewed the contents of the user account database.
* Created multiple user accounts for analysis.
* Modified one user account to use UID 0.
* Compared user IDs to identify duplicate root-level accounts.
* Reviewed login shell configurations for all users.
* Reviewed login shell assignments for all users.
* Compared interactive and non-interactive shells.
* Modified a user's login shell.
* Investigated accounts configured with `/usr/sbin/nologin`.
* Compared service accounts with normal user accounts.

### What I Observed

The system contained multiple user accounts with different UIDs.

After assigning UID 0 to a non-root account, the account obtained root-level privileges despite having a different username.

Filtering accounts by UID immediately revealed the hidden privileged account.

Reviewing login shells also helped distinguish normal service accounts from interactive user accounts.

Most service accounts used non-interactive shells, preventing direct login.

After modifying a normal user's shell, the account behaved differently from other users.

Abnormal shell assignments became easier to identify by comparing all users against the system baseline.


### Evidence

Step 1. - <img width="1119" height="296" alt="Screenshot 2026-06-27 at 4 52 44 PM" src="https://github.com/user-attachments/assets/9ab86e4c-9b03-4e17-abb2-00105571dc11" />

Step 2. - <img width="574" height="293" alt="Screenshot 2026-06-27 at 4 53 06 PM" src="https://github.com/user-attachments/assets/c123055d-cd35-4736-b0fe-db68efbda630" />

Step 3. - <img width="1150" height="563" alt="Screenshot 2026-06-27 at 4 53 24 PM" src="https://github.com/user-attachments/assets/8194dff6-ee1a-4373-ac97-902178175a0a" />

Step 4. - <img width="1038" height="261" alt="Screenshot 2026-06-27 at 4 53 51 PM" src="https://github.com/user-attachments/assets/2d10f8db-d74c-4d58-8bec-ca9b5d9ff688" />

Step 5. - <img width="1262" height="158" alt="Screenshot 2026-06-27 at 4 54 21 PM" src="https://github.com/user-attachments/assets/45ee5964-c17e-4396-9b96-15172280aad3" />

Step 6. - <img width="675" height="522" alt="Screenshot 2026-06-27 at 4 54 39 PM" src="https://github.com/user-attachments/assets/66c69b84-0603-40f0-b162-06a953aa43d0" />

Step 7. - <img width="828" height="524" alt="Screenshot 2026-06-27 at 4 55 04 PM" src="https://github.com/user-attachments/assets/f261094c-2516-428e-b68c-3def78ecf95b" />


### What I Learned

Linux privileges are determined by the User ID (UID), not by the username.

Any account assigned UID 0 effectively becomes a root account, making duplicate UID detection an important forensic technique during incident response.

---



## Overall Learning

During this practical I learned how investigators analyze Linux user accounts without relying solely on usernames.

I can now:

* Enumerate Linux user accounts.
* Detect duplicate UID 0 accounts.
* Identify hidden root backdoor accounts.
* Analyze login shell configurations.
* Differentiate service accounts from interactive users.
* Identify suspicious account modifications.

These techniques help investigators quickly identify unauthorized accounts and privilege escalation mechanisms during Linux forensic investigations.



---

## IV. Practical 3 - Group Membership Investigation

### Task

I wanted to investigate Linux groups and determine whether an attacker had added users to privileged groups for persistence or privilege escalation.

### What I Did

* Reviewed all system groups.
* Examined the contents of the group database.
* Added users to different groups for analysis.
* Added one user to the **root** group.
* Added another user to the **shadow** group.
* Enumerated user group memberships.
* Reviewed members of specific privileged groups.

### What I Observed

Most users belonged only to their default groups.

After modifying group memberships, the affected users immediately appeared inside privileged groups.

Users added to the **root** and **shadow** groups stood out during investigation because these groups provide elevated capabilities that normal users should not possess.

### Evidence

Step 1. - <img width="290" height="196" alt="Screenshot 2026-06-27 at 5 04 12 PM" src="https://github.com/user-attachments/assets/7ef19bf0-c4b0-4c07-b657-3aadcbfdc432" />

Step 2. - <img width="432" height="499" alt="Screenshot 2026-06-27 at 5 04 36 PM" src="https://github.com/user-attachments/assets/8a9620ef-e611-4e72-a99f-807a348762b1" />

Step 3. - <img width="507" height="282" alt="Screenshot 2026-06-27 at 5 04 52 PM" src="https://github.com/user-attachments/assets/b7a0b274-20d1-44cc-88fd-b2844dd36627" />

### What I Learned

Group memberships are an important forensic artifact.

Attackers may abuse privileged groups instead of directly modifying permissions because group membership provides elevated access while appearing less suspicious.

---

## Overall Learning

During this practical I learned how investigators identify privilege escalation through Linux groups.

I can now:

* Enumerate Linux groups.
* Review group memberships.
* Identify privileged groups.
* Detect unauthorized group assignments.
* Investigate privilege escalation through group membership.

These techniques help investigators identify compromised accounts that have been granted additional privileges.


---


## V. Practical 4 - Authentication Investigation

### Task

I wanted to investigate authentication artifacts generated during normal and malicious user activity.

### What I Did

* Generated successful SSH logins.
* Generated failed SSH login attempts.
* Attempted logins using an invalid username.
* Switched users using `su`.
* Executed privileged commands using `sudo`.
* Reviewed authentication logs.
* Examined login history.
* Reviewed failed login history.

### What I Observed

Successful SSH logins recorded the username, source IP address, login time, and session information.

Failed login attempts generated authentication failure events.

Attempts using invalid usernames were also recorded separately.

User switching (`su`) and privileged command execution (`sudo`) generated dedicated log entries, allowing the entire authentication timeline to be reconstructed.

### Evidence

Step 1. - <img width="1255" height="502" alt="Screenshot 2026-06-27 at 4 57 41 PM" src="https://github.com/user-attachments/assets/d4b54100-6f15-4ad8-bdd3-559600eef760" />

Step 2. - <img width="1313" height="414" alt="Screenshot 2026-06-27 at 4 58 06 PM" src="https://github.com/user-attachments/assets/6b922b00-bf6b-4aa1-813b-6d109a2587d9" />

Step 3. - <img width="1201" height="315" alt="Screenshot 2026-06-27 at 4 58 28 PM" src="https://github.com/user-attachments/assets/9e45f5cb-3588-4ac9-9fd9-4a507f3f3549" />

Step 4. - <img width="1087" height="519" alt="Screenshot 2026-06-27 at 4 58 52 PM" src="https://github.com/user-attachments/assets/43c0524b-8693-44bc-ac4e-f8eee9cafcc3" />

Step 5. - <img width="1214" height="336" alt="Screenshot 2026-06-27 at 4 59 09 PM" src="https://github.com/user-attachments/assets/79fdb5cd-6fcf-412c-af6f-b9535db3af11" />

Step 6. - <img width="1092" height="522" alt="Screenshot 2026-06-27 at 4 59 31 PM" src="https://github.com/user-attachments/assets/5e2832f6-8fad-4ae2-afbc-540d49dacd0d" />

Step 7. - <img width="1082" height="531" alt="Screenshot 2026-06-27 at 4 59 52 PM" src="https://github.com/user-attachments/assets/333302df-fa87-4a82-84ee-ac0604e08f35" />

Step 8. - <img width="1317" height="427" alt="Screenshot 2026-06-27 at 5 00 12 PM" src="https://github.com/user-attachments/assets/3573c7eb-5a8a-4fef-8a79-315572f254f2" />

Step 9. - <img width="1287" height="367" alt="Screenshot 2026-06-27 at 5 00 28 PM" src="https://github.com/user-attachments/assets/91688844-6ea5-4a42-a903-2b3aeda3fd38" />

Step 10. - <img width="1331" height="588" alt="Screenshot 2026-06-27 at 5 00 50 PM" src="https://github.com/user-attachments/assets/c78554eb-cb74-4147-a67a-d6a15ab43620" />
 
Step 11. - <img width="1285" height="623" alt="Screenshot 2026-06-27 at 5 01 07 PM" src="https://github.com/user-attachments/assets/23b3101e-8db4-4f66-afa4-380b442e6b33" />

Step 12. - <img width="1094" height="460" alt="Screenshot 2026-06-27 at 5 01 23 PM" src="https://github.com/user-attachments/assets/ed1f4522-a2c8-480e-9bd0-efb16ec46945" />

Step 13. - <img width="1120" height="143" alt="Screenshot 2026-06-27 at 5 01 45 PM" src="https://github.com/user-attachments/assets/5fab574d-31c1-4e5c-8be0-89750827efce" />

Step 14. - <img width="1248" height="347" alt="Screenshot 2026-06-27 at 5 02 01 PM" src="https://github.com/user-attachments/assets/f1d646ba-70cf-487c-8925-605f3c888075" />

Step 15. - <img width="1085" height="527" alt="Screenshot 2026-06-27 at 5 02 19 PM" src="https://github.com/user-attachments/assets/7b266a92-67ab-4ab3-9ecb-e7fbf3fdf54c" />


### What I Learned

Authentication artifacts preserve valuable evidence during incident response.

Successful logins, failed logins, privilege escalation events, and user switching collectively help investigators reconstruct attacker activity and identify suspicious behavior.

---

## Overall Learning

During this practical I learned how Linux records authentication events.

I can now:

* Investigate successful SSH logins.
* Investigate failed authentication attempts.
* Detect brute-force activity.
* Identify invalid username attacks.
* Analyze `su` activity.
* Analyze `sudo` usage.
* Reconstruct authentication timelines.

These artifacts are essential during Linux forensic investigations because they reveal who accessed the system, when they logged in, and what privileged actions they performed.


---

## VI. Practical 5 - Current Session Investigation

### Task

I wanted to investigate active user sessions on a running Linux system.

### What I Did

* Established remote SSH sessions.
* Logged in using multiple user accounts.
* Created simultaneous active sessions.
* Reviewed currently logged-in users.
* Examined active terminals and remote connections.

### What I Observed

Each active session displayed the username, terminal, login time, and source IP address.

Multiple simultaneous sessions became immediately visible.

Active session information allows investigators to quickly determine who is currently using the system and whether any unexpected users are present.

### Evidence

Step 1. - <img width="714" height="699" alt="Screenshot 2026-06-27 at 5 02 56 PM" src="https://github.com/user-attachments/assets/89d22e57-bf14-4cc6-85b0-8a77322f0c6a" />

Step 2. - <img width="1205" height="248" alt="Screenshot 2026-06-27 at 5 03 12 PM" src="https://github.com/user-attachments/assets/0476aed5-9527-4ad3-8da6-d9e070fb2a52" />


### What I Learned

Live response provides immediate visibility into current system usage.

Unexpected users, unfamiliar source IP addresses, or unauthorized active sessions may indicate an ongoing compromise that requires immediate investigation.

---

## Overall Learning

During this practical I learned how investigators analyze active Linux sessions.

I can now:

* Identify currently logged-in users.
* Review active terminals.
* Identify remote SSH sessions.
* Verify source IP addresses.
* Detect unauthorized active sessions.

These techniques help investigators rapidly assess whether a compromised system is still being actively accessed by an attacker.

---
# Topic 05 — Checksums

## Concept

---

## Attacker Perspective

---

## Investigator Perspective

---

## Practical Validation

### Hypothesis

### Actions Performed

### Observations

### Evidence

### Analysis

### Key Takeaways

---

# Topic 06 — Timestamps

## Concept

---

## Attacker Perspective

---

## Investigator Perspective

---

## Practical Validation

### Hypothesis

### Actions Performed

### Observations

### Evidence

### Analysis

### Key Takeaways

---

# Overall Findings

- Finding 1
- Finding 2
- Finding 3

---

# Investigator Notes

- What helped the investigation?
- What could be misleading?
- What additional artifacts should be reviewed?

---

# Skills Practiced

- Linux File System Analysis
- Artifact Validation
- Timeline Analysis
- Ownership Analysis
- Metadata Analysis
- DFIR Thinking
