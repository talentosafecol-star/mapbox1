# UI Components Guide

## Button

### Variants

```tsx
type ButtonVariant = 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';
type ButtonSize = 'sm' | 'md' | 'lg';
```

### Implementation

```tsx
interface ButtonProps {
  variant?: ButtonVariant;
  size?: ButtonSize;
  loading?: boolean;
  disabled?: boolean;
  icon?: ReactNode;
  children: ReactNode;
}

export function Button({
  variant = 'primary',
  size = 'md',
  loading = false,
  disabled = false,
  icon,
  children,
  ...props
}: ButtonProps) {
  const classes = [
    'btn',
    `btn-${variant}`,
    `btn-${size}`,
    loading && 'btn-loading'
  ].filter(Boolean).join(' ');

  return (
    <button className={classes} disabled={disabled || loading} {...props}>
      {loading ? <Spinner /> : icon}
      {children}
    </button>
  );
}
```

### States

- Default
- Hover
- Focus (accessibility)
- Active
- Disabled

## Input

### Types

```tsx
type InputType = 'text' | 'email' | 'password' | 'number' | 'tel' | 'search' | 'url';
```

### Implementation

```tsx
interface InputProps {
  label: string;
  type?: InputType;
  placeholder?: string;
  error?: string;
  hint?: string;
  required?: boolean;
  disabled?: boolean;
}

export function Input({
  label,
  type = 'text',
  placeholder,
  error,
  hint,
  required,
  disabled,
  id = useId(),
  ...props
}: InputProps) {
  return (
    <div className="input-group">
      <label htmlFor={id}>
        {label}
        {required && <span aria-hidden="true">*</span>}
      </label>
      <input
        id={id}
        type={type}
        placeholder={placeholder}
        disabled={disabled}
        aria-invalid={!!error}
        aria-describedby={hint ? `${id}-hint` : undefined}
        {...props}
      />
      {hint && <span id={`${id}-hint`}>{hint}</span>}
      {error && <span role="alert">{error}</span>}
    </div>
  );
}
```

## Select

### Implementation

```tsx
interface SelectOption {
  value: string;
  label: string;
  disabled?: boolean;
}

interface SelectProps {
  label: string;
  options: SelectOption[];
  placeholder?: string;
  error?: string;
}

export function Select({ label, options, placeholder, error, id = useId() }: SelectProps) {
  return (
    <div className="select-group">
      <label htmlFor={id}>{label}</label>
      <select id={id} aria-invalid={!!error}>
        {placeholder && <option value="">{placeholder}</option>}
        {options.map(opt => (
          <option key={opt.value} value={opt.value} disabled={opt.disabled}>
            {opt.label}
          </option>
        ))}
      </select>
      {error && <span role="alert">{error}</span>}
    </div>
  );
}
```

## Modal

### Implementation

```tsx
interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title: string;
  children: ReactNode;
  footer?: ReactNode;
}

export function Modal({ isOpen, onClose, title, children, footer }: ModalProps) {
  useEffect(() => {
    if (isOpen) {
      document.body.style.overflow = 'hidden';
      // Trap focus
    }
    return () => {
      document.body.style.overflow = '';
    };
  }, [isOpen]);

  if (!isOpen) return null;

  return (
    <div className="modal-overlay" onClick={onClose}>
      <div 
        className="modal" 
        role="dialog" 
        aria-modal="true"
        aria-labelledby="modal-title"
        onClick={e => e.stopPropagation()}
      >
        <header>
          <h2 id="modal-title">{title}</h2>
          <button aria-label="Close" onClick={onClose}>×</button>
        </header>
        <main>{children}</main>
        {footer && <footer>{footer}</footer>}
      </div>
    </div>
  );
}
```

## Card

### Implementation

```tsx
interface CardProps {
  title?: string;
  description?: string;
  image?: string;
  footer?: ReactNode;
  action?: ReactNode;
  variant?: 'default' | 'elevated' | 'outlined';
}

export function Card({ 
  title, 
  description, 
  image, 
  footer, 
  action,
  variant = 'default' 
}: CardProps) {
  return (
    <article className={`card card-${variant}`}>
      {image && <img src={image} alt="" />}
      <div className="card-content">
        {title && <h3>{title}</h3>}
        {description && <p>{description}</p>}
        {action}
      </div>
      {footer && <footer>{footer}</footer>}
    </article>
  );
}
```

## Toast/Notification

### Types

```tsx
type ToastType = 'success' | 'error' | 'warning' | 'info';
```

### Implementation

```tsx
interface ToastProps {
  type: ToastType;
  message: string;
  onClose: () => void;
  duration?: number;
}

export function Toast({ type, message, onClose, duration = 5000 }: ToastProps) {
  useEffect(() => {
    const timer = setTimeout(onClose, duration);
    return () => clearTimeout(timer);
  }, [duration, onClose]);

  return (
    <div className={`toast toast-${type}`} role="alert">
      <Icon name={type} />
      <span>{message}</span>
      <button aria-label="Close" onClick={onClose}>×</button>
    </div>
  );
}
```

## Badge

### Variants

```tsx
type BadgeVariant = 'default' | 'success' | 'warning' | 'error' | 'info';
type BadgeSize = 'sm' | 'md';
```

### Implementation

```tsx
interface BadgeProps {
  variant?: BadgeVariant;
  size?: BadgeSize;
  children: ReactNode;
}

export function Badge({ variant = 'default', size = 'md', children }: BadgeProps) {
  return (
    <span className={`badge badge-${variant} badge-${size}`}>
      {children}
    </span>
  );
}
```

## Dropdown Menu

### Implementation

```tsx
interface MenuItem {
  label: string;
  icon?: ReactNode;
  onClick: () => void;
  disabled?: boolean;
  danger?: boolean;
}

interface DropdownProps {
  trigger: ReactNode;
  items: MenuItem[];
}

export function Dropdown({ trigger, items }: DropdownProps) {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className="dropdown">
      <button onClick={() => setIsOpen(!isOpen)}>
        {trigger}
      </button>
      {isOpen && (
        <menu>
          {items.map((item, i) => (
            <button
              key={i}
              onClick={item.onClick}
              disabled={item.disabled}
              className={item.danger ? 'danger' : ''}
            >
              {item.icon}
              {item.label}
            </button>
          ))}
        </menu>
      )}
    </div>
  );
}
```

## Tabs

### Implementation

```tsx
interface Tab {
  id: string;
  label: string;
  content: ReactNode;
}

interface TabsProps {
  tabs: Tab[];
  defaultTab?: string;
}

export function Tabs({ tabs, defaultTab }: TabsProps) {
  const [activeTab, setActiveTab] = useState(defaultTab || tabs[0].id);

  return (
    <div className="tabs">
      <div role="tablist">
        {tabs.map(tab => (
          <button
            key={tab.id}
            role="tab"
            aria-selected={activeTab === tab.id}
            onClick={() => setActiveTab(tab.id)}
          >
            {tab.label}
          </button>
        ))}
      </div>
      <div role="tabpanel">
        {tabs.find(t => t.id === activeTab)?.content}
      </div>
    </div>
  );
}
```
