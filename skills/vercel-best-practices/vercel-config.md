# Vercel Configuration Guide

## Project Setup

### Required Files

- `package.json` - Project dependencies
- `vercel.json` - Vercel configuration (optional)
- `next.config.js` - Next.js configuration

### vercel.json Structure

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
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-XSS-Protection", "value": "1; mode=block" }
      ]
    }
  ],
  "env": {
    "NEXT_PUBLIC_API_URL": "@api-url"
  },
  "regions": ["iad1", "sfo1"],
  "functions": {
    "api/**/*.ts": {
      "memory": 1024,
      "maxDuration": 30,
      "runtime": "nodejs18.x"
    }
  }
}
```

## Environment Variables

### CLI Commands

```bash
# Add environment variable
vercel env add VARIABLE_NAME production

# List environment variables
vercel env ls

# Pull to local .env
vercel env pull .env.local
```

### Best Practices

- Use `@secret-name` syntax for Vercel Secrets
- Prefix public variables with `NEXT_PUBLIC_`
- Never commit `.env` files to git

## Serverless Functions

### API Routes Structure

```
api/
├── hello.js          # Basic handler
├── users/
│   ├── index.js       # GET /api/users
│   └── [id].js       # GET /api/users/:id
└── posts/
    └── create.js      # POST /api/posts/create
```

### Handler Template

```javascript
export default async function handler(req, res) {
  if (req.method !== 'GET') {
    return res.status(405).json({ error: 'Method not allowed' });
  }
  
  const data = await fetchData();
  return res.status(200).json(data);
}
```

## Edge Functions

### Runtime Configuration

```javascript
export const config = {
  runtime: 'edge',
  regions: ['iad1', 'sfo1']
};

export default function handler(req) {
  return new Response('Hello Edge!');
}
```

## Cron Jobs

### vercel.json Configuration

```json
{
  "crons": [
    {
      "path": "/api/cron/daily",
      "schedule": "0 0 * * *"
    }
  ]
}
```

## Rewrites and Redirects

```json
{
  "rewrites": [
    { "source": "/old-path", "destination": "/new-path" }
  ],
  "redirects": [
    { "source": "/legacy", "destination": "/modern", "statusCode": 301 }
  ]
}
```
