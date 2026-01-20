# RANTAI Sentinel - System Architecture

> **Live Demo:** [https://sentinel.elpeef.com/](https://sentinel.elpeef.com/)  
> **Repository:** [https://github.com/mrbrightsides/sentinel](https://github.com/mrbrightsides/sentinel)

---

## Table of Contents

1. [Overview](#overview)
2. [High-Level Architecture](#high-level-architecture)
3. [Frontend Architecture](#frontend-architecture)
4. [Backend Architecture](#backend-architecture)
5. [AI Integration Layer](#ai-integration-layer)
6. [Data Flow](#data-flow)
7. [Component Structure](#component-structure)
8. [State Management](#state-management)
9. [Security Architecture](#security-architecture)
10. [Performance Optimization](#performance-optimization)
11. [Scalability Considerations](#scalability-considerations)

---

## Overview

RANTAI Sentinel is a modern, AI-powered ESG compliance platform built on a **serverless, edge-optimized architecture** using Next.js 15, TypeScript, and OpenAI GPT-4. The system follows a **component-based, modular design** with strict type safety and separation of concerns.

### Key Architectural Principles

- **Type Safety First**: 100% TypeScript coverage with strict mode enabled
- **Component Modularity**: Atomic design pattern with reusable components
- **API-First Design**: RESTful API routes with proper error handling
- **Client-Side Data Persistence**: localStorage for rapid prototyping and offline-first capability
- **Progressive Enhancement**: Works without JavaScript, enhanced with React
- **Mobile-First Responsive**: Touch-optimized interactions, fluid layouts
- **Accessibility**: WCAG 2.1 AA compliant with semantic HTML

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                          │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  Next.js App Router (React 18 + Server Components)    │ │
│  │  - Pages & Layouts                                      │ │
│  │  - Client Components (Interactive UI)                   │ │
│  │  - Server Components (Static Content)                   │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                      API LAYER (Next.js)                     │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  API Routes (/api/*)                                    │ │
│  │  - /api/ai/predictions  → OpenAI GPT-4 ESG Forecasting │ │
│  │  - /api/ai/alerts       → Anomaly Detection & Insights │ │
│  │  - /api/ai/blockchain   → Blockchain Analysis          │ │
│  │  - /api/ai/reports      → Report Intelligence          │ │
│  │  - /api/ai/tasks        → Task Optimization            │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                   EXTERNAL SERVICES LAYER                    │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  OpenAI GPT-4 API                                       │ │
│  │  - Chat Completions (gpt-4o-mini)                       │ │
│  │  - Structured JSON Responses                            │ │
│  │  - Context-Aware Analysis                               │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                     DATA PERSISTENCE LAYER                   │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  Browser localStorage                                   │ │
│  │  - ESG Metrics & Scores                                 │ │
│  │  - Blockchain Transactions                              │ │
│  │  - Tasks & Reports                                      │ │
│  │  - User Preferences (theme, settings)                   │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## Frontend Architecture

### Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Framework** | Next.js 15.3.8 | React framework with App Router, SSR, SSG |
| **Language** | TypeScript 5.0 | Type safety, intellisense, compile-time checks |
| **Styling** | Tailwind CSS v4 | Utility-first CSS, responsive design |
| **UI Components** | Radix UI | Accessible, unstyled primitives |
| **Charts** | Recharts | Declarative charting library |
| **Animations** | Framer Motion | Production-ready motion library |
| **Icons** | Lucide React | Consistent icon system |

### Directory Structure

```
src/
├── app/                          # Next.js App Router
│   ├── layout.tsx                # Root layout with providers
│   ├── page.tsx                  # Main dashboard page
│   ├── api/                      # API routes
│   │   └── ai/                   # AI-related endpoints
│   │       ├── predictions/route.ts
│   │       ├── alerts/route.ts
│   │       ├── blockchain/route.ts
│   │       ├── reports/route.ts
│   │       └── tasks/route.ts
│   └── globals.css               # Global styles + Tailwind
│
├── components/                   # React components (atomic design)
│   ├── ui/                       # Base UI components (Radix UI wrappers)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── tabs.tsx
│   │   ├── dialog.tsx
│   │   └── ...
│   │
│   ├── dashboard/                # Dashboard-specific components
│   │   ├── ESGMetricsPanel.tsx
│   │   ├── ComplianceScore.tsx
│   │   └── AlertsPanel.tsx
│   │
│   ├── esg/                      # ESG Management components
│   │   ├── DataInputPanel.tsx
│   │   ├── AnalyticsPanel.tsx
│   │   └── BenchmarkingPanel.tsx
│   │
│   ├── security/                 # Security & Risk components
│   │   ├── SecurityDashboard.tsx
│   │   ├── AIThreatDetection.tsx
│   │   └── BlockchainAudits.tsx
│   │
│   ├── ai/                       # AI & Insights components
│   │   ├── ESGPredictionsPanel.tsx
│   │   ├── AlertsPanel.tsx
│   │   └── BlockchainPanel.tsx
│   │
│   └── reports/                  # Reports & Tasks components
│       ├── ReportsPanel.tsx
│       └── TasksPanel.tsx
│
├── lib/                          # Utility functions & helpers
│   └── utils.ts                  # Shared utilities (cn, formatters)
│
└── types/                        # TypeScript type definitions
    ├── esg.ts
    ├── security.ts
    ├── ai.ts
    └── reports.ts
```

### Component Architecture Patterns

#### 1. **Atomic Design Pattern**

```
Atoms → Buttons, Inputs, Icons
  ↓
Molecules → Cards, Form Fields, Chart Elements
  ↓
Organisms → Panels, Tables, Complex Forms
  ↓
Templates → Page Layouts
  ↓
Pages → Complete Views (Dashboard, ESG, Security)
```

#### 2. **Component Composition**

```typescript
// Example: ESG Predictions Panel
<ESGPredictionsPanel>
  <PredictionHeader />
  <AIInsightsCard>
    <RefreshButton />
    <ConfidenceScore />
  </AIInsightsCard>
  <PredictionsChart>
    <LineChart />
    <AreaChart />
  </PredictionsChart>
  <ScenarioAnalysis>
    <ScenarioCard type="optimistic" />
    <ScenarioCard type="realistic" />
    <ScenarioCard type="pessimistic" />
  </ScenarioAnalysis>
</ESGPredictionsPanel>
```

#### 3. **Smart vs Presentation Components**

- **Smart Components** (Container): Handle data fetching, state management, business logic
- **Presentation Components** (Presentational): Pure functions, receive props, render UI

```typescript
// Smart Component (Container)
export function ESGPredictionsPanel() {
  const [predictions, setPredictions] = useState<Prediction[]>([]);
  const [loading, setLoading] = useState(false);
  
  const fetchPredictions = async () => {
    // API call logic
  };
  
  return <PredictionsView data={predictions} loading={loading} />;
}

// Presentation Component
interface PredictionsViewProps {
  data: Prediction[];
  loading: boolean;
}

export function PredictionsView({ data, loading }: PredictionsViewProps) {
  // Pure rendering logic
}
```

---

## Backend Architecture

### API Routes Structure

All API routes follow a consistent pattern:

```typescript
// /api/ai/[feature]/route.ts

import { NextRequest, NextResponse } from 'next/server';
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: 'sk-proj-...' // Hardcoded for prototype (production: env vars)
});

export async function POST(request: NextRequest) {
  try {
    // 1. Parse request body
    const body = await request.json();
    
    // 2. Validate inputs
    if (!body.data) {
      return NextResponse.json(
        { error: 'Missing required data' },
        { status: 400 }
      );
    }
    
    // 3. Call OpenAI API
    const completion = await openai.chat.completions.create({
      model: 'gpt-4o-mini',
      messages: [
        {
          role: 'system',
          content: 'You are an ESG compliance expert...'
        },
        {
          role: 'user',
          content: JSON.stringify(body.data)
        }
      ],
      response_format: { type: 'json_object' },
      temperature: 0.7
    });
    
    // 4. Parse and validate response
    const result = JSON.parse(
      completion.choices[0].message.content || '{}'
    );
    
    // 5. Return structured response
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

### API Endpoints

| Endpoint | Method | Purpose | Input | Output |
|----------|--------|---------|-------|--------|
| `/api/ai/predictions` | POST | ESG forecasting | ESG scores | 6-month predictions, scenarios |
| `/api/ai/alerts` | POST | Anomaly detection | ESG metrics | Alerts, insights, recommendations |
| `/api/ai/blockchain` | POST | Blockchain analysis | Transactions | Anomalies, security, carbon verification |
| `/api/ai/reports` | POST | Report intelligence | Report data | Executive summary, insights, risks |
| `/api/ai/tasks` | POST | Task optimization | Tasks, action type | Prioritization, generation, portfolio analysis |

---

## AI Integration Layer

### OpenAI GPT-4 Integration Pattern

```typescript
// Shared AI Service Pattern
interface AIRequest {
  context: ESGData;
  prompt: string;
  responseSchema: JSONSchema;
}

interface AIResponse<T> {
  data: T;
  confidence: number;
  metadata: {
    model: string;
    tokensUsed: number;
    responseTime: number;
  };
}

async function callOpenAI<T>(request: AIRequest): Promise<AIResponse<T>> {
  const startTime = Date.now();
  
  const completion = await openai.chat.completions.create({
    model: 'gpt-4o-mini',
    messages: [
      {
        role: 'system',
        content: request.prompt
      },
      {
        role: 'user',
        content: JSON.stringify(request.context)
      }
    ],
    response_format: { type: 'json_object' },
    temperature: 0.7
  });
  
  const result = JSON.parse(completion.choices[0].message.content || '{}');
  
  return {
    data: result as T,
    confidence: calculateConfidence(result),
    metadata: {
      model: 'gpt-4o-mini',
      tokensUsed: completion.usage?.total_tokens || 0,
      responseTime: Date.now() - startTime
    }
  };
}
```

### AI Module Responsibilities

#### 1. **ESG Predictions** (`/api/ai/predictions`)
- **Input**: Current ESG scores (environmental, social, governance)
- **Processing**: 
  - Analyzes historical trends
  - Applies machine learning patterns
  - Considers industry benchmarks
- **Output**:
  - 6-month forecasts with confidence intervals
  - Optimistic/realistic/pessimistic scenarios
  - Key insights with probability scores

#### 2. **Alerts & Anomaly Detection** (`/api/ai/alerts`)
- **Input**: Real-time ESG metrics, historical data
- **Processing**:
  - Statistical anomaly detection
  - Pattern recognition
  - Predictive modeling
- **Output**:
  - Categorized alerts (critical/high/medium/low)
  - Anomaly descriptions with confidence scores
  - Actionable recommendations

#### 3. **Blockchain Intelligence** (`/api/ai/blockchain`)
- **Input**: Blockchain transactions, carbon credits
- **Processing**:
  - Transaction pattern analysis
  - Smart contract vulnerability scanning
  - Carbon credit verification
- **Output**:
  - Transaction anomalies
  - Security assessments
  - Carbon credit verification status

#### 4. **Report Analysis** (`/api/ai/reports`)
- **Input**: Generated compliance reports
- **Processing**:
  - Content analysis
  - Compliance gap detection
  - Risk assessment
- **Output**:
  - Executive summaries
  - Key insights with confidence scores
  - Compliance scores
  - Risk matrices

#### 5. **Task Intelligence** (`/api/ai/tasks`)
- **Input**: Current tasks, ESG context
- **Processing**:
  - Priority optimization algorithms
  - Workload analysis
  - Gap identification
- **Output**:
  - Prioritized task recommendations
  - Auto-generated tasks
  - Portfolio health metrics

---

## Data Flow

### 1. User Interaction → AI Analysis Flow

```
User Action
    ↓
Client Component (React)
    ↓
API Call (fetch to /api/ai/*)
    ↓
Next.js API Route
    ↓
Read localStorage Context
    ↓
OpenAI API Call (GPT-4)
    ↓
Parse & Validate Response
    ↓
Return to Client
    ↓
Update UI State
    ↓
Render Results
```

### 2. Data Persistence Flow

```
User Input
    ↓
Form Validation (React Hook Form + Zod)
    ↓
State Update (useState)
    ↓
localStorage.setItem()
    ↓
Storage Event Listener (cross-tab sync)
    ↓
UI Re-render
```

### 3. Real-time Updates Flow

```
Component Mount
    ↓
useEffect Hook
    ↓
setInterval (15s for notifications, 30s for data refresh)
    ↓
Check for Updates (localStorage + AI predictions)
    ↓
State Update if Changed
    ↓
UI Re-render
    ↓
Cleanup on Unmount
```

---

## Component Structure

### Key Components

#### 1. **Dashboard Components**

```typescript
// ESGMetricsPanel.tsx
interface ESGMetrics {
  environmental: number;
  social: number;
  governance: number;
  overall: number;
}

export function ESGMetricsPanel({ metrics }: { metrics: ESGMetrics }) {
  // Renders: Score cards, trend charts, drill-down analytics
}
```

#### 2. **AI Components**

```typescript
// ESGPredictionsPanel.tsx
interface Prediction {
  month: string;
  environmental: number;
  social: number;
  governance: number;
  confidence: number;
}

export function ESGPredictionsPanel() {
  const [predictions, setPredictions] = useState<Prediction[]>([]);
  const [loading, setLoading] = useState(false);
  
  const fetchPredictions = async () => {
    const esgData = JSON.parse(localStorage.getItem('esgData') || '{}');
    const response = await fetch('/api/ai/predictions', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ data: esgData })
    });
    const result = await response.json();
    setPredictions(result.predictions);
  };
  
  // Renders: Predictions chart, scenarios, insights
}
```

#### 3. **Security Components**

```typescript
// AIThreatDetection.tsx
interface Threat {
  id: string;
  type: string;
  severity: 'critical' | 'high' | 'medium' | 'low';
  confidence: number;
  description: string;
  mitigation: string;
}

export function AIThreatDetection() {
  const [threats, setThreats] = useState<Threat[]>([]);
  // Real-time threat monitoring logic
}
```

---

## State Management

### Strategy: **Local State + localStorage**

```typescript
// Pattern: Persistent State Hook
function usePersistentState<T>(
  key: string,
  initialValue: T
): [T, (value: T) => void] {
  const [state, setState] = useState<T>(() => {
    if (typeof window === 'undefined') return initialValue;
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });
  
  const setPersistentState = (value: T) => {
    setState(value);
    localStorage.setItem(key, JSON.stringify(value));
  };
  
  return [state, setPersistentState];
}

// Usage
const [esgData, setESGData] = usePersistentState('esgData', defaultESGData);
```

### State Organization

```typescript
// localStorage keys
const STORAGE_KEYS = {
  ESG_DATA: 'esgData',
  SECURITY_DATA: 'securityData',
  BLOCKCHAIN_DATA: 'blockchainData',
  TASKS: 'tasks',
  REPORTS: 'reports',
  USER_PREFERENCES: 'userPreferences'
};

// Type-safe storage
interface StorageData {
  esgData: ESGData;
  securityData: SecurityData;
  blockchainData: BlockchainData;
  tasks: Task[];
  reports: Report[];
  userPreferences: UserPreferences;
}
```

---

## Security Architecture

### 1. **API Security**

```typescript
// API Route Protection (future enhancement)
import { rateLimit } from '@/lib/rate-limit';

export async function POST(request: NextRequest) {
  // Rate limiting
  const identifier = request.ip || 'anonymous';
  const { success } = await rateLimit.check(identifier);
  
  if (!success) {
    return NextResponse.json(
      { error: 'Too many requests' },
      { status: 429 }
    );
  }
  
  // Request validation
  const body = await request.json();
  const validated = validateSchema(body);
  
  // Process request...
}
```

### 2. **Data Validation**

```typescript
import { z } from 'zod';

// Schema validation
const ESGDataSchema = z.object({
  environmental: z.number().min(0).max(100),
  social: z.number().min(0).max(100),
  governance: z.number().min(0).max(100)
});

// Usage
const validated = ESGDataSchema.parse(userInput);
```

### 3. **XSS Prevention**

- All user inputs sanitized
- React's built-in XSS protection (JSX auto-escaping)
- Content Security Policy headers (future)

### 4. **API Key Management**

**Current (Prototype):**
```typescript
// Hardcoded in API routes (NOT for production)
const openai = new OpenAI({
  apiKey: 'sk-proj-...'
});
```

**Production (Recommended):**
```typescript
// Environment variables
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});
```

---

## Performance Optimization

### 1. **Code Splitting**

```typescript
// Dynamic imports for heavy components
import dynamic from 'next/dynamic';

const ESGPredictionsPanel = dynamic(
  () => import('@/components/ai/ESGPredictionsPanel'),
  { loading: () => <LoadingSpinner /> }
);
```

### 2. **Memoization**

```typescript
import { useMemo, useCallback } from 'react';

function ESGDashboard({ data }: { data: ESGData[] }) {
  // Expensive calculations cached
  const processedData = useMemo(() => {
    return data.map(item => complexCalculation(item));
  }, [data]);
  
  // Callbacks memoized
  const handleRefresh = useCallback(() => {
    fetchData();
  }, []);
  
  return <Chart data={processedData} onRefresh={handleRefresh} />;
}
```

### 3. **Image Optimization**

```typescript
import Image from 'next/image';

<Image
  src="/logo.png"
  alt="RANTAI Sentinel"
  width={200}
  height={50}
  priority // Above-the-fold images
/>
```

### 4. **API Response Caching**

```typescript
// Client-side caching
const CACHE_DURATION = 5 * 60 * 1000; // 5 minutes

function useCachedAI(endpoint: string) {
  const [cached, setCached] = useState<{
    data: any;
    timestamp: number;
  } | null>(null);
  
  const fetchWithCache = async () => {
    if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
      return cached.data;
    }
    
    const response = await fetch(endpoint);
    const data = await response.json();
    setCached({ data, timestamp: Date.now() });
    return data;
  };
  
  return fetchWithCache;
}
```

---

## Scalability Considerations

### Current Architecture (Phase 1)
- **Client-side data**: localStorage (max ~10MB per domain)
- **Serverless functions**: Next.js API routes (stateless)
- **AI calls**: Direct OpenAI API (no caching layer)

### Scalability Roadmap

#### Phase 2: Database Integration
```
localStorage → PostgreSQL / MongoDB
- User authentication
- Multi-user support
- Historical data tracking
- Advanced queries
```

#### Phase 3: Caching Layer
```
Direct API → Redis Cache → OpenAI
- Reduce API costs
- Faster response times
- Request deduplication
```

#### Phase 4: Microservices
```
Monolithic API Routes → Separate Services
- AI Service (predictions, analysis)
- Blockchain Service (verification, audits)
- Reporting Service (generation, storage)
- Notification Service (real-time alerts)
```

#### Phase 5: Real-time Infrastructure
```
Polling → WebSockets / Server-Sent Events
- Live updates
- Multi-user collaboration
- Real-time notifications
```

### Horizontal Scaling Strategy

```
Load Balancer (Vercel Edge)
    ↓
├── Region 1 (US East)
│   ├── Next.js Instance 1
│   ├── Next.js Instance 2
│   └── Next.js Instance N
│
├── Region 2 (EU West)
│   ├── Next.js Instance 1
│   └── Next.js Instance N
│
└── Region 3 (Asia Pacific)
    ├── Next.js Instance 1
    └── Next.js Instance N
```

---

## Deployment Architecture

### Current: Vercel Edge Network

```
Git Push (GitHub)
    ↓
Vercel Build
    ↓
Edge Network Deployment (Global CDN)
    ↓
Automatic HTTPS
    ↓
Production: sentinel.elpeef.com
```

### Build Process

```bash
# Build optimization
next build
# Output:
# - Static pages: Pre-rendered HTML
# - API routes: Serverless functions
# - Client bundles: Code-split JavaScript
# - Assets: Optimized images, fonts
```

### Environment-Specific Builds

| Environment | URL | Purpose |
|-------------|-----|---------|
| **Development** | localhost:3000 | Local development |
| **Preview** | vercel-preview-*.vercel.app | PR previews |
| **Production** | sentinel.elpeef.com | Live application |

---

## Monitoring & Observability

### Future Enhancements

#### 1. **Error Tracking**
```typescript
// Sentry integration
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  tracesSampleRate: 1.0
});
```

#### 2. **Performance Monitoring**
```typescript
// Web Vitals
export function reportWebVitals(metric: any) {
  console.log(metric); // LCP, FID, CLS, FCP, TTFB
}
```

#### 3. **Analytics**
```typescript
// PostHog / Google Analytics
analytics.track('ai_prediction_generated', {
  model: 'gpt-4o-mini',
  responseTime: 1234,
  confidence: 0.92
});
```

---

## Technology Decision Rationale

### Why Next.js 15?
- **App Router**: Modern routing with layouts, streaming SSR
- **Server Components**: Reduced client bundle size
- **API Routes**: Serverless backend without separate server
- **Edge Runtime**: Global, low-latency deployments
- **Image Optimization**: Automatic WebP/AVIF conversion
- **TypeScript First**: Native TypeScript support

### Why OpenAI GPT-4?
- **Industry Leading**: State-of-the-art language model
- **Structured Outputs**: JSON mode for predictable responses
- **Context Window**: 128k tokens for comprehensive analysis
- **Reliability**: 99.9% uptime SLA
- **Developer Experience**: Simple API, extensive documentation

### Why Tailwind CSS?
- **Rapid Development**: Utility-first approach
- **Consistency**: Design system baked into classes
- **Performance**: Purged unused CSS in production
- **Responsive**: Mobile-first breakpoints
- **Dark Mode**: Built-in dark mode support

### Why localStorage?
- **Prototype Speed**: No backend setup required
- **Offline Capability**: Works without network
- **Zero Latency**: Instant reads/writes
- **Privacy**: Data stays on user device
- **Migration Path**: Easy to migrate to database later

---

## Future Architecture Evolution

### Planned Enhancements

#### Q2 2025: Backend Infrastructure
```
Current: localStorage
    ↓
Next: PostgreSQL + Prisma ORM
    ↓
Features:
  - Multi-user authentication (NextAuth.js)
  - Role-based access control
  - Historical data tracking
  - Advanced querying
```

#### Q3 2025: Real-time Collaboration
```
Current: Single-user
    ↓
Next: SpacetimeDB / Supabase Realtime
    ↓
Features:
  - Live multi-user editing
  - Presence indicators
  - Conflict resolution
  - Activity feeds
```

#### Q4 2025: Advanced AI
```
Current: GPT-4o-mini
    ↓
Next: Multi-model ensemble
    ↓
Models:
  - GPT-4 Turbo (general analysis)
  - Claude 3.5 Sonnet (compliance review)
  - Gemini Pro (data processing)
  - Custom fine-tuned models (ESG-specific)
```

#### 2026: Blockchain Integration
```
Current: Mock blockchain data
    ↓
Next: Live blockchain APIs
    ↓
Integrations:
  - Ethereum (smart contract audits)
  - Polygon (carbon credits)
  - Base (transaction verification)
  - IPFS (immutable storage)
```

---

## Contact & Support

**Technical Questions:**
- **Email**: [support@elpeef.com](mailto:support@elpeef.com)
- **Telegram**: [@khudriakhmad](https://t.me/khudriakhmad)
- **Discord**: @khudri_61362

**Repository:**
- **GitHub**: [https://github.com/mrbrightsides/sentinel](https://github.com/mrbrightsides/sentinel)

**Live Demo:**
- **Production**: [https://sentinel.elpeef.com/](https://sentinel.elpeef.com/)

---

## License

MIT License - See [LICENSE](./LICENSE) for details

---

**Last Updated**: January 2025  
**Version**: 2.0.0  
**Maintained by**: RANTAI Sentinel Team
