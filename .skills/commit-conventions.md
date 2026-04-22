# Skill: Commit Conventions

Guidelines for commit messages and version management in the design system.

## Overview

This project uses [auto](https://intuit.github.io/auto/) for automated versioning and changelog generation. Commit messages follow conventions that determine release type (major, minor, patch).

## Commit Message Format

All commits should follow the **Conventional Commits** specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type

Required. Must be one of:

- **feat**: A new feature → **MINOR** version bump
- **fix**: A bug fix → **PATCH** version bump
- **BREAKING CHANGE**: Breaking API change → **MAJOR** version bump
- **docs**: Documentation only
- **style**: Code style changes (no logic change)
- **refactor**: Code refactoring (no behavior change)
- **perf**: Performance improvements
- **test**: Test additions or updates
- **chore**: Build, dependencies, tooling changes

### Scope

Optional. Name of the affected component or area:

```
feat(Button): add loading state
fix(Badge): correct color contrast
docs(Avatar): add usage examples
refactor(shared/styles): reorganize color tokens
```

### Subject

- Use imperative, present tense: "add" not "added" or "adds"
- Don't capitalize first letter
- No period (.) at the end
- Limit to 50 characters

### Body

Optional but recommended for non-trivial changes:

- Explain **what** and **why**, not how
- Wrap at 72 characters
- Separate from subject with blank line

Example:
```
feat(Button): add loading state

The button now accepts a loading prop that:
- Shows a spinner inside the button
- Disables user interaction
- Adds visual feedback for async operations

This enables better UX for async actions.
```

### Footer

Optional. Use for:

- **Issue references**: `Fixes #123`, `Closes #456`
- **Breaking changes**: `BREAKING CHANGE: description`

Example:
```
fix(Link): update active state styling

Fixes #789
```

With breaking change:
```
feat(Button)!: remove deprecated appearance

BREAKING CHANGE: The 'ghost' appearance has been removed. Use 'tertiary' instead.

Fixes #321
```

## Examples

### Simple Bug Fix
```
fix(Button): correct padding on small size
```

### New Feature
```
feat(Icon): add new 'chevron' icon variant
```

### Breaking Change
```
feat(Badge)!: rename appearance variants

BREAKING CHANGE: Renamed PRIMARY → PRIMARY_SOLID, SECONDARY → SECONDARY_OUTLINE

Fixes #445
```

### Documentation
```
docs(AvatarList): add composition example
```

### Multiple Changes
```
feat(components): various improvements

- Add loading states to Button
- Fix Badge color contrast
- Improve Icon accessibility

Also includes:
- Updated design tokens
- New animation utilities
```

## Release Workflow

### Automated Releases

```bash
yarn release
```

This command:
1. Runs `yarn build` to create distributions
2. Uses `auto` to analyze commits
3. Determines version bump (MAJOR.MINOR.PATCH)
4. Updates CHANGELOG.md
5. Creates git tag
6. Publishes to npm

### Version Determination

- Any `feat:` commit → MINOR bump (unless already MAJOR)
- Any `fix:` commit → PATCH bump (unless already MAJOR/MINOR)
- Any `BREAKING CHANGE` → MAJOR bump

Example commit history:
```
feat(Button): add size variants      ← MINOR
fix(Badge): color bug                ← PATCH
feat(Icon): new icons                ← MINOR
```
Result: New version is MINOR (1.2.0 → 1.3.0)

With breaking change:
```
feat(Button)!: breaking change       ← MAJOR
fix(Badge): minor fix                ← Ignored
```
Result: New version is MAJOR (1.2.0 → 2.0.0)

## Best Practices

✅ **Do:**
- Use present tense imperative
- Be specific about what changed
- Reference related issues
- Keep commits focused and atomic
- Write descriptive bodies for complex changes

❌ **Don't:**
- Use past tense ("added", "fixed")
- Mix unrelated changes in one commit
- Use vague subjects ("update stuff", "misc fixes")
- Forget to reference closing issues
- Make commits too large to review

## Examples of Good Commits

```
feat(Button): add loading prop

Add optional loading state that displays spinner and
disables interaction during async operations.

Fixes #123
```

```
fix(Badge): improve color contrast for accessibility

Increase text color darkness from #666 to #333
to meet WCAG AA standards (4.5:1 ratio).

Refs #234
```

```
refactor(shared/styles): consolidate color definitions

Reorganize color token exports for better tree-shaking.
No functional changes.
```

```
chore: update Storybook to 10.3.5
```

## Common Mistakes

❌ Wrong subject format
```
Added new Button variants
Fixed bug in Badge
```

✅ Correct format
```
feat(Button): add new size variants
fix(Badge): resolve color issue
```

---

❌ No scope or vague scope
```
fix: stuff
feat: improvements
```

✅ Clear scope
```
fix(Button): correct hover state color
feat(Avatar): add initials fallback
```

---

❌ Unclear body
```
Changed some stuff in the component
```

✅ Clear body
```
Modified the component padding and border styles
for better mobile responsiveness.
```

## Tools

The project uses:
- **auto**: Automated versioning and publishing
- **CHANGELOG.md**: Auto-generated release notes (don't edit manually)
- **GitHub releases**: Created automatically from changelog

No manual version bumps needed—just commit with proper messages!
