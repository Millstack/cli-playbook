# Git Authentication

## 1. Global Identity

Before you even log in, you must tell Git who you are so your commits are labeled correctly. 
This doesn't log you in; it just labels your "work."

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

<br>

## 2. Personal Access Token (PAT)

### 2.1. Generate the PAT Token

1. Go to `GitHub Settings` ➔ `Developer Settings` ➔ `Personal Access Tokens` ➔ `Tokens` (classic).
2. Click `Generate new token`.
3. Select scopes (at minimum: `repo`, `workflow`, `write:packages`).
4. Copy the token immediately. The secret value is visible only once.

### 2.2. Logging in on a New Computer

When we try to `git push` or `git clone` a private repo, a popup or terminal prompt will ask for:
* `Username`: Your GitHub username.
* `Password`: Paste the PAT (not your actual password).

<br>

## 3. Caching Credentials (Stay Logged In)

We don't want to paste that long `PAT` every time. So we are gonna use a **Credential Helper** to remember it.

* Windows:
  ```bash
  git config --global credential.helper wincred
  ```
  
* Mac:
  ```bash
  git config --global credential.helper osxkeychain
  ```
  
* Linux:
  ```bash
  git config --global credential.helper cache
  ```

<br>

## 4. SSH Keys (Professional Method)

`SSH` is mostly preferred because it’s "set and forget" and doesn't require typing a token.

### Step 1: Generate the key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
(Press Enter to save in the default location)

### Step 2: Add it to the SSH Agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Step 3: Add to GitHub
1. Copy the key: `cat ~/.ssh/id_ed25519.pub`
2. Go to `GitHub Settings` ➔ `SSH and GPG keys` ➔ `New SSH Key`.
3. Paste the content and save.

### Step 4: Test it
```bash
ssh -T git@github.com
# You should see: Hi [username]! You've successfully authenticated...
```

<br>

## 5. Switching Users on One Machine

If you have a personal GitHub and a work GitHub on the same laptop, you can use local config overrides:
1. Navigate to your work folder.
2. Run: `git config user.email "work-email@company.com"` (without `--global`).
3. This ensures that in that "*specific folder*", Git uses your work identity instead of your global personal one.









