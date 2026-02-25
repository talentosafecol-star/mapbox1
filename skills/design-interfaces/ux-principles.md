# UX Principles Guide

## Core Principles

### 1. Visibility of System Status

Always keep users informed about what's happening through appropriate feedback.

**Examples:**
- Loading spinners during async operations
- Progress bars for file uploads
- Success/error toast notifications
- Form validation messages

### 2. Match Between System and Real World

Use familiar language, concepts, and symbols that users recognize.

**Guidelines:**
- Use icons that represent real-world objects
- Write in user's native language
- Follow cultural conventions
- Use recognizable patterns (calendar for dates)

### 3. User Control and Freedom

Users often choose system functions by mistake and need clearly marked "emergency exits."

**Examples:**
- Undo/redo functionality
- Cancel buttons
- Back navigation
- Confirmation dialogs for destructive actions

### 4. Consistency and Standards

Users shouldn't wonder whether different words, situations, or actions mean the same thing.

**Types of Consistency:**
- Internal: Within your application
- External: With industry standards
- Platform: Platform conventions (iOS, Android, Web)

### 5. Error Prevention

Even better than good error messages is a careful design which prevents a problem from occurring.

**Strategies:**
- Input constraints
- Confirmation for irreversible actions
- Disabled states for unavailable actions
- Auto-save drafts

### 6. Recognition Rather Than Recall

Minimize the user's memory load by making objects, actions, and options visible.

**Techniques:**
- Visible menus and toolbars
- Recent items/history
- Search functionality
- Breadcrumbs

### 7. Flexibility and Efficiency of Use

Accelerators, unseen by novice users, may often speed up the interaction for expert users.

**Features:**
- Keyboard shortcuts
- Customizable interfaces
- Advanced search filters
- Bulk operations

### 8. Aesthetic and Minimalist Design

Dialogues should not contain information which is irrelevant or rarely needed.

**Principles:**
- Progressive disclosure
- Clear visual hierarchy
- Adequate white space
- Remove visual clutter

### 9. Help Users Recognize, Diagnose, and Recover from Errors

Error messages should be expressed in plain language, precisely indicate the problem, and constructively suggest a solution.

**Error Message Guidelines:**
- Be specific and helpful
- Use human-readable language
- Provide actionable solutions
- Maintain polite tone

### 10. Help and Documentation

Even though it is better if the system can be used without documentation, it may be necessary to provide help.

**Documentation Best Practices:**
- Contextual help
- Searchable documentation
- Tooltips for complex features
- Tutorial/onboarding flows

## Visual Hierarchy

### Size and Scale

```
H1: 2.5rem (40px) - Page titles
H2: 2rem (32px) - Section headers  
H3: 1.5rem (24px) - Subsection headers
H4: 1.25rem (20px) - Card titles
Body: 1rem (16px) - Regular text
Small: 0.875rem (14px) - Captions, labels
```

### Color as Hierarchy

- Primary color: Main actions, highlights
- Secondary color: Supporting elements
- Neutral: Background, containers
- Error/Success: System feedback

### Spacing System

```
--space-1: 4px
--space-2: 8px
--space-3: 12px
--space-4: 16px
--space-6: 24px
--space-8: 32px
--space-12: 48px
--space-16: 64px
```

## Affordance and Signifiers

### Affordance
An attribute of an object that defines its possible uses or makes clear how it can or should be used.

**Examples:**
- Button appears clickable
- Input field appears typeable
- Slider appears draggable

### Signifiers
Indicators that communicate where an action should happen.

**Examples:**
- Underlined text (link)
- Icon + label
- Hover effects
- Cursor changes

## Loading States

### Skeleton Screens

```tsx
function SkeletonCard() {
  return (
    <div className="skeleton-card">
      <div className="skeleton skeleton-image" />
      <div className="skeleton skeleton-title" />
      <div className="skeleton skeleton-text" />
    </div>
  );
}
```

### Progressive Loading

```tsx
<Image 
  src={src}
  placeholder="blur"
  blurDataURL={blurHash}
  loading="lazy"
/>
```

## Empty States

### Components

```tsx
function EmptyState({ title, description, action }) {
  return (
    <div className="empty-state">
      <Icon name="empty" size={48} />
      <h3>{title}</h3>
      <p>{description}</p>
      {action && <Button>{action}</Button>}
    </div>
  );
}
```

### Best Practices

- Be helpful, not just informative
- Provide clear call-to-action
- Use illustrations appropriately
- Keep it simple
