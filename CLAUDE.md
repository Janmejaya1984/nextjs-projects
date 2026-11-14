# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Linkbase** is a Next.js 16.4.6 application with React 19, TypeScript, and Tailwind CSS v4. It uses the App Router with server components as the default.

**Important**: This version of Next.js includes breaking changes from earlier versions. Before writing code, consult `node_modules/next/dist/docs/` for the latest API documentation.

## Development Commands

```bash
npm run dev         # Start development server (http://localhost:3000)
npm run build       # Build for production
npm start           # Start production server
npm run lint        # Run ESLint with Next.js config
```

## Architecture & Structure

### App Router (App Directory)
- **Location**: `app/` directory
- **Default**: Server components (React Server Components - RSC)
- **Mark client components**: Use `"use client"` directive at the top of files
- **Root layout**: `app/layout.tsx` wraps all pages with Metadata, fonts, globals.css
- **Pages**: Route structure matches file paths (e.g., `app/page.tsx` → `/`, `app/about/page.tsx` → `/about`)

### Key Files
- `app/layout.tsx` - Root layout with Geist fonts from next/font, Tailwind classes (h-full, min-h-full flex flex-col), and globals.css import
- `app/page.tsx` - Home page (currently default template)
- `app/globals.css` - Global styles and Tailwind directives
- `next.config.ts` - TypeScript config with cacheComponents and partialPrefetching enabled, Tailwind CSS Turbopack loader

### Configuration

**TypeScript** (`tsconfig.json`):
- Strict mode enabled
- Target: ES2017
- Path alias: `@/*` maps to `./*` (use for imports: `import { Component } from "@/components/Component"`)
- Includes `.next/types` and `.next/dev/types` for Next.js auto-generated types

**Styling**:
- Tailwind CSS v4.3.3 with Turbopack loader
- No `tailwind.config.ts` needed (v4 uses minimal config by default)
- Globals imported in `app/layout.tsx`
- Dark mode supported (use `dark:` prefix)

**ESLint** (`eslint.config.mjs`):
- Uses ESLint flat config format (v9+)
- Extends `eslint-config-next/core-web-vitals` and `/typescript`
- Ignores: `.next/`, `out/`, `build/`, `next-env.d.ts`
- Run with `npm run lint`

## Next.js 16 Key Features & Changes

### Server Components (Default)
- Pages and components are server components by default
- Use `"use client"` at the top of files that need interactivity (useState, useEffect, event handlers)
- Server components cannot use hooks or browser APIs

### Metadata API
- Define page metadata in `layout.tsx` and `page.tsx` using `export const metadata`
- Example: `export const metadata: Metadata = { title: "...", description: "..." }`

### Image Optimization
- Use `next/image` for optimized images
- Requires `width`, `height`, and `src` props
- Supports `priority` prop for above-the-fold images

### Font Optimization
- Use `next/font/google` for hosted Google Fonts
- Fonts applied via CSS variables (e.g., `--font-geist-sans`)
- Already configured in root layout with Geist font family

## Common Patterns

### Adding a New Page
1. Create file: `app/my-page/page.tsx`
2. Define default export (server component by default)
3. Route will be `/my-page`

### Making a Client Component
```tsx
"use client";

import { useState } from "react";

export default function MyComponent() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

### Using Path Alias
```tsx
import { MyComponent } from "@/components/MyComponent";  // maps to ./components/MyComponent
```

## Testing & Type Checking

Currently no test framework is configured. Consider adding based on project needs:
- Jest + React Testing Library for component tests
- Cypress or Playwright for e2e tests

Run `npm run lint` to check for ESLint violations before committing.

## Build & Deployment

- `npm run build` compiles the app to `.next/` directory
- Production: use `npm start` to run the optimized build locally
- Deploy: Follow Vercel platform documentation for deployment (default Next.js hosting)
