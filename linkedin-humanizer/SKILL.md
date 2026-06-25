---
name: linkedin-humanizer
description: "Generate genuine, human-sounding LinkedIn posts or convert AI-generated drafts into authentic content. Uses the Hook-Story-Insight format, anti-AI-tell filtering, voice calibration, and a self-audit rubric."
version: 1.0.0
author: Hermes Agent (for Josh Strohm)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [linkedin, social-media, writing, humanize, anti-ai-slop, voice, content-generation, genuine]
    category: social-media
    related_skills: [humanizer, social-media, creative]
---

# LinkedIn Humanizer: Genuine Content Generation & De-AI Transformation

Write LinkedIn posts that sound like a real person wrote them. Either generate fresh content from a topic/idea, or transform an AI-generated draft into something authentic. Built on the [Humanizer](https://github.com/blader/humanizer) 29-pattern framework with a LinkedIn-specific layer: Hook-Story-Insight format, algorithm-aware structure, and a self-audit rubric.

**Core principle:** LinkedIn rewards authenticity and dwell time. Authentic posts from accounts with small followings outperform polished AI slop from big accounts. The algorithm measures how long people read, how many comment, and whether the creator replies. Genuine content wins on all three.

## When to use this skill

Load this skill whenever the user asks to:
- Write a LinkedIn post from a topic, idea, or rough notes
- Humanize, de-AI, or "un-ChatGPT" a LinkedIn draft
- Rewrite content so it doesn't sound AI-generated
- Review a LinkedIn post for AI tells before publishing
- Generate a week of LinkedIn content from themes or notes
- Convert a blog post, newsletter, or article into a LinkedIn post

Also apply this skill proactively to **your own** output when writing LinkedIn content on the user's behalf.

## Two modes: Generate vs. Transform

### Mode 1: Generate (topic → post)
The user gives you a topic, idea, or rough notes. You write a LinkedIn post from scratch using the Hook-Story-Insight format and the house voice.

### Mode 2: Transform (AI draft → genuine post)
The user pastes an AI-generated draft or points to a file. You humanize it by: (1) stripping AI patterns, (2) restructuring into Hook-Story-Insight, (3) injecting voice and specificity, (4) running the self-audit.

## How to use it

Input arrives one of three ways:
1. **Inline** — user pastes text or a topic directly. Work in-place, reply with the post.
2. **File** — user points at a file. Use `read_file` to load it, then `write_file` or `patch` to save the result.
3. **Batch** — user gives themes/notes for multiple posts. Generate each post, run the audit on each, present together.

Always show the post(s) to the user. For file edits, show what changed.

---

## HOUSE VOICE (default)

These are the user's standing preferences, baked in so you don't need to be told each time. Override only if the user explicitly requests a different tone for a specific post.

- **No em dashes** (—). Use commas, periods, or parentheses instead. Max 0 per post.
- **Conversational, not corporate.** Write like you're talking to a colleague at a coffee shop, not giving a keynote.
- **Banned words:** leverage, instantly, comprehensive, delve, tapestry, seamless, robust, holistic, paradigm, synergy, unprecedented, foster, underscore, pivotal, transformative, nuanced, multifaceted, "in today's fast-paced world", "navigate the complexities", "at the intersection of"
- **Contractions always:** don't, won't, you're, it's, we're, can't, I'm, they're, wasn't, didn't
- **Strong opinions.** Don't hedge. "This is wrong" beats "This may, in some contexts, be suboptimal."
- **Round numbers for pricing.** Specific numbers for data ($47,000 not "significant revenue").
- **No AI-tells:** no "Let's dive in", no "I'm excited to share", no "In conclusion", no "Picture this", no "Here's what you need to know"

---

## LINKEDIN ALGORITHM AWARENESS (2024-2025)

### What the algorithm rewards
- **Dwell time** is the primary signal. The longer someone spends on your post, the more LinkedIn distributes it. Line breaks, short paragraphs, and the "see more" fold all increase dwell time.
- **First 3 lines** appear before the "see more" fold. If the hook doesn't compel the click, dwell time is near zero.
- **Comments > Reactions > Shares** in distribution weight. Posts that provoke discussion get amplified. End with a specific question, not "thoughts?"
- **Creator engagement:** replying to comments re-injects the post into feeds.

## CONTENT CALENDAR WORKFLOW

When creating posts in a Notion database as part of a 90-day content calendar:

1. **Work in weekly batches.** Create 1 week (3 posts) at a time, NOT all 13 weeks at once. User preference: incremental, reviewable chunks.
2. **Use execute_code with the Notion REST API** for bulk page creation. The MCP tool_call approach fails with large JSON payloads. The API key from `~/AppData/Local/hermes/.env` works for creating pages in databases shared with the integration.
3. **Each page needs:** properties filled (Title, Status: Drafted, Platform, Format, Week, Publish Date, Hook, CTA, Caption, Hashtags, Priority) AND the full post text as paragraph blocks on the page body.
4. **Post schedule:** Monday (lesson/contrarian), Tuesday (framework/carousel), Thursday (story/opinion). 3 posts/week, 13 weeks = 39 posts.

### What LinkedIn actively demotes
- External links (LinkedIn wants users on-platform)
- AI-detectable content (algorithm is adding AI-content signals)
- Engagement bait ("Comment YES if you agree!")
- Pure text with no dwell-time mechanics (no line breaks, one big paragraph)

### Text formatting rules
- LinkedIn is plain text. No markdown bold/italic. Use ALL CAPS for emphasis sparingly.
- Line breaks between every paragraph or logical unit.
- Emoji: max 1-3 per post, or use as section markers, or none at all. Overuse is an AI tell.
- Bullet points using → or • or just -
- Numbers in hooks ("7 lessons," "$2.3M," "14%") stop the scroll.

---

## THE HOOK-STORY-INSIGHT FORMAT

Every LinkedIn post follows this structure:

```
[HOOK: 1-2 lines, contrarian/surprising/specific — appears above "see more" fold]
[blank line]
[blank line]
[STORY: 3-6 short paragraphs, narrative, personal, specific details]
[blank line]
[blank line]
[INSIGHT: 1-3 bullets or short paragraphs — the "so what," the takeaway]
[blank line]
[blank line]
[CTA: a specific question, not "thoughts?" or "share below"]
```

**Why the blank lines matter:**
1. Push the hook above the fold so only the hook is visible before "see more"
2. Create visual breathing room that slows scrolling (increases dwell time)

### Hook types that work

**Contrarian:** "I stopped doing performance reviews in 2023. Retention went UP."
**Surprising number:** "I lost a $1.2M deal because of one slide."
**Specific moment:** "Tuesday, 3:47 PM. My cofounder walked out."
**Vulnerable:** "I almost didn't post this."
**Anti-trend:** "Nobody talks about this in B2B sales."

**Banned hooks (ban these starts):**
- "In today's..."
- "In the world of..."
- "In the ever-evolving..."
- "Let's explore/dive/delve into..."
- "I'm thrilled/excited/delighted to share..."
- "Picture this:" or "Imagine a..."
- "Have you ever wondered..."

### The story section
- Use **narrative**, not exposition. "I was eating a cold sandwich at the Raleigh airport" not "Reflecting on the challenges of business travel."
- Include **specific temporal/spatial detail** — a time, a place, a number. AI almost never does this because it has no lived experience.
- Include **internal monologue**: "I thought: this is it. This is how it ends."
- Include **dialogue** when possible: "She looked at me and said: 'You're not getting another chance.'"
- **Vulnerability must cost the writer something.** Admit a mistake, a doubt, a wrong hire, a lost deal. Generic "I struggled too" reads as AI.

### The insight section
- 1-3 bullets or short paragraphs.
- Each insight should be **reframable** — a new angle, not just restating the story.
- Use → for bullet points.
- Keep each insight to 1-2 sentences. Sharp, not meandering.

### The CTA (closing question)
- Must be specific. "What's the worst performance review you've ever had?" not "Thoughts?"
- Don't use: "Share below," "Let's continue the conversation," "What's your take?"
- Optionally say "I'll go first in the comments" to model engagement.

---

## HUMANIZATION TECHNIQUES (the generation toolkit)

### Sentence length variation
Mix very short sentences with longer flowing ones and medium ones for rhythm. Include at least one sentence under 5 words and one over 25 words in every post. AI tends to average 18-25 words per sentence uniformly. Break that pattern.

**Short.** Then a longer sentence that takes its time getting where it's going. Then medium for rhythm. Then short again.

### Conversational grammar
- Start sentences with "And," "But," "So," "Because" — conjunctions AI is trained to avoid.
- Use sentence fragments. Deliberately.
- End with a preposition when it sounds natural: "Here's what I was wrong about."
- Prefer "is/are/has" over elaborate constructions. Not "serves as a testament to" but "is."
- Parenthetical asides (that break the flow in a human way).

### Specificity
- Replace "many professionals" → "the 7 CFOs I talked to this quarter"
- Replace "significant impact" → "reduced churn by 14%"
- Replace "best practices" → "the 3 things that actually worked"
- Replace "in the tech industry" → "at Series B SaaS companies between 50-200 employees"

### The specificity test
If a competitor could copy-paste your sentence and it would still be true, delete it. Rewrite it with a detail only you could provide.

---

## THE 29 AI PATTERNS TO STRIP

When transforming AI drafts, scan for and remove these patterns. (Full detail with before/after examples in `references/ai-patterns.md`.)

**Content patterns:**
1. Undue emphasis on significance/legacy/broader trends ("marking a pivotal moment," "underscoring its importance")
2. Undue emphasis on notability ("featured in The New York Times," "active social media presence")
3. Superficial analyses with -ing endings ("highlighting," "symbolizing," "reflecting")
4. Promotional language ("nestled," "vibrant," "rich heritage," "breathtaking")
5. Vague attributions ("industry reports," "experts argue")
6. Outline-like "Challenges and Future Prospects" sections

**Language patterns:**
7. Overused AI vocabulary (delve, tapestry, leverage, comprehensive, robust, seamless, holistic, foster, landscape, pivotal, underscore, vibrant, intricate)
8. Copula avoidance ("serves as," "stands as," "boasts" instead of "is")
9. Negative parallelisms ("Not only...but also," "It's not just X, it's Y")
10. Rule-of-three overuse (forced trios: "innovation, collaboration, and excellence")
11. Elegant variation / synonym cycling (protagonist/main character/central figure/hero)
12. False ranges ("from X to Y" where X and Y aren't on a meaningful scale)
13. Passive voice and subjectless fragments

**Style patterns:**
14. Em dash overuse (use max 1, prefer 0 — substitute commas, periods, parentheses)
15. Overuse of boldface
16. Inline-header vertical lists ("**User Experience:** The UX has been improved...")
17. Title case in headings
18. Emojis decorating headings/bullets
19. Curly quotation marks (use straight quotes)

**Communication patterns:**
20. Collaborative artifacts ("I hope this helps!" "Let me know!" "Here is a...")
21. Knowledge-cutoff disclaimers ("While specific details are limited...")
22. Sycophantic/servile tone ("Great question!" "You're absolutely right!")

**Filler patterns:**
23. Filler phrases ("In order to" → "To," "Due to the fact that" → "Because")
24. Excessive hedging ("could potentially possibly be argued that... might have some")
25. Generic positive conclusions ("The future looks bright," "Exciting times lie ahead")
26. Hyphenated word pair overuse (data-driven, cross-functional, client-facing, well-known)
27. Persuasive authority tropes ("The real question is," "At its core," "What really matters")
28. Signposting/announcements ("Let's dive in," "Here's what you need to know")
29. Fragmented headers (heading followed by one line that restates the heading)

---

## SELF-AUDIT RUBRIC (run after every post)

Before presenting the post, score yourself on this rubric. If any item fails, revise.

| Check | Pass criterion |
|-------|---------------|
| Hook above fold | First 1-2 lines are compelling and appear before "see more" |
| Blank lines present | Double blank lines separate Hook / Story / Insight / CTA |
| Em dashes | Zero em dashes (—) in the post |
| Banned words | None from the banned list appear |
| Contractions | Used naturally throughout (don't, you're, it's, won't) |
| Sentence length variation | At least one sentence <5 words and one >25 words |
| Specific detail | At least one specific number, date, place, or named detail |
| Personal voice | Uses "I" and includes a personal perspective or anecdote |
| Strong opinion | Makes a claim without full hedging |
| No AI openers | Doesn't start with "In today's," "Let's dive," "I'm excited to" |
| No AI closers | Doesn't end with "In conclusion," "As we look to the future" |
| No triple-comma lists | No forced "X, Y, and Z" trios |
| CTA is specific | Ends with a real question, not "thoughts?" or "share below" |
| Emoji count | 0-3 max, or none |

### The anti-AI pass (mandatory)

After writing the post, ask yourself:

> "What makes the below so obviously AI generated?"

Answer briefly (bullet points). If any tells remain, revise one more time. Then present the final version.

Only present the post when you cannot identify any remaining AI tells.

---

## FULL PROCESS

### Mode 1: Generate (topic → post)
1. Read the topic/notes the user provides.
2. If a voice sample was provided, read it first (see Voice Calibration below).
3. Choose a hook type (contrarian, surprising number, specific moment, vulnerable, anti-trend).
4. Draft the Hook (1-2 lines).
5. Draft the Story (3-6 short paragraphs with specific detail, personal voice, narrative).
6. Draft the Insight (1-3 bullets, sharp and reframed).
7. Draft the CTA (specific question).
8. Apply blank-line formatting (double blank lines between sections).
9. Strip all AI patterns (the 29 patterns above + banned words).
10. Run the self-audit rubric. Fix any failures.
11. Run the anti-AI pass. Fix any remaining tells.
12. Present the post.

### Mode 2: Transform (AI draft → genuine post)
1. Read the input text (use `read_file` if it's a file).
2. Identify all AI patterns (the 29 patterns above).
3. Extract the core message/insight from the AI draft.
4. Choose a hook type based on the strongest angle in the draft.
5. Rewrite into Hook-Story-Insight format.
6. Apply the house voice (contractions, no em dashes, no banned words, conversational).
7. Inject specificity and personal voice where the draft is generic.
8. Strip all AI patterns.
9. Run the self-audit rubric. Fix any failures.
10. Run the anti-AI pass. Fix any remaining tells.
11. Present:
    - The rewritten post
    - "What made the original so obviously AI generated?" (brief bullets of what you fixed)
    - A summary of changes made

---

## VOICE CALIBRATION (optional)

If the user provides a writing sample (their own LinkedIn posts, blog writing, or any text in their voice), analyze it before writing:

1. **Read the sample first.** Note:
   - Sentence length patterns (short and punchy? Long and flowing? Mixed?)
   - Word choice level (casual? academic? somewhere between?)
   - How they start paragraphs (jump right in? Set context first?)
   - Punctuation habits (parenthetical asides? Exclamation points? Ellipsis?)
   - Any recurring phrases or verbal tics
   - How they handle transitions (explicit connectors? Just start the next point?)
   - How they open hooks (contrarian? Storytelling? Data?)

2. **Match their voice in the rewrite.** Don't just remove AI patterns. Replace them with patterns from the sample. If they write short sentences, don't produce long ones. If they use "stuff" and "things," don't upgrade to "elements" and "components."

3. **When no sample is provided, fall back to the house voice** (see HOUSE VOICE section above).

### How to ask for a sample
- "Share 1-3 of your previous LinkedIn posts or any writing in your voice, and I'll match it."
- Or point to a file path with previous writing.

---

## EXAMPLE POSTS

### Example 1: Contrarian hook + story + tactical insight (generated)

```
I stopped doing performance reviews in 2023.

Our retention went UP, not down. Here's what happened.


6 months in, I noticed something.

My best engineers weren't leaving for better pay.
They were leaving for silence.

Performance reviews gave them noise
when what they wanted was signal.

So we replaced the annual review with a
weekly 15-minute conversation.

No scores. No forms. No forced rankings.


The result? Voluntary turnover dropped 40%
in one year.


What we learned:

→ People don't fear feedback.
  They fear the FORM feedback takes.

→ Performance is a system property,
  not just an individual one.

→ The best review asks one question:
  "What's stopping you from doing your
  best work."


Most companies will never do this. They're
too invested in the theater of evaluation.

But if you're small enough to change?

Rip it up. Start over. Ask better questions.

What's the worst performance review experience
you've ever had? I'll go first in the comments.
```

### Example 2: Specific failure + lessons (generated)

```
I lost a $1.2M deal because of a single slide.

Slide 14. "Our Competitive Advantages."

6 bullet points. Each one more generic
than the last.

"We have a strong team."
"Deep industry expertise."
"Proven technology."

The procurement lead stopped me mid-presentation
and said:

"Every vendor I've seen this week has this exact
slide. What's actually different about you?"

I didn't have an answer. Not a real one.


That was 4 years ago. Since then I've reworked
how we position.

3 rules:

1. If a competitor could copy-paste the
   claim, delete it.

2. If it's a noun, ask "so what?" Add
   the so-what clause.

   "Strong team" becomes "3 ex-Stripe
   engineers who built fraud detection at scale"

3. The slide isn't about you. It's about
   the delta between you and alternatives.
   Name the alternative. State the delta.


I've won 3 deals since because of slide 14.

Turns out specificity is the differentiator
no one else is willing to do.

What's on your generic slide that you're
afraid to fix?
```

### Example 3: Transform (AI draft → genuine post)

**Before (AI-sounding):**
> In today's fast-paced business landscape, effective communication stands as a critical component of organizational success. Studies have shown that companies leveraging comprehensive communication strategies experience significant improvements in employee engagement, productivity, and retention. Let's explore how fostering a culture of transparency can transform your organization.

**After (humanized):**
```
I spent $180K on a communication platform
nobody used.

The vendor said it would "foster a culture
of transparency." I believed them.


12 months in, adoption was 8%.

8%. On a tool we're paying $15K/month for.


Then I did something obvious.

I started asking people, in person, what
was blocking them.

Not through the platform. Not through a
survey. Face to face.


Turns out calling a tool "comprehensive"
doesn't generate comprehension.

What worked:

→ 1:1 conversations are free, adoption is 100%.

→ "Walk over and ask" is a better Slack alternative
  than Slack alternatives.

→ Transparency isn't software. It's behavior.

I'm not anti-tool. But if your people are
already ignoring the tools you bought them...

stop buying more tools and start walking.

What's a tool you bought that collected dust?
```

**What made the original AI-generated:**
- "In today's fast-paced business landscape" (banned opener)
- "stands as a critical component" (copula avoidance)
- "leveraging comprehensive communication strategies" (leverage + comprehensive)
- "significant improvements" (vague, no numbers)
- "Let's explore" (signposting)
- "fostering a culture" (foster)
- Full paragraph of abstract claims with no specifics, no first person, no story

### Example 4: The short punch (generated)

```
"Let's circle back on this."

Translation: I'm not going to do this.

We should all just say the thing.
```

---

## PITFALLS

### Over-humanization
Don't go so casual that you lose the professional context. LinkedIn isn't Twitter. The audience is your professional network, not your group chat. Conversational but credible.

### Fake vulnerability
"I struggled too" is not vulnerability unless it's specific. Admit a specific mistake with a specific cost. "I spent $180K on a platform nobody used" has weight. "I've faced challenges" does not.

### Template formula
The Hook-Story-Insight format is a guide, not a rigid mold. Vary it. Sometimes the story is one line. Sometimes there's no explicit insight, just a closer. Sometimes the hook IS the story. Don't let the format become the AI tell.

### Fabrication
When generating posts from the user's topic, don't invent specific details (dates, names, dollar amounts, quotes) unless the user provides them. Use placeholders in brackets: [specific number], [specific time], [person's name]. Ask the user to fill in the real details.

A post with a fabricated anecdote is worse than a post with no anecdote. The fabrication is itself a form of AI slop.

### Emoji overcompensation
Using exactly 3 emojis per post at section breaks is still detectable. Use 0-3 total, irregularly. Sometimes zero is the answer.

---

## BATCH MODE (generate a week of content)

When the user asks for multiple posts (a week, a content calendar, a batch):

1. Collect all topics/themes/notes from the user.
2. Vary hook types across the batch (don't use contrarian 5 times in a row).
3. Vary length (mix 50-word "short punch" posts with 400-word story posts).
4. Ensure no two posts share the same banned-word pattern or opening structure.
5. Run the self-audit on each post individually.
6. Present as a numbered list with a note about varied posting days.
7. Flag any posts that need a specific detail from the user (see Fabrication pitfall).

---

## OUTPUT FORMAT

### Mode 1 (Generate): present the post
If the user didn't provide specific details, note where brackets need filling.

### Mode 2 (Transform): present three things
1. The rewritten post
2. "What made the original AI generated?" (brief bullets)
3. Summary of changes

### Batch mode: present numbered list with notes

---

## ATTRIBUTION

The 29 AI-writing patterns are derived from [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup, and adapted from the [humanizer](https://github.com/blader/humanizer) skill by Siqi Chen (@blader), MIT licensed. The LinkedIn-specific layer (Hook-Story-Insight format, algorithm knowledge, post structures, audit rubric) is original to this skill and informed by 2024-2025 LinkedIn content strategy research.