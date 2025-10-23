# AGENTS.md — AI Agent Guidelines for v0-Newsletter-Template

This document provides guidance for AI agents working with the **v0-newsletter-template** repository. It outlines the project structure, best practices, common tasks, and constraints to ensure quality contributions.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Key Technologies & Stack](#key-technologies--stack)
3. [Repository Structure](#repository-structure)
4. [Code Patterns & Conventions](#code-patterns--conventions)
5. [Common Agent Tasks](#common-agent-tasks)
6. [Before Making Changes](#before-making-changes)
7. [File Modification Guidelines](#file-modification-guidelines)
8. [Testing & Validation](#testing--validation)
9. [Environment Setup](#environment-setup)
10. [Critical Files & Areas](#critical-files--areas)
11. [Constraints & Limitations](#constraints--limitations)
12. [Helpful Resources](#helpful-resources)

---

## Project Overview

**v0-newsletter-template** is a production-ready email capture application built with:

- **Framework**: Next.js 15 with App Router
- **Runtime**: React 19
- **Language**: TypeScript
- **Purpose**: Modern, reusable newsletter subscription form template
- **Key Feature**: Email capture with validation, animations, and serverless database storage
- **Hosting**: Vercel with v0.dev integration support

This is a **high-quality, well-maintained codebase** that serves as a reference implementation for modern web development practices.

---

## Key Technologies & Stack

### Frontend

| Technology           | Version | Purpose                                       |
| -------------------- | ------- | --------------------------------------------- |
| React                | 19      | UI components with enhanced hooks             |
| Next.js              | 15.2.4  | Framework with App Router & Server Components |
| Tailwind CSS         | 3.4.17  | Utility-first styling with dark mode          |
| Framer Motion        | Latest  | Animations and transitions                    |
| shadcn/ui + Radix UI | Latest  | Accessible component primitives               |
| React Hook Form      | Latest  | Form state management                         |
| Zod                  | 3.24.1  | Schema validation                             |

### Backend & Data

| Technology             | Purpose                                     |
| ---------------------- | ------------------------------------------- |
| Next.js Server Actions | Backend mutations and logic                 |
| Upstash Redis          | Serverless subscriber email storage         |
| Vercel KV              | Redis integration via environment variables |

### Development

| Tool         | Purpose                       |
| ------------ | ----------------------------- |
| ESLint       | Code quality & consistency    |
| Tailwind CSS | CSS framework & configuration |
| PostCSS      | CSS processing                |

---

## Repository Structure

```
v0-newsletter-template/
├── app/                          # Next.js App Router
│   ├── layout.tsx               # Root layout with fonts, providers, metadata
│   ├── page.tsx                 # Home page (main landing page)
│   └── globals.css              # Global styles & CSS variables
│
├── components/                  # React components
│   ├── ui/                      # shadcn/ui components (40+ built-in)
│   ├── form-newsletter.tsx      # Newsletter form with validation
│   ├── newsletter.tsx           # Main newsletter section with animations
│   ├── background.tsx           # Dynamic background (image/video)
│   ├── footer.tsx               # Footer with social links
│   ├── theme-provider.tsx       # Theme switching infrastructure
│   ├── v0-setup.tsx             # Vercel v0.dev setup helper
│   └── icons/
│       └── x.tsx                # Custom X (Twitter) logo SVG
│
├── lib/                         # Utilities & server logic
│   ├── subscribe.ts             # Server Action: email subscription
│   ├── actions.ts               # Server Actions: environment checks
│   ├── schema.ts                # Zod validation schemas
│   ├── context.tsx              # React Context for v0.dev detection
│   ├── constants.ts             # Global constants
│   └── utils.ts                 # Utility functions & type definitions
│
├── hooks/                       # Custom React hooks
│   ├── use-mobile.tsx           # Mobile viewport detection
│   └── use-toast.ts             # Toast notification management
│
├── public/                      # Static assets
│   └── alt-placeholder.png      # Placeholder image
│
├── styles/                      # Styling (symlink to app/globals.css)
│   └── globals.css
│
├── Configuration Files
│   ├── next.config.mjs          # Next.js configuration
│   ├── tsconfig.json            # TypeScript configuration
│   ├── tailwind.config.js       # Tailwind CSS configuration
│   ├── eslint.config.mjs        # ESLint configuration
│   ├── postcss.config.js        # PostCSS configuration
│   ├── components.json          # shadcn/ui configuration
│   └── package.json             # Dependencies & scripts
│
└── Documentation
    ├── README.md                # Project introduction
    ├── CODE_STRUCTURE_ANALYSIS.md  # Detailed code analysis
    └── AGENTS.md                # This file
```

---

## Code Patterns & Conventions

### 1. Component Patterns

#### Server Components (Default)

```typescript
// ✅ Default export, no "use client"
export function Footer() {
  return <footer>{/* static content */}</footer>
}
```

#### Client Components (When Needed)

```typescript
// ✅ Explicitly marked with "use client"
"use client"

export function Newsletter() {
  const [state, setState] = useState(false)
  // Interactive logic
}
```

#### Component Props

```typescript
// ✅ Named props with explicit types
export const FormNewsletter = ({
  input,
  submit,
}: {
  input: (props: React.ComponentProps<"input">) => React.ReactNode
  submit: (props: React.ComponentProps<"button">) => React.ReactNode
}) => {
  // implementation
}
```

### 2. Form Handling

**Always use this pattern for forms**:

```typescript
"use client"

import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { newsletterSchema } from "@/lib/schema"

export function MyForm() {
  const form = useForm({
    resolver: zodResolver(newsletterSchema),
    defaultValues: { email: "" },
  })

  async function onSubmit(values) {
    const result = await serverAction(values)
    // Handle result
  }

  return <form onSubmit={form.handleSubmit(onSubmit)}>{/* form fields */}</form>
}
```

### 3. Server Actions

**Pattern for server mutations**:

```typescript
"use server"

import { ActionResult, success, error } from "@/lib/utils"

export async function myAction(input: string): Promise<ActionResult<string>> {
  try {
    // Validate
    const parsed = mySchema.safeParse({ value: input })
    if (!parsed.success) {
      return error(parsed.error.message)
    }

    // Process
    const result = await doSomething(parsed.data)

    return success(result)
  } catch (err) {
    return error(err instanceof Error ? err.message : "Unknown error")
  }
}
```

### 4. Styling with Tailwind

**Use utility-first CSS**:

```typescript
// ✅ Good
<div className="flex items-center justify-center gap-4 px-4 py-2">
  Content
</div>

// ❌ Bad
<div style={{ display: 'flex', alignItems: 'center', padding: '8px 16px' }}>
  Content
</div>
```

**Dark mode support**:

```typescript
// ✅ Always include dark variants
<div className="bg-white dark:bg-slate-950 text-black dark:text-white">
  Content
</div>
```

### 5. Animation Patterns

**Use Framer Motion for animations**:

```typescript
import { motion } from "framer-motion"

export function AnimatedComponent() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -10 }}
      transition={{ duration: 0.3 }}
    >
      Content
    </motion.div>
  )
}
```

### 6. Validation

**Always validate both client and server**:

```typescript
// lib/schema.ts
export const newsletterSchema = z.object({
  email: z.string().email(),
})

// components/form.tsx (client)
const form = useForm({ resolver: zodResolver(newsletterSchema) })

// lib/subscribe.ts (server)
const parsed = newsletterSchema.safeParse(data)
if (!parsed.success) return error(parsed.error.message)
```

### 7. Constants & Configuration

**Centralize constants in `lib/constants.ts`**:

```typescript
// ✅ Good
import { TABLES } from "@/lib/constants"
await redis.get(TABLES.EMAIL_LIST)

// ❌ Bad
await redis.get("email-list")
```

---

## Common Agent Tasks

### 1. Adding a New Feature

**Steps**:

1. Create new component in `components/` (mark with `"use client"` if interactive)
2. Define types in the component file or `lib/` if shared
3. Add Zod schema to `lib/schema.ts` if validation needed
4. Create server action in `lib/` if backend logic needed
5. Integrate with existing components
6. Test in browser and validate TypeScript compilation

**Example**: Adding a "Subscribe with Password" feature

```typescript
// lib/schema.ts
export const secureSubscribeSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
})

// lib/subscribe.ts
export async function secureSubscribe(
  email: string,
  password: string
): Promise<ActionResult<string>> {
  const parsed = secureSubscribeSchema.safeParse({ email, password })
  // ... implement
}

// components/secure-form.tsx
;("use client")
export function SecureForm() {
  // ... use secureSubscribe action
}
```

### 2. Modifying Existing Components

**When changing `components/form-newsletter.tsx` or similar**:

1. Understand current implementation first
2. Preserve all existing functionality
3. Update animations consistently with Framer Motion patterns
4. Test form submission flow end-to-end
5. Verify error handling still works

### 3. Updating Styles

**When modifying Tailwind CSS**:

1. Use only utility classes (no custom CSS unless necessary)
2. Respect existing theme variables in `app/globals.css`
3. Support dark mode with `dark:` variants
4. Test responsive breakpoints: `sm:`, `md:`, `lg:`, `xl:`
5. Verify accessibility: sufficient color contrast, readable text

### 4. Adding New UI Components

**From shadcn/ui**:

1. Check if component already exists in `components/ui/`
2. If needed, add via shadcn/ui CLI or copy from upstream
3. Verify Radix UI base is correct
4. Test accessibility features

### 5. Modifying Server Actions

**When working with `lib/subscribe.ts` or `lib/actions.ts`**:

1. Always mark with `"use server"` directive
2. Validate inputs with Zod schemas
3. Use `ActionResult<T>` for return type
4. Handle errors with try-catch
5. Return `success()` or `error()` helpers
6. Never expose sensitive data in error messages
7. Test with edge cases (duplicate emails, invalid input, etc.)

### 6. Adding Environment Variables

**Process**:

1. Document in `.env.example` file
2. Add to this AGENTS.md under [Environment Setup](#environment-setup)
3. Handle missing variables gracefully (demo mode pattern)
4. Validate variables on app startup
5. Use `process.env.VAR_NAME` with nullish coalescing or defaults

---

## Before Making Changes

### Checklist

- [ ] **Understand the current code** — Read related files first
- [ ] **Identify impacted areas** — What else might break?
- [ ] **Check for existing patterns** — Follow conventions, don't reinvent
- [ ] **Test plan** — How will I verify the change works?
- [ ] **Backward compatibility** — Does this break existing functionality?
- [ ] **Documentation** — Do I need to update comments or docs?

### Code Review Mindset

Before committing changes, ask yourself:

- ✅ Does this follow the project's conventions?
- ✅ Is all code properly typed?
- ✅ Are server actions used for mutations?
- ✅ Are forms using react-hook-form + Zod?
- ✅ Does it work in both light and dark modes?
- ✅ Is error handling comprehensive?
- ✅ Can a developer understand this code?

---

## File Modification Guidelines

### Safe to Modify

| File                 | Risk   | Notes                                         |
| -------------------- | ------ | --------------------------------------------- |
| `components/*.tsx`   | Low    | Add new components or modify existing UI      |
| `lib/subscribe.ts`   | Medium | Core subscription logic; test thoroughly      |
| `lib/schema.ts`      | Low    | Add validation schemas; don't remove existing |
| `lib/constants.ts`   | Low    | Add new constants; don't remove existing      |
| `lib/utils.ts`       | Medium | Modify with care; widely imported             |
| `app/globals.css`    | Low    | Add CSS variables, update styles              |
| `tailwind.config.js` | Low    | Extend theme, add custom utilities            |

### Critical / Requires Care

| File                | Why                            | Precautions                     |
| ------------------- | ------------------------------ | ------------------------------- |
| `app/layout.tsx`    | Root layout affects entire app | Test metadata, fonts, providers |
| `app/page.tsx`      | Main landing page              | Ensure all sections render      |
| `package.json`      | Dependency management          | Verify version compatibility    |
| `tsconfig.json`     | TypeScript compilation         | Don't break path aliases        |
| `next.config.mjs`   | Next.js build behavior         | Test production build           |
| `eslint.config.mjs` | Code quality                   | Verify linting still works      |

### Do Not Modify (Usually)

| File             | Why                             |
| ---------------- | ------------------------------- |
| `next-env.d.ts`  | Auto-generated by Next.js       |
| `pnpm-lock.yaml` | Auto-generated by pnpm          |
| `.git/`          | Version control; managed by git |

---

## Testing & Validation

### Before Submitting Changes

```bash
# 1. Type check
npm run build

# 2. Lint code
npm run lint

# 3. Run dev server to test UI
npm run dev

# 4. Test the newsletter form manually
# - Try valid email
# - Try invalid email (should show error)
# - Try duplicate email (if Redis configured)
# - Check localStorage persistence
```

### Form Testing Checklist

- [ ] ✅ Displays properly on mobile and desktop
- [ ] ✅ Validates email format correctly
- [ ] ✅ Shows error messages for invalid input
- [ ] ✅ Disables submit button while loading
- [ ] ✅ Shows success message after subscription
- [ ] ✅ Resets form after successful submission
- [ ] ✅ Persists email to localStorage
- [ ] ✅ Keyboard navigation works (Escape, Enter, Tab)
- [ ] ✅ Animations are smooth and not jarring
- [ ] ✅ Works in light and dark modes

### Component Testing

For new components, verify:

- TypeScript compilation passes
- Component renders without errors
- Props are typed correctly
- Event handlers work as expected
- Responsive design works (test at multiple breakpoints)
- Accessibility features work (keyboard nav, screen readers if applicable)

---

## Environment Setup

### Required Environment Variables

For **production** (Vercel deployment):

```env
KV_REST_API_URL=https://your-redis-endpoint.upstash.io
KV_REST_API_TOKEN=your-redis-token
```

### Optional Environment Variables

```env
# For Vercel
VERCEL_URL=your-deployment-url  # Auto-set by Vercel

# Development
NEXT_PUBLIC_API_URL=http://localhost:3000
```

### Development Setup

```bash
# 1. Clone repo
git clone https://github.com/ajtruex/v0-newsletter-template.git
cd v0-newsletter-template

# 2. Install dependencies
npm install
# or
pnpm install

# 3. Create .env.local (copy from .env if it exists)
cp .env .env.local

# 4. Run dev server
npm run dev

# 5. Open browser
open http://localhost:3000
```

### Without Redis (Demo Mode)

The app gracefully handles missing Redis credentials:

```typescript
// lib/subscribe.ts
const IS_DEMO = !process.env.KV_REST_API_URL || !process.env.KV_REST_API_TOKEN

if (IS_DEMO) {
  return error("Missing required setup")
}
```

This allows exploring the UI without configuring a database.

---

## Critical Files & Areas

### Files to Understand First

1. **`lib/subscribe.ts`** — Core subscription logic

   - Validates emails with Zod
   - Checks Redis for duplicates
   - Returns `ActionResult` type
   - Handles Redis connection

2. **`components/form-newsletter.tsx`** — Form component

   - Uses react-hook-form
   - Integrates with `subscribe` server action
   - Displays animated errors/success
   - Persists email to localStorage

3. **`components/newsletter.tsx`** — Main UI orchestration

   - Manages manifesto toggle state
   - Complex Framer Motion animations
   - Responsive layout
   - Keyboard event handling

4. **`lib/schema.ts`** — Validation schemas

   - Zod schema for email validation
   - Client-side and server-side validation
   - Type inference with `z.infer<typeof>`

5. **`app/layout.tsx`** — Root layout
   - Font imports and CSS variables
   - V0Provider context
   - Metadata configuration
   - Dynamic V0Setup component

### Data Flow to Understand

```
User Input → FormNewsletter Component → react-hook-form validation
  ↓
  subscribe() Server Action
  ↓
  Zod validation (server-side)
  ↓
  Redis operations (add email to list)
  ↓
  ActionResult<string> response
  ↓
  FormNewsletter processes result → Show success/error message
  ↓
  localStorage persistence
```

---

## Constraints & Limitations

### Framework Constraints

- **Next.js 15 App Router** — Must use new patterns (Server Components, Server Actions)
- **React 19** — Use new hook APIs; avoid legacy patterns
- **Vercel Deployment** — Optimized for Vercel; some features may differ on other hosts

### Business Logic Constraints

- **Email validation only** — Forms only capture emails, no additional user data
- **Redis storage** — Emails stored as simple list in Redis, no user profiles
- **No authentication** — Public form; no user accounts or login
- **Single email validation schema** — All email fields use same Zod schema

### Performance Constraints

- **Server Components by default** — Client components used sparingly
- **Minimal client JavaScript** — Optimized bundle size
- **Animations use Framer Motion** — No CSS-in-JS or custom animation libs
- **No third-party form library** — Only react-hook-form for forms

### UI/UX Constraints

- **Responsive design required** — Must work on mobile, tablet, desktop
- **Dark mode support** — All components must support light/dark themes
- **Accessibility compliance** — Uses Radix UI for a11y
- **Smooth animations** — No jarring transitions; use Framer Motion patterns

---

## Helpful Resources

### Project-Specific

- **[CODE_STRUCTURE_ANALYSIS.md](./CODE_STRUCTURE_ANALYSIS.md)** — Detailed code structure breakdown
- **[README.md](./README.md)** — Project overview and getting started

### Framework Documentation

- **[Next.js 15 Docs](https://nextjs.org/docs)** — Server Components, Actions, App Router
- **[React 19 Docs](https://react.dev)** — New hooks and features
- **[TypeScript Handbook](https://www.typescriptlang.org/docs/)** — Type system

### Library Documentation

- **[Tailwind CSS](https://tailwindcss.com/docs)** — Styling utilities
- **[Framer Motion](https://www.framer.com/motion/)** — Animations
- **[React Hook Form](https://react-hook-form.com/)** — Form management
- **[Zod](https://zod.dev/)** — Schema validation
- **[Radix UI](https://radix-ui.com/docs/primitives/overview/introduction)** — UI primitives
- **[shadcn/ui](https://ui.shadcn.com/)** — Component collection

### Tools & Testing

- **npm/pnpm** — Package management
- **TypeScript** — Type checking: `npm run build`
- **ESLint** — Code quality: `npm run lint`
- **Next.js Dev Server** — `npm run dev`
- **Git** — Version control and history

---

## Quick Reference: Common Commands

```bash
# Development
npm run dev              # Start dev server on http://localhost:3000

# Building
npm run build            # Build for production (includes TypeScript check)
npm start                # Start production server

# Code Quality
npm run lint             # Run ESLint
npm run build            # TypeScript compilation check

# Dependencies
npm install              # Install packages
npm update               # Update packages
```

---

## Tips for AI Agents

### ✅ Do's

- ✅ **Read before modifying** — Understand existing patterns first
- ✅ **Follow conventions** — Match the project's code style
- ✅ **Test your changes** — Run `npm run build` and `npm run dev`
- ✅ **Handle errors gracefully** — Use ActionResult pattern
- ✅ **Think accessibility** — Use Radix UI and semantic HTML
- ✅ **Respect performance** — Keep client JavaScript minimal
- ✅ **Document changes** — Update comments if logic is complex

### ❌ Don'ts

- ❌ **Don't use CSS-in-JS** — Use Tailwind CSS utilities only
- ❌ **Don't create client components unnecessarily** — Use Server Components by default
- ❌ **Don't import directly from node_modules** — Use path aliases (`@/`)
- ❌ **Don't hardcode strings** — Use constants from `lib/constants.ts`
- ❌ **Don't skip validation** — Validate both client and server
- ❌ **Don't break TypeScript** — All code must compile in strict mode
- ❌ **Don't ignore error handling** — Always handle errors gracefully
- ❌ **Don't make unnecessary dependencies** — Keep the dependency tree lean

---

## Feedback & Improvements

If you (an AI agent) find:

- 🔴 **Missing documentation** — Add it to this file
- 🔴 **Unclear patterns** — Suggest improvements in comments
- 🔴 **Type inconsistencies** — Fix them with proper types
- 🔴 **Performance issues** — Optimize and document the change

This document should evolve as the project grows. Keep it updated!

---

## Final Notes

This is a **professional, well-maintained codebase** that demonstrates modern web development best practices. When working on it:

1. **Respect the existing quality** — Don't lower standards
2. **Learn from the patterns** — This is reference-quality code
3. **Maintain consistency** — New code should feel native to the project
4. **Test thoroughly** — Changes affect users
5. **Document well** — Future developers (including other agents) will thank you

Happy coding! 🚀
