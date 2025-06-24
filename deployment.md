# Deployment Guide

This guide explains how to deploy your Vue.js LaTeX app to GitHub Pages using the `gh-pages` branch with a pull request workflow.

## Prerequisites

- A GitHub repository with your Vue.js app
- Node.js and npm installed locally
- GitHub account with repository access

## Setup

### 1. Repository Configuration

Your `package.json` is already configured with the necessary scripts:

```json
{
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  },
  "devDependencies": {
    "gh-pages": "^6.3.0"
  }
}
```

### 2. GitHub Pages Settings

1. Go to your repository on GitHub
2. Click **Settings** tab
3. Scroll down to **Pages** section (in the left sidebar)
4. Under **Source**, select **Deploy from a branch**
5. Choose **gh-pages** branch
6. Select **/(root)** folder
7. Click **Save**

## Deployment Workflow

### Recommended Workflow: Pull Request to gh-pages

The `.github/workflows/deploy.yml` is configured to automatically deploy when you create a pull request to the `gh-pages` branch.

**Workflow:**

1. Make changes to your code in a feature branch
2. Create a pull request targeting the `gh-pages` branch
3. The GitHub Action will automatically build and deploy
4. Review the changes in the deployed preview
5. Merge the pull request to update the live site

**Steps:**

```bash
# Create a feature branch
git checkout -b feature/new-feature

# Make your changes
# ... edit files ...

# Commit and push
git add .
git commit -m "Add new feature"
git push origin feature/new-feature

# Create pull request to gh-pages branch on GitHub
```

### Alternative: Manual Deployment

For quick deployments or testing, you can still deploy manually:

```bash
npm run deploy
```

This will:

- Build your Vue app (`npm run build`)
- Create/update the `gh-pages` branch
- Push the built files to GitHub

## What Happens During Deployment

1. **Build Process**: Your Vue app is built using Vite, creating optimized files in the `dist` folder
2. **Branch Creation**: The `gh-pages` package creates a `gh-pages` branch if it doesn't exist
3. **File Upload**: The contents of the `dist` folder are pushed to the `gh-pages` branch
4. **GitHub Pages**: GitHub serves your app from the `gh-pages` branch

## Accessing Your Deployed App

Your app will be available at:

```
https://[your-username].github.io/[repository-name]/
```

For example, if your username is `john` and repository is `latex-app`:

```
https://john.github.io/latex-app/
```

## Benefits of Pull Request Workflow

- **Preview Changes**: See your changes before they go live
- **Review Process**: Code can be reviewed before deployment
- **Rollback Capability**: Easy to revert changes by reverting the PR
- **Better Collaboration**: Team members can review and approve changes
- **Safer Deployments**: No accidental deployments from direct pushes

## Troubleshooting

### No gh-pages branch appears

- Check that the GitHub Action completed successfully
- Verify GitHub Pages is configured to use the `gh-pages` branch
- Try running `npm run deploy` locally

### Build fails

- Check that all dependencies are installed: `npm install`
- Verify your Vue app builds locally: `npm run build`
- Check the GitHub Actions logs for specific error messages

### Page not found (404)

- Ensure GitHub Pages is enabled in repository settings
- Wait a few minutes after deployment (GitHub Pages can take time to update)
- Check that the `gh-pages` branch contains your built files

### Pull request not triggering deployment

- Ensure the PR is targeting the `gh-pages` branch
- Check that the workflow file is in the correct location (`.github/workflows/deploy.yml`)
- Verify the workflow syntax is correct

## File Structure After Deployment

The `gh-pages` branch will contain:

```
gh-pages/
├── index.html
├── assets/
│   ├── css/
│   └── js/
└── ... (other built files)
```

## Updating Your App

To update your deployed app:

1. Create a feature branch from `main`
2. Make your code changes
3. Create a pull request targeting `gh-pages` branch
4. Review the changes in the deployed preview
5. Merge the pull request to update the live site

## Local Development

For local development, use:

```bash
npm run dev
```

This starts the development server at `http://localhost:5173` (or similar port).
