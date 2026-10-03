Phishing Email Analyser (Directing an AI Coding Agent to Build a Privacy First Triage Tool)

A free, private phishing email analyser that runs entirely in your browser paste an email, get a 0–100 risk score with plain-English explanations. No servers, no tracking

 # Phishing Email Analyser (Directing an AI Coding Agent to Build a Privacy First Triage Tool)

`HTML` · `CSS` · `JavaScript` · `AI Assisted Development` · `Phishing Analysis` · `Social Engineering` · `Explainable Scoring`

## Overview
This lab was a different kind of hands-on practice from my usual tool-based work. Instead of running someone else's security tool, I wanted to understand how phishing detection actually works by getting one built. I have no programming background, so I used an AI coding agent (Cline running DeepSeek V4.1 Flash) to write the code while I acted as the person setting the requirements, approving the plan, and testing the result.

The finished product is a single page web app called **Phishing Email Analyser**. You paste in a suspicious email, click "Analyse Email", and it returns a risk score from 0 to 100, a classification (Low Risk, Suspicious or High Risk), a plain English explanation of every warning sign it found with the evidence quoted, and practical next steps. Everything runs inside the browser. No email text is ever uploaded anywhere.

I want to be upfront about what this project is and is not. The agent wrote the code. What I did was write the brief, set the rules, review the plan, run the tool, and test it against fictional emails to see whether the scores made sense. The goal was to learn the logic of phishing detection, not to pretend I became a developer in one evening.

## Objective
Understand how social engineering warning signs can be turned into an explainable scoring system, and practise writing clear security requirements (privacy, safety, honesty about limits) that a tool has to respect.

## Environment
- **Host machine:** Windows 11 laptop, project stored in a local Documents folder
- **AI coding agent:** Cline with DeepSeek V4.1 Flash, working inside an empty project folder called `Phishing Analyser`
- **Browser used for testing:** Microsoft Edge, opening `index.html` straight from disk
- **Software installed for this project:** none. The agent checked the machine and found Node.js, npm, Git and Python were not installed. That did not matter, because my requirements ruled out backends and build tools anyway.

## Tools I Used

| Tool | What It Does | Why I Used It |
|------|--------------|----------------|
| **Cline (AI coding agent)** | Plans and writes code files inside a chosen folder | Did the actual coding, since I cannot code |
| **Notepad** | Plain text editor | Wrote and refined my full project brief before sending it |
| **Microsoft Edge** | Web browser | Ran the finished analyser locally and tested it |
| **HTML, CSS, JavaScript** | The three languages every browser understands | Kept the project static, free to host and easy to read |
| **Fictional demo emails** | Invented phishing and legitimate samples | Safe test inputs using reserved example domains only |

## What I Did

### Writing the Brief
Before touching the agent I wrote my requirements in Notepad. This turned out to be the most important step. I made the brief specific in four areas:

1. **What it should do:** a 0 to 100 score, a classification, an explanation of every warning sign, and recommended next steps.
2. **Privacy:** analysis must happen locally in the browser, with no server, AI provider, analytics, tracking or telemetry.
3. **Simplicity:** no database, user accounts, paid APIs, API keys, frameworks like React, or unnecessary dependencies, so it could be hosted as a free static site.
4. **Honesty:** the tool must never claim it can definitively decide whether an email is malicious, and must warn people not to click links or open attachments just to test them.

I also listed the indicators I wanted it to look for: urgency, threats of suspension, password requests, MFA code requests, payment or gift card requests, bank detail changes, suspicious and shortened URLs, raw IP address URLs, misleading link text, impersonation language, unusual or executable attachments, requests to enable macros, unexpected invoices, credential harvesting language, sender and Reply To mismatches, and SPF, DKIM and DMARC failures when headers are available.

### Making the Agent Plan Before Building
Because I cannot read code, I added a rule to the brief: explain significant technical decisions in plain English, ask approval before consequential commands, and inspect the empty workspace and present a plan before writing anything. The agent followed that. It confirmed the folder was empty, noted that nothing needed installing, and proposed seven files with a one line purpose for each, including a separate file for demo emails and a self test page. It also explained why it split the project into several files instead of one big one (so the rules I wanted to learn from stay separate from the design and the sample data).

### The Build
Once the plan was clear, the agent created the project files. The main logic file (`app.js`) opens with comments stating that there are no network requests, no analytics and no data uploads, then defines a fixed scoring configuration at the top: a maximum score of 100, a High Risk threshold of 60, a Suspicious threshold of 25, and the order that warning categories appear in the results. All of the files were created within about 20 minutes of me sending the brief.

### Running It and Testing with Fictional Emails
I opened `index.html` in Edge and tested two of the built in fictional examples.

**Test 1: Obvious phishing email.** A fake PayPal style message with an account suspension threat, a 24 hour deadline, a password request, and a raw IP address link.
- Result: **100 / 100, High Risk**, with 8 warning signs found.
- The raw points added up to 113, and the interface said so ("sum of every warning sign (113), capped at 100"), which is exactly the kind of transparency I asked for.
- The two biggest contributors were "Requests your password" (+25) and "Credential harvesting language" (+15), each with the matching phrase from the email quoted as evidence.

**Test 2: Fake parcel delivery email.** A fake DHL message asking for a small redelivery fee.
- Result: **48 / 100, Suspicious**, with 4 warning signs found.
- The findings were payment request (+20), suspicious URL characteristics (+10), impersonation or authority language (+10) and urgency (+8), which adds up to 48.
- The URL finding explained itself well: it pointed out that the trusted name "dhl" was placed in a sub domain while the real domain was something else entirely, and it flagged a sender domain with several hyphens as a possible look alike.
- One thing I noticed: the impersonation rule fired partly because of the vague greeting "Dear Customer". That is a genuine phishing signal, but it also shows how a single generic phrase can add points, which connects to the false positive discussion below.

## What's in This Repo

```
phishing-email-analyser/
├── README.md                # This file
├── index.html               # The page the visitor sees
├── styles.css               # Visual design (dark security theme, mobile friendly)
├── app.js                   # Detection rules, scoring and result display
├── demo-emails.js           # Six fictional demonstration emails
├── tests.html               # Self test page that runs automatic checks
├── tests.js                 # The checks used by tests.html
└── screenshots/
    ├── 01-prompt-part-1.png
    ├── 02-prompt-part-2.png
    ├── 03-agent-with-prompt-loaded.png
    ├── 04-empty-project-folder.png
    ├── 05-agent-plan.png
    ├── 06-agent-writing-app-js.png
    ├── 07-generated-files.png
    ├── 08-analyser-home.png
    ├── 09-obvious-phish-input.png
    ├── 10-obvious-phish-result.png
    ├── 11-parcel-input.png
    ├── 12-parcel-result.png
    └── 13-parcel-url-and-urgency-findings.png
```

## How the Scoring Works
Every warning sign has a fixed point value. The points are added together and the total is capped at 100.

| Score | Classification |
|-------|----------------|
| 0 to 24 | Low Risk |
| 25 to 59 | Suspicious |
| 60 to 100 | High Risk |

Some of the weights: password request 25, MFA code request 22, bank detail change 22, payment or gift card request 20, sender and Reply To mismatch 18, executable attachment 18, enable macros request 16, credential harvesting language 15, raw IP URL 15, suspension threat 12, shortened URL 12, SPF or DMARC failure 12, suspicious URL 10, impersonation 10, urgency 8.

Each rule only scores once, no matter how many trigger words it finds. Three different urgent phrases score the same as one. That is a deliberate choice so that word counting cannot inflate a result.

## Skills I Picked Up
- **Turning a vague idea into clear requirements.** The quality of what I got back depended directly on how specific my brief was, especially the privacy and honesty rules.
- **Reading an explainable score.** A number on its own is useless. A number next to the exact reasons, with the evidence quoted, is something an analyst can actually check and challenge.
- **Spotting social engineering patterns.** Urgency, threats, requests for credentials or payment, and vague greetings do most of the work in phishing. The technical tricks matter, but the pressure language is often the giveaway.
- **Reading a URL backwards.** A link like `dhl.example-delivery-payments.net` looks like DHL but the real domain is at the end. The tool explained this in plain language, which reinforced it for me.
- **Treating missing data carefully.** Missing headers or missing authentication results are not the same as "passed". A message without headers is not cleared, it is just unchecked.
- **Why tests matter.** I did not write the tests, but the project notes show the self test page caught real problems that nobody would have spotted by eye, and it made me appreciate that you cannot trust a detection tool you have not tested against known inputs.

## How This Applies in the Real World
Phishing is still one of the most common ways attackers get initial access, and a large part of an entry level security analyst's day involves triaging reported suspicious emails. The workflow is the same as what this tool imitates: check the sender and headers, examine the links without clicking them, look for pressure and credential requests, and decide whether to escalate.

The privacy design is also realistic. Real organisations are cautious about pasting internal emails into outside services, so a tool that provably makes no network calls is a more honest answer than one that merely promises to be careful.

## Where I'm Coming From
I'm making the jump into cybersecurity from a background in **healthcare**. It is a different field on paper, but a lot of the habits carry over: following procedures carefully, protecting sensitive information, and staying calm and methodical when something is not behaving as expected. I'm currently studying for **CompTIA Security+** and building labs like this one to get real hands on reps in.

I'll also be honest that I did not write this code myself. I used an AI agent for that and I am treating it as a learning project, not as proof of programming skill. What I can speak to is the thinking behind it: what the tool looks for, why each indicator matters, how the scoring is weighted, and where it fails.

## What I Want to Learn Next
- Reading `app.js` properly, one rule at a time, until I can explain every detection rule and change a weight with confidence
- Testing more legitimate looking emails to measure the false positive rate more formally
- Learning how real mail servers report SPF, DKIM and DMARC results in headers
- Hosting the finished site for free on a static host such as GitHub Pages

## Limitations and What I'd Do Differently in Production
- **It matches words and patterns, not meaning.** A carefully worded phishing email that avoids the usual trigger phrases can score low, and an innocent email can trip a rule. A low score is never proof an email is genuine.
- **Several checks need the full email source.** Sender and Reply To mismatches and SPF, DKIM and DMARC checks only work if headers are pasted. Link text checks need the HTML source and the matching checkbox ticked.
- **It never visits or scans anything.** URLs are judged purely on how they look. It cannot check reputation, domain age, certificates or live content.
- **Attachments are judged by filename only.** The tool cannot open a file, so a harmful file with an innocent name would not be caught.
- **Not covered at all:** text inside images, QR code phishing, non English emails, heavy HTML obfuscation, and blocklist or WHOIS lookups.
- **The score is a heuristic, not a measurement.** The 0 to 100 number is a transparent weighting I can explain, not a scientific probability.
- **I did not independently review the generated code.** In a real setting I would have the code reviewed before trusting a tool like this with anything important.

## References
- [CompTIA Security+ (SY0-701) Exam Objectives](https://www.comptia.org/certifications/security)
- [CISA: Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/news-events/news/avoiding-social-engineering-and-phishing-attacks)
- [MDN Web Docs: HTML, CSS and JavaScript](https://developer.mozilla.org/)
- Cline AI coding agent with DeepSeek V4.1 Flash, used to generate the code from my written brief
