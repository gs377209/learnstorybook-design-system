# Skill: Component Creation

Create new design system components following the established patterns.

## Overview

This skill scaffolds a complete new component with proper structure, styling, stories, and documentation.

## Usage

When asked to create or add a new component, use this pattern:

```
Create a new [ComponentName] component with:
- Visual variants (e.g., appearance, size options)
- Interactive states (hover, disabled, loading, etc.)
- Complete Storybook stories
- Proper documentation
```

## Component Structure Template

### 1. **src/ComponentName/ComponentName.jsx**

```jsx
import { forwardRef } from 'react';
import PropTypes from 'prop-types';
import styled from '@emotion/styled';
import { darken, rgba } from 'polished';
import { color, typography } from '../shared/styles';
import { easing } from '../shared/animation';

// Define appearance variants
const APPEARANCES = {
  PRIMARY: 'primary',
  SECONDARY: 'secondary',
};

// Define size variants
const SIZES = {
  SMALL: 'small',
  MEDIUM: 'medium',
  LARGE: 'large',
};

// Styled components
const StyledComponentName = styled.button`
  /* Base styles */
  font-family: ${typography.base};
  font-size: ${(props) => 
    props.size === SIZES.SMALL ? '12px' : 
    props.size === SIZES.LARGE ? '18px' : 
    '14px'};
  
  color: ${(props) => 
    props.appearance === APPEARANCES.PRIMARY ? color.white :
    color.darkGray};
    
  background: ${(props) => 
    props.appearance === APPEARANCES.PRIMARY ? color.primary :
    color.lighterGray};
    
  padding: ${(props) => 
    props.size === SIZES.SMALL ? '8px 12px' :
    props.size === SIZES.LARGE ? '16px 24px' :
    '12px 16px'};
    
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 150ms ${easing.easeOut};
  
  &:hover {
    background: ${(props) => 
      props.appearance === APPEARANCES.PRIMARY ? 
      darken(0.1, color.primary) :
      darken(0.05, color.lighterGray)};
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
`;

// Component implementation
export const ComponentName = forwardRef((
  { 
    appearance = APPEARANCES.PRIMARY,
    size = SIZES.MEDIUM,
    children,
    ...props 
  }, 
  ref
) => {
  return (
    <StyledComponentName
      ref={ref}
      appearance={appearance}
      size={size}
      {...props}
    >
      {children}
    </StyledComponentName>
  );
});

// Display name for debugging
ComponentName.displayName = 'ComponentName';

// PropTypes validation
ComponentName.propTypes = {
  appearance: PropTypes.oneOf(Object.values(APPEARANCES)),
  size: PropTypes.oneOf(Object.values(SIZES)),
  children: PropTypes.node,
  disabled: PropTypes.bool,
};

// Export constants for story usage
export { APPEARANCES, SIZES };
```

### 2. **src/ComponentName/ComponentName.stories.jsx**

```jsx
import { ComponentName, APPEARANCES, SIZES } from './ComponentName';

export default {
  title: 'Components/ComponentName',
  component: ComponentName,
  argTypes: {
    appearance: {
      control: 'select',
      options: Object.values(APPEARANCES),
    },
    size: {
      control: 'select',
      options: Object.values(SIZES),
    },
    disabled: {
      control: 'boolean',
    },
    children: {
      control: 'text',
    },
  },
};

// Primary variant
export const Primary = {
  args: {
    children: 'Button',
    appearance: APPEARANCES.PRIMARY,
  },
};

// Secondary variant
export const Secondary = {
  args: {
    children: 'Button',
    appearance: APPEARANCES.SECONDARY,
  },
};

// Size variants
export const Small = {
  args: {
    children: 'Button',
    size: SIZES.SMALL,
  },
};

export const Medium = {
  args: {
    children: 'Button',
    size: SIZES.MEDIUM,
  },
};

export const Large = {
  args: {
    children: 'Button',
    size: SIZES.LARGE,
  },
};

// State variants
export const Disabled = {
  args: {
    children: 'Button',
    disabled: true,
  },
};

// Matrix of all combinations
export const AllVariants = {
  render: () => (
    <div style={{ display: 'flex', gap: '16px', flexWrap: 'wrap' }}>
      {Object.values(APPEARANCES).map((app) =>
        Object.values(SIZES).map((size) => (
          <ComponentName
            key={`${app}-${size}`}
            appearance={app}
            size={size}
          >
            {app} {size}
          </ComponentName>
        ))
      )}
    </div>
  ),
};
```

### 3. **src/ComponentName/ComponentName.mdx**

```mdx
import { Meta, Story, Canvas } from '@storybook/blocks';
import * as ComponentNameStories from './ComponentName.stories';

<Meta of={ComponentNameStories} />

# ComponentName

Brief description of the component's purpose and use cases.

## Usage

Explain when and why to use this component.

## Appearance Variants

Description of the available appearances.

<Canvas of={ComponentNameStories.Primary} />
<Canvas of={ComponentNameStories.Secondary} />

## Size Variants

Description of available sizes.

<Canvas of={ComponentNameStories.Small} />
<Canvas of={ComponentNameStories.Medium} />
<Canvas of={ComponentNameStories.Large} />

## States

### Disabled

The disabled state prevents user interaction.

<Canvas of={ComponentNameStories.Disabled} />

## All Variants

<Canvas of={ComponentNameStories.AllVariants} />

## Related Components

- Link to similar components
- Link to composition examples
```

### 4. **src/ComponentName/index.js**

```jsx
export * from './ComponentName';
```

## Implementation Steps

1. Create the folder: `mkdir src/ComponentName`
2. Create the four files above (JSX, stories, mdx, index.js)
3. Update `src/index.js` to include: `export * from './ComponentName'`
4. Run `yarn storybook` to verify the component appears
5. Test all variants and states in Storybook
6. Run `yarn build` to ensure the build completes successfully

## Key Requirements

✅ Use `forwardRef` for components that may need refs
✅ Export variant constants (APPEARANCES, SIZES, etc.)
✅ Use PropTypes for prop validation
✅ Emotion styling only - no external CSS
✅ Import design tokens from `../shared/styles`
✅ Document all prop combinations in stories
✅ Include interactive states (hover, disabled, etc.)
✅ Update main index.js with new export

## Design Tokens Reference

Use these from `src/shared/styles.js`:
- `color.primary`, `color.secondary`, `color.white`, `color.darkGray`, `color.lighterGray`
- `typography.base`, `typography.heading`, `typography.weight.bold`
- Line heights, letter spacing, etc.

Use these from `src/shared/animation.js`:
- `easing.easeOut`, `easing.easeIn`, `easing.easeInOut`

Use these from `polished`:
- `darken()`, `lighten()`, `rgba()`, `saturate()`, `desaturate()`
