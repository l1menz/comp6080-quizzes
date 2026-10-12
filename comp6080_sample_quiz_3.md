Q. A product grid uses CSS Grid to lay out six cards. (7 marks)

<div class="grid">
  <div class="product">A</div>
  <div class="product">B</div>
  <div class="product">C</div>
  <div class="product">D</div>
  <div class="product">E</div>
  <div class="product">F</div>
</div>

CSS:

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  width: 660px;
}

(a) [2 marks] How many rows does the grid end up with, and how wide is each column in pixels? Show your working for the column width.

Model answer:
Two rows (6 items ÷ 3 columns per row = 2 rows), created automatically even
though grid-template-rows was never set.
Column width: the 660px total width minus the two 20px gaps between the
three columns (2 × 20 = 40px) leaves 620px to split evenly across 3 equal
(1fr) columns: 620 ÷ 3 ≈ 206.67px each.
1 mark for correctly stating 2 rows and explaining rows are created automatically.
1 mark for correctly calculating column width accounting for the gaps (approx 206.67px, accept rounding).

(b) [2 marks] The developer changes grid-template-columns to repeat(4, 1fr) but doesn't add any more product divs. Describe what the grid looks like now with the same six items.

Model answer:
The grid now has 4 columns, so with 6 items: the first row fills with items
A, B, C, D (4 items), and the second row starts filling left-to-right again
with E and F, leaving the remaining two cells in that row empty (no items to
fill them) — items do not resize to stretch across the empty cells by default.
1 mark for correctly describing the first row holding 4 items.
1 mark for correctly describing the second row holding only 2 items with
empty cells, not stretched items.

(c) [2 marks] The developer wants item "A" (the first product) to span across the first two columns, making it twice as wide as the others, without changing grid-template-columns. Write the one CSS rule/property that achieves this, and explain what it does.

Model answer:
.product:first-child {
  grid-column: span 2;
}
grid-column: span 2; tells that specific grid item to occupy two column
tracks instead of the usual one, making it twice as wide as a normal 1fr
cell (while the grid automatically reflows the remaining items around it).
1 mark for the correct property/value (grid-column: span 2, or an equivalent
explicit grid-column: 1 / 3).
1 mark for correctly explaining what spanning multiple column tracks does.

(d) [1 mark] Is this layout one-dimensional or two-dimensional, and what single word distinguishes Grid from Flexbox in this respect?

Model answer:
Two-dimensional — Grid lays items out across both rows and columns
simultaneously, which is the key distinction from Flexbox (one-dimensional:
a single row or column at a time).
1 mark for correctly identifying "two-dimensional" and naming the row/column
distinction.

---

Q. A list of to-do items can each be marked complete by clicking anywhere on them. (8 marks)

<ul id="todo-list">
  <li class="todo">Buy milk</li>
  <li class="todo">Finish assignment</li>
  <li class="todo">Walk the dog</li>
</ul>

JS:

document.getElementById('todo-list').addEventListener('click', (event) => {
  event.target.classList.toggle('done');
});

(a) [2 marks] Only one event listener is attached, to the <ul> itself, yet clicking on any individual <li> correctly toggles that specific item. Explain the mechanism that makes this work, by name.

Model answer:
This relies on event bubbling (sometimes combined with the technique name
"event delegation"): when a user clicks an <li>, the click event fires on
that <li> first, then bubbles upward through its ancestors, including the
parent <ul> — since the listener is attached to the <ul> and listens during
the (default) bubbling phase, it catches the click once it bubbles up to it.
event.target inside the handler still correctly refers to the actual <li>
that was originally clicked, not the <ul> itself, which is why only that one
item gets toggled.
1 mark for correctly naming/explaining event bubbling.
1 mark for correctly explaining that event.target still identifies the
originally-clicked element even though the listener lives higher up.

(b) [2 marks] A developer adds a "Clear all" button inside the same <ul>, as a fourth <li> with no class="todo":
<li id="clear-btn">Clear all</li>
Clicking it currently also toggles a (nonexistent, harmless) 'done' class
onto itself due to the existing handler. Explain why this happens, and
suggest a one-line fix inside the existing handler (not a new listener) that
would stop "Clear all" from being affected, while leaving the todo items
working as before.

Model answer:
It happens because the listener is attached to the whole <ul> and reacts to
ANY click that bubbles up through it, regardless of which child was clicked
— there's nothing in the current code that distinguishes a genuine .todo
item from the "Clear all" item.
Fix: check the clicked element's class before toggling, e.g.:
document.getElementById('todo-list').addEventListener('click', (event) => {
  if (event.target.classList.contains('todo')) {
    event.target.classList.toggle('done');
  }
});
1 mark for correctly explaining that the handler currently reacts to all
clicks indiscriminately.
1 mark for a correct guard condition restricting the toggle to .todo elements
specifically.

(c) [2 marks] With the fix from (b) in place, what would happen if the developer instead used event.currentTarget.classList.contains('todo') for the check? Explain why this is a meaningful bug, not just a stylistic difference.

Model answer:
event.currentTarget always refers to the element the LISTENER is attached to
— in this case always the <ul> itself, regardless of which child was
actually clicked. Since the <ul> never has the class "todo" on it,
event.currentTarget.classList.contains('todo') would always evaluate to
false, meaning the if block would never run and NO todo item (nor the
clear button) would ever get toggled again — a real functional bug, not
just a style choice.
1 mark for correctly explaining what currentTarget refers to (always the ul,
not the clicked child).
1 mark for correctly concluding this breaks all toggling, not just the clear
button case.

(d) [2 marks] Instead of using classList.toggle, a developer proposes directly setting event.target.style.textDecoration = 'line-through'; to show completion. Give one reason the classList/CSS-class approach (with a .done { text-decoration: line-through; } rule) is better software design.

Model answer:
Using a class keeps the "what completion looks like" decision in CSS, where
visual styling belongs, rather than hardcoding a specific style property
directly in the JS logic. This makes the completed-item style reusable and
easy to change everywhere at once (e.g. also adding opacity or a colour
change later) by editing one CSS rule, and avoids JS and CSS both trying to
control the same inline style property, which can cause conflicts or make
debugging harder. It's also trivial to reverse with toggle, whereas manually
unsetting a specific inline style value back to nothing is comparatively
fiddly.
1 mark for any reasonable, correctly-explained point along these lines.
1 mark for depth/clarity of the explanation (e.g. correctly contrasting
"behaviour in JS" vs "appearance in CSS").

---

Q. A multi-field registration form reads several related fields. (7 marks)

<form id="register">
  <input type="text" name="username">
  <input type="radio" name="plan" value="free" checked>
  <input type="radio" name="plan" value="pro">
  <select name="country">
    <option value="au">Australia</option>
    <option value="nz" selected>New Zealand</option>
  </select>
  <button type="submit">Register</button>
</form>

(a) [2 marks] Write the JS to get a reference to this form using document.forms, and then read the current value typed into the username field, using the shorthand form access (not getElementById).

Model answer:
const form = document.forms['register'];
const username = form.username.value;
(document.forms.register is also an acceptable equivalent to the bracket
version.)
1 mark for correctly accessing the form via document.forms (named or
bracket notation).
1 mark for correctly reading .value off the username field via shorthand
form.username.

(b) [3 marks] There are two radio inputs sharing name="plan". Explain why they share the same name, what form.elements.plan would return given there are multiple same-named elements, and write the JS needed to find out which one (if any) is currently checked and log its value.

Model answer:
Sharing the same name groups the two radios so that the browser treats them
as a single mutually-exclusive set — selecting one automatically deselects
the other. Because there's more than one element sharing that name,
form.elements.plan returns a collection (RadioNodeList), not a single
element.
To find the checked one:
const planInputs = form.elements.plan;
for (const input of planInputs) {
  if (input.checked) {
    console.log(input.value);
  }
}
(Alternative valid approach: document.querySelector('input[name="plan"]:checked').value)
1 mark for correctly explaining why shared name groups radios into a mutually
exclusive set.
1 mark for correctly stating a collection (not a single element) is returned.
1 mark for working code that finds and logs the checked radio's value.

(c) [2 marks] What will select.value and select.selectedIndex each return for the country dropdown as given (before any user interaction), and explain the relationship between the two.

Model answer:
select.value returns "nz" (the value attribute of the selected option),
select.selectedIndex returns 1 (the New Zealand option is the second
<option>, at index 1, since indices are zero-based and Australia is at
index 0). selectedIndex identifies the position of the selected option
within the list, while value identifies the actual value string carried by
whichever option sits at that position — selectedIndex changing always
changes which value is reported, and vice versa, since they always refer to
the same currently-selected option.
1 mark for correctly stating value = "nz" and selectedIndex = 1.
1 mark for correctly explaining the relationship (index locates the option;
value reads its value attribute).

---

Q. A profile picture needs to look sharp across different screen densities and crop ratios. (8 marks)

The developer currently has:
<img class="avatar" src="profile.jpg" alt="User profile photo">

CSS:

.avatar {
  width: 120px;
  height: 120px;
  border-radius: 50%;
}

(a) [2 marks] The uploaded profile.jpg is a rectangular 800×600px photo, not a square. Explain what the browser does to make it fit the 120×120px circular avatar box as currently styled, and name the CSS property that would need to be added if it currently isn't achieving a sensible crop.

Model answer:
As written, there is no object-fit property set, so the image's default
behaviour applies: the browser stretches/squashes the rectangular image to
fully fill the 120×120px box, distorting the photo's proportions (people
would look squeezed). Adding object-fit: cover; would instead scale the
image to completely fill the square box while cropping the overflow and
preserving its original aspect ratio, avoiding the distortion.
1 mark for correctly explaining the default distortion/squashing behaviour
without object-fit.
1 mark for correctly naming object-fit (and ideally cover) as the fix.

(b) [2 marks] border-radius: 50%; is applied to .avatar. Explain exactly why 50% specifically produces a perfect circle here, and what shape would result if width were changed to 200px while height stayed at 120px.

Model answer:
border-radius: 50% rounds each corner by half of the element's own
width/height — on a square box (equal width and height), rounding each
corner by half the side length makes the corners meet exactly in the
middle on each axis, producing a perfect circle. If width became 200px
while height stayed 120px, the box would no longer be square (200×120),
so 50% rounding would instead produce a "pill"/stadium shape (fully rounded
short ends, flatter long sides) rather than a circle, because the
horizontal and vertical radii are no longer equal.
1 mark for correctly explaining why equal width/height + 50% produces a
perfect circle.
1 mark for correctly identifying the pill/stadium shape result on a
non-square box.

(c) [2 marks] A classmate suggests just using a smaller source image (say, 120×120px) directly so no scaling is needed at all. Explain one real drawback of this approach for users on high-density ("Retina"-style) phone screens, given what the slides describe about CSS pixels versus physical device pixels.

Model answer:
A high-density screen packs multiple physical device pixels into each
single CSS pixel (e.g. 2x or 3x density), so a 120×120 CSS-pixel box is
actually rendered using far more physical pixels on such a screen. An image
file that is only exactly 120×120 source pixels would need to be stretched
to cover those extra physical pixels, making it look soft/blurry on
high-density screens — exactly the opposite of sharp. A properly sized
source image (e.g. 240×240 or larger) scaled down via CSS/object-fit avoids
this.
1 mark for correctly invoking the CSS pixel vs physical pixel distinction
from the slides.
1 mark for correctly concluding the image would look blurry/soft on
high-density screens as a result.

(d) [2 marks] Beyond resizing, the developer is deciding between JPG, PNG, and SVG for this profile photo. Which is most appropriate and why, referencing the nature of a real photographic image (not a logo or icon)?

Model answer:
JPG — a real photograph has continuous tones, fine detail and many subtly
varying colours, which is exactly what JPG's lossy compression is designed
to shrink efficiently while remaining visually close to the original (the
human eye doesn't notice most of what's discarded). PNG would be lossless
and therefore unnecessarily large for this kind of content with no real
visual benefit, and SVG (a vector format built from mathematical shapes) is
not suited to photographic, non-geometric imagery at all — it cannot
meaningfully represent a photo's organic detail the way a raster format can.
1 mark for correctly choosing JPG.
1 mark for a correct, specific justification referencing lossy compression
suiting photographic/continuous-tone content.

---

Q. A pricing page highlights whichever plan the user last hovered, and disables an unavailable plan. (6 marks)

<div class="plan" id="basic">Basic — $5/mo</div>
<div class="plan" id="pro">Pro — $15/mo</div>
<div class="plan disabled" id="enterprise">Enterprise — Contact us</div>

CSS:

.plan {
  padding: 16px;
  border: 1px solid #ccc;
}

.plan:hover {
  border-color: blue;
}

.plan.disabled {
  opacity: 0.5;
  pointer-events: none;
}

#enterprise:hover {
  border-color: blue;
}

(a) [2 marks] The developer notices that hovering over the "Enterprise" box never actually shows the blue border, despite there being a rule #enterprise:hover specifically targeting it, which should win on specificity over .plan:hover. Explain the real reason the hover effect never triggers, given this has nothing to do with the cascade or specificity.

Model answer:
pointer-events: none on .plan.disabled removes the element from receiving
any mouse interaction entirely — the browser effectively treats it as if the
mouse passes straight through it, so :hover can never be triggered on it at
all (clicks would also be ignored). This is independent of CSS specificity:
even a perfectly-targeted, maximally-specific :hover rule cannot fire if the
element itself has been removed from mouse interaction.
1 mark for correctly identifying pointer-events: none as the real cause.
1 mark for correctly explaining that it blocks all mouse interaction/hover
detection, regardless of selector specificity.

(b) [2 marks] Between .plan:hover and #enterprise:hover (ignoring the pointer-events issue for a moment, e.g. imagining the disabled class was removed), which would win if both applied to the Enterprise box, and why?

Model answer:
#enterprise:hover would win. An ID selector combined with a pseudo-class
is more specific than a class selector combined with the same pseudo-class
— ID selectors always outrank class selectors in the specificity hierarchy,
regardless of the matching pseudo-class being identical in both rules.
1 mark for correctly identifying #enterprise:hover as the winner.
1 mark for correctly explaining that ID specificity outranks class
specificity even when combined with the same pseudo-class.

(c) [2 marks] The developer wants hovering on Basic or Pro (but never Enterprise, which stays disabled) to also slightly enlarge the box using a smooth animation rather than an instant jump. Name the two CSS properties/concepts needed together to achieve a smooth, animated size change on hover (not just an instant one), and briefly explain the role of each.

Model answer:
transform: scale(1.05); (or similar) to actually define the enlarged size on
hover, combined with transition: transform 0.2s; (or similar) on the base
.plan rule to make the change animate smoothly between its normal size and
the hovered size over time, rather than snapping instantly between the two.
Without the transition property, the scale change would still work but
would happen instantaneously with no animation.
1 mark for correctly naming transform (e.g. scale) as the property that
defines the new hovered size.
1 mark for correctly naming transition as the property that makes the
change animate smoothly rather than jumping instantly.

---

Q. A character counter below a bio textarea. (7 marks)

<textarea id="bio" maxlength="150"></textarea>
<p id="counter">0 / 150</p>

JS:

const bio = document.getElementById('bio');
const counter = document.getElementById('counter');

bio.addEventListener('input', () => {
  counter.textContent = bio.value.length + ' / 150';
});

(a) [2 marks] The user types "Hi there!" (9 characters, including the space and exclamation mark). What does the counter show immediately after the final character is typed, and why does it update live rather than only after the user clicks away?

Model answer:
The counter shows "9 / 150". It updates live because the 'input' event
fires on every single change to the textarea's content as the user types —
each keystroke triggers the handler again, recalculating bio.value.length
(the current number of characters) and rewriting the counter's text
immediately, rather than waiting for a 'blur' or 'change' event which would
only fire once the field loses focus.
1 mark for correctly counting 9 characters.
1 mark for correctly explaining why 'input' causes live, per-keystroke updates.

(b) [2 marks] The maxlength="150" attribute is present on the textarea. Explain what this HTML attribute does on its own, with no JavaScript at all, and why the counter JS is still useful even though the browser already enforces a 150-character limit.

Model answer:
maxlength="150" is a native HTML constraint that stops the browser from
accepting any more than 150 characters of input into that field in the
first place — the user simply cannot type a 151st character, with no
JavaScript required. The counter JS is still useful purely as visual
feedback: without it, the user has no way of knowing how close they are to
the limit as they type (e.g. "I'm at 140/150, I should wrap up") — the
HTML attribute enforces the limit but doesn't display it.
1 mark for correctly explaining maxlength's native enforcement behaviour.
1 mark for correctly explaining the counter's complementary UX role (visible
feedback, not enforcement).

(c) [3 marks] The developer wants the counter text to turn red once the user has used 90% or more of the limit (135+ characters), reverting to the normal colour otherwise. Rewrite the event handler to add this behaviour, using classList rather than directly setting style.color.

Model answer:
bio.addEventListener('input', () => {
  const length = bio.value.length;
  counter.textContent = length + ' / 150';
  if (length >= 135) {
    counter.classList.add('warning');
  } else {
    counter.classList.remove('warning');
  }
});
(paired with a CSS rule such as .warning { color: red; } elsewhere in the stylesheet)
1 mark for correctly calculating/using the 135-character threshold (90% of 150).
1 mark for correctly adding the class when at/above the threshold.
1 mark for correctly removing the class when below the threshold (so it
reverts, not just a one-way switch).

---

Q. Reading layout and specificity from a browser's dev tools panel. (5 marks)

A student inspects a paragraph and sees the following in the Styles pane,
listed top to bottom exactly as the browser shows them (later/higher in the
list means applied later in the cascade order shown by the tool):

element.style { } (empty — no inline styles)
.intro (specificity: 0,1,0) { font-size: 14px; }
p.intro (specificity: 0,1,1) { font-size: 18px; }
#main .intro (specificity: 1,1,0) { font-size: 22px; }

(a) [2 marks] What font-size is actually applied to the paragraph, and why — referencing the specificity values shown?

Model answer:
22px. Specificity is compared by column: the id-selectors-involved column
comes first — #main .intro has a 1 in that column (one ID is involved
somewhere in the selector), while the other two rules both have a 0 there.
Since no other rule has any ID involved, #main .intro automatically outranks
them regardless of its class-or-element counts, so its 22px applies.
1 mark for correctly stating 22px is applied.
1 mark for correctly explaining the ID-count column decides it, beating the
other rules regardless of their own class/element counts.

(b) [2 marks] Explain, using the given specificity values, why p.intro (0,1,1) beats .intro (0,1,0) specifically, even though both have exactly one class in common.

Model answer:
After the ID column (both are 0, tied), the next column compared is the
class/attribute/pseudo-class count — both are 1 here too (tied again). The
decider is the final column, element/type selector count: p.intro includes
the type selector p (count of 1), while .intro has no type selector at all
(count of 0). Since every earlier column was tied, this final column breaks
the tie in favour of p.intro.
1 mark for correctly identifying that the first two specificity columns are tied.
1 mark for correctly identifying the type-selector column as the actual
tiebreaker.

(c) [1 mark] If the developer wanted the paragraph to be 14px specifically — overriding all three stylesheet rules — without editing any of the three existing rules and without adding !important anywhere, what's one valid approach?

Model answer:
Add an inline style directly to the element, e.g. style="font-size: 14px;"
in the HTML — inline styles are applied outside the normal specificity
cascade entirely and win over any stylesheet rule (short of !important), so
this achieves the override without touching the existing rules or using
!important.
1 mark for correctly suggesting an inline style as the valid approach.
