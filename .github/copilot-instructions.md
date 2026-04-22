# GitHub Copilot Instructions

This is a React-based design system component library. For comprehensive guidance on project structure, conventions, and development workflows, see [AGENTS.md](../AGENTS.md).

## Key Commands

- `yarn storybook` - Start Storybook dev server
- `yarn build` - Build CJS + ESM distributions
- `yarn test-storybook` - Run interaction tests
- `yarn watch` - Watch mode for development

## Component Structure

Each component lives in `src/[ComponentName]/` with:

- `ComponentName.jsx` - React component with Emotion styling
- `ComponentName.stories.jsx` - Storybook stories
- `ComponentName.mdx` - Documentation
- `index.js` - Export statement

## Styling

- Use `@emotion/styled` for all styling
- Import design tokens from `../shared/styles`
- Use `polished` for color utilities
- No external CSS files

## Props Pattern

Export constant objects (APPEARANCES, SIZES) and use `PropTypes` for validation:

```jsx
const APPEARANCES = { PRIMARY: 'primary', SECONDARY: 'secondary' };
const SIZES = { SMALL: 'small', MEDIUM: 'medium' };

export const Button = forwardRef((props, ref) => {
  // component logic
});

Button.propTypes = {
  appearance: PropTypes.oneOf(Object.values(APPEARANCES)),
  size: PropTypes.oneOf(Object.values(SIZES)),
};
```

## Build Output

Rollup creates two distributions:

- `dist/cjs/index.js` - CommonJS format
- `dist/esm/index.js` - ES modules format

Both formats have external deps: react, react-dom, @emotion/react, @emotion/styled
