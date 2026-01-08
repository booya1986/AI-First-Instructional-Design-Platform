---
name: frontend-engineer
description: Build and refactor production-grade HTML/CSS/JavaScript interfaces following this project's design system. Specializes in dark-themed, minimal, data-driven presentation decks with RTL support, interactive navigation, and responsive layouts. Use for slideshow development, component creation, and maintaining consistent visual identity across English and Hebrew versions.
---

# Frontend Engineer Skill

Build production-grade web interfaces following the **Claude Code Dark** design system used in this instructional design platform project.

## Project Context

This skill applies to building interactive presentation decks (investor decks, user story decks) with:
- **Single-file HTML architecture** (embedded CSS/JS, no external dependencies except fonts)
- **Dark monochrome aesthetic** (#0B0B0B background, greyscale data viz, white text)
- **Bilingual support** (English LTR + Hebrew RTL with Noto Sans Hebrew)
- **Interactive navigation** (keyboard, touch, button controls)
- **Responsive design** (1440×900 base, scales to mobile)

## Phase 1: Design Thinking

Before writing code, establish:

### 1. Purpose & Audience
- **Who**: Investors, stakeholders, Hebrew/English speakers
- **Goal**: Professional presentations with data visualization
- **Context**: Corporate pitch decks, user story narratives

### 2. Design System Constraints
Reference: `sildeshow deck design principelce.md`

**Core Principles:**
- 🩶 **Monochrome Discipline**: Greyscale only, no color accents
- ⬛ **Dark is Primary**: #0B0B0B background, white text
- 📐 **Precise Grid**: 1440×900 artboard, 12 columns, 24px gutter, 64px margins
- 🧠 **Data Over Decoration**: Structure and information first
- 🕊️ **Breathe**: Generous spacing, no clutter

**Typography:**
- **Headers**: JetBrains Mono, 48px, weight 600
- **Body**: Inter, 16px, weight 400
- **Hebrew**: Noto Sans Hebrew (all weights), 1.6+ line-height, RTL-aligned

**Color Palette:**
```css
--bg-base: #0B0B0B;
--bg-elevated: #111111;
--text-primary: #FFFFFF;
--text-secondary: rgba(255,255,255,0.72);
--text-tertiary: rgba(255,255,255,0.56);
--border-subtle: rgba(255,255,255,0.12);
--dot-grid: rgba(255,255,255,0.08);
```

### 3. RTL Requirements
When building Hebrew versions:
- Mirror layouts horizontally (charts right, legends left)
- Right-align all text blocks
- Reverse button order (Next left, Previous right)
- Move pagination to bottom-left
- Use `[dir="rtl"]` CSS selectors, not duplication

## Phase 2: Implementation Standards

### Architecture Pattern
✅ **Single HTML File**
- Embed all CSS in `<style>` tags
- Embed all JavaScript in `<script>` tags
- Only external: Google Fonts (Noto Sans Hebrew, JetBrains Mono, Inter)
- No build tools, no frameworks, vanilla JS only

✅ **Slide Structure**
```html
<div class="slide" data-slide="1">
    <div class="slide-header">
        <h1>Slide Title</h1>
        <p class="subtitle">Subtitle text</p>
    </div>
    <div class="slide-content">
        <!-- Content here -->
    </div>
</div>
```

### Component Patterns

**1. Character Avatars**
```css
.character-avatar {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background-size: cover;
    background-position: center;
    background-image: url('img/character.jpg');
}
```
- Use optimized JPEGs (200px, ~15KB)
- Hide text with `font-size: 0`

**2. Data Visualization**
Prefer inline SVG for charts:
```html
<svg width="100%" height="180" viewBox="0 0 900 180">
    <!-- For RTL: anchor text-anchor="end" at x="900" -->
    <rect x="350" y="50" width="450" height="40" fill="#FFFFFF" rx="4"/>
    <text x="850" y="75" text-anchor="end">Label</text>
</svg>
```
- Use greyscale fills only
- Position labels outside bars for visibility
- RTL: align from right (x=900) to left

**3. Dot Grid Background**
```css
body::before {
    content: '';
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background-image: radial-gradient(
        circle,
        rgba(255,255,255,0.08) 1.5px,
        transparent 1.5px
    );
    background-size: 20px 20px;
    pointer-events: none;
    z-index: 0;
}
```

**4. Navigation Controls**
```html
<div class="nav-controls">
    <button class="nav-btn" id="prevBtn">← Previous</button>
    <button class="nav-btn" id="nextBtn">Next →</button>
    <div class="slide-counter">1 of 21</div>
</div>
```
JavaScript requirements:
- Arrow keys: Left/Right navigate
- Space: Next slide
- Home/End: First/Last slide
- Touch: Swipe gestures
- URL hash: `#slide-number` for deep linking

### RTL-Specific CSS

```css
/* Essential RTL overrides */
[dir="rtl"] body {
    line-height: 1.7; /* Hebrew readability */
}

[dir="rtl"] .column li {
    padding-left: 0;
    padding-right: 20px;
}

[dir="rtl"] .column li::before {
    content: '←'; /* Reverse arrows */
    left: auto;
    right: 0;
}

[dir="rtl"] .slide-counter {
    right: auto;
    left: 64px;
}

/* Keep centered elements centered */
[dir="rtl"] h1,
[dir="rtl"] .subtitle {
    text-align: center; /* Don't force right-align titles */
}
```

### Mobile Responsiveness

**Viewport Meta Tag (Required):**
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

**Responsive Breakpoints:**
```css
/* Tablet: 768px - 1024px */
@media (max-width: 1024px) {
    .slide {
        padding: 48px 32px;
    }

    h1 {
        font-size: 36px;
    }

    .two-column {
        grid-template-columns: 1fr;
        gap: 32px;
    }

    .stat-large {
        font-size: 48px;
    }
}

/* Mobile: < 768px */
@media (max-width: 768px) {
    .slide {
        padding: 32px 24px;
        min-height: 100vh; /* Full viewport height */
    }

    h1 {
        font-size: 28px;
        line-height: 1.2;
    }

    .subtitle {
        font-size: 16px;
    }

    .stat-large {
        font-size: 36px;
    }

    .nav-controls {
        bottom: 24px;
        right: 24px;
        gap: 12px;
    }

    .nav-btn {
        padding: 10px 16px;
        font-size: 13px;
    }

    .slide-counter {
        bottom: 24px;
        right: 24px;
        font-size: 12px;
    }

    /* RTL adjustments */
    [dir="rtl"] .nav-controls {
        right: auto;
        left: 24px;
    }

    [dir="rtl"] .slide-counter {
        right: auto;
        left: 24px;
    }

    /* Stack cards vertically */
    .card {
        padding: 24px;
        max-width: 100%;
    }

    /* Hide complex charts on very small screens */
    svg {
        overflow: visible;
        max-width: 100%;
        height: auto;
    }
}

/* Small phones: < 375px */
@media (max-width: 375px) {
    h1 {
        font-size: 24px;
    }

    .stat-large {
        font-size: 32px;
    }

    .slide {
        padding: 24px 16px;
    }
}
```

**Touch-Friendly Sizing:**
- Buttons: minimum 44×44px tap target
- Font size: minimum 16px for body (prevents zoom on iOS)
- Spacing: increase touch areas on mobile

**Mobile-Specific Patterns:**
```css
/* Hide desktop-only elements */
@media (max-width: 768px) {
    .desktop-only {
        display: none;
    }
}

/* Show mobile-only elements */
.mobile-only {
    display: none;
}

@media (max-width: 768px) {
    .mobile-only {
        display: block;
    }
}

/* Prevent horizontal scroll */
body {
    overflow-x: hidden;
    max-width: 100vw;
}

/* Improve touch scrolling */
.slide {
    -webkit-overflow-scrolling: touch;
}
```

### Performance Optimization

**Image Guidelines:**
- Character avatars: 200×200px, JPEG 85% quality, ~15KB
- Responsive images: Use `srcset` for different screen densities
  ```html
  <img src="avatar.jpg"
       srcset="avatar.jpg 1x, avatar@2x.jpg 2x"
       alt="Character name">
  ```
- Use `sips` for batch optimization:
  ```bash
  sips -Z 200 --setProperty format jpeg \
       --setProperty formatOptions 85 input.png --out output.jpg
  ```
- Total page weight target: <500KB

**CSS Best Practices:**
- Use CSS variables for all colors/spacing
- Minimize specificity (avoid deep nesting)
- Use `will-change` for animated elements only
- Prefer `transform` over `top/left` for animations
- Load critical CSS inline, defer non-critical

**Mobile Performance:**
- Minimize JavaScript execution on scroll
- Use passive event listeners: `{ passive: true }`
- Debounce resize events
- Lazy load images below the fold

## Phase 3: Quality Checklist

Before considering work complete, verify:

### Visual Quality
- [ ] Dark background (#0B0B0B) renders correctly
- [ ] Dot grid visible (opacity 0.08, dots 1.5px)
- [ ] All text readable (contrast ratio ≥7:1)
- [ ] Character images load and display circular
- [ ] Charts use greyscale only
- [ ] Spacing follows 64px margins, 24px gutters

### RTL Compliance (Hebrew versions)
- [ ] `<html lang="he" dir="rtl">`
- [ ] Noto Sans Hebrew font loads (check DevTools Network)
- [ ] All text flows right-to-left
- [ ] Numbers remain LTR (23%, 73%, etc.)
- [ ] Navigation buttons reversed
- [ ] Slide counter on bottom-left

### Functionality
- [ ] Arrow keys navigate (Right=next, Left=prev)
- [ ] Space bar advances slides
- [ ] Home/End work
- [ ] Touch swipe works (trackpad/mobile)
- [ ] URL hash updates (#1, #2, etc.)
- [ ] Deep linking works (open #15 directly)
- [ ] Slide counter updates correctly

### Cross-Browser Testing
- [ ] Chrome (Blink engine)
- [ ] Safari (WebKit engine)
- [ ] Firefox (Gecko engine)
- [ ] No console errors
- [ ] Fonts load (no fallback to system fonts)

### Performance
- [ ] Page load <2 seconds on 3G
- [ ] All images <20KB each
- [ ] Total page size <500KB
- [ ] Smooth animations (60fps)

### Mobile Responsiveness
- [ ] Viewport meta tag present
- [ ] Minimum 16px font size (prevents iOS zoom)
- [ ] Touch targets ≥44×44px
- [ ] No horizontal scroll on mobile
- [ ] Swipe gestures work smoothly
- [ ] Text readable without zooming
- [ ] Images scale properly
- [ ] Navigation accessible on small screens

### Mobile Device Testing
- [ ] iPhone (Safari iOS)
- [ ] Android phone (Chrome)
- [ ] Tablet (iPad/Android)
- [ ] Test in portrait orientation
- [ ] Test in landscape orientation
- [ ] Verify touch gestures work
- [ ] Check text legibility

## Phase 4: Common Patterns

### Creating a New Slide Deck

1. **Start from Template**
   - Copy `user_story_deck.html` or `investor_deck.html`
   - Update `<title>` and meta tags
   - Clear slide content, keep structure

2. **Set Up Fonts**
   ```html
   <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Hebrew:wght@400;500;600;700&family=JetBrains+Mono:wght@400;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
   ```

3. **Define CSS Variables**
   ```css
   :root {
       --bg-base: #0B0B0B;
       --font-heading: 'JetBrains Mono', monospace;
       --font-body: 'Inter', sans-serif;
   }
   ```

4. **Build Slides**
   - Use semantic HTML (`<h1>`, `<p>`, `<ul>`)
   - Follow grid system (64px margins)
   - Add data-slide attributes sequentially

5. **Test Navigation**
   - Verify `totalSlides` constant matches actual count
   - Test all keyboard/touch controls
   - Check URL hash routing

### Converting to Hebrew RTL

1. **HTML Root Changes**
   ```html
   <html lang="he" dir="rtl">
   <title>כותרת בעברית</title>
   ```

2. **Add Noto Sans Hebrew**
   ```css
   --font-heading: 'Noto Sans Hebrew', sans-serif;
   --font-body: 'Noto Sans Hebrew', sans-serif;
   ```

3. **RTL CSS Overrides**
   - Add `[dir="rtl"]` selectors after main CSS
   - Mirror layouts (flexbox `row-reverse`)
   - Right-align text blocks
   - Reverse arrows (→ becomes ←)

4. **Translate Content**
   - Maintain HTML structure
   - Keep numeric values (23%, 73%)
   - Update character names
   - Translate button labels

5. **Update Navigation JS**
   ```javascript
   document.getElementById('slideCounter').textContent =
       `${currentSlide} מתוך ${totalSlides}`;
   ```

### Adding Charts/Graphs

**Bar Charts (Horizontal)**
```html
<svg width="100%" height="180" viewBox="0 0 900 180">
    <!-- RTL: bars start from right -->
    <rect x="350" y="50" width="450" height="40" fill="#FFFFFF" rx="4"/>
    <text x="850" y="75" text-anchor="end">Label</text>
    <text x="310" y="75" text-anchor="end">45%</text>
</svg>
```

**Stat Cards**
```html
<div class="stat-card">
    <span class="stat-large">94%</span>
    <span class="label">של יעד הרבעון</span>
    <span class="small">שיפור משמעותי</span>
</div>
```

**Donut Charts** (if needed)
- Use 420px diameter, 84px thickness
- 7 greyscale shades (#FFFFFF → #6C6C6C)
- Start angle: -90°
- Legend: vertical list, right-aligned (RTL)

## Phase 5: Anti-Patterns (Avoid These)

❌ **Don't:**
- Use colors (stick to greyscale)
- Add drop shadows on data viz
- Mix Hebrew/English fonts in same heading
- Break the grid with floating elements
- Use external dependencies (jQuery, React, etc.)
- Create separate RTL/LTR CSS files (use `[dir="rtl"]`)
- Make images >50KB
- Use `position: absolute` for layout (use flexbox/grid)
- Hardcode slide numbers in content (use JS counter)

✅ **Do:**
- Maintain spacing consistency
- Use semantic HTML
- Keep animations subtle
- Optimize images aggressively
- Test on actual devices
- Validate HTML/CSS
- Use CSS variables for theming
- Follow 1440×900 base artboard

## Resources

**Design System Reference:**
- `sildeshow deck design principelce.md` - Complete design tokens and principles

**Existing Examples:**
- `user_story_deck.html` - 21-slide character-driven narrative (English)
- `user_story_deck_he.html` - Hebrew RTL version with Noto Sans Hebrew
- `investor_deck.html` - Business case presentation (English)

**Image Assets:**
- `img/` - Optimized character avatars (200×200px JPEGs)

## When to Use This Skill

Invoke this skill when:
- Creating new presentation slides
- Building interactive data visualizations
- Converting English decks to Hebrew RTL
- Optimizing images for web
- Debugging layout/navigation issues
- Ensuring design system compliance
- Adding new chart types or components

---

*This skill ensures all frontend work maintains the distinctive Claude Code Dark aesthetic while supporting bilingual (English/Hebrew) audiences with production-grade performance and accessibility.*
