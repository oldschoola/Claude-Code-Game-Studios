---
paths:
  - "Libraries/**"
---

# Third-Party Library Rules

- NEVER modify third-party library code without explicit approval
- All library modifications must be tracked and documented in architecture decisions
- Prefer wrapping/extending library types over modifying source
- Lock library versions — do not update without testing full game functionality
- Document any library limitations or workarounds in `docs/architecture/`
- If a library needs a fix, contribute upstream first before forking
