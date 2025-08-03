# Usage Guide - Open Next.js Template

This guide provides practical examples and step-by-step instructions for developers working with the Open Next.js template.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Building Your First Page](#building-your-first-page)
3. [Working with Components](#working-with-components)
4. [Creating Custom Features](#creating-custom-features)
5. [Styling and Theming](#styling-and-theming)
6. [Adding Interactivity](#adding-interactivity)
7. [Performance Optimization](#performance-optimization)
8. [Deployment Guide](#deployment-guide)
9. [Common Patterns](#common-patterns)
10. [Troubleshooting](#troubleshooting)

## Getting Started

### Installation and Setup

1. **Clone or download the template:**
```bash
git clone https://github.com/cruip/open-react-template.git my-project
cd my-project
```

2. **Install dependencies:**
```bash
pnpm install
# or
npm install
```

3. **Start the development server:**
```bash
pnpm dev
# or
npm run dev
```

4. **Open in browser:**
Visit `http://localhost:3000` to see your application.

### Project Structure Overview

```
my-project/
├── app/                    # Next.js App Router pages
│   ├── (auth)/            # Authentication pages
│   ├── (default)/         # Main site pages
│   ├── api/               # API routes
│   ├── css/               # Global styles
│   └── layout.tsx         # Root layout
├── components/            # React components
│   ├── ui/               # Layout components
│   └── *.tsx             # Feature components
├── utils/                # Utility hooks
├── public/               # Static assets
├── package.json
└── tsconfig.json
```

## Building Your First Page

### Step 1: Create a New Page

Create a new page in the `app` directory:

```tsx
// app/about/page.tsx
import PageIllustration from '@/components/page-illustration';

export default function AboutPage() {
  return (
    <main className="grow">
      <PageIllustration />
      
      <section className="relative">
        <div className="mx-auto max-w-6xl px-4 sm:px-6">
          <div className="py-12 md:py-20">
            <div className="mx-auto max-w-3xl text-center">
              <h1 className="font-nacelle text-4xl font-semibold text-white md:text-5xl">
                About Our Company
              </h1>
              <p className="mt-6 text-lg text-indigo-200/65">
                Learn more about our mission, values, and the team behind our success.
              </p>
            </div>
          </div>
        </div>
      </section>
    </main>
  );
}
```

### Step 2: Add a Layout (Optional)

Create a layout for your page group:

```tsx
// app/about/layout.tsx
export default function AboutLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div className="about-section">
      {children}
    </div>
  );
}
```

### Step 3: Add Navigation

Update the header component to include your new page:

```tsx
// components/ui/header.tsx (modify existing)
import Link from "next/link";
import Logo from "./logo";

export default function Header() {
  return (
    <header className="z-30 mt-2 w-full md:mt-5">
      <div className="mx-auto max-w-6xl px-4 sm:px-6">
        <div className="relative flex h-14 items-center justify-between gap-3 rounded-2xl bg-gray-900/90 px-3">
          <Logo />
          
          {/* Add navigation */}
          <nav className="hidden md:flex items-center space-x-6">
            <Link href="/" className="text-gray-300 hover:text-white transition">
              Home
            </Link>
            <Link href="/about" className="text-gray-300 hover:text-white transition">
              About
            </Link>
          </nav>
          
          {/* Auth links */}
          <div className="flex items-center gap-3">
            {/* ... existing auth links ... */}
          </div>
        </div>
      </div>
    </header>
  );
}
```

## Working with Components

### Using Existing Components

#### 1. Hero Section

```tsx
// Create a custom hero for your page
import HeroHome from '@/components/hero-home';

export default function LandingPage() {
  return (
    <main className="grow">
      <HeroHome />
    </main>
  );
}
```

#### 2. Features Section

```tsx
import Features from '@/components/features';

export default function ProductPage() {
  return (
    <main className="grow">
      <Features />
    </main>
  );
}
```

#### 3. Modal Video

```tsx
import ModalVideo from '@/components/modal-video';
import DemoThumbnail from '@/public/images/demo-thumbnail.jpg';

export default function VideoSection() {
  return (
    <section className="py-12">
      <div className="mx-auto max-w-4xl px-4">
        <ModalVideo
          thumb={DemoThumbnail}
          thumbWidth={800}
          thumbHeight={450}
          thumbAlt="Product demo video"
          video="videos/product-demo.mp4"
          videoWidth={1920}
          videoHeight={1080}
        />
      </div>
    </section>
  );
}
```

### Creating Custom Components

#### 1. Simple Content Component

```tsx
// components/team-section.tsx
interface TeamMember {
  name: string;
  role: string;
  image: string;
  bio: string;
}

interface TeamSectionProps {
  members: TeamMember[];
}

export default function TeamSection({ members }: TeamSectionProps) {
  return (
    <section className="py-12 md:py-20">
      <div className="mx-auto max-w-6xl px-4 sm:px-6">
        <div className="text-center mb-12">
          <h2 className="font-nacelle text-3xl font-semibold text-white md:text-4xl">
            Meet Our Team
          </h2>
          <p className="mt-4 text-lg text-indigo-200/65">
            The talented people behind our success
          </p>
        </div>
        
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          {members.map((member, index) => (
            <div 
              key={index}
              className="text-center"
              data-aos="fade-up"
              data-aos-delay={index * 100}
            >
              <img
                src={member.image}
                alt={member.name}
                className="w-32 h-32 rounded-full mx-auto mb-4"
              />
              <h3 className="text-xl font-semibold text-white">{member.name}</h3>
              <p className="text-indigo-300 mb-2">{member.role}</p>
              <p className="text-gray-400 text-sm">{member.bio}</p>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

#### 2. Interactive Card Component

```tsx
// components/pricing-card.tsx
interface PricingPlan {
  name: string;
  price: number;
  features: string[];
  highlighted?: boolean;
}

interface PricingCardProps extends PricingPlan {
  onSelect: (plan: string) => void;
}

export default function PricingCard({ 
  name, 
  price, 
  features, 
  highlighted = false, 
  onSelect 
}: PricingCardProps) {
  return (
    <div 
      className={`
        relative p-6 rounded-2xl transition-all duration-300
        ${highlighted 
          ? 'bg-gradient-to-br from-indigo-500/20 to-purple-500/20 border-2 border-indigo-500/50' 
          : 'bg-gray-800/50 border border-gray-700/50 hover:border-indigo-500/30'
        }
      `}
      data-aos="fade-up"
    >
      {highlighted && (
        <div className="absolute -top-3 left-1/2 transform -translate-x-1/2">
          <span className="bg-indigo-500 text-white px-4 py-1 rounded-full text-sm font-medium">
            Most Popular
          </span>
        </div>
      )}
      
      <div className="text-center">
        <h3 className="text-xl font-semibold text-white mb-2">{name}</h3>
        <div className="mb-6">
          <span className="text-4xl font-bold text-white">${price}</span>
          <span className="text-gray-400">/month</span>
        </div>
        
        <ul className="space-y-3 mb-8">
          {features.map((feature, index) => (
            <li key={index} className="flex items-center text-gray-300">
              <svg className="w-5 h-5 text-indigo-400 mr-3" fill="currentColor" viewBox="0 0 20 20">
                <path fillRule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clipRule="evenodd" />
              </svg>
              {feature}
            </li>
          ))}
        </ul>
        
        <button
          onClick={() => onSelect(name)}
          className={`
            w-full py-3 px-6 rounded-lg font-medium transition-all duration-300
            ${highlighted
              ? 'bg-indigo-500 hover:bg-indigo-600 text-white'
              : 'bg-gray-700 hover:bg-gray-600 text-gray-200'
            }
          `}
        >
          Choose {name}
        </button>
      </div>
    </div>
  );
}
```

## Creating Custom Features

### 1. Contact Form with Validation

```tsx
// components/contact-form.tsx
'use client';

import { useState } from 'react';

interface FormData {
  name: string;
  email: string;
  message: string;
}

export default function ContactForm() {
  const [formData, setFormData] = useState<FormData>({
    name: '',
    email: '',
    message: ''
  });
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submitStatus, setSubmitStatus] = useState<'idle' | 'success' | 'error'>('idle');

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setIsSubmitting(true);
    
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      
      if (response.ok) {
        setSubmitStatus('success');
        setFormData({ name: '', email: '', message: '' });
      } else {
        setSubmitStatus('error');
      }
    } catch (error) {
      setSubmitStatus('error');
    } finally {
      setIsSubmitting(false);
    }
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    setFormData(prev => ({
      ...prev,
      [e.target.name]: e.target.value
    }));
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-6">
      <div>
        <label htmlFor="name" className="block text-sm font-medium text-gray-200 mb-2">
          Name
        </label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          required
          className="w-full px-4 py-3 rounded-lg bg-gray-800 border border-gray-700 text-white focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
        />
      </div>
      
      <div>
        <label htmlFor="email" className="block text-sm font-medium text-gray-200 mb-2">
          Email
        </label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          required
          className="w-full px-4 py-3 rounded-lg bg-gray-800 border border-gray-700 text-white focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
        />
      </div>
      
      <div>
        <label htmlFor="message" className="block text-sm font-medium text-gray-200 mb-2">
          Message
        </label>
        <textarea
          id="message"
          name="message"
          rows={5}
          value={formData.message}
          onChange={handleChange}
          required
          className="w-full px-4 py-3 rounded-lg bg-gray-800 border border-gray-700 text-white focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
        />
      </div>
      
      <button
        type="submit"
        disabled={isSubmitting}
        className="w-full bg-indigo-500 hover:bg-indigo-600 disabled:bg-gray-600 text-white font-medium py-3 px-6 rounded-lg transition-colors duration-300"
      >
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </button>
      
      {submitStatus === 'success' && (
        <p className="text-green-400 text-center">Message sent successfully!</p>
      )}
      {submitStatus === 'error' && (
        <p className="text-red-400 text-center">Failed to send message. Please try again.</p>
      )}
    </form>
  );
}
```

### 2. API Route for Contact Form

```tsx
// app/api/contact/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  try {
    const { name, email, message } = await request.json();
    
    // Validate input
    if (!name || !email || !message) {
      return NextResponse.json(
        { error: 'All fields are required' },
        { status: 400 }
      );
    }
    
    // Here you would typically:
    // 1. Send email via service (SendGrid, Nodemailer, etc.)
    // 2. Save to database
    // 3. Send to CRM or notification service
    
    console.log('Contact form submission:', { name, email, message });
    
    return NextResponse.json(
      { message: 'Message sent successfully' },
      { status: 200 }
    );
  } catch (error) {
    console.error('Contact form error:', error);
    return NextResponse.json(
      { error: 'Internal server error' },
      { status: 500 }
    );
  }
}
```

## Styling and Theming

### 1. Custom Color Scheme

```css
/* app/css/custom-theme.css */
:root {
  /* Custom color palette */
  --color-primary-50: #eff6ff;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-900: #1e3a8a;
  
  --color-secondary-500: #06b6d4;
  --color-secondary-600: #0891b2;
  
  --color-accent-500: #f59e0b;
  --color-accent-600: #d97706;
}

/* Custom component styles */
.btn-custom {
  @apply px-6 py-3 rounded-lg font-medium transition-all duration-300;
  background: linear-gradient(135deg, var(--color-primary-500), var(--color-secondary-500));
}

.btn-custom:hover {
  background: linear-gradient(135deg, var(--color-primary-600), var(--color-secondary-600));
  transform: translateY(-1px);
  box-shadow: 0 10px 25px rgba(59, 130, 246, 0.3);
}
```

### 2. Dark/Light Theme Toggle

```tsx
// components/theme-toggle.tsx
'use client';

import { useState, useEffect } from 'react';

export default function ThemeToggle() {
  const [isDark, setIsDark] = useState(true);
  
  useEffect(() => {
    const saved = localStorage.getItem('theme');
    if (saved) {
      setIsDark(saved === 'dark');
    }
  }, []);
  
  const toggleTheme = () => {
    const newTheme = !isDark;
    setIsDark(newTheme);
    localStorage.setItem('theme', newTheme ? 'dark' : 'light');
    document.documentElement.classList.toggle('light', !newTheme);
  };
  
  return (
    <button
      onClick={toggleTheme}
      className="p-2 rounded-lg bg-gray-800 hover:bg-gray-700 transition-colors"
      aria-label="Toggle theme"
    >
      {isDark ? (
        <svg className="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
          <path fillRule="evenodd" d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" clipRule="evenodd" />
        </svg>
      ) : (
        <svg className="w-5 h-5 text-gray-400" fill="currentColor" viewBox="0 0 20 20">
          <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
        </svg>
      )}
    </button>
  );
}
```

## Adding Interactivity

### 1. Animated Counter

```tsx
// components/animated-counter.tsx
'use client';

import { useState, useEffect, useRef } from 'react';

interface AnimatedCounterProps {
  end: number;
  duration?: number;
  suffix?: string;
}

export default function AnimatedCounter({ 
  end, 
  duration = 2000, 
  suffix = '' 
}: AnimatedCounterProps) {
  const [count, setCount] = useState(0);
  const [isVisible, setIsVisible] = useState(false);
  const counterRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting && !isVisible) {
          setIsVisible(true);
        }
      },
      { threshold: 0.1 }
    );

    if (counterRef.current) {
      observer.observe(counterRef.current);
    }

    return () => observer.disconnect();
  }, [isVisible]);

  useEffect(() => {
    if (!isVisible) return;

    let startTime: number;
    const animate = (currentTime: number) => {
      if (startTime === undefined) startTime = currentTime;
      const progress = Math.min((currentTime - startTime) / duration, 1);
      
      setCount(Math.floor(progress * end));
      
      if (progress < 1) {
        requestAnimationFrame(animate);
      }
    };
    
    requestAnimationFrame(animate);
  }, [isVisible, end, duration]);

  return (
    <div ref={counterRef} className="text-4xl font-bold text-white">
      {count.toLocaleString()}{suffix}
    </div>
  );
}
```

### 2. Parallax Section

```tsx
// components/parallax-section.tsx
'use client';

import { useEffect, useState } from 'react';

interface ParallaxSectionProps {
  children: React.ReactNode;
  speed?: number;
  className?: string;
}

export default function ParallaxSection({ 
  children, 
  speed = 0.5, 
  className = '' 
}: ParallaxSectionProps) {
  const [offsetY, setOffsetY] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setOffsetY(window.scrollY * speed);
    };

    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, [speed]);

  return (
    <div 
      className={`relative ${className}`}
      style={{ transform: `translateY(${offsetY}px)` }}
    >
      {children}
    </div>
  );
}
```

## Performance Optimization

### 1. Image Optimization

```tsx
// components/optimized-image.tsx
import Image from 'next/image';
import { useState } from 'react';

interface OptimizedImageProps {
  src: string;
  alt: string;
  width: number;
  height: number;
  priority?: boolean;
  className?: string;
}

export default function OptimizedImage({
  src,
  alt,
  width,
  height,
  priority = false,
  className = ''
}: OptimizedImageProps) {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <div className={`relative overflow-hidden ${className}`}>
      {isLoading && (
        <div className="absolute inset-0 bg-gray-800 animate-pulse" />
      )}
      <Image
        src={src}
        alt={alt}
        width={width}
        height={height}
        priority={priority}
        className={`transition-opacity duration-300 ${
          isLoading ? 'opacity-0' : 'opacity-100'
        }`}
        onLoad={() => setIsLoading(false)}
      />
    </div>
  );
}
```

### 2. Lazy Loading Components

```tsx
// components/lazy-section.tsx
'use client';

import { useState, useEffect, useRef } from 'react';

interface LazySectionProps {
  children: React.ReactNode;
  placeholder?: React.ReactNode;
  threshold?: number;
}

export default function LazySection({ 
  children, 
  placeholder,
  threshold = 0.1 
}: LazySectionProps) {
  const [isVisible, setIsVisible] = useState(false);
  const ref = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsVisible(true);
          observer.disconnect();
        }
      },
      { threshold }
    );

    if (ref.current) {
      observer.observe(ref.current);
    }

    return () => observer.disconnect();
  }, [threshold]);

  return (
    <div ref={ref}>
      {isVisible ? children : placeholder}
    </div>
  );
}
```

## Deployment Guide

### 1. Vercel Deployment

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Custom domain (after initial deployment)
vercel --prod
```

### 2. Environment Variables

```bash
# .env.local
NEXT_PUBLIC_SITE_URL=https://yoursite.com
EMAIL_SERVER_HOST=smtp.gmail.com
EMAIL_SERVER_PORT=587
EMAIL_SERVER_USER=your-email@gmail.com
EMAIL_SERVER_PASSWORD=your-app-password
```

### 3. Build Optimization

```json
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    domains: ['example.com'],
    formats: ['image/webp', 'image/avif']
  },
  experimental: {
    optimizeCss: true
  },
  compiler: {
    removeConsole: process.env.NODE_ENV === 'production'
  }
};

module.exports = nextConfig;
```

## Common Patterns

### 1. Data Fetching

```tsx
// app/blog/page.tsx
interface BlogPost {
  id: string;
  title: string;
  excerpt: string;
  date: string;
}

async function getBlogPosts(): Promise<BlogPost[]> {
  // Fetch from API or CMS
  const response = await fetch('https://api.yoursite.com/posts');
  return response.json();
}

export default async function BlogPage() {
  const posts = await getBlogPosts();

  return (
    <main className="grow">
      <section className="py-12">
        <div className="mx-auto max-w-6xl px-4 sm:px-6">
          <h1 className="text-4xl font-bold text-white mb-8">Blog</h1>
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            {posts.map((post) => (
              <article key={post.id} className="bg-gray-800 rounded-lg p-6">
                <h2 className="text-xl font-semibold text-white mb-3">
                  {post.title}
                </h2>
                <p className="text-gray-400 mb-4">{post.excerpt}</p>
                <time className="text-sm text-indigo-400">
                  {new Date(post.date).toLocaleDateString()}
                </time>
              </article>
            ))}
          </div>
        </div>
      </section>
    </main>
  );
}
```

### 2. Error Handling

```tsx
// app/error.tsx
'use client';

import { useEffect } from 'react';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    console.error(error);
  }, [error]);

  return (
    <div className="min-h-screen flex items-center justify-center">
      <div className="text-center">
        <h2 className="text-2xl font-bold text-white mb-4">
          Something went wrong!
        </h2>
        <button
          onClick={() => reset()}
          className="bg-indigo-500 hover:bg-indigo-600 text-white px-6 py-3 rounded-lg"
        >
          Try again
        </button>
      </div>
    </div>
  );
}
```

### 3. Loading States

```tsx
// app/loading.tsx
export default function Loading() {
  return (
    <div className="min-h-screen flex items-center justify-center">
      <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-indigo-500"></div>
    </div>
  );
}
```

## Troubleshooting

### Common Issues

1. **Hydration Errors:**
```tsx
// Use dynamic imports for client-only components
import dynamic from 'next/dynamic';

const ClientOnlyComponent = dynamic(
  () => import('@/components/client-only'),
  { ssr: false }
);
```

2. **Image Optimization Issues:**
```tsx
// next.config.js
module.exports = {
  images: {
    domains: ['yourdomain.com'],
    unoptimized: process.env.NODE_ENV === 'development'
  }
};
```

3. **CSS-in-JS with SSR:**
```tsx
// Use CSS modules or Tailwind instead of styled-components for SSR
import styles from './component.module.css';

export default function Component() {
  return <div className={styles.container}>Content</div>;
}
```

### Performance Tips

1. **Bundle Analysis:**
```bash
npm install @next/bundle-analyzer
```

2. **Lighthouse Optimization:**
- Use `next/image` for all images
- Implement proper loading states
- Minimize JavaScript bundles
- Enable compression

3. **SEO Optimization:**
```tsx
// app/layout.tsx
export const metadata = {
  title: 'Your Site Title',
  description: 'Your site description',
  openGraph: {
    title: 'Your Site Title',
    description: 'Your site description',
    images: ['/og-image.jpg']
  }
};
```

---

This usage guide provides practical examples and patterns for building with the Open Next.js template. Each example can be adapted to your specific needs while maintaining the design system and performance characteristics of the template.