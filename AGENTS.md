# Working in this repository

This repository distributes public customer-facing skills for Durga Digital apps.

- Keep skills under `skills/<app>/<app>-<workflow>/SKILL.md`.
- Keep skill names globally unique and app-prefixed. App folder READMEs are catalogs, not skills.
- Each skill must be independently installable. Bundle its required references/scripts; use public URLs for optional links to other skills.
- Use the portable Agent Skills format. Verify app tools and permissions before changing workflow instructions.
- Do not invent capabilities for scaffolded apps. Add usable skills only when their app contract is known.
- Never commit credentials, user data, local environment files, or private application code.
- Update the root/app catalog and skill metadata version when changing published instructions.
- Validate discovery with `npx skills add . --list`. Do not install globally or exercise live app mutations just to validate the catalog.
- Keep app source, AGENTS.md templates and software-factory infrastructure in their owning repositories.
