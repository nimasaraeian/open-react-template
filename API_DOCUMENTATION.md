# API Documentation - Open Next.js Template

![Open React / Next.js template](https://github.com/user-attachments/assets/522a5e46-2a0e-48ca-80eb-87c7fa58f3ea)

## Table of Contents

1. [Overview](#overview)
2. [API Routes](#api-routes)
3. [React Components](#react-components)
4. [Utility Hooks](#utility-hooks)
5. [Layout Components](#layout-components)
6. [Page Components](#page-components)
7. [Usage Examples](#usage-examples)
8. [Configuration](#configuration)

## Overview

Open is a free React/Next.js landing page template built with Tailwind CSS. This documentation covers all public APIs, functions, and components available in the template.

**Tech Stack:**
- Next.js 15.1.6 with App Router
- React 19.0.0
- TypeScript 5.7.3
- Tailwind CSS 4.0.3
- Headless UI 2.2.0
- AOS (Animate On Scroll) 3.0.0-beta.6

## API Routes

### `/api/hello`

A simple GET endpoint that returns a greeting message.

**Endpoint:** `GET /api/hello`

**Response:**
```
Hello, Next.js!
```

**Example Usage:**
```javascript
const response = await fetch('/api/hello');
const message = await response.text();
console.log(message); // "Hello, Next.js!"
```

**Location:** `app/api/hello/route.ts`

## React Components

### Core Components

#### HeroHome

The main hero section component for the homepage.

**Props:** None

**Features:**
- Animated gradient text effect
- Call-to-action buttons
- Integrated video modal
- AOS animations

**Usage:**
```tsx
import HeroHome from '@/components/hero-home';

export default function HomePage() {
  return <HeroHome />;
}
```

**Location:** `components/hero-home.tsx`

---

#### ModalVideo

A reusable modal component for displaying videos with a thumbnail preview.

**Props:**
```typescript
interface ModalVideoProps {
  thumb: StaticImageData;        // Thumbnail image
  thumbWidth: number;            // Thumbnail width in pixels
  thumbHeight: number;           // Thumbnail height in pixels
  thumbAlt: string;             // Alt text for thumbnail
  video: string;                // Video file path
  videoWidth: number;           // Video width in pixels
  videoHeight: number;          // Video height in pixels
}
```

**Features:**
- Click-to-play video modal
- Responsive design
- Accessibility support
- Auto-focus management

**Usage:**
```tsx
import ModalVideo from '@/components/modal-video';
import VideoThumb from '@/public/images/hero-image-01.jpg';

<ModalVideo
  thumb={VideoThumb}
  thumbWidth={1104}
  thumbHeight={576}
  thumbAlt="Modal video thumbnail"
  video="videos/video.mp4"
  videoWidth={1920}
  videoHeight={1080}
/>
```

**Location:** `components/modal-video.tsx`

---

#### Spotlight

An interactive component that creates a spotlight effect following the mouse cursor.

**Props:**
```typescript
interface SpotlightProps {
  children: React.ReactNode;
  className?: string;           // Optional CSS classes
}
```

**Features:**
- Mouse tracking spotlight effect
- CSS custom properties for positioning
- Responsive behavior
- Performance optimized

**Usage:**
```tsx
import Spotlight from '@/components/spotlight';

<Spotlight className="grid gap-6 lg:grid-cols-3">
  <div>Card 1</div>
  <div>Card 2</div>
  <div>Card 3</div>
</Spotlight>
```

**Location:** `components/spotlight.tsx`

---

#### PageIllustration

Background illustration component for pages.

**Props:**
```typescript
interface PageIllustrationProps {
  multiple?: boolean;           // Show multiple illustrations
}
```

**Features:**
- Single or multiple illustration modes
- Absolute positioning
- Responsive design

**Usage:**
```tsx
import PageIllustration from '@/components/page-illustration';

// Single illustration
<PageIllustration />

// Multiple illustrations
<PageIllustration multiple={true} />
```

**Location:** `components/page-illustration.tsx`

---

#### Features

Main features showcase component with animated elements.

**Props:** None

**Features:**
- Animated gradient backgrounds
- Responsive grid layout
- Feature cards with descriptions
- Background illustrations

**Usage:**
```tsx
import Features from '@/components/features';

export default function FeaturesPage() {
  return <Features />;
}
```

**Location:** `components/features.tsx`

---

#### Workflows

Component showcasing workflow cards with spotlight effects.

**Props:** None

**Features:**
- Spotlight interactive cards
- Gradient backgrounds
- Workflow step visualization
- Responsive design

**Usage:**
```tsx
import Workflows from '@/components/workflows';

export default function WorkflowsSection() {
  return <Workflows />;
}
```

**Location:** `components/workflows.tsx`

---

#### Testimonials

Dynamic testimonials component with filtering and masonry layout.

**Props:** None

**Features:**
- Category-based filtering
- Masonry layout using custom hook
- Client logos and testimonials
- Responsive design

**Usage:**
```tsx
import Testimonials from '@/components/testimonials';

export default function TestimonialsSection() {
  return <Testimonials />;
}
```

**Location:** `components/testimonials.tsx`

---

#### Cta (Call to Action)

Simple call-to-action section component.

**Props:** None

**Features:**
- Animated gradient text
- Action buttons
- Background effects

**Usage:**
```tsx
import Cta from '@/components/cta';

export default function CTASection() {
  return <Cta />;
}
```

**Location:** `components/cta.tsx`

## Utility Hooks

### useMasonry

Custom hook for creating masonry layouts by dynamically adjusting item positions.

**Returns:** `React.RefObject<HTMLDivElement>`

**Features:**
- Automatic item positioning
- Responsive behavior
- Gap size calculation
- Window resize handling

**Usage:**
```tsx
import useMasonry from '@/utils/useMasonry';

export default function MasonryComponent() {
  const masonryContainer = useMasonry();

  return (
    <div ref={masonryContainer} className="grid grid-cols-3 gap-4">
      <div>Item 1</div>
      <div>Item 2</div>
      <div>Item 3</div>
    </div>
  );
}
```

**Location:** `utils/useMasonry.tsx`

---

### useMousePosition

Hook for tracking mouse position coordinates.

**Returns:**
```typescript
interface MousePosition {
  x: number;
  y: number;
}
```

**Features:**
- Real-time mouse tracking
- Event listener cleanup
- Performance optimized

**Usage:**
```tsx
import useMousePosition from '@/utils/useMousePosition';

export default function MouseTracker() {
  const mousePosition = useMousePosition();

  return (
    <div>
      Mouse position: {mousePosition.x}, {mousePosition.y}
    </div>
  );
}
```

**Location:** `utils/useMousePosition.tsx`

## Layout Components

### Header

Navigation header component with authentication links.

**Props:** None

**Features:**
- Logo integration
- Sign in/Register buttons
- Responsive design
- Gradient backgrounds

**Usage:**
```tsx
import Header from '@/components/ui/header';

export default function Layout() {
  return (
    <>
      <Header />
      {/* Page content */}
    </>
  );
}
```

**Location:** `components/ui/header.tsx`

---

### Footer

Comprehensive footer component with multiple sections.

**Props:** None

**Features:**
- Multi-column layout
- Navigation links
- Social media links
- Newsletter signup
- Client logos

**Usage:**
```tsx
import Footer from '@/components/ui/footer';

export default function Layout() {
  return (
    <>
      {/* Page content */}
      <Footer />
    </>
  );
}
```

**Location:** `components/ui/footer.tsx`

---

### Logo

Simple logo component with link functionality.

**Props:** None

**Features:**
- SVG logo integration
- Next.js Link component
- Accessibility attributes

**Usage:**
```tsx
import Logo from '@/components/ui/logo';

export default function Navigation() {
  return (
    <nav>
      <Logo />
      {/* Other nav items */}
    </nav>
  );
}
```

**Location:** `components/ui/logo.tsx`

## Page Components

### Authentication Pages

#### Sign In Page
**Location:** `app/(auth)/signin/page.tsx`
- Email/password form
- Social login options
- Form validation

#### Sign Up Page
**Location:** `app/(auth)/signup/page.tsx`
- Registration form
- Terms acceptance
- Email verification

#### Reset Password Page
**Location:** `app/(auth)/reset-password/page.tsx`
- Password reset form
- Email input
- Success confirmation

### Home Page
**Location:** `app/(default)/page.tsx`
- Hero section
- Features showcase
- Testimonials
- Call-to-action

## Usage Examples

### Creating a New Landing Page

```tsx
import PageIllustration from '@/components/page-illustration';
import HeroHome from '@/components/hero-home';
import Features from '@/components/features';
import Testimonials from '@/components/testimonials';
import Cta from '@/components/cta';

export default function LandingPage() {
  return (
    <main className="grow">
      <PageIllustration />
      <HeroHome />
      <Features />
      <Testimonials />
      <Cta />
    </main>
  );
}
```

### Using Interactive Components

```tsx
import Spotlight from '@/components/spotlight';
import useMousePosition from '@/utils/useMousePosition';

export default function InteractiveSection() {
  const mousePosition = useMousePosition();

  return (
    <Spotlight className="grid grid-cols-3 gap-6">
      <div className="card">
        Content 1
        <p>Mouse: {mousePosition.x}, {mousePosition.y}</p>
      </div>
      <div className="card">Content 2</div>
      <div className="card">Content 3</div>
    </Spotlight>
  );
}
```

### Creating Masonry Layouts

```tsx
import useMasonry from '@/utils/useMasonry';

export default function Gallery() {
  const masonryContainer = useMasonry();

  return (
    <div 
      ref={masonryContainer}
      className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
      style={{ gridRowGap: '24px' }}
    >
      {items.map((item, index) => (
        <div key={index} className="break-inside-avoid">
          {item.content}
        </div>
      ))}
    </div>
  );
}
```

## Configuration

### Font Configuration

The template uses two fonts configured in `app/layout.tsx`:

- **Inter**: Google Font for body text
- **Nacelle**: Local font for headings

```typescript
const inter = Inter({
  subsets: ["latin"],
  variable: "--font-inter",
  display: "swap",
});

const nacelle = localFont({
  src: [
    {
      path: "../public/fonts/nacelle-regular.woff2",
      weight: "400",
      style: "normal",
    },
    // ... other font variants
  ],
  variable: "--font-nacelle",
  display: "swap",
});
```

### Tailwind CSS Configuration

The template uses Tailwind CSS v4.0.3 with custom configurations for:
- Colors (gray scale, indigo palette)
- Typography (font families)
- Animations (gradient animations)
- Custom utilities

### AOS (Animate On Scroll) Integration

Components use AOS attributes for scroll animations:

```tsx
<div 
  data-aos="fade-up"
  data-aos-delay={200}
>
  Content
</div>
```

Common AOS values used:
- `fade-up`: Fade in from bottom
- `fade-down`: Fade in from top
- `fade-left`: Fade in from right
- `fade-right`: Fade in from left

## Development Guidelines

### Component Structure
1. Use TypeScript for all components
2. Define proper prop interfaces
3. Include JSDoc comments for complex functions
4. Follow Next.js App Router conventions

### Styling Guidelines
1. Use Tailwind CSS classes
2. Implement responsive design (`sm:`, `md:`, `lg:` prefixes)
3. Use CSS custom properties for dynamic values
4. Follow the established color scheme

### Performance Considerations
1. Use `next/image` for optimized images
2. Implement proper loading states
3. Clean up event listeners in useEffect
4. Use React.memo for expensive components when needed

---

For more information about the template, visit the [GitHub repository](https://github.com/cruip/open-react-template) or check out the [live demo](https://open.cruip.com/).