# Accessibility Guide (WCAG 2.1)

## Core Principles

### Perceivable

Information must be presentable to users in ways they can perceive.

1. **Text Alternatives**: Provide text alternatives for non-text content
2. **Time-based Media**: Provide alternatives for time-based media
3. **Adaptable**: Create content that can be presented in different ways
4. **Distinguishable**: Make it easier for users to see and hear content

### Operable

User interface components must be operable.

1. **Keyboard Accessible**: All functionality keyboard accessible
2. **Enough Time**: Provide enough time for users to read and use content
3. **Seizures**: Do not design content that could cause seizures
4. **Navigable**: Provide ways to help users navigate and find content

### Understandable

Information and UI operation must be understandable.

1. **Readable**: Make text content readable and understandable
2. **Predictable**: Make Web pages appear and operate in predictable ways
3. **Input Assistance**: Help users avoid and correct mistakes

### Robust

Content must be robust enough for various user agents.

1. **Compatible**: Maximize compatibility with current and future user agents

## Color Contrast

### Requirements

| Level | Normal Text | Large Text | UI Components |
|-------|-------------|------------|----------------|
| AA    | 4.5:1       | 3:1        | 3:1            |
| AAA   | 7:1         | 4.5:1      | 4.5:1          |

### Large Text Definition

- 18pt (24px) or larger regular text
- 14pt (18.66px) or larger bold text

### CSS Implementation

```css
:root {
  /* Passes WCAG AA */
  --color-primary: #0052cc;
  --color-text: #172b4d;
  
  /* Passing contrast ratios */
  --color-success: #006644; /* 4.52:1 on white */
  --color-warning: #8a6d3b;  /* 4.52:1 on white */
  --color-error: #bf2600;    /* 5.24:1 on white */
}
```

## Keyboard Accessibility

### Focus Indicators

```css
:focus-visible {
  outline: 2px solid var(--color-focus);
  outline-offset: 2px;
}

/* Remove default outline for mouse users */
:focus:not(:focus-visible) {
  outline: none;
}
```

### Skip Links

```html
<a href="#main-content" class="skip-link">
  Skip to main content
</a>

<main id="main-content">
  <!-- Page content -->
</main>
```

```css
.skip-link {
  position: absolute;
  top: -100px;
  left: 0;
  background: var(--color-primary);
  color: white;
  padding: 8px 16px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

## Screen Reader Support

### Semantic HTML

```html
<!-- Good -->
<header>
  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>

<main>
  <h1>Page Title</h1>
  <article>
    <h2>Article Title</h2>
    <p>Content...</p>
  </article>
</main>

<footer>
  <p>Copyright 2024</p>
</footer>
```

### ARIA Labels

```tsx
// Button with icon only
<button 
  aria-label="Close dialog"
  aria-describedby="dialog-description"
>
  <Icon name="close" />
</button>

// Input with error
<input 
  id="email"
  aria-invalid="true"
  aria-describedby="email-error"
  aria-required="true"
/>
<span id="="alert">
  Please enter a valid email
</span>

email-error" role// Live region for dynamic content
<div aria-live="polite" aria-atomic="true">
  {notification}
</div>
```

## Forms

### Label Association

```html
<!-- Explicit association -->
<label for="username">Username</label>
<input id="username" type="text" />

<!-- Implicit association -->
<label>
  Email
  <input type="email" />
</label>
```

### Error Handling

```tsx
function FormInput({ label, error, id, ...props }) {
  return (
    <div className="form-field">
      <label htmlFor={id}>
        {label}
        {props.required && <span aria-hidden="true"> *</span>}
      </label>
      <input
        id={id}
        aria-invalid={!!error}
        aria-describedby={error ? `${id}-error` : undefined}
        {...props}
      />
      {error && (
        <span id={`${id}-error`} role="alert" className="error">
          {error}
        </span>
      )}
    </div>
  );
}
```

## Focus Management

### Trap Focus in Modal

```tsx
function Modal({ isOpen, onClose, children }) {
  const modalRef = useRef(null);

  useEffect(() => {
    if (!isOpen) return;

    const focusableElements = modalRef.current.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    
    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];

    function handleTab(e) {
      if (e.key !== 'Tab') return;
      
      if (e.shiftKey) {
        if (document.activeElement === firstElement) {
          e.preventDefault();
          lastElement.focus();
        }
      } else {
        if (document.activeElement === lastElement) {
          e.preventDefault();
          firstElement.focus();
        }
      }
    }

    document.addEventListener('keydown', handleTab);
    firstElement?.focus();

    return () => document.removeEventListener('keydown', handleTab);
  }, [isOpen]);

  if (!isOpen) return null;

  return (
    <div 
      ref={modalRef}
      role="dialog"
      aria-modal="true"
      // ... styles
    >
      {children}
    </div>
  );
}
```

### Restore Focus After Modal

```tsx
function Modal({ isOpen, onClose }) {
  const buttonRef = useRef(null);

  useEffect(() => {
    const previouslyFocused = document.activeElement;
    
    return () => {
      previouslyFocused?.focus();
    };
  }, [isOpen]);

  return (
    <dialog open={isOpen}>
      {/* Modal content */}
    </dialog>
  );
}
```

## Testing Accessibility

### Automated Testing

```bash
# axe CLI
npx axe https://example.com

# Lighthouse
# Run in Chrome DevTools > Lighthouse > Accessibility
```

### Manual Checklist

- [ ] All images have alt text
- [ ] Form inputs have labels
- [ ] Color is not the only indicator
- [ ] Focus states are visible
- [ ] Page has proper heading hierarchy
- [ ] Links have descriptive text
- [ ] Error messages are associated with inputs
- [ ] Modal traps focus
- [ ] Skip link exists and works

### Screen Reader Testing

1. Navigate with VoiceOver (Mac) or NVDA (Windows)
2. Check reading order makes sense
3. Verify all interactive elements are announced
4. Test form error announcements
