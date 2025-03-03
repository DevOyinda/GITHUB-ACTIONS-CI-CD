# GITHUB-ACTIONS-CI-CD
# CI/CD with Docker, Node.js, and GitHub Actions

This project demonstrates the setup of Continuous Integration and Continuous Deployment (CI/CD) pipelines using **GitHub Actions** and **Docker**. It focuses on building and deploying a Node.js application in a Docker container with automated builds triggered by events such as `push` and `pull_request`.

## Table of Contents

- [Project Overview](#project-overview)
- [Technologies Used](#technologies-used)
- [Setup and Installation](#setup-and-installation)
- [How It Works](#how-it-works)
- [GitHub Actions Workflows](#github-actions-workflows)
- [Docker Image Build Process](#docker-image-build-process)
- [Running Locally](#running-locally)
- [License](#license)

## Project Overview

This project consists of a simple **Node.js** application that is containerized using **Docker**. The application uses GitHub Actions for automating the CI/CD pipeline. The pipeline is triggered when there are changes to the `main` branch or when pull requests are made to it. The Docker image is built and pushed to a Docker registry, and the containerized app is deployed to the chosen platform.

## Technologies Used

- **Node.js**: JavaScript runtime used for server-side development.
- **Express**: Web framework used in the Node.js application.
- **Docker**: Platform for developing, shipping, and running applications in containers.
- **GitHub Actions**: Automation tool for continuous integration and delivery (CI/CD).
- **npm**: Node.js package manager for dependency management.

## Setup and Installation

To set up this project locally, follow the steps below:

1. **Clone the repository**:

   ```bash
   git clone https://github.com/yourusername/your-repository-name.git
   cd your-repository-name


2. Creating the `index.js` File

The `index.js` file serves as the entry point for your Node.js application. This file sets up an Express server that listens on port 3000 and responds with a simple "Hello World!" message when accessed at the root URL.

#### Steps to Create `index.js`:
1. Create a file named `index.js` in your project directory.
2. Add the following code to set up the server:

```bash
const express = require('express'); // Import express
const app = express(); // Create an instance of express

// Define a route for the root URL
app.get('/', (req, res) => {
  res.send('Hello World!'); // Respond with "Hello World!" when accessing the root URL
});

// Set the server to listen on port 3000
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

3. Running the Application Locally
To run your Node.js application locally, follow these steps:

Open a terminal/command prompt and navigate to the directory where your index.js file is located.
Run the application with the following command:
```bash
node index.js
```
* Open a web browser and visit http://localhost:3000 to see the "Hello World!" message.

4. Creating the package.json File
The package.json file is an essential part of any Node.js project. It contains metadata about the project and is used for managing dependencies, scripts, and other configurations. Here’s how the package.json file was created for this project:

* Creating the package.json Using npm init
To create the package.json file, I used the npm init command. This command prompted me with a series of questions, including the project’s name, version, entry point file (usually index.js), and author information. Here's a breakdown of the command:

```bash
npm init
```


5. Setting Up GitHub Actions with .github/workflows/main.yml
To automate the CI/CD pipeline, you created a GitHub Actions workflow file located in .github/workflows/main.yml. This file defines the automated steps to run tests and build the application on GitHub whenever changes are pushed to the repository.

Steps to Create the GitHub Actions Workflow:
In your project directory, create the .github/workflows folder:

```bash
mkdir -p .github/workflows
touch .github/workflows/main.yml
```

Add the following content to the main.yml file:
```bash
name: Build and Deploy Node.js Docker Image

on:
  push:
    branches:
      - main  # Runs the workflow when changes are pushed to the main branch

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
         # 'actions/checkout@v2' is a pre-made action that checks out your repository under $GITHUB_WORKSPACE, so your workflow can access it.

      - name: Install dependencies
        run: npm install
         # 'npm install' installs the dependencies defined in your project's 'package.json' file.
         
      - name: Build
        run: npm run build
        # 'npm run build' runs the build script defined in your 'package.json'. This is typically used for compiling or preparing your code for deployment.


      - name: Run tests
        run: npm test
        # 'npm test' runs the test script defined in your 'package.json'. It's crucial for ensuring that your code works as expected before deployment.


      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_TOKEN }}

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_HUB_USERNAME }}/github-actions:v3 .

      - name: Push Docker image
        run: docker push ${{ secrets.DOCKER_HUB_USERNAME }}/github-actions:v3
```

on: Specifies the events that trigger the workflow. It will run whenever changes are pushed to the main branch.

jobs: Defines the steps of the CI/CD pipeline. The build job performs:

Checking out the code from the repository.
Running npm install to install dependencies.
Running tests using npm test.
runs-on: ubuntu-latest: Specifies that the job will run on the latest version of Ubuntu.

6. Generating a Docker Hub Personal Access Token

Instead of using your Docker password for authentication, you can generate a personal access token. This is more secure, especially for automating interactions via CI/CD pipelines.

Steps:
Log in to your Docker Hub account at Docker Hub.
Click on your profile icon in the top-right corner and select Account Settings.
Navigate to the Security tab.
Under Access Tokens, click Create Token.
Name your token (e.g., GitHub Actions Token) to keep track of its purpose.
Click Generate, and copy the token immediately because it won't be shown again.
Outcome:
You now have a Docker Hub token (e.g., my-docker-token), which will allow you to authenticate securely when pushing or pulling Docker images.

7. Storing Docker Credentials in GitHub Secrets
To keep the Docker Hub credentials secure, we store the username and token in GitHub Secrets. These secrets can then be used in GitHub Actions workflows without exposing sensitive information.

Steps:
Go to your GitHub repository and click on Settings.
In the left sidebar, under Secrets and Variables, click Actions.
Click on New repository secret to add the Docker username and token.
Add the following secrets:
DOCKER_HUB_USERNAME: Your Docker Hub username (e.g., myusername).
DOCKER_HUB_TOKEN: Your Docker Hub personal access token (e.g., my-docker-token).
Outcome:
These secrets are now securely stored and can be accessed in GitHub Actions workflows for authentication.

# Conclusion
This project demonstrates how to set up a CI/CD pipeline with Docker, Node.js, and GitHub Actions. It shows how to create a Node.js app, build Docker images, and securely authenticate with Docker Hub using GitHub Actions and personal access tokens.