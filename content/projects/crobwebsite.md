---
title: "crobwebsite"
date: 2025-07-18
draft: false
designator: "U2"
summary: "A Django site with accounts, per-user file storage, a to-do list with email reminders, and a staff-only server status page."
tags: ["python", "django", "web"]
math: false
---

A personal Django site built around a few small apps rather than one monolithic feature: accounts and profiles, per-user file uploads/downloads, a to-do list, and a staff-only server diagnostics page.

**Apps:**

- **main** — home page, register, login/logout, profile
- **fileManagement** — per-user file uploads and downloads
- **todo** — per-user to-do list with due dates and reminders
- **serverInfo** — server/system diagnostics, staff-only (hostname, DB path, IPs)

The to-do app's reminder system is a management command (`send_reminders`) meant to run on a schedule (cron/Task Scheduler) — it emails users about items whose remind date has passed, and without SMTP configured it just prints to the console instead, which made local development a lot easier to test.

**Stack:** Python, Django

**Status:** in progress

**Repo:** [github.com/robertscharlie/WebsiteProject](https://github.com/robertscharlie/WebsiteProject)
