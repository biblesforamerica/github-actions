# GitHub Actions for BFA 🚀

This repository houses reusable **GitHub Actions & Workflows** used across the **Bibles for America (BFA)** GitHub account. These workflows help streamline deployment, testing, and database management for various projects.

## 📂 Available Workflows

### 1️⃣ **Heroku Deploy**

**File:** `.github/workflows/heroku_deploy.yml`  
**Purpose:** Deploys applications to Heroku.

#### 🔧 **Inputs:**

| Name              | Type   | Required | Description                                        |
| ----------------- | ------ | -------- | -------------------------------------------------- |
| `repo`            | string | ✅       | Repository name for deployment.                    |
| `heroku_app_name` | string | ✅       | Target Heroku application name.                    |
| `branch_name`     | string | ❌       | Branch to deploy (defaults to repository default). |

#### 🔑 **Secrets:**

| Name                   | Required | Description                               |
| ---------------------- | -------- | ----------------------------------------- |
| `HEROKU_API_KEY`       | ✅       | Heroku API key for authentication.        |
| `HEROKU_API_KEY_EMAIL` | ✅       | Email associated with the Heroku API key. |

#### 🏗 **Usage Example:**

```yaml
jobs:
  deploy:
    uses: biblesforamerica/github-actions/.github/workflows/heroku_deploy.yml@main
    with:
      repo: biblesforamerica/oms_on_rails
      heroku_app_name: bfaops-staging
      branch_name: staging
    secrets:
      HEROKU_API_KEY: ${{ secrets.HEROKU_API_KEY }}
      HEROKU_API_KEY_EMAIL: ${{ secrets.HEROKU_API_KEY_EMAIL }}
```

### 2️⃣ **Jekyll Cypress Tests**

**File:** `.github/workflows/jekyll_tests.yml`  
**Purpose:** Runs Cypress tests for Jekyll sites.

#### 🔑 **Secrets:**

| Name                                  | Required | Description                            |
| ------------------------------------- | -------- | -------------------------------------- |
| `PAT_GITHUB_ACTIONS_ACCESS_TO_JEKYLL` | ✅       | Personal access token for repo access. |
| `NODE_VERSION`                        | ✅       | Node.js version to use.                |
| `CYPRESS_*` Variables                 | ✅       | Various Cypress authentication tokens. |

#### 🏗 **Usage Example:**

```yaml
jobs:
  test:
    uses: biblesforamerica/github-actions/.github/workflows/jekyll_tests.yml@main
    secrets:
      PAT_GITHUB_ACTIONS_ACCESS_TO_JEKYLL: ${{ secrets.PAT_GITHUB_ACTIONS_ACCESS_TO_JEKYLL }}
      NODE_VERSION: "16"
      CYPRESS_TWILIO_ACCOUNT_SID: ${{ secrets.CYPRESS_TWILIO_ACCOUNT_SID }}
      CYPRESS_TWILIO_AUTH_TOKEN: ${{ secrets.CYPRESS_TWILIO_AUTH_TOKEN }}
```

### 3️⃣ **Reset Heroku Review Database**

**File:** `.github/workflows/heroku_reset_db.yml`  
**Purpose:** Resets the review database in Heroku by restoring it from staging.

#### 🔧 **Inputs:**

| Name              | Type   | Required | Description            |
| ----------------- | ------ | -------- | ---------------------- |
| `repo`            | string | ✅       | Repository name.       |
| `staging_remote`  | string | ✅       | Staging remote URL.    |
| `review_remote`   | string | ✅       | Review app remote URL. |
| `heroku_app_name` | string | ✅       | Target Heroku app.     |

#### 🔑 **Secrets:**

| Name                   | Required | Description                               |
| ---------------------- | -------- | ----------------------------------------- |
| `HEROKU_API_KEY`       | ✅       | Heroku API key for authentication.        |
| `HEROKU_API_KEY_EMAIL` | ✅       | Email associated with the Heroku API key. |

#### 🏗 **Usage Example:**

```yaml
jobs:
  reset-db:
    uses: biblesforamerica/github-actions/.github/workflows/heroku_reset_db.yml@main
    with:
      repo: biblesforamerica/oms_on_rails
      staging_remote: "https://git.heroku.com/bfaops-staging.git"
      review_remote: "https://git.heroku.com/bfaops-review.git"
      heroku_app_name: bfaops-review
    secrets:
      HEROKU_API_KEY: ${{ secrets.HEROKU_API_KEY }}
      HEROKU_API_KEY_EMAIL: ${{ secrets.HEROKU_API_KEY_EMAIL }}
```
