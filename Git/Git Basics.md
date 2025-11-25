## Create GitHub Repos from the Command Line #GitBasics
### Importing a Git Repository with the command line
- Open Git Bash
- Navigate to the root repository directory of your project
- Initialize the local directory as a Git repository. By default, the initial branch is called `main`
		`git init -b main`
- Add files in your new local repository. This stages them for the first commit
		`git add .`
- Commit the files that you've staged in your local repository.
		`git commit -m "First commit"`

### Adding a local repository to GitHub using Git
- Create a new repository on GitHub
	- To avoid errors, do not initialize the new repository with README, license, or gitignore files
- Open Git Bash
- Change the current working directory to your local project
- Add a new "remote" (an external repository) to your local git project
	- `git remote add origin <REMOTE-URL>` (the `<REMOTE-URL>` could be SSH/HTTP)
- Push local `main` branch to the `main` branch on your remote `origin` and sets up tracking for easier future pushes and pulls
```
git push -u origin main
git --set-upstream origin main
```

## What is `.gitattributes`?
- a special file that tells Git how to handle specific files in your repository
- controls things like line endings, file types, merge behavior, custom diff tools, and more

### When to use
- enforce consistent line endings across different operating systems
- make files as binary -> Git doesn't try to merge or change them -> you're telling Git:
	1. Don't try to blend changes inside this file
	2. Don't show line-by-line-diffs
	3. Don't touch line endings or other texty stuff

## What does `git remote set-url origin git@github.com:minhunpam/to-do-list.git` do?
- **Meaning**: replace the url of the remote named origin with the SSH URL
- after this git command, all other commands that target `origin` will talk to that repository -- just updates the pointer (no repo created, history isn't moved)

## `git config --global user.{name|email}`

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
- This saves the settings in `~/.gitconfig` and applies to all your repositories

### Checks:
```
git config --global user.name
git config --global user.email
```

- If you want to see the full config file
```bash
git config --global --list
```
