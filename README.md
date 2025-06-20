# 🚀 Deploy to Cloud Run using GitHub Actions with .env from GCP Secret Manager

This repository shows how to deploy a Dockerized application to **Google Cloud Run**, while securely managing environment variables using **Google Secret Manager**, and deploying via **GitHub Actions**.

---------------------------------------------------------------------------------------------------------------------

## 📌 What You’ll Learn

- ✅ How to store `.env` securely using **GCP Secret Manager**
- 🧩 How to manage **common and service-specific env files**
- 📥 How to **combine secrets** at runtime
- 🐳 How to use `.env` during **Docker build**
- 🚀 How to **deploy to Cloud Run** using **GitHub Actions**
- 🔐 How to inject `.env` at **runtime using `--env-vars-file`**

---

## 🧱 Project Structure

```

.
├── .github/workflows/deploy.yml     # GitHub Actions CI/CD workflow
├── Dockerfile                       # Dockerfile for your app
├── .dockerignore
├── src/                             # Your source code
└── README.md

````

---------------------------------------------------------------------------------------------------------------------

## 🔐 Secret Setup in Google Cloud

Create two secrets in [GCP Secret Manager](https://console.cloud.google.com/security/secret-manager):

### 1. Common Environment Variables

**Secret Name:** `env-common`  
**Example content:**

```env
DB_HOST=sql.example.com
API_KEY=abc123
````

### 2. Service-Specific Environment Variables

**Secret Name:** `env-myservice`
**Example content:**

```env
PORT=8080
DEBUG=true
```

---------------------------------------------------------------------------------------------------------------------

## ⚙️ GitHub Actions Workflow

> 📍 Get this from `.github/workflows/deploy.yml` in your repo

---------------------------------------------------------------------------------------------------------------------

## 🐳 Dockerfile Example

> This shows how `.env` can be used during the build

```Dockerfile
# Dockerfile

FROM node:18-alpine
WORKDIR /app

COPY . .
COPY .env .env  # Optional: Needed if app uses envs inside container

RUN npm install

CMD ["node", "src/index.js"]
```

---------------------------------------------------------------------------------------------------------------------

## 📘 How .env is Used

| Stage          | Purpose                                     | How to Use                           |
| -------------- | ------------------------------------------- | ------------------------------------ |
| **Build Time** | Inject config into Docker image (if needed) | `COPY .env .env` or `--build-arg`    |
| **Run Time**   | Inject dynamic secrets into Cloud Run       | `--env-vars-file .env` during deploy |

---------------------------------------------------------------------------------------------------------------------

## 🔑 GitHub Secrets Required

| Secret Name      | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `GCP_SA_KEY`     | JSON key of a service account with deploy + secret access |
| `GCP_PROJECT_ID` | Your GCP project ID                                       |

---------------------------------------------------------------------------------------------------------------------

## 🧠 Best Practices

* ✅ Never commit your `.env` files into Git
* 🔁 Use `common.env` for shared keys across services
* 📦 Use `env-<service>` for service-specific keys
* 🔐 Use IAM to restrict access to secrets only for CI/CD service accounts
* 🚫 Don’t hardcode secrets in workflows

---------------------------------------------------------------------------------------------------------------------

## 📚 Resources

* [📘 Cloud Run Docs](https://cloud.google.com/run/docs)
* [🔐 GCP Secret Manager Docs](https://cloud.google.com/secret-manager/docs)
* [🤖 GitHub Actions for GCP](https://github.com/google-github-actions)
* [🐳 Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
