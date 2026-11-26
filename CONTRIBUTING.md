# Contributing to cansat-hab-digital-twin

## Ground rules

- **Never commit directly to `main`.** All changes go through a Pull Request.
  > Note: branch protection requiring a PR before merging is enforced by team
  > convention. (GitHub branch-protection rules on private repos require
  > GitHub Pro/Team; if the repo is on a free plan the rule is not enforced
  > by GitHub itself — but the team agrees to follow it regardless.)

---

## Branch naming

```
<your-first-name>/<short-topic>
```

Examples:
- `deniz/sitl-state-machine`
- `ege/container-cad-v1`
- `huseyin/parachute-drag-model`
- `mehmet/openmct-telemetry-plugin`

---

## Workflow

```
git checkout main
git pull origin main
git checkout -b <name>/<topic>

# ... make changes ...

git add <files>
git commit -m "Add descent model v1"
git push -u origin <name>/<topic>
```

Then open a Pull Request on GitHub:

1. Fill in the PR template (summary, what changed, how tested).
2. At least **1 approving review** is required before merging — preferably from
   the folder's code owner listed in `.github/CODEOWNERS`.
3. Use **Squash merge** (the only merge strategy enabled on this repo).
4. The branch is deleted automatically after merge.

---

## Commit message style

Short imperative sentence in English, 50 characters or fewer for the subject:

```
Add descent model v1
Fix thermal boundary condition for cold soak
Update Open MCT telemetry schema
```

---

## Shared mission values

All numerical mission parameters live in **`params/mission_params.yaml`**.
Do not hard-code altitudes, masses, diameters, descent rates, or temperatures
anywhere else. Import or read from that file in your code.

---

## Git LFS setup

Install Git LFS **before cloning**:

```bash
# Windows (winget or Git for Windows)
winget install -e --id GitHub.GitLFS
# or: included with Git for Windows — just run:
git lfs install

# macOS
brew install git-lfs
git lfs install

# Ubuntu / Debian
sudo apt install git-lfs
git lfs install
```

Clone with LFS:

```bash
git lfs install          # once per machine
git clone https://github.com/dnzgrkn/cansat-hab-digital-twin.git
cd cansat-hab-digital-twin
git lfs pull             # download any existing LFS objects
```

---

## UnityYAMLMerge setup (required for xr/ contributors)

Unity `.unity` and `.prefab` files are YAML text (not in LFS) but need a
special merge driver to avoid destructive conflicts. Configure it once per
machine after cloning:

**1. Find UnityYAMLMerge** — it ships with Unity Editor:

| Platform | Typical path |
|----------|-------------|
| Windows  | `C:\Program Files\Unity\Hub\Editor\<version>\Editor\Data\Tools\UnityYAMLMerge.exe` |
| macOS    | `/Applications/Unity/Hub/Editor/<version>/Unity.app/Contents/Tools/UnityYAMLMerge` |
| Linux    | `~/Unity/Hub/Editor/<version>/Editor/Data/Tools/UnityYAMLMerge` |

**2. Register the merge driver** (run once in the repo root):

```bash
git config merge.unityyamlmerge.name "Unity Smart Merge"
git config merge.unityyamlmerge.driver \
  "'<path-to-UnityYAMLMerge>' merge -p %O %B %A %A"
```

Replace `<path-to-UnityYAMLMerge>` with the actual path for your machine.

**3. Verify** — open a `.unity` or `.prefab` conflict; Git will invoke
UnityYAMLMerge automatically.
