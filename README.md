# CI/CD Configuration Files

This folder contains all CI/CD related configuration files for deploying the FastAPI application to Google Cloud Platform.

## 📁 Files

- **cloudbuild.yaml** - Google Cloud Build pipeline configuration
- **Dockerfile** - Production Docker container configuration
- **.gcloudignore** - Files to exclude from builds
- **SETUP_GUIDE.md** - Complete step-by-step setup instructions
- **github-actions.yml** - Optional GitHub Actions workflow

## 🚀 Quick Start

1. Read `SETUP_GUIDE.md` for complete instructions
2. Copy `cloudbuild.yaml`, `Dockerfile`, and `.gcloudignore` to project root
3. Set up GCP project and enable APIs
4. Connect GitHub repository to Cloud Build
5. Create build trigger
6. Push to main branch → automatic deployment!

## 🎯 What Gets Deployed

- FastAPI application
- Containerized with Docker
- Deployed to Google Cloud Run
- Auto-scaling serverless infrastructure
- HTTPS endpoint with custom domain support

## 📊 Pipeline Stages

1. ✅ Install dependencies
2. ✅ Run tests
3. ✅ Build Docker image
4. ✅ Push to Container Registry
5. ✅ Deploy to Cloud Run

## 🔗 Resources

- [Cloud Build Documentation](https://cloud.google.com/build/docs)
- [Cloud Run Documentation](https://cloud.google.com/run/docs)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

---

**Questions?** Check `SETUP_GUIDE.md` for troubleshooting and detailed instructions.
