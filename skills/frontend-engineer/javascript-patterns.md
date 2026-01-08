# JavaScript Patterns Reference

Common JavaScript patterns for interactive slideshow navigation. Load this when implementing navigation logic.

## Core Navigation Pattern

### Basic Setup
```javascript
// Configuration
const totalSlides = 21; // Update based on actual slide count
let currentSlide = 1;

// Initialize on page load
document.addEventListener('DOMContentLoaded', () => {
    initializeSlideshow();
    attachEventListeners();
    handleDeepLink();
});
```

### Slide Navigation Functions
```javascript
function showSlide(n) {
    // Hide all slides
    const slides = document.querySelectorAll('.slide');
    slides.forEach(slide => {
        slide.style.display = 'none';
    });

    // Boundary checks
    if (n > totalSlides) {
        currentSlide = totalSlides;
    } else if (n < 1) {
        currentSlide = 1;
    } else {
        currentSlide = n;
    }

    // Show current slide
    const targetSlide = document.querySelector(`[data-slide="${currentSlide}"]`);
    if (targetSlide) {
        targetSlide.style.display = 'flex';
    }

    // Update UI
    updateSlideCounter();
    updateURL();
    updateButtonStates();
}

function nextSlide() {
    if (currentSlide < totalSlides) {
        showSlide(currentSlide + 1);
    }
}

function prevSlide() {
    if (currentSlide > 1) {
        showSlide(currentSlide - 1);
    }
}

function firstSlide() {
    showSlide(1);
}

function lastSlide() {
    showSlide(totalSlides);
}
```

### UI Updates
```javascript
function updateSlideCounter() {
    const counter = document.getElementById('slideCounter');
    if (counter) {
        // English
        counter.textContent = `${currentSlide} of ${totalSlides}`;

        // Hebrew (RTL)
        // counter.textContent = `${currentSlide} מתוך ${totalSlides}`;
    }
}

function updateButtonStates() {
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');

    if (prevBtn) {
        prevBtn.disabled = currentSlide === 1;
    }

    if (nextBtn) {
        nextBtn.disabled = currentSlide === totalSlides;
    }
}

function updateURL() {
    // Update URL hash without scrolling
    history.replaceState(null, null, `#${currentSlide}`);
}
```

## Event Listeners Pattern

### Keyboard Navigation
```javascript
function attachEventListeners() {
    // Keyboard controls
    document.addEventListener('keydown', (e) => {
        switch(e.key) {
            case 'ArrowRight':
            case ' ':
                e.preventDefault();
                nextSlide();
                break;
            case 'ArrowLeft':
                e.preventDefault();
                prevSlide();
                break;
            case 'Home':
                e.preventDefault();
                firstSlide();
                break;
            case 'End':
                e.preventDefault();
                lastSlide();
                break;
        }
    });

    // Button controls
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');

    if (prevBtn) {
        prevBtn.addEventListener('click', prevSlide);
    }

    if (nextBtn) {
        nextBtn.addEventListener('click', nextSlide);
    }

    // Touch/swipe controls
    attachTouchListeners();
}
```

### Touch/Swipe Navigation
```javascript
let touchStartX = 0;
let touchEndX = 0;

function attachTouchListeners() {
    const container = document.body;

    container.addEventListener('touchstart', (e) => {
        touchStartX = e.changedTouches[0].screenX;
    }, { passive: true });

    container.addEventListener('touchend', (e) => {
        touchEndX = e.changedTouches[0].screenX;
        handleSwipe();
    }, { passive: true });
}

function handleSwipe() {
    const minSwipeDistance = 50;
    const swipeDistance = touchEndX - touchStartX;

    if (Math.abs(swipeDistance) < minSwipeDistance) {
        return; // Too small, ignore
    }

    if (swipeDistance > 0) {
        // Swipe right -> previous slide
        prevSlide();
    } else {
        // Swipe left -> next slide
        nextSlide();
    }
}
```

## Deep Linking Pattern

### Handle URL Hash
```javascript
function handleDeepLink() {
    // Check for hash in URL
    const hash = window.location.hash;

    if (hash) {
        // Extract slide number from hash (#1, #2, etc.)
        const slideNum = parseInt(hash.substring(1));

        if (!isNaN(slideNum) && slideNum >= 1 && slideNum <= totalSlides) {
            showSlide(slideNum);
            return;
        }
    }

    // Default: show first slide
    showSlide(1);
}

// Listen for hash changes (browser back/forward)
window.addEventListener('hashchange', () => {
    handleDeepLink();
});
```

## Initialization Pattern

### Complete Setup
```javascript
function initializeSlideshow() {
    // Set up all slides
    const slides = document.querySelectorAll('.slide');

    slides.forEach((slide, index) => {
        // Ensure data-slide attribute exists
        if (!slide.getAttribute('data-slide')) {
            slide.setAttribute('data-slide', index + 1);
        }

        // Hide all slides initially
        slide.style.display = 'none';
    });

    // Log setup complete
    console.log(`Slideshow initialized: ${totalSlides} slides`);
}
```

## RTL-Specific Adjustments

### RTL Navigation Logic
```javascript
// For RTL layouts, you might want to reverse swipe direction
function handleSwipeRTL() {
    const isRTL = document.documentElement.dir === 'rtl';
    const minSwipeDistance = 50;
    const swipeDistance = touchEndX - touchStartX;

    if (Math.abs(swipeDistance) < minSwipeDistance) {
        return;
    }

    if (isRTL) {
        // In RTL: swipe left = previous, swipe right = next
        if (swipeDistance > 0) {
            nextSlide();
        } else {
            prevSlide();
        }
    } else {
        // In LTR: swipe right = previous, swipe left = next
        if (swipeDistance > 0) {
            prevSlide();
        } else {
            nextSlide();
        }
    }
}
```

### RTL Counter Update
```javascript
function updateSlideCounterRTL() {
    const counter = document.getElementById('slideCounter');
    const isRTL = document.documentElement.dir === 'rtl';

    if (counter) {
        if (isRTL) {
            counter.textContent = `${currentSlide} מתוך ${totalSlides}`;
        } else {
            counter.textContent = `${currentSlide} of ${totalSlides}`;
        }
    }
}
```

## Animation Helpers

### Slide Transitions
```javascript
function showSlideWithTransition(n) {
    const slides = document.querySelectorAll('.slide');
    const direction = n > currentSlide ? 1 : -1;

    // Hide all slides
    slides.forEach(slide => {
        slide.style.display = 'none';
        slide.classList.remove('slide-enter', 'slide-exit');
    });

    // Update current slide
    if (n > totalSlides) {
        currentSlide = totalSlides;
    } else if (n < 1) {
        currentSlide = 1;
    } else {
        currentSlide = n;
    }

    // Show with animation
    const targetSlide = document.querySelector(`[data-slide="${currentSlide}"]`);
    if (targetSlide) {
        targetSlide.style.display = 'flex';

        // Trigger animation
        requestAnimationFrame(() => {
            targetSlide.classList.add('slide-enter');
        });
    }

    updateSlideCounter();
    updateURL();
    updateButtonStates();
}
```

## Progress Tracking

### Track User Progress
```javascript
const visitedSlides = new Set();

function trackSlideVisit(slideNum) {
    visitedSlides.add(slideNum);
    updateProgressBar();
}

function updateProgressBar() {
    const progress = (visitedSlides.size / totalSlides) * 100;
    const progressBar = document.getElementById('progressBar');

    if (progressBar) {
        progressBar.style.width = `${progress}%`;
    }
}

// Call in showSlide function
function showSlide(n) {
    // ... existing code ...
    trackSlideVisit(currentSlide);
}
```

## Accessibility Helpers

### Keyboard Focus Management
```javascript
function manageFocus() {
    const currentSlideEl = document.querySelector(`[data-slide="${currentSlide}"]`);

    if (currentSlideEl) {
        // Set focus to slide for screen readers
        currentSlideEl.setAttribute('tabindex', '-1');
        currentSlideEl.focus();

        // Announce slide change
        announceSlideChange();
    }
}

function announceSlideChange() {
    const announcement = document.createElement('div');
    announcement.setAttribute('role', 'status');
    announcement.setAttribute('aria-live', 'polite');
    announcement.className = 'sr-only';
    announcement.textContent = `Slide ${currentSlide} of ${totalSlides}`;

    document.body.appendChild(announcement);

    // Remove after announcement
    setTimeout(() => {
        document.body.removeChild(announcement);
    }, 1000);
}
```

### Screen Reader Only CSS
```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border-width: 0;
}
```

## Performance Optimization

### Debounce Helper
```javascript
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// Usage: Debounce resize events
const handleResize = debounce(() => {
    // Recalculate layout
    console.log('Resize handled');
}, 250);

window.addEventListener('resize', handleResize);
```

### Lazy Load Images
```javascript
function lazyLoadImages() {
    const images = document.querySelectorAll('[data-lazy-src]');

    const imageObserver = new IntersectionObserver((entries, observer) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const img = entry.target;
                img.src = img.getAttribute('data-lazy-src');
                img.removeAttribute('data-lazy-src');
                observer.unobserve(img);
            }
        });
    });

    images.forEach(img => imageObserver.observe(img));
}

// Call after DOM ready
document.addEventListener('DOMContentLoaded', lazyLoadImages);
```

## Complete Example

### Full Implementation
```javascript
(function() {
    'use strict';

    // Config
    const totalSlides = 21;
    let currentSlide = 1;
    let touchStartX = 0;
    let touchEndX = 0;

    // Initialize
    document.addEventListener('DOMContentLoaded', init);

    function init() {
        setupSlides();
        attachListeners();
        handleDeepLink();
    }

    function setupSlides() {
        const slides = document.querySelectorAll('.slide');
        slides.forEach(slide => slide.style.display = 'none');
    }

    function attachListeners() {
        // Keyboard
        document.addEventListener('keydown', handleKeyboard);

        // Buttons
        document.getElementById('prevBtn')?.addEventListener('click', prevSlide);
        document.getElementById('nextBtn')?.addEventListener('click', nextSlide);

        // Touch
        document.body.addEventListener('touchstart', e => {
            touchStartX = e.changedTouches[0].screenX;
        }, { passive: true });

        document.body.addEventListener('touchend', e => {
            touchEndX = e.changedTouches[0].screenX;
            handleSwipe();
        }, { passive: true });

        // Hash changes
        window.addEventListener('hashchange', handleDeepLink);
    }

    function handleKeyboard(e) {
        const actions = {
            'ArrowRight': nextSlide,
            ' ': nextSlide,
            'ArrowLeft': prevSlide,
            'Home': () => showSlide(1),
            'End': () => showSlide(totalSlides)
        };

        if (actions[e.key]) {
            e.preventDefault();
            actions[e.key]();
        }
    }

    function handleSwipe() {
        const threshold = 50;
        const distance = touchEndX - touchStartX;

        if (Math.abs(distance) < threshold) return;

        distance > 0 ? prevSlide() : nextSlide();
    }

    function showSlide(n) {
        // Bounds check
        currentSlide = Math.max(1, Math.min(n, totalSlides));

        // Update DOM
        document.querySelectorAll('.slide').forEach(s => s.style.display = 'none');
        document.querySelector(`[data-slide="${currentSlide}"]`).style.display = 'flex';

        // Update UI
        updateUI();
    }

    function updateUI() {
        const counter = document.getElementById('slideCounter');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');

        if (counter) {
            counter.textContent = `${currentSlide} of ${totalSlides}`;
        }

        if (prevBtn) prevBtn.disabled = currentSlide === 1;
        if (nextBtn) nextBtn.disabled = currentSlide === totalSlides;

        history.replaceState(null, null, `#${currentSlide}`);
    }

    function nextSlide() {
        if (currentSlide < totalSlides) showSlide(currentSlide + 1);
    }

    function prevSlide() {
        if (currentSlide > 1) showSlide(currentSlide - 1);
    }

    function handleDeepLink() {
        const hash = window.location.hash;
        const slideNum = parseInt(hash.substring(1));

        if (!isNaN(slideNum) && slideNum >= 1 && slideNum <= totalSlides) {
            showSlide(slideNum);
        } else {
            showSlide(1);
        }
    }

})();
```

---

*Use these patterns when implementing slideshow navigation and interactivity.*
