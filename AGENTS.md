# Agent Instructions for learnstorybook-design-system

This is a **React-based design system** component library built with Storybook, Emotion (CSS-in-JS), and Rollup. The project demonstrates best practices for writing and publishing a reusable design system.

## Quick Start Commands

```bash
# Development
yarn storybook              # Start Storybook dev server (port 6006)
yarn watch                  # Watch mode for library builds
yarn build                  # Build distribution files (CJS + ESM)

# Testing & Quality
yarn test-storybook         # Run Storybook tests (interaction & visual)
yarn test-vite              # Run Vitest tests
yarn chromatic              # Visual regression testing on Chromatic

# Documentation
yarn build-storybook        # Build static Storybook docs
yarn build-storybook-docs   # Build Storybook with docs mode
```

## Project Structure

```
src/
├── shared/                 # Shared utilities and design tokens
│   ├── styles.js          # Color and typography constants
│   ├── animation.js       # Animation/easing utilities
│   ├── icons.js           # Icon SVG definitions
│   └── global.js          # Global styles/themes
├── [ComponentName]/        # Each component in its own folder
│   ├── ComponentName.jsx   # Component implementation
│   ├── ComponentName.stories.jsx   # Storybook stories
│   ├── ComponentName.mdx   # Component documentation
│   └── index.js            # Export statement
├── stories/                # Example Storybook pages (not exported)
└── index.js                # Main entry point (exports all public components)
```

## Component Architecture & Conventions

### Component Pattern
Each component follows a consistent structure:

1. **ComponentName.jsx** - Component implementation using Emotion styled components
   - Use `forwardRef` for components that may need ref forwarding
   - Export both the component and typed constants (e.g., APPEARANCES, SIZES)
   - Use PropTypes for prop validation

2. **ComponentName.stories.jsx** - Storybook stories (CSF format)
   - Include basic story + variants showing all prop combinations
   - Document interactive states (hover, active, disabled, loading, etc.)

3. **ComponentName.mdx** - Markdown documentation
   - Describe purpose and use cases
   - Show code examples
   - Link to related components

4. **index.js** - Always export: `export * from './ComponentName'`

### Styling with Emotion
- Use `styled()` from `@emotion/styled` for component styling
- Import design tokens from `../shared/styles` (colors, typography)
- Import animations from `../shared/animation`
- Use helper functions from `polished` (darken, rgba, lighten, etc.)
- Use CSS-in-JS for all styling; no external CSS files per component

### Props Pattern
```jsx
// Example from Button component
const APPEARANCES = {
  PRIMARY: 'primary',
  SECONDARY: 'secondary',
  // ... more variants
};

const SIZES = {
  SMALL: 'small',
  MEDIUM: 'medium',
};

const StyledButton = styled.button`...`;

export const Button = forwardRef((props, ref) => {
  // Component logic
});

Button.propTypes = {
  appearance: PropTypes.oneOf(Object.values(APPEARANCES)),
  size: PropTypes.oneOf(Object.values(SIZES)),
  // ... other props
};
```

## Build Process

- **Development**: Storybook + Vite for fast HMR and testing
- **Distribution**: Rollup outputs two formats:
  - **CommonJS** (CJS): `dist/cjs/index.js` for Node.js/require()
  - **ESM**: `dist/esm/index.js` for modern bundlers
  - External dependencies: react, react-dom, @emotion/react, @emotion/styled
  - Minification: terser plugin applied
  - Source maps: Enabled for both formats

## Design Tokens & Utilities

### Shared Styles (`src/shared/styles.js`)
- `color` - Color palette constants
- `typography` - Font families, sizes, weights, line heights

### Shared Animation (`src/shared/animation.js`)
- `easing` - Easing function curves (ease-out, ease-in-out, etc.)

### Icons (`src/shared/icons.js`)
- SVG icon definitions

### Global (`src/shared/global.js`)
- Global theme configuration

## Testing Strategy

- **Storybook Tests**: Interactive and visual tests run via vitest plugin
- **Tool**: @storybook/addon-vitest for integration
- **Visual Regression**: Chromatic for automated visual testing
- **Accessibility**: @storybook/addon-a11y for a11y audits in Storybook

## When Creating New Components

1. Create a new folder: `src/ComponentName/`
2. Add ComponentName.jsx with styled components and PropTypes
3. Add ComponentName.stories.jsx with variants
4. Add ComponentName.mdx with documentation
5. Add index.js with named export
6. Update `src/index.js` to export the new component
7. Run `yarn storybook` to verify
8. Create stories for all appearance/size combinations
9. Ensure PropTypes and TypeScript compatibility

## Key Dependencies

- **React** 18+ (peer dependency)
- **Emotion** v11 (@emotion/react, @emotion/styled)
- **Storybook** 10.x with React-Vite setup
- **Rollup** for bundling (ESM + CJS)
- **Vite** for testing (vitest)
- **Polished** for color utilities
- **PropTypes** for runtime prop validation

## Publishing & Release

- Uses **auto** CLI for automated releases
- Publishes to npm as @gs377209/learnstorybook-design-system-template
- Changelog automatically generated
- Release workflow: `yarn release`

## Documentation Links

- [README.md](README.md) - Project overview
- [CONTRIBUTING.md](CONTRIBUTING.md) - Contribution guidelines
- [Storybook Docs](https://storybook.js.org/) - Storybook best practices
- [Emotion Docs](https://emotion.sh/docs/styled) - Emotion CSS-in-JS patterns
