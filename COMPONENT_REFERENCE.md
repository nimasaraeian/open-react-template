# Component Reference Guide

This guide provides detailed information about each component in the Open Next.js template, including implementation details, customization options, and advanced usage patterns.

## Table of Contents

1. [Component Architecture](#component-architecture)
2. [Interactive Components](#interactive-components)
3. [Layout Components](#layout-components)
4. [Content Components](#content-components)
5. [Utility Components](#utility-components)
6. [Styling Guide](#styling-guide)
7. [Animation Patterns](#animation-patterns)
8. [Customization Examples](#customization-examples)

## Component Architecture

### Design Principles

The Open template follows these design principles:

1. **Modularity**: Each component is self-contained and reusable
2. **Accessibility**: Components include proper ARIA attributes and semantic HTML
3. **Performance**: Optimized for loading speed and runtime performance
4. **Responsiveness**: Mobile-first design with progressive enhancement
5. **TypeScript**: Full type safety throughout the codebase

### File Structure
```
components/
├── ui/                 # Layout and navigation components
│   ├── header.tsx
│   ├── footer.tsx
│   └── logo.tsx
├── hero-home.tsx       # Landing page hero section
├── modal-video.tsx     # Video modal component
├── spotlight.tsx       # Interactive spotlight effect
├── page-illustration.tsx # Background illustrations
├── features.tsx        # Features showcase
├── workflows.tsx       # Workflow demonstration
├── testimonials.tsx    # Customer testimonials
└── cta.tsx            # Call-to-action sections
```

## Interactive Components

### Spotlight Component

The Spotlight component creates an interactive mouse-following effect using CSS custom properties.

#### Implementation Details

```typescript
interface SpotlightProps {
  children: React.ReactNode;
  className?: string;
}

export default function Spotlight({ children, className = "" }: SpotlightProps)
```

#### Key Features

1. **Mouse Tracking**: Uses `useMousePosition` hook for real-time tracking
2. **CSS Custom Properties**: Sets `--mouse-x` and `--mouse-y` variables
3. **Performance Optimization**: Bounds checking to prevent unnecessary calculations
4. **Container Management**: Tracks container dimensions for accurate positioning

#### Customization Options

**Basic Usage:**
```tsx
<Spotlight className="grid grid-cols-3 gap-6">
  <div className="spotlight-card">Card 1</div>
  <div className="spotlight-card">Card 2</div>
  <div className="spotlight-card">Card 3</div>
</Spotlight>
```

**Custom Spotlight Effects:**
```css
.spotlight-card {
  @apply relative overflow-hidden rounded-lg bg-gray-800 p-6;
  
  &::before {
    content: '';
    position: absolute;
    width: 200px;
    height: 200px;
    background: radial-gradient(circle, rgba(99, 102, 241, 0.3) 0%, transparent 70%);
    transform: translate(var(--mouse-x, 0), var(--mouse-y, 0)) translate(-50%, -50%);
    pointer-events: none;
    transition: opacity 0.3s ease;
    opacity: 0;
  }
  
  &:hover::before {
    opacity: 1;
  }
}
```

#### Advanced Configuration

**Multi-layer Effects:**
```tsx
<Spotlight className="relative">
  {children}
  <div className="spotlight-overlay" />
  <div className="spotlight-glow" />
</Spotlight>
```

### ModalVideo Component

Accessible video modal with thumbnail preview and full-screen playback.

#### Props Interface

```typescript
interface ModalVideoProps {
  thumb: StaticImageData;     // Thumbnail image import
  thumbWidth: number;         // Thumbnail display width
  thumbHeight: number;        // Thumbnail display height
  thumbAlt: string;          // Accessibility description
  video: string;             // Video file path (relative to public/)
  videoWidth: number;        // Video natural width
  videoHeight: number;       // Video natural height
}
```

#### Implementation Features

1. **Headless UI Integration**: Uses Dialog components for accessibility
2. **Focus Management**: Automatic focus on video element when opened
3. **Keyboard Navigation**: ESC key closes modal
4. **Responsive Design**: Adapts to different screen sizes
5. **Video Controls**: Native HTML5 video controls

#### Customization Examples

**Custom Thumbnail Overlay:**
```tsx
<ModalVideo
  thumb={VideoThumb}
  thumbWidth={1104}
  thumbHeight={576}
  thumbAlt="Product demo video"
  video="videos/product-demo.mp4"
  videoWidth={1920}
  videoHeight={1080}
/>

<style jsx>{`
  .video-thumbnail::after {
    content: 'Watch Demo';
    position: absolute;
    bottom: 20px;
    left: 20px;
    color: white;
    font-weight: 600;
  }
`}</style>
```

**Multiple Video Support:**
```tsx
const videos = [
  { id: 1, title: "Introduction", src: "intro.mp4" },
  { id: 2, title: "Features", src: "features.mp4" },
  { id: 3, title: "Tutorial", src: "tutorial.mp4" }
];

{videos.map(video => (
  <ModalVideo
    key={video.id}
    thumb={getVideoThumbnail(video.id)}
    thumbWidth={400}
    thumbHeight={225}
    thumbAlt={`${video.title} video`}
    video={`videos/${video.src}`}
    videoWidth={1920}
    videoHeight={1080}
  />
))}
```

## Layout Components

### Header Component

Navigation header with authentication links and responsive design.

#### Structure Analysis

```tsx
export default function Header() {
  return (
    <header className="z-30 mt-2 w-full md:mt-5">
      <div className="mx-auto max-w-6xl px-4 sm:px-6">
        <div className="nav-container">
          <Logo />
          <NavigationLinks />
        </div>
      </div>
    </header>
  );
}
```

#### Styling Breakdown

1. **Glass Effect**: Uses backdrop blur and gradient borders
2. **Responsive Spacing**: Adapts margins for mobile and desktop
3. **Z-Index Management**: Ensures header stays above content
4. **Container Width**: Matches site-wide max-width pattern

#### Customization Options

**Adding Navigation Items:**
```tsx
// Create enhanced header with navigation menu
export default function EnhancedHeader() {
  return (
    <header className="z-30 mt-2 w-full md:mt-5">
      <div className="mx-auto max-w-6xl px-4 sm:px-6">
        <div className="relative flex h-14 items-center justify-between gap-3 rounded-2xl bg-gray-900/90 px-3">
          <Logo />
          
          {/* Navigation Menu */}
          <nav className="hidden md:flex items-center space-x-6">
            <Link href="/features" className="text-gray-300 hover:text-white">
              Features
            </Link>
            <Link href="/pricing" className="text-gray-300 hover:text-white">
              Pricing
            </Link>
            <Link href="/docs" className="text-gray-300 hover:text-white">
              Docs
            </Link>
          </nav>
          
          {/* Auth Links */}
          <div className="flex items-center gap-3">
            <Link href="/signin">Sign In</Link>
            <Link href="/signup">Register</Link>
          </div>
        </div>
      </div>
    </header>
  );
}
```

**Mobile Menu Integration:**
```tsx
import { useState } from 'react';
import { Bars3Icon, XMarkIcon } from '@heroicons/react/24/outline';

export default function HeaderWithMobileMenu() {
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false);
  
  return (
    <header>
      {/* Desktop Header */}
      <div className="hidden md:block">
        {/* Standard header content */}
      </div>
      
      {/* Mobile Header */}
      <div className="md:hidden">
        <button onClick={() => setMobileMenuOpen(!mobileMenuOpen)}>
          {mobileMenuOpen ? <XMarkIcon /> : <Bars3Icon />}
        </button>
      </div>
      
      {/* Mobile Menu */}
      {mobileMenuOpen && (
        <div className="md:hidden">
          {/* Mobile navigation items */}
        </div>
      )}
    </header>
  );
}
```

### Footer Component

Comprehensive footer with multiple sections and responsive layout.

#### Section Structure

The footer is organized into several main sections:

1. **Product Links**: Feature and product-related navigation
2. **Company Links**: About, careers, contact information
3. **Resources**: Documentation, support, community
4. **Legal**: Privacy policy, terms of service
5. **Newsletter**: Email subscription form
6. **Social Links**: Social media presence

#### Responsive Behavior

```css
.footer-grid {
  /* Mobile: 2 columns */
  grid-template-columns: repeat(2, 1fr);
  
  /* Tablet: 4 columns */
  @media (min-width: 768px) {
    grid-template-columns: repeat(4, 1fr);
  }
  
  /* Desktop: Custom column sizes */
  @media (min-width: 1024px) {
    grid-template-columns: repeat(4, minmax(0, 140px)) 1fr;
  }
}
```

#### Customization Examples

**Adding New Footer Section:**
```tsx
// Add a new "Developers" section
<div className="space-y-2">
  <h3 className="text-sm font-medium text-gray-200">Developers</h3>
  <ul className="space-y-2 text-sm">
    <li><Link href="/api">API Reference</Link></li>
    <li><Link href="/sdk">SDK</Link></li>
    <li><Link href="/webhooks">Webhooks</Link></li>
    <li><Link href="/changelog">Changelog</Link></li>
  </ul>
</div>
```

**Newsletter Integration:**
```tsx
import { useState } from 'react';

export function NewsletterSignup() {
  const [email, setEmail] = useState('');
  const [status, setStatus] = useState('idle');
  
  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setStatus('loading');
    
    try {
      await fetch('/api/newsletter', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email })
      });
      setStatus('success');
    } catch (error) {
      setStatus('error');
    }
  };
  
  return (
    <form onSubmit={handleSubmit} className="space-y-3">
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Enter your email"
        required
      />
      <button type="submit" disabled={status === 'loading'}>
        {status === 'loading' ? 'Subscribing...' : 'Subscribe'}
      </button>
    </form>
  );
}
```

## Content Components

### Features Component

Showcase component for highlighting product features with interactive elements.

#### Content Structure

The Features component typically includes:

1. **Section Header**: Title, subtitle, and decorative elements
2. **Feature Grid**: Responsive grid layout for feature cards
3. **Feature Cards**: Individual feature descriptions with icons
4. **Background Elements**: Decorative illustrations and gradients

#### Data-Driven Approach

```tsx
interface Feature {
  icon: React.ReactNode;
  title: string;
  description: string;
  link?: string;
}

const features: Feature[] = [
  {
    icon: <IconComponent />,
    title: "Advanced Analytics",
    description: "Deep insights into user behavior and performance metrics.",
    link: "/analytics"
  },
  // ... more features
];

export default function Features() {
  return (
    <section>
      <div className="feature-grid">
        {features.map((feature, index) => (
          <FeatureCard key={index} {...feature} />
        ))}
      </div>
    </section>
  );
}
```

#### Animation Integration

```tsx
export function FeatureCard({ icon, title, description, index }: FeatureCardProps) {
  return (
    <div 
      className="feature-card"
      data-aos="fade-up"
      data-aos-delay={index * 100}
    >
      <div className="feature-icon">{icon}</div>
      <h3 className="feature-title">{title}</h3>
      <p className="feature-description">{description}</p>
    </div>
  );
}
```

### Testimonials Component

Dynamic testimonials with filtering and masonry layout.

#### Data Structure

```typescript
interface Testimonial {
  img: StaticImageData;
  clientImg: StaticImageData;
  name: string;
  company: string;
  content: string;
  categories: number[];
}

interface Category {
  id: number;
  name: string;
}
```

#### Filtering Logic

```tsx
const [activeCategory, setActiveCategory] = useState(0);

const filteredTestimonials = activeCategory === 0 
  ? testimonials 
  : testimonials.filter(testimonial => 
      testimonial.categories.includes(activeCategory)
    );
```

#### Masonry Integration

```tsx
export default function Testimonials() {
  const masonryContainer = useMasonry();
  
  return (
    <div 
      ref={masonryContainer}
      className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
    >
      {filteredTestimonials.map((testimonial, index) => (
        <TestimonialCard key={index} {...testimonial} />
      ))}
    </div>
  );
}
```

## Utility Components

### Page Illustration

Background illustration component with configurable layouts.

#### Usage Patterns

```tsx
// Single illustration (default)
<PageIllustration />

// Multiple illustrations for complex layouts
<PageIllustration multiple={true} />

// Custom positioning
<PageIllustration 
  multiple={true}
  className="custom-illustration-position"
/>
```

#### Custom Illustrations

```tsx
interface PageIllustrationProps {
  multiple?: boolean;
  variant?: 'default' | 'hero' | 'contact' | 'about';
}

export default function PageIllustration({ 
  multiple = false, 
  variant = 'default' 
}: PageIllustrationProps) {
  const illustrations = {
    default: MainIllustration,
    hero: HeroIllustration,
    contact: ContactIllustration,
    about: AboutIllustration
  };
  
  return (
    <div className={`illustration-container illustration-${variant}`}>
      <Image src={illustrations[variant]} alt="Page illustration" />
      {multiple && <AdditionalIllustrations />}
    </div>
  );
}
```

## Styling Guide

### Color System

The template uses a consistent color system based on Tailwind CSS:

```css
:root {
  /* Gray Scale */
  --color-gray-50: #f9fafb;
  --color-gray-200: #e5e7eb;
  --color-gray-300: #d1d5db;
  --color-gray-600: #4b5563;
  --color-gray-700: #374151;
  --color-gray-800: #1f2937;
  --color-gray-900: #111827;
  --color-gray-950: #030712;
  
  /* Indigo Palette */
  --color-indigo-200: #c7d2fe;
  --color-indigo-300: #a5b4fc;
  --color-indigo-500: #6366f1;
  --color-indigo-600: #4f46e5;
}
```

### Typography Scale

```css
.font-nacelle {
  font-family: var(--font-nacelle);
}

.font-inter {
  font-family: var(--font-inter);
}

/* Heading Styles */
.text-4xl { font-size: 2.25rem; }
.text-3xl { font-size: 1.875rem; }
.text-xl { font-size: 1.25rem; }
.text-lg { font-size: 1.125rem; }
```

### Gradient Patterns

```css
/* Animated Gradient Text */
.animate-gradient {
  background: linear-gradient(
    to right,
    var(--color-gray-200),
    var(--color-indigo-200),
    var(--color-gray-50),
    var(--color-indigo-300),
    var(--color-gray-200)
  );
  background-size: 200% auto;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: gradient 6s linear infinite;
}

@keyframes gradient {
  to { background-position: 200% center; }
}

/* Button Gradients */
.btn-gradient {
  background: linear-gradient(to top, #4f46e5, #6366f1);
  background-size: 100% 100%;
  background-position: bottom;
  transition: background-size 0.3s ease;
}

.btn-gradient:hover {
  background-size: 100% 150%;
}
```

## Animation Patterns

### AOS (Animate On Scroll) Usage

Common animation patterns used throughout the template:

```tsx
// Fade up animations
<div data-aos="fade-up">Content</div>
<div data-aos="fade-up" data-aos-delay={200}>Delayed content</div>

// Staggered animations
{items.map((item, index) => (
  <div 
    key={index}
    data-aos="fade-up"
    data-aos-delay={index * 100}
  >
    {item.content}
  </div>
))}
```

### Custom CSS Animations

```css
/* Hover Effects */
.hover-lift {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.hover-lift:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
}

/* Loading States */
.loading-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
```

## Customization Examples

### Creating Custom Components

```tsx
// Custom hero section based on HeroHome
export function CustomHero({ 
  title, 
  subtitle, 
  primaryAction, 
  secondaryAction 
}: CustomHeroProps) {
  return (
    <section className="relative">
      <PageIllustration />
      
      <div className="hero-container">
        <h1 className="hero-title">{title}</h1>
        <p className="hero-subtitle">{subtitle}</p>
        
        <div className="hero-actions">
          <button className="btn-primary">{primaryAction}</button>
          <button className="btn-secondary">{secondaryAction}</button>
        </div>
      </div>
    </section>
  );
}
```

### Theme Customization

```tsx
// Custom theme provider
export function ThemeProvider({ children }: { children: React.ReactNode }) {
  return (
    <div className="theme-custom">
      {children}
    </div>
  );
}

// Custom CSS variables
.theme-custom {
  --color-primary: #8b5cf6;
  --color-secondary: #06b6d4;
  --color-accent: #f59e0b;
}
```

### Advanced Spotlight Effects

```tsx
// Multi-layered spotlight component
export function EnhancedSpotlight({ children, intensity = 1 }: SpotlightProps) {
  const mousePosition = useMousePosition();
  
  return (
    <div 
      className="enhanced-spotlight"
      style={{
        '--mouse-x': `${mousePosition.x}px`,
        '--mouse-y': `${mousePosition.y}px`,
        '--intensity': intensity
      }}
    >
      {children}
    </div>
  );
}
```

---

This component reference provides the foundation for understanding and customizing the Open template. Each component is designed to be flexible and extensible while maintaining consistent design patterns and accessibility standards.