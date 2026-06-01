---
name: project-setup
description: Sets up a new project from scratch with folder structure, tooling, and optional modules. Use this skill whenever the user wants to start a new project, bootstrap an app, initialize a codebase, or says things like "create a new project", "new Next.js app", "new Node app", "setup a project", "start fresh". Always invoke before doing any project initialization work.
---

# Project Setup

## Step 1: Project basics

Ask:
1. **Project name** — will be used as the folder name and in `package.json`
2. **Location** — full path to the parent directory where the project folder should be created

Navigate to that directory:
```bash
cd <location>
```

Confirm you're in the right place before proceeding.

---

## Step 2: Framework

Ask: **What are we building?**

1. **Next.js** — fullstack app (App Router, React, server + client)
2. **Node.js** — backend API only (Fastify / Express)
3. **React SPA** — frontend only (Vite)
4. Other — proceed with first principles

---

## Step 3: Follow the framework guide

Read the corresponding reference guide and follow it from start to finish:

- Next.js → `references/nextjs/setup.md`
- Node.js → `references/nodejs/setup.md`
- React SPA → `references/react/setup.md`

Each guide handles its own architecture questions and optional modules — don't mix between guides.
