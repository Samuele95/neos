# Contributing to NEOS

Thank you for your interest in NEOS — the Neural Field Operating System. Contributions of all kinds are welcome.

## Ways to Contribute

### Report a Bug
- Use the [Bug Report](https://github.com/Samuele95/neos/issues/new?template=bug_report.md) issue template
- Include your LLM platform (Claude, Gemini CLI, OpenCode, etc.) and the command/session that triggered the issue

### Request a Feature
- Use the [Feature Request](https://github.com/Samuele95/neos/issues/new?template=feature_request.md) issue template
- Describe the use case, not just the desired feature

### Improve the Specification
NEOS is a **specification-first** project. The core value is in the markdown specs under `core/`, `commands/`, `autonomy/`, `visualization/`, `interfaces/`, and `persistence/`.

Contributions that clarify, extend, or fix specs are especially valuable:
- Fix ambiguities in command definitions
- Add new command specifications
- Improve the field dynamics equations
- Extend visualization generator specs

### Add Examples and Sessions
- New example workflows in `examples/`
- Session evidence (collapsed fields, convergence data) in `sessions/`

### Improve Documentation
- Expand the detailed guide (`docs/DETAILED-GUIDE.md`)
- Fix typos or broken links in `README.md`
- Improve the website (`NEOS-BREAKTHROUGH.html`) or presentation (`NEOS-PRESENTATION.html`)

## How to Submit Changes

1. **Fork** the repository
2. **Create a branch** from `main`:
   ```bash
   git checkout -b your-feature-name
   ```
3. **Make your changes** — keep commits focused and descriptive
4. **Test** your changes:
   - For spec changes: load the bootloader (`prompts/nfos-kernel.md`) in your LLM and verify behavior
   - For HTML changes: open locally in a browser and test across viewports
5. **Push** and open a **Pull Request** against `main`

## Guidelines

- **Spec files** use markdown with NEOS conventions (field equations in LaTeX-style, command signatures in code blocks)
- **HTML files** follow the existing NEOS design system: dark theme (`#07070c`), cyan/purple/pink accent palette, Outfit + JetBrains Mono fonts
- **SVG assets** maintain the 1200px-wide viewBox convention with the dot-grid texture
- Keep PRs focused — one logical change per PR

## Code of Conduct

Be respectful, constructive, and curious. NEOS is a research project exploring the frontier of machine intelligence — we welcome diverse perspectives and approaches.

## Questions?

Open a [Discussion](https://github.com/Samuele95/neos/discussions) or reach out via an issue. We're happy to help you get started.
