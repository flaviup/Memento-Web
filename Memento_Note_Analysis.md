# Memento Note - Rich Text Editor Application

## Overview

Memento Note is a sophisticated single-page HTML application that functions as a rich note-taking editor. This document provides a comprehensive analysis of the application's architecture, features, and implementation.

## 🏗️ App Structure & Architecture

### File Organization
- **Single HTML file** (~5000+ lines) containing everything: HTML, CSS, and JavaScript
- **Modular JavaScript** organized into multiple `<script>` blocks with different purposes
- **Progressive enhancement** with feature detection and fallbacks

### Main Components
- **Header**: Title input and creation/modification dates
- **Toolbar**: Rich formatting controls (sticky at top)
- **Editor**: Main content area with `contenteditable`
- **Footer**: Attribution and version info

## 🎨 Design & Theming

### CSS Variables System
```css
:root {
  --page-bg-color: #f8f9fa;
  --note-bg-color: #fff;
  --control-bg-color: #f8f9fa;
  /* 8 predefined text colors */
  --note-text-color-1: #000000; /* black */
  --note-text-color-2: #FF3B30; /* red */
  /* 8 predefined highlight colors */
}
```

### Dark Mode Support
- Automatic detection via `@media (prefers-color-scheme: dark)`
- Complete color scheme redefinition

### Responsive Design
- Mobile-first approach with `@media (max-width: 800px)`
- Different toolbar layouts for mobile vs desktop
- Print-friendly styles

## 📝 Core Editor Features

### Text Formatting
- **Font sizes**: XL, L, M, S (different text sizes)
- **Font scaling**: 25% to 350% increments
- **Text styles**: Bold, Italic, Underline, Strikethrough
- **Colors**: 8 predefined text colors
- **Highlighting**: 8 predefined highlight colors
- **Alignment**: Left, Center, Right, Justify

### Rich Content Types
1. **Text blocks** - Regular paragraphs with formatting
2. **Code blocks** - Syntax-highlighted code areas
3. **Tables** - Simple and headless table support
4. **Mathematical expressions** - LaTeX-style math using MathUp library
5. **Media embedding**:
   - Images (local files or URLs)
   - Audio files (local or URLs) + audio recording
   - Video links
   - PDFs (local files or URLs)
   - ePub documents
   - Web content (iframes)

## 🔧 Advanced Technical Features

### 1. Undo/Redo System
```javascript
class UndoStack {
  static MAX_UNDO = 10_000;
  // Command pattern implementation
  push(doFn, undoFn, ...args) { /* */ }
  undo() { /* */ }
  redo() { /* */ }
}
```

### 2. Text Transformation Engine
- **LCS Algorithm** (Longest Common Subsequence) for optimal text diffs
- **Binary Indexed Tree** for efficient style application
- **BitArray** for style pattern management
- Smart text selection and styling with conflict resolution

### 3. Audio Recording
```javascript
const audioRecorder = {
  start: function() { /* MediaRecorder API */ },
  stop: function() { /* Returns audio blob */ },
  // Real-time recording with visual feedback
}
```

### 4. File Management
- **Save options**: Download, Save To (File System Access API), Share (Web Share API)
- **Read-only mode**: Stripped-down version for sharing
- **Export formats**: Self-contained HTML files

### 5. Content Management
- **Unique ID generation** for all elements
- **Drag-and-drop** file support
- **X-remover buttons** for deleting embedded content
- **Smart caret positioning**

## 📚 External Libraries Integration

1. **MathUp** - LaTeX math rendering
2. **PDF.js/PDFObject** - PDF embedding and display
3. **ePub.js** - ePub book reader functionality
4. **JSZip** - For ePub file processing

## 💾 Data Persistence

### Metadata Tracking
```javascript
// Automatic timestamping
noteDates.setAttribute("data-created-time", now.toISOString());
noteDates.setAttribute("data-last-saved-time", now.toISOString());
```

### Export Formats
- **Editable HTML**: Full-featured version
- **Read-only HTML**: Cleaned version without editing capabilities
- **Self-contained**: All assets embedded as base64

## 🔄 State Management

### Editor State
- **Selection tracking** for toolbar button states
- **Content validation** and cleanup
- **Cross-browser compatibility** with vendor prefixes
- **Event delegation** for dynamic content

### UI State
- **Dropdown management** (color pickers, menus)
- **Mobile/desktop layout switching**
- **Recording status indicators**

## 🎯 Key Innovations

1. **Smart Text Styling**: Uses advanced algorithms to efficiently apply overlapping text styles
2. **Embedded Document Viewer**: Can display PDFs and ePubs inline
3. **Audio Recording Integration**: Direct in-browser audio capture
4. **Mathematical Expression Support**: LaTeX rendering
5. **Responsive Toolbar**: Adapts to screen size automatically
6. **No Backend Required**: Everything runs client-side

## 🚀 Performance Optimizations

- **Lazy loading** of heavy libraries (PDF, ePub, Math)
- **Event delegation** for dynamic content
- **Memory management** with cleanup functions
- **Efficient DOM manipulation** with DocumentFragment
- **Debounced operations** for real-time features

## 🏛️ Architecture Deep Dive

### Module Organization
The application is organized into several JavaScript modules:

#### Core Module (`core-script`)
- Basic helper functions
- ID generation
- Caret positioning
- Element insertion/removal
- Inter-module communication object

#### ePub Module (`epub-loading-script`)
- ePub book loading and display
- Navigation controls
- Table of contents generation
- Book destruction cleanup

#### Content Editing Module (`content-editing-script`)
- Advanced text styling system
- Undo/redo stack implementation
- Media insertion functions
- PDF handling with PDFObject
- Audio recording management
- Mathematical expression support

### Advanced Algorithms

#### Longest Common Subsequence (LCS)
Used for efficient text transformation:
```javascript
function getLCS(a, b) {
  // Dynamic programming approach
  // Finds optimal sequence of changes
}
```

#### Binary Indexed Tree for Styling
Efficiently applies overlapping text styles:
```javascript
function getStyledPositions(positions, styles, maxTextLength) {
  // Uses bit arrays for style pattern management
  // Handles complex style overlaps
}
```

#### BitArray Implementation
Custom bit manipulation for style patterns:
```javascript
class BitArray {
  set(i) { /* Set bit */ }
  clear(i) { /* Clear bit */ }
  bitOr(ba) { /* Combine patterns */ }
  bitAnd(ba) { /* Intersect patterns */ }
}
```

## 🔍 Detailed Feature Analysis

### Text Styling System
The application implements a sophisticated text styling system that can handle:
- **Overlapping styles** (bold + colored + highlighted text)
- **Partial style application** (applying bold to part of already colored text)
- **Style removal** (removing specific styles while preserving others)
- **Conflict resolution** (handling competing styles)

### Media Embedding Architecture
Each media type has its own insertion and management system:

#### Images
- Support for local files and URLs
- Base64 encoding for local files
- Responsive sizing
- Removal controls

#### Audio
- Local file support
- URL streaming
- Live recording with MediaRecorder API
- Real-time recording indicators
- Export as embedded data

#### PDFs
- Browser PDF support detection
- Fallback to PDF.js viewer
- Downloadable links for unsupported browsers
- Configurable viewing parameters

#### ePub Books
- Full ePub.js integration
- Interactive table of contents
- Navigation controls
- Keyboard shortcuts (arrow keys)
- Chapter-based navigation

### Responsive Design Implementation
- **CSS Grid** for toolbar layout
- **Flexbox** for content alignment
- **Media queries** for breakpoint management
- **Touch-friendly** controls on mobile
- **Print optimization** with specific styles

## 🔒 Security Considerations

### Content Sanitization
- Controlled `contenteditable` implementation
- Attribute filtering for embedded content
- XSS prevention through controlled HTML generation

### File Handling
- Type validation for uploaded files
- Size limitations through browser constraints
- Safe base64 encoding for local files

## 🎛️ User Interface Design

### Toolbar Architecture
- **Sticky positioning** for constant access
- **Responsive breakdown** for mobile devices
- **Dropdown menus** for complex controls
- **Visual feedback** for active states
- **Keyboard shortcuts** support

### Content Management
- **Visual content blocks** with hover controls
- **Drag indicators** for media elements
- **Removal confirmations** for destructive actions
- **Undo/redo visual states** in toolbar

## 📱 Cross-Platform Compatibility

### Browser Support
- **Modern browsers** with feature detection
- **Fallbacks** for unsupported features
- **Progressive enhancement** approach
- **Vendor prefix** handling for older browsers

### Device Adaptations
- **Touch interfaces** optimization
- **Keyboard navigation** support
- **Screen reader** compatibility considerations
- **High DPI** display support

## 🔄 Data Flow Architecture

### Event Handling
- **Event delegation** for dynamic content
- **Custom event system** for module communication
- **State synchronization** across components
- **Memory leak prevention** with proper cleanup

### Content Persistence
- **Local state** management
- **Export/import** functionality
- **Version tracking** with timestamps
- **Conflict resolution** for concurrent edits

## 🎯 Conclusion

This is an impressive example of a modern, feature-rich web application built with vanilla JavaScript, showcasing:

- **Advanced DOM manipulation** techniques
- **Complex state management** without external frameworks
- **Multimedia handling** with modern web APIs
- **Mathematical rendering** integration
- **Document viewing** capabilities
- **Audio recording** functionality
- **Responsive design** principles
- **Performance optimization** strategies

The application demonstrates how a single HTML file can contain a fully-featured, production-ready note-taking application with enterprise-level features, all running entirely client-side without requiring any backend infrastructure.

---

*Analysis completed: 2025-07-10*  
*File: memento.html (~5000+ lines)*  
*Version: 1.0.20250710*
