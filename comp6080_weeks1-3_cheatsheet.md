# COMP6080 Weeks 1–3 Cheatsheet: HTML, CSS, Vanilla JS

Built from your actual lecture slides (HTML Fundamentals, CSS Layouts, CSS Flexbox, CSS Formatting, CSS Mobile, DOM, Events, Forms, Dev Tools) plus Assignment 1 (pixel-replication) and Assignment 2 (FormFiddle) specs. Weighting below: **~65% HTML/CSS, ~35% JS**, since that's what you said the quiz leans toward.

---

## PART A — HTML (slides: "HTML Fundamentals")

### The three layers
- **HTML** = structure/content, **CSS** = presentation/style, **JS** = behaviour/interactivity. This divide shows up constantly in quiz questions ("which layer handles X").

### Document skeleton
```html
<!DOCTYPE html>
<html lang="en-AU">
<head>
  <meta charset="UTF-8">
  <title>Page title</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  ...
</body>
</html>
```
- `<!DOCTYPE html>` tells the browser to use modern standards mode.
- `<head>` holds things that are *not* rendered directly: `<title>`, `<meta>` tags, CSS `<link>`, `<script>` tags.
- `<body>` holds everything that *is* rendered.
- `lang` attribute on `<html>` (e.g. `lang="en-AU"`) matters for SEO and accessibility — slides flagged this explicitly.

### Layout / structural tags
`<div>` (generic block container), `<span>` (generic inline container), `<p>`, `<h1>`–`<h6>`, `<ul>`/`<ol>` + `<li>`, `<table>` (+ `<tr>`/`<td>`/`<th>`).
- `<div>` vs `<span>`: div is block-level (own line), span is inline (sits in text flow) — this is the HTML-level version of the block/inline distinction you also see in CSS.

### Formatting tags
`<b>` (bold), `<i>` (italic), `<u>` (underline) — visual-only, no semantic meaning (contrast with CSS `font-weight`/`font-style` which do the same visually but are the "correct" way to do it in a styled page).

### Links and images
```html
<a href="https://example.com" title="tooltip text" target="_blank">Visit</a>
<a href="file.pdf" download>Download</a>
<img src="pic.png" alt="Description of pic" width="200" height="100">
```
- `href` = destination URL. `title` = tooltip on hover. `target="_blank"` = opens in new tab. `download` = forces download instead of navigation.
- `<img>` is a **void/self-closing element** — no closing tag. `alt` is required for accessibility/screen readers and shows if the image fails to load. `src`, `width`, `height` round out the common attributes.

### Forms — overview (slides: HTML Fundamentals + JS Browser Forms)
```html
<form>
  <label for="name">Name:</label>
  <input type="text" id="name" name="firstname" placeholder="Jane" disabled>

  <textarea rows="4" cols="30" placeholder="Comments"></textarea>

  <select>
    <option value="nsw">NSW</option>
    <option value="vic">VIC</option>
  </select>

  <button type="submit">Submit</button>
  <input type="submit">
</form>
```
- `<input>` types you should recognise: `text`, `email`, `number`, `checkbox`, `radio`, `submit`, `password`.
- Common `<input>` attributes: `type`, `name`, `value`, `placeholder`, `disabled`.
- `<textarea rows cols placeholder>` — multi-line text.
- `<select><option value="...">Label</option></select>` — dropdown; `value` is what's read in JS/on submit, the text between the tags is what's displayed.
- `<label for="id">` — must match the target input's `id` to be properly associated (clicking the label focuses the input — accessibility requirement).
- `<button>` vs `<input type="submit">` — both submit a form by default when inside one; `<button>` can contain arbitrary HTML content (e.g. an icon + text), `<input type="submit">` can only show plain text via `value`.

### Media embedding
`<iframe>` (embed another page), `<audio>`, `<video>` — all mentioned in the fundamentals deck as embeddable media tags.

### document.forms / form.elements (slides: JS Browser Forms — ties HTML forms to JS)
```js
document.forms.myForm          // form with name="myForm"
document.forms["myForm"]       // same, bracket notation
document.forms[0]               // first form on the page

const form = document.forms[0];
form.elements.fname;            // input with name="fname"
form.fname;                     // shorthand — same thing
const ages = form.elements.age; // multiple same-name elements → collection
```
- Every form element has a **backreference** to its form: `input.form` points back to the `<form>` it belongs to.
- Reading values:
  - `input.value` — text/textarea current value
  - `input.checked` — boolean, checkbox/radio
  - `select.options` — collection of `<option>`s; `select.value` — current selection's value; `select.selectedIndex` — its index
- `<input type="submit">` triggers a `"submit"` event on the `<form>` when clicked — this is the standard pattern for form validation (see JS section below).

---

## PART B — CSS

### Selectors & specificity
- Element `p {}`, class `.my-class {}`, ID `#my-id {}`, descendant `.parent .child {}`, direct child `.parent > .child {}`, pseudo-class `:hover`/`:focus`/`:nth-child(n)`, pseudo-element `::before`/`::after` (need `content: ""`), attribute `input[type="checkbox"] {}`.
- **Specificity, highest to lowest:** inline style > ID > class/attribute/pseudo-class > element. Equal specificity → last declared wins. This is a classic short-answer question format.

### The Box Model (slides: CSS Layouts)
Every element = `content → padding → border → margin`, outside-in.
- `box-sizing: content-box` (default): declared `width`/`height` = content only, padding/border add on top of that.
- `box-sizing: border-box`: declared `width`/`height` **include** padding and border. This was the exact bug in your Assignment 1 `.white-box` — without `border-box`, your rendered box was bigger than the 885px/909px you declared because padding added on top.

### Display types (slides: CSS Layouts)
- **Block**: takes full available width, forces a new line before/after, can contain both block and inline children (`div`, `p`, `h1`–`h6`, `ul`, `table`).
- **Inline**: only as wide as its content, sits within the text flow, does *not* break the flow, can only contain text/other inline elements — width/height have **no effect** on a plain inline element (`span`, `a`, `strong`).
- **Inline-block**: flows inline like `inline`, but accepts `width`/`height`/vertical `margin` like `block`. The middle ground.
- `display: none` — removes the element from the layout entirely (no space reserved).
- `visibility: hidden` — hides the element visually but **still reserves its space** in the layout. Classic "what's the difference" quiz pairing.

### Float & overflow (slides: CSS Layouts)
- `float: left/right` — removes the element from normal flow and pushes it to one side; text and inline content wrap around it.
- `overflow`: `visible` (default, content spills out), `hidden` (clipped), `auto` (scrollbar only if needed), `scroll` (always shows scrollbar).

### Positioning (slides: CSS Layouts)
- `static` (default) — normal flow, `top`/`left`/etc. have no effect.
- `relative` — stays in normal flow (space still reserved), but `top`/`right`/`bottom`/`left` shift it *from where it would otherwise be*. Also becomes the positioning **anchor** for any `absolute` descendants.
- `absolute` — removed from flow entirely, positioned relative to the nearest ancestor that is `relative`/`absolute`/`fixed` (or the page/`<html>` if none exists). This is why Assignment 1's overlapping Gyarados images worked: wrap in a `relative` container, then `absolute`-position the children inside it.
- `fixed` — removed from flow, positioned relative to the **browser viewport**, stays in place when the page scrolls. This is exactly how your Task 1 `.chat-bubble` was pinned to the bottom-right of the window regardless of scroll.
- `sticky` — acts `relative` until a scroll threshold is crossed, then sticks like `fixed`.
- `z-index` — controls stacking order on overlapping elements (higher = closer to viewer); **only has an effect on positioned elements** (anything other than `static`). Easy quiz trap: `z-index` on a `static` element silently does nothing.
- Only `position: static | relative | absolute | fixed | sticky` are valid values — a value like `center` is invalid and silently does nothing (a real bug you hit).

### Flexbox (slides: CSS Flexbox) — one-dimensional layout
```css
.flex-container {
  display: flex;
  flex-direction: row;       /* row | row-reverse | column | column-reverse */
  justify-content: center;   /* main-axis: flex-start | flex-end | center | space-between | space-around */
  align-items: center;       /* cross-axis: stretch | flex-start | flex-end | center */
  flex-wrap: wrap;
  gap: 16px;
}
```
- `flex-direction` sets which axis is the "main axis" — `row` lays children left-to-right, `column` top-to-bottom; the `-reverse` variants flip the order.
- `justify-content` controls spacing **along the main axis** (e.g. horizontal spacing in a row): `flex-start`/`flex-end` bunch items to one side, `center` bunches them in the middle, `space-between` spreads them with no outer gap, `space-around` spreads them with even outer gap too.
- `align-items` controls alignment **along the cross axis** (e.g. vertical alignment in a row): `stretch` (default — fills the container), `center`, `flex-start`, `flex-end`.
- **Common trap in your own code**: `justify-content`/`align-items` only do anything if the *parent* has `display: flex` (or `grid`) — you hit this bug directly in Task 1.
- On children: `flex: 1` (grow to fill available space), `flex-grow`, `flex-shrink`, `flex-basis`, `align-self` (override `align-items` for one child).

### Grid — two-dimensional layout
```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
  grid-template-rows: auto;
  gap: 15px;
}
```
Used in Task 2 for the 3-card route layout (`repeat(3, 1fr)`). `grid-column`/`grid-row` span/place specific items when you need irregular layouts.

### Text & formatting (slides: CSS Formatting)
- `color` — text colour. `font-size` — text size. `font-family` — ordered fallback list of fonts to try. `font-weight` — boldness (e.g. `400` normal, `700` bold — this is how Assignment 1 pulled in specific Roboto/Roboto Condensed weights via Google Fonts). `font-style` — `italic`/`oblique`/`normal`. `text-decoration` — underline/strikethrough/etc.

### Backgrounds (slides: CSS Formatting) — relevant to both tasks' hero/promo image sections
- `background-image: url("img.jpg")` — sets an image as the background. **Important distinction for quiz purposes**: a background-image is purely decorative — it cannot be interacted with/selected/alt-texted like a real `<img>`, so meaningful content images should always be real `<img>` tags, not CSS backgrounds.
- `background-size`: `contain` (scales image to fit entirely inside the box, may leave empty space, no cropping) vs `cover` (scales image to fill the box completely, cropping whatever overflows) — same cover/contain logic as `object-fit` on `<img>`, just for CSS backgrounds instead.
- `background-position` — sets the X/Y anchor of the image within its box (e.g. `center`, `top right`).
- `background-repeat` — default is `repeat` (tiles in both directions until it fills the box); `repeat-x`/`repeat-y` tile in one direction only; `no-repeat` shows it once.

### `object-fit` / `object-position` — for real `<img>` elements
- Only work on **replaced elements**: `<img>`, `<video>`, `<canvas>` — **not** on `<div>`s. (This was the exact bug behind your Task 1 Gyarados image not cropping correctly — the properties were on a wrapping `<div>` instead of the `<img>` itself.)
- `object-fit: cover` — scale to fill the box, cropping overflow, no distortion.
- `object-fit: contain` — scale to fit entirely inside the box, no cropping.
- `object-position` — biases which part of the image survives the crop (e.g. `right center` keeps the right-hand side of the image visible, as used to keep Gyarados in frame).

### Responsive design & mobile (slides: CSS Mobile)
- **Mobile-subdomain approach** (separate `m.example.com` site) vs **responsive-website approach** (one site, CSS adapts). Slides cover pros/cons of each — responsive is now the standard approach, but duplicate-site approaches can be simpler to build and fully tailor to small screens; responsive means one codebase but more complex CSS.
- `@media` query syntax:
  ```css
  @media screen and (max-width: 768px) {
    .container { flex-direction: column; }
  }
  ```
  - **Media types**: `all`, `print`, `screen`, `speech`.
  - **Media features**: `min-width`/`max-width`, `hover`, `prefers-color-scheme`.
  - Combine conditions with `and`, combine alternatives with a comma (OR logic), negate with `not`.
- **Viewport**: the visible area of a page in the browser. CSS pixels ≠ physical device pixels (a high-density screen packs more physical pixels into each CSS pixel). Without the viewport meta tag, mobile browsers render at a wide "desktop" virtual width and zoom out, so pages look tiny.
  ```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  ```
  This tells the browser to make the viewport width match the device's actual width.
- **Viewport units**: `vw` = 1% of viewport width, `vh` = 1% of viewport height. **Quiz-worthy gotcha from the slides**: `100vh` is "counter-intuitive" on mobile because the address bar can show/hide as you scroll, changing the *actual* visible viewport height — so `100vh` can be taller than what's visibly on screen or cause jumpy layout. This is directly relevant to Assignment 1's Task 3, which must be responsive between a mobile and desktop breakpoint.

### Units recap
`px` (fixed), `%` (relative to parent), `em` (relative to parent's font-size), `rem` (relative to root/`<html>` font-size), `vw`/`vh` (relative to viewport).

---

## PART C — Dev Tools (slides: Dev Tools) — practical/short-answer territory

- Open DevTools: `Cmd+Opt+I` (Mac) / `Ctrl+Shift+I` (Windows/Linux).
- **Select Element tool** (the cursor/arrow icon) — click it, then click any element on the page to jump straight to it in the Elements panel.
- **Editing the DOM live**: double-click text/attributes to edit, `Delete` key removes the selected element, drag-and-drop reorders elements, pressing `H` toggles `visibility:hidden` on the selected element as a quick hide/show.
- **Styles tab** — shows every CSS rule applying to the selected element, in specificity/cascade order, with checkboxes to toggle rules on/off live and autocomplete when typing new property names/values.
- **Computed tab** — shows the final computed value of every CSS property after the cascade resolves, including a live box-model diagram (content/padding/border/margin, with actual pixel numbers) for the selected element.
- **Force element state** — lets you pin an element into `:active`, `:hover`, `:focus`, `:visited`, or `:focus-within` so you can inspect/style states that normally only show momentarily.
- Visual tools: colour picker for any colour value, box-shadow visual editor.
- **Local Changes panel** — shows a diff of every CSS/HTML edit you've made in DevTools during the session, so you can copy them back into your real source files.

---

## PART D — JavaScript (DOM, Events — this is Assignment 2's whole territory)

### Variables & basics
- `let` (block-scoped, reassignable), `const` (block-scoped, cannot be reassigned — but array/object *contents* can still mutate), `var` (function-scoped, avoid).
- `===`/`!==` (strict, no type coercion) — always prefer over `==`/`!=`.
- Template literals: `` `Hello ${name}` `` — backticks + `${}` interpolation.

### The DOM (slides: JS Browser DOM)
- The **DOM** (Document Object Model) is a tree-structured in-memory representation of the page that JS can read and manipulate.
- **Selecting elements**:
  ```js
  document.getElementById('id')
  document.getElementsByTagName('div')     // live HTMLCollection
  document.getElementsByClassName('cls')   // live HTMLCollection
  document.querySelector('.class')         // first match, any CSS selector
  document.querySelectorAll('.class')      // NodeList of ALL matches — iterate with .forEach()
  ```
- **Creating/modifying elements**:
  ```js
  const el = document.createElement('div');
  const txt = document.createTextNode('hello');
  el.appendChild(txt);
  parent.appendChild(el);
  parent.removeChild(el);
  el.setAttribute('data-id', '42');
  ```
- **Styling via JS**: `element.style.propertyName = value` — note **camelCase** property names in JS even though CSS uses kebab-case (e.g. `element.style.backgroundColor = 'red'` for the CSS property `background-color`).
- `window.getComputedStyle(element)` — reads the final resolved style of an element (like the Computed tab in DevTools, but from code).
- `classList.add()/.remove()/.toggle()/.contains()` — the standard way to manipulate CSS classes from JS rather than overwriting `style` directly.
- `.textContent` (plain text, safe) vs `.innerHTML` (parsed as HTML — more powerful but can introduce bugs/injection risk if misused).

### Events (slides: JS Browser Events — the core of Assignment 2)
- **What is an event?** A signal that something has happened to a DOM element — the element loading, being clicked, a key being pressed, etc. We use events to run JS in response to user actions.
- **Examples of event types**:
  - Mouse: `click`, `dblclick`, `mouseup`, `mousedown`, `mouseenter`, `mouseleave`
  - Keyboard: `keydown`, `keypress`, `keyup`
  - Other: `load`, `error`, `submit`, `fullscreenchange`, `animationstart`
- **Three ways to add an event handler** (slides walk through all three, in this order, worst→best):
  1. Inline HTML attribute — `<input onclick="alert('Clicked!')">` — works but mixes JS into HTML, generally avoided.
  2. DOM property — `element.onclick = () => { ... }` — **only one handler can be attached this way**; assigning a second overwrites the first.
     - **Classic slide "what's wrong with this code" trap**: `element.onclick = doSomething();` — the parentheses *call* `doSomething` immediately and assign its return value (often `undefined`) to `onclick`, instead of assigning the function itself. Correct: `element.onclick = doSomething;` (no parentheses).
  3. `addEventListener` — **the correct/standard approach**, and what Assignment 2 expects:
     ```js
     target.addEventListener(type, listener, [options]);
     // e.g.
     element.addEventListener('click', handler);
     element.removeEventListener('click', handler); // can remove it later too
     ```
     Unlike the DOM-property approach, `addEventListener` lets you attach **multiple** handlers to the same event on the same element.
- **The event object**: the callback receives an event object as its parameter.
  ```js
  document.addEventListener('mousemove', (event) => {
    console.log(event.clientX, event.clientY);
  });
  ```
  - General properties on every event: `event.currentTarget` (element the handler is running on), `event.timeStamp`, `event.type` (e.g. `'click'`).
  - Type-specific properties: a `MouseEvent` has `event.clientX`/`clientY`; a `KeyboardEvent` has `event.key`.
  - `event.target` — the actual element that triggered the event (useful when a handler is on a parent but you need to know which child was clicked — relevant for Assignment 2's dynamically-added checkboxes).
- **preventDefault** — many elements have built-in default behaviour: clicking a checkbox toggles it, dragging an image moves it, typing in a text field enters the text. `event.preventDefault()` stops that default behaviour — most commonly used on a form's `submit` event so the page doesn't reload:
  ```js
  form.addEventListener('submit', (event) => {
    event.preventDefault();
    // validation / custom handling goes here
  });
  ```
- **Event capturing and bubbling**: when an event fires on a nested element, it goes through three phases —
  1. **Capture phase** — the event travels *down* from `window` → `document` → ancestors → the target.
  2. **Target phase** — the event reaches the actual element clicked.
  3. **Bubbling phase** — the event then travels back *up* through all the same ancestors.
  By default, `addEventListener` listens during the bubbling phase.
- **The event loop**: JavaScript is single-threaded. The event loop constantly checks a queue: when an event fires, it's added to the queue; the loop only picks up the next queued event once the currently-running code has **fully finished** (run-to-completion model) — JS will never interrupt a running function to handle a new event mid-way.

### FormFiddle-style pattern (ties DOM + Events + Forms together — exactly Assignment 2's shape)
```js
function render() {
  const name = document.getElementById('preferredName').value.trim();
  // validate with regex, e.g. /^[a-zA-Z]{3,50}$/.test(name)
  // build output string with a template literal
  document.getElementById('output').value = outputString;
}

document.getElementById('preferredName').addEventListener('blur', render);

document.getElementById('selectAll').addEventListener('change', () => {
  const checkboxes = document.querySelectorAll('.location-checkbox');
  checkboxes.forEach(cb => cb.checked = document.getElementById('selectAll').checked);
  render();
});
```
Key habits this reflects: validate with regex inside the handler, use `blur` (loses focus) for "check once you're done typing" fields vs `input`/`change` for live updates, mutate the DOM for cascading checkbox logic, always re-render after any state change, read `.value`/`.checked` (not `.textContent`/`.innerHTML`) for live form state.

### Arrays & strings (validation-relevant)
- `.map()`, `.filter()`, `.forEach()`, `.find()`, `.includes()`, `.join(separator)` (useful for building sentences from an array, e.g. a list of selected checkboxes).
- `.trim()`, `.length`, `.split()`, `.toLowerCase()`/`.toUpperCase()`.
- Regex: `/^[a-zA-Z]{3,50}$/.test(string)` — `^`/`$` anchor start/end, `[a-zA-Z]` character class, `{3,50}` repetition count.

---

# 35 Short-Answer Practice Questions
**(≈65% HTML/CSS — Q1–23, ≈35% JS — Q24–35)**

### HTML (Q1–8)

**Q1.** What is the difference between the `<head>` and `<body>` sections of an HTML document?
*A: `<head>` holds non-rendered metadata (title, meta tags, CSS/JS links); `<body>` holds everything that is actually rendered/visible on the page.*

**Q2.** Why is the `alt` attribute required on `<img>` tags?
*A: Accessibility (screen readers read it aloud) and it's shown if the image fails to load.*

**Q3.** What's the difference between `<div>` and `<span>`?
*A: `<div>` is block-level (takes full width, starts a new line); `<span>` is inline (only as wide as its content, stays in the text flow).*

**Q4.** In a `<label for="email">`, what must the `for` value match, and why does it matter?
*A: It must match the target input's `id`. It associates the label with the input so clicking the label focuses/activates the input — an accessibility requirement.*

**Q5.** What is the difference between `<button>` and `<input type="submit">`?
*A: Both can submit a form by default, but `<button>` can contain arbitrary HTML content (icons, nested tags); `<input type="submit">` can only display plain text via its `value` attribute.*

**Q6.** Why is `<img>` called a "void" or "self-closing" element?
*A: It has no closing tag and cannot contain child content — it's just `<img src="..." alt="...">` on its own.*

**Q7.** In Assignment 1, the chat bubble icon was supplied as a pre-made image asset rather than built with CSS. What HTML would correctly display it pinned to the page?
*A: An `<img src="assets/chat.png" alt="Chat">`, styled in CSS with `position: fixed` and offsets from the viewport edges (see CSS Q below).*

**Q8.** What does the `lang` attribute on `<html>` do, and why does it matter?
*A: It declares the page's language (e.g. `lang="en-AU"`) — used by screen readers for correct pronunciation and by search engines for SEO.*

### CSS — Box Model, Display, Positioning (Q9–16)

**Q9.** Explain the difference between `box-sizing: content-box` and `box-sizing: border-box`.
*A: `content-box` (default): declared width/height apply to content only, padding and border add on top, so the rendered box is bigger than declared. `border-box`: declared width/height include padding and border, so the rendered box matches exactly what you declared.*

**Q10.** In Assignment 1, a `.white-box` rendered larger than its declared 885×909px dimensions. What CSS property was missing, and why did that cause the bug?
*A: `box-sizing: border-box` was missing. Without it, the element used the default `content-box` model, so its internal padding was added on top of the declared width/height instead of being included within them.*

**Q11.** What's the difference between `display: none` and `visibility: hidden`?
*A: `display: none` removes the element from the layout entirely — no space is reserved. `visibility: hidden` hides the element visually but still reserves its space in the layout.*

**Q12.** Why does `width`/`height` have no visible effect on a plain inline element like `<span>`, but does work on `inline-block`?
*A: Inline elements size themselves to their content and don't accept box dimensions. `inline-block` keeps inline flow behaviour (sits in text, doesn't force a new line) but accepts block-style sizing (width/height/vertical margin).*

**Q13.** Explain the difference between `position: relative` and `position: absolute`.
*A: `relative` stays in normal document flow (its original space is still reserved) and offsets (`top`/`left` etc.) shift it from where it would otherwise sit. `absolute` is removed from flow entirely and positions relative to the nearest ancestor that is `relative`/`absolute`/`fixed` (or the page if none exists).*

**Q14.** In Assignment 1's Task 1, two Pokémon images were layered on top of each other with a slight rotation/offset. What CSS positioning technique achieves this?
*A: Wrap both images in a `position: relative` container, then give each image `position: absolute` with `top`/`left`/`transform: rotate(...)` offsets, and use `z-index` to control which sits on top.*

**Q15.** Why does `position: fixed` keep an element pinned to the same screen location even when the user scrolls, and where does Assignment 1 use this?
*A: `fixed` positions relative to the browser viewport (not the page or any ancestor), so it doesn't move with page scroll. Task 1's `.chat-bubble` uses `position: fixed; right: 23px; bottom: 20px;` to stay pinned to the bottom-right of the window.*

**Q16.** You set `z-index: 10` on an element but it has no effect on stacking order. What's the most likely cause?
*A: `z-index` only works on positioned elements (anything other than the default `position: static`) — the element is probably still `static`.*

### CSS — Flexbox & Grid (Q17–19)

**Q17.** What's the difference between `justify-content` and `align-items` in a flex container?
*A: `justify-content` controls spacing/alignment along the main axis (e.g. horizontal, in a `row` container). `align-items` controls alignment along the cross axis (e.g. vertical, in a `row` container).*

**Q18.** You apply `justify-content: center` to an element but nothing happens. What's the likely bug?
*A: The parent element is missing `display: flex` (or `display: grid`) — `justify-content` only has an effect inside a flex or grid container.*

**Q19.** Assignment 1's Task 2 rail-booking page uses a 3-column card grid. What CSS Grid property/value creates three equal-width columns?
*A: `grid-template-columns: repeat(3, 1fr);`*

### CSS — Images, Responsive, Mobile (Q20–23)

**Q20.** Why did `object-fit: cover` have no visible effect when applied to a `<div>` in Assignment 1, and what fixed it?
*A: `object-fit`/`object-position` only work on replaced elements (`<img>`, `<video>`, `<canvas>`) — not on `<div>`. The fix was moving those properties onto the `<img>` element itself.*

**Q21.** What's the difference between `object-fit: cover` and `object-fit: contain`?
*A: `cover` scales the image to completely fill its box, cropping whatever overflows, with no distortion. `contain` scales the image to fit entirely inside the box without cropping, which may leave empty space.*

**Q22.** Why does the slide deck describe `100vh` as "counter-intuitive" on mobile, and how is that relevant to Assignment 1's Task 3?
*A: On mobile, the browser's address bar can show/hide as the user scrolls, which changes the actual visible viewport height — so `100vh` doesn't reliably mean "fills the visible screen" and can cause jumpy or overflowing layout. Task 3 is required to be responsive between a 1663×2000 desktop layout and a 440×3767 mobile layout, so viewport-unit quirks like this directly affect how reliably the page fills the screen at each breakpoint.*

**Q23.** Write a media query that applies `flex-direction: column` only when the viewport is 768px wide or narrower.
*A:*
```css
@media screen and (max-width: 768px) {
  .container { flex-direction: column; }
}
```

### JavaScript — DOM & Events (Q24–31)

**Q24.** What's the difference between `document.querySelector()` and `document.querySelectorAll()`?
*A: `querySelector()` returns the first matching element (or `null`). `querySelectorAll()` returns a NodeList of all matching elements, which you iterate with `.forEach()`.*

**Q25.** Why is `element.style.backgroundColor` written in camelCase, when the CSS property is `background-color`?
*A: JavaScript property names can't contain hyphens (it would be parsed as subtraction), so CSS properties are exposed on `element.style` using camelCase equivalents.*

**Q26.** Name the three ways to attach an event handler to an element, and state which one is generally preferred and why.
*A: (1) Inline HTML attribute (`onclick="..."`), (2) DOM property (`element.onclick = fn`), (3) `addEventListener(type, fn)`. `addEventListener` is preferred because it allows multiple handlers on the same event/element and can be removed later with `removeEventListener`, unlike the DOM-property approach which only allows one handler at a time.*

**Q27.** What's wrong with this code?
```js
element.onclick = doSomething();
```
*A: The parentheses immediately **call** `doSomething`, assigning its return value (often `undefined`) to `onclick`, instead of assigning the function itself as the handler. It should be `element.onclick = doSomething;` (no parentheses) — or better, `element.addEventListener('click', doSomething);`.*

**Q28.** In an event handler, what does `event.target` refer to, and why is it useful for a FormFiddle-style dynamically-generated checkbox list?
*A: `event.target` is the actual element that triggered the event — useful when one handler is attached to a parent/group but you need to know exactly which child element (e.g. which specific checkbox) was interacted with.*

**Q29.** What does `event.preventDefault()` do, and give a concrete example of when you'd use it in a form.
*A: It stops the browser's default behaviour for that event. The classic example is calling it inside a form's `submit` handler to stop the page from reloading/navigating away, so you can run custom validation/JS instead.*

**Q30.** Why should you use `blur` rather than `input` for a field that should only be validated once the user finishes typing?
*A: `input` fires on every keystroke (too frequent — would show errors mid-typing); `blur` fires once when the element loses focus, i.e. once the user has finished with that field — matching Assignment 2's spec for fields like name/email/WAM.*

**Q31.** Describe, in order, the three phases an event goes through when it fires on a deeply nested element.
*A: (1) Capture phase — travels down from `window`/`document` through ancestors to the target. (2) Target phase — reaches the actual element. (3) Bubbling phase — travels back up through the same ancestors. `addEventListener` listens during the bubbling phase by default.*

### JavaScript — Forms & general (Q32–35)

**Q32.** How would you get a reference to the first `<form>` on a page, and then read the value of an input named `"email"` inside it?
*A:*
```js
const form = document.forms[0];
const email = form.elements.email.value; // or form.email.value
```

**Q33.** For a `<select>` element, what's the difference between `.value` and `.selectedIndex`?
*A: `.value` gives the `value` attribute of the currently selected `<option>`. `.selectedIndex` gives its numeric position (0-based) within the list of options.*

**Q34.** Why does JavaScript's "run-to-completion" event loop model mean a long-running click handler will block other events from being handled?
*A: JS is single-threaded with one event loop; it only picks the next event off the queue once the current handler has fully finished executing — it will never interrupt a running function partway through to handle a newly-fired event.*

**Q35.** Write a regex test that checks whether a string `name` is made up of 3–50 letters only (matching Assignment 2's name-validation style).
*A:*
```js
/^[a-zA-Z]{3,50}$/.test(name)
```

---

*Good luck Tuesday.*
