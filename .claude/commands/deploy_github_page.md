Deploy this project to GitHub Pages and return the live URL.

## Steps

Follow these steps in order. Stop and report clearly if any step fails.

### 1. Check prerequisites

Run these checks:
```bash
gh --version
gh auth status
git --version
node --version
```

If `gh` is not installed, tell the user to install it from https://cli.github.com and stop.
If `gh auth status` fails, tell the user to run `gh auth login` and stop.

### 2. Determine repository name

Ask the user: "What GitHub repository name would you like to use? (suggested: `claude-code-treasure-game`)"

Wait for their answer before continuing.

### 3. Initialize git if needed

Check if this is already a git repository:
```bash
git rev-parse --is-inside-work-tree
```

If it is NOT a git repo, initialize one:
```bash
git init
git add .
git commit -m "Initial commit"
```

If it IS a git repo, check for uncommitted changes with `git status` and commit them:
```bash
git add .
git commit -m "Pre-deploy commit" --allow-empty
```

### 4. Create or connect GitHub repository

Check if a remote already exists:
```bash
git remote get-url origin
```

If NO remote exists, create a new GitHub repo and add it as origin:
```bash
gh repo create <REPO_NAME> --public --source=. --remote=origin
```

If a remote DOES exist, note the existing repo URL and continue.

Get the GitHub username:
```bash
gh api user --jq '.login'
```

Store the username — you'll need it for the final URL.

### 5. Configure Vite base path for GitHub Pages

Read the current `vite.config.ts`. The `base` option must be set to `/<REPO_NAME>/` so assets load correctly on GitHub Pages.

Edit `vite.config.ts` to add or update the `base` property inside `defineConfig`:
```ts
base: '/<REPO_NAME>/',
```

Place it at the top level of the config object (alongside `plugins`, `build`, `server`).

### 6. Build the project

```bash
npm run build
```

The output goes to `./build` (as configured in vite.config.ts). Verify the `build/` directory was created.

### 7. Deploy to gh-pages branch

Install `gh-pages` as a dev dependency if not already present:
```bash
npm list gh-pages || npm install --save-dev gh-pages
```

Deploy the `build` directory to the `gh-pages` branch:
```bash
npx gh-pages -d build -m "Deploy to GitHub Pages"
```

### 8. Enable GitHub Pages on the repository

Use the GitHub API to configure Pages to serve from the `gh-pages` branch:
```bash
gh api repos/:owner/<REPO_NAME>/pages \
  --method POST \
  -f source[branch]=gh-pages \
  -f source[path]=/ \
  2>/dev/null || echo "Pages already configured or updating..."
```

Wait a few seconds for GitHub to process (use: `Start-Sleep -Seconds 5` in PowerShell).

### 9. Get the live URL and report to user

The GitHub Pages URL follows this pattern:
```
https://<GITHUB_USERNAME>.github.io/<REPO_NAME>/
```

Verify the Pages URL via the API:
```bash
gh api repos/:owner/<REPO_NAME>/pages --jq '.html_url' 2>/dev/null
```

Report to the user:
- The live URL
- A note that GitHub Pages may take 1–2 minutes to go live after first deployment
- The repository URL: `https://github.com/<GITHUB_USERNAME>/<REPO_NAME>`
