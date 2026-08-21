---
title: "websiteproject"
date: 2025-07-18
draft: false
summary: "A personal Django site built around a few small apps — accounts, file storage, a to-do list with email reminders, and more — self-hosted on a Raspberry Pi at home."
tags: ["python", "django", "web", "backend", "self-hosted"]
math: false
image: "images/projects/crobwebsite/random-tools.png"
category: "Web App"
link: "https://github.com/robertscharlie/WebsiteProject"
linkLabel: "Repo"
highlights:
  - "Five small apps: accounts, file storage, to-do, random tools, server diagnostics"
  - "File uploads can now be edited and deleted, with validation on what's accepted"
  - "To-do list emails you reminders on a schedule, via a custom management command"
  - "Hosted it myself on a Raspberry Pi, with my own port forwarding and DNS"
---

A personal Django site built around a few small apps: accounts, per-user file storage, a to-do list with scheduled email reminders, a page of random-generator tools, and a staff-only diagnostics page. Hosted on a Raspberry Pi at home, reachable through manually configured port forwarding and DNS. Originally called crobwebsite — renamed to websiteproject as it grew past a single-purpose script.

![Home page after logging in, listing the apps available on the site](../../images/projects/crobwebsite/home.png)

**main** handles accounts — register, login/logout, profile — through Django's built-in auth, and now redirects you straight to somewhere useful after logging in instead of dropping you back on the login page.

**todo** is a per-user to-do list with title, due date, an optional remind date, and now a priority level. A management command (`send_reminders`), meant to run on a schedule, emails users about any item whose remind date has passed and hasn't already been sent — editing the remind date un-marks it, so pushing it back doesn't get silently skipped. Without SMTP configured it just prints the email to the console, which made local testing much easier.

![To-do list with a mix of open and completed items, each showing its due and remind dates](../../images/projects/crobwebsite/todo.png)

**fileManagement** is per-user file storage, each file scoped to the account that uploaded it. Beyond upload and download, files can now be edited and deleted, and uploads are validated before they're accepted.

**randomTools** is a grab-bag of client-side generators on one page: coin flip, dice roller, random number, spin-the-wheel picker, magic 8-ball, password generator, colour swatch, and list shuffler. All local JavaScript — no database involved.

**serverInfo** is a staff-only diagnostics page — Python/Django versions, installed apps, database engine, OS/CPU/memory/disk stats — gated by Django's `is_staff` flag rather than a hardcoded check.

![Server Information page, showing the Python/Django versions and installed apps](../../images/projects/crobwebsite/server-info.png)

Configuration (`SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`) comes from environment variables rather than hardcoded, with a `.env.example` documenting what's needed; `db.sqlite3` and uploaded `media/` are gitignored.

**Stack:** Python, Django

**Status:** in progress

**Repo:** [github.com/robertscharlie/WebsiteProject](https://github.com/robertscharlie/WebsiteProject)
