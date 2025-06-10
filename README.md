# Build Changed Services GitHub Action Workflow

This workflow automatically builds and packages services in a monorepo based on changes in the last commit, including common dependencies, and creates a GitHub Release with the zipped artifacts.

---

## Features

- Detects files changed in the **last commit**.
- If global files like `package.json`, `dependencies/`, or `libraries/` change, builds **all services**.
- Otherwise, builds and zips only the changed services.
- Each zip includes the service folder, `dependencies/`, `libraries/`, and `package.json`.
- Creates an additional full zip with **all services plus common dependencies**.
- Uploads all generated zip files as assets to a GitHub Release.

---

## Variables

| Variable      | Description                              | Default       |
|---------------|------------------------------------------|---------------|
| `PROJECT_PATH`| Root directory of the project            | `project_18`  |
| `APPS_PATH`   | Directory inside project containing services | `apps`       |
| `OUTPUT_DIR`  | Directory where zip files are saved      | `release_zips`|

---

## Workflow Details

1. **Checkout** full git history to get accurate diffs.
2. **Identify changed files** in the last commit.
3. **Determine services** that need building:
   - Build all if global files changed.
   - Otherwise, build changed services only.
4. **Build and package** each service along with common dependencies and `package.json`.
5. **Build and package** all services together.
6. **Create a GitHub Release** and upload zip files.

---

## Directory Structure Assumption

project_18/
├── apps/
│ ├── service1/
│ ├── service2/
│ └── service3/
├── dependencies/
├── libraries/
└── package.json


---

## Extending

- Add specific build commands (e.g., `npm install`, `npm run build`) before zipping services.
- Add tests, linting, or deployment steps after build.
- Trigger on pull requests or different branches.
- Customize release tagging or descriptions.

---

## Prerequisites

- GitHub repository with the described structure.
- `GITHUB_TOKEN` secret available (default in GitHub Actions).
- Permissions to create releases in the repo.

---

## Notes

- Uses `zip` command to create archives.
- Uses `mkdir` instead of `mktemp` for temp directories.
- Temporary build folders cleaned and recreated for each service.

---

