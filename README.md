# Shopify Workflow SOP

## Overview

To maintain code integrity and protect the live storefront, all Shopify development must follow a structured workflow.

- **Never make changes directly to the Dawn - PRODUCTION (DO NOT TOUCH).**
- All development must occur in a dedicated **shopify-test/main** connected to a corresponding **Git branch**.
- GitHub serves as the **single source of truth** for all code.
- Only one designated person should deploy changes to the live branch to prevent conflicts.

Clear communication, proper branch management, and disciplined deployment practices ensure stability and consistency across environments.

---

# Shopify CLI Installation

## Requirements

Before installing Shopify CLI, ensure the following dependencies are installed:

- [Node.js](https://nodejs.org/en/download/): 20.10 or higher
- A Node.js package manager: [npm](https://docs.npmjs.com/getting-started), or [Yarn 1.x](https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable)
- [Git](https://git-scm.com/install/): 2.28.0 or higher

---

## Install Shopify CLI (via npm)

```bash
npm install -g @shopify/cli@latest
```

### Verify Installation

```bash
shopify help
```

---

## Upgrade Shopify CLI

Check your current version:

```bash
shopify version
```

Example output:

```bash
> Current Shopify CLI version: 3.50.0
> 💡 Version 3.51.0 available!
```

To upgrade:

```bash
npm install -g @shopify/cli@latest
```

---

# GitHub as the Source of Truth

## ⚠️ Production Theme Policy: No Direct Changes

The Dawn - PRODUCTION (DO NOT TOUCH) theme must never be edited directly. All development flows through main.

---

# Initial Environment Setup

## Step 1 – Duplicate the Production Theme

1. Navigate to: **Online Store → Themes**
2. Click `...` on `Dawn - PRODUCTION (DO NOT TOUCH)`
3. Select **Duplicate**
4. Rename the duplicate (example): `shopify-test/staging`
5. Access the duplicated theme from the **Theme Library**

This unpublished theme will serve as the staging environment.

---

## Step 2 – Create a Staging Branch in GitHub

```bash
git checkout -b staging
git push -u origin staging
```

---

## Step 3 – Connect the Staging Branch to the Unpublished Theme

Ensure the file `shopify.theme.toml` exists in the project root with the correct:

- Store name 
- Theme ID 
- Environment configuration 

Confirm you are on the staging branch:

```bash
git checkout staging
```

Push the branch to the staging theme:

```bash
shopify theme push --environment=staging
```

At this point:
- The **staging Git branch**
- The **unpublished staging theme**
- Your **local development environment**

are fully synchronized.

---

# Daily Development Workflow

## 1️⃣ Sync Before Starting Work

Always pull the latest changes before development:

```bash
shopify theme pull --environment=staging
git pull origin staging
```

---

## 2️⃣ Develop and Push Code

Work locally (VSCode or preferred IDE), then:

```bash
git push origin staging
```

---

## 3️⃣ Sync Admin Content Changes

If changes are made via the Shopify Admin (e.g., sections, blocks, template JSON updates):

```bash
shopify theme pull --environment=staging
git push origin staging
```

This ensures Git remains aligned with any admin-level content adjustments.

---

# Deploying to Production (Main Branch)

⚠️ **Important:** 
Only one designated person should deploy to production to prevent conflicts or overlapping pushes.

## Deployment Steps

```bash
git checkout main
git pull origin main
git merge staging
git push origin main
```

**No manual Shopify theme push is required for production if the live theme is already connected to GitHub via Shopify Admin integration.**

---

# Key Principles

- Production is protected.
- Staging is the active development environment.
- GitHub is the source of truth.
- Admin changes must be synced back to Git.
- Deployment responsibility is centralized.
