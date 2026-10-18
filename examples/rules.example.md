# Team review rules (example)

Copy to `.pr-review/rules.md` in your repo and edit. The skill loads this file before reviewing.

## Stack limits
- Framework / library versions to respect (e.g. "React 17, no concurrent features").
- Language (e.g. "JavaScript only, no TypeScript").
- Styling approach (e.g. "CSS Modules only, tokens from `src/styles/vars`").

## Conventions
- Use the shared `FileUpload` component for uploads.
- Debounce search inputs 300-500 ms.
- All user-facing strings go through `t()`; add keys to every locale file.

## Always flag
- Leftover `console.log`, hardcoded ids, forced feature flags.
- Mutations without error handling or user feedback.
- New dependencies without a stated reason.

## Never flag
- Formatting already enforced by the pre-commit hook.
