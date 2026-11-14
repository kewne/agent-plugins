# Architectural Documents (`<aspect>.architecture.md`)

An architectural document provides a high-level description of an aspect of the system. It must not be a mere restatement of the code; instead, it should allow the reader to quickly grasp how that part of the system works.

## Guidelines

- **Central Building Blocks Only**: Only describe the central building blocks of the aspect being documented.
- **Libraries**: Mention the libraries used to implement the feature or aspect.
- **No Exhaustive Enumerations**: Never enumerate all concrete instances of something in the code (e.g., all the controllers with authentication, or all the authentication rules).
- **Cross-Cutting Concerns**: If the document is about a cross-cutting concern of the system, only add cross-references to other documents with general concerns.
- **Diagrams**: Include diagrams (such as Mermaid diagrams) to illustrate structure.

## Structure

1. **Aspect Identification**: Begin by identifying the aspect of the system being described.
2. **Context**: Provide a short section explaining the context and the reason for this aspect to exist.
3. **Overview**: Summarize the aspect as a whole so the reader can understand it at once.
4. **Detailed Subsections**: Follow the overview with subsections that describe the parts of the aspect in more detail.
