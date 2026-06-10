
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

# Topic 03 — Ownership and Permissions

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

# Topic 04 — Metadata

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
