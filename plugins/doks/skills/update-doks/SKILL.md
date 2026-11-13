---
name: update-doks
description: "Update project technical documentation and specifications based on the current session's context, decisions, and code changes. Reads the documentation root folder from `base-dir` in `./doks.yml` (or prompts the user to create `./doks.yml` if missing). Use when asked to write, update, or sync documentation from the current session."
---

# Update Doks (`update-doks`)

Update and maintain high-quality technical documentation and specifications in the project's configured documentation directory using the context, architectural decisions, and changes from the current session.

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
   - Identify what new behavior, interfaces, invariants, architectural decisions, or workflows were introduced, modified, or clarified in this session.

3. **Inspect Existing Documentation in `base-dir`**
   - List and read relevant files inside `<base-dir>/` to understand the existing organization, terminology, document structure, and cross-references.
   - Prefer updating existing specification or documentation files that cover the affected domain before creating new files.

4. **Write or Update Documentation**
   - Update affected documents or create new files under `<base-dir>/` so the documentation accurately reflects the current session's outcomes and can serve as the source of truth for spec-anchored development:
     - **Specifications & Contracts**: Clearly state module responsibilities, interfaces/types, configuration, behavioral requirements, invariants, and verification/acceptance criteria.
     - **Code Grounding**: Reference the actual file paths, symbols, and commands in the repository rather than vague or speculative descriptions.
     - **Precision**: Make surgical updates that preserve valid existing documentation while removing or updating any statements superseded by the current session's work.
   - If `<base-dir>/` has an index or table of contents (such as `README.md` or `index.md`), update it when adding new documentation files.
