# AGENTS.md

This file contains guidelines and commands for agentic coding agents working in the spoilerjs repository.

## Project Overview

spoilerjs is a monorepo containing a lightweight web component library for creating spoiler text with particle effects, built with StencilJS and TypeScript.

- **Main Package**: `packages/components` - StencilJS web component library
- **Documentation Site**: `packages/site` - SvelteKit documentation site
- **Package Manager**: pnpm (required)
- **Node Version**: >=16.0.0

## Build Commands

### Root Level Commands
```bash
# Install all dependencies
pnpm install

# Development (starts documentation site)
pnpm dev

# Development (components only)
pnpm dev:components

# Build all packages
pnpm build

# Build components only
pnpm build:components

# Build documentation site only
pnpm build:site
```

### Components Package Commands
```bash
# Navigate to components directory
cd packages/components

# Development server with hot reload
pnpm start

# Build for production
pnpm build

# Run all tests (spec + e2e)
pnpm test

# Run tests in watch mode
pnpm test.watch

# Generate new component
pnpm generate
```

### Documentation Site Commands
```bash
# Navigate to site directory
cd packages/site

# Development server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview

# Type checking
pnpm check

# Type checking in watch mode
pnpm check:watch

# Lint code
pnpm lint

# Format code
pnpm format
```

## Testing

- **Test Framework**: Jest with StencilJS testing utilities
- **E2E Testing**: Puppeteer
- **Single Test**: Use `pnpm test --spec` to run unit tests only
- **Watch Mode**: Use `pnpm test.watch` for development
- **Test Location**: Tests should be placed in `packages/components/src/` alongside components

## Code Style Guidelines

### TypeScript Configuration
- **Target**: ES2022
- **Module**: ESNext with bundler resolution
- **Strict Mode**: Enabled (noUnusedLocals, noUnusedParameters)
- **Decorators**: Experimental decorators enabled for StencilJS
- **JSX**: React-like with `h` function factory

### Import Style
```typescript
// StencilJS imports first
import { Component, State, Prop, Element, h } from '@stencil/core';

// Local imports next
import { BoundingBox, ParticleConfig } from './types';
import { ParticleManager } from './particle-manager';

// External libraries last
import { SomeLibrary } from 'some-library';
```

### Component Structure
```typescript
@Component({
    tag: 'component-name',
    styleUrl: 'component-name.css',
    shadow: true,
})
export class ComponentName {
    @Element() el: HTMLElement;
    
    // Props first with JSDoc comments
    @Prop() propName: type = defaultValue;
    
    // State variables
    @State() stateVar: type = defaultValue;
    
    // Private properties
    private property: type;
    
    // Lifecycle methods
    componentDidLoad() { }
    disconnectedCallback() { }
    
    // Private methods
    private method() { }
    
    // Event handlers
    private handleEvent = () => { };
    
    // Render method last
    render() { }
}
```

### Naming Conventions
- **Components**: PascalCase (e.g., `SpoilerSpan`)
- **Custom Elements**: kebab-case (e.g., `spoiler-span`)
- **Files**: kebab-case for components (e.g., `spoiler-span.tsx`)
- **Methods**: camelCase with descriptive names
- **Private Methods**: prefix with `private` keyword
- **Constants**: UPPER_SNAKE_CASE for static values
- **Interfaces**: PascalCase with descriptive names (e.g., `ParticleConfig`)

### Formatting Rules (Components Package)
- **Indentation**: 2 spaces (no tabs)
- **Quotes**: Single quotes
- **Semicolons**: Required
- **Trailing Commas**: All (ES5 compatible)
- **Print Width**: 180 characters
- **Arrow Parentheses**: Avoid when possible
- **Bracket Spacing**: Enabled

### Error Handling
- Use try-catch blocks for async operations
- Log errors with context using `console.error`
- Return early for error conditions
- Use TypeScript's strict typing to prevent runtime errors

### Performance Guidelines
- Use `requestAnimationFrame` for animations
- Implement debouncing for expensive operations (scroll, resize)
- Clean up event listeners and timers in `disconnectedCallback`
- Use ResizeObserver for efficient size monitoring
- Throttle animation frames based on target FPS

### Web Component Best Practices
- Always include accessibility attributes (role, aria-label, tabindex)
- Handle keyboard events (Enter, Space) for interactive components
- Use shadow DOM for style encapsulation
- Provide semantic HTML structure
- Consider touch events for mobile compatibility

### CSS Guidelines
- Use CSS custom properties for theming
- Organize styles with clear sections
- Use relative units (em, rem) for scalability
- Include CSS custom properties in component interface
- Follow BEM methodology for class names when needed

## Development Workflow

1. **Setup**: Run `pnpm install` from root
2. **Development**: Use `pnpm dev:components` for component work
3. **Testing**: Run `pnpm test.watch` during development
4. **Building**: Use `pnpm build:components` before publishing
5. **Linting**: Check code style before commits
6. **Type Checking**: Ensure TypeScript compilation succeeds

## Package Publishing

- Components are published to npm as `spoilerjs`
- Use `pnpm publish:components` from root
- Pre-publish hook runs build automatically
- Version management with `pnpm version:components`

## File Structure

```
packages/components/src/
├── components/
│   └── component-name/
│       ├── component-name.tsx
│       ├── component-name.css
│       ├── types.ts
│       └── utils.ts
├── utils/
│   └── shared-utils.ts
├── index.ts
└── components.d.ts
```

## Important Notes

- Always use TypeScript interfaces for prop and state types
- Follow StencilJS lifecycle methods properly
- Implement proper cleanup in `disconnectedCallback`
- Use JSDoc comments for all public APIs
- Test both unit and integration scenarios
- Consider browser compatibility when using modern APIs