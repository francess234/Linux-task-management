# Linux User Management & Filesystem (FHS) Practical

A GitHub-ready activity pack for completing the **Linux User Management & Filesystem Practical Assessment**.

## Objective

Work as a junior Linux support technician: create a separate local user, sign in as that user, navigate common Linux Filesystem Hierarchy Standard (FHS) locations, manage files, edit a configuration file with `nano`, search with `find` and `grep`, and apply permissions.

**Estimated duration:** 90–120 minutes  
**Environment:** Linux virtual machine or dedicated lab computer  
**Prerequisite:** An administrator account with `sudo` privileges for the initial user-creation step.

## Repository structure

```text
linux-user-management-fhs-github-workflow/
├── .github/
│   └── workflows/
│       └── validate-submission.yml
├── docs/
│   ├── fhs-notes.md
│   └── reflection-answers.md
├── evidence/
│   ├── command-log.md
│   └── README.md
├── screenshots/
│   └── README.md
├── templates/
│   ├── app.conf
│   ├── checklist.txt
│   └── overview.txt
├── .gitignore
└── README.md
```

## Safety rules

- Use a Linux VM or dedicated lab computer. Do not perform this assessment on a production server.
- Use the instructor-approved username format.
- After creating the account, switch to it with a login shell and complete the practical tasks from that account.
- Do not delete, rename, or change permissions on system directories or files.
- Work inside your home directory unless a task explicitly says otherwise.
- Do not use recursive permission changes such as `chmod -R`.
- Never put passwords, private keys, tokens, or other secrets in this repository or screenshots.
- Commands below use `<student_username>` as a placeholder. Replace it with your actual assigned username; do not type the angle brackets.

## Activity workflow

### 1. Create a user and switch identity

From your administrator account:

```bash
sudo adduser student_username
su - student_username
```

If `adduser` is unavailable, use the `useradd` workflow taught by your instructor and document it in `evidence/command-log.md`.

As the new user, run:

```bash
whoami
id
pwd
echo "$HOME"
```

**Check:** `whoami` should display the new username, and `pwd` should show that user's home directory.

### 2. Explore the FHS

Inspect and navigate these locations without modifying their contents:

`/`, `/home`, `/etc`, `/var`, `/tmp`, `/usr`, `/bin`

Example commands:

```bash
pwd
ls /
ls -l /etc
cd /var
pwd
cd ~
```

Write the purpose of each location in `docs/fhs-notes.md`.

### 3. Inspect visible and hidden entries

```bash
cd ~
ls
ls -l
ls -a
mkdir -p ~/.lab-notes
ls -a ~
```

Record one difference between the three `ls` outputs in the evidence log.

### 4. Build the project workspace

```bash
mkdir -p ~/linux-practical/{config,documents,backups,evidence}
nano ~/linux-practical/documents/overview.txt
nano ~/linux-practical/documents/checklist.txt
```

Use `templates/overview.txt` and `templates/checklist.txt` as content examples. Your `overview.txt` must contain at least four lines describing the workspace. Your checklist must contain at least three task items.

Nano: save with **Ctrl+O**, press **Enter**, then exit with **Ctrl+X**.

### 5. Create and edit the configuration file

Create:

```bash
nano ~/linux-practical/config/app.conf
```

Enter the settings below, replacing the owner placeholder with your Linux username:

```ini
APP_NAME=LinuxPractical
OWNER=student_username
ENVIRONMENT=training
LOG_LEVEL=info
BACKUP_ENABLED=true
```

Save, reopen the file, change `LOG_LEVEL=debug`, add `CONTACT=student`, then save and verify:

```bash
nano ~/linux-practical/config/app.conf
cat ~/linux-practical/config/app.conf
```

A starter example is in `templates/app.conf`.

### 6. Copy, rename, and remove files

```bash
cp ~/linux-practical/documents/overview.txt ~/linux-practical/backups/
cp ~/linux-practical/config/app.conf ~/linux-practical/backups/
mv ~/linux-practical/documents/checklist.txt ~/linux-practical/documents/tasks.txt
touch ~/linux-practical/documents/remove-me.txt
rm ~/linux-practical/documents/remove-me.txt
ls -l ~/linux-practical/documents ~/linux-practical/backups
```

Only remove the named temporary file.

### 7. Search for files and content

```bash
find ~/linux-practical -type f -name 'app.conf'
grep -n 'YOUR_SEARCH_TERM' ~/linux-practical/documents/overview.txt
grep -n 'LOG_LEVEL' ~/linux-practical/config/app.conf
```

Replace `YOUR_SEARCH_TERM` with a word or phrase that actually appears in your overview file. Record the commands and matching output.

### 8. Apply and verify permissions

```bash
chmod 644 ~/linux-practical/config/app.conf
chmod 600 ~/linux-practical/documents/tasks.txt
ls -l ~/linux-practical/config/app.conf ~/linux-practical/documents/tasks.txt
id
```

Explain the meaning of `644` and `600`, and describe the user/group information shown by `id`, in `docs/reflection-answers.md`.

### 9. Final verification

```bash
find ~/linux-practical -maxdepth 2 -print
ls -la ~/linux-practical
```

Capture screenshots or terminal captures and save them in this repository's `screenshots/` folder. Keep a written command/output record in `evidence/command-log.md`. Do not capture passwords.

## Evidence and submission

1. Complete all nine tasks in the Linux environment.
2. Update `docs/fhs-notes.md` and `docs/reflection-answers.md`.
3. Record commands and meaningful outputs in `evidence/command-log.md`.
4. Add screenshots to `screenshots/` (PNG/JPG preferred). Use descriptive names such as `01-user-identity.png` and `09-final-structure.png`.
5. Review the submission checklist below.
6. Commit and push the repository to GitHub using Git Bash.

### Submission checklist

- [ ] Created the account and completed the practical as that user.
- [ ] Recorded `whoami`, `id`, `pwd`, and `$HOME`.
- [ ] Documented the required FHS directory purposes.
- [ ] Demonstrated `ls`, `ls -l`, and `ls -a`.
- [ ] Created `config`, `documents`, `backups`, and `evidence` in the Linux workspace.
- [ ] Created and edited `app.conf` as required.
- [ ] Demonstrated copy, rename/move, and removal operations.
- [ ] Demonstrated `find` and `grep`.
- [ ] Applied and verified permissions `644` and `600`.
- [ ] Included command evidence and screenshots, with no passwords or secrets.

## Push this activity to GitHub using Git Bash

Open Git Bash in the extracted repository folder and run:

```bash
git init
git add .
git commit -m "Add Linux user management and FHS practical"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPOSITORY_NAME.git
git push -u origin main
```

Create an empty repository on GitHub first, then replace the remote URL with your repository URL. If `origin` already exists, use `git remote set-url origin YOUR_REPOSITORY_URL` instead of adding it again.

After adding your evidence and notes, push updates with:

```bash
git add .
git commit -m "Complete Linux practical evidence"
git push
```

## GitHub Actions

The workflow in `.github/workflows/validate-submission.yml` runs when you push. It checks that the expected folders and documentation files exist. It does **not** execute the Linux practical or verify that screenshots prove each task; your instructor must assess that evidence.

## Important distinction

This GitHub repository is your **activity record and submission pack**. The actual Linux workspace you create during the lab is `~/linux-practical/` on the Linux VM. Copy only appropriate, non-sensitive evidence into this repository.
