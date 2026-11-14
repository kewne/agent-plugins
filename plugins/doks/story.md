# User Story Files (`<operation>.story.md`)

A user story file describes an operation a user (or external actor) can perform on the system and explains its intended behavior.

## Guidelines

- **No Status**: Story files must not contain an implementation status (e.g., draft, in progress, or done). A user story defines intention, and recording a status in the file can go out of date.
- **No Implementation Details**: Story files must not contain any implementation details or reference code. When implementation details need to be documented, they belong in [architectural documents](./architecture.md) (`*.architecture.md`).

## Structure

1. **Story**: Begin with the standard user story format: "As a X, I want Y so I can Z".
2. **Context**: Provide additional explanation for why this story is needed.
3. **Acceptance Criteria**: List the criteria that must be met for the story to be considered implemented.
