# Skill: Styling Guidelines

Best practices for styling components in this design system using Emotion CSS-in-JS.

## Overview

All components use `@emotion/styled` for styling. No external CSS files are permitted per component. Design tokens are centralized in `src/shared/styles.js`.

## Core Principles

1. **Token-based**: Always use design tokens for colors, typography, spacing
2. **Emotion only**: No CSS modules, no inline styles, no SASS/LESS
3. **Utility functions**: Use `polished` for color transformations
4. **Animations**: Reference easing functions from `src/shared/animation.js`
5. **Responsive**: Use CSS media queries within Emotion styled components
6. **Accessibility**: Ensure proper color contrast and semantic HTML

## Design Tokens

### Colors (`src/shared/styles.js`)

```jsx
import { color } from '../shared/styles';

// Common usage
background: ${color.primary};       // Main brand color
color: ${color.text.primary};       // Primary text
border-color: ${color.border.light}; // Subtle borders
```

### Typography (`src/shared/styles.js`)

```jsx
import { typography } from '../shared/styles';

// Font family
font-family: ${typography.base};    // Body text
font-family: ${typography.heading}; // Headings

// Font weights
font-weight: ${typography.weight.normal};   // 400
font-weight: ${typography.weight.bold};     // 700

// Sizes (use your own sizing)
font-size: 14px;
line-height: ${typography.lineHeight};
letter-spacing: ${typography.letterSpacing};
```

### Animation (`src/shared/animation.js`)

```jsx
import { easing } from '../shared/animation';

transition: all 150ms ${easing.easeOut};
transition: background 300ms ${easing.easeInOut};
```

## Styling Patterns

### Basic Styled Component

```jsx
import styled from '@emotion/styled';
import { color, typography } from '../shared/styles';

const StyledButton = styled.button`
  background: ${color.primary};
  color: ${color.white};
  font-family: ${typography.base};
  font-weight: ${typography.weight.bold};
  padding: 12px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  &:hover {
    background: ${color.primaryHover};
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
`;
```

### With Props

```jsx
const StyledButton = styled.button`
  padding: ${(props) => 
    props.size === 'small' ? '8px 12px' :
    props.size === 'large' ? '16px 24px' :
    '12px 16px'};
    
  font-size: ${(props) => 
    props.size === 'small' ? '12px' :
    props.size === 'large' ? '18px' :
    '14px'};
`;
```

### Using Polished for Colors

```jsx
import { darken, lighten, rgba } from 'polished';
import { color } from '../shared/styles';

const StyledCard = styled.div`
  background: ${color.background};
  border: 1px solid ${lighten(0.2, color.border)};
  
  &:hover {
    background: ${rgba(color.primary, 0.05)};
    border-color: ${darken(0.1, color.border)};
  }
`;
```

### Responsive Styles

```jsx
const StyledContainer = styled.div`
  padding: 16px;
  font-size: 14px;
  
  @media (max-width: 768px) {
    padding: 12px;
    font-size: 12px;
  }
  
  @media (min-width: 1024px) {
    padding: 24px;
    font-size: 16px;
  }
`;
```

### Nested Selectors

```jsx
const StyledForm = styled.form`
  input {
    font-family: ${typography.base};
    padding: 8px 12px;
    border: 1px solid ${color.border};
    border-radius: 4px;
    
    &:focus {
      outline: none;
      border-color: ${color.primary};
      box-shadow: 0 0 0 3px ${rgba(color.primary, 0.1)};
    }
  }
  
  button {
    margin-top: 16px;
  }
`;
```

### CSS-in-JS Variables

```jsx
const StyledBox = styled.div`
  --padding: ${(props) => props.padding || '16px'};
  --color: ${(props) => props.color || color.text};
  
  padding: var(--padding);
  color: var(--color);
`;
```

## Common Patterns

### Loading State with Opacity

```jsx
const StyledButton = styled.button`
  opacity: ${(props) => props.loading ? 0.6 : 1};
  cursor: ${(props) => props.loading ? 'not-allowed' : 'pointer'};
  pointer-events: ${(props) => props.loading ? 'none' : 'auto'};
`;
```

### Focus Styles for Accessibility

```jsx
const StyledInput = styled.input`
  padding: 8px 12px;
  border: 1px solid ${color.border};
  border-radius: 4px;
  
  &:focus {
    outline: none;
    border-color: ${color.primary};
    box-shadow: 0 0 0 3px ${rgba(color.primary, 0.1)};
  }
`;
```

### Animation Transitions

```jsx
import { easing } from '../shared/animation';

const StyledMenuItem = styled.li`
  transition: all 150ms ${easing.easeOut};
  color: ${color.text};
  
  &:hover {
    background: ${rgba(color.primary, 0.1)};
    transform: translateX(4px);
  }
`;
```

### Multiple States

```jsx
const StyledBadge = styled.span`
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: ${typography.weight.bold};
  
  ${(props) => {
    switch (props.variant) {
      case 'success':
        return `background: ${color.success}; color: ${color.white};`;
      case 'warning':
        return `background: ${color.warning}; color: ${color.white};`;
      case 'error':
        return `background: ${color.error}; color: ${color.white};`;
      default:
        return `background: ${color.gray}; color: ${color.text};`;
    }
  }}
`;
```

## Performance Tips

1. **Use CSS props for rarely-changing styles**: `css` prop instead of creating new styled components
2. **Memoize complex styled components**: Cache expensive calculations
3. **Avoid inline functions in styled components**: Define outside if possible
4. **Use transient props**: `$visible` instead of `visible` to avoid passing to DOM

Example:
```jsx
const StyledBox = styled.div`
  display: ${(props) => props.$visible ? 'block' : 'none'};
`;
```

## Common Mistakes to Avoid

❌ **Don't mix CSS and Emotion**
```jsx
// Wrong
<div className="external-css" css={emotionStyles}>
```

✅ **Do use only Emotion**
```jsx
const StyledDiv = styled.div`/* emotion styles */`;
<StyledDiv>Content</StyledDiv>
```

❌ **Don't hardcode colors**
```jsx
// Wrong
background: #007bff;
```

✅ **Do use design tokens**
```jsx
import { color } from '../shared/styles';
background: ${color.primary};
```

❌ **Don't use inline styles**
```jsx
// Wrong
<div style={{ color: 'red', padding: '16px' }}>
```

✅ **Do use styled components**
```jsx
const StyledDiv = styled.div`
  color: ${color.error};
  padding: 16px;
`;
```

## Debugging

Use browser DevTools to inspect Emotion styles:
- Emotion generates unique class names like `.css-xyz`
- Open DevTools → Styles tab to see compiled CSS
- Use `&&` selector for specificity when needed
- Check Emotion cache in React DevTools
