---
name: client-website-build
description: Reusable behavioral checklist for building client websites with Claude Code. Covers build sequence, hard-won failure modes, asset intake rules, revision workflow, verification pitfalls, CSS/JS gotchas, git hygiene, and post-launch patterns. Paste-and-use on any client build. Update when a new mistake or confirmed-good pattern emerges.
version: 1.0
author: michaelbazzi
---
# SKILL.md: Client Website Build Behavior

A reusable behavioral checklist for building and maintaining client websites
with Claude Code. Contains no project-specific colors, fonts, layouts, or
content. Paste and use on any client build. Update it when a new mistake or
a new confirmed-good pattern shows up on a future build.

Covers two phases: the **initial build** (first section) and **ongoing
revisions after a site is live** (second section): the second phase is
where most builds spend most of their lifetime, and where most tokens/time
get burned if the workflow isn't disciplined.

---

## PART 1: Initial build

### Build sequence: follow this order on every build

Most of the failure modes in this document happen because a step got done
out of order, not because the individual step was done badly. Build in this
order and most of them never occur in the first place:

0. **Repo & hosting setup first.** Create the GitHub repo **private by
   default** (flip to public only if there's a specific reason). Confirm you
   are the only collaborator. Connect the hosting platform's continuous
   deployment (e.g. Netlify) to the repo before writing any code, so every
   later push has a working deploy path already verified. **Also verify the
   domain's email DNS at this same stage**: before any contact email
   address gets published on the site or built into any automation, run a
   live DNS lookup (`dig MX yourdomain.com`, `dig TXT yourdomain.com`) and
   confirm MX, SPF, and (if using Google Workspace) DKIM records actually
   resolve. Don't take "it's a paid Google Workspace account" as proof;
   verify the DNS directly. See lesson 12 below for why this matters enough
   to check this early.
1. **Assets first.** Convert every photo to JPG and every video to MP4,
   verify orientation, file size, and quality on each one: before a single
   line of HTML references them. Never wire an unconverted or unverified
   asset into the build "to come back to later."
2. **Structure second.** Semantic HTML, section order, navigation, page
   architecture (single- vs. multi-page). Get the skeleton confirmed before
   spending time on how it looks.
3. **Styling third.** Colors, type, spacing, layout polish: applied to the
   already-correct structure with already-verified assets.
4. **Interactions last.** Hover states, animations, micro-interactions.
   These are the easiest thing to get wrong in isolation (see lesson 7
   below), so they come after the static page is already right.
5. **Responsive check before calling anything done.** Not a final glance:
   actually check every breakpoint (see Definition of Done in the master
   prompt) before saying a build is finished.

Working out of this order is how half the lessons below happen: styling
gets built around an unconverted photo, an interaction gets tuned before the
layout settled, or a "final" responsive pass reveals structural problems
that are now expensive to fix.

### Hard-won lessons: check for these proactively

**1. Media readiness: format discipline is not optional.**
All photos supplied for a build must be JPG: not HEIC, not PNG used as a
photograph, not any camera-native format. All video must be MP4: not MOV,
AVI, or any camera-native container. Convert and verify *before* the file
ever lands in the working directory, let alone gets referenced in code.
Never build a section around a photo or video that hasn't been converted
yet "to swap in later": that's how unconverted media ends up shipped.

**2. Baked-in text/logos in supplied photos.**
Before overlaying any name, title, or logo on a client-supplied photo, open
and inspect it for existing baked-in text or watermarks. Don't assume a
"portrait" or "photo" asset is a blank canvas.

**3. Aspect-ratio mismatches crop baked-in content.**
`object-fit: cover` crops the edges of an image when the container's aspect
ratio doesn't match the image's native ratio (a square image in a taller box
gets left/right cropped equally; a square image in a wider box gets
top/bottom cropped). If the image is a fixed graphic design rather than a
generic crop-safe photo, either match the container ratio to the image's
native ratio, or use `object-fit: contain`. Check what's near the edges
before cropping anything.

**4. Image format conversion can silently rotate output wrong.**
Auto-rotation on conversion (e.g. `sips`) only works if the source file has
an EXIF orientation tag. If it doesn't, the output can come out sideways
with no error or warning. After any batch format conversion, visually verify
orientation on every output. Not a sample of a few. Every file.

**5. Batch image identification breaks down past a handful of images.**
When identifying or naming more than ~4 images in one pass, verify each one
individually (one read, one written description) before building a final
filename/content mapping. A single batched judgment across many simultaneous
image results can silently mismatch: the failure is confident, not obvious.

**6. Chronological ordering needs real capture metadata, not file dates.**
Filesystem modification/creation timestamps are unreliable for "in the order
it happened" content: re-exporting, AirDropping, or re-encoding a file
resets them. Pull actual capture-date metadata per file. Re-encoding video
strips this metadata unless explicitly preserved: check the *original*
file's metadata, never the converted output's.

**7. Responsive grids: an unconstrained `1fr` max stretches lone items.**
`grid-template-columns: repeat(auto-fit, minmax(Xpx, 1fr))` lets a single
item in a row balloon to fill the entire row while multi-item rows look
normal: producing visually inconsistent tile sizes across a page with
variable group sizes. If tile size must stay visually consistent regardless
of item count, cap both ends of `minmax()` to fixed pixel values and use
`auto-fill` instead of `auto-fit`.

**8. A micro-interaction shown in isolation reads stronger than in context.**
The same numeric animation values (scale amount, duration, easing) feel far
more subtle once placed in a real page next to real content than they did in
an isolated demo. If a client says an animation "isn't landing" or "isn't
the same as the example," don't just scale up the same technique: check
whether the underlying mechanism (a single eased transition vs. a real
multi-keyframe animation) is the actual gap.

**9. Don't fabricate content from assets that aren't what's needed.**
If only placeholder, brand-kit, or unrelated assets exist (no real product/
work photos yet), say so plainly and frame that section honestly instead of
dressing up unrelated images as real portfolio content. Swap in the honest
framing for the real one once real content arrives.

**10. Guessing on structure or ambiguity instead of asking.**
Page architecture (single- vs. multi-page), any style choice not explicitly
specified, and any instruction with more than one reasonable reading:
propose the interpretation and wait for confirmation, don't ship a guess.
These are expensive to undo once built.

**11. A rejected choice stays rejected.**
If a style or approach is tried and then explicitly reverted, it's off the
table for the rest of that project. Don't quietly re-suggest it later.

**12. A domain's contact email being "set up" doesn't mean the DNS is
actually configured. Verify with a live lookup, not the platform's own
status badge.** A client's contact email (e.g. `hello@theirdomain.com`) can
have a real, paid mailbox (Google Workspace, etc.) while the domain itself
has zero MX, SPF, or DKIM records. This happens most often when a domain's
nameservers get pointed at a new DNS host (Netlify, etc.) and the old mail
records never get recreated, since DNS migrations don't carry old records
over automatically. The failure mode is deceptive: outbound mail can still
report "sent successfully" with a real message ID (the mailbox provider's
own servers relay it regardless of the sending domain's DNS), while the
email silently never arrives, and the admin dashboard can show a
reassuring status like "Gmail activated" that only reflects the mailbox
service being licensed, not the DNS actually being live. Don't trust any
of that. The only real proof is: run `dig MX yourdomain.com` and
`dig TXT yourdomain.com` yourself, and send an actual test email round
trip (to and from the address) before treating any contact email as
functional, whether that's the first time it's published on a site, or
before building any automation (auto-responders, notifications) on top of
it. This should be checked at initial launch, not discovered later while
debugging an unrelated-seeming automation bug.

### Process patterns that work every time

- Pull color, type, and brand specifics from whatever source-of-truth the
  client provides (brand doc, style guide, existing assets). Never invent
  or approximate values.
- Convert all non-web media formats (e.g. HEIC, MOV) before wiring them into
  the build, and verify orientation, file size, and quality on the output.
- Use lightweight/built-in tools first: OS-native conversion tools for basic
  image work, a dedicated video encoder for video, and a dedicated format
  encoder if the primary tool's default build lacks support for a needed
  format (e.g. transparency-capable formats).
- If a reference site/screenshot is given, extract its specific signature
  move (the exact layout pattern, spacing rhythm, or one distinctive
  element). Not just "the vibe". Adapt that move to the new brand's
  own palette and content. Copy the mechanism, not the content.
- Before calling any build finished, run a vibe-code check: no purple/
  violet as a primary color, no gradients anywhere, no gradient-filled
  headline text, no fake/meaningless stat blocks, no emoji in headings, no
  generic "Why Choose Us" section, no glassmorphism, a real display+body
  font pairing (not default system font), and verified responsive behavior
  at minimum 375px / 768px / 1024px / 1440px widths.
- Every client site built for a local service business needs these six SEO
  items before launch: location in title tag, location in meta description,
  LocalBusiness schema in the head, location in footer copy, sitemap.xml,
  and robots.txt. Never launch a local business site without these. They
  take 20 minutes and are the difference between invisible and findable on
  local search.
- Never include a physical street address in schema or copy for a mobile
  service business. Use city and state for addressLocality and
  addressRegion and set areaServed to the service region.
- Google Search Console must be set up the day the site goes live. It
  cannot be automated. Flag it in every client handoff checklist.

---

## PART 2: Revision workflow (once a site is live)

This is the phase every build spends most of its life in. A client-approved
site gets dozens of small follow-up requests over time: copy tweaks, new
sections, style adjustments, small feature additions. The discipline here is
what determines whether that goes fast and safely, or slow and riskily.

### The core rule: never edit live files directly

Unless a client explicitly says otherwise, **assume every change should be
staged in preview files first and only promoted to the live files on
explicit approval.** This is not extra ceremony: it's what makes it safe to
experiment, iterate on client feedback, and never accidentally ship a
half-approved change. State this working agreement up front on any
revision-phase project if it isn't already established.

### Step-by-step: the preview → live cycle

1. **Create preview copies of every live file you'll touch**, using a
   `-preview` suffix on the same name: `index.html` → `index-preview.html`,
   `style.css` → `style-preview.css`, `script.js` → `script-preview.js`. Do
   this fresh at the start of *each* round of changes: don't reuse stale
   preview files left over from a prior round that was already pushed live.
2. **Repoint the preview HTML's asset references** to the preview CSS/JS:
   `<link href="style.css">` → `<link href="style-preview.css">`, same for
   the `<script src>` tag. This is what lets you view the changes live
   without touching the real files at all.
3. **Make every requested edit only in the preview files.**
4. **Verify in a real rendered preview, not the raw file.** Start a local
   static server (e.g. `python3 -m http.server` via the project's preview
   tooling) and load `http://localhost:PORT/index-preview.html`: never open
   the HTML file directly via `file://`, which breaks relative CSS/JS paths
   and can make a perfectly fine build look completely broken. If a user
   reports "the whole layout changed" or something looks wrong that you
   can't reproduce, ask whether they opened the raw file or the served URL
   before assuming there's a real bug.
5. **On explicit approval ("push it live", "send it to the live files"),
   promote the preview content into the real files:** copy
   `index-preview.html` → `index.html` (etc.), then **immediately grep the
   promoted file for the string `preview`** to catch any leftover
   `-preview.css` / `-preview.js` references that need to be pointed back at
   the real filenames. This step is easy to forget and ships a live page
   that depends on files you're about to delete.
6. **Delete the preview files** once promoted: don't let them accumulate
   in the repo across rounds.
7. **Commit and push directly** (this workflow doesn't use feature branches
   or pull requests: every approved round goes straight to `main`).  Write
   the commit message around *why* the change was made, not a restatement
   of the diff.
8. **Verify the actual live file content independently of the browser**
   before considering the round done: `grep`/`curl` the file on disk or
   the served URL directly. See the caching pitfall below for why this
   matters.

If a project has more than one HTML page (e.g. a homepage and a gallery
page), apply this per HTML file: every live page that needs edits gets its
own `-preview.html` twin, while shared `style.css`/`script.js` only need one
preview copy each.

### Verification pitfalls: check these before trusting a "broken" result

**Browser HTTP cache survives file changes and even server restarts.**
After copying new content into `script.js`/`style.css`, a browser tab that
already loaded those exact URLs earlier in the session can keep serving the
old cached bytes: even after stopping and restarting the local dev server,
because HTTP cache is keyed by URL, not by server process. If a change you
just made isn't showing up in a rendered check, don't assume the file write
failed. Confirm the file on disk is correct first (`grep`/`cat`/`curl` the
served URL directly, bypassing the browser), and if the disk content is
right but the rendered page still shows old content, force a real reload
(inject a cache-busted `<script src="script.js?v=timestamp">` at runtime, or
open a completely fresh tab/server) rather than restarting the same tab
repeatedly.

**A blank or oddly-sized screenshot is very often a tooling artifact, not a
real bug.** Check the DOM, computed styles, and accessibility tree directly
(via a JS eval, not a screenshot) before concluding the page itself is
broken. If the viewport looks unexpectedly narrow/mobile-shaped in a
screenshot, explicitly resize to a real desktop width before judging layout,
and don't assume the reported viewport is accurate.

**`requestAnimationFrame`-driven effects don't reliably tick in an automated
eval context.** If you dispatch a synthetic event and then immediately read
back a style value that's supposed to update on the next animation frame,
you may read a stale value even though the code is correct: headless/
backgrounded tabs can pause `rAF` entirely. Taking an actual screenshot
forces a real compositor frame and is a more reliable way to confirm
animation-driven state than polling computed styles via eval alone.

**To visually verify a UI element too small to judge in a normal
screenshot** (e.g. a 16×22px custom cursor icon), temporarily inflate its
size and reposition it via eval, screenshot it at the larger size to confirm
the artwork/color is correct, then reload the page to restore its real
size: don't ship-and-hope on something you couldn't actually see.

**When a user reports a specific visual defect with an annotated
screenshot, treat the location as ground truth but verify the actual
mechanism before fixing it.** Don't pattern-match to "the most recently
touched related code" as the cause without checking. In one case here, a
"broken line" in a decorative SVG divider was initially assumed to be a
side effect of a just-added cursor blend-mode effect (a plausible, fast
hypothesis): the real cause was an unrelated pre-existing geometry bug (an
SVG smooth-curve control point that mirrored past the viewBox and got
silently clipped). Trace the actual coordinate math or logic before
declaring a fix and showing it to the client.

### CSS/JS gotchas worth knowing in advance

**Cascade order beats intent for equal-specificity overrides.** A rule like
`html.some-state * { cursor: none; }` can still lose to an unrelated
`.btn { cursor: pointer; }` declared later in the same stylesheet, because
equal-specificity ties resolve by source order, not by which one "seems"
more global. For an opt-in, JS-gated cosmetic override meant to always win
regardless of what else exists in the stylesheet, use `!important`
deliberately on that one narrow rule rather than chasing every conflicting
declaration.

**`mix-blend-mode` on an overlay affects everything visually beneath it,
including thin decorative line art.** A blend-mode trick used to make a
custom cursor auto-contrast against both light and dark backgrounds will
also visibly distort any hairline SVG stroke or fine graphic it passes over,
which can look like the graphic itself is broken. Fix by giving the
decorative element `isolation: isolate`, which creates its own stacking
context so external blend modes can't reach into it.

**A `position: fixed` element with `z-index: -1` sits behind normal-flow
siblings but is still visible through any of them that don't set their own
opaque background.** This is a cheap way to add a full-page background
effect (e.g. a canvas animation) that automatically "respects" section band
colors with zero extra logic: it shows through transparent/default
sections and disappears behind any section with a solid background.

**Design cosmetic JS/CSS additions (confetti, custom cursors, decorative
animations) to be trivially removable.** Keep them as self-contained,
clearly commented blocks appended to the end of `script.js`/`style.css`
rather than interleaved with core functionality. If a client tries a
decorative feature and later wants it gone, that should be a clean deletion
of an isolated block, not an archaeology project. Gate anything
motion/hover-based behind `prefers-reduced-motion` and, for cursor-style
effects, `matchMedia('(hover: hover) and (pointer: fine)')` so touch devices
and accessibility preferences are never affected.

### Content-editing discipline

**When asked to change tone/pronouns/wording "everywhere except section
X,"** grep for every instance first, list what you found, edit each
instance individually, and re-grep afterward to confirm the excluded
section still has the original wording and every other instance changed.
Don't trust a single mental pass over a long file.

**When enforcing a brand voice doc or banned-word list, verify
programmatically, not by eye.** Grep the finished content for each banned
term and for banned punctuation (e.g. em dashes) and report a zero count.
Don't just assert compliance.

### Git & repo hygiene

- Confirm repo visibility and collaborator list early, and default to
  **private** for client codebases: there's rarely a reason for the source
  itself to be publicly browsable, independent of the live site being
  public. Public repo visibility does not affect who can push; only
  collaborator access controls that. Recommend the client also enable 2FA
  on the GitHub account itself, since that's the actual point of failure,
  not repo visibility or permissions.
- Before running any git command the client didn't explicitly and
  unambiguously request, check `git status` first: a "commit the working
  tree changes" type request can be ambiguous about scope (e.g. leftover
  scratch/preview files sitting in the working directory that shouldn't be
  committed). Clarify or clean up scope before committing.
- Client-facing IDE/app chrome (e.g. a "Commit changes" or "Create PR"
  button in a desktop app's own sidebar) is separate from anything done via
  chat-driven git commands: don't assume the client knows the difference;
  say so plainly if asked, and don't rely on that UI as part of the
  workflow you're driving.

### Common post-launch add-on requests (reference, not a checklist)

- **"Can we auto-text/auto-email clients when a form is submitted?"**
  Make.com (or Zapier) plus the form tool's native webhook/watch-submission
  trigger is the standard pattern. For SMS specifically: there is no real
  free tier for outbound business SMS anywhere (carriers charge termination
  fees regardless of provider): Twilio's pay-as-you-go cost is low enough
  (roughly $1-2/month for a number, fractions of a cent per text) that it's
  the practical answer rather than chasing a free option. Flag **A2P 10DLC
  registration** early if the client will text US numbers for business
  purposes: approval takes 1-3 days and unregistered senders get filtered
  by carriers. Google Voice has no send API and cannot be wired into an
  automation platform: steer clients toward it only for personal/manual
  use, never as the automation endpoint.
- **Before building any email auto-responder against a client's contact
  address, verify that address's domain DNS first** (see lesson 12 in Part
  1). A JotForm/webhook/Gmail automation can look completely broken
  (webhook fires, Gmail module reports success, email never arrives) when
  the real cause is missing MX/SPF/DKIM on the sending domain, not
  anything wrong with the automation itself. Rule this out with a live
  `dig` lookup before debugging the scenario logic.
- **A Gmail "Send an Email" module reporting success (a real Message ID)
  does not mean the email was delivered**, only that the mailbox's own
  servers accepted it for relay. Confirm actual delivery by checking the
  recipient inbox directly (and Spam), not by trusting the automation
  platform's execution log.
- **JotForm's webhook payload nests all real answers inside a single
  `rawRequest` string** (JSON-encoded, keyed by internal question IDs like
  `q4_name`, `q9_email`, not the visible field labels), not as clean
  top-level fields. Add a JSON → Parse JSON step right after the webhook
  trigger, with its schema generated from one real captured submission, to
  get individually mappable fields.
- **A successful manual test does not mean the automation is live.**
  Make.com scenarios have a separate on/off toggle ("Immediately as data
  arrives") from the "Run once" manual test button. Confirming a test
  submission works and then walking away without switching that toggle on
  is a real, easy-to-make mistake: everything looks done, but the
  automation silently never fires on real submissions. Always verify the
  toggle is on as the explicit last step, not an assumed side effect of
  testing successfully.
- **If a scenario shows zero execution activity for a real trigger event
  (not even a failed run), the problem is upstream of the automation
  logic**, most likely the source platform's own webhook/integration
  connection having gone inactive or incomplete on its end (e.g. JotForm's
  Settings → Integrations → Webhooks silently reverting to "incomplete" and
  needing "Complete Integration" clicked again). Check the trigger side
  first, and use the automation platform's execution **History** view (not
  just the visual canvas) to distinguish a real automatic run from a
  manual test run before debugging downstream modules.
- **When rebuilding deleted automation modules, action names that sound
  similar are not interchangeable.** A Gmail "Reply to an Email" action and
  "Send an Email" action are separate items in the same app's action list;
  picking the wrong one produces a module asking for a required field that
  makes no sense for the task (e.g. a "Thread ID" for what should be a
  fresh outbound email), which is the tell that the wrong action was
  selected, not a configuration bug.
- **When verifying an outbound automated email, check the recipient's
  inbox, not the sending account's own inbox.** The sending account's own
  Inbox/Sent being empty of anything recent is not evidence of failure by
  itself, especially after testing where old sends may have been manually
  cleaned out of Sent.
- **A save in an automation platform's module editor can appear to succeed
  in the UI without actually persisting.** A large paste immediately
  followed by clicking Save can lose the update if the app's internal
  state hasn't caught up yet (seen in Make.com's field editors). After
  editing any critical field, close the module and reopen it to visually
  confirm the edit actually stuck before running a test against it. If a
  fix seems to not have taken effect, check what was *actually used* for
  that specific run in the platform's execution history/logs, not just
  what the editor currently displays; the two can disagree.
