# CSS Patterns Reference

Common CSS patterns used in this project. Load this file when implementing specific UI components.

## Layout Patterns

### Slide Layout (Base)
```css
.slide {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 64px;
    position: relative;
}

.slide-header {
    text-align: center;
    margin-bottom: 48px;
}

.slide-content {
    max-width: 1200px;
    width: 100%;
}
```

### Two-Column Layout
```css
.two-column {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: start;
}

/* RTL: No changes needed, grid auto-flows correctly */
```

### Card Layout
```css
.card {
    background: var(--bg-elevated);
    border: 1px solid var(--border-subtle);
    border-radius: 12px;
    padding: 32px;
    max-width: 700px;
    margin: 0 auto;
}

[dir="rtl"] .card {
    /* Cards work the same in RTL */
}
```

## Typography Patterns

### Heading Hierarchy
```css
h1 {
    font-family: var(--font-heading);
    font-size: 48px;
    font-weight: 600;
    line-height: 1.1;
    color: var(--text-primary);
    margin: 0 0 16px 0;
}

.subtitle {
    font-family: var(--font-body);
    font-size: 18px;
    font-weight: 400;
    line-height: 1.5;
    color: var(--text-secondary);
    margin: 0;
}

/* Hebrew: Increase line-height */
[dir="rtl"] h1,
[dir="rtl"] .subtitle {
    line-height: 1.7;
}
```

### Stats Display
```css
.stat-large {
    display: block;
    font-family: var(--font-heading);
    font-size: 64px;
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 8px;
}

.label {
    display: block;
    font-size: 14px;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    color: var(--text-tertiary);
    margin-bottom: 4px;
}

.small {
    font-size: 13px;
    line-height: 1.4;
    color: var(--text-tertiary);
}
```

### List Styling
```css
ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

li {
    position: relative;
    padding-left: 20px;
    margin-bottom: 12px;
    font-size: 16px;
    line-height: 1.6;
    color: var(--text-secondary);
}

li::before {
    content: '→';
    position: absolute;
    left: 0;
    color: var(--text-tertiary);
}

/* RTL */
[dir="rtl"] li {
    padding-left: 0;
    padding-right: 20px;
}

[dir="rtl"] li::before {
    content: '←';
    left: auto;
    right: 0;
}
```

## Component Patterns

### Loading Indicator
```css
.loading-indicator {
    display: flex;
    gap: 8px;
    justify-content: center;
    margin-bottom: 16px;
}

.loading-dot {
    width: 8px;
    height: 8px;
    background: var(--text-tertiary);
    border-radius: 50%;
    animation: pulse 1.4s ease-in-out infinite;
}

.loading-dot:nth-child(2) {
    animation-delay: 0.2s;
}

.loading-dot:nth-child(3) {
    animation-delay: 0.4s;
}

@keyframes pulse {
    0%, 100% { opacity: 0.3; transform: scale(0.8); }
    50% { opacity: 1; transform: scale(1.2); }
}
```

### Message Bubble
```css
.message {
    display: flex;
    gap: 12px;
    margin-bottom: 16px;
    align-items: flex-start;
}

.message.user {
    flex-direction: row;
}

.message.ai {
    flex-direction: row-reverse;
}

.message-text {
    background: var(--bg-elevated);
    border: 1px solid var(--border-subtle);
    border-radius: 12px;
    padding: 16px;
    max-width: 600px;
    font-size: 15px;
    line-height: 1.6;
}

/* RTL */
[dir="rtl"] .message.user {
    flex-direction: row-reverse;
}

[dir="rtl"] .message.ai {
    flex-direction: row;
}
```

### Notification Card
```css
.notification {
    background: var(--bg-elevated);
    border: 1px solid var(--border-subtle);
    border-radius: 8px;
    padding: 16px;
    display: flex;
    align-items: center;
    gap: 12px;
}

.notification-icon {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: var(--text-tertiary);
    display: flex;
    align-items: center;
    justify-content: center;
}

.notification-content {
    flex: 1;
}

.notification-time {
    font-size: 12px;
    color: var(--text-tertiary);
    margin-left: auto;
}

[dir="rtl"] .notification-time {
    margin-left: 0;
    margin-right: auto;
}
```

### Character Avatar
```css
.character-avatar {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background-size: cover;
    background-position: center;
    display: inline-block;
    vertical-align: middle;
}

.character-avatar.small {
    width: 60px;
    height: 60px;
}

.character-avatar.large {
    width: 120px;
    height: 120px;
}

/* Hide text fallback when image loads */
.character-avatar.maria,
.character-avatar.david,
.character-avatar.sarah,
.character-avatar.jake {
    font-size: 0;
}
```

### Character Card
```css
.character-card {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px;
    background: var(--bg-elevated);
    border: 1px solid var(--border-subtle);
    border-radius: 12px;
}

.character-name {
    font-size: 16px;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 4px;
}

.character-role {
    font-size: 14px;
    color: var(--text-tertiary);
}
```

## Navigation Patterns

### Navigation Controls
```css
.nav-controls {
    position: fixed;
    bottom: 64px;
    right: 64px;
    display: flex;
    gap: 16px;
    z-index: 100;
}

.nav-btn {
    background: var(--bg-elevated);
    border: 1px solid var(--border-subtle);
    color: var(--text-primary);
    padding: 12px 24px;
    border-radius: 12px;
    font-family: var(--font-body);
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
}

.nav-btn:hover {
    background: #1a1a1a;
    border-color: rgba(255,255,255,0.24);
}

.nav-btn:disabled {
    opacity: 0.3;
    cursor: not-allowed;
}

.slide-counter {
    position: fixed;
    bottom: 64px;
    right: 64px;
    font-size: 13px;
    color: var(--text-tertiary);
    font-family: var(--font-heading);
}

/* RTL */
[dir="rtl"] .nav-controls {
    right: auto;
    left: 64px;
}

[dir="rtl"] .slide-counter {
    right: auto;
    left: 64px;
}
```

## Data Visualization Patterns

### Stat Card
```css
.stat-card {
    background: var(--bg-elevated);
    border: 1px solid var(--border-subtle);
    border-radius: 12px;
    padding: 24px;
    text-align: center;
}

.stat-card .stat-large {
    font-size: 48px;
}

.stat-card .label {
    margin-top: 8px;
}
```

### Progress Bar
```css
.progress-bar {
    width: 100%;
    height: 8px;
    background: var(--bg-elevated);
    border-radius: 4px;
    overflow: hidden;
}

.progress-fill {
    height: 100%;
    background: var(--text-primary);
    border-radius: 4px;
    transition: width 0.3s ease;
}
```

### Timeline
```css
.timeline {
    position: relative;
    padding: 32px 0;
}

.timeline-step {
    position: relative;
    padding-left: 40px;
    margin-bottom: 32px;
}

.timeline-step::before {
    content: '';
    position: absolute;
    left: 12px;
    top: 0;
    width: 2px;
    height: 100%;
    background: var(--border-subtle);
}

.timeline-step::after {
    content: '';
    position: absolute;
    left: 8px;
    top: 8px;
    width: 10px;
    height: 10px;
    background: var(--text-primary);
    border-radius: 50%;
}

.timeline-label {
    font-size: 13px;
    color: var(--text-tertiary);
    margin-bottom: 4px;
}

.timeline-value {
    font-size: 16px;
    font-weight: 600;
    color: var(--text-primary);
}

/* RTL */
[dir="rtl"] .timeline-step {
    padding-left: 0;
    padding-right: 40px;
}

[dir="rtl"] .timeline-step::before {
    left: auto;
    right: 12px;
}

[dir="rtl"] .timeline-step::after {
    left: auto;
    right: 8px;
}
```

## Animation Patterns

### Fade In
```css
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.fade-in {
    animation: fadeIn 0.6s ease-out;
}
```

### Slide Enter (LTR)
```css
@keyframes slideEnter {
    from {
        opacity: 0;
        transform: translateX(40px);
    }
    to {
        opacity: 1;
        transform: translateX(0);
    }
}

.slide-enter {
    animation: slideEnter 0.5s ease-out;
}

/* RTL: Reverse direction */
[dir="rtl"] @keyframes slideEnter {
    from {
        transform: translateX(-40px);
    }
    to {
        transform: translateX(0);
    }
}
```

### Scale In
```css
@keyframes scaleIn {
    from {
        opacity: 0;
        transform: scale(0.9);
    }
    to {
        opacity: 1;
        transform: scale(1);
    }
}

.scale-in {
    animation: scaleIn 0.4s ease-out;
}
```

## Responsive Patterns

### Mobile-First Base Styles
```css
/* Start mobile, enhance for desktop */
.slide {
    padding: 24px 16px;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

h1 {
    font-size: 24px;
    line-height: 1.2;
}

.subtitle {
    font-size: 14px;
    line-height: 1.5;
}

.stat-large {
    font-size: 32px;
}
```

### Tablet Breakpoint (768px+)
```css
@media (min-width: 768px) {
    .slide {
        padding: 32px 24px;
    }

    h1 {
        font-size: 32px;
    }

    .subtitle {
        font-size: 16px;
    }

    .stat-large {
        font-size: 36px;
    }

    .two-column {
        grid-template-columns: 1fr;
        gap: 32px;
    }

    .nav-controls {
        bottom: 24px;
        right: 24px;
        gap: 12px;
    }

    [dir="rtl"] .nav-controls {
        right: auto;
        left: 24px;
    }
}
```

### Desktop Breakpoint (1024px+)
```css
@media (min-width: 1024px) {
    .slide {
        padding: 48px 32px;
    }

    h1 {
        font-size: 36px;
    }

    .stat-large {
        font-size: 48px;
    }

    .two-column {
        grid-template-columns: 1fr 1fr;
        gap: 80px;
    }
}
```

### Large Desktop (1440px+)
```css
@media (min-width: 1440px) {
    .slide {
        padding: 64px;
    }

    h1 {
        font-size: 48px;
    }

    .stat-large {
        font-size: 64px;
    }
}
```

### Mobile-Specific Patterns

#### Touch-Friendly Buttons
```css
.nav-btn {
    /* Minimum 44x44px touch target */
    min-width: 44px;
    min-height: 44px;
    padding: 12px 20px;
    font-size: 14px;

    /* Improve touch feedback */
    -webkit-tap-highlight-color: rgba(255,255,255,0.1);
    touch-action: manipulation;
}

@media (max-width: 768px) {
    .nav-btn {
        padding: 10px 16px;
        font-size: 13px;
        /* Still maintains 44px minimum */
    }
}
```

#### Prevent Zoom on Input Focus (iOS)
```css
input,
textarea,
select {
    font-size: 16px; /* Prevents iOS zoom */
}
```

#### Safe Area for Notch Devices
```css
.slide {
    /* Account for iPhone notch/home indicator */
    padding-left: env(safe-area-inset-left);
    padding-right: env(safe-area-inset-right);
    padding-bottom: calc(64px + env(safe-area-inset-bottom));
}

.nav-controls {
    bottom: calc(24px + env(safe-area-inset-bottom));
    right: calc(24px + env(safe-area-inset-right));
}

[dir="rtl"] .nav-controls {
    right: auto;
    left: calc(24px + env(safe-area-inset-left));
}
```

#### Prevent Horizontal Scroll
```css
body {
    overflow-x: hidden;
    max-width: 100vw;
}

* {
    max-width: 100%;
}

/* Ensure images don't overflow */
img {
    max-width: 100%;
    height: auto;
}

/* Constrain text */
h1, p, li {
    overflow-wrap: break-word;
    word-wrap: break-word;
    hyphens: auto;
}
```

#### Smooth Touch Scrolling
```css
.slide-content {
    -webkit-overflow-scrolling: touch;
    overscroll-behavior-y: contain;
}

/* Prevent pull-to-refresh on some browsers */
body {
    overscroll-behavior-y: none;
}
```

#### Responsive Typography Scale
```css
/* Fluid typography */
h1 {
    font-size: clamp(24px, 5vw, 48px);
}

.subtitle {
    font-size: clamp(14px, 3vw, 18px);
}

body {
    font-size: clamp(14px, 2.5vw, 16px);
}

.stat-large {
    font-size: clamp(32px, 8vw, 64px);
}
```

#### Responsive Spacing
```css
.slide {
    padding: clamp(16px, 4vw, 64px);
}

.slide-content {
    gap: clamp(16px, 3vw, 32px);
}
```

#### Mobile Navigation
```css
@media (max-width: 768px) {
    .nav-controls {
        /* Full width on very small screens */
        width: 100%;
        bottom: 0;
        left: 0;
        right: 0;
        padding: 16px;
        background: var(--bg-base);
        border-top: 1px solid var(--border-subtle);
        justify-content: space-between;
    }

    .nav-btn {
        flex: 1;
        max-width: 48%;
    }

    .slide-counter {
        position: static;
        text-align: center;
        width: 100%;
        margin-bottom: 12px;
    }
}
```

#### Responsive Charts/SVG
```css
svg {
    max-width: 100%;
    height: auto;
    overflow: visible;
}

@media (max-width: 768px) {
    /* Simplify charts on mobile */
    svg text {
        font-size: 12px;
    }

    /* Hide less important labels */
    svg .secondary-label {
        display: none;
    }
}
```

#### Landscape Mobile Adjustments
```css
@media (max-width: 768px) and (orientation: landscape) {
    .slide {
        padding: 16px 24px;
    }

    h1 {
        font-size: 20px;
    }

    .nav-controls {
        bottom: 12px;
        right: 12px;
    }
}
```

#### Hide/Show Elements by Screen Size
```css
/* Desktop only */
.desktop-only {
    display: block;
}

@media (max-width: 768px) {
    .desktop-only {
        display: none;
    }
}

/* Mobile only */
.mobile-only {
    display: none;
}

@media (max-width: 768px) {
    .mobile-only {
        display: block;
    }
}

/* Tablet and up */
@media (min-width: 768px) {
    .tablet-up {
        display: block;
    }
}
```

#### Responsive Grid Layouts
```css
.grid {
    display: grid;
    gap: 16px;
    /* Mobile: 1 column */
    grid-template-columns: 1fr;
}

@media (min-width: 768px) {
    .grid {
        gap: 24px;
        /* Tablet: 2 columns */
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (min-width: 1024px) {
    .grid {
        gap: 32px;
        /* Desktop: 3 columns */
        grid-template-columns: repeat(3, 1fr);
    }
}
```

#### Touch Feedback
```css
.nav-btn,
.interactive {
    /* Visual feedback on touch */
    transition: transform 0.1s, background 0.2s;
}

.nav-btn:active,
.interactive:active {
    transform: scale(0.95);
    background: rgba(255,255,255,0.1);
}

/* Disable on desktop (use hover instead) */
@media (hover: hover) {
    .nav-btn:active,
    .interactive:active {
        transform: none;
    }

    .nav-btn:hover {
        background: rgba(255,255,255,0.05);
    }
}
```

#### Performance: Reduce Animations on Mobile
```css
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}

/* Simpler animations on mobile */
@media (max-width: 768px) {
    .fade-in,
    .slide-enter {
        animation-duration: 0.3s; /* Faster than desktop */
    }
}
```

---

*Load this reference when implementing specific components or debugging CSS issues.*
