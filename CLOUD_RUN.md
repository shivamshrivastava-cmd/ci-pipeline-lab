# Cloud Run deployment mapping

This project currently deploys a Docker Hub image to a Docker Compose runtime.
For a Cloud Run deployment, retain the build stage but replace the registry,
deployment command, and authentication method as follows:

| Current pipeline concern | Cloud Run equivalent |
| --- | --- |
| Push `${DOCKERHUB_USER}/app:${GITHUB_SHA}` to Docker Hub | Push the SHA-tagged image to Artifact Registry, for example `${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY}/app:${GITHUB_SHA}`. |
| `docker compose up -d --no-build` on the runner | `gcloud run deploy app --image <artifact-registry-image> --region <region>`. |
| Docker Hub username and access token | Authenticate GitHub Actions with a Google Cloud service account, preferably through GitHub OIDC / Workload Identity Federation rather than a long-lived key. |

The Cloud Run workflow would need permissions for Artifact Registry writes and
Cloud Run deployment. It should also provide Cloud Run with a runtime service
account that has only the application permissions it needs. This document is a
mapping note only: it does not configure Google Cloud credentials or deploy to
Cloud Run.
