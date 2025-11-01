---
title: "Git & GitHub"
datePublished: Fri Oct 10 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm2bp8zed000c09l5gjdb7vf4
slug: git-and-github
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729072622823/9a31c42b-99c8-409d-aa3e-e06a30ca8d98.webp
tags: linux, aws, github, bash, git, google, devops, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge

---

## Deep Dive into Git & GitHub for DevOps Engineers

Today, we’re going to explore the tools that DevOps engineers rely on every day—**Git** and **GitHub**. These are essential for managing code, keeping track of changes, and collaborating with teams, especially when working with infrastructure and automation. Let’s dive into what they are, how they work, and how to get started with them.

### What is Git, and Why is It Important?

Git is basically a tool that helps you manage your code, keeping track of every change you make, like a save button that logs every version of your project. It allows you to go back to a previous version of your code whenever you need to, which is super useful if something breaks. This is called **version control**.

It’s also great for working with others—Git lets multiple people work on the same project at the same time, and it helps merge everyone’s changes together without messing things up. In DevOps, where you’re working with code and configurations constantly, Git ensures you don’t lose track of what’s happening and who made which changes..

### What’s the Difference Between the "Main" and "Master" Branch?

If you’ve used GitHub, you might have seen some projects using the "main" branch and others using "master." The truth is, they’re the same thing functionally—it’s just a naming difference.

Historically, Git and GitHub used "master" as the default name for the primary branch, but more recently, they switched to using "main" by default to adopt more inclusive terminology. So, don’t worry about the names—the main/master branch is just the default branch where the core of your project lives.

### Git vs. GitHub: What’s the Difference?

This one’s easy to mix up! Here’s a simple way to look at it:

* **Git** is the tool you use on your computer to track code changes, manage branches, and collaborate on code. You can use Git without ever touching the internet.
    
* **GitHub** is a website where you store your Git repositories online. It’s like a cloud backup for your Git projects and a place where teams can collaborate remotely.
    

So, Git is the actual software, while GitHub is the platform that hosts your Git projects.

### How to Create a New Repository on GitHub

Creating a repository (repo) on GitHub is simple:

1. **Log in to GitHub**: Head over to [GitHub](https://github.com) and sign in.
    
2. **Click “New”**: Go to your dashboard, find the “New” button, and click it.
    
3. **Name Your Repo**: Give your repo a name (let’s call it "DevOps"). You can add a description if you want, and choose whether you want it to be public or private.
    
4. **Create the Repo**: Hit **Create repository**, and you’re all set!
    

Now you have a brand-new GitHub repo where you can push your code.

### What’s the Difference Between Local and Remote Repositories?

A **local repository** is the version of your project that lives on your computer, while a **remote repository** is a version that’s stored somewhere online, like on GitHub. Typically, you’ll work on your project locally, then push your changes to the remote repo for safekeeping or for others to collaborate with you.

To connect your local repo to GitHub, you’ll need to run a few commands in your terminal:

1. Create a new repo on GitHub.
    
2. Link your local project to this new repo with:
    
    ```bash
    git remote add origin https://github.com/your-username/repository-name.git
    git push -u origin main
    ```
    

Now, your local changes are linked to the online version, and you can push updates whenever you want.

---

### Tasks: Hands-On with Git & GitHub

Let’s get practical with a few simple tasks that will help you understand how Git and GitHub work together.

#### Task 1: Set Up Your Git User Info

Before you start committing changes, Git needs to know who’s making the changes (so it can track them properly). You can set up your name and email with these commands:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

This info will be attached to all your commits.

#### Task 2: Create a Repository on GitHub and Push Changes

Here’s how you can create a new project on GitHub and push your local files:

1. **Create a Repo on GitHub**: Name it `DevOps` by following the steps we discussed earlier.
    
2. **Set Up a Local Repo**: On your computer, create a new directory for your project and initialize Git:
    
    ```bash
    mkdir DevOps
    cd DevOps
    git init
    ```
    
3. **Create a File**: Add a new file to the project. You can create a folder structure and add some content to a file like this:
    
    ```bash
    mkdir -p Git
    echo "Day 2 Git content" > Git/Day-02.txt
    ```
    
4. **Stage and Commit the Changes**: This tells Git to keep track of the changes you’ve made:
    
    ```bash
    git add .
    git commit -m "Added Day-02.txt with content"
    ```
    
5. **Connect Your Repo to GitHub**: Link your local project to the GitHub repo:
    
    ```bash
    git remote add origin https://github.com/your-username/DevOps.git
    ```
    
6. **Push Your Changes**: Finally, push your local commits to GitHub with:
    
    ```bash
    git push -u origin main
    ```
    

And just like that, your local changes are now uploaded to GitHub!

# Advanced Git & GitHub for DevOps Engineers

Today, I took a deeper dive into some advanced Git and GitHub features that are crucial for DevOps engineers. These tools not only help us manage code but also keep our development process organized and efficient. Let’s walk through some of the key concepts I learned and the tasks I worked on today.

### Git Branching: Keeping Your Work Isolated

Branches in Git are like different pathways where you can make changes without affecting the main project. This is super useful when developing new features, fixing bugs, or experimenting with new ideas. You can always merge these branches back into the main codebase when you're ready.

Each project starts with a default branch (often called `main` or `master`), and you can create as many other branches as you need. Once your feature is complete, you can combine it with the default branch through a **pull request**. This way, you’re not messing up any existing code while you work.

### Git Revert and Reset: Undoing Changes When Things Go Wrong

At some point, we all make mistakes in our code—it happens! But Git makes it easy to backtrack. Two commands that help with this are `git revert` and `git reset`. Both allow you to undo or adjust previous changes, but they work differently.

* `git reset` lets you remove a commit and erase it from history as if it never happened.
    
* `git revert` creates a new commit that undoes the changes made by a previous commit. It’s a bit safer because it preserves the history of everything, even the mistakes.
    

Knowing when to use each is key when managing a large project!

### Git Rebase vs. Merge: What's the Difference?

Git provides two ways to bring changes from one branch into another: **rebase** and **merge**.

* **Rebase**: This command helps you move or combine a sequence of commits to another branch. After rebasing, the commit history is cleaned up, making it look like the changes were made in a linear order.
    
* **Merge**: When you merge one branch into another, Git keeps the full history of both branches intact. Merging is simpler because it preserves every little detail of what happened in each branch.
    

Both are useful depending on whether you want a neat, linear history (rebase) or a complete picture of all the work that’s been done (merge).

For a deeper understanding, you might want to read more about [**Git Rebase vs. Git Merge**](https://devopswithalimasiuzama.hashnode.dev/git-github-02-day-13#).

---

### Tasks: Putting Git Concepts into Action

#### Task 1: Feature Development with Branches

1. **Create a Branch**: First, I created a new branch from `master` to work on a new feature:
    
    ```plaintext
     git checkout -b dev
    ```
    
2. **Add a Feature**: Inside the `Devops/Git/` directory, I added a file called `version01.txt` and included the text:  
    *"This is the first feature of our application."*
    
3. **Commit the Changes**: After making the change, I committed it with:
    
    ```plaintext
     git add Devops/Git/version01.txt
     git commit -m "Added new feature"
    ```
    
4. **Push to GitHub**: Then, I pushed these changes to my GitHub repository:
    
    ```plaintext
     git push origin dev
    ```
    

#### Task 2: Add More Features in Separate Commits

Next, I made multiple updates to `version01.txt`, committing after each change:

* **First Update**: I added a bug fix and committed:
    
    ```plaintext
      echo "This is the bug fix in development branch" >> Devops/Git/version01.txt
      git commit -am "Added feature2 in development branch"
    ```
    
* **Second Update**: I intentionally added some bad code to demonstrate a revert later:
    
    ```plaintext
      echo "This is gadbad code" >> Devops/Git/version01.txt
      git commit -am "Added feature3 in development branch"
    ```
    
* **Third Update**: More "gadbad" code for good measure:
    
    ```plaintext
      echo "This feature will gadbad everything from now" >> Devops/Git/version01.txt
      git commit -am "Added feature4 in development branch"
    ```
    

#### Task 3: Revert to an Earlier Version

After playing around with some bad code, I restored the file to its previous state using `git revert`. This brought the file back to where it said *"This is the bug fix in development branch"*, undoing the last two commits:

```plaintext
git revert HEAD~2
```

---

### Task 4: Working with Branches

1. **Create Multiple Branches**: I created two more branches and took screenshots to visualize the structure of my project. Each branch allows me to work on separate features or fixes in isolation.
    
2. **Merge Changes to Master**: Once I finished making changes to the `dev` branch, I merged them into the `master` branch:
    
    ```plaintext
     git checkout master
     git merge dev
    ```
    
3. **Rebase Practice**: I also practiced using `git rebase` to see how it affects commit history. Rebase rewrites the commit history, unlike merging, which keeps a full record of every commit.
    
    ```plaintext
     git rebase master
    ```
    

### Final- - -

Understanding advanced Git concepts like branching, merging, resetting, and rebasing is critical for managing complex projects in DevOps. These tools help keep our work organized, track changes efficiently, and collaborate smoothly with our teams.