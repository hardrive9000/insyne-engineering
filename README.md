# INSYNE Engineering

> **Engineering Knowledge Base**
>
> *Minimum Action Engineered.*

---

## Overview

INSYNE Engineering is a curated technical knowledge base focused on engineering, electronics, embedded systems, software development, cybersecurity and maker projects.

The objective is simple:

- Document knowledge once.
- Preserve it.
- Reuse it indefinitely.

Every article is intended to become a long-term technical reference rather than a temporary blog post.

---

## Topics

The knowledge base currently includes and will continue to expand with content about:

- Electronics
- Embedded Systems
- Arduino
- ESP8266 / ESP32
- Raspberry Pi
- Programming (.NET / C#)
- Reverse Engineering
- Ethical Hacking
- Networking
- Home Automation
- Hardware Design
- Datasheets
- Practical Engineering Projects

---

## Technology Stack

- Obsidian
- Quartz v5
- GitHub Pages
- GitHub Actions

---

## Repository Structure

```text
.github/workflows/    GitHub Actions deployment

content/              Knowledge Base (Markdown)

quartz/               Quartz source and customization
```

---

## Local Development

Install dependencies:

```bash
npm ci
```

Install Quartz plugins:

```bash
npx quartz plugin install
```

Run the local development server:

```bash
npx quartz build --serve
```

---

## Deployment

Every push to the `main` branch automatically:

- installs dependencies
- installs Quartz plugins
- builds the website
- deploys it to GitHub Pages

No manual deployment is required.

---

## Website

https://hardrive9000.github.io/insyne-engineering/

---

## License

See `LICENSE.txt`.