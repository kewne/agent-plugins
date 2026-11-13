---
name: refine-doks
description: "Audit and refine existing project documentation in `base-dir` (from `./doks.yml`) against Doks guidelines for glossary (`*.glossary.md`), user story (`*.story.md`), and architectural (`*.arch.md`) files. Updates the body of only one documentation file per run while allowing cross-reference updates in other files. Use when asked to refine, review, improve, or polish existing documentation."
---

# Refine Doks (`refine-doks`)

Read existing technical documentation and specifications in the project's configured documentation directory, check them against the Doks guidelines, and refine them with focused, surgical edits.

## Scope & Change Rules

- **Single-File Body Rule**: Each invocation must update the body content of **at most one** documentation file.
- **Cross-Reference Exception**: Modifying other documentation files in the same run is permitted **only** to add or update cross-references (such as linking a domain term to its `*.glossary.md` file, linking between `*.story.md` and `*.arch.md` files, or fixing broken relative links). Do not rewrite, expand, or restructure the prose body of those secondary files.

## Documentation Guidelines

Documentation under `<base-dir>/` (organized either flat or grouped into domain subfolders) must adhere to the following standards:

### Document Types

1. **Glossary Files (`<term>.glossary.md`)**
   - Defines a single domain or system term.
   - Follow the guidelines in [glossary.md](../../glossary.md) when writing or refining glossary files.

2. **User Story Files (`<operation>.story.md`)**
   - Describes an operation a user (or external actor) can perform on the system.
   - Specifies the actor, preconditions, trigger/inputs, step-by-step behavior, expected outcomes/postconditions, error/edge cases, and testable acceptance criteria.

3. **Architectural Documents (`<component-or-subsystem>.arch.md`)**
   - Provides a high-level overview of how a part of the system works.
   - Explains component responsibilities, boundaries, data/control flow, key design trade-offs, and anchors to the source files/modules that implement it.

### Cross-Linking & Grounding Rules

- **Glossary Links**: When a user story or architectural document references a defined domain term, link it to its corresponding `*.glossary.md` file using a relative Markdown link.
- **Story <-> Architecture Links**: Cross-reference user stories (`*.story.md`) with the architectural documents (`*.arch.md`) that implement them, and list supported user stories or key domain terms in architectural documents where relevant.
- **Code Anchors**: Ground glossary definitions, user stories, and architectural overviews with accurate links or paths to the actual source files, symbols, and tests in the repository.

## Workflow

1. **Resolve Configuration (`./doks.yml`)**
   - Check the repository root for `./doks.yml` and read the `base-dir` property.
   - If `./doks.yml` does not exist or `base-dir` is not configured:
     1. Ask the user which root directory they want to use for documentation (e.g., `docs`).
     2. Create `./doks.yml` at the repository root with:
        ```yaml
        base-dir: <chosen-directory>
        ```
   - Verify that `<base-dir>/` exists and contains documentation files to refine.

2. **Audit Existing Documentation via a Sub-Agent**
   - Do **not** scan or read all documentation files directly in the main agent, as doing so pollutes the main agent's context window.
   - Launch a read-only exploration sub-agent (e.g., using the `task` tool with `agent_type: "explore"`) and pass it:
     - The configured `<base-dir>` path.
     - Any specific target file or topic requested by the user (if provided).
     - A summary of the Document Types and Cross-Linking & Grounding Rules above.
   - Instruct the sub-agent to inspect `<base-dir>/` (`*.glossary.md`, `*.story.md`, `*.arch.md`, and any index files) alongside relevant source files in the repository, and return **only**:
     1. **Target File**: The single documentation file path that should have its body refined (either the user-specified file or the file with the most significant gaps, inaccuracies, stale code anchors, or guideline violations).
     2. **Findings**: Specific guideline gaps or inaccuracies found in that target file (missing sections, vague criteria, outdated code paths/symbols, missing invariants, etc.).
     3. **Cross-Reference Candidates**: Other existing documentation files that should be linked from the target file or that need cross-reference link updates pointing to the target file.

3. **Refine the Target File and Update Cross-References**
   - In the main agent, read the selected target documentation file (and any relevant source code files needed to verify accuracy) along with the specific sections of candidate files that require cross-reference links.
   - **Update the Target File's Body**:
     - Bring the target file into full compliance with its document type guidelines (`*.glossary.md`, `*.story.md`, or `*.arch.md`).
     - Ensure all code anchors (file paths, symbols, types, tests) match the current codebase.
     - Add missing relative Markdown links from the target file to related glossary, user story, and architectural files.
   - **Update Cross-References in Other Files (If Needed)**:
     - In other documentation files, **only** add or update relative Markdown links / cross-references (for example, linking mentions of a glossary term or adding a link in a "Related Stories/Architecture" section).
     - Never modify or rewrite the body prose of any file other than the single target file.
