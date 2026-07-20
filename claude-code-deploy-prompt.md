# Claude Code Prompt — Deploy "The Unlived Life" to GitHub Pages

Copy everything below into Claude Code:

---

I have a single self-contained HTML file called `the-unlived-life.html` (an interactive scroll-driven visual essay — no build step, no dependencies, just HTML/CSS/vanilla JS inline). I want to deploy it publicly via GitHub Pages.

Please do the following:

1. Check if a git repo already exists in this folder. If not, initialize one.
2. Rename or copy `the-unlived-life.html` to `index.html` at the repo root (keep the original filename too if useful for reference, but `index.html` must exist at root for GitHub Pages to serve it as the homepage).
3. Create a short `README.md` describing the project: title "The Unlived Life", one line saying it's an interactive visual essay based on a Bhandara message by Daaji, and a note that it's a static site with no build process.
4. Add a `.gitignore` for OS/editor cruft (`.DS_Store`, `.vscode/`, etc.) — nothing else needed since there's no build output.
5. Commit everything with a clear message like "Initial commit: The Unlived Life interactive experience".
6. If a GitHub remote isn't already configured, ask me for:
   - my GitHub username
   - the repo name I want (e.g. `the-unlived-life`)
   - whether the repo should be public (it needs to be public, or on a paid plan, for free GitHub Pages)
7. Push to GitHub, then enable GitHub Pages for this repo (via `gh` CLI if available, otherwise give me the exact manual steps: Settings → Pages → Source: Deploy from branch → branch: main → folder: / (root)).
8. Once Pages is enabled, give me the live URL (it will look like `https://<username>.github.io/<reponame>/`).
9. Confirm the page loads correctly at that URL and that scroll animations, the key-hold interaction, and the external "Read the Full Bhandara Message" link all work as expected.

Do not modify any of the HTML/CSS/JS content itself — this is a deployment task only, not an editing task. If you notice anything that would break when hosted (e.g. absolute local file paths, broken relative links), flag it to me before changing anything.

---
