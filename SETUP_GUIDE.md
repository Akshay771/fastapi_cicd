# CI/CD Setup Guide - Google Cloud Build + GitHub

## 📋 Prerequisites

1. Google Cloud Platform account
2. GitHub account
3. gcloud CLI installed
4. Project code in GitHub repository

## 🚀 Step-by-Step Setup

### 1. Create GCP Project

```bash
# Set your project ID
export PROJECT_ID="your-project-id"

# Create project
gcloud projects create $PROJECT_ID

# Set as active project
gcloud config set project $PROJECT_ID
```

### 2. Enable Required APIs

```bash
# Enable Cloud Build API
gcloud services enable cloudbuild.googleapis.com

# Enable Cloud Run API
gcloud services enable run.googleapis.com

# Enable Container Registry API
gcloud services enable containerregistry.googleapis.com

# Enable Artifact Registry API (optional, recommended)
gcloud services enable artifactregistry.googleapis.com
```

### 3. Connect GitHub Repository

**Option A: Using GCP Console (Easiest)**

1. Go to Cloud Build > Triggers
2. Click "Connect Repository"
3. Select "GitHub (Cloud Build GitHub App)"
4. Authenticate with GitHub
5. Select your repository
6. Click "Connect"

**Option B: Using gcloud CLI**

```bash
# Install GitHub app
gcloud beta builds repositories create \
    --remote-uri=https://github.com/YOUR_USERNAME/YOUR_REPO.git \
    --connection=YOUR_CONNECTION
```

### 4. Create Build Trigger

**Via Console:**
1. Go to Cloud Build > Triggers
2. Click "Create Trigger"
3. Configure:
   - Name: `fastapi-deploy`
   - Event: `Push to branch`
   - Branch: `^main$`
   - Configuration: `Cloud Build configuration file (yaml or json)`
   - Location: `cicd/cloudbuild.yaml` ⚠️ **Note the cicd/ prefix!**
4. Click "Create"

**Via gcloud CLI:**

```bash
gcloud builds triggers create github \
  --name="fastapi-deploy" \
  --repo-name="YOUR_REPO" \
  --repo-owner="YOUR_USERNAME" \
  --branch-pattern="^main$" \
  --build-config="cicd/cloudbuild.yaml"
```

### 5. Set Up IAM Permissions

```bash
# Get project number
PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)')

# Grant Cloud Build service account Cloud Run Admin role
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:${PROJECT_NUMBER}@cloudbuild.gserviceaccount.com" \
  --role="roles/run.admin"

# Grant Cloud Build service account Service Account User role
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:${PROJECT_NUMBER}@cloudbuild.gserviceaccount.com" \
  --role="roles/iam.serviceAccountUser"
```

### 6. Copy Files to Project Root

The CI/CD needs some files in your project root:

```bash
# Copy cloudbuild.yaml to root (or keep in cicd/ and update trigger path)
cp cicd/cloudbuild.yaml ./

# Copy Dockerfile to root
cp cicd/Dockerfile ./

# Copy .gcloudignore to root
cp cicd/.gcloudignore ./
```

**OR** keep files in `cicd/` folder and update the build trigger to use `cicd/cloudbuild.yaml`

### 7. Push Code to GitHub

```bash
# Initialize git (if not already)
git init

# Add all files
git add .

# Commit
git commit -m "Add CI/CD setup"

# Add remote
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git

# Push to main branch
git push -u origin main
```

## 🔄 CI/CD Pipeline Flow

