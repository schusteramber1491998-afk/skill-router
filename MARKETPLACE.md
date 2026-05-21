# Marketplace Notes

Skill Router is a Codex skill. GitHub Marketplace does not list Codex skills directly, so this repository includes a small GitHub Action wrapper.

The Action copies `SKILL.md` and `agents/openai.yaml` into a Codex skills directory on the current GitHub Actions runner. For local Codex Desktop or CLI use, install the repository directly into your local `~/.codex/skills/skill-router` directory.

To publish this wrapper to GitHub Marketplace:

1. Create a release tag, for example `v0.1.0`.
2. Open the GitHub release page.
3. Select **Publish this Action to the GitHub Marketplace**.
4. Choose a primary category.
5. Publish the release.
