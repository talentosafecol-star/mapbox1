# Design Interfaces - AGENTS.md

## Overview

This document provides comprehensive guidelines for AI agents designing and implementing user interfaces.

## UX Principles

### Visual Hierarchy

- Size: Larger elements = more important
- Color: Bright/saturated = attention
- Position: Top-left = first scan
- Spacing: More space = more importance

### Consistency

```css
/* Use design tokens for consistency */
:root {
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
  
  --color-primary: #3b82f6;
  --color-secondary: #64748b;
}
```

### Feedback

- Loading states for async operations
- Success/error messages
- Hover/focus states
- Disabled states

## Component Guidelines

### Buttons

```tsx
// Primary button
<button className="btn btn-primary">
  Primary Action
</button>

// Secondary button  
<button className="btn btn-secondary">
  Secondary
</button>

// Disabled state
<button className="btn btn-primary" disabled>
  Disabled
</button>
```

### Forms

```tsx
<form>
  <label htmlFor="email">Email</label>
  <input 
    id="email"
    type="email" 
    placeholder="user@example.com"
    aria-describedby="email-hint"
  />
  <span id="email-hint">We'll never share your email</span>
  
  <label htmlFor="password">Password</label>
  <input 
    id="password" 
    type="password"
    required
  />
  
  <button type="submit">Submit</button>
</form>
```

### Modals

```tsx
<dialog>
  <header>
    <h2>Modal Title</h2>
    <button aria-label="Close">×</button>
  </header>
  <main>
    Modal content
  </main>
  <footer>
    <button>Cancel</button>
    <button>Confirm</button>
  </footer>
</dialog>
```

## Accessibility (WCAG 2.1)

### Color Contrast

- Normal text: 4.5:1 minimum
- Large text (18px+): 3:1 minimum
- UI components: 3:1 minimum

### Focus Management

```tsx
// Trap focus in modal
useEffect(() => {
  if (isOpen) {
    const focusable = modalRef.current.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    focusable[0]?.focus();
  }
}, [isOpen]);
```

### ARIA Labels

```tsx
<button 
  aria-label="Close menu"
  aria-expanded={isOpen}
  aria-controls="main-menu"
>
  <span className="sr-only">Close</span>
  <Icon />
</button>
```

### Skip Links

```html
<a href="#main" class="skip-link">Skip to main content</a>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px;
  z-index: 100;
}
.skip-link:focus {
  top: 0;
}
</style>
```

## Responsive Design

### Mobile-First Approach

```css
/* Base styles (mobile) */
.container {
  padding: 16px;
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    padding: 24px;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    padding: 32px;
    max-width: 1200px;
  }
}
```

### Fluid Typography

```css
:root {
  --font-size-base: clamp(1rem, 0.34vw + 0.91rem, 1.19rem);
  --font-size-lg: clamp(1.33rem, 1.2vw + 1rem, 2rem);
}
```

## Dark Mode

### Implementation

```css
@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #1a1a2e;
    --text-primary: #e4e4e4;
    --border-color: #333;
  }
}

/* Toggle class */
.dark {
  --bg-primary: #1a1a2e;
  --text-primary: #e4e4e4;
}
```

### React Implementation

```tsx
import { useState, useEffect } from 'react';

function useDarkMode() {
  const [theme, setTheme] = useState('light');
  
  useEffect(() => {
    const saved = localStorage.getItem('theme');
    if (saved) setTheme(saved);
    else if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
      setTheme('dark');
    }
  }, []);
  
  useEffect(() => {
    document.documentElement.classList.toggle('dark', theme === 'dark');
    localStorage.setItem('theme', theme);
  }, [theme]);
  
  return [theme, setTheme];
}
```

## Design Tokens

### Structure

```json
{
  "color": {
    "primary": {
      "50": "#eff6ff",
      "500": "#3b82f6",
      "900": "#1e3a8a"
    }
  },
  "spacing": {
    "1": "4px",
    "2": "8px",
    "4": "16px"
  },
  "font": {
    "family": {
      "sans": "Inter, system-ui, sans-serif"
    },
    "size": {
      "xs": "12px",
      "sm": "14px",
      "base": "16px"
    }
  },
  "radius": {
    "sm": "4px",
    "md": "8px",
    "full": "9999px"
  }
}
```
