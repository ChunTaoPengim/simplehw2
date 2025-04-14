# simplehw2
This is my hw2 file, hello!!

First we create a public repository on github, and made modifications to README.md
```
git clone <url>
cd <repository>
nano README.md
```
after some modifications
```
git add README.md
git commit -m "<anything you want to say>"
git push origin main
```
now we successfully modify the README file, the next step is creating 2 branches
```
git branch hw1-p
git branch hw1-f
git push origin hw1-p
git push origin hw1-f
```
On the website, we can see there are 3 branches, next we create issue template (on main branch)
```
mkdir -p .github/ISSUE_TEMPLATE
nano .github/ISSUE_TEMPLATE/my_custom_template.md
```
After define the markdown files for issue template, push the changes
```
git add .github/
git commit -m "Add custom issue template"
git push origin main
```
Now we can start a custom issue on Github, next create pull requests, to do so, I add text files for the 2 branches
```
git checkout hw1-p
touch file-for-p.txt
git commit -m "feat: Add changes for hw1-p pull request"
git push origin hw1-p
git checkout hw1-f
touch file-for-f.txt
git commit -m "feat: Add changes for hw1-f pull request"
git push origin hw1-f
```
Now we can use the compare & merge buttons on Github, start 2 pull requests, and add comments to it.

Next step is creating the action workflow
```
git checkout main
mkdir -p .github/workflows
nano .github/workflows/ci.yml
```
In the yml file, we difinw the action as
```
name: Homework CI Check

# Run this workflow on pull requests targeting the main branch
on:
  pull_request:
    branches: [ main ]

jobs:
  script_check:
    runs-on: ubuntu-latest # Use a standard runner

    steps:
      # Step 1: Checkout the code from the PR branch
      - name: Checkout code
        uses: actions/checkout@v4 # This is a standard step

      # Requirement 2: Add at least two extra steps
      - name: Extra Step 1 - Show Current Directory # Extra Step 1
        run: pwd

      - name: Extra Step 2 - List Files # Extra Step 2
        run: ls -la

      # Requirement 3 & 4: Run the script which differs between branches
      # This step's success/failure depends on check_script.sh from the specific PR branch
      - name: Run the check script
        run: ./check_script.sh # Execute the script in the root directory
```
push the changes
```
git add .github/workflows/ci.yml
git commit -m "ci: Add workflow to run check script on PRs"
git push origin main
```
Now we have fulfilled the requirements for defining actions, we create a success action in hw1-p branch
```
git checkout hw1-p

echo '#!/bin/bash' > check_script.sh
echo "echo 'Running the success script on hw1-p. Everything is okay'" >> check_script.sh
echo "exit 0" >> check_script.sh 

chmod +x check_script.sh

git add check_script.sh
git commit -m "feat: Add check_script.sh for hw1-p (success)"
git push origin hw1-p
```
and add a fail script to hw1-f branch
```
git checkout hw1-f

echo '#!/bin/bash' > check_script.sh
echo "echo 'Running the failing script on hw1-f. Simulating an error'" >> check_script.sh
echo "exit 1" >> check_script.sh 

chmod +x check_script.sh

git add check_script.sh
git commit -m "feat: Add check_script.sh for hw1-f (failure)"
git push origin hw1-f
```
Now we can successfully see the actions.