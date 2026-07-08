# Master Website Build Prompt: Michael Bazzi

Paste this at the start of any new Claude Code session before starting a
client website build. Fill in every bracket before sending. The more you
fill in here, the fewer questions Code asks and the fewer tokens you burn
on discovery instead of building.

---

I need a website built for **[CLIENT NAME]**, a **[ONE-LINE BUSINESS DESCRIPTION]**.
Read this entire prompt before writing any code.

---

## Client brief: fill this out completely before pasting

- **Client name:**
- **Business type:**
- **Target client** (who books or buys from them, described as a real person):
- **Brand source-of-truth** (attached file, Google Drive link, or "none, see below"):
- **Pages required** (list every page):
- **Structure:** single-page scroll with anchor nav OR multi-page site
- **Deployment target:** Netlify static / Framer / WordPress / other
- **Key reference or inspiration** (URL or screenshot; be specific about what to take from it):
- **Assets provided** (list every file attached: photos, logos, videos, brand docs):
- **Known constraints** (deadline, platform limitations, budget tier, client tech comfort):
- **Brand voice file attached:** yes / no

---

## Before you start building

- Read the entire prompt. Then read the client brief again.
- Tell me what you understand the brief to be: the site's job, the audience,
  the structure, the deployment target, and any ambiguities you need resolved.
- Ask everything you need to ask in one message. Not one question at a time.
- Do not write a single line of code until I confirm your understanding is correct.

---

## Non-negotiables: every build, every client

**Mobile-first. Always.**
Build at the smallest breakpoint first and scale up. Never build desktop and shrink.
A layout that works at 375px and scales up is always better than one that breaks at 375px.

**No AI visual fingerprints.**
No purple or violet as a primary color. No gradients anywhere. No gradient-filled
headline text. No fake stat blocks with meaningless numbers. No emoji in headings.
No "Why Choose Us" sections. No glassmorphism. No centered-everything single-column
layout from top to bottom. If a stranger glances at it for two seconds and thinks
an AI made it, the build failed. That's true regardless of whether the code works.

**Real typography. Always.**
A display font paired with a body font. Never the default system font as the entire
type system. Pull the exact font names from the client's brand source-of-truth.
Never approximate or substitute.

**Brand values are non-negotiable inputs, not starting points.**
Pull colors, fonts, and copy tone from the client's actual brand source-of-truth.
Never invent hex values. Never approximate font names. Never write copy that sounds
like it could belong to any company in the category.

**Copy must sound like the founder.**
If a brand voice file is attached, every line of copy must pass this test before
shipping: would the founder actually say this out loud? Generic luxury language,
em dashes used for effect, interchangeable template phrases, and corporate buzzwords
are build failures. They are not style choices. Flag any copy that does not pass
the test before delivering it.

**No em dashes anywhere in body copy.**
Not in headlines. Not in descriptions. Not in CTAs. Not in footers. If a sentence
needs an em dash to work, rewrite the sentence. This applies to every piece of
copy on every page of every build.

**Reference sites: take the mechanism, not the content.**
If I give you a reference site or screenshot, identify the specific signature move
that makes it feel like itself. That means the exact layout pattern, spacing rhythm,
or one distinctive element. Adapt that mechanism to this client's palette and content.
Never copy the reference's actual content, structure, or copy.

**Grid discipline.**
In any grid where item count varies per row, cap track sizes to fixed pixel values.
An unconstrained 1fr max lets lone items balloon to fill the row. Use auto-fill
not auto-fit when tile size must stay consistent.

**Interactions: one considered motion beats several competing ones.**
Keep micro-interactions restrained by default. Premium and refined brands stay subtle.
Playful brands can go bigger. Always respect prefers-reduced-motion.

**Image handling.**
Before overlaying any text or logo on a supplied photo, inspect it for existing
baked-in text or watermarks. Match container aspect ratios to source image ratios
for any fixed graphic design asset. Check what is near the edges before cropping.

**Chronological content.**
Sequence by real capture-date metadata, never filesystem timestamps. Re-exported
or AirDropped files reset their timestamps. Check the original file's metadata.

---

## Asset intake: before a single line of HTML references them

1. All photos must be JPG. All video must be MP4. Convert everything else before
   it enters the working directory. Verify orientation, file size, and quality on
   every converted file. Not a sample. Every file.
2. Never build a section around an unconverted file to swap in later.
3. If supplied photos are too dark, poorly composed, or too low-resolution for
   hero or portfolio use, say so before building around them. A flagged photo
   can be reshot. A finished layout built around a bad photo just looks bad.
4. No file over 200kb in the output without a documented reason.

---

## Working style

- Propose and wait on anything structural or ambiguous. Page architecture, any
  style choice not explicitly specified, any instruction with more than one
  reasonable reading: state your interpretation and wait for confirmation.
  Do not guess and ship.
- If I reject a choice, it is gone for the rest of that project. Do not quietly
  re-suggest it later.
- If I say something is not landing, reconsider the underlying technique, not
  just the numeric values. A subtle scale transform and a real keyframe animation
  are not the same thing.
- If a preview looks broken or blank, check the DOM and computed styles before
  concluding the page is broken. Stale preview servers cause more false alarms
  than actual build errors.
- If I go quiet mid-build, stop. Do not proceed on assumptions. Document what is
  outstanding and wait for the next session.

---

## Definition of done: every delivery, every client

- No console errors. No broken links. No missing assets.
- Responsive and actually verified at 375px, 768px, 1024px, and 1440px.
  Not assumed to work. Actually checked.
- Lighthouse performance score above 85 on mobile. Flag any image over 200kb
  or any render-blocking resource before handoff.
- All media optimized: correctly sized, compressed, right format, real poster
  frame on any video.
- Visible keyboard focus states on every interactive element.
- Copy is specific to this business. No line of copy could belong to a
  competitor's site without changing a name.
- Zero em dashes in any body copy, headline, description, CTA, or footer.
- A client handoff checklist covering every step that requires action outside
  this codebase: domain connection, DNS configuration, deployment, third-party
  embeds (forms, chat, analytics), form notification setup, and anything the
  client needs to do manually after handoff.
- A build summary: what was built, what assumptions were made, and what I
  should personally verify before this goes live.
- Local SEO baseline complete: location appears in title tag and meta
  description, LocalBusiness schema is in the head, sitemap.xml and
  robots.txt exist in the root, and footer copy references the service area.
- Google Search Console is listed in the client handoff checklist. It
  requires manual setup and cannot be skipped.
- Every contact email address published anywhere on the site (header,
  footer, CTA, schema markup) has been verified with a live DNS lookup
  (`dig MX yourdomain.com`, `dig TXT yourdomain.com`) to actually have MX,
  SPF, and DKIM records, not just a paid mailbox account. Send one real
  test email to and from that address and confirm it arrives. A mailbox
  provider reporting "activated" or a send API returning a message ID is
  not proof of working DNS or actual delivery. Do this before the address
  goes live on the site, and again before wiring any automation to it.

---

**Now:** tell me what you understand the brief to be, ask everything you need
in one message, and do not write a single line of code until I confirm.

---
---

# Brand Voice Discovery Questions
## Run these with every client before opening Code

These questions are asked in a conversation, not a form. Let the client
talk. Their exact words and phrasing become the raw material for copy.
The goal is to capture how they actually speak, not how they think they
should sound.

---

### The Founding Story
1. Why did you start this business? Not the polished version. What actually happened?
2. Was there a specific moment, experience, or frustration that pushed you to do it yourself?
3. What were you doing before this, and how does that background shape how you work now?

### The Work
4. How do you describe what you do when someone asks at a party? Not your bio. What actually comes out of your mouth?
5. What does a client get from you that they cannot get anywhere else in your market?
6. Walk me through what happens from the moment a client reaches out to the moment the job is done.
7. What do people consistently misunderstand about your work or your industry?
8. What do you refuse to compromise on, no matter what?

### The Client
9. Describe your dream client like a real person. What do they care about, what stresses them out, and what are they really paying for when they hire you?
10. Who is NOT your client? Who should not contact you?
11. What does a client say to you after a job that makes you feel like you nailed it?

### The Voice
12. Say something you have actually said to a client, in a text, a DM, or a consultation, that landed well. Something they responded to.
13. What words or phrases in your industry make you cringe when you hear other businesses use them?
14. Read these back to me and tell me which ones sound like you:
    - "We create unforgettable experiences."
    - "Every detail matters."
    - "You deserve better."
    - "We handle everything."
    - "Your vision, brought to life."
    Which one sounds most like you? Which sounds least like you? Why?
15. Finish this sentence without thinking: "I started this because..."

### The Differentiator
16. If your top three competitors all went out of business tomorrow, what would their clients be missing and what would they find when they came to you instead?
17. What do you do that looks easy from the outside but is actually hard?
18. What is something you know about your craft that most clients do not know and probably should?

---

## How to use these answers

After the conversation, look for:

- **Recurring words and phrases.** These become the brand vocabulary.
  If she says "show up" three times, that phrase goes in the copy.
- **The specific detail that no competitor would say.** That is the
  differentiator. It goes in the hero.
- **The founding wound.** The experience that made them start the business.
  That is the emotional core of the About section.
- **What they hate hearing from others in their industry.** That is the
  words-to-avoid list.
- **How they talk vs. how they think they should talk.** Use how they
  actually talk. The polished version is never as good.

Build the brand voice file from their answers before writing a single word
of copy. Every line of copy gets tested against one question: would this
founder actually say this out loud?

If the answer is no, rewrite it until they would.
