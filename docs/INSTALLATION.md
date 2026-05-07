# Installation

## Option 1: Install from this repo (Recommended)

1. **Clone the repo:**
   ```bash
   git clone https://github.com/jennyrhamrick/productmgmt.git
   cd productmgmt
   ```

2. **In Claude Code:**
   - Open Settings (gear icon)
   - Go to Skills
   - Click "Add Custom Skill"
   - Point to: `<path-to-repo>/skills/okr-cascade`
   - The skill is now available as `/okr-cascade`

## Option 2: Install a single skill

If you only want `okr-cascade`:

1. Download the `skills/okr-cascade/` folder
2. In Claude Code: Settings → Skills → Add Custom Skill
3. Point to that folder
4. Done

## Verify Installation

In Claude Code, type:

```
/okr-cascade
```

You should see the skill trigger in the command palette. If not, check that you pointed to the correct folder path.

## Updating the Skill

Skills update automatically when you pull new changes from GitHub:

```bash
cd productmgmt
git pull
```

Reload Claude Code and you'll have the latest version.
