---
name: update-doks
description: "Update project technical documentation and specifications based on the current session's context, decisions, and code changes. Reads the documentation root folder from `base-dir` in `./doks.yml` (or prompts the user to create `./doks.yml` if missing) and maintains glossary (`*.glossary.md`), user story (`*.story.md`), and architectural (`*.arch.md`) files. Use when asked to write, update, or sync documentation from the current session."
---

# Update Doks (`update-doks`)

Update and maintain high-quality technical documentation and specifications in the project's configured documentation directory using the context, architectural decisions, and changes from the current session.

## Writing Style

All documentation must follow the [writing style guide](../../style.md).

## Document Types

Documentation under `<base-dir>/` (organized either flat or grouped into domain subfolders) is composed of three types of files identified by their filename suffix:

1. **Glossary Files (`<term>.glossary.md`)**
   - Defines a single domain or system term.
   - Follow the guidelines in [glossary.md](../../glossary.md) when writing or updating glossary files.

2. **User Story Files (`<operation>.story.md`)**
   - Describes an operation a user (or external actor) can perform on the system.
   - Specifies the actor, preconditions, trigger/inputs, step-by-step behavior, expected outcomes/postconditions, error/edge cases, and testable acceptance criteria.

3. **Architectural Documents (`<aspect>.arch.md`)**
   - Provides a high-level description of an aspect of the system.
   - Follow the guidelines in [architecture.md](../../architecture.md) when writing or updating architectural documents.

## Cross-Linking Rules

- **Once Per Section**: Link to a given glossary term, user story, architectural document, or code anchor at most once per section (on its first mention in that section). Do not repeat the same link on subsequent mentions within the same section, keeping cross-references accessible without cluttering the prose with link formatting.
- **Glossary Links**: When a user story or architectural document references a defined domain term, link its first mention in each section to its corresponding `*.glossary.md` file using a relative Markdown link.
- **Story <-> Architecture Links**: Cross-reference user stories (`*.story.md`) with the architectural documents (`*.arch.md`) that implement them, and list supported user stories or key domain terms in architectural documents where relevant.
- **Code Anchors**: Ground glossary definitions, user stories, and architectural overviews with links or paths to the actual source files, symbols, and tests in the repository.

## Workflow

1. **Resolve Configuration (`./doks.yml`)**
   - Check the repository root for `./doks.yml` and read the `base-dir` property.
   - If `./doks.yml` does not exist or `base-dir` is not configured:
     1. Ask the user which root directory they want to use for documentation (e.g., `docs`).
     2. Create `./doks.yml` at the repository root with:
        ```yaml
        base-dir: <chosen-directory>
        ```
   - Ensure the configured `base-dir` directory exists before writing documentation files.

2. **Gather Current Session Context**
   - Review the conversation history, design decisions, requirements discussed, and code or configuration changes made during the current session (including uncommitted diffs if applicable).
   - Identify:
     - New or updated domain terms -> `*.glossary.md`
     - New or modified user-facing operations/workflows -> `*.story.md`
     - New or changed system components, data flows, or boundaries -> `*.arch.md`

3. **Inspect Existing Documentation via a Sub-Agent**
   - Do **not** scan or read all documentation files directly in the main agent, as doing so pollutes the main agent's context window.
   - Instead, launch a read-only exploration sub-agent (e.g., using the `task` tool with `agent_type: "explore"`) and pass it:
     - The configured `<base-dir>` path.
     - A concise summary of the changes, decisions, domain terms, user operations, and architectural components affected in the current session.
   - Instruct the sub-agent to search and inspect `<base-dir>/` (`*.glossary.md`, `*.story.md`, `*.arch.md`, and any index files) and return **only**:
     - The list of relevant existing documentation file paths (and why each is relevant, including files that should be updated or cross-linked).
     - The observed directory/naming layout in `<base-dir>/` so any newly created files follow the same structure.

4. **Write or Update Documentation**
   - In the main agent, read only the relevant files returned by the sub-agent.
   - Prefer updating existing files that cover the affected term, operation, or subsystem before creating new files.
   - Create or update the relevant `*.glossary.md`, `*.story.md`, and `*.arch.md` files under `<base-dir>/` so they accurately reflect the current session's outcomes and serve as a reliable basis for spec-anchored development.
   - Maintain relative cross-links between glossary terms, user stories, and architectural documents, and remove or update statements superseded by the current session's work.
   - If `<base-dir>/` has an index or table of contents (such as `README.md` or `index.md`), update it when adding new files.

