## Creating a GitHub Personal Access Token

1. Go to your GitHub profile and click on **Settings**.
2. In the left sidebar, click on **Developer settings**.
3. Click on **Personal access tokens**.
4. Click on **Generate new token**.
5. Give the token a descriptive name.
6. Select the scopes you need for the token (e.g., repo, read:user, user:email).
7. Click on **Generate token**.
8. Copy the token to your clipboard.
9. Open notepad on the Windows Host, create a new file and paste your token into the file for easy access.

## Using the Personal Access Token to Push Changes to a Cloned Repository

1. Clone the repository to your local machine using the `git clone` command.
2. Make changes to the files in the repository as needed.
3. Open a terminal and navigate to the local repository.
4. Configure Git to use your GitHub username and email address by running the following commands, replacing the placeholders with your own information:

```
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

5. Add the changes to the local repository using the `git add` command. For example, if you want to add all changes, run `git add .`.
6. Commit the changes using the `git commit` command. For example, if you want to commit with a message "Updated files", run `git commit -m "Updated files"`.
7. Push the changes to the remote repository using the `git push` command and the personal access token you created earlier. For example, if you want to push changes to the `main` branch, run `git push https://<personal-access-token>@github.com/<username>/<repository-name>.git main`.

Note: Replace `<personal-access-token>`, `<username>`, and `<repository-name>` with your own information.

That's it! Your changes should now be pushed to the remote repository using your GitHub Personal Access Token.
