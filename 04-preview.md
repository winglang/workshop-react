
---
In this section, we set up automatic preview environments on every pull request using wing cloud.

## Instructions

### Set Up Your Repo in GitHub

1. Clean up the `create-react-app` `.git` and `.gitignore` from the main project:
```
rm -rvf client/.git client/.gitignore
```
2. Add the `.gitignore` to the root project path, with the following content:
```
.DS_Store
backend/target
node_modules
client/public/wing.js
 ```
3. Initialize a git repo, add the files & folders, and commit:
```
git init
git add ./
git commit -m "Initial commit" -a 
```
4. Log in to GitHub, create a new empty repository called `wing-react-workshop`.
5. Push your local repo to GitHub (note that you should use your username, instead of `<your-username>`):
```
git remote add origin git@github.com:<your-username>/wing-react-workshop.git
git branch -M main
git push -u origin main
```

### Connect The Repo With Wing.Cloud

1. Add the `backend/wing.sh` with the following to your repo:
```
#!/bin/sh
DIR="$( cd "$( dirname "$0" )" && pwd )"

cd "$DIR"/../client
npm install
```
2. Commit and push to the main branch:
```
git add backend/wing.sh
git commit -m "Adding wing install helper" backend/wing.sh
git push -u origin main
```
4. Go to [production.wingcloud.io](https://production.wingcloud.io/).
5. Log in with your GitHub credentials.
6. Click on the `+ Add` button --> Connect an existing repository.
7. Give permissions to your GitHub repository.
8. Wait for the main branch to finish building successfully.
9. Open a pull request with some change to the API returned value (e.g., `body: "Hello from the API PR"` instead of `body: "Hello from the API"`).
10. Wait for the PR comment to be updated to the status deployed, and then visit the ReactApp endpoint and the Console.

🚀 You have set up an automatic preview environment on every PR 🤯 🚀
---
