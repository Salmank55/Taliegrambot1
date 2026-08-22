# Taliegrambot1 Skills Lab

A practical collection of privacy-aware skills for building useful software with limited connectivity, modest budgets, and user-controlled data.

> চারটি আলাদা skill—offline Android, local LLM, offline document processing, এবং frugal automation—এক জায়গায় রাখা হয়েছে। প্রতিটিতে বাস্তব example, reusable template এবং demo GIF আছে।

## Included skills

| Skill | Best for | Example |
| --- | --- | --- |
| [`offline-android-studio`](skills/offline-android-studio/SKILL.md) | Offline-first Android apps, local storage, dependency-minimal builds, and APK release checks | [Offline Notes app](skills/offline-android-studio/examples/offline-notes-app.md) |
| [`local-llm-workbench`](skills/local-llm-workbench/SKILL.md) | Private local-model workflows, structured extraction, evaluation, and cloud-fallback boundaries | [Support-ticket extractor](skills/local-llm-workbench/examples/support-ticket-extractor.md) |
| [`offline-document-lab`](skills/offline-document-lab/SKILL.md) | Local OCR, PDF and image processing, table extraction, evidence preservation, and quality checks | [Invoice batch OCR](skills/offline-document-lab/examples/invoice-batch-ocr.md) |
| [`frugal-automation-architect`](skills/frugal-automation-architect/SKILL.md) | Low-cost bots, scheduled jobs, webhooks, safe retries, observability, and credential hygiene | [Telegram daily digest](skills/frugal-automation-architect/examples/telegram-daily-digest.md) |

## Demo previews

### Offline Android Studio

![Offline Android workflow demo](skills/offline-android-studio/assets/demo.gif)

### Local LLM Workbench

![Local LLM workflow demo](skills/local-llm-workbench/assets/demo.gif)

### Offline Document Lab

![Offline document workflow demo](skills/offline-document-lab/assets/demo.gif)

### Frugal Automation Architect

![Frugal automation workflow demo](skills/frugal-automation-architect/assets/demo.gif)

## Reusable templates

Each skill includes a copy-ready template:

- [Offline app brief](skills/offline-android-studio/templates/offline-app-brief.md)
- [Local LLM task specification](skills/local-llm-workbench/templates/llm-task-spec.yaml)
- [Document job manifest](skills/offline-document-lab/templates/document-job-manifest.csv)
- [Automation contract](skills/frugal-automation-architect/templates/automation-contract.yaml)

## Design principles

These skills favor **offline-first execution**, minimum data collection, free or open-source components, explicit failure handling, and honest reporting of what was actually tested. They are written as reusable instructions for AI-assisted software and automation work rather than as end-user application manuals.

## Suggested use

Open the skill that matches the task, read its `SKILL.md`, and apply the workflow from the constraint brief through validation. Use the included example as a starting point, then copy the template into your own project. Keep secrets, private documents, generated artifacts, and personal data outside the repository.

## Search keywords

offline-first, Android, local LLM, offline AI, OCR, PDF processing, document extraction, Telegram bot, webhook, cron job, privacy, open-source, free automation.

## Repository status

This repository is maintained as a focused public skill collection. Each skill is self-contained so it can be copied, reviewed, or adapted independently.
