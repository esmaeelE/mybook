# Create and Publish a Book Quickly with mdBook and GitHub Pages 🚀

This guide walks you through creating a beautiful online book using **[mdBook](https://rust-lang.github.io/mdBook/)** (a Rust-based Markdown book generator) and automatically publishing it to **GitHub Pages** for free hosting.

Every push to the `main` branch will trigger a GitHub Actions workflow to build and deploy your book — no manual uploads needed!

## Prerequisites (One-Time Setup)

- Install **Rust and Cargo** (if not already installed): Visit [rustup.rs](https://rustup.rs/).
- Install **mdBook**:

  ```bash
  cargo install mdbook
  ```

- Verify installations:

  ```bash
  git --version
  cargo --version
  mdbook --version
  ```

## Step 1: Create a New GitHub Repository

1. Go to [https://github.com/new](https://github.com/new).
2. Set:
   - **Repository name**: e.g., `my-mdbook` (or any name you like).
   - **Visibility**: Public (required for free GitHub Pages).
3. **Do NOT** initialize with a README.
4. Click **Create repository**.

Keep the page open — you'll need the repo URL soon.

## Step 2: Set Up mdBook Locally

```bash
mkdir my-mdbook
cd my-mdbook
mdbook init --title "My Awesome Book" --ignore .gitignore
```

During setup:

- Accept defaults or customize as needed (e.g., create `src` directory: **yes**).

Test it locally:

```bash
mdbook serve --open
```

This opens your book at [http://localhost:3000](http://localhost:3000).  
Stop with `Ctrl+C` when done.

## Step 3: Push the Initial Project to GitHub

```bash
git init
git add .
git commit -m "Initial mdBook setup"
git branch -M main
git remote add origin https://github.com/<your-username>/my-mdbook.git
git push -u origin main
```

Your repo now contains the mdBook source.

## Step 4: Set Up Automatic Deployment with GitHub Actions

Create the workflow file for building and deploying to GitHub Pages.

```bash
mkdir -p .github/workflows
touch .github/workflows/deploy.yml
```

Paste this into `.github/workflows/deploy.yml` (recommended official approach as of 2026):

```yaml
name: Deploy mdBook to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup mdBook
        run: |
          curl -sSL https://github.com/rust-lang/mdBook/releases/latest/download/mdbook-x86_64-unknown-linux-gnu.tar.gz | tar -xz --directory=$HOME/.cargo/bin

      - name: Build book
        run: mdbook build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: book/

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push:

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow for mdBook deployment"
git push
```

## Step 5: Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages** (in the left sidebar).
2. Under **Build and deployment** → **Source**, select **GitHub Actions**.
3. Save.

Your first deployment will start automatically (takes 1–2 minutes).

## Step 6: View Your Live Book

Once the workflow succeeds, your book is live at:

```bash
https://<your-username>.github.io/my-mdbook/
```

## Editing Your Book

All content lives in the `src/` folder:

- `SUMMARY.md`: Table of contents and navigation.
- Other `.md` files: Chapters and pages.

To update:

1. Edit files in `src/`.
2. Commit and push:

   ```bash
   git add .
   git commit -m "Update chapter X"
   git push
   ```

3. GitHub Actions rebuilds and deploys automatically.

Enjoy writing — your book updates instantly with every push!

For more mdBook features (themes, preprocessors, etc.), check the [official documentation](https://rust-lang.github.io/mdBook/).
