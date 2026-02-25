# Vercel Best Practices - AGENTS.md

## Overview

This document provides comprehensive guidelines for AI agents working with Vercel, Next.js, and deployment configurations.

## Vercel Configuration

### vercel.json

```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/next"
    }
  ],
  "routes": [
    {
      "src": "/api/(.*)",
      "dest": "/api/$1"
    }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "DENY" }
      ]
    }
  ],
  "regions": ["iad1"],
  "functions": {
    "api/**/*.js": {
      "memory": 1024,
      "maxDuration": 30
    }
  }
}
```

### Environment Variables

- Use `vercel env` CLI for management
- Never commit `.env.local` to git
- Use Vercel Secrets for sensitive data

## Next.js Best Practices

### Server vs Client Components

```tsx
// Server Component (default) - for data fetching
async function ServerComponent() {
  const data = await fetchData();
  return <div>{data.title}</div>;
}

// Client Component - for interactivity
'use client';
function ClientComponent() {
  const [state, setState] = useState();
  return <button onClick={() => setState()}>Click</button>;
}
```

### Data Fetching Patterns

```tsx
// Parallel fetching - avoid waterfalls
async function Page() {
  const [user, posts] = await Promise.all([
    fetchUser(),
    fetchPosts()
  ]);
  return <div>{/* content */}</div>;
}

// Streaming with Suspense
import { Suspense } from 'react';

async function Page() {
  return (
    <Suspense fallback={<Skeleton />}>
      <Comments />
    </Suspense>
  );
}
```

### ISR Implementation

```tsx
export const revalidate = 60; // seconds
export async function generateStaticParams() {
  const posts = await fetchPosts();
  return posts.map(post => ({ slug: post.slug }));
}
```

## Performance Guidelines

### Image Optimization

- Use `next/image` for automatic optimization
- Define explicit sizes for responsive images
- Use WebP/AVIF formats (automatic with Next.js)

### Font Optimization

```tsx
import { Inter } from 'next/font/google';
const inter = Inter({ subsets: ['latin'] });
```

### Bundle Size

- Use `@next/bundle-analyzer` to monitor
- Code split with dynamic imports
- Avoid barrel files (index.ts re-exports)

## Deployment Checklist

1. Configure `vercel.json` properly
2. Set up environment variables
3. Configure security headers
4. Set up proper caching headers
5. Test locally with `vercel dev`
6. Use preview deployments for PRs

## Common Issues

### Build Failures
- Check Node.js version compatibility
- Verify dependency versions
- Ensure proper TypeScript configuration

### Runtime Errors
- Check environment variable availability
- Verify API routes configuration
- Review function logs in Vercel dashboard
