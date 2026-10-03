# NLP for Requirements (Extract, Trace, Flag Vagueness)


## Description
Stakeholder notes about student login, event registration and reminders
were run through an AI assistant for requirement extraction,
classification, duplicate detection and a vagueness check. Every
requirement was traced back to an exact quote in the notes, and anything
without a source was kept as a finding instead of being deleted.

## How to Run
```
python "Lab 5 Sheeza (SP24-BSE-43).py"
```
No extra libraries are needed. Expected output:
- 9 register lines (R1 to R9) showing type, source sentence number and text
- `U1 UNSOURCED | The system should support multiple languages`
- 4 lines of `original -> rewrite` for the vague requirements
- `Duplicate positions: 2 and 5`

## What I Did
1. **Extraction without inventing:** the AI returned 10 items. Checking
   against the source showed one was not in the notes: "the system should
   support multiple languages". The other 9 are genuine.
2. **Classification:** 5 Functional (login, register, reminder, add event,
   remove event) and 4 Non-Functional (fast, secure, quick login, many
   users). Two of the non-functional ones had problems: the duplicate
   "quick" and the vague "many users".
3. **Duplicate:** "Login must also be quick" (sentence 5) restates
   "It should be fast" (sentence 2). The AI listed them as separate items.
4. **Vague requirements rewritten** with a number, a condition and a
   tolerance.

## Requirements Register (Task 1)
Each row has an exact source quote. `sentence_number()` finds the quote
in the notes and the code asserts that it exists.

| ID | Requirement | Type | Source sentence |
|----|-------------|------|-----------------|
| R1 | Students can log in with their university email | Functional | 1 |
| R2 | The app is fast | Non-Functional | 2 |
| R3 | The app is secure | Non-Functional | 2 |
| R4 | Students can register for events | Functional | 3 |
| R5 | Students get a reminder before each event | Functional | 3 |
| R6 | Admins can add events | Functional | 4 |
| R7 | Admins can remove events | Functional | 4 |
| R8 | Login is quick | Non-Functional | 5 |
| R9 | The system handles many users | Non-Functional | 6 |
| U1 | The system should support multiple languages | UNSOURCED | none |

U1 has no quote in the notes, so it is kept in a separate `unsourced`
list. It is a proposal, not a requirement.

## Catching the Model Inventing (Task 2)
- Invented requirement: U1, "the system should support multiple languages".
- What it probably over-read: "handle many users", the model assumed a
  large user base means international users.
- Second run with a different prompt: `<paste your real second prompt>`
- Did U1 appear again? `<yes / no, from your own run>`

## Testable Rewrites (Task 3)
| Original | Rewrite | How to measure |
|----------|---------|----------------|
| It should be fast | Login completes in under 2 seconds on university WiFi, for 95% of attempts | 200 scripted logins from campus network, check 95th percentile is under 2 s |
| secure | Passwords hashed, all traffic HTTPS, account locks after 5 failed logins | Inspect DB for plain-text passwords, scan for non-HTTPS endpoints, try 6 wrong passwords |
| handle many users | 5,000 concurrent users with response time within 10% of the 100-user response time | Ramp load test from 100 to 5,000 virtual users and compare response times |
| a reminder before each event | Reminder sent 24 hours (+/- 5 minutes) before the event to every registered student | Create a test event 24 h ahead with 3 test registrations and compare send timestamps |

The code asserts that every rewrite has a measurement, because a
requirement with no way to measure it is still vague.

<img width="1246" height="275" alt="LAB5" src="https://github.com/user-attachments/assets/e4f5adf9-4968-4fa1-94b2-d2465adefc89" />

