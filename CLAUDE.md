# Claude Code Game Studios -- Game Studio Agent Architecture

Indie game development on s&box, managed through coordinated Claude Code subagents.
Each agent owns a specific domain, enforcing separation of concerns and quality.

## Technology Stack

- **Engine**: s&box (Source 2 / .NET 10)
- **Language**: C#
- **Version Control**: Git with trunk-based development
- **Build System**: `.sbproj` addon projects
- **Asset Pipeline**: s&box content system (models, materials, sounds via editor)
- **UI Framework**: Razor (PanelComponent with HTML/CSS)

> **Note**: s&box-specialist agents exist for core engine, networking, Razor UI,
> and tooling. Use the specialist matching your current task.

## Project Structure

@.claude/docs/directory-structure.md

## Engine Version Reference

@docs/engine-reference/sbox/VERSION.md

## Technical Preferences

@.claude/docs/technical-preferences.md

## Coordination Rules

@.claude/docs/coordination-rules.md

## Collaboration Protocol

**User-driven collaboration, not autonomous execution.**
Every task follows: **Question -> Options -> Decision -> Draft -> Approval**

- Agents MUST ask "May I write this to [filepath]?" before using Write/Edit tools
- Agents MUST show drafts or summaries before requesting approval
- Multi-file changes require explicit approval for the full changeset
- No commits without user instruction

See `docs/COLLABORATIVE-DESIGN-PRINCIPLE.md` for full protocol and examples.

## Coding Standards

@.claude/docs/coding-standards.md

## Context Management

@.claude/docs/context-management.md
