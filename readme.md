🚀 Quick Start Instructions


1. Create the project locally
# Copy all the files above into their respective locations

Open your terminal and run:
npm init -y
npm install


2. Test locally

# Run tests
npm test

# Start the app
npm start
# Visit http://localhost:3000


3. Set up in Jenkins
Option A: Using Docker Agent (Recommended)

In Jenkins, create a new Pipeline job

In Pipeline configuration:

Definition: Pipeline script from SCM

SCM: Git

Repository URL: Your Git repo URL
## To create a Git repo:
(go to the github.com, create a new repository, and push your local project to it)
Then go to your terminal and run:
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <Your Git repo URL>
git push -u origin main

Copy the repo URL and paste it on the Jenkins configuration(Repository URL field)

Script Path: Jenkinsfile

Option B: Quick Test with Direct Pipeline Script

Create new Pipeline job

In Pipeline configuration:

Definition: Pipeline script

Copy the entire Jenkinsfile content

Update the DOCKER_IMAGE variable with your Docker Hub username


4. Configure Jenkins Credentials (if pushing to Docker Hub)
In Jenkins: Manage Jenkins > Credentials

Add Username with password credential:

ID: docker-hub-credentials

Username: Your Docker Hub username

Password: Your Docker Hub password/access token

🔧 Customization Tips
For Simple Testing Only (no Docker):
Remove the Docker stages from the Jenkinsfile:

groovy
// Comment out or remove these stages:
// - 'Build Docker Image'
// - 'Push Docker Image'
// - 'Deploy'