# The Unlived Life — Workflow, Process & Logic Documentation

*A record of how this interactive experience was built, and why — for your own reference, for briefing others, or for building the next one.*

---

## 1. Starting Point

The source material was a **Production Document** for a planned short film/visual essay — already containing:
- A three-act structure (Parable → Nachiketa → Awakening)
- A full shot list with visual descriptions and AI-image/video prompts
- A complete voiceover script
- Color palettes and cinematography references for each visual "world"

Rather than building the planned video (which needs AI image/video generation, editing software, and weeks of production), the same story and visual direction were adapted into a **single scrollable web page** — a much faster, cheaper, and equally immersive format for early sharing and review.

---

## 2. Core Design Decision: Scrollytelling

**Why scroll instead of video, slides, or a static article:**
- No player controls, no loading, no "duration" to commit to upfront — a person can begin without deciding to "watch a video"
- Scroll position becomes the timeline — the reader controls pace, which suits reflective/spiritual content better than autoplay
- Works identically on phone, laptop, or large monitor with no separate versions needed
- One link, no app, no login — lowest possible friction to share and view

**Technical approach:**
- Plain HTML + CSS + vanilla JavaScript — no frameworks, no build step, no dependencies. This keeps it: fast to load, easy to host anywhere (including GitHub Pages), and trivial for another developer to read/modify later.
- Visuals are built with CSS gradients, SVG, and CSS animation rather than image/video files — this was a deliberate substitute for the AI-generated imagery described in the production document, so the prototype could be built and shared immediately without a rendering pipeline. (These CSS/SVG visuals are placeholders in spirit — real AI-generated imagery from the production doc's prompts could be swapped in later for a higher-fidelity version.)

---

## 3. Structural Logic

The page is built as a sequence of full-height `<section>` blocks, one per beat of the story (mirroring the shot list in the production document almost 1:1). Each section:
- Sits in its own color world matching the act (walnut/gold for Act 1, indigo/starlight for Act 2, dawn gradient for Act 3)
- Contains one idea at a time — never more than 1-2 short lines of narration, so pacing matches the measured, contemplative tone specified in the original document

### The three visual worlds (from the production doc, carried through faithfully)
| Act | Palette | Mood |
|---|---|---|
| 1 — The Parable | Deep walnut, aged brass, warm amber | Intimate, sacred, Caravaggio-style chiaroscuro |
| 2 — Nachiketa | Indigo night, saffron, starlight | Mythic, cosmic, temple architecture |
| 3 — The Awakening | Dawn pinks/lavender to gold | Hope, expansion, arrival |

### Reveal-on-scroll
Every text/visual element starts invisible and fades/rises into place using the **Intersection Observer API** — a performant browser mechanism that detects when an element enters the viewport and triggers a CSS transition, rather than checking scroll position manually on every frame. This keeps the page smooth even on older phones.

### The sticky door (signature moment, Act 1 opening)
The opening door is built as a `position: sticky` element inside a tall (180vh) container. As the user scrolls through that container, JavaScript reads how far they've scrolled through it (0 to 1) and maps that directly to a CSS `rotateY()` transform on the door panel — so the door physically opens as they scroll, rather than being a fixed illustration. This was the first "the interface performs the metaphor" moment in the page.

---

## 4. The Key-Hold Ritual — Deepest Design Logic

This was the main addition when asked to make the experience more *immersive* rather than just more *decorated*. The reasoning:

**The problem identified:** the story is *about* the difference between guarding a door out of fear vs. picking up the key despite fear — but a reader could scroll through the entire thing passively, feeling nothing of that tension themselves. A beautiful scroll experience can still be entirely observational.

**The idea:** make the reader **enact** the story's core teaching for two seconds, instead of just reading about it.

**How it works, mechanically:**
1. A callback line is planted early (Act 1): *"The key lay at his feet the whole time. He never once picked it up."* — this seeds the metaphor so the later action means something.
2. In Act 3, the page **gates** all remaining content (`display:none` on a wrapper `div.gated`) behind a single interaction: a glowing key that must be **pressed and held** for ~2 seconds.
3. While holding, a progress ring fills (SVG `stroke-dashoffset` animated via `requestAnimationFrame`, so it's frame-accurate rather than relying on CSS transition timing alone) and dark "fear wisps" (blurred, animated shapes) swirl around the key.
4. **If the user lets go early**, nothing punishes them — but a whisper appears: *"Fear says: let go. This is silly."* — cycling through a few different lines on repeated attempts. This mirrors the real experience of resistance: fear doesn't stop you once, it repeats, in different costumes (a direct callback to the source message's own language).
5. **Only on a full, uninterrupted hold** does the key visually "turn," a warm flash washes the screen, and the gated sections are revealed. The copy immediately names what happened: *"The key turned. Not because fear left — because you held on anyway."* — reinforcing that the point was never the absence of fear, but holding on despite it.

**Accessibility considerations built in:**
- Works via mouse, touch, **and keyboard** (holding Space or Enter) — not gesture-only
- Respects `prefers-reduced-motion`: for those users, the hold duration drops to near-instant so no one is blocked by an intentionally uncomfortable interaction if they've asked their system to minimize motion
- `aria-label` on the key button describes the interaction in words for screen readers

---

## 5. The Personal Step — Second Layer of Participation

After the ritual unlocks the ending, near the closing landscape scene, the page asks one direct question:

> "What is one thing fear has told you not to do?"

with a plain text input. Whatever is typed is:
- **Never transmitted or stored anywhere** — it lives only in that browser tab's memory (a JS variable tied to the DOM), gone the moment the tab closes
- Immediately **echoed back** into the final message: *"Fear says, 'What if…?' / Faith says, 'Even if…' / Even then — [their own words]."*

**Why this matters:** the entire message is about applying an ancient teaching to *your* specific fear, not fear in the abstract. This closes that gap in the last ten seconds of the experience without requiring any backend, database, or privacy consideration — it's purely client-side, ephemeral, and safe by construction.

---

## 6. The Closing CTA — Converting the Experience into Action

The brief for this was clear: *the takeaway is the book.* So the final end card (which only appears after the key ritual is complete — i.e., after full engagement, not before) contains one button:

**"Read the Full Bhandara Message"** → links to the official Heartfulness registration/event page for the book.

Design choices:
- Styled as a solid gold pill button (not a plain text link) so it reads as a clear next action, not an afterthought
- Opens in a **new tab** (`target="_blank"` + `rel="noopener noreferrer"`) so clicking it doesn't lose the person's place in the interactive page
- Placed *after* the emotional payoff (the duet line, the personalized echo) rather than earlier — so the ask comes at the moment of highest resonance, not before the story has landed

---

## 7. Responsive & Cross-Device Logic

- All typography uses `clamp()` so font sizes scale fluidly between phone and large monitor without separate breakpoints for most elements
- Full-height sections use `svh`/`dvh` units (dynamic/small viewport height) instead of plain `vh`, specifically to avoid the common mobile bug where the browser's address bar causes "full screen" sections to jump or get cut off
- A dedicated breakpoint at 1900px+ scales up the door, SVGs, and body text so the page doesn't feel small and lost on a large desktop monitor
- Verified directly via automated screenshot testing at three sizes: 390×844 (phone), 1440×900 (laptop), 2560×1400 (large monitor)

---

## 8. What Was Deliberately Left Out (and why)

- **No sound/music**, despite the production doc's detailed music plan — autoplay audio is unreliable across browsers and often blocked; adding it would need an explicit "tap to enable sound" step, which was left out of this version to keep the entry frictionless. Worth revisiting for a polished v2.
- **No real AI-generated imagery yet** — current visuals are CSS/SVG so the prototype could be built and reviewed immediately. The production document's actual GPT-Image/Veo prompts are ready to swap in once there's appetite/budget for that pass.
- **No backend, no analytics, no data collection** — kept deliberately as a static file so it can be hosted anywhere (including free GitHub Pages) with zero ongoing cost or maintenance.

---

## Questions for You

If anything here needs adjusting before you brief someone else or hand this off, a few things worth deciding:

1. Do you want a "skip the ritual" escape hatch for people who bounce off the key-hold (mentioned earlier), or keep it as an uncompromising gate?
2. Should the personal-step question be optional to skip forward from (currently it just doesn't block anything if left blank — worth confirming that's the intended behavior)?
3. Do you want this doc kept updated as a living record if we build the *next* Bhandara message page, so patterns/decisions carry forward consistently?

Happy to expand any section above in more depth if you're sharing this doc with others who weren't part of the build process.
