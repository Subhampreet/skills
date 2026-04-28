# Skills

This repository contains internal **Skills** for GitHub Copilot to execute.

## Overview

Skills are specialized, reusable instruction sets that extend GitHub Copilot's capabilities within VS Code. Each skill packages domain knowledge, workflows, and best practices so Copilot can perform complex tasks consistently and reliably.

## Structure

Skills are organized under `.github/skills/`, with each skill in its own folder:

```
.github/
  skills/
    <skill-name>/
      SKILL.md   # Instructions and workflow definition for the skill
```

## Usage

Skills are automatically detected by GitHub Copilot. When a user's request matches a skill's domain, Copilot loads the corresponding `SKILL.md` and follows its instructions to produce high-quality, consistent outputs.

## Contributing

To add a new skill:
1. Create a folder under `.github/skills/<skill-name>/`
2. Add a `SKILL.md` file with the skill's description, trigger conditions, and step-by-step instructions
3. Update this README to document the new skill

## Available Skills

| Skill | Description |
|-------|-------------|
| [rebase-branch-onto-main](.github/skills/rebase-branch-onto-main/SKILL.md) | Rebase the current branch onto `origin/main` to remove previously merged changes from a PR |
