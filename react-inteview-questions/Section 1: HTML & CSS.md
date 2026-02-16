# HTML & CSS Interview Questions

## Section 1: HTML & CSS

- [1. Make Responsive Webpage](#1-make-responsive-webpage)
- [2. CSS Grid vs CSS Flexbox](#2-css-grid-vs-css-flexbox)
- [3. Browser Compatibility](#3-browser-compatibility)
- [4. Pseudo Class vs Pseudo Element](#4-pseudo-class-vs-pseudo-element)
- [5. CSS Positions](#5-css-positions)
- [6. Visibility vs Display](#6-visibility-vs-display)
- [7. Transform in CSS](#7-transform-in-css)
- [8. CSS Preprocessors (SASS/LESS)](#8-css-preprocessors-sassless)
- [9. Critical Rendering Path (CRP) and Optimization](#9-critical-rendering-path-crp-and-optimization)
- [10. Box Sizing](#10-box-sizing)
- [11. What are Semantic Elements?](#11-what-are-semantic-elements)
- [12. Accessibility (ADA, WCAG 2.1)](#12-accessibility-ada-wcag-21)
- [13. HTML Tags (doctype, head, meta)](#13-html-tags-doctype-head-meta)
- [14. Advanced CSS (Transforms, Animations, Gradients)](#14-advanced-css-transforms-animations-gradients)
- [15. Inline 5 <div>s Without Flex/Grid](#15-inline-5-divs-without-flexgrid)
Coding: 
- [16. How to Center Div (Multiple Methods)](#16-how-to-center-div-multiple-methods)
- [17. Change Background Based on Screen](#17-change-background-based-on-screen)
- [18. Center Text and Block Elements](#18-center-text-and-block-elements)

# Section 1: HTML & CSS

## 1. Make Responsive Webpage
Expected Answer: Use fluid layouts, media queries, flexible images, and responsive units. Hints:
•	Media Queries: CSS rules breakpoints to change the layout for different devices.
•	Relative units: rem, %, vw/vh instead of px
•	Flexible images: max-width: 100%; height: auto;
•	Viewport units or Responsive typography with clamp()
•	Using Flexbox and CSS grid: use to create responsive layouts
•	Viewport Meta Tag: The viewport meta tag in HTML is used to control how the webpage is initially displayed on a mobile device, ensuring proper scaling. 
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## 2. CSS Grid vs CSS Flexbox
Expected Answer:,  Grid is for 2-dimensional layouts (rows AND columns simultaneously). Hints:
Flexbox: Flexbox is for 1-dimensional layouts (row OR column)
-	Great for aligning items inside a container (nav bars, toolbars, card content).
-	display: flex, good for navigation bars, Image cards in a row.
-	Controls order, alignment, spacing along single axis (justify-content, align-items).
-	Use Flexbox for small-scale, linear arrangements. 
Grid: Two-dimensional layout (rows and columns).
-	Best for page layouts and complex component grids.
-	Define explicit tracks with grid-template-columns/rows, place items precisely.
-	Use Grid for overall page/grid layout. They can be combined: grid for structure, flex for inside-card layout.
```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr; /* Define columns */
  grid-template-rows: 100px auto; /* Define rows */
}
```
CSS Flexbox and Grid responsiveness is achieved by properties like flex-wrap: wrap, min-width, max-width,  grid-template-columns, and media queries.

## 3. Browser Compatibility
Expected Answer: Use @supports, feature detection, and progressive enhancement. Hints:
•	@supports (display: grid) { /* grid styles */ }
•	Progressive enhancement: basic functionality for all, enhanced for modern browsers
•	Polyfills for missing features, vendor prefixes for experimental features
•	add a vendor prefix to a CSS property -webkit- for Chrome, Safari -moz- for Firefox, tools: Autoprefixer

## 4. Pseudo Class vs Pseudo Element
Expected Answer: Pseudo-classes target element specific states or or relationship to other elements, pseudo-elements target specific parts of an element for selecting or styling without adding additional class. Hints:
•	Pseudo-class: :hover, :focus, :nth-child(n) (single colon)
•	Pseudo-element: ::before, ::after, ::first-line (double colon)
•	:first-child targets an element, ::first-letter targets text part

## 5. CSS Positions
Expected Answer: Control element positioning with static, relative, absolute, fixed, and sticky values. Hints:
•	Static: default, Element follows the normal document flow.
•	Relative (Parent): The element remains in the normal document flow, but can be offset with top, right, bottom, left.
```css
.parent {
  position: relative;
  top: 20px;  /* Moves down 20px from original position */
  left: 30px; /* Moves right 30px from original position */
}
```

•	Absolute (child): The element is removed from the normal document flow.
positioned relative to nearest positioned ancestor. If no parent has a set position (i.e., they are all static), the element defaults to the browser window (viewport). 
```css
.child {
  position: absolute;
  top: 0;
  right: 0; /* Positions at top-right corner of parent */
}
```

•	Fixed: positioned relative to viewport, stays on scroll
•	Sticky: hybrid of relative and fixed based on scroll position

## 6. Visibility vs Display
Expected Answer: visibility: hidden hides element but preserves space, display: none removes element from layout. Hints:
•	visibility: hidden: element invisible but occupies space
•	display: none: element completely removed from layout
•	opacity: 0: element invisible but still interactive

## 7. Transform in CSS
Expected Answer: Apply 2D/3D transformations to elements without affecting document flow. Hints:
•	Translate: transform: translate(50px, 100px);
•	Scale: transform: scale(1.5);
•	Rotate: transform: rotate(45deg);
•	Skew: transform: skew(20deg);
•	3D: transform: rotateX(45deg);

## 8. CSS Preprocessors (SASS/LESS)
Expected Answer: Use SASS features like variables, nesting, mixins, and functions to write maintainable CSS. Hints:
•	Variables for consistent theming and colors
•	Mixins for reusable code patterns and vendor prefixes
•	Nesting for component-scoped styles (avoid deep nesting)
•	Functions and operators for calculations

## 9. Critical Rendering Path (CRP)  and Optimization
Critical rendering path (CRP) is the sequence of steps a browser takes to convert HTML, CSS, and JavaScript into pixels on the screen, encompassing DOM/CSSOM construction, layout, and painting.
Steps in the Critical Rendering Path
1.	DOM Construction (Document Object Model): The browser parses HTML to create the object model of the page.
2.	CSSOM Construction (CSS Object Model): The browser parses CSS to create the style model.
3.	Render Tree Construction: The DOM and CSSOM are combined to form a tree representing what will be rendered.
4.	Layout (Reflow): The browser calculates the geometry (position and size) of each element in the viewport.
5.	Paint: The browser fills in pixels on the screen (colors, borders, images). 
Optimization: CSS for faster page rendering and better performance.
•	Inline critical CSS in <head>
•	Defer non-critical CSS with media="print" or preload
•	Minify and compress CSS files
•	Remove unused CSS with tools like PurgeCSS
•	Optimize JavaScript Execution: Use async or defer attributes to prevent scripts from blocking DOM construction. 
For async and defer: Scripts with these attributes do download in parallel while HTML parses
async    = Downloads script in parallel while HTML parsing, then PAUSES parsing to execute immediately upon download completion.
defer    = Downloads script in parallel while HTML parsing, but WAITS to execute until AFTER parsing is 100% complete, just before DOMContentLoaded.
normal   = (no async/defer) = STOPS HTML parsing to download, then executes immediately, then resumes parsing.

## 10. Box Sizing
Expected Answer: Controls how an element's total width and height are calculated when you set width and height properties. Hints:
•	content-box (not recommended): default, width/height = content only. If width = 200px, other left, right padding, left, right border will be added
```css
box-sizing: content-box; /* Default */
```
•	border-box: Padding and border are included in the specified width/height. If width = 200px, final width will be 200px.
```css
 box-sizing: border-box;
 Specified Width = Content + (Left Padding + Right Padding) + (Left Border + Right Border)
```
•	Universal: *, *::before, *::after { box-sizing: border-box; }

## 11. What are Semantic Elements?
Semantic elements (Meaningful HTML) are HTML tags that clearly describe their meaning to both the browser and the developer. 
<header>, <nav (navigation link)>, <main (main content)>, <footer>, <aside (sidebar)>, <figure>, <figcaption>, <h1 (h1-h6 Heading labels)>
Benefits: *Navigation Benefits/Keyboard navigation, Screen Reader advantage, *SEO friendly easy to index by google, *Clear structure for other developers.

## 12. Accessibility (ADA, WCAG 2.1): 
The practice of making products (webapp) and services usable by as many people as possible, regardless of their physical or different abilities.
•	Alt Text: Provide descriptive text for every meaningful image so screen readers can explain it. Use empty alt text (alt="") for purely decorative images to skip them.
•	Color Contrast: Maintain a minimum contrast ratio of 4.5:1 for normal text and 3:1 for large text or UI components.
•	Keyboard Navigation: Ensure every link, button, and form can be used without a mouse. Use a visible focus indicator (like a blue border) so users know where they are.
•	Semantic Elements/Structure: Use proper HTML tags (e.g., <h1> through <h6>) in order to create a logical page hierarchy. No role required when using semantic elements.
•	Use Native HTML First: Use <button> instead of <div role="button"> whenever possible because native elements have built-in accessibility.
•	ARIA Attributes: Use aria-label, aria-labelledby, aria-current or roles.
•	Language: Specify the site's language in the header (e.g., <html lang="en">) so screen readers use the correct pronunciation. 
Testing:
•	Voice Over on Mac (test: NVDA, VoiceOver, JAWS): Screen Reader
•	Automated: WAVE, axe DevTools, or Google Lighthouse.
•	Manual: Unplug your mouse and try navigating your site using only the Tab, Enter, and Arrow keys.

Details of Aria Attributes:
Use aria-label when: No visible text exists: The element (button, link, region) has no text content, only an icon or image. Note: aria-labelledby takes precedence over aria-label if both are present. 
```html
<button aria-label="Close" onclick="..."><span class="icon-x"></span></button>
```

Use aria-labelledby when: Visible text already exists: A label for a form field or widget is already visible on the screen. Linking a form input to a specific, unique header, even if that header is not directly adjacent. 
<h2 id="billing">Billing</h2> followed by <div role="form" aria-labelledby="billing">...</div>

Use aria-current when: Indicating the current page/location: Identifying which link in a breadcrumb or navigation menu represents the page the user is currently viewing. 
Better for multi-step process like pagination, calendar date Values: Can be used with aria-current="page", step, location, date, time, true, or false
Example: <a href="/home" aria-current="page">Home</a>

Use ARIA roles when:
The element lacks native semantics: Use roles for custom widgets (like a <div> acting as a button or creating custom button) that don't exist in standard HTML.

## 13. HTML Tags (doctype, head, meta)
<!DOCTYPE html> is an instruction to the web browser about which version of the HTML markup language the page is written in. For modern web projects, HTML5. 
<head> Tag :It contains elements that define the page's title, link to external stylesheets and scripts (including the main bundle for your React app), and provides other critical information. It is is vital for how the browser, search engines, and social media platforms interact with the page
<meta> Tags
<meta> tags are used to specify metadata within the <head> section. They provide additional information about the document that can't be defined by other head elements (like <title> or <link>). 
Common uses in a React project include:
•	Character Encoding: <meta charset="UTF-8"> is essential for ensuring the browser correctly displays text characters.
•	Viewport Settings: <meta name="viewport" content="width=device-width, initial-scale=1.0"> is critical for responsive web design, telling mobile devices how to scale the page correctly.
•	SEO and Social Media:
o	<meta name="description" content="..."> provides a summary of the page content for search engines.

## 14. Advanced CSS (Transforms, Animations, Gradients)
Expected Answer: Create smooth animations and visual effects using CSS transforms, transitions, and gradients. Hints:
•	Use transform (translate, scale, rotate) for better performance than top/left
•	CSS transitions for simple state changes, keyframes for complex animations
•	Linear/radial gradients for background effects without images
•	Hardware acceleration with transform: translateZ(0) or will-change

## Practical HTML/CSS Questions
Expected Answer: Implement semantic HTML, accessible forms, and modern CSS techniques. Hints:
•	Semantic tags (article, section, nav, header, footer) for SEO and accessibility
•	ARIA attributes for screen reader support
•	CSS methodologies (BEM, SMACSS) for maintainable code
•	CSS custom properties (variables) for theming

## 15. Inline 5 <div>s Without Flex/Grid
Expected Answer: Use CSS properties like display: inline-block, float, or display: table to horizontally align elements without flexbox or grid. Hints:
•	Use display: inline-block with vertical-align: top and font-size: 0 on parent to remove whitespace gaps
•	Alternative: float: left requires clearfix on parent container
•	Can also use display: table-cell on children with display: table on parent

## 16. How to Center Div (Multiple Methods)
Expected Answer: Multiple techniques including Flexbox, Grid, positioning, and margin auto. Hints:
•	Flexbox: display: flex; justify-content: center; align-items: center;
•	Grid: display: grid; place-items: center;
•	Position + transform: position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
•	Margin auto: margin: auto; (requires fixed width)

## 17. Change Background Based on Screen
Expected Answer: Use media queries for responsive design and prefers-color-scheme for dark/light mode. Hints:
•	Media query: @media (max-width: 768px) { background: red; }
•	Dark mode: @media (prefers-color-scheme: dark) { background: #333; }
•	Combine: @media screen and (max-width: 600px) and (prefers-color-scheme: dark)

## 18. Center Text and Block Elements
Expected Answer: Different techniques for centering text vs block elements. Hints:
•	Text: text-align: center;
•	Inline/inline-block: text-align: center on parent
•	Block elements: use Flexbox, Grid, or margin auto
•	Vertical text: line-height equal to container height
