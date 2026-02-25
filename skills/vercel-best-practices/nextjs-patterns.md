# Next.js Patterns Guide

## App Router

### File-Based Routing

```
app/
├── layout.tsx        # Root layout
├── page.tsx          # Home page (/)
├── about/
│   └── page.tsx      # (/about)
├── blog/
│   ├── page.tsx      # (/blog)
│   └── [slug]/
│       └── page.tsx  # (/blog/:slug)
└── api/
    └── route.ts      # API handler
```

### Layouts

```tsx
// Root layout - applies to all pages
export default function RootLayout({ children }) {
  return (
    <html>
      <body>{children}</body>
    </html>
  );
}

// Nested layout
export default function DashboardLayout({ children }) {
  return (
    <div className="dashboard">
      <Sidebar />
      {children}
    </div>
  );
}
```

## Server Components

### Data Fetching

```tsx
async function getData() {
  const res = await fetch('https://api.example.com/data', {
    next: { revalidate: 60 } // ISR: Revalidate every 60s
  });
  if (!res.ok) throw new Error('Failed to fetch');
  return res.json();
}

export default async function Page() {
  const data = await getData();
  return <div>{data.title}</div>;
}
```

### Parallel Data Loading

```tsx
async function getUser() {
  const res = await fetch('/api/user');
  return res.json();
}

async function getPosts() {
  const res = await fetch('/api/posts');
  return res.json();
}

export default async function Page() {
  const [user, posts] = await Promise.all([getUser(), getPosts()]);
  return <div>{/* Display data */}</div>;
}
```

### Streaming with Suspense

```tsx
import { Suspense } from 'react';

export default function Page() {
  return (
    <main>
      <Suspense fallback={<Loading />}>
        <Comments />
      </Suspense>
      <Suspense fallback={<Loading />}>
        <Recommendations />
      </Suspense>
    </main>
  );
}

async function Comments() {
  const comments = await fetchComments();
  return <div>{comments.map(c => <Comment key={c.id} {...c} />)}</div>;
}
```

## Client Components

### When to Use

- Event handlers (onClick, onChange)
- useState, useEffect
- Browser APIs
- Custom hooks with state

```tsx
'use client';

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  );
}
```

### Passing Server Data to Client

```tsx
// Server Component
import ClientComponent from './ClientComponent';

export default function ServerComponent() {
  const data = getData(); // async
  
  return <ClientComponent initialData={data} />;
}

// Client Component
'use client';
export default function ClientComponent({ initialData }) {
  // Use initialData as starting point
}
```

## Server Actions

### Defining Actions

```tsx
'use server';

export async function createPost(formData: FormData) {
  const title = formData.get('title');
  
  // Validate
  if (!title) return { error: 'Title required' };
  
  // Database operation
  await db.post.create({ title });
  
  // Revalidate
  revalidatePath('/posts');
  
  return { success: true };
}
```

### Using in Components

```tsx
'use client';

import { createPost } from '@/actions';

export function CreatePost() {
  return (
    <form action={createPost}>
      <input name="title" />
      <button type="submit">Create</button>
    </form>
  );
}
```

## Dynamic Routes

### generateStaticParams

```tsx
export async function generateStaticParams() {
  const posts = await getPosts();
  return posts.map(post => ({ slug: post.slug }));
}

export default async function PostPage({ params }) {
  const post = await getPost(params.slug);
  return <article>{post.content}</article>;
}
```

### Dynamic Metadata

```tsx
export async function generateMetadata({ params }) {
  const post = await getPost(params.slug);
  return {
    title: post.title,
    description: post.excerpt
  };
}
```

## Error Handling

### error.tsx

```tsx
'use client';

export default function Error({ error, reset }) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}
```

### not-found.tsx

```tsx
import Link from 'next/link';

export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <Link href="/">Return Home</Link>
    </div>
  );
}
```

## Loading States

### loading.tsx

```tsx
export default function Loading() {
  return <div>Loading...</div>;
}
```

## Middleware

### matcher

```javascript
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  // Check auth
  const token = request.cookies.get('token');
  
  if (!token && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url));
  }
  
  return NextResponse.next();
}

export const config = {
  matcher: ['/dashboard/:path*', '/api/:path*']
};
```
