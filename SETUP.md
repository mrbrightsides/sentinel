# RANTAI Sentinel - Setup Guide

> **Live Demo:** [https://sentinel.elpeef.com/](https://sentinel.elpeef.com/)  
> **Repository:** [https://github.com/mrbrightsides/sentinel](https://github.com/mrbrightsides/sentinel)

Complete setup guide for **RANTAI Sentinel** - from installation to deployment.

---

## Table of Contents

1. [Quick Start](#quick-start)
2. [Prerequisites](#prerequisites)
3. [Installation](#installation)
4. [Environment Configuration](#environment-configuration)
5. [Development Workflow](#development-workflow)
6. [Building for Production](#building-for-production)
7. [Deployment](#deployment)
8. [Troubleshooting](#troubleshooting)
9. [Advanced Configuration](#advanced-configuration)
10. [FAQ](#faq)

---

## Quick Start

**Get up and running in 3 minutes:**

```bash
# 1. Clone the repository
git clone https://github.com/mrbrightsides/sentinel.git
cd sentinel

# 2. Install dependencies
npm install

# 3. Run development server
npm run dev

# 4. Open in browser
# Navigate to http://localhost:3000
```

**That's it!** The app runs with mock data by default (no API keys required for basic functionality).

---

## Prerequisites

### Required Software

| Software | Version | Download |
|----------|---------|----------|
| **Node.js** | ≥ 18.0.0 | [nodejs.org](https://nodejs.org/) |
| **npm** | ≥ 9.0.0 | Included with Node.js |
| **Git** | ≥ 2.30.0 | [git-scm.com](https://git-scm.com/) |

**Check your versions:**

```bash
node --version   # Should show v18.0.0 or higher
npm --version    # Should show 9.0.0 or higher
git --version    # Should show 2.30.0 or higher
```

### Recommended Software

| Software | Purpose |
|----------|---------|
| **VS Code** | Code editor with excellent TypeScript support |
| **pnpm** | Faster alternative to npm (optional) |
| **Chrome DevTools** | Browser debugging and testing |

### System Requirements

**Minimum:**
- **OS**: Windows 10, macOS 10.15, Ubuntu 20.04 (or equivalent)
- **RAM**: 4GB
- **Disk**: 500MB free space
- **CPU**: Dual-core processor

**Recommended:**
- **RAM**: 8GB+
- **Disk**: 1GB+ free space
- **CPU**: Quad-core processor

---

## Installation

### 1. Clone the Repository

**Option A: HTTPS (Recommended for most users)**
```bash
git clone https://github.com/mrbrightsides/sentinel.git
cd sentinel
```

**Option B: SSH (If you have SSH keys configured)**
```bash
git clone git@github.com:mrbrightsides/sentinel.git
cd sentinel
```

**Option C: GitHub CLI**
```bash
gh repo clone mrbrightsides/sentinel
cd sentinel
```

**Option D: Download ZIP**
1. Visit [https://github.com/mrbrightsides/sentinel](https://github.com/mrbrightsides/sentinel)
2. Click **Code** → **Download ZIP**
3. Extract and open terminal in the extracted folder

### 2. Install Dependencies

**Using npm (default):**
```bash
npm install
```

**Using pnpm (faster):**
```bash
# Install pnpm globally (if not already installed)
npm install -g pnpm

# Install dependencies
pnpm install
```

**Using yarn:**
```bash
yarn install
```

**Expected output:**
```
added 432 packages, and audited 433 packages in 45s

152 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

### 3. Verify Installation

```bash
# Check if node_modules exists
ls node_modules

# Verify Next.js is installed
npm list next
# Should show: next@15.3.8
```

---

## Environment Configuration

### Basic Setup (No API Keys Required)

The app works with **mock data** by default. No configuration needed for development!

### Advanced Setup (With AI Features)

To enable **real AI predictions, insights, and analysis**, you'll need API keys.

#### Step 1: Create Environment File

```bash
# Create .env.local file in project root
touch .env.local
```

#### Step 2: Add API Keys

**Open `.env.local` and add:**

```bash
# OpenAI GPT-4 (Required for AI features)
OPENAI_API_KEY=sk-proj-YOUR_KEY_HERE

# Anthropic Claude (Optional - Future use)
ANTHROPIC_API_KEY=sk-ant-YOUR_KEY_HERE

# Google Gemini (Optional - Future use)
GOOGLE_AI_API_KEY=YOUR_KEY_HERE

# App Configuration (Optional)
NEXT_PUBLIC_APP_NAME=RANTAI Sentinel
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

#### Step 3: Get API Keys

##### OpenAI GPT-4

1. **Sign up**: [platform.openai.com/signup](https://platform.openai.com/signup)
2. **Add payment**: [platform.openai.com/account/billing](https://platform.openai.com/account/billing)
   - Credit card required
   - Minimum: $5
3. **Create API key**: [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
   - Click **Create new secret key**
   - Name it "RANTAI Sentinel"
   - Copy the key (starts with `sk-proj-...`)
   - **Important**: Save it immediately (shown only once)
4. **Add to `.env.local`**:
   ```bash
   OPENAI_API_KEY=sk-proj-T_XVinLM25AeOgG3vpHg...
   ```

**Cost estimate:** ~$0.01-0.03 per AI request (~$3-5/month for typical usage)

##### Anthropic Claude (Future)

1. **Sign up**: [console.anthropic.com](https://console.anthropic.com)
2. **Create API key**: Console → API Keys
3. **Add to `.env.local`**:
   ```bash
   ANTHROPIC_API_KEY=sk-ant-api03-jmUH...
   ```

##### Google Gemini (Future)

1. **Get key**: [ai.google.dev](https://ai.google.dev)
2. **Add to `.env.local`**:
   ```bash
   GOOGLE_AI_API_KEY=AIzaSyAE...
   ```

#### Step 4: Update API Routes (Current Prototype Method)

**For the current version, hardcode keys in API routes:**

```typescript
// src/app/api/ai/predictions/route.ts
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: 'sk-proj-YOUR_KEY_HERE' // Paste your key here
});
```

**Repeat for all AI API routes:**
- `src/app/api/ai/predictions/route.ts`
- `src/app/api/ai/alerts/route.ts`
- `src/app/api/ai/blockchain/route.ts`
- `src/app/api/ai/reports/route.ts`
- `src/app/api/ai/tasks/route.ts`

**Production Recommendation:**
```typescript
// Use environment variables (more secure)
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});
```

### Environment Variables Reference

| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| `OPENAI_API_KEY` | No* | OpenAI API key for AI features | `sk-proj-...` |
| `ANTHROPIC_API_KEY` | No | Anthropic Claude API key (future) | `sk-ant-...` |
| `GOOGLE_AI_API_KEY` | No | Google Gemini API key (future) | `AIzaSy...` |
| `NEXT_PUBLIC_APP_NAME` | No | Application name | `RANTAI Sentinel` |
| `NEXT_PUBLIC_APP_URL` | No | App URL for metadata | `https://sentinel.elpeef.com` |

*Required only if you want real AI predictions (app works with mock data by default)

---

## Development Workflow

### Start Development Server

```bash
npm run dev
```

**Expected output:**
```
  ▲ Next.js 15.3.8
  - Local:        http://localhost:3000
  - Network:      http://192.168.1.100:3000

 ✓ Ready in 2.3s
```

**Access the app:**
- **Local**: [http://localhost:3000](http://localhost:3000)
- **Network** (mobile testing): `http://YOUR_LOCAL_IP:3000`

### Development Scripts

| Command | Purpose | Usage |
|---------|---------|-------|
| `npm run dev` | Start development server | Hot reload, fast refresh |
| `npm run build` | Build for production | Creates optimized build |
| `npm run start` | Run production build | Must run `build` first |
| `npm run lint` | Run ESLint | Check code quality |
| `npm run type-check` | Run TypeScript check | Verify type safety |

### Development Features

#### Hot Reload

**File changes automatically reload the browser:**
- ✅ Component changes → instant update
- ✅ Style changes → instant update
- ✅ API route changes → server restart (2-3 seconds)

#### Fast Refresh

**React state persists during updates:**
```typescript
// Edit this component while app is running
export function Counter() {
  const [count, setCount] = useState(0); // State preserved!
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

#### Error Overlay

**Errors shown directly in browser:**
- TypeScript errors
- Runtime errors
- Build errors
- ESLint warnings

### Project Structure

```
sentinel/
├── public/                    # Static assets
│   ├── logo.png
│   └── favicon.ico
│
├── src/                       # Source code
│   ├── app/                   # Next.js App Router
│   │   ├── layout.tsx         # Root layout
│   │   ├── page.tsx           # Main dashboard
│   │   ├── globals.css        # Global styles
│   │   └── api/               # API routes
│   │       └── ai/
│   │           ├── predictions/route.ts
│   │           ├── alerts/route.ts
│   │           └── ...
│   │
│   ├── components/            # React components
│   │   ├── ui/                # Base UI components
│   │   ├── dashboard/
│   │   ├── esg/
│   │   ├── security/
│   │   ├── ai/
│   │   └── reports/
│   │
│   ├── lib/                   # Utilities
│   │   └── utils.ts
│   │
│   └── types/                 # TypeScript types
│       └── esg.ts
│
├── .env.local                 # Environment variables (git-ignored)
├── .gitignore                 # Git ignore rules
├── next.config.ts             # Next.js configuration
├── tailwind.config.ts         # Tailwind CSS config
├── tsconfig.json              # TypeScript config
├── package.json               # Dependencies & scripts
├── README.md                  # Project overview
├── ARCHITECTURE.md            # System architecture
├── CONTRIBUTING.md            # Contribution guide
├── THIRD_PARTY_API.md         # API integration guide
└── SETUP.md                   # This file
```

### Making Changes

#### 1. **Edit Components**

```bash
# Open a component
code src/components/dashboard/ESGMetricsPanel.tsx

# Make changes
# Save file (Cmd+S / Ctrl+S)
# Browser auto-refreshes with changes
```

#### 2. **Add New Component**

```bash
# Create new component file
touch src/components/dashboard/NewPanel.tsx
```

```typescript
// src/components/dashboard/NewPanel.tsx
'use client';

import { Card } from '@/components/ui/card';

interface NewPanelProps {
  title: string;
}

export function NewPanel({ title }: NewPanelProps) {
  return (
    <Card>
      <h2>{title}</h2>
    </Card>
  );
}
```

```typescript
// Use in page
import { NewPanel } from '@/components/dashboard/NewPanel';

<NewPanel title="My New Panel" />
```

#### 3. **Modify Styles**

```typescript
// Edit Tailwind classes
<div className="bg-blue-500 text-white p-4 rounded-lg">
  Hello World
</div>

// Dark mode support
<div className="bg-white dark:bg-gray-900 text-black dark:text-white">
  Auto dark mode!
</div>
```

#### 4. **Add API Route**

```bash
# Create new API route
mkdir -p src/app/api/custom
touch src/app/api/custom/route.ts
```

```typescript
// src/app/api/custom/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function GET(request: NextRequest) {
  return NextResponse.json({ message: 'Hello from API!' });
}

export async function POST(request: NextRequest) {
  const body = await request.json();
  return NextResponse.json({ received: body });
}
```

**Test API:**
```bash
# GET request
curl http://localhost:3000/api/custom

# POST request
curl -X POST http://localhost:3000/api/custom \
  -H "Content-Type: application/json" \
  -d '{"test": "data"}'
```

### Testing Your Changes

#### Browser Testing

**Desktop:**
1. Chrome: `Cmd+Option+I` (Mac) / `F12` (Windows)
2. Firefox: `Cmd+Option+K` (Mac) / `F12` (Windows)
3. Safari: Enable **Develop** menu → **Show Web Inspector**

**Mobile (Chrome DevTools):**
1. Open DevTools (`F12`)
2. Click **Toggle Device Toolbar** (phone icon)
3. Select device (iPhone 14 Pro, Pixel 7, etc.)
4. Test responsive behavior

**Real Mobile Device:**
```bash
# Find your local IP
# Mac/Linux:
ifconfig | grep "inet "

# Windows:
ipconfig

# Use this URL on mobile:
http://YOUR_LOCAL_IP:3000
```

#### TypeScript Type Checking

```bash
# Check for type errors
npm run type-check

# Watch mode (continuous checking)
npx tsc --noEmit --watch
```

#### Linting

```bash
# Run ESLint
npm run lint

# Auto-fix issues
npm run lint -- --fix
```

---

## Building for Production

### Production Build

```bash
# Create optimized production build
npm run build
```

**Build process:**
1. **Type checking** - Validates TypeScript
2. **Linting** - Checks code quality
3. **Compilation** - Converts TypeScript to JavaScript
4. **Optimization** - Minifies code, optimizes images
5. **Static generation** - Pre-renders pages when possible

**Expected output:**
```
Route (app)                              Size     First Load JS
┌ ○ /                                    142 kB         284 kB
├ ○ /_not-found                          142 B          142 B
└ ƒ /api/ai/predictions                  0 B                0 B

○  (Static)   prerendered as static HTML
ƒ  (Dynamic)  server-rendered on demand

✓ Compiled successfully
```

### Test Production Build Locally

```bash
# Run production server
npm run start

# Access at http://localhost:3000
```

**Differences from dev mode:**
- No hot reload
- Optimized JavaScript bundles
- Production-level error messages
- Faster page loads

### Build Optimization

**Next.js automatically optimizes:**
- ✅ **Code splitting** - Only loads needed JavaScript
- ✅ **Tree shaking** - Removes unused code
- ✅ **Minification** - Compresses JavaScript/CSS
- ✅ **Image optimization** - Converts to WebP/AVIF
- ✅ **Font optimization** - Inlines critical fonts
- ✅ **CSS optimization** - Purges unused Tailwind classes

**Bundle size analysis:**
```bash
# Install bundle analyzer
npm install --save-dev @next/bundle-analyzer

# Analyze bundle
ANALYZE=true npm run build
```

---

## Deployment

### Vercel (Recommended)

**Vercel is the easiest deployment option (creators of Next.js).**

#### Option 1: GitHub Integration (Automatic)

1. **Push to GitHub:**
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Connect to Vercel:**
   - Visit [vercel.com](https://vercel.com)
   - Click **Add New Project**
   - Import your GitHub repository
   - Click **Deploy**

3. **Configure environment variables:**
   - In Vercel dashboard → **Settings** → **Environment Variables**
   - Add:
     ```
     OPENAI_API_KEY = sk-proj-YOUR_KEY
     ```

4. **Done!** Vercel auto-deploys on every push to `main`

**Your app is live at:** `https://your-project.vercel.app`

#### Option 2: Vercel CLI (Manual)

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy
vercel

# Production deployment
vercel --prod
```

### Netlify

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login
netlify login

# Deploy
netlify deploy --prod
```

**Configure:**
- Build command: `npm run build`
- Publish directory: `.next`

### Custom Server (VPS, Cloud)

**Requirements:**
- Node.js 18+
- PM2 (process manager)
- Nginx (reverse proxy)

```bash
# Install PM2
npm install -g pm2

# Build app
npm run build

# Start with PM2
pm2 start npm --name "sentinel" -- start

# Configure Nginx
# /etc/nginx/sites-available/sentinel
server {
    listen 80;
    server_name sentinel.elpeef.com;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

# Enable site
sudo ln -s /etc/nginx/sites-available/sentinel /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# SSL with Let's Encrypt
sudo certbot --nginx -d sentinel.elpeef.com
```

### Docker

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

```bash
# Build image
docker build -t sentinel .

# Run container
docker run -p 3000:3000 -e OPENAI_API_KEY=sk-proj-... sentinel
```

### Environment Variables (Production)

**Add in deployment platform:**

| Platform | How to Add |
|----------|------------|
| **Vercel** | Dashboard → Settings → Environment Variables |
| **Netlify** | Site settings → Build & deploy → Environment |
| **Docker** | `-e KEY=value` or `.env` file |
| **VPS** | Create `.env.local` on server |

---

## Troubleshooting

### Common Issues

#### 1. **"Cannot find module 'next'"**

**Cause:** Dependencies not installed

**Fix:**
```bash
rm -rf node_modules package-lock.json
npm install
```

#### 2. **Port 3000 already in use**

**Cause:** Another process using port 3000

**Fix:**
```bash
# Find process using port 3000
# Mac/Linux:
lsof -ti:3000 | xargs kill -9

# Windows:
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# Or use different port:
PORT=3001 npm run dev
```

#### 3. **TypeScript errors**

**Cause:** Type mismatches

**Fix:**
```bash
# Check specific errors
npm run type-check

# Clear cache
rm -rf .next
npm run dev
```

#### 4. **Build fails**

**Cause:** Various (check error message)

**Common fixes:**
```bash
# Clear Next.js cache
rm -rf .next

# Clear node_modules
rm -rf node_modules package-lock.json
npm install

# Check for TypeScript errors
npm run type-check

# Check for ESLint errors
npm run lint
```

#### 5. **AI features not working**

**Cause:** Missing or invalid API key

**Check:**
1. API key in `.env.local` or hardcoded in routes
2. Key starts with `sk-proj-` (OpenAI)
3. Billing enabled on OpenAI account
4. No typos in key

**Test API key:**
```bash
curl https://api.openai.com/v1/models \
  -H "Authorization: Bearer sk-proj-YOUR_KEY"
```

#### 6. **Slow build times**

**Fix:**
```bash
# Use pnpm (faster than npm)
npm install -g pnpm
pnpm install

# Clear cache
rm -rf .next node_modules/.cache

# Disable telemetry
npx next telemetry disable
```

#### 7. **"Hydration mismatch" error**

**Cause:** Server HTML doesn't match client rendering

**Fix:**
```typescript
// Use 'use client' for components with browser-only code
'use client';

import { useEffect, useState } from 'react';

export function ClientOnly() {
  const [mounted, setMounted] = useState(false);
  
  useEffect(() => {
    setMounted(true);
  }, []);
  
  if (!mounted) return null;
  
  return <div>Client-only content</div>;
}
```

### Getting Help

**Before asking for help:**
1. Check this guide
2. Search [GitHub Issues](https://github.com/mrbrightsides/sentinel/issues)
3. Check [Next.js docs](https://nextjs.org/docs)
4. Review error messages carefully

**Where to get help:**
- 📧 Email: [support@elpeef.com](mailto:support@elpeef.com)
- 💬 Telegram: [@khudriakhmad](https://t.me/khudriakhmad)
- 🎮 Discord: @khudri_61362
- 🐙 GitHub Issues: [Create new issue](https://github.com/mrbrightsides/sentinel/issues/new)

**Include in your help request:**
- Error message (full text)
- Steps to reproduce
- Environment (OS, Node version, npm version)
- What you've already tried

---

## Advanced Configuration

### Custom Port

```bash
# .env.local
PORT=3001
```

```bash
# Or command line
PORT=3001 npm run dev
```

### TypeScript Configuration

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "preserve",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

### Next.js Configuration

```typescript
// next.config.ts
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  reactStrictMode: true,
  
  // Image optimization
  images: {
    domains: ['example.com'], // External image domains
    formats: ['image/webp', 'image/avif']
  },
  
  // Environment variables
  env: {
    CUSTOM_KEY: 'value'
  },
  
  // Redirects
  async redirects() {
    return [
      {
        source: '/old-path',
        destination: '/new-path',
        permanent: true
      }
    ];
  }
};

export default nextConfig;
```

### Tailwind Configuration

```typescript
// tailwind.config.ts
import type { Config } from 'tailwindcss';

const config: Config = {
  darkMode: 'class',
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}'
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          50: '#f0f9ff',
          100: '#e0f2fe',
          // ... custom colors
        }
      }
    }
  },
  plugins: []
};

export default config;
```

### VS Code Configuration

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "tailwindCSS.experimental.classRegex": [
    ["cn\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"]
  ]
}
```

---

## FAQ

### General

**Q: Do I need an OpenAI API key to run the app?**  
A: No! The app works with mock data by default. API keys only needed for real AI predictions.

**Q: How much does OpenAI API cost?**  
A: ~$0.01-0.03 per AI request. Typical usage: ~$3-5/month.

**Q: Can I use Claude or Gemini instead of OpenAI?**  
A: Currently OpenAI only. Claude and Gemini support planned for Q2 2025.

**Q: Is this free to use?**  
A: Yes! MIT license - use it for anything.

### Development

**Q: Which editor should I use?**  
A: VS Code recommended (best TypeScript support), but any editor works.

**Q: Can I use JavaScript instead of TypeScript?**  
A: Not recommended. The codebase is 100% TypeScript for type safety.

**Q: How do I add new pages?**  
A: Create files in `src/app/` folder. Next.js automatically creates routes.

**Q: Can I use a different CSS framework?**  
A: Yes, but Tailwind is integrated deeply. Switching would require significant changes.

### Deployment

**Q: Where should I deploy?**  
A: Vercel (easiest), Netlify, or any Node.js hosting.

**Q: How do I get a custom domain?**  
A: Buy domain → Add DNS records → Configure in Vercel/Netlify settings.

**Q: Can I deploy to shared hosting?**  
A: No. Requires Node.js support (VPS, cloud, or platforms like Vercel).

**Q: Is HTTPS automatic?**  
A: Yes on Vercel/Netlify. On VPS, use Let's Encrypt (free).

### Data & Privacy

**Q: Where is data stored?**  
A: Browser localStorage (client-side only). No server database yet.

**Q: Is my data secure?**  
A: Data never leaves your browser (unless using AI features). API keys should be in environment variables.

**Q: Can multiple users use the same installation?**  
A: Not yet. Each browser has separate localStorage. Multi-user support planned for Q2 2025.

---

## Next Steps

**You're all set! 🎉**

### Learn More

- 📖 [README.md](./README.md) - Project overview
- 🏗️ [ARCHITECTURE.md](./ARCHITECTURE.md) - System architecture
- 🤝 [CONTRIBUTING.md](./CONTRIBUTING.md) - Contribution guide
- 🔌 [THIRD_PARTY_API.md](./THIRD_PARTY_API.md) - API integration

### Get Involved

- ⭐ Star the repo on GitHub
- 🐛 Report bugs in Issues
- 💡 Suggest features in Discussions
- 🔀 Submit pull requests

### Stay Updated

- 📧 Email: [support@elpeef.com](mailto:support@elpeef.com)
- 💬 Telegram: [@khudriakhmad](https://t.me/khudriakhmad)
- 🎮 Discord: @khudri_61362

**Happy Building! 🚀**

---

**Last Updated**: January 2025  
**Version**: 2.0.0  
**Maintained by**: RANTAI Sentinel Team
