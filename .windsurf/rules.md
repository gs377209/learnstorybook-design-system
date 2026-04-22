# Windsurf Rules for learnstorybook-design-system

React design system with Storybook and Emotion CSS-in-JS.

## Quick Setup

```bash
yarn storybook          # Start dev server
yarn build              # Build distributions
yarn test-storybook     # Run tests
```

## Component Architecture

```
src/[ComponentName]/
├── ComponentName.jsx         # React component (use Emotion + forwardRef)
├── ComponentName.stories.jsx # Storybook stories (all variants)
├── ComponentName.mdx         # Documentation
└── index.js                  # export * from './ComponentName'
```

## Styling with Emotion

```jsx
import styled from '@emotion/styled';
import { color, typography } from '../shared/styles';
import { easing } from '../shared/animation';
import { darken, rgba } from 'polished';

const StyledButton = styled.button`
  color: ${color.primary};
  font-family: ${typography.base};
  transition: all 150ms ${easing.easeOut};
  
  &:hover {
    background: ${darken(0.1, color.primary)};
  }
`;
```

## Component Props Template

```jsx
import { forwardRef } from 'react';
import PropTypes from 'prop-types';

const APPEARANCES = {
  PRIMARY: 'primary',
  SECONDARY: 'secondary',
};

const SIZES = {
  SMALL: 'small',
  MEDIUM: 'medium',
};

export const Button = forwardRef(({ appearance, size, ...props }, ref) => {
  return <StyledButton ref={ref} {...props} />;
});

Button.propTypes = {
  appearance: PropTypes.oneOf(Object.values(APPEARANCES)),
  size: PropTypes.oneOf(Object.values(SIZES)),
};
```

## Story Template

```jsx
import { Button, APPEARANCES, SIZES } from './Button';

export default {
  title: 'Components/Button',
  component: Button,
  argTypes: {
    appearance: { control: 'select', options: Object.values(APPEARANCES) },
    size: { control: 'select', options: Object.values(SIZES) },
  },
};

export const Primary = { args: { appearance: APPEARANCES.PRIMARY } };
export const Secondary = { args: { appearance: APPEARANCES.SECONDARY } };
export const Small = { args: { size: SIZES.SMALL } };
export const Medium = { args: { size: SIZES.MEDIUM } };
```

## Build Details

- **Format**: Dual output (CJS + ESM)
- **Bundler**: Rollup with babel, node-resolve, commonjs
- **Minification**: Terser plugin enabled
- **External**: react, react-dom, @emotion/react, @emotion/styled
- **Babel**: @emotion/babel-plugin for optimizations

## Key Files

- `src/shared/styles.js` - Color, typography tokens
- `src/shared/animation.js` - Easing functions
- `src/shared/icons.js` - SVG icons
- `src/index.js` - Main entry point
- `rollup.config.mjs` - Build configuration
