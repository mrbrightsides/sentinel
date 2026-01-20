# Contributing to RANTAI Sentinel

> **Live Demo:** [https://sentinel.elpeef.com/](https://sentinel.elpeef.com/)  
> **Repository:** [https://github.com/mrbrightsides/sentinel](https://github.com/mrbrightsides/sentinel)

Thank you for your interest in contributing to **RANTAI Sentinel**! We welcome contributions from the community to help make ESG compliance more accessible and intelligent.

---

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Workflow](#development-workflow)
4. [Code Standards](#code-standards)
5. [Commit Guidelines](#commit-guidelines)
6. [Pull Request Process](#pull-request-process)
7. [Testing Guidelines](#testing-guidelines)
8. [Documentation](#documentation)
9. [Issue Reporting](#issue-reporting)
10. [Feature Requests](#feature-requests)
11. [Community](#community)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for everyone. We expect all participants to:

- **Be Respectful**: Treat everyone with respect and kindness
- **Be Constructive**: Provide helpful feedback and suggestions
- **Be Inclusive**: Welcome contributors of all backgrounds and experience levels
- **Be Professional**: Maintain a harassment-free environment

### Unacceptable Behavior

- Harassment, discrimination, or offensive comments
- Personal attacks or trolling
- Publishing others' private information
- Spamming or excessive self-promotion

**Violations** will result in removal from the project and community.

---

## Getting Started

### Prerequisites

Before contributing, ensure you have:

```bash
# Required
Node.js >= 18.0.0
npm >= 9.0.0 or pnpm >= 8.0.0
Git >= 2.30.0

# Recommended
VS Code with extensions:
  - ESLint
  - Prettier
  - TypeScript and JavaScript Language Features
  - Tailwind CSS IntelliSense
```

### Fork and Clone

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:

```bash
git clone https://github.com/YOUR_USERNAME/sentinel.git
cd sentinel
```

3. **Add upstream remote**:

```bash
git remote add upstream https://github.com/mrbrightsides/sentinel.git
```

4. **Install dependencies**:

```bash
npm install
# or
pnpm install
```

5. **Create a branch** for your work:

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-number-description
```

### Environment Setup

See [SETUP.md](./SETUP.md) for detailed environment configuration.

**Quick start:**

```bash
# Copy environment template (when available)
cp .env.example .env.local

# Add your API keys (optional for development)
# OPENAI_API_KEY=sk-proj-...
# ANTHROPIC_API_KEY=sk-ant-...
# GOOGLE_AI_API_KEY=...

# Run development server
npm run dev
```

---

## Development Workflow

### 1. **Sync with Upstream**

Before starting work, sync your fork:

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

### 2. **Create a Feature Branch**

```bash
git checkout -b feature/add-new-chart
# or
git checkout -b fix/predictions-api-error
```

**Branch naming conventions:**
- `feature/description` - New features
- `fix/description` - Bug fixes
- `docs/description` - Documentation updates
- `refactor/description` - Code refactoring
- `test/description` - Test additions/updates
- `chore/description` - Maintenance tasks

### 3. **Make Your Changes**

Follow our [Code Standards](#code-standards) and ensure:
- ✅ Code is properly typed (TypeScript)
- ✅ Components are responsive (mobile-first)
- ✅ Accessibility standards are met (WCAG 2.1 AA)
- ✅ No console errors or warnings
- ✅ Code is formatted (Prettier)
- ✅ Linting passes (ESLint)

### 4. **Test Your Changes**

```bash
# Run development server
npm run dev

# Test in browser
# - Desktop: Chrome, Firefox, Safari
# - Mobile: iOS Safari, Chrome Android
# - Themes: Light and Dark mode
# - Breakpoints: Mobile (320px), Tablet (768px), Desktop (1024px+)

# Run type checking
npm run type-check

# Run linting
npm run lint

# Build for production (verify no errors)
npm run build
```

### 5. **Commit Your Changes**

Follow our [Commit Guidelines](#commit-guidelines):

```bash
git add .
git commit -m "feat: add interactive ESG trend chart"
```

### 6. **Push to Your Fork**

```bash
git push origin feature/add-new-chart
```

### 7. **Open a Pull Request**

1. Go to your fork on GitHub
2. Click "Compare & pull request"
3. Fill out the PR template (see [Pull Request Process](#pull-request-process))
4. Submit for review

---

## Code Standards

### TypeScript Guidelines

#### 1. **Strict Type Safety**

```typescript
// ✅ GOOD: Explicit types
interface ESGData {
  environmental: number;
  social: number;
  governance: number;
}

function calculateScore(data: ESGData): number {
  return (data.environmental + data.social + data.governance) / 3;
}

// ❌ BAD: Implicit any
function calculateScore(data) { // Error: implicit any
  return (data.environmental + data.social + data.governance) / 3;
}
```

#### 2. **Interface Over Type**

```typescript
// ✅ GOOD: Use interface for objects
interface User {
  id: string;
  name: string;
  email: string;
}

// ⚠️ ACCEPTABLE: Use type for unions/intersections
type Status = 'active' | 'inactive' | 'pending';
type UserWithStatus = User & { status: Status };
```

#### 3. **Avoid `any` and `unknown`**

```typescript
// ❌ BAD
function processData(data: any) { }

// ✅ GOOD: Use generics
function processData<T extends Record<string, unknown>>(data: T): T {
  return data;
}
```

#### 4. **Proper Null Handling**

```typescript
// ✅ GOOD: Optional chaining and nullish coalescing
const userName = user?.name ?? 'Guest';

// ❌ BAD: Unsafe access
const userName = user.name; // May throw if user is undefined
```

### React Component Guidelines

#### 1. **Functional Components**

```typescript
// ✅ GOOD: Functional component with typed props
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
}

export function Button({ label, onClick, variant = 'primary' }: ButtonProps) {
  return (
    <button onClick={onClick} className={`btn-${variant}`}>
      {label}
    </button>
  );
}

// ❌ BAD: No prop types
export function Button({ label, onClick }) { // Error: implicit any
  return <button onClick={onClick}>{label}</button>;
}
```

#### 2. **Client vs Server Components**

```typescript
// Server Component (default in Next.js 15 App Router)
// - No 'use client' directive
// - Can fetch data directly
// - Cannot use hooks or event handlers

export async function ESGDashboard() {
  const data = await fetchESGData(); // Server-side fetch
  return <div>{data.score}</div>;
}

// Client Component
// - Requires 'use client' directive
// - Can use hooks, state, event handlers
// - Runs in browser

'use client';

import { useState } from 'react';

export function InteractiveChart() {
  const [selected, setSelected] = useState<string | null>(null);
  return <div onClick={() => setSelected('item')}>...</div>;
}
```

#### 3. **Hook Usage**

```typescript
'use client';

import { useState, useEffect, useCallback, useMemo } from 'react';

export function ESGMetrics() {
  // State
  const [data, setData] = useState<ESGData | null>(null);
  const [loading, setLoading] = useState(false);
  
  // Memoized values
  const averageScore = useMemo(() => {
    if (!data) return 0;
    return (data.environmental + data.social + data.governance) / 3;
  }, [data]);
  
  // Callbacks
  const fetchData = useCallback(async () => {
    setLoading(true);
    const response = await fetch('/api/esg');
    const result = await response.json();
    setData(result);
    setLoading(false);
  }, []);
  
  // Effects
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  
  return <div>{averageScore}</div>;
}
```

### Styling Guidelines

#### 1. **Tailwind CSS Best Practices**

```typescript
// ✅ GOOD: Responsive, semantic classes
<div className="flex flex-col gap-4 p-4 md:flex-row md:gap-6 md:p-6">
  <Card className="bg-white dark:bg-gray-900 shadow-lg" />
</div>

// ❌ BAD: Inline styles, non-responsive
<div style={{ display: 'flex', padding: '16px' }}>
  <Card />
</div>
```

#### 2. **Dark Mode Support**

```typescript
// Always provide dark mode variants
<div className="bg-white text-black dark:bg-gray-900 dark:text-white">
  <h1 className="text-2xl font-bold text-gray-900 dark:text-gray-100">
    Title
  </h1>
</div>
```

#### 3. **Use `cn()` Utility for Class Merging**

```typescript
import { cn } from '@/lib/utils';

interface CardProps {
  className?: string;
  variant?: 'default' | 'outlined';
}

export function Card({ className, variant = 'default' }: CardProps) {
  return (
    <div
      className={cn(
        'rounded-lg p-4',
        variant === 'default' && 'bg-white shadow-md',
        variant === 'outlined' && 'border-2 border-gray-300',
        className
      )}
    />
  );
}
```

### API Route Guidelines

#### 1. **Standard Structure**

```typescript
// src/app/api/feature/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  try {
    // 1. Parse and validate input
    const body = await request.json();
    
    if (!body.data) {
      return NextResponse.json(
        { error: 'Missing required field: data' },
        { status: 400 }
      );
    }
    
    // 2. Process request
    const result = await processData(body.data);
    
    // 3. Return response
    return NextResponse.json(result);
    
  } catch (error) {
    console.error('API Error:', error);
    return NextResponse.json(
      { error: 'Internal server error' },
      { status: 500 }
    );
  }
}
```

#### 2. **Error Handling**

```typescript
// ✅ GOOD: Specific error messages
if (!apiKey) {
  return NextResponse.json(
    { error: 'API key is required' },
    { status: 401 }
  );
}

if (!data || !Array.isArray(data)) {
  return NextResponse.json(
    { error: 'Invalid data format. Expected array.' },
    { status: 400 }
  );
}

// ❌ BAD: Generic errors
if (!data) {
  return NextResponse.json({ error: 'Error' }, { status: 500 });
}
```

### File Organization

```
src/
├── app/                    # Next.js App Router
│   ├── page.tsx            # Main page
│   ├── layout.tsx          # Root layout
│   └── api/                # API routes
│       └── [feature]/
│           └── route.ts    # API endpoint
│
├── components/             # React components
│   ├── ui/                 # Base UI components (Radix wrappers)
│   │   ├── button.tsx      # One component per file
│   │   ├── card.tsx
│   │   └── index.ts        # Barrel export (optional)
│   │
│   └── [feature]/          # Feature-specific components
│       ├── FeaturePanel.tsx
│       ├── FeatureCard.tsx
│       └── index.ts
│
├── lib/                    # Utilities and helpers
│   ├── utils.ts            # Shared utilities
│   ├── api.ts              # API client functions
│   └── constants.ts        # Constants and config
│
└── types/                  # TypeScript type definitions
    ├── esg.ts              # ESG-related types
    ├── api.ts              # API response types
    └── index.ts            # Barrel export
```

---

## Commit Guidelines

We follow **Conventional Commits** specification.

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(ai): add blockchain analysis endpoint` |
| `fix` | Bug fix | `fix(dashboard): resolve chart rendering issue` |
| `docs` | Documentation | `docs(readme): update installation steps` |
| `style` | Code style (formatting) | `style(components): fix indentation` |
| `refactor` | Code refactoring | `refactor(api): extract OpenAI client` |
| `test` | Tests | `test(predictions): add unit tests` |
| `chore` | Maintenance | `chore(deps): update Next.js to 15.3.8` |
| `perf` | Performance | `perf(dashboard): optimize re-renders` |

### Examples

```bash
# Good commits
git commit -m "feat(ai): add ESG predictions with GPT-4 integration"
git commit -m "fix(security): resolve threat detection API timeout"
git commit -m "docs(architecture): add scalability section"

# Bad commits
git commit -m "update stuff"
git commit -m "fix bug"
git commit -m "WIP"
```

### Detailed Commit

```bash
git commit -m "feat(ai): add blockchain transaction analysis

- Integrate OpenAI GPT-4 for anomaly detection
- Add smart contract vulnerability scanning
- Implement carbon credit verification
- Include confidence scoring for all findings

Closes #123"
```

---

## Pull Request Process

### Before Submitting

**Checklist:**
- [ ] Code follows TypeScript and React guidelines
- [ ] All files are properly typed (no `any` or implicit types)
- [ ] Components are responsive (tested on mobile and desktop)
- [ ] Dark mode works correctly
- [ ] No console errors or warnings
- [ ] Code is formatted (`npm run format`)
- [ ] Linting passes (`npm run lint`)
- [ ] Type checking passes (`npm run type-check`)
- [ ] Build succeeds (`npm run build`)
- [ ] Changes are documented (if needed)

### PR Template

```markdown
## Description
<!-- Briefly describe what this PR does -->

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issues
<!-- Link related issues: Closes #123, Fixes #456 -->

## Changes Made
<!-- List the key changes -->
- Added X feature
- Fixed Y bug
- Refactored Z component

## Screenshots (if applicable)
<!-- Add screenshots for UI changes -->

### Desktop
![Desktop view](url)

### Mobile
![Mobile view](url)

### Dark Mode
![Dark mode](url)

## Testing Checklist
- [ ] Tested on Chrome (desktop)
- [ ] Tested on Firefox (desktop)
- [ ] Tested on Safari (desktop)
- [ ] Tested on iOS Safari (mobile)
- [ ] Tested on Chrome Android (mobile)
- [ ] Tested in light mode
- [ ] Tested in dark mode
- [ ] Tested responsive breakpoints (320px, 768px, 1024px, 1440px)

## Additional Notes
<!-- Any additional context or notes for reviewers -->
```

### Review Process

1. **Automated Checks**: CI/CD runs linting, type-checking, and builds
2. **Code Review**: At least one maintainer reviews the code
3. **Testing**: Reviewer tests changes locally
4. **Approval**: PR is approved and merged

### After Merge

- Your branch will be deleted automatically
- Update your local fork:

```bash
git checkout main
git pull upstream main
git push origin main
```

---

## Testing Guidelines

### Manual Testing

#### 1. **Functional Testing**

Test all user interactions:
```
✅ Dashboard loads correctly
✅ All tabs are accessible
✅ Forms submit successfully
✅ API calls complete without errors
✅ Data persists in localStorage
✅ Charts render with real data
✅ Modals and dialogs work
✅ Buttons and links function
```

#### 2. **Responsive Testing**

Test on multiple breakpoints:
```
✅ Mobile (320px - 767px)
  - Touch interactions work
  - Text is readable
  - No horizontal scroll
  - Navigation is accessible

✅ Tablet (768px - 1023px)
  - Layout adapts properly
  - Images scale correctly

✅ Desktop (1024px+)
  - Full features available
  - Optimal use of screen space
```

#### 3. **Cross-Browser Testing**

```
✅ Chrome (latest)
✅ Firefox (latest)
✅ Safari (latest)
✅ Edge (latest)
✅ iOS Safari (mobile)
✅ Chrome Android (mobile)
```

#### 4. **Accessibility Testing**

```
✅ Keyboard navigation works
  - Tab through all interactive elements
  - Enter/Space activate buttons
  - Esc closes modals

✅ Screen reader compatible
  - Alt text on images
  - ARIA labels on controls
  - Semantic HTML structure

✅ Color contrast sufficient
  - WCAG AA compliance (4.5:1 for text)
  - Works in dark mode

✅ Focus indicators visible
  - All interactive elements have focus states
```

### Unit Testing (Future)

```typescript
// Example: Component test with Jest + React Testing Library
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from '@/components/ui/button';

describe('Button', () => {
  it('renders with label', () => {
    render(<Button label="Click me" onClick={() => {}} />);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });
  
  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button label="Click" onClick={handleClick} />);
    fireEvent.click(screen.getByText('Click'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### Integration Testing (Future)

```typescript
// Example: API route test
import { POST } from '@/app/api/ai/predictions/route';

describe('AI Predictions API', () => {
  it('returns predictions for valid input', async () => {
    const request = new Request('http://localhost/api/ai/predictions', {
      method: 'POST',
      body: JSON.stringify({ data: mockESGData })
    });
    
    const response = await POST(request);
    const data = await response.json();
    
    expect(response.status).toBe(200);
    expect(data.predictions).toHaveLength(6);
  });
});
```

---

## Documentation

### Code Documentation

#### 1. **Component Documentation**

```typescript
/**
 * ESG Predictions Panel
 * 
 * Displays AI-powered 6-month ESG predictions with scenario analysis.
 * Integrates with OpenAI GPT-4 for forecasting.
 * 
 * @example
 * ```tsx
 * <ESGPredictionsPanel />
 * ```
 */
export function ESGPredictionsPanel() {
  // Component implementation
}
```

#### 2. **Complex Function Documentation**

```typescript
/**
 * Calculates weighted ESG score based on industry standards
 * 
 * @param data - ESG metrics (environmental, social, governance)
 * @param weights - Optional weights for each pillar (default: equal)
 * @returns Weighted score (0-100)
 * 
 * @example
 * ```typescript
 * const score = calculateWeightedScore(
 *   { environmental: 80, social: 75, governance: 85 },
 *   { environmental: 0.4, social: 0.3, governance: 0.3 }
 * );
 * // Returns: 79.5
 * ```
 */
export function calculateWeightedScore(
  data: ESGData,
  weights?: ESGWeights
): number {
  // Implementation
}
```

#### 3. **API Documentation**

```typescript
/**
 * POST /api/ai/predictions
 * 
 * Generates 6-month ESG predictions using OpenAI GPT-4
 * 
 * @request
 * ```json
 * {
 *   "data": {
 *     "environmental": 75,
 *     "social": 80,
 *     "governance": 85
 *   }
 * }
 * ```
 * 
 * @response
 * ```json
 * {
 *   "predictions": [
 *     { "month": "Feb 2025", "environmental": 76, ... },
 *     ...
 *   ],
 *   "insights": [...],
 *   "scenarios": { "optimistic": {...}, ... }
 * }
 * ```
 */
export async function POST(request: NextRequest) {
  // Implementation
}
```

### README Updates

When adding new features, update:
- **Features** section: Add your feature
- **Usage** section: Add usage instructions
- **API** section: Document new endpoints
- **Screenshots** section: Add visuals (if UI change)

---

## Issue Reporting

### Bug Reports

Use this template when reporting bugs:

```markdown
**Bug Description**
A clear description of the bug.

**Steps to Reproduce**
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

**Expected Behavior**
What should happen.

**Actual Behavior**
What actually happens.

**Screenshots**
If applicable, add screenshots.

**Environment**
- OS: [e.g., macOS 14.1]
- Browser: [e.g., Chrome 120]
- Device: [e.g., iPhone 15 Pro]
- Screen Size: [e.g., 1920x1080]

**Console Errors**
```
Paste any console errors here
```

**Additional Context**
Any other relevant information.
```

### Security Issues

**DO NOT** open public issues for security vulnerabilities.

Instead, email: [support@elpeef.com](mailto:support@elpeef.com) with:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

We will respond within **48 hours**.

---

## Feature Requests

Use this template for feature requests:

```markdown
**Feature Description**
Clear description of the feature.

**Problem It Solves**
What problem does this address?

**Proposed Solution**
How should it work?

**Alternatives Considered**
Other approaches you've thought about.

**Additional Context**
Mockups, examples, references, etc.

**Would You Implement This?**
- [ ] Yes, I can submit a PR
- [ ] No, but I'm willing to test
- [ ] Just suggesting
```

---

## Community

### Communication Channels

- **GitHub Issues**: Bug reports, feature requests
- **GitHub Discussions**: Questions, ideas, showcase
- **Email**: [support@elpeef.com](mailto:support@elpeef.com)
- **Telegram**: [@khudriakhmad](https://t.me/khudriakhmad)
- **Discord**: @khudri_61362

### Getting Help

**Before asking:**
1. Check [README.md](./README.md)
2. Check [SETUP.md](./SETUP.md)
3. Search existing issues
4. Review [ARCHITECTURE.md](./ARCHITECTURE.md)

**When asking:**
- Be specific about your problem
- Include error messages and console logs
- Describe what you've already tried
- Provide environment details

### Recognition

Contributors will be recognized in:
- **README.md** (Contributors section)
- **Release notes** (for significant contributions)
- **Project website** (when available)

---

## Development Tips

### Performance

```typescript
// Use React.memo for expensive components
import { memo } from 'react';

export const ExpensiveChart = memo(function ExpensiveChart({ data }) {
  // Heavy rendering logic
});

// Use useMemo for expensive calculations
const processedData = useMemo(() => {
  return data.map(item => expensiveOperation(item));
}, [data]);

// Use useCallback for functions passed as props
const handleClick = useCallback(() => {
  doSomething();
}, []);
```

### Debugging

```typescript
// Type-safe console logging
const debugLog = (message: string, data?: unknown) => {
  if (process.env.NODE_ENV === 'development') {
    console.log(`[DEBUG] ${message}`, data);
  }
};

// Use React DevTools
// - Install extension
// - Inspect component tree
// - Profile performance
```

### VS Code Tips

```json
// .vscode/settings.json (recommended)
{
  "editor.formatOnSave": true,
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

## License

By contributing, you agree that your contributions will be licensed under the **MIT License**.

---

## Thank You! 🙏

Your contributions help make ESG compliance more accessible and intelligent for organizations worldwide. Every PR, issue, and suggestion makes a difference!

**Questions?**
- 📧 Email: [support@elpeef.com](mailto:support@elpeef.com)
- 💬 Telegram: [@khudriakhmad](https://t.me/khudriakhmad)
- 🎮 Discord: @khudri_61362

**Happy Contributing! 🚀**

---

**Last Updated**: January 2025  
**Version**: 2.0.0  
**Maintained by**: RANTAI Sentinel Team
