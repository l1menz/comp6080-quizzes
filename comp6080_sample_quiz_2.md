Q. A card component has conflicting padding and sizing rules. (7 marks)

<div class="card featured" id="promo" style="padding: 10px;">
  <h3>Summer Sale</h3>
</div>

CSS:

.card {
  width: 250px;
  padding: 30px;
  box-sizing: content-box;
}

.featured {
  padding: 15px;
  box-sizing: border-box;
}

#promo {
  width: 300px;
}

div.card.featured {
  padding: 20px;
}

(a) [2 marks] Which padding value wins, and why does the inline style beat all of the stylesheet rules regardless of how specific they are?

Model answer:
The padding is 10px, from the inline style.
Inline styles are attached directly to the element in the style attribute, which
sits outside the normal selector-based cascade entirely — the browser always
applies it after (and instead of) any rule written in a stylesheet, no matter
how many classes, IDs or element names that rule combines. Specificity is only
ever compared between stylesheet rules; it never has to be weighed against an
inline style because inline always wins first.
1 mark for identifying the padding as 10px.
1 mark for explaining that inline styles are not part of the specificity
competition and simply override stylesheet rules unconditionally.

(b) [2 marks] Ignoring the inline style, which padding value would win among the four remaining rules, and what does this tell you about combining class/type selectors versus using an ID?

Model answer:
#promo doesn't set padding at all, so it's not in contention. Among the rest,
.featured (padding: 15px) and div.card.featured (padding: 20px) are both class-based,
but div.card.featured combines three selectors (one type + two classes) which is
more specific than .featured alone (one class), so div.card.featured (20px) wins
over .card and .featured. Still, none of them can beat an ID selector if one set
padding — ID specificity outranks any number of combined classes/type selectors.
1 mark for correctly identifying 20px as the winner among the stylesheet rules.
1 mark for explaining that stacking class/type selectors increases specificity,
but still cannot out-rank a single ID selector.

(c) [2 marks] Calculate the box's total rendered width in pixels, showing your working, given that box-sizing: border-box is what ultimately applies (the last rule to set box-sizing wins) and width: 300px from #promo applies.

Model answer:
300px.
Because border-box applies, the declared width of 300px already includes
padding and border — there is no border declared here, so:
  rendered width = 300px (content + padding all included, no extra added)
1 mark for stating 300px.
1 mark for explaining that border-box bakes padding/border into the declared width,
so nothing is added on top (contrast with content-box, where it would be
300 + 10 + 10 = 320px using the winning 10px inline padding).

(d) [1 mark] Why is relying on cascading rules like this, instead of giving the element one clear class, considered poor practice even when you can correctly work out the final result?

Model answer:
Even though the cascade is deterministic, a page with many overlapping
selectors targeting the same element becomes very hard for a human to reason
about quickly — every change risks fixing one rule while accidentally breaking
specificity elsewhere. Clear, single-purpose classes make the intended style
obvious without having to mentally run the cascade algorithm.
1 mark for any reasonable explanation along these lines.

---

Q. A gallery lays out thumbnails using Flexbox. (8 marks)

<div class="gallery">
  <img class="thumb" src="a.jpg" alt="Photo A">
  <img class="thumb" src="b.jpg" alt="Photo B">
  <img class="thumb" src="c.jpg" alt="Photo C">
</div>

CSS:

.gallery {
  display: flex;
  width: 600px;
  height: 150px;
}

.thumb {
  width: 150px;
  height: 150px;
}

(a) [2 marks] With three 150px-wide thumbnails inside a 600px container and no other rules, is there empty space in the gallery, and if so, where does it sit by default?

Model answer:
Yes — 3 × 150px = 450px of content against a 600px container leaves 150px of
empty space. By default (flex-direction: row, justify-content: flex-start),
the three images are pushed to the start (left) of the container, so the
150px gap sits at the right-hand end.
1 mark for correctly calculating the 150px of leftover space.
1 mark for stating it appears at the right (start-aligned by default).

(b) [2 marks] The developer adds justify-content: space-between;. Describe exactly how the three images are now arranged.

Model answer:
The first image sits flush against the gallery's left edge and the third sits
flush against its right edge, with the 150px of leftover space split evenly
into the two gaps between image 1–2 and image 2–3 (75px each) — no space is
placed before the first or after the last item.
1 mark for correctly describing the first/last items sitting flush at the edges.
1 mark for correctly describing the leftover space being split only between items.

(c) [2 marks] A fourth 150px-wide image is added, making the content 600px wide — exactly the width of the container. flex-wrap is not set. What happens, and what single line would allow the row to wrap onto a second line instead?

Model answer:
Nothing wraps — the default flex-wrap: nowrap forces all flex items to stay
on a single line even if it means they overflow or get squeezed, so with
exactly 600px of content in a 600px container they just barely fit on one row
(and if a fifth image were added, items would start overflowing rather than
dropping to a new line).
Add: flex-wrap: wrap;
1 mark for explaining that nowrap keeps everything on one line regardless of overflow.
1 mark for the correct property/value to enable wrapping.

(d) [2 marks] The developer wants each image vertically centred within the gallery if any image is shorter than the 150px row height (say, a 100px-tall thumbnail). Which Flexbox property should they add to .gallery, and what does its default value do instead?

Model answer:
Add align-items: center; to .gallery.
The default value, stretch, instead forces every flex item to stretch to
fill the full cross-axis size of the container (so a thumbnail without an
explicit height would stretch from 100px up to the full 150px row height,
potentially distorting it) rather than keeping its own height and centring
within the leftover space.
1 mark for correctly naming align-items: center.
1 mark for correctly explaining the default stretch behaviour.

---

Q. A tooltip needs to appear next to a help icon. (6 marks)

<span class="help-wrapper">
  <button class="help-icon">?</button>
  <span class="tooltip">Enter your 8-digit student number</span>
</span>

CSS:

.help-wrapper {
  display: inline-block;
}

.help-icon {
  width: 24px;
  height: 24px;
}

.tooltip {
  position: absolute;
  top: 30px;
  left: 0;
  background: black;
  color: white;
  padding: 6px;
}

(a) [2 marks] As written, where in the page does the tooltip actually appear, and why — given that .help-wrapper looks like it should be the obvious anchor?

Model answer:
It appears 30px from the top and flush with the left edge of the page (or
nearest positioned ancestor further up the DOM, if any exists) — NOT next to
the help icon. Even though .help-wrapper visually wraps the icon and tooltip,
it is never given position: relative (only display: inline-block), so it
does not become a positioning context. .tooltip's position: absolute
therefore skips straight past it and anchors to the next ancestor up the
tree that IS positioned, or the page itself if none is.
1 mark for stating it anchors to the page/viewport rather than the wrapper.
1 mark for explaining that .help-wrapper isn't a positioning ancestor because
it was never given a position value other than static.

(b) [2 marks] What is the minimal one-line fix to make the tooltip position itself relative to .help-wrapper instead?

Model answer:
Add position: relative; to .help-wrapper.
This makes it the nearest positioned ancestor for .tooltip, so top: 30px and
left: 0 are now measured from the wrapper's own top-left corner instead of
the page's.
1 mark for the correct fix (position: relative on .help-wrapper).
1 mark for explaining why it works (becomes the positioning anchor).

(c) [2 marks] After the fix in (b), the developer additionally wants the tooltip to only become visible on hover, and to not take up space in the layout when hidden. Name the one CSS property/value they should use for the hidden state (not display: none — explain why not, given an opacity-based fade transition is also planned later), and the selector that would show it again on hover.

Model answer:
Use visibility: hidden; as the default state for .tooltip, and
.help-wrapper:hover .tooltip { visibility: visible; } to reveal it again.
display: none is avoided here because it cannot be animated/transitioned
smoothly (an element cannot "partially" exist in the layout), which would
break the planned fade transition; visibility can be combined with opacity
and transitioned, while still not reserving visible space the way a plain
opacity: 0 alone would (the element would still be clickable/present even
while invisible).
1 mark for correctly suggesting visibility: hidden/visible rather than display: none, with a valid reason.
1 mark for the correct hover selector syntax targeting the descendant tooltip.

---

Q. A page caches the user's last search so it survives a refresh. (6 marks)

<input id="search" type="text" placeholder="Search courses">
<ul id="results"></ul>

JS:

const search = document.getElementById('search');

search.value = localStorage.getItem('lastSearch') || '';

search.addEventListener('input', () => {
  localStorage.setItem('lastSearch', search.value);
});

(a) [2 marks] The user types "COMP6080" into the box, then immediately refreshes the page without clicking anything else. What is shown in the search box after the refresh, and why does it work even though there's no submit button or click involved?

Model answer:
"COMP6080" is shown in the box after refresh. The 'input' event fires on
every keystroke (not just on submit/blur), so by the time the last character
is typed, localStorage.setItem has already run and saved "COMP6080". On
reload, the script runs search.value = localStorage.getItem('lastSearch') ||
'' before the user does anything, pulling the saved string back in.
1 mark for stating "COMP6080" is shown after refresh.
1 mark for explaining that 'input' fires per keystroke, so the value is
already saved before any refresh happens.

(b) [2 marks] What does the || '' at the end of the localStorage.getItem(...) line do, and what would search.value be set to on a brand new browser (never visited the site before) if it were removed?

Model answer:
localStorage.getItem returns null if the key doesn't exist yet. || '' falls
back to an empty string in that case, so search.value is always set to a
valid string. Without it, on a brand new browser search.value would be set
to null, which gets coerced to the literal text "null" being shown inside
the input box — a visible bug.
1 mark for correctly explaining the fallback/null-coalescing behaviour.
1 mark for correctly identifying the bug that would occur without it (literal "null" text).

(c) [2 marks] The developer wants to add a "Clear history" button that removes the saved search term (but doesn't need to clear the visible input box, since that's handled separately). Write the one line of JS that would go inside its click handler.

Model answer:
localStorage.removeItem('lastSearch');
1 mark for using removeItem (not setItem to an empty string, which is a
plausible but different approach — accept setItem('lastSearch', '') as a
valid alternative provided it's explained correctly).
1 mark for correct key name and syntax.

---

Q. A FAQ page expands and collapses answers. (7 marks)

<div class="faq-item">
  <button class="faq-question">What is COMP6080?</button>
  <p class="faq-answer">An intro to web front-end programming.</p>
</div>
<div class="faq-item">
  <button class="faq-question">Is there an exam?</button>
  <p class="faq-answer">No, but there is a quiz.</p>
</div>

JS:

const questions = document.querySelectorAll('.faq-question');

questions.onclick = (event) => {
  event.target.nextElementSibling.classList.toggle('open');
};

(a) [2 marks] Does this code run without errors? Explain what document.querySelectorAll() actually returns and why assigning directly to .onclick on that result does not do what the developer intended.

Model answer:
This throws an error / silently fails to work as intended — querySelectorAll
returns a NodeList (a collection of multiple elements), not a single element,
and a NodeList has no .onclick property of its own to assign a handler to
(assigning to it just sets an arbitrary unused property on the NodeList
object itself — it does not attach a listener to any of the buttons inside it).
1 mark for correctly explaining that querySelectorAll returns a NodeList/collection.
1 mark for explaining that .onclick cannot be assigned to a collection — it must be
attached to each individual element.

(b) [2 marks] Rewrite the JS so that clicking either question correctly toggles its own answer, using addEventListener and a loop/iteration method of your choice.

Model answer:
questions.forEach((question) => {
  question.addEventListener('click', (event) => {
    event.target.nextElementSibling.classList.toggle('open');
  });
});
1 mark for iterating over the NodeList (forEach, for...of, or a plain for loop
with an index are all acceptable).
1 mark for correctly calling addEventListener on each individual element inside
the loop.

(c) [2 marks] After fixing the bug, clicking "What is COMP6080?" toggles open the "An intro to..." answer correctly. Explain, using the HTML structure given, why event.target.nextElementSibling correctly refers to that specific `<p>` and not, say, the second FAQ item's question.

Model answer:
nextElementSibling returns the next element at the SAME level in the DOM tree
immediately after the current one — here, each <button class="faq-question">
is immediately followed, as a sibling within the same <div class="faq-item">,
by its own <p class="faq-answer">. Since event.target is the specific button
that was clicked, nextElementSibling always resolves to that same button's
own neighbouring answer paragraph, not any other FAQ item's elements, because
siblings are scoped to children of the same parent.
1 mark for correctly explaining what nextElementSibling returns (next sibling
at the same DOM level).
1 mark for correctly tying it to the specific HTML structure (each button/answer
pair shares a parent .faq-item div).

(d) [1 mark] Why is classList.toggle('open') generally preferable here to writing event.target.nextElementSibling.style.display = 'block' directly in JS?

Model answer:
Toggling a class keeps the actual visual styling (what "open" looks like —
e.g. display, max-height, animation) defined in CSS where it belongs, rather
than hardcoding specific style values in JS; it also makes it trivial to
reuse/adjust the "open" appearance everywhere without touching the JS, and
avoids JS and CSS fighting over the same style property.
1 mark for any reasonable explanation along these lines.

---

Q. A newsletter signup form validates an email before submitting. (8 marks)

<form id="newsletter">
  <input id="email" type="text" placeholder="you@example.com">
  <span id="error"></span>
  <button type="submit">Subscribe</button>
</form>

JS:

const form = document.getElementById('newsletter');
const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

form.addEventListener('submit', (event) => {
  const email = document.getElementById('email').value;

  if (!emailPattern.test(email)) {
    event.preventDefault();
    document.getElementById('error').textContent = 'Please enter a valid email';
  }
});

(a) [2 marks] The user types "sam@uni" (no dot, no domain suffix) and clicks Subscribe. Walk through what emailPattern.test(email) evaluates to, and what the user sees.

Model answer:
It evaluates to false. The pattern requires at least one non-whitespace/non-@
character, then @, then at least one non-whitespace/non-@ character, then a
literal dot, then at least one more such character — "sam@uni" has no dot
after the @ at all, so it cannot match. Since the test fails, !emailPattern.test(...)
is true, so event.preventDefault() runs (blocking the page from submitting/
reloading) and "Please enter a valid email" is displayed in the error span.
1 mark for correctly determining the regex fails to match and explaining why
(missing the dot/domain part).
1 mark for correctly describing the resulting behaviour (submission blocked,
error message shown).

(b) [2 marks] The user instead types "sam@gmail.com" and clicks Subscribe. What happens now, and why is there a subtle problem with the user experience even though the email is valid?

Model answer:
emailPattern.test(email) returns true, so !true is false, so the if block
is skipped entirely — event.preventDefault() is never called, and the form
submits normally (triggering the browser's default navigation/reload
behaviour). The subtle UX problem: because there's no handling for the
"valid" case other than letting the default submission proceed, the page
will reload/navigate away with no confirmation message or AJAX-style submit,
and any previously-shown error message would also only disappear because
the whole page reloads, not because it was explicitly cleared — a more
robust implementation would handle the success path explicitly too.
1 mark for correctly tracing through that the regex passes and the default
submit proceeds.
1 mark for identifying a legitimate UX concern (e.g. no explicit success
handling, reliance on full page reload, error text not being proactively
cleared on success).

(c) [2 marks] A classmate suggests changing the input's type from "text" to "email" and relying on the browser's built-in validation instead of the regex. Give one advantage and one disadvantage of this approach compared to the current JS-based validation.

Model answer:
Advantage: it requires no JavaScript at all — the browser automatically
blocks submission and shows a native validation message if the format is
wrong, which is simpler and works even if the page's JS fails to load.
Disadvantage: the developer has much less control over exactly what counts
as "valid" and cannot easily customise the error message's wording/styling
to match the rest of the page, and different browsers can have slightly
different built-in validation behaviour/messages.
1 mark for a valid advantage.
1 mark for a valid disadvantage.

(d) [2 marks] The developer wants the error message to automatically clear itself the moment the user starts fixing their email, rather than staying on screen until the next submit attempt. Which event should they listen for on the email input, and write the one line that would clear the error span's text inside that handler.

Model answer:
Listen for the 'input' event (fires on every keystroke) on the email field.
document.getElementById('email').addEventListener('input', () => {
  document.getElementById('error').textContent = '';
});
1 mark for correctly identifying 'input' as the right event (not 'change',
which only fires once focus leaves the field — too late for "as they type"
clearing).
1 mark for correct syntax clearing the error span's textContent.

---

Q. A recipe page needs to adapt between mobile and desktop. (6 marks)

<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
  <div class="recipe">
    <img class="recipe-photo" src="pasta.jpg" alt="Pasta dish">
    <div class="recipe-text">
      <h2>Garlic Pasta</h2>
      <p>Serves 2. Ready in 20 minutes.</p>
    </div>
  </div>
</body>

CSS:

.recipe {
  display: flex;
  flex-direction: row;
}

.recipe-photo {
  width: 50%;
}

.recipe-text {
  width: 50%;
}

@media (max-width: 500px) {
  .recipe {
    flex-direction: column;
  }

  .recipe-photo,
  .recipe-text {
    width: 100%;
  }
}

(a) [2 marks] On a desktop window 1200px wide, describe the layout: how are the photo and text arranged relative to each other, and how wide is each?

Model answer:
They sit side by side in a row (photo on the left, text on the right, per
source order), because flex-direction: row is the default/explicitly set
value and the media query (max-width: 500px) does not apply at 1200px. Each
takes up 50% of .recipe's width, so each is 600px wide.
1 mark for correctly describing the side-by-side row layout.
1 mark for correctly calculating 600px each (50% of 1200px).

(b) [2 marks] On a phone 375px wide, describe the layout now, and explain precisely which rule changes to cause it.

Model answer:
The photo stacks directly above the text (column layout), and both the
photo and text now each take up the full 375px width of the container. This
is because 375px is ≤ 500px, so the media query's rules apply and override
the earlier ones: flex-direction switches from row to column (stacking
instead of side-by-side), and both widths switch from 50% to 100%.
1 mark for correctly describing the stacked, full-width layout.
1 mark for correctly explaining that the media query overrides flex-direction
and width at this breakpoint.

(c) [2 marks] A student removes the <meta name="viewport"> tag entirely but keeps everything else. On the same 375px-wide phone, is the layout still correctly stacked? Explain what actually happens and why.

Model answer:
No — without the viewport meta tag, most mobile browsers render the page at
a wide virtual viewport (often around 980px) and then zoom out to fit the
screen, rather than using the phone's actual 375px width. Since the media
query checks the viewport width (which the browser now reports as ~980px,
not 375px), max-width: 500px is false, so the row layout (not the stacked
one) is used — and the whole page appears tiny and zoomed out rather than
fitting the screen naturally.
1 mark for explaining that the browser falls back to a wide virtual viewport
without the meta tag.
1 mark for correctly concluding the media query no longer applies / the mobile
layout breaks.

---

Q. A dev tools investigation into a broken layout. (5 marks)

(a) [2 marks] A student inspects a `<div>` in the Elements panel and the Computed tab shows: width 220px, padding 20px (all sides), border 0px, margin 16px (all sides). The student expected the div to be exactly 180px wide based on their CSS rule width: 180px;. Using the box model, explain the discrepancy.

Model answer:
220px = 180px (declared content width) + 20px + 20px (left/right padding) —
the box-sizing in effect must be content-box (the default), so the declared
width only ever applied to the content area, and the padding is adding on
top of it, exactly as shown in the Computed tab's box-model diagram. (Margin
is separate again — it affects spacing to neighbours, not the element's own
220px box size.)
1 mark for correctly reconstructing 180 + 20 + 20 = 220px.
1 mark for correctly identifying this as content-box behaviour (declared
width excludes padding).

(b) [2 marks] In the Styles pane, the student sees their width: 180px; rule with a strikethrough line through it, and a different rule, width: 220px; !important;, listed above it without a strikethrough. Explain what's happening and what the student would need to change to make their 180px rule actually apply.

Model answer:
The !important rule is winning regardless of specificity or source order —
!important rules take priority over normal rules, which is why the student's
normal width: 180px; is crossed out/overridden even though it might otherwise
have equal or higher specificity. To make 180px actually apply, the student
would need to either remove the !important from the other rule (if they
control it), add !important to their own rule (not recommended, as it just
escalates the problem for the next person), or address the root cause making
someone add !important there in the first place.
1 mark for correctly explaining !important overrides normal specificity/cascade rules.
1 mark for a reasonable fix/explanation that doesn't just say "add !important
back" without caveats.

(c) [1 mark] What is one reason the Computed tab is more useful than the Styles tab specifically for diagnosing "why is this element sized the way it is" type questions?

Model answer:
The Computed tab shows the single final resolved value for every property
after the entire cascade has already been resolved (including a box-model
diagram with actual pixel numbers), so the student doesn't have to manually
trace through every competing rule in the Styles tab themselves to work out
which one "won" — the browser has already done that calculation for them.
1 mark for any reasonable explanation along these lines.

---

Q. A single-page site has three sections and a sticky nav bar. (6 marks)

<nav class="navbar">Home | Projects | Contact</nav>
<section id="home">...</section>
<section id="projects">...</section>
<section id="contact">...</section>

CSS:

.navbar {
  position: sticky;
  top: 0;
  background: white;
}

section {
  height: 600px;
}

(a) [2 marks] Describe what the user sees as they scroll down the page from the very top, in terms of the navbar's position.

Model answer:
Initially (at the very top of the page) the navbar behaves just like a
normal, non-positioned element — it sits inline at the top of the document
in its natural place. As the user scrolls down and the navbar would
otherwise scroll out of view past the top of the viewport, it instead
"sticks" at top: 0 and stays pinned to the top of the screen for the rest
of the scroll, rather than disappearing.
1 mark for correctly describing the initial normal-flow behaviour before the
scroll threshold.
1 mark for correctly describing it sticking to the top of the viewport once
that threshold is crossed.

(b) [2 marks] What's the key difference between this behaviour and if the developer had used position: fixed; top: 0; instead?

Model answer:
With fixed, the navbar would be pinned to the top of the viewport from the
very start of the page (immediately removed from normal flow and anchored to
the viewport), rather than only "activating" once the user scrolls past its
natural position. A side effect: with fixed, something else in the normal
flow would need to compensate for the navbar no longer taking up space (e.g.
padding-top on the body) to stop content starting underneath it immediately;
sticky avoids this because it stays in normal flow until the scroll
threshold is reached.
1 mark for correctly explaining fixed is pinned to the viewport immediately,
unlike sticky's "later" activation.
1 mark for correctly explaining that fixed removes the element from flow
from the start (space not reserved), requiring a manual workaround.

(c) [2 marks] The developer wraps .navbar in a parent <header> that has overflow: hidden; set on it for an unrelated reason. After this change, position: sticky stops working and the navbar scrolls away with the page as normal. Explain why.

Model answer:
position: sticky requires the browser to track scrolling within whatever
containing block/scrollport is relevant, and overflow: hidden (or auto/
scroll) on an ancestor changes how scrolling is clipped/calculated for its
descendants — in practice, sticky positioning breaks (stops sticking)
if any ancestor between the sticky element and the viewport has
overflow set to anything other than visible, because that ancestor
effectively becomes its own scrolling context that the sticky calculation
can't "see past."
1 mark for identifying that an ancestor's overflow setting is the cause.
1 mark for a reasonable explanation of why overflow other than visible on an
ancestor breaks sticky's ability to track the viewport scroll correctly.

---

Q. Choosing the right HTML elements for a contact form. (4 marks)

A developer has built this using only <div>s and JavaScript click handlers:

<div class="label">Email</div>
<div class="input-box" contenteditable="true"></div>
<div class="btn" onclick="submitForm()">Send</div>

(a) [2 marks] Name two concrete problems with building a form this way instead of using semantic form elements (<form>, <label>, <input>, <button>), beyond simply "it's not best practice."

Model answer:
Any two of: (1) Accessibility — screen readers and other assistive tech
recognise real form controls and labels automatically (announcing "edit
text field," associating labels, etc.), but a <div> with contenteditable
gives none of that out of the box. (2) No built-in validation — a real
<input type="email" required> gets free browser-level validation and
appropriate mobile keyboards (e.g. an email keyboard with @ key); the div
version gets none of this and must have everything reimplemented in JS.
(3) No native form submission/keyboard behaviour — pressing Enter inside a
real <input> inside a <form> can trigger submission naturally, and a real
<button type="submit"> is also focusable/keyboard-activatable by default,
whereas a <div onclick> is not keyboard-accessible (can't be Tabbed to or
activated with Enter/Space) without extra ARIA/tabindex work.
1 mark for each distinct, correctly explained problem (max 2).

(b) [2 marks] Rewrite the three elements above using correct semantic HTML, keeping the same visual labels/text.

Model answer:
<form>
  <label for="email">Email</label>
  <input id="email" type="email">
  <button type="submit">Send</button>
</form>
1 mark for correct <label for="..."> correctly associated with a matching
input id.
1 mark for using a real <input> (ideally type="email") and <button
type="submit"> inside a <form>.
