# claude-skills

A collection of custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills.

## Skills

| Skill | Description |
|-------|-------------|
| [pymol-figures](pymol-figures/) | Generate publication-quality PyMOL protein structure figures with ray tracing and transparent backgrounds |

## Installation

Clone this repo, then symlink individual skills into your Claude Code skills directory.

### Personal skills (available in all projects)

```bash
git clone git@github.com:bendorr/claude-skills.git ~/claude-skills

# Symlink the skills you want
ln -s ~/claude-skills/pymol-figures ~/.claude/skills/pymol-figures
```

### Project skills (available in one project)

```bash
git clone git@github.com:bendorr/claude-skills.git ~/claude-skills

# From your project root
mkdir -p .claude/skills
ln -s ~/claude-skills/pymol-figures .claude/skills/pymol-figures
```

### Adding new skills later

As new skills are added to this repo, pull the latest and create a new symlink:

```bash
cd ~/claude-skills && git pull
ln -s ~/claude-skills/new-skill ~/.claude/skills/new-skill
```

## Usage

Once installed, invoke a skill in Claude Code with:

```
/pymol-figures structure1.pdb structure2.pdb
```

## Requirements

Skill-specific requirements are listed in each skill's directory.
