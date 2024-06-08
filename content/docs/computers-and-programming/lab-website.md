---
title: "Editing the Lab Website"
linkTitle: "Lab Website"
summary: " "
---

## Cloning the Website Repository
First, fork the [website repository](://github.com/weecology/website) to your own GitHub account. Then, clone your fork to your local machine and create a new branch. It's recommended to work on a branch rather than the default `main` branch to keep your changes organized.

## One-Time Setup
To get started, follow these steps for the initial setup:

1. **Install the `blogdown` R package:**
   ```r
   install.packages("blogdown")
   ```

2. **Install Hugo:**
   ```r
   blogdown::install_hugo()
   ```

3. **Create a New R Project:**
   Open RStudio, navigate to your local copy of the website repository, and create a new R project in that directory.

## Adding New Members
To add a new member to the website, follow these steps:

1. **Create a Directory:**
   Create a directory named `content/authors/first-lastname/`.

2. **Add an Avatar Image:**
   Place an avatar image file named `avatar.jpg` in the directory: `content/authors/first-lastname/avatar.jpg`.

3. **Create and Customize an `_index.md` File:**
   Copy an existing `_index.md` file from `content/authors/` into the new directory and customize it with the new member's details:
   ```bash
   cp content/authors/existing-author/_index.md content/authors/first-lastname/_index.md
   ```

## Viewing Your Changes Locally
To see your changes in real-time on your local computer, follow these steps:

1. **Open the R Project:**
   Open your R project in RStudio.

2. **Serve the Site:**
   Run the following command in the R console:
   ```r
   blogdown::serve_site()
   ```

3. **View in Browser:**
   The site will appear in a pane within RStudio. Click the `Show in new window` button to open it in your web browser. The site should update automatically as you save files, though it may take a few seconds to reflect changes.

## Submitting Your Changes
When you're ready to submit your changes, follow these steps:

1. **Create a Pull Request (PR):**
   Push your branch to your GitHub fork and create a pull request to the original repository. This will allow others to review your changes.

2. **Netlify Deployment Preview:**
   After creating the PR, Netlify will automatically provide a deploy preview for the website. Check this preview to ensure everything looks correct.

3. **Merge the PR:**
   If everything works fine, the PR will be merged.

## After Merging
Once the PR is merged, the website will rebuild automatically. This process may take a few minutes, so please be patient.
 any issues, don't hesitate to ask for help.
