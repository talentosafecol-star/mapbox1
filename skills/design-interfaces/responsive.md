# Responsive Design Guide

## Mobile-First Approach

### Base Styles (Mobile)

```css
.container {
  width: 100%;
  padding: 16px;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}
```

### Tablet (min-width: 768px)

```css
@media (min-width: 768px) {
  .container {
    padding: 24px;
  }
  
  .grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}
```

### Desktop (min-width: 1024px)

```css
@media (min-width: 1024px) {
  .container {
    padding: 32px;
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 32px;
  }
}
```

### Large Desktop (min-width: 1440px)

```css
@media (min-width: 1440px) {
  .container {
    max-width: 1400px;
  }
}
```

## Breakpoints

### Common Breakpoints

```scss
$breakpoints: (
  'sm': 640px,
  'md': 768px,
  'lg': 1024px,
  'xl': 1280px,
  '2xl': 1536px
);
```

### CSS Custom Properties

```css
:root {
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
}
```

## Fluid Typography

### Clamp Function

```css
:root {
  --font-size-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
  --font-size-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
  --font-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
  --font-size-lg: clamp(1.125rem, 1rem + 0.625vw, 1.25rem);
  --font-size-xl: clamp(1.25rem, 1rem + 1.25vw, 1.5rem);
  --font-size-2xl: clamp(1.5rem, 1rem + 2.5vw, 2rem);
  --font-size-3xl: clamp(1.875rem, 1rem + 4.375vw, 3rem);
}
```

### Viewport Units

```css
.title {
  font-size: 5vw;
  line-height: 1.1;
  max-font-size: 80px;
  min-font-size: 32px;
}
```

## Container Queries

### Basic Usage

```css
@container (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: auto 1fr;
  }
  
  .card-image {
    aspect-ratio: 1;
  }
}
```

### Component Implementation

```tsx
function Card({ title, image, description }) {
  return (
    <article className="card" style={{ containerType: 'inline-size' }}>
      {image && <img src={image} alt="" />}
      <div className="card-content">
        <h3>{title}</h3>
        <p>{description}</p>
      </div>
    </article>
  );
}
```

## Grid Systems

### 12-Column Grid

```css
.grid-12 {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 16px;
}

.col-12 { grid-column: span 12; }
.col-6  { grid-column: span 6; }
.col-4  { grid-column: span 4; }
.col-3  { grid-column: span 3; }
.col-2  { grid-column: span 2; }

@media (max-width: 768px) {
  .col-6, .col-4, .col-3, .col-2 {
    grid-column: span 12;
  }
}
```

### Flexbox Grid

```css
.row {
  display: flex;
  flex-wrap: wrap;
  margin: -8px;
}

.col {
  flex: 1;
  padding: 8px;
}

.col-auto { flex: 0 0 auto; }
.col-6    { flex: 0 0 50%; }
.col-4    { flex: 0 0 33.333%; }
.col-3    { flex: 0 0 25%; }
```

## Responsive Navigation

### Mobile Menu

```tsx
function Navigation() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <nav className="navbar">
      <Logo />
      
      {/* Desktop Menu */}
      <ul className="nav-links desktop-only">
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</a></li>
      </ul>
      
      {/* Mobile Menu Button */}
      <button 
        className="mobile-menu-btn"
        onClick={() => setIsOpen(!isOpen)}
        aria-label="Toggle menu"
      >
        <Icon name={isOpen ? 'close' : 'menu'} />
      </button>
      
      {/* Mobile Menu */}
      {isOpen && (
        <div className="mobile-menu">
          <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/about">About</a></li>
            <li><a href="/contact">Contact</a></li>
          </ul>
        </div>
      )}
    </nav>
  );
}
```

## Responsive Images

### srcset and sizes

```html
<img 
  src="image-800.jpg"
  srcset="
    image-400.jpg 400w,
    image-800.jpg 800w,
    image-1200.jpg 1200w,
    image-1600.jpg 1600w
  "
  sizes="
    (max-width: 640px) 100vw,
    (max-width: 1024px) 50vw,
    33vw
  "
  alt="Description"
  loading="lazy"
/>
```

### Picture Element

```html
<picture>
  <source 
    media="(min-width: 1024px)" 
    srcset="image-lg.avif"
    type="image/avif"
  />
  <source 
    media="(min-width: 768px)" 
    srcset="image-md.avif"
    type="image/avif"
  />
  <img 
    src="image-sm.jpg"
    alt="Description"
    loading="lazy"
  />
</picture>
```

## Responsive Components

### Responsive Card Grid

```tsx
function CardGrid({ items }) {
  return (
    <div className="card-grid">
      {items.map(item => (
        <Card key={item.id} {...item} />
      ))}
    </div>
  );
}
```

```css
.card-grid {
  display: grid;
  gap: 16px;
  grid-template-columns: 1fr;
}

@media (min-width: 640px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 24px;
  }
}

@media (min-width: 1280px) {
  .card-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

## Testing Responsive Design

### Chrome DevTools

1. Toggle device toolbar: Ctrl+Shift+M (Cmd+Shift+M)
2. Add custom device dimensions
3. Test responsive breakpoints

### Test Checklist

- [ ] Mobile (320px - 480px)
- [ ] Tablet (481px - 768px)
- [ ] Laptop (769px - 1024px)
- [ ] Desktop (1025px+)
- [ ] Touch interactions on mobile
- [ ] Readability at all sizes
- [ ] No horizontal scroll
- [ ] Images scale properly
