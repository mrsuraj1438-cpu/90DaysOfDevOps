# Day 22 – Git Basics (All Steps in One File)

1. Understand What Git Is  
   Git is a distributed version control system. It tracks changes in files and manages the history of your code.  
   In DevOps, Git is essential because all CI/CD pipelines, automation, and team collaboration rely on version control.

2. Install and Configure Git  
   - Check your Git version:  
     ```  
     git --version  
     ```  
   - Set your Git user name and email:  
     ```  
     git config --global user.name "Suraj"  
     git config --global user.email "suraj@example.com"  
     ```  
   - Verify your configuration:  
     ```  
     git config --list  
     ```  

3. Create a Git Project  
   - Create a new directory:  
     ```  
     mkdir devops-git-practice  
     cd devops-git-practice  
     ```  
   - Initialize a Git repository:  
     ```  
     git init  
     ```  
   - Check status:  
     ```  
     git status  
     ```  
   - Look at the hidden .git directory:  
     ```  
     ls -a  
     ```  

4. Git Commands Reference  
   - Setup & Config  
     - git config: Sets your Git user name and email.  
       Example:  
       ```  
       git config --global user.name "Suraj"  
       git config --global user.email "suraj@example.com"  
       ```  
     - git init: Initializes a new Git repository.  
       Example:  
       ```  
       git init  
       ```  
   - Basic Workflow  
     - git status: Shows the current state of the repository.  
       Example:  
       ```  
       git status  
       ```  
     - git add: Adds files to the staging area.  
       Example:  
       ```  
       git add file.txt  
       ```  
     - git commit: Commits the staged changes.  
       Example:  
       ```  
       git commit -m "Initial commit"  
       ```  

   - Viewing Changes  
     - git log: Shows the commit history.  
     - git branch: Lists all branches.  
     - git checkout: Switches to a branch.  
       Example:  
       ```  
       git checkout main  
       ```  
     - git checkout -b: Creates a new branch and switches to it.  
       Example:  
       ```  
       git checkout -b dev  
       ```  

