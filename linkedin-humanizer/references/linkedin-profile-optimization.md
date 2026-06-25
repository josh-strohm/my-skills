# LinkedIn Profile Optimization

Load this file when the user asks to revamp, audit, or optimize their LinkedIn profile (headline, About section, skills, banner, featured section, experience). This is distinct from writing LinkedIn posts — this is about the profile itself as a landing page.

The framework below is based on Diandra Escobar / Distinctiva's "5-Step Play from Zero" (YouTube, 2026) which identifies LinkedIn as the most underpriced attention market in 2026 (1.3B users, only 3% post) with 90-day or less time-to-first-client.

---

## Step 1: Headline (Value Proposition, Not Job Title)

The headline is the #1 mistake most profiles make. "Owner of X" or "CEO at Y" tells nobody why they should care. The headline should follow the formula:

**Who you help + the mechanism + the outcome**

Examples:
- "AI Consultant & Founder, Strohm Partners LLC | I help small businesses stop losing leads to missed calls and slow responses"
- "Helping trade businesses never miss a lead | AI receptionist & text-back systems | Founder, Strohm Partners LLC"

**Pitfalls:**
- Don't use a rule-of-three ending ("X, Y, and Z") — it reads as an AI pattern. Two concrete items beat three vague ones. If the third item is weak ("manual workflows"), drop it.
- Don't list a job title alone. "Owner of Strohm Media" tells nobody what you do for them.
- Keep it under 220 characters (LinkedIn's limit).

---

## Step 2: About Section (Pain Points + Trust + CTA)

The About section must touch the buyer's pain points, not recite a resume. Structure:

1. **Hook**: Open with a pain statement the buyer would recognize from their own week. Not "I am passionate about..." Not "With over X years of experience..."
2. **Who you are**: One sentence — name, role, firm.
3. **The problem**: 2-3 sentences naming what's costing the buyer money. Be specific.
4. **The solution**: What you build/do. Use a short bulleted list with concrete deliverables (e.g., "AI receptionists that answer every call, book the job, and route emergencies — 24/7, no salary").
5. **Positioning**: What makes you different (consultant vs. agency, no vendor kickbacks, etc.).
6. **CTA**: Soft call to action.

**Critical platform quirk: LinkedIn About section links are NOT clickable.** They render as plain text. Do not put a URL in the About section expecting people to click it. Instead:
- Use a soft CTA like "Send me a message or connect with me here" in the About section
- Put actual clickable links in the **Featured section** (pinned items) and the **custom button** on the intro card (LinkedIn lets you add a "Visit website" or "Book an appointment" button under the headline)
- Mention the URL in the About only as text the person might type, but don't rely on it

**Style rules (same as post writing):**
- First person, conversational
- No em dashes, no rule-of-three, no buzzwords ("comprehensive," "seamless," "leverage")
- No "Let's dive in," no "In today's world of..."
- Specific numbers ground the bio. Flag any invented numbers in a "review these" note.

---

## Step 3: Skills (Max 5 Visible)

**Platform quirk: LinkedIn only lets you add 5 skills** in the profile editing flow (not 50 as some documentation suggests). Choose the 5 most relevant to the positioning:

For an AI consultancy:
1. Artificial Intelligence
2. AI Consulting
3. Business Automation
4. Lead Generation
5. Workflow Automation

**Remove** any marketing-agency-era skills (Social Media Marketing, Digital Marketing, Content Marketing, Graphic Design) that dilute the AI consultancy positioning.

---

## Step 4: Banner (Cover Image)

The banner is top real estate. It should funnel people to your world — your website, booking a call, etc.

### Critical layout rule: Profile photo overlay

LinkedIn's circular profile photo sits at the **bottom-left** of the banner and covers anything in that zone. All text must be positioned in the **RIGHT 60%** of the image. The left 40% should be empty background.

### Always check the user's actual brand colors first

Before generating a banner image, extract the real brand colors from the user's website. Use `browser_navigate` then `browser_console` to read computed styles:

```javascript
(() => {
  const body = getComputedStyle(document.body);
  const h1 = document.querySelector('h1') ? getComputedStyle(document.querySelector('h1')).color : null;
  const link = document.querySelector('a') ? getComputedStyle(document.querySelector('a')).color : null;
  // Scan CSS for hex colors and rgb values
  const sheets = document.styleSheets;
  const colors = new Set();
  for (const sheet of sheets) {
    try {
      for (const rule of sheet.cssRules) {
        const text = rule.cssText;
        if (text) {
          (text.match(/#[0-9a-fA-F]{3,8}/g) || []).forEach(m => colors.add(m));
          (text.match(/rgba?\([^)]+\)/g) || []).forEach(m => colors.add(m));
        }
      }
    } catch(e) {}
  }
  return JSON.stringify({
    bodyBg: body.backgroundColor,
    bodyColor: body.color,
    bodyFont: body.fontFamily,
    h1Color: h1,
    linkColor: link,
    allColors: [...colors].slice(0, 30)
  }, null, 2);
})()
```

Also use `browser_vision` to visually confirm the colors and aesthetic.

**Pitfall:** Do not guess brand colors. Do not default to "professional" palettes like navy+gold. The user's website is the source of truth. Generating a banner with wrong colors wastes a round and frustrates the user.

### Banner content

Keep it minimal:
- Company name (right side)
- Title/role (right side, below name)
- Thin accent line in brand color
- One CTA: "Book a Call →" (right side, below accent line)

No stock photos, no people, no icons. Clean typography on the brand background color.

### Provide the prompt alongside the image

When generating a banner with an AI image tool, always give the user:
1. The generated image
2. The full text prompt used

The user may want to try the same prompt in a different image generator. Giving them the prompt saves them from having to ask.

### LinkedIn banner dimensions

Recommended: 1584x396px (4:1 ratio). Most image generators produce 16:9 (landscape), which the user can crop when uploading. Note this in the output.

---

## Step 5: Featured Section

The Featured section is where clickable links actually work. Pin 3 items:
1. Booking page (e.g., strohmpartners.com/book or trades.strohmpartners.com/book-a-call.html)
2. Main landing page
3. A short post or case study (once the user starts posting)

---

## Step 6: Experience Section

- Update current title to match the new positioning
- Move old roles to past experience
- Bullets should focus on outcomes for clients, not generic "comprehensive marketing solutions"
- Use specific numbers (missed calls recovered, response time, lead value). Flag invented numbers.

---

## Workflow: Step-by-Step, One Item at a Time

When doing a full profile revamp, work through each section sequentially:
1. Headline
2. About section
3. Skills
4. Banner
5. Featured section
6. Experience section

**Do not move to the next item until the current one is confirmed.** The user wants to review and approve each section individually. Draft the current section, present it, get confirmation, then move on. Do not dump all sections at once.

---

## Content Pillars (Step 2 of the 5-Step Play)

After the profile is fixed, pick 3-4 content pillars. Topics the user can discuss endlessly that buyers care about. For an AI consultancy serving service businesses:

1. AI adoption for service businesses — what AI receptionist/text-back actually does, real implementation stories
2. Missed call economics — the math on what a missed call costs a business
3. Owner-operator reality — what it's like running a small service business, real talk about operations
4. AI consulting vs. marketing agency — contrarian positioning, why most businesses waste money on the wrong solutions

If you try to be everything, you become nothing. Three to four pillars max.