[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19994036&assignment_repo_type=AssignmentRepo)
# Deployment and DevOps for MERN Applications

This assignment focuses on deploying a full MERN stack application to production, implementing CI/CD pipelines, and setting up monitoring for your application.

## Assignment Overview

You will:
1. Prepare your MERN application for production deployment
2. Deploy the backend to a cloud platform
3. Deploy the frontend to a static hosting service
4. Set up CI/CD pipelines with GitHub Actions
5. Implement monitoring and maintenance strategies

## Getting Started

1. Accept the GitHub Classroom assignment invitation
2. Clone your personal repository that was created by GitHub Classroom
3. Follow the setup instructions in the `Week7-Assignment.md` file
4. Use the provided templates and configuration files as a starting point

## Files Included

- `Week7-Assignment.md`: Detailed assignment instructions
- `.github/workflows/`: GitHub Actions workflow templates
- `deployment/`: Deployment configuration files and scripts
- `.env.example`: Example environment variable templates
- `monitoring/`: Monitoring configuration examples

## Requirements

- A completed MERN stack application from previous weeks
- Accounts on the following services:
  - GitHub
  - MongoDB Atlas
  - Render, Railway, or Heroku (for backend)
  - Vercel, Netlify, or GitHub Pages (for frontend)
- Basic understanding of CI/CD concepts

## Deployment Platforms

### Backend Deployment Options
- **Render**: Easy to use, free tier available
- **Railway**: Developer-friendly, generous free tier
- **Heroku**: Well-established, extensive documentation

### Frontend Deployment Options
- **Vercel**: Optimized for React apps, easy integration
- **Netlify**: Great for static sites, good CI/CD
- **GitHub Pages**: Free, integrated with GitHub

## CI/CD Pipeline

The assignment includes templates for setting up GitHub Actions workflows:
- `frontend-ci.yml`: Tests and builds the React application
- `backend-ci.yml`: Tests the Express.js backend
- `frontend-cd.yml`: Deploys the frontend to your chosen platform
- `backend-cd.yml`: Deploys the backend to your chosen platform

## Submission

Your work will be automatically submitted when you push to your GitHub Classroom repository. Make sure to:

1. Complete all deployment tasks                                                                                                                                         const express = require('express');
const mongoose = require('mongoose');
const app = express();

const PORT = process.env.PORT || 5000;
const MONGODB_URI = process.env.MONGODB_URI;

mongoose.connect(MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.log(err));

app.use(express.json());

// Your routes here

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
2. Set up CI/CD pipelines with GitHub Actions                                                                                                                          github/workflows/ci-cd.yml
yaml
Copy
Edit
name: MERN CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  backend:
    name: Build & Test Backend
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./server

    steps:
    - uses: actions/checkout@v3

    - name: Use Node.js 18
      uses: actions/setup-node@v3
      with:
        node-version: 18

    - name: Install dependencies
      run: npm install

    # Optional: run backend tests if you have any
    # - name: Run tests
    #   run: npm test

  frontend:
    name: Build Frontend
    runs-on: ubuntu-latest
    needs: backend
    defaults:
      run:
        working-directory: ./client

    steps:
    - uses: actions/checkout@v3

    - name: Use Node.js 18
      uses: actions/setup-node@v3
      with:
        node-version: 18

    - name: Install dependencies
      run: npm install

    - name: Build React app
      run: npm run build

  deploy-backend:
    name: Deploy Backend to Render
    runs-on: ubuntu-latest
    needs: backend

    steps:
    - name: Trigger Render deployment via API
      env:
        RENDER_API_KEY: ${{ secrets.RENDER_API_KEY }}
        RENDER_SERVICE_ID: ${{ secrets.RENDER_SERVICE_ID }}
      run: |
        curl -X POST \
          -H "Authorization: Bearer $RENDER_API_KEY" \
          -H "Accept: application/json" \
          https://api.render.com/deploy/srv-${RENDER_SERVICE_ID}

  deploy-frontend:
    name: Deploy Frontend to Vercel
    runs-on: ubuntu-latest
    needs: frontend

    steps:
    - uses: actions/checkout@v3

    - name: Install Vercel CLI
      run: npm install -g vercel

    - name: Deploy to Vercel
      env:
        VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}
        VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
        VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
      run: |
        vercel deploy --prod --token $VERCEL_TOKEN --confirm \
          --scope $VERCEL_ORG_ID \
          --project $VERCEL_PROJECT_ID
3. Deploy both frontend and backend to production                                                                                                                     Backend (/server)
Make sure backend listens on process.env.PORT

Use MONGODB_URI env variable for Mongo connection string

Add "start": "node server.js" (or your main entry) in /server/package.json

Frontend (/client)
Use an environment variable for API base URL in React (REACT_APP_API_URL)

In React API calls, reference process.env.REACT_APP_API_URL

Run npm run build locally to test a production build before deploy

Step 2: Deploy backend on Render
Log in to https://render.com

Create New Web Service → Connect GitHub repo

Select your backend directory (e.g. /server)

Build Command: npm install

Start Command: npm start

Set Environment Variables:

MONGODB_URI = your MongoDB Atlas connection string

Deploy and wait for it to finish.

Note the backend URL (something like https://your-backend.onrender.com).


4. Document your deployment process in the README.md                                                                                                                     MERN Stack Application

## Deployment Process

This document explains how to deploy the MERN stack application to production using MongoDB Atlas, Render (backend), and Vercel (frontend).

---

### 1. Prerequisites

- GitHub repository with full MERN codebase.
- MongoDB Atlas account with a cluster and user set up.
- Render account for backend deployment.
- Vercel account for frontend deployment.

---

### 2. Backend Deployment (Render)

1. Go to [Render](https://render.com) and log in.
2. Create a **New Web Service**.
3. Connect your GitHub repository.
4. Select the backend directory (e.g. `/server`).
5. Set build command:
    ```
    npm install
    ```
6. Set start command:
    ```
    npm start
    ```
7. Add environment variable:
    - `MONGODB_URI` — your MongoDB Atlas connection string.
8. Deploy the service.
9. Once deployed, note the backend URL, e.g., `https://your-backend.onrender.com`.

---

### 3. Frontend Deployment (Vercel)

1. Go to [Vercel](https://vercel.com) and log in.
2. Create a **New Project**.
3. Connect your GitHub repository.
4. Set the root directory to `/client` (where React app lives).
5. Set build command:
    ```
    npm run build
    ```
6. Set output directory:
    ```
    build
    ```
7. Add environment variable:
    - `REACT_APP_API_URL` — URL of the backend deployed on Render (e.g., `https://your-backend.onrender.com`).
8. Deploy the frontend.
9. After deployment, open the frontend URL and verify the app works correctly.

---

### 4. Environment Variables Summary

| Service        | Variable          | Description                          |
|----------------|-------------------|------------------------------------|
| Backend (Render) | MONGODB_URI       | MongoDB Atlas connection string    |
| Frontend (Vercel) | REACT_APP_API_URL | URL to deployed backend API        |

---

### 5. Running Locally

- Backend:

  ```bash
  cd server
  npm install
  export MONGODB_URI=your_mongo_uri
  npm start
Frontend:

bash
Copy
Edit
cd client
npm install
export REACT_APP_API_URL=http://localhost:5000
npm start
5. Include screenshots of your CI/CD pipeline in action                                                                                                               mern-testing/
├── docs/
│   └── images/
│       ├── ci-pipeline-overview.png
│       ├── ci-tests-running.png
│       └── ci-build-success.png
CI/CD Pipeline

Our project uses an automated CI/CD pipeline that runs tests, builds the project, and deploys code on every push.

### Pipeline Overview

![Pipeline Overview](docs/images/ci-pipeline-overview.png)

### Running Tests

![Running Tests](docs/images/ci-tests-running.png)

### Build Success

![Build Success](docs/images/ci-build-success.png)
6. Add URLs to your deployed applications                                                                                                                             MERN Stack Application

## Deployment Process

This document explains how to deploy the MERN stack application to production using MongoDB Atlas, Render (backend), and Vercel (frontend).

---

### 1. Prerequisites

- GitHub repository with full MERN codebase.
- MongoDB Atlas account with a cluster and user set up.
- Render account for backend deployment.
- Vercel account for frontend deployment.

---

### 2. Backend Deployment (Render)

1. Go to [Render](https://render.com) and log in.
2. Create a **New Web Service**.
3. Connect your GitHub repository.
4. Select the backend directory (e.g. `/server`).
5. Set build command:
    ```
    npm install
    ```
6. Set start command:
    ```
    npm start
    ```
7. Add environment variable:
    - `MONGODB_URI` — your MongoDB Atlas connection string.
8. Deploy the service.
9. Once deployed, note the backend URL, e.g.,  
   **Backend URL:** [https://your-backend.onrender.com](https://your-backend.onrender.com)

---

### 3. Frontend Deployment (Vercel)

1. Go to [Vercel](https://vercel.com) and log in.
2. Create a **New Project**.
3. Connect your GitHub repository.
4. Set the root directory to `/client` (where React app lives).
5. Set build command:
    ```
    npm run build
    ```
6. Set output directory:
    ```
    build
    ```
7. Add environment variable:
    - `REACT_APP_API_URL` — URL of the backend deployed on Render (e.g., `https://your-backend.onrender.com`).
8. Deploy the frontend.
9. After deployment, open the frontend URL and verify the app works correctly.  
   **Frontend URL:** [https://your-frontend.vercel.app](https://your-frontend.vercel.app)

---

### 4. Environment Variables Summary

| Service          | Variable          | Description                          |
|------------------|-------------------|------------------------------------|
| Backend (Render) | MONGODB_URI       | MongoDB Atlas connection string    |
| Frontend (Vercel)| REACT_APP_API_URL | URL to deployed backend API        |

---

### 5. Running Locally

- Backend:

  ```bash
  cd server
  npm install
  export MONGODB_URI=your_mongo_uri
  npm start
Frontend:

bash
Copy
Edit
cd client
npm install
export REACT_APP_API_URL=http://localhost:5000
npm start


## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [MongoDB Atlas Documentation](https://docs.atlas.mongodb.com/)
- [Render Documentation](https://render.com/docs)
- [Railway Documentation](https://docs.railway.app/)
- [Vercel Documentation](https://vercel.com/docs)
- [Netlify Documentation](https://docs.netlify.com/) 
