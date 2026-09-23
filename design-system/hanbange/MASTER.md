# 한방에 — Redesign Master Spec

**Direction:** B+ "Modern Hanbang Editorial". This is the editorial proposal, which scored highest, with the judges' grafts from Clinical (A) and Accessible (C) applied, every rejected item removed, and every mustFix and constraint violation resolved.

**Persistence:** this file follows the ui-ux-pro-max Master persistence pattern (`design-system/<project>/MASTER.md`).

**Target file:** `C:\Users\junho\Downloads\교통사고 예약페이지\index.html`, a single file written in vanilla HTML, CSS and JS.

**Scope for engineers:**
- Rewrite the entire `<style>` block from scratch as one layered stylesheet with no override stacks.
- Apply the numbered markup edits in §4 and the small JS edits in §5.
- `photos.js` and `fonts/` are untouched.

**Contrast:** every ratio below was computed with the WCAG 2.x relative-luminance formula, with alpha compositing applied to translucent colours. Scripts: `C:\Users\junho\AppData\Local\Temp\claude\…\scratchpad\final\cr.py`, `final.py`.

---

## 0. Summary

### 0.1 What changes visibly

1. **First screen.** The home first screen is still the 문진.
   - A 64px masthead: a 300-weight kicker "교통사고 처리도," above the wordmark in 900/300.
   - Left: a paper 문진 카드. The sheet behind it tilts further as progress advances, a "· 작성 중" marker shows the next field, and a square 도장 "추천 완료" stamps in with an ink bleed.
   - Right: a white 문진 panel. Its header holds the progress bar, the step count and 처음부터 다시, and the safety notice is always visible.
   - When the user picks an answer, the chip flies into its card field and is "inked" in.
   - On mobile, the card becomes a sticky peek strip showing the silhouette, pain gauge and progress dots.
2. **Result.**
   - A result hero opens it: summary tags, three KPI tiles (예상 합의금 carries a `예시` tag) and a "1순위 예약하기" button.
   - A row of jump chips follows.
   - An urgent warning comes first when severe pain or red-flag symptoms were reported.
   - Evidence (insurer disclosures, composition, notes) is folded away.
   - On desktop the Q&A log collapses, the page scrolls to the top of the result and focus moves to its heading.
3. **Colour.**
   - Canvas `#f4f6f1`, deep green `#0f4a39` and mint `#7fd1b0` are kept.
   - White cards sit on a 4-step green-tinted shadow scale.
   - Sections follow a rhythm: canvas, then a white band, then the celadon "data room", then a deep-green card.
   - Amber is used only for 예시/데모 and caution, at 3% of the area or less.
4. **Type.**
   - Wanted Sans 500 display type, with a 300 kicker used only on the masthead.
   - Section titles in 600. Nothing is smaller than 12px. Body text is 16/1.7.
   - Each section gets a thin chapter numeral (01, 02…) and a short deep-green rule.
   - The highlighter marks one key phrase per heading and sweeps in once when the heading enters the viewport.
5. **Home section order.** 문진, then a trust strip (입점 12곳, 표준약관, 공시, 후기 0개, plus a 예시/데모/확인 중 legend), then the key-visual stage card, then 한눈에 보기 as 5 tiles, then 부위 선택, then 치료 프로그램 as 4×2 tiles, then the data room (후유증 and 보험 꿀팁), then 이용 안내 as 3 cards, then 왜 한의원 as a 2×2 grid, then a deep-green 찾기 card with region chips. No section, feature or content is removed.
6. **찾기, 상세, 예약.**
   - Clinic rows become white cards, with 예약 as the primary button.
   - A note explains what "확인 중" means, and the result bar stays sticky.
   - The compare tray floats.
   - Clinic detail opens as a right-side drawer on desktop and a bottom sheet on mobile.
   - Form fields are filled, 48px tall, with inline errors.
7. **Accessibility.**
   - A doctype and `lang="ko"` are added.
   - All text is at least 4.5:1 and body text at least 7:1. Control boundaries are at least 3:1.
   - Touch targets are 44px. There is one focus ring style, and focus carries on to the next question.
   - Dialogs get an accessible name, a focus trap and an inert background.
   - The tabbar 홈 bug is fixed. Charts get table alternatives.
8. **Motion.**
   - Durations are 120–400ms, using transform and opacity.
   - The spring easing is reserved for the chosen answer, the stamp and the hotspot ping.
   - Infinite decorative loops are removed (the card background spin).
   - Under reduced motion, every element shows its final state immediately.

### 0.2 Rollback (원복) — already saved, do NOT create another backup

| Where | What |
|---|---|
| git tag | `design-v1-sage` (commit `21bb3f7`). It differs from the working copy only in CRLF/LF line endings, because `core.autocrlf=true`. |
| file copy | `backups/design-v1-sage-2026-09-23/index.html`, byte-identical to the current `index.html` (verified with `cmp`). Instructions are in `backups/README.md`. |

```bash
cd "/c/Users/junho/Downloads/교통사고 예약페이지"
git checkout design-v1-sage -- index.html          # or:
cp backups/design-v1-sage-2026-09-23/index.html index.html
```

**Before starting implementation:** commit the current tree, so that the redesign becomes its own commit and the rollback stays a one-liner.

### 0.3 Decisions ledger

**Base:** Direction B (editorial), including:
- the 3/9 chapter lockup and thin numerals
- the paper card: double-rule header, rotating back sheet, "작성 중" caret, square 도장 with ink bleed
- live-ink field fill
- the 분석 중 checklist beat
- range narrowing
- the highlighter sweep
- the trust strip and legend
- the data-room band
- the guide TOC with scrollspy
- the light nav with a 3px forest masthead rule
- keyboard-only focus advance (`kb` flag)
- CSS-class log collapse
- hotspot ring radius 100
- the "이전 질문" bottom link

**Grafted from A (Clinical):**
- the white chat panel with a progress header
- the grid-paper dot texture and mint glow behind the 문진
- the answer fly-in
- the result hero with KPIs, the jump-chip nav, and evidence in `details`
- the KV stage card
- the inset deep-green find card
- the desktop right-side drawer for detail and booking
- the floating compare tray
- the sticky peek strip without an expand toggle
- filled inputs

**Grafted from C (Accessible):**
- the reveal gating: only decoration waits for reveal, content never does
- the inverted "0원" first KPI tile
- the `.find-note`
- the hatched and outlined stack segments
- the "표로 보기" table with real values
- spelled-out chart `aria-label`s
- the TOC chips
- the full-bleed mobile orbit grid

**Rejected, and not in this spec:**
- A's global 700 headline weight
- A/C's swap of the tabbar 문진 tab for 가이드
- A's removal of the nav 홈 link
- C's mint-500 wordmark dot
- C's `@layer legacy` approach. This spec is a full rewrite with no legacy layer.
- C's keycap badges and its global 1–9 key handler
- B's warmer `#f4f5ee` canvas and full-page grain. Grain appears only on the paper card and the footer.
- flat hairline lists for the program index, glance, why and find rows. These are now tiles or cards.
- numerals on everything. Numerals appear only on section chapters and inside program tiles; there are no row, result or TOC counters.
- C's red-bordered result warnbox and zebra compare rows
- B's asynchronous `closeOverlay` timer. Close stays synchronous.
- B's sticky expandable card
- A's hotspot ring radius 130, A's unconditional focus jump, A's move of the log into `<details>`, A's `opacity:0` reveal of content, and A's wholesale `h4→h3` restructure. The result template here is replaced with its content preserved verbatim.
- any infinite decorative motion
- copy changes the user has not approved (see §0.4)

**Fixed decisions honoured:**

| Decision | How this spec honours it |
|---|---|
| Wordmark | Markup and Noto 900/300 are unchanged. The dot is always `#7fd1b0`. Noto is loaded only for the glyphs "한방에." |
| Typeface | Wanted Sans for all other text. |
| Colour | Green-led and bright, with no dark theme. The only dark surfaces are the find card, footer, compare tray and toast. |
| Highlighter | Mint highlighter kept. |
| Icons | Monochrome line SVG only; no emoji. |
| Assets | The key-visual image and the AI silhouette are kept. |
| Honesty labels | Every "확인 중", "데모", "예시" and "첫 후기를 기다리고 있어요" is kept, with higher contrast. |
| Home first screen | The 문진 stays first. |
| Sections | Every section and feature stays. |

### 0.4 Pending — ask the user before changing (NOT implemented)

1. **Eyebrow "30초 문진".** The traffic-accident path has 12 questions. Option: "핵심 문진" or "약 1분".
2. **H1 wording.** It says only 교통사고, but a general-pain funnel exists. Option: `교통사고도, 일상 통증도 한방에.`
3. **"한방(韓方)" in the KV title.** Wanted Sans has no Hanja, so these characters render in a system font. Options: keep, shrink to 0.6em, or remove.
4. **Honesty inconsistency.** The "제공" tag shows for the 5 default treatments even when a clinic has `unverified:true`, while the principles copy says "제공" appears only after the clinic confirms. Option: show `제공 · 확인 중`. No label would be removed.
5. **Slab tile "20회 제한 없음".** The large numeral "20" contradicts the message. Option: "제한 없음".
6. **Tabbar 문진 → 가이드.** Not done.
7. **Footer marble.** Dropped in favour of solid forest plus light grain. To restore it, add `footer.foot{background-image:var(--marble)}`. JS still sets `--marble`.
8. **New microcopy.** These new labels were added; the user may edit any of them:
   - trust strip: 4 lines and a legend
   - result KPI labels, "빠른 문진" panel title, "근거 더 보기", "내 답변 다시 보기", "이전 질문"
   - the find-note text
   - hero CTA "지역별 한의원 바로 찾기"
9. **Review placeholder bug fix (applied).** "200자 이상" became "10자 이상" to match the validator. Tell the user.

---

## 1. Tokens

### 1.1 Stylesheet skeleton (the whole new `<style>`)

```css
@font-face { font-family: "Wanted Sans Variable"; font-weight: 100 900; font-style: normal; font-display: swap; src: url("fonts/WantedSansVariable.woff2") format("woff2-variations"), url("fonts/WantedSansVariable.woff2") format("woff2"); }
@layer reset, tokens, base, components, sections, utilities;
/* §1.2  → @layer tokens { … }
   §2    → @layer reset { … } and @layer base { … }
   §3.1, 3.2, 3.5 → @layer components { … }
   §3.3, 3.4, 3.6, 3.7 → @layer sections { … }
   §3.8  → @layer utilities { … }
   Rules:
   - Every rule lives in exactly one layer.
   - Each selector is defined once; its @media variants follow it in the same block.
   - No !important except [hidden] and reduced-motion (both in the reset layer, where important wins).
   - No raw hex outside @layer tokens (exceptions: #000 in mask gradients, white/brand in data-URI icons). */
```

### 1.2 Tokens (paste inside `@layer tokens { … }`)

```css
:root{
  /* ================= PRIMITIVES — never referenced by components ================= */
  --white:#ffffff;
  --ivory-50:#fffdf8;                 /* 문진 카드 paper */
  --sage-50:#f4f6f1;                  /* canvas (user-fixed) */
  --sage-100:#edf2ed;                 /* sunk */
  --sage-150:#e6eee7;                 /* wash — data-room band */
  --sage-200:#d9e3db;                 /* hairline — DECORATIVE ONLY (1.3:1) */
  --sage-300:#bccdc3;                 /* soft line — DECORATIVE ONLY */
  --sage-400:#8fa89c;                 /* disabled/decor — never text (2.5:1) */
  --sage-500:#67877a;                 /* UI boundary ≥3.33:1 on every light surface */
  --sage-600:#476b5d;                 /* text-3 ≥5.03:1 */
  --sage-700:#285849;                 /* text-2 ≥6.87:1 */
  --sage-800:#14483a;                 /* text-1 ink ≥8.80:1 */
  --sage-900:#0f3d30;                 /* text-strong ≥10.26:1 */
  --forest-700:#1a6c50;               /* accent text ≥5.37:1 */
  --forest-800:#0f4a39;               /* brand (user-fixed) */
  --forest-900:#0b3a2c;
  --forest-950:#082c22;
  --mint-50:#effaf5; --mint-100:#dcf3e8; --mint-200:#bfe8d6; --mint-300:#a3dec5;
  --mint-400:#7fd1b0;                 /* brand mint (user-fixed): fills/decor only, never text on light */
  --mint-600:#2f8f6b;                 /* state marks ≥3.37:1 */
  --mint-650:#267f5f;                 /* chart series 2 ≥4.14:1 */
  --amber-50:#fbf1e1; --amber-600:#b45309; --amber-700:#8f5412;   /* restrained secondary: 예시/데모/caution only */
  --red-50:#fdeeec; --red-600:#b42318;                             /* errors only */

  /* ================= SEMANTIC ================= */
  --font-sans:"Wanted Sans Variable","Wanted Sans",-apple-system,BlinkMacSystemFont,"Apple SD Gothic Neo","Malgun Gothic",system-ui,sans-serif;
  --font-brand:"Noto Sans KR",sans-serif;          /* ONLY .logo, .wm, .seal-wm */
  /* surfaces */
  --bg:var(--sage-50);
  --surface:var(--white);
  --surface-paper:var(--ivory-50);
  --surface-sunk:var(--sage-100);
  --surface-wash:var(--sage-150);
  --surface-tonal:var(--mint-100);
  --surface-tonal-soft:var(--mint-50);
  --surface-inverse:var(--forest-800);
  --surface-inverse-deep:var(--forest-900);
  --scrim:rgb(8 44 34 / .56);
  /* text */
  --text-strong:var(--sage-900);
  --text-1:var(--sage-800);
  --text-2:var(--sage-700);
  --text-3:var(--sage-600);
  --text-accent:var(--forest-700);
  --text-disabled:var(--sage-400);
  --text-on-brand:var(--white);
  /* lines */
  --line:var(--sage-200);             /* decorative dividers, card borders */
  --line-soft:var(--sage-300);        /* decorative dashed rules */
  --line-ui:var(--sage-500);          /* perceivable boundaries: chips, inputs, checkbox, toggle, gauge */
  --line-strong:var(--sage-800);      /* card header double rule, est rule */
  /* brand + accent */
  --brand:var(--forest-800); --brand-hover:var(--forest-900); --brand-press:var(--forest-950);
  --accent:var(--mint-400); --accent-strong:var(--mint-600);
  --hl:rgb(127 209 176 / .55);
  /* status — always paired with icon/text */
  --success-fg:var(--forest-800); --success-bg:var(--mint-100);
  --pending-fg:var(--sage-600);   --pending-line:var(--sage-500);           /* 확인 중 */
  --demo-fg:var(--amber-700);     --demo-bg:var(--amber-50); --demo-line:var(--amber-600);  /* 데모/예시 */
  --warn-fg:var(--amber-700);     --warn-bg:var(--amber-50); --warn-mark:var(--amber-600);
  --danger-fg:var(--red-600);     --danger-bg:var(--red-50);
  /* interaction */
  --focus:var(--forest-800); --focus-w:2px; --focus-offset:2px; --focus-halo:rgb(127 209 176 / .5);
  --state-hover:rgb(15 74 57 / .05); --state-press:rgb(15 74 57 / .09);
  --opacity-disabled:.45;
  /* data-viz */
  --chart-1:var(--forest-800);        /* series 1 solid */
  --chart-2:var(--mint-650);          /* series 2 dashed */
  --chart-3:var(--amber-600);         /* "방치" caution series, dotted */
  --chart-bar:var(--forest-800); --chart-good:var(--mint-600); --chart-context:var(--sage-500);
  --chart-track:var(--sage-100); --chart-grid:var(--sage-200); --chart-axis:var(--sage-600);
  --chart-avg:var(--sage-800); --chart-mine:var(--mint-400); --chart-band:var(--amber-50);
  --stack-1:var(--forest-800); --stack-2:var(--mint-600); --stack-3:var(--mint-300);
  --stack-4:repeating-linear-gradient(135deg,var(--sage-300) 0 2px,var(--white) 2px 6px);

  /* textures (paper objects only: .ccard, footer) */
  --grain:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 .043 0 0 0 0 .227 0 0 0 0 .173 0 0 0 .07 0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url%28%23n%29'/%3E%3C/svg%3E");
  --grain-light:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 1 0 0 0 0 1 0 0 0 0 1 0 0 0 .05 0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url%28%23n%29'/%3E%3C/svg%3E");
  /* icon masks (monochrome line; tinted with background:currentColor) */
  --ico-check:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23000' stroke-width='2.4' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M5 12l5 5L20 7'/%3E%3C/svg%3E");
  --ico-check-white:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='white' stroke-width='3' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M5 12l5 5L20 7'/%3E%3C/svg%3E");
  --ico-clock:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23000' stroke-width='2' stroke-linecap='round'%3E%3Ccircle cx='12' cy='12' r='8.5'/%3E%3Cpath d='M12 7.5V12l3 2'/%3E%3C/svg%3E");
  --ico-alert:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23000' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M12 3.5l9 16H3z'/%3E%3Cpath d='M12 10v4.5M12 17.5v.01'/%3E%3C/svg%3E");
  --ico-chev:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23000' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M9 6l6 6-6 6'/%3E%3C/svg%3E");
  --ico-chat:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23000' stroke-width='1.8' stroke-linejoin='round'%3E%3Cpath d='M4 5h16v11H9l-5 4z'/%3E%3C/svg%3E");

  /* type (desktop; overridden at 1023/767 below). Weights used: 300 (kicker/numeral), 400, 500, 600, 700. 800/900 only in the wordmark. */
  --t-display-xl:64px;   /* home masthead h1 */
  --t-display:52px;      /* page h1 .display */
  --t-display-2:44px;    /* KV h2 (always < masthead) */
  --t-h1:40px;           /* .h-md section headline */
  --t-h2:22px;           /* .h-sm */
  --t-h3:18px;           /* card titles */
  --t-lead:19px; --t-body:16px; --t-small:15px; --t-meta:14px; --t-label:13px; --t-caption:12px;  /* floor = 12 */
  --t-kpi:48px; --t-kpi-sm:30px; --t-numeral:40px;
  /* space (4pt) */
  --s-1:4px; --s-2:8px; --s-3:12px; --s-4:16px; --s-5:20px; --s-6:24px; --s-8:32px; --s-10:40px;
  --s-12:48px; --s-14:56px; --s-16:64px; --s-20:80px; --s-24:96px; --s-26:104px; --s-30:120px;
  --section-y:104px; --lockup-gap:48px; --card-pad:24px;
  /* radius */
  --r-xs:6px; --r-sm:10px; --r-md:16px; --r-lg:20px; --r-xl:28px; --r-pill:9999px;
  /* elevation — green-tinted, flat by default */
  --e1:0 1px 2px rgb(15 74 57 / .05),0 2px 6px -2px rgb(15 74 57 / .06);
  --e2:0 2px 4px rgb(15 74 57 / .04),0 10px 24px -8px rgb(15 74 57 / .14);
  --e3:0 4px 10px -4px rgb(15 74 57 / .08),0 20px 48px -16px rgb(15 74 57 / .22);
  --e4:0 8px 20px -8px rgb(15 74 57 / .12),0 32px 72px -20px rgb(15 74 57 / .30);
  /* motion */
  --d-press:100ms; --d-fast:160ms; --d-base:220ms; --d-slow:320ms; --d-max:400ms; --d-hl:480ms; --d-chart:480ms; --d-exit:200ms;
  --ease-out:cubic-bezier(.22,1,.36,1); --ease-in:cubic-bezier(.55,0,1,.45); --ease-std:cubic-bezier(.2,0,0,1);
  --ease-soft:cubic-bezier(.25,.46,.45,.94); --ease-spring:cubic-bezier(.34,1.4,.64,1);  /* spring: chosen answer, stamp, ping only */
  /* z-index */
  --z-raised:10; --z-sticky:20; --z-nav:50; --z-tabbar:55; --z-cbar:60; --z-overlay:70; --z-modal:80; --z-toast:90; --z-fly:95; --z-skip:100;
  /* layout — --nav-h must stay a px value (JS navH() parses it) */
  --wrap:1280px; --gutter:32px; --nav-h:64px; --tabbar-h:56px;
  color-scheme:light;
}
@media (max-width:1023px){ :root{ --gutter:24px; --section-y:80px; --t-display-xl:48px; --t-display:42px; --t-display-2:38px; --t-h1:34px; --t-kpi:40px; --t-numeral:34px; } }
@media (max-width:767px){ :root{ --gutter:16px; --nav-h:56px; --section-y:64px; --lockup-gap:32px; --card-pad:20px;
  --t-display-xl:30px; --t-display:32px; --t-display-2:28px; --t-h1:26px; --t-h2:19px; --t-h3:17px; --t-lead:16px; --t-kpi:32px; --t-kpi-sm:26px; --t-numeral:28px; } }

/* ================= SCOPED INVERSE (no dark theme; only these 4 surfaces) ================= */
.band-ink, footer.foot, .cbar, .toast{
  --text-strong:var(--white); --text-1:var(--white);
  --text-2:rgb(255 255 255 / .8);  --text-3:rgb(255 255 255 / .72);  --text-accent:var(--mint-300);
  --line:rgb(255 255 255 / .16);   --line-ui:rgb(255 255 255 / .48);  --line-strong:rgb(255 255 255 / .48);
  --focus:var(--mint-400);         --hl:rgb(127 209 176 / .32);       --state-hover:rgb(255 255 255 / .08);
  color:var(--text-1);
}

/* ================= LEGACY ALIASES — only names still used in inline styles (markup/JS) ================= */
:root{ --ink:var(--text-1); --slate:var(--text-3); --stone:var(--text-3); }
```

### 1.3 Contrast verification (Python, WCAG 2.x)

Surfaces used in the table:

| Surface | Value |
|---|---|
| canvas | `#f4f6f1` |
| surface | `#fff` |
| paper | `#fffdf8` |
| sunk | `#edf2ed` |
| wash | `#e6eee7` |
| tonal | `#dcf3e8` |
| tonal-soft | `#effaf5` |
| amber-50 | `#fbf1e1` |
| red-50 | `#fdeeec` |
| paper + grain | worst case, 7% ink = `#eef0e8` |
| nav glass | white at 88% over canvas |

**Text colours** (target: ≥4.5 for secondary text; ≥7 for body and headings)

| Token | canvas | surface | paper | sunk | wash | tonal | tonal-soft | amber-50 | red-50 | paper+grain | nav glass |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `--text-strong` #0f3d30 | 11.15 | 12.14 | 11.94 | 10.70 | 10.26 | 10.42 | 11.36 | 10.85 | 10.76 | 10.50 | 12.03 |
| `--text-1` #14483a | 9.56 | 10.41 | 10.24 | 9.18 | 8.80 | 8.93 | 9.74 | 9.30 | 9.23 | 9.00 | 10.31 |
| `--brand` #0f4a39 | 9.37 | 10.20 | 10.03 | 9.00 | 8.62 | 8.76 | 9.55 | 9.12 | 9.04 | 8.83 | 10.11 |
| `--text-2` #285849 | 7.47 | 8.13 | 8.00 | 7.17 | 6.87 | 6.98 | 7.61 | 7.27 | 7.21 | 7.04 | 8.06 |
| `--text-3` #476b5d | 5.47 | 5.95 | 5.85 | 5.25 | 5.03 | 5.11 | 5.57 | 5.32 | 5.28 | 5.15 | 5.90 |
| `--text-accent` #1a6c50 | 5.84 | 6.35 | 6.25 | 5.61 | 5.37 | 5.46 | 5.95 | 5.68 | 5.63 | 5.50 | 6.30 |
| `--warn-fg`/`--demo-fg` #8f5412 | 5.60 | 6.10 | 6.00 | 5.38 | 5.15 | 5.23 | 5.71 | 5.45 | 5.40 | 5.27 | 6.04 |
| `--danger-fg` #b42318 | 6.04 | 6.57 | 6.47 | 5.80 | 5.56 | 5.64 | 6.16 | 5.88 | 5.83 | 5.69 | 6.51 |

`--text-2` falls just below 7 on wash (6.87) and tonal (6.98). On those two surfaces it is used only for secondary copy, which still clears AA by a wide margin.

**Non-text marks** (target ≥3:1)

| Token | canvas | surface | paper | sunk | wash | tonal | tonal-soft | amber-50 | red-50 | paper+grain | nav glass |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `--line-ui` #67877a | 3.63 | 3.94 | 3.88 | 3.48 | 3.33 | 3.39 | 3.69 | 3.53 | 3.50 | 3.41 | 3.91 |
| `--chart-2` #267f5f | 4.51 | 4.90 | 4.82 | 4.32 | 4.14 | 4.21 | 4.59 | 4.38 | 4.35 | 4.24 | 4.86 |
| `--accent-strong` #2f8f6b | 3.67 | 3.99 | 3.93 | 3.52 | 3.37 | 3.43 | 3.74 | 3.57 | 3.54 | 3.45 | 3.95 |
| `--warn-mark`/`--chart-3` #b45309 | 4.61 | 5.02 | 4.94 | 4.43 | 4.24 | 4.31 | 4.70 | 4.49 | 4.45 | 4.34 | 4.98 |
| `--focus` #0f4a39 | 9.37 | 10.20 | 10.03 | 9.00 | 8.62 | 8.76 | 9.55 | 9.12 | 9.04 | 8.83 | 10.11 |

**Forbidden uses** (they fail):
- `--accent` #7fd1b0 as text or as the only state cue on light surfaces (1.52–1.80).
- `--line` #d9e3db (1.11–1.32) and `--line-soft` #bccdc3 (1.40–1.66) as control boundaries.
- `--text-disabled` #8fa89c as text (2.15–2.55).

**Inverse surfaces**

| Pair | Ratio |
|---|---|
| white / forest-800 | 10.20 |
| white / forest-900 | 12.68 |
| white 80% (`--text-2` inverse) / forest-800 | 7.16 |
| white 80% / forest-900 | 8.68 |
| white 72% (`--text-3` inverse) / forest-800 | 6.13 |
| white 72% / forest-900 | 7.34 |
| white 48% (UI border) / forest-800 | 3.64 |
| white 48% / forest-900 | 4.14 |
| mint-400 / forest-800 | 5.66 |
| mint-400 / forest-900 | 7.04 |
| mint-300 (`--text-accent` inverse) / forest-800 | 6.71 |
| mint-300 / forest-900 | 8.34 |
| forest-900 text on mint-400 (`.btn.on-dark`) | 7.04 |
| forest-900 text on mint-300 (hover) | 8.34 |
| white on dark highlighter (mint 32% over forest-800) | 5.44 |

**Highlighter** (mint-400 at 55%)

| Base surface | Band colour | `--text-strong` | `--text-1` |
|---|---|---|---|
| canvas | `#b4e2cd` | 8.47 | 7.26 |
| surface | `#b9e6d4` | 8.83 | 7.57 |
| sunk | `#b0e0cb` | 8.30 | 7.12 |
| wash | `#addec9` | 8.14 | 6.98 |
| paper | `#b9e5d0` | 8.76 | 7.51 |

**Component pairs**

Tags and chips:

| Pair | Ratio |
|---|---|
| `.tag.yes` forest / tonal | 8.76 |
| `.tag.demo` amber-700 / amber-50 | 5.45 |
| `.tag.fill` text-2 / sunk | 7.17 |
| `.tag.ask` text-3 on any light surface | ≥5.03 |
| chip selected, white / brand | 10.20 |
| photo "데모" badge, white on forest-950 at 80% over a white photo | 8.00 |

Chat and card:

| Pair | Ratio |
|---|---|
| bot bubble text-1 / sunk | 9.18 |
| `.me` bubble white / brand | 10.20 |
| `.cf.next` background `#ebf6ec`: text-accent / text-1 / text-3 | 5.72 / 9.37 / 5.36 |
| `.prog .dur` forest / tonal | 8.76 |
| resbar/TOC glass `#f5f7f2`: text-1 / text-3 | 9.61 / 5.50 |
| danger / red-50 | 5.83 |
| text-1 / red-50 | 9.23 |
| text-1 / amber-50 | 9.30 |
| amber-700 icon / amber-50 | 5.45 |

Body figure:

| Pair | Ratio |
|---|---|
| rest dot (mint 80% over silhouette = `#6ab698`) vs silhouette `#14483a` | 4.32 |
| selected mint-400 vs silhouette | 5.77 |
| white dot stroke vs silhouette | 10.41 |
| forest-900 focus/selected ring vs figure background: sunk / mint-50 | 11.19 / 11.88 |

Forms:

| Pair | Ratio |
|---|---|
| toggle off-knob text-3 / white | 5.95 |
| toggle track border line-ui / white | 3.94 |
| toggle on-knob white / brand | 10.20 |
| card gauge/dot borders line-ui / paper | 3.88 |

Charts:

| Pair | Ratio |
|---|---|
| s1 forest vs white | 10.20 |
| s2 mint-600 vs white | 3.99 |
| s3 mint-300 vs white | 1.52 (so it gets a 1px ink outline) |
| s4 sage-200 hatch vs white | 1.32 (so it gets hatch plus an ink outline) |
| adjacent segments s1/s2, s2/s3, s3/s4 | 2.56, 2.62, 1.16 |
| text-1 label on s3 | 6.85 |
| lines l1 vs l2 | 2.08 (so series are also told apart by dash pattern) |
| l3 amber vs white | 5.02 |
| context bar line-ui vs sunk track / white | 3.48 / 3.94 |
| hit bar forest vs tonal-soft row | 9.55 |
| severe gauge amber-600 / paper | 4.94 |
| typing dots accent-strong / sunk | 3.52 |

Stacked-bar segments are separated by a 2px surface gap, s3 and s4 carry the outline and hatch listed above, and every percentage is printed in the legend.

Decorative and exempt pairs:

| Pair | Ratio | Status |
|---|---|---|
| scrim over canvas | 3.62 | perceptible |
| wordmark dot mint-400 / nav glass | 1.79 | decorative (brand-fixed); the word is legible without it |
| wordmark dot on footer | 7.04 | — |
| disabled buttons at opacity .45 | — | exempt, and always paired with `disabled` / `aria-disabled` |

---

## 2. Base & typography

```css
@layer reset {
  *,*::before,*::after{box-sizing:border-box}
  [hidden]{display:none!important}
  html{-webkit-text-size-adjust:100%;text-size-adjust:100%;scroll-padding-top:calc(var(--nav-h) + 16px)}
  body{margin:0}
  h1,h2,h3,h4,h5,p,ol,ul,dl,dd,figure{margin:0}
  ol,ul{padding:0;list-style:none}
  button,input,select,textarea{font:inherit;color:inherit;letter-spacing:inherit}
  button{cursor:pointer;background:none;border:0;padding:0}
  a{color:inherit;text-decoration:none}
  img{display:block;max-width:100%}
  table{border-collapse:collapse}
  @media (prefers-reduced-motion:reduce){
    *,*::before,*::after{animation-duration:1ms!important;animation-iteration-count:1!important;animation-delay:0ms!important;transition-duration:1ms!important;transition-delay:0ms!important;scroll-behavior:auto!important}
  }
}

@layer base {
  body{background:var(--bg);color:var(--text-1);font:400 var(--t-body)/1.65 var(--font-sans);letter-spacing:-.005em;word-break:keep-all;overflow-wrap:break-word;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}
  body.has-tabbar{padding-bottom:calc(var(--tabbar-h) + env(safe-area-inset-bottom))}
  body:has(#cbar[data-show="true"]){padding-bottom:96px}
  body.has-tabbar:has(#cbar[data-show="true"]){padding-bottom:calc(var(--tabbar-h) + 80px + env(safe-area-inset-bottom))}
  h1,h2,h3,h4,h5{text-wrap:balance}
  p,li,dd{text-wrap:pretty}
  ::selection{background:var(--mint-200);color:var(--text-1)}
  :focus-visible{outline:var(--focus-w) solid var(--focus);outline-offset:var(--focus-offset)}
  svg.i,svg.pic{flex:0 0 auto;fill:none;stroke:currentColor;stroke-width:1.5;stroke-linecap:round;stroke-linejoin:round}
  svg.i{width:20px;height:20px}
  svg.pic{width:22px;height:22px}
  .num{font-variant-numeric:tabular-nums}
  .sr{position:absolute;width:1px;height:1px;margin:-1px;padding:0;overflow:hidden;clip:rect(0 0 0 0);white-space:nowrap;border:0}
  .skip{position:absolute;left:16px;top:-64px;z-index:var(--z-skip);display:inline-flex;align-items:center;min-height:44px;padding:0 18px;border-radius:var(--r-pill);background:var(--brand);color:var(--white);font:600 15px/1 var(--font-sans);box-shadow:var(--e3)}
  .skip:focus{top:12px}

  /* ---- type roles ---- */
  .display{font-size:var(--t-display);font-weight:500;line-height:1.1;letter-spacing:-.03em;color:var(--text-strong);outline:none}
  .display .k{font-weight:300}                       /* kicker — only ≥28px */
  .h-md{font-size:var(--t-h1);font-weight:600;line-height:1.2;letter-spacing:-.025em;color:var(--text-strong)}
  .h-sm{font-size:var(--t-h2);font-weight:600;line-height:1.35;letter-spacing:-.015em;color:var(--text-1)}
  .lead{max-width:34em;font-size:var(--t-lead);font-weight:400;line-height:1.6;letter-spacing:-.005em;color:var(--text-2)}
  .body{font-size:var(--t-body);line-height:1.7;color:var(--text-2)}
  .eyebrow{display:inline-flex;align-items:center;gap:8px;font-size:var(--t-label);font-weight:600;line-height:1.4;letter-spacing:.02em;color:var(--text-accent)}
  .eyebrow::before{content:"";flex:0 0 auto;width:6px;height:6px;border-radius:50%;background:var(--accent)}  /* echoes the logo dot */
  .meta{font-size:var(--t-meta);line-height:1.55;color:var(--text-3)}
  p.meta{max-width:46em}
  .micro{font-size:var(--t-caption);font-weight:600;line-height:1.35;letter-spacing:.02em;color:var(--text-3)}
  .muted{color:var(--text-3)}

  /* ---- highlighter: one key phrase per heading, never body/buttons/numbers/wordmark ---- */
  .hl{background-image:linear-gradient(transparent 58%,var(--hl) 58%,var(--hl) 94%,transparent 94%);background-repeat:no-repeat;background-position:0 0;background-size:100% 100%;
      -webkit-box-decoration-break:clone;box-decoration-break:clone;padding:0 .08em;margin:0 -.08em;transition:background-size var(--d-hl) var(--ease-out) 120ms}

  /* ---- wordmark (Noto Sans KR 900/300, mint dot — user-fixed) ---- */
  .logo{display:inline-flex;align-items:center;min-height:44px;font-family:var(--font-brand);font-size:24px;line-height:1;letter-spacing:-.05em;color:var(--text-strong);white-space:nowrap}
  .wm{font-family:var(--font-brand);letter-spacing:-.05em;white-space:nowrap}
  .logo b,.wm b{font-weight:900}
  .logo i,.wm i{font-style:normal;font-weight:300}
  .logo .dot,.wm .dot{margin-left:.02em;font-weight:900;color:var(--mint-400)}
  @media (max-width:767px){ .logo{font-size:22px} }

  /* ---- keyframes (all ≤400ms except one-shot chart/hl 480ms; loaders may loop) ---- */
  @keyframes fade-in{from{opacity:0}to{opacity:1}}
  @keyframes fade-up{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}
  @keyframes rise{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}
  @keyframes me-in{from{opacity:0;transform:scale(.92)}to{opacity:1;transform:none}}
  @keyframes pick{0%{transform:scale(.96)}60%{transform:scale(1.04)}100%{transform:none}}
  @keyframes blink{0%,80%,100%{opacity:.3}40%{opacity:1}}
  @keyframes ink{from{clip-path:inset(0 100% 0 0)}to{clip-path:inset(0 0 0 0)}}
  @keyframes ping{0%{transform:scale(.4)}60%{transform:scale(1.25)}100%{transform:scale(1)}}
  @keyframes stamp{0%{opacity:0;transform:rotate(-14deg) scale(1.3)}70%{opacity:1;transform:rotate(-7deg) scale(.97)}100%{opacity:1;transform:rotate(-8deg) scale(1)}}
  @keyframes bleed{from{box-shadow:0 0 0 0 rgb(15 74 57 / .28)}to{box-shadow:0 0 0 14px rgb(15 74 57 / 0)}}
  @keyframes step-in{from{opacity:0;transform:translateX(16px)}to{opacity:1;transform:none}}
  @keyframes step-back{from{opacity:0;transform:translateX(-16px)}to{opacity:1;transform:none}}
  @keyframes sheet-up{from{opacity:.6;transform:translateY(32px)}to{opacity:1;transform:none}}
  @keyframes drawer-in{from{opacity:.6;transform:translateX(40px)}to{opacity:1;transform:none}}
  @keyframes drop-in{from{opacity:0;transform:translateY(-8px)}to{opacity:1;transform:none}}
  @keyframes toast-in{from{opacity:0;transform:translate(-50%,8px)}to{opacity:1;transform:translate(-50%,0)}}
  @keyframes spin{to{transform:rotate(360deg)}}
}
```

**Global interaction rules** (these apply to every component in §3):
- **State priority:** disabled > loading > selected/active > focus > hover > default.
- **Hover** is only defined inside `@media (hover:hover)`.
- **Press:** `scale(.97–.98)` over `--d-press`.
- **Touch:** targets are 44px or larger under `(pointer:coarse)`.
- **Icons:** `aria-hidden="true"` when decorative.
- **Programmatic focus targets** (`.display`, `.res-title`, `.step-q`, `#panelTitle`) have `outline:none`. They are `tabindex="-1"` headings, not controls.

---

## 3. Component specs by area

Each block is literal CSS. The `/* note */` comments state intent. Any class not listed is either dead (the list is in §6.10) or intentionally unstyled.

### 3.1 FOUNDATION — `@layer components`

Classes covered:

| Group | Classes |
|---|---|
| Layout | `page`, `wrap`, `sec`, `rule` (unstyled on purpose), `g57`, `g75`, `band-surface`, `band-wash`, `lockup` |
| Type | `display`, `eyebrow`, `lead`, `h-md`, `h-sm`, `body`, `meta`, `micro`, `muted`, `num`, `sr`, `skip`, `hl` (these are in §2) |
| Brand | `wm`, `logo`, `dot` |
| Icons | `i`, `pic` |
| Buttons and links | `btn`, `primary`, `ghost`, `ghost-dark`, `on-dark`, `sm`, `lg`, `tlink` |
| Chips and tags | `chip`, `chips`, `cnt`, `tag`, `tags`, `yes`, `ask`, `fill`, `no` |
| Honesty and state | `demo`, `on` (scoped per component) |
| Lists | `spec`, `dash` |

```css
/* ---------- layout ---------- */
.wrap{max-width:var(--wrap);margin:0 auto;padding:0 var(--gutter)}
.page{counter-reset:chap}
.sec{padding:var(--section-y) 0}
/* .sec.rule: intentionally unstyled — the lockup carries the rule now */
.g75,.g57{display:grid;align-items:start;gap:24px 48px}
.g75{grid-template-columns:minmax(0,7fr) minmax(0,5fr)}
.g57{grid-template-columns:minmax(0,5fr) minmax(0,7fr)}
.band-surface{background:var(--surface)}
.band-wash{background:var(--surface-wash)}
.band-wash > .sec + .sec{padding-top:32px}
@media (max-width:1023px){ .g75,.g57{grid-template-columns:minmax(0,1fr);gap:32px} }

/* ---------- section lockup = editorial chapter (full-width lockups only) ---------- */
.lockup{display:grid;gap:12px;max-width:760px;margin-bottom:var(--lockup-gap)}
.lockup .lead{max-width:34em}
.sec > .wrap > .lockup{position:relative;max-width:none;padding-top:20px;border-top:1px solid var(--line);counter-increment:chap}
.sec > .wrap > .lockup::before{content:"";position:absolute;left:0;top:-1px;width:48px;height:2px;background:var(--brand)}
.sec > .wrap > .lockup > .eyebrow{display:flex;flex-direction:column;align-items:flex-start;gap:10px}
.sec > .wrap > .lockup > .eyebrow::before{content:counter(chap,decimal-leading-zero);content:counter(chap,decimal-leading-zero) / "";
  width:auto;height:auto;border-radius:0;background:none;font:300 var(--t-numeral)/1 var(--font-sans);letter-spacing:-.02em;color:var(--text-1);font-variant-numeric:tabular-nums}
#home-glance .lockup{counter-increment:none}                       /* service index: unnumbered */
#home-glance .lockup > .eyebrow{flex-direction:row;align-items:center;gap:8px}
#home-glance .lockup > .eyebrow::before{content:"";width:6px;height:6px;border-radius:50%;background:var(--accent)}
@media (min-width:1024px){
  .sec > .wrap > .lockup{grid-template-columns:minmax(0,3fr) minmax(0,9fr);column-gap:32px}
  .sec > .wrap > .lockup > .eyebrow{grid-column:1;grid-row:1 / span 3;align-self:start}
  .sec > .wrap > .lockup > :not(.eyebrow){grid-column:2}
}
/* numbering result: home 01 부위 · 02 프로그램 · 03 후유증 · 04 보험 꿀팁 · 05 이용 안내 · 06 왜 한의원;
   guide 01–04; reviews 01; partner 01–02. Hidden lockups (display:none) do not count. */

/* ---------- buttons (local tokens; variants override them) ---------- */
.btn{--btn-h:44px;--btn-px:20px;--btn-fs:15px;--btn-bg:transparent;--btn-fg:var(--text-1);--btn-bd:transparent;--btn-bg-h:var(--btn-bg);--btn-bd-h:var(--btn-bd);
  position:relative;display:inline-flex;align-items:center;justify-content:center;gap:8px;min-height:var(--btn-h);padding:0 var(--btn-px);
  border:1px solid var(--btn-bd);border-radius:var(--r-pill);background:var(--btn-bg);color:var(--btn-fg);font:600 var(--btn-fs)/1 var(--font-sans);letter-spacing:-.005em;white-space:nowrap;cursor:pointer;touch-action:manipulation;
  transition:background-color var(--d-fast) var(--ease-std),border-color var(--d-fast) var(--ease-std),color var(--d-fast) var(--ease-std),box-shadow var(--d-fast) var(--ease-std),transform var(--d-press) var(--ease-out)}
.btn .i{width:18px;height:18px}
.btn.primary{--btn-bg:var(--brand);--btn-fg:var(--text-on-brand);--btn-bd:var(--brand);--btn-bg-h:var(--brand-hover);--btn-bd-h:var(--brand-hover)}            /* 10.20 */
.btn.ghost{--btn-bg:var(--surface);--btn-fg:var(--text-1);--btn-bd:var(--line-ui);--btn-bg-h:var(--surface-tonal-soft);--btn-bd-h:var(--brand)}          /* border 3.94 */
.btn.on-dark{--btn-bg:var(--mint-400);--btn-fg:var(--forest-900);--btn-bd:var(--mint-400);--btn-bg-h:var(--mint-300);--btn-bd-h:var(--mint-300)}           /* 7.04 / 8.34 */
.btn.ghost-dark{--btn-bg:transparent;--btn-fg:var(--white);--btn-bd:rgb(255 255 255 / .48);--btn-bg-h:rgb(255 255 255 / .08);--btn-bd-h:var(--white)}   /* border 3.64 */
.btn.sm{--btn-h:36px;--btn-px:14px;--btn-fs:14px}
.btn.lg{--btn-h:52px;--btn-px:28px;--btn-fs:16px}
@media (pointer:coarse){ .btn.sm{--btn-h:44px} }
@media (hover:hover){
  .btn:not(:disabled):hover{background:var(--btn-bg-h);border-color:var(--btn-bd-h)}
  .btn.primary:not(:disabled):hover{box-shadow:var(--e2);transform:translateY(-1px)}
}
.btn:not(:disabled):active{transform:scale(.98);transition-duration:60ms}
.btn.primary:not(:disabled):active{background:var(--brand-press)}
.btn:disabled,.btn[aria-disabled="true"]{opacity:var(--opacity-disabled);cursor:not-allowed;box-shadow:none;transform:none}   /* fixes #cbarOpen */
.btn[aria-busy="true"]{color:transparent;pointer-events:none}
.btn[aria-busy="true"]::after{content:"";position:absolute;width:18px;height:18px;border:2px solid var(--btn-fg);border-right-color:transparent;border-radius:50%;animation:spin 700ms linear infinite}

/* ---------- text link: underline always present (not colour-only) ---------- */
.tlink{display:inline-flex;align-items:center;gap:6px;min-height:44px;padding:0 2px;font:600 15px/1.3 var(--font-sans);color:var(--text-accent);cursor:pointer;
  text-decoration:underline;text-decoration-thickness:1px;text-underline-offset:5px;text-decoration-color:var(--line-soft);transition:text-decoration-color var(--d-fast)}
@media (hover:hover){ .tlink:hover{text-decoration-color:currentColor} }
.tlink.muted{color:var(--text-2)}
.tlink.on-dark{color:var(--text-2);text-decoration-color:rgb(127 209 176 / .6)}
p .tlink,.meta .tlink,.body .tlink{min-height:0;padding:0}

/* ---------- chips (filters, districts, hot chips, form chips, slots) ---------- */
.chips{display:flex;flex-wrap:wrap;gap:8px}
.chip{--chip-h:40px;display:inline-flex;align-items:center;gap:6px;min-height:var(--chip-h);padding:0 16px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);color:var(--text-1);
  font:500 14px/1 var(--font-sans);white-space:nowrap;cursor:pointer;touch-action:manipulation;
  transition:background-color var(--d-fast) var(--ease-std),border-color var(--d-fast) var(--ease-std),color var(--d-fast) var(--ease-std),transform var(--d-press) var(--ease-out)}
@media (pointer:coarse){ .chip{--chip-h:44px} }
.chip .pic{width:16px;height:16px;color:var(--text-3)}
.chip .cnt{font-size:13px;font-weight:600;color:var(--text-3)}
@media (hover:hover){ .chip:not(:disabled):not([aria-pressed="true"]):hover{border-color:var(--brand);background:var(--surface-tonal-soft)} }
.chip:not(:disabled):active{transform:scale(.97)}
.chip[aria-pressed="true"]{padding-left:12px;border-color:var(--brand);background:var(--brand);color:var(--text-on-brand);font-weight:600}   /* 10.20 + check = not colour-only */
.chip[aria-pressed="true"]::before{content:"";flex:0 0 auto;width:14px;height:14px;background:currentColor;-webkit-mask:var(--ico-check) center/contain no-repeat;mask:var(--ico-check) center/contain no-repeat}
.chip[aria-pressed="true"] .pic{display:none}
.chip[aria-pressed="true"] .cnt{color:rgb(255 255 255 / .8)}
.chip:disabled{border-style:dashed;background:var(--surface-sunk);color:var(--text-3);text-decoration:line-through;cursor:not-allowed}

/* ---------- tags: honest-data vocabulary (restyle only — never remove) ---------- */
.tags{display:flex;flex-wrap:wrap;gap:6px}
.tag{display:inline-flex;align-items:center;gap:4px;height:24px;padding:0 8px;border:1px solid transparent;border-radius:var(--r-xs);font:600 12px/1 var(--font-sans);letter-spacing:.01em;white-space:nowrap;vertical-align:middle}
.tag.yes{--m:var(--ico-check);background:var(--success-bg);color:var(--success-fg)}                       /* 제공 · 자보 적용 · 진료 확인됨 — 8.76 + check */
.tag.ask{--m:var(--ico-clock);border:1px dashed var(--pending-line);background:transparent;color:var(--pending-fg)}  /* 확인 중 — ≥5.03 + clock + dashed */
.tag.fill{background:var(--surface-sunk);color:var(--text-2)}                                                /* neutral facts — 7.17 */
.tag.demo{border:1px dashed var(--demo-line);background:var(--demo-bg);color:var(--demo-fg)}                /* 데모 · 예시 — 5.45 + dashed */
.tag.yes::before,.tag.ask::before{content:"";flex:0 0 auto;width:12px;height:12px;background:currentColor;-webkit-mask:var(--m) center/contain no-repeat;mask:var(--m) center/contain no-repeat}
/* photo "데모" badges */
.thumb .demo,.gal .demo,.pgrid .demo,.rvcard .ph .demo{position:absolute;left:8px;bottom:8px;display:inline-flex;align-items:center;height:22px;padding:0 7px;border-radius:var(--r-xs);background:rgb(8 44 34 / .8);color:var(--white);font:600 12px/1 var(--font-sans);letter-spacing:.01em}

/* ---------- definition lists + dash lists ---------- */
dl.spec{border-top:1px solid var(--line)}
dl.spec > div{display:grid;grid-template-columns:112px minmax(0,1fr);gap:16px;padding:12px 0;border-bottom:1px solid var(--line)}
dl.spec dt{padding-top:2px;font:600 13px/1.4 var(--font-sans);color:var(--text-3)}
dl.spec dd{font-size:16px;line-height:1.5;color:var(--text-1)}
dl.spec dd small{display:block;margin-top:2px;font-size:13px;line-height:1.45;color:var(--text-3)}
@media (max-width:767px){ dl.spec > div{grid-template-columns:96px minmax(0,1fr);gap:12px} }
.dash{display:grid;gap:8px}
.dash li{position:relative;padding-left:18px;font-size:15px;line-height:1.65;color:var(--text-2)}
.dash li::before{content:"";position:absolute;left:2px;top:.78em;width:8px;height:2px;border-radius:2px;background:var(--text-3)}
```

### 3.2 NAV + FOOTER + TABBAR + OVERLAYS — `@layer components`

Classes covered:

| Group | Classes |
|---|---|
| Nav | `nav`, `nav-in`, `nav-right`, `menu`, `hamb`, `mmenu`, `auth`, `region-btn` (new) |
| Footer | `foot`, `cols`, `bot` |
| Tabbar and compare bar | `tabbar`, `has-tabbar`, `cbar`, `names` |
| Overlays | `toast`, `overlay`, `center`, `sheet`, `sheet-h`, `sheet-b`, `sheet-f`, `one`, `modal`, `xbtn` |

Nav states:
- **Default:** light frosted glass with a 3px forest masthead rule.
- **Scrolled** (`.scrolled`, set by J7): adds `--e2`.
- **Active menu link** (`.on` plus `aria-current`): text-strong, weight 700 and a 2px brand bar. The bar is the cue that does not rely on colour.

```css
/* ---------- nav ---------- */
.nav{position:sticky;top:0;z-index:var(--z-nav);background:rgb(255 255 255 / .88);-webkit-backdrop-filter:saturate(1.3) blur(14px);backdrop-filter:saturate(1.3) blur(14px);
  border-top:3px solid var(--brand);box-shadow:inset 0 -1px 0 var(--line);color:var(--text-1);transition:box-shadow var(--d-base) var(--ease-std)}
.nav.scrolled{box-shadow:inset 0 -1px 0 var(--line),var(--e2)}
.nav-in{height:calc(var(--nav-h) - 3px);display:flex;align-items:center;gap:28px}
.menu{display:flex;gap:2px}
.menu a{position:relative;display:inline-flex;align-items:center;min-height:44px;padding:0 12px;border-radius:var(--r-pill);font:600 15px/1 var(--font-sans);color:var(--text-2);transition:color var(--d-fast),background-color var(--d-fast)}
@media (hover:hover){ .menu a:hover{color:var(--text-1);background:var(--state-hover)} }
.menu a.on{color:var(--text-strong);font-weight:700}
.menu a.on::after{content:"";position:absolute;left:12px;right:12px;bottom:4px;height:2px;border-radius:2px;background:var(--brand)}
.nav-right{display:flex;align-items:center;gap:8px;margin-left:auto}
.nav-right .tlink{padding:0 8px;color:var(--text-2);text-decoration:none}
.region-btn .i{width:16px;height:16px;color:var(--brand)}
.hamb{display:none;align-items:center;justify-content:center;width:44px;height:44px;border-radius:var(--r-pill);color:var(--text-1)}
@media (hover:hover){ .hamb:hover{background:var(--state-hover)} }
.mmenu{display:none}
.mmenu[data-open="true"]{position:absolute;left:0;right:0;top:100%;display:block;max-height:calc(100vh - var(--nav-h));max-height:calc(100dvh - var(--nav-h));overflow-y:auto;overscroll-behavior:contain;
  background:var(--surface);border-top:1px solid var(--line);box-shadow:var(--e3);animation:drop-in var(--d-base) var(--ease-out)}      /* fixes white-on-white (old L610) */
.mmenu a{display:flex;align-items:center;justify-content:space-between;min-height:56px;padding:0 var(--gutter);border-bottom:1px solid var(--line);font:600 17px/1.2 var(--font-sans);color:var(--text-1)}
.mmenu a::after{content:"";width:16px;height:16px;background:var(--text-3);-webkit-mask:var(--ico-chev) center/contain no-repeat;mask:var(--ico-chev) center/contain no-repeat}
.mmenu a.on{color:var(--brand);background:var(--surface-tonal-soft);box-shadow:inset 3px 0 0 var(--brand)}
.mmenu .auth{display:flex;gap:8px;padding:16px var(--gutter) 24px}
.mmenu .auth .btn{flex:1;--btn-h:48px}
@media (max-width:1023px){ .menu{display:none} .hamb{display:inline-flex} .nav-right .tlink{display:none} }
@media (max-width:767px){
  .nav-right .btn.primary{display:none}
  .region-btn{--btn-px:0;width:44px}
  .region-btn #regionLabel{position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0 0 0 0);white-space:nowrap}   /* stays in accessible name */
}

/* ---------- footer (site footer only — never .mock .foot) ---------- */
footer.foot{position:relative;overflow:hidden;padding:72px 0 calc(40px + env(safe-area-inset-bottom));background:var(--surface-inverse-deep) var(--grain-light);color:var(--text-1)}
footer.foot::before{content:"";position:absolute;right:-10%;top:-45%;width:60%;aspect-ratio:1;border-radius:50%;pointer-events:none;background:radial-gradient(circle,rgb(127 209 176 / .14),transparent 65%)}
footer.foot .wrap{position:relative}
footer.foot .cols{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:24px}
footer.foot .cols h4{margin-bottom:12px;font:600 12px/1.3 var(--font-sans);letter-spacing:.06em;color:var(--text-3)}          /* 7.34 */
footer.foot .cols a{display:flex;align-items:center;min-height:40px;font-size:15px;color:var(--text-2)}                        /* 8.68 */
@media (pointer:coarse){ footer.foot .cols a{min-height:44px} }
@media (hover:hover){ footer.foot .cols a:hover{color:var(--white);text-decoration:underline;text-underline-offset:4px} }
footer.foot .bot{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:baseline;gap:24px;margin-top:48px;padding-top:24px;border-top:1px solid var(--line)}
footer.foot .logo{font-size:22px;color:var(--white)}
footer.foot .bot .meta{max-width:60em;font-size:13px;color:var(--text-3)}
@media (max-width:767px){ footer.foot{padding:56px 0 32px} footer.foot .cols{grid-template-columns:repeat(2,minmax(0,1fr))} }

/* ---------- tabbar (mobile) ---------- */
.tabbar{display:none}
@media (max-width:767px){
  .tabbar{position:fixed;left:0;right:0;bottom:0;z-index:var(--z-tabbar);display:grid;grid-template-columns:repeat(5,minmax(0,1fr));height:calc(var(--tabbar-h) + env(safe-area-inset-bottom));padding-bottom:env(safe-area-inset-bottom);
    background:rgb(255 255 255 / .96);-webkit-backdrop-filter:blur(12px);backdrop-filter:blur(12px);border-top:1px solid var(--line);box-shadow:0 -4px 16px -8px rgb(15 74 57 / .12)}
  .tabbar button{position:relative;display:grid;justify-items:center;align-content:center;gap:3px;min-height:var(--tabbar-h);font:600 12px/1 var(--font-sans);color:var(--text-3)}     /* 5.95 */
  .tabbar button .i{width:24px;height:24px}
  .tabbar button.on{color:var(--brand);font-weight:700}
  .tabbar button.on .i{stroke-width:2}
  .tabbar button.on::before{content:"";position:absolute;top:0;left:30%;right:30%;height:3px;border-radius:0 0 3px 3px;background:var(--brand)}   /* non-colour cue */
  .tabbar .dot{position:absolute;top:8px;left:calc(50% + 7px);width:9px;height:9px;border-radius:50%;background:var(--accent-strong);box-shadow:0 0 0 2px var(--surface)}
}

/* ---------- compare tray (floating) ---------- */
.cbar{position:fixed;left:50%;bottom:calc(16px + env(safe-area-inset-bottom));z-index:var(--z-cbar);width:min(760px,calc(100% - 32px));min-height:64px;border-radius:var(--r-lg);
  background:var(--surface-inverse);box-shadow:var(--e4);visibility:hidden;transform:translate(-50%,calc(100% + 48px));
  transition:transform var(--d-exit) var(--ease-in),visibility 0s linear var(--d-exit)}
.cbar[data-show="true"]{visibility:visible;transform:translate(-50%,0);transition:transform var(--d-slow) var(--ease-out),visibility 0s}
.cbar .wrap{display:flex;align-items:center;gap:16px;max-width:none;min-height:64px;padding:0 10px 0 20px}
.cbar .lbl{font:700 15px/1 var(--font-sans);white-space:nowrap}
.cbar .names{flex:1;min-width:0;overflow:hidden;font-size:14px;color:var(--text-2);text-overflow:ellipsis;white-space:nowrap}
.cbar .act{display:flex;align-items:center;gap:8px}
.cbar .tlink{padding:0 10px;text-decoration:none}
@media (max-width:767px){
  .cbar{left:8px;right:8px;width:auto;bottom:calc(var(--tabbar-h) + 8px + env(safe-area-inset-bottom));transform:translateY(calc(100% + 160px))}
  .cbar[data-show="true"]{transform:none}
  .cbar .names{display:none}
  .cbar .wrap{padding:0 8px 0 16px}
}

/* ---------- toast (4s, J1) ---------- */
.toast{position:fixed;left:50%;bottom:calc(24px + env(safe-area-inset-bottom));z-index:var(--z-toast);max-width:min(480px,calc(100% - 32px));padding:12px 18px;border-radius:var(--r-md);
  background:var(--surface-inverse);box-shadow:var(--e3);font:500 14px/1.5 var(--font-sans);transform:translateX(-50%);animation:toast-in var(--d-base) var(--ease-out)}
body:has(#cbar[data-show="true"]) .toast{bottom:calc(96px + env(safe-area-inset-bottom))}
@media (max-width:767px){
  .toast{bottom:calc(var(--tabbar-h) + 16px + env(safe-area-inset-bottom))}
  body:has(#cbar[data-show="true"]) .toast{bottom:calc(var(--tabbar-h) + 88px + env(safe-area-inset-bottom))}
}

/* ---------- overlays: bottom sheet (<1024) / right drawer (≥1024); modal centered ---------- */
.overlay{position:fixed;inset:0;z-index:var(--z-overlay);display:flex;align-items:flex-end;justify-content:center;background:var(--scrim);-webkit-backdrop-filter:blur(4px);backdrop-filter:blur(4px);animation:fade-in var(--d-base) var(--ease-out)}
.overlay.center{z-index:var(--z-modal);align-items:center;padding:24px}
.sheet{display:grid;grid-template-rows:auto minmax(0,1fr) auto;width:min(880px,100%);max-height:92vh;max-height:92dvh;overflow:hidden;border-radius:var(--r-xl) var(--r-xl) 0 0;
  background:var(--surface);color:var(--text-1);box-shadow:var(--e4);animation:sheet-up var(--d-slow) var(--ease-out)}
@media (min-width:1024px){
  .overlay:not(.center){justify-content:flex-end;align-items:stretch}
  .sheet{width:min(600px,100%);height:100%;max-height:none;border-radius:var(--r-xl) 0 0 var(--r-xl);animation-name:drawer-in}
}
.modal{display:grid;grid-template-rows:auto minmax(0,1fr);width:min(1040px,100%);max-height:92vh;max-height:92dvh;overflow:hidden;border-radius:var(--r-xl);background:var(--surface);color:var(--text-1);box-shadow:var(--e4);animation:sheet-up var(--d-slow) var(--ease-out)}
.sheet-h{position:relative;display:flex;align-items:center;justify-content:space-between;gap:16px;min-height:64px;padding:0 20px 0 24px;border-bottom:1px solid var(--line);background:var(--surface)}
.sheet-h .t{overflow:hidden;font:700 16px/1.3 var(--font-sans);text-overflow:ellipsis;white-space:nowrap}
.xbtn{display:inline-flex;align-items:center;justify-content:center;width:44px;height:44px;margin-right:-8px;border-radius:var(--r-pill);background:var(--surface-sunk);color:var(--text-1)}
@media (hover:hover){ .xbtn:hover{background:var(--line)} }
.sheet-b{overflow-y:auto;overscroll-behavior:contain;padding:0 24px 24px}
.sheet-f{display:grid;grid-template-columns:1fr 2fr;gap:8px;padding:12px 24px calc(12px + env(safe-area-inset-bottom));border-top:1px solid var(--line);background:var(--surface);box-shadow:0 -8px 24px -12px rgb(15 74 57 / .16)}
.sheet-f.one{grid-template-columns:1fr}
.sheet-f .btn{--btn-h:52px}
.modal .sheet-b{padding:24px 32px 32px}
@media (max-width:767px){
  .overlay.center{align-items:flex-end;padding:0}
  .modal{border-radius:var(--r-xl) var(--r-xl) 0 0}
  .modal .sheet-b{padding:16px}
  .sheet-h{padding:0 12px 0 16px}
  .sheet-h::before{content:"";position:absolute;left:50%;top:6px;width:36px;height:4px;margin-left:-18px;border-radius:2px;background:var(--line-soft)}  /* decorative grabber; close button stays visible */
  .sheet-b{padding:0 16px 16px}
  .sheet-f{padding:12px 16px calc(12px + env(safe-area-inset-bottom))}
}
```

### 3.3 HOME-CHAT — `@layer sections`

Classes covered:

| Group | Classes |
|---|---|
| Shell | `chat-wrap`, `home-chat`, `chat-grid`, `chat-side`, `chat`, `chat-progress`, and new `chat-mast`, `chat-panel`, `chat-top`, `chat-top-t`, `safety`, `log-toggle` |
| Card | `ccard`, `ccard-bg`, `ccard-dots`, `ccard-grid`, `ccard-h`, `ccard-kind`, `ccard-no`, `ccard-row`, `ccard-wrap` |
| Card fields | `cf`, `cf-body`, `cf-est`, `cf-wide`, `cl`, `cv`, `small`, `fill`, `next` (new) |
| Card widgets | `mini-body`, `dotp`, `has`, `gauge`, `severe`, `tl-dots`, `now`, `seal`, `seal-in`, `seal-wm` |
| Messages and options | `msg`, `me`, `wide`, `av`, `bubble`, `sub`, `opts`, `opt`, `go`, `fly` (new), `typing`, `analyzing` (new) |
| Step mode | `step`, `step-back`, `step-bar`, `step-foot`, `step-h`, `step-n`, `step-q`, `step-sub`, `step-prev` (new) |
| Result | `res`, `res-step`, `sum`, `acts`, and new `res-hero`, `res-title`, `res-kpis`, `res-cta`, `res-nav` |
| Estimate | `est`, `est-big`, `est-chart`, `est-cmp`, `est-h`, `est-k`, `est-mix`, `est-notes`, `est-std`, `est-more` (new), `k`, `b`, `mx`, `ind` |
| Other result blocks | `cov`, `cov-r`, `know`, `warnbox` |

```css
/* ---------- shell ---------- */
.chat-wrap{position:relative}
.home-chat{isolation:isolate;padding:0 0 64px;
  background:radial-gradient(60% 55% at 90% 0%,rgb(127 209 176 / .22),transparent 70%),radial-gradient(40% 45% at 0% 30%,rgb(191 232 214 / .30),transparent 70%),var(--bg)}
.home-chat::before{content:"";position:absolute;inset:0;z-index:-1;pointer-events:none;background-image:radial-gradient(rgb(15 74 57 / .09) 1px,transparent 1.2px);background-size:22px 22px;
  -webkit-mask-image:linear-gradient(180deg,#000,transparent 72%);mask-image:linear-gradient(180deg,#000,transparent 72%)}          /* grid-paper texture */
.chat-mast{position:relative;display:grid;grid-template-columns:minmax(0,7fr) minmax(0,5fr);align-items:end;gap:12px 48px;margin-bottom:32px;padding:40px 0 28px;border-bottom:1px solid var(--line)}
.chat-mast::after{content:"";position:absolute;left:0;bottom:-1px;width:48px;height:2px;background:var(--brand)}
.chat-mast .eyebrow{grid-column:1 / -1}
.chat-mast .display{font-size:var(--t-display-xl);line-height:1.06;letter-spacing:-.035em}
.chat-mast .display .wm{font-size:1.04em}
.chat-mast .lead{max-width:28em;padding-bottom:6px;font-size:17px}
.chat-grid{display:grid;grid-template-columns:minmax(0,5fr) minmax(0,7fr);align-items:start;gap:32px 48px}
.chat-side{position:sticky;top:calc(var(--nav-h) + 24px);z-index:1}
.chat-panel{position:relative;z-index:2;overflow:clip;border:1px solid var(--line);border-radius:var(--r-xl);background:var(--surface);box-shadow:var(--e3)}
.chat-top{display:flex;align-items:center;gap:12px;min-height:60px;padding:8px 12px 8px 20px;border-bottom:1px solid var(--line);border-radius:var(--r-xl) var(--r-xl) 0 0;background:linear-gradient(180deg,var(--surface),var(--surface-tonal-soft))}
.chat-top-t{display:inline-flex;align-items:center;gap:10px;font:700 15px/1 var(--font-sans);color:var(--text-1);white-space:nowrap}
.chat-top .av{width:28px;height:28px;font-size:12px}
.chat-progress{flex:1;display:flex;align-items:center;min-width:48px}
.chat-progress .bar{flex:1;height:6px;overflow:hidden;border-radius:var(--r-pill);background:var(--surface-sunk)}
.chat-progress .bar i{display:block;width:100%;height:100%;border-radius:inherit;background:var(--brand);transform:scaleX(0);transform-origin:0 50%;transition:transform var(--d-slow) var(--ease-out)}  /* J10-C5 drives scaleX */
#chatStep{font:700 14px/1 var(--font-sans);color:var(--text-2);white-space:nowrap}
.chat-top .tlink{min-height:44px;padding:0 8px;font-size:14px}
.safety{position:relative;margin:0;padding:12px 20px 12px 48px;border-bottom:1px solid var(--line);background:var(--warn-bg);font-size:14px;line-height:1.55;color:var(--text-1)}   /* 9.30 */
.safety .i{position:absolute;left:20px;top:13px;width:18px;height:18px;color:var(--warn-fg)}
.log-toggle{display:none;margin:12px 24px 0}
.home-chat.done .log-toggle{display:inline-flex}
.home-chat.done:not(.show-log) #chat > .msg:not(.wide){display:none}      /* Q&A log collapses (class-based; nothing moves in the live region) */
.chat{display:grid;align-content:start;gap:8px;min-height:560px;padding:20px 24px 28px}

/* ---------- messages ---------- */
.av{position:relative;flex:0 0 auto;display:grid;place-items:center;width:32px;height:32px;border-radius:50%;background:var(--brand);color:var(--white);font:700 13px/1 var(--font-sans)}
.av::after{content:"";position:absolute;right:-1px;bottom:-1px;width:9px;height:9px;border:2px solid var(--surface);border-radius:50%;background:var(--mint-400)}
.msg{display:flex;align-items:flex-end;gap:10px;animation:rise var(--d-base) var(--ease-out) both}
.msg .bubble{max-width:min(84%,34em);padding:12px 16px;border-radius:20px 20px 20px 6px;background:var(--surface-sunk);color:var(--text-1);font-size:16px;line-height:1.6}   /* 9.18 */
.msg .bubble .sub{display:block;margin-top:4px;font-size:14px;line-height:1.5;color:var(--text-3)}                                                                        /* 5.25 */
.msg.me{justify-content:flex-end;animation:none}
.msg.me .bubble{border-radius:20px 20px 6px 20px;background:var(--brand);color:var(--white);transform-origin:right bottom;animation:me-in var(--d-slow) var(--ease-spring) both}  /* spring #1 */
.msg.wide{display:block}
.msg.wide .av{display:none}
.msg.wide .bubble{width:100%;max-width:none;padding:4px 0 0;border-radius:0;background:transparent}

/* ---------- answer options ---------- */
.opts{display:flex;flex-wrap:wrap;gap:8px;margin:4px 0 16px 42px}
.opts > *{animation:rise var(--d-base) var(--ease-out) both}
.opts > :nth-child(2){animation-delay:30ms} .opts > :nth-child(3){animation-delay:60ms} .opts > :nth-child(4){animation-delay:90ms}
.opts > :nth-child(5){animation-delay:120ms} .opts > :nth-child(6){animation-delay:150ms} .opts > :nth-child(7){animation-delay:180ms} .opts > :nth-child(n+8){animation-delay:210ms}
.opt{display:inline-flex;align-items:center;gap:8px;min-height:44px;padding:8px 18px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);box-shadow:var(--e1);
  color:var(--text-1);font:500 15px/1.3 var(--font-sans);text-align:left;cursor:pointer;touch-action:manipulation;
  transition:border-color var(--d-fast) var(--ease-std),background-color var(--d-fast) var(--ease-std),transform var(--d-press) var(--ease-out)}
.opt .pic{width:18px;height:18px;color:var(--text-3)}
@media (hover:hover){ .opt:not([aria-pressed="true"]):hover{border-color:var(--brand);background:var(--surface-tonal-soft)} }
.opt:active{transform:scale(.97)}
.opt[aria-pressed="true"]{border-color:var(--brand);background:var(--brand);color:var(--white);animation:pick var(--d-base) var(--ease-spring)}
.opt[aria-pressed="true"] .pic{color:var(--white)}
.opt[aria-pressed="true"]::before{content:"";flex:0 0 auto;width:14px;height:14px;background:currentColor;-webkit-mask:var(--ico-check) center/contain no-repeat;mask:var(--ico-check) center/contain no-repeat}
.opts .go{--btn-h:44px}
.opt.fly{position:fixed;z-index:var(--z-fly);margin:0;border-color:var(--brand);background:var(--brand);color:var(--white);box-shadow:var(--e3);pointer-events:none;transform-origin:0 0;animation:none}  /* J10-C3 clone */

/* ---------- typing (loader — infinite allowed) + 분석 중 beat ---------- */
.typing{display:inline-flex;gap:5px;padding:6px 2px}
.typing i{width:7px;height:7px;border-radius:50%;background:var(--accent-strong);animation:blink 1s infinite}      /* 3.52 on sunk */
.typing i:nth-child(2){animation-delay:.15s} .typing i:nth-child(3){animation-delay:.3s}
.analyzing{display:grid;gap:10px;padding:14px 16px;border-radius:var(--r-md);background:var(--surface-sunk);font-size:15px;color:var(--text-2)}
.analyzing li{display:flex;align-items:center;gap:10px;opacity:.5;transition:opacity var(--d-fast)}
.analyzing li::before{content:"";flex:0 0 auto;width:18px;height:18px;border:1.5px solid var(--line-ui);border-radius:50%;background:transparent center/12px no-repeat}
.analyzing li.ok{opacity:1}
.analyzing li.ok::before{border-color:var(--brand);background-color:var(--brand);background-image:var(--ico-check-white)}

/* ---------- 문진 카드 (hero object) ---------- */
.ccard-wrap{position:relative;isolation:isolate;--p:0}
.ccard-wrap::before{content:"";position:absolute;inset:14px -8px -12px 10px;z-index:-1;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface-sunk);
  transform:rotate(calc(1.2deg + var(--p) * 1.8deg));transform-origin:10% 100%;transition:transform var(--d-slow) var(--ease-out)}   /* back sheet "breathes" with progress (J10-C5) */
.ccard-bg{display:none}
@media (min-width:1280px){
  .ccard-bg{position:absolute;left:50%;top:50%;z-index:-2;display:block;width:160%;max-width:none;aspect-ratio:1;object-fit:cover;opacity:.3;pointer-events:none;transform:translate(-50%,-50%);
    -webkit-mask-image:radial-gradient(circle,transparent 34%,#000 41%,#000 47%,transparent 60%);mask-image:radial-gradient(circle,transparent 34%,#000 41%,#000 47%,transparent 60%)}  /* static — no infinite spin */
}
.ccard{position:relative;padding:20px 20px 22px;border:1px solid var(--line);border-radius:var(--r-lg);background-color:var(--surface-paper);
  background-image:repeating-linear-gradient(180deg,transparent 0 31px,rgb(15 74 57 / .05) 31px 32px),var(--grain);box-shadow:var(--e3);
  transition:border-color var(--d-slow),box-shadow var(--d-slow) var(--ease-out)}
.ccard.done{border-color:var(--brand);box-shadow:var(--e4)}
.ccard-h{display:flex;justify-content:space-between;align-items:baseline;gap:12px;padding-bottom:12px;border-bottom:3px double var(--line-strong)}
.ccard-h .wm{font-size:22px;color:var(--text-strong)}
.ccard-no{font:600 12px/1 var(--font-sans);letter-spacing:.04em;color:var(--text-3)}
.ccard-row{display:flex;justify-content:space-between;align-items:center;gap:12px;min-height:24px;margin:12px 0 6px}
.ccard-dots{display:flex;flex-wrap:wrap;gap:5px}
.ccard-dots i{width:9px;height:9px;border:1.5px solid var(--line-ui);border-radius:50%;background:var(--surface-paper);transition:background-color var(--d-fast),border-color var(--d-fast),box-shadow var(--d-fast)}
.ccard-dots i.on{border-color:var(--brand);background:var(--brand)}
.ccard-dots i.now{border-color:var(--brand);background:var(--mint-400);box-shadow:0 0 0 3px var(--focus-halo)}
.ccard-kind{display:inline-flex;align-items:center;height:24px;padding:0 10px;border:1px solid var(--brand);border-radius:var(--r-pill);font:700 12px/1 var(--font-sans);color:var(--brand);animation:fade-in var(--d-base) var(--ease-out)}
.ccard-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:2px 16px;margin-top:6px}
.cf{position:relative;display:grid;align-content:start;gap:4px;min-height:58px;padding:9px 8px 9px 6px;border-bottom:1px dashed var(--line-soft);border-radius:8px 8px 0 0;transition:background-color var(--d-fast),box-shadow var(--d-fast)}
.cf-body{grid-row:span 3;border-bottom:0}
.cf-wide{grid-column:1 / -1}
.cf .cl{font:600 12px/1.3 var(--font-sans);letter-spacing:.02em;color:var(--text-3)}
.cf .cv{min-height:22px;font:600 16px/1.35 var(--font-sans);color:var(--text-1)}
.cf .cv.small{font-size:14px;font-weight:500;color:var(--text-2)}
.cf .cv.fill{animation:ink var(--d-slow) var(--ease-out) both}                  /* live ink — only the changed field (J10-C1) */
.cf.next{background:rgb(127 209 176 / .16);box-shadow:inset 0 -2px 0 var(--accent-strong)}   /* "작성 중" caret (J10-C2) */
.cf.next .cl{color:var(--text-accent)}
.cf.next .cl::after{content:" · 작성 중";content:" · 작성 중" / ""}
.cf-est{margin-top:6px;padding-right:112px;border-top:1.5px solid var(--line-strong);border-bottom:0;border-radius:0}
.cf-est .cv{font:700 24px/1.1 var(--font-sans);letter-spacing:-.02em;color:var(--brand)}
.cf-est .meta{font-size:13px;line-height:1.45}                                   /* caption, not a sentence */
.mini-body{width:78px;height:106px}
.mini-body svg{display:block;width:100%;height:100%}
.mini-body .sil{fill:var(--sage-300);transition:fill var(--d-slow)}              /* empty state (decor) */
.mini-body.has .sil{fill:var(--text-1)}
.mini-body .dotp{fill:var(--mint-400);stroke:var(--surface-paper);stroke-width:12;opacity:0;transform-origin:center;transform-box:fill-box}
.mini-body .dotp.on{opacity:1;animation:ping var(--d-max) var(--ease-spring)}  /* spring #3 */
.tl-dots{display:flex;align-items:center;gap:6px}
.tl-dots i{position:relative;width:10px;height:10px;border:1.5px solid var(--line-ui);border-radius:50%;background:var(--surface-paper)}
.tl-dots i + i::before{content:"";position:absolute;right:100%;top:50%;width:6px;height:1.5px;margin-top:-.75px;background:var(--line-ui)}
.tl-dots i.on{border-color:var(--brand);background:var(--brand)}
.tl-dots i.on + i.on::before{background:var(--brand)}
.gauge{display:flex;gap:4px}
.gauge i{width:28px;height:10px;border:1px solid var(--line-ui);border-radius:3px;background:var(--surface-sunk);transition:background-color var(--d-base),border-color var(--d-base)}
.gauge i.on{border-color:var(--brand);background:var(--brand)}
.gauge.severe i.on{border-color:var(--warn-mark);background:var(--warn-mark)}     /* 4.94 + text label #cfPainT always shown */
.seal{position:absolute;right:16px;bottom:16px;z-index:2;pointer-events:none}
.seal .seal-in{position:relative;display:grid;place-items:center;align-content:center;width:96px;height:96px;border:2.5px solid var(--brand);border-radius:18px;outline:1px solid var(--brand);outline-offset:-8px;
  background:rgb(255 253 248 / .86);color:var(--brand);font-family:var(--font-sans);line-height:1;mix-blend-mode:multiply;transform:rotate(-8deg);animation:stamp var(--d-max) var(--ease-spring) both}  /* spring #2 */
.seal .seal-wm{display:block;font-family:var(--font-brand);font-size:24px;letter-spacing:-.05em}
.seal .seal-in b{font-weight:900}
.seal .seal-in i{font-style:normal;font-weight:300}
.seal .seal-in small{display:block;margin-top:6px;font:700 12px/1 var(--font-sans);letter-spacing:.04em}   /* was 9px */
.seal .seal-in::after{content:"";position:absolute;inset:-6px;border-radius:22px;animation:bleed 480ms var(--ease-out) 280ms both}
@media (min-width:1024px) and (max-height:820px){ .cf{min-height:50px;padding-block:6px} .mini-body{width:60px;height:82px} .ccard{padding:16px 16px 18px} }

/* ---------- ≤1023: sticky peek strip until done; full static card after done ---------- */
@media (max-width:1023px){
  .chat-mast{grid-template-columns:minmax(0,1fr);padding:28px 0 20px;margin-bottom:16px}
  .chat-grid{display:flex;flex-direction:column;gap:12px}
  .home-chat .chat-side{position:sticky;top:var(--nav-h);z-index:var(--z-sticky);margin:0 calc(-1 * var(--gutter));padding:8px var(--gutter);background:rgb(244 246 241 / .92);-webkit-backdrop-filter:blur(10px);backdrop-filter:blur(10px)}
  .home-chat.done .chat-side{position:static;margin:0;padding:0;background:none;-webkit-backdrop-filter:none;backdrop-filter:none}
  .home-chat:not(.done) .ccard-wrap::before,.home-chat:not(.done) .ccard-bg{display:none}
  .home-chat:not(.done) .ccard{display:flex;align-items:center;gap:12px;min-height:56px;padding:8px 14px;border-radius:var(--r-md);box-shadow:var(--e1);background-image:none}
  .home-chat:not(.done) .ccard-h,.home-chat:not(.done) .ccard-kind,.home-chat:not(.done) .ccard-grid > .cf:not(.cf-body),.home-chat:not(.done) .cf .cl,.home-chat:not(.done) #cfPainT{display:none}
  .home-chat:not(.done) .ccard-grid{display:contents}
  .home-chat:not(.done) .cf-body{display:flex;align-items:center;gap:8px;min-height:0;padding:0;border:0;background:none;box-shadow:none}
  .home-chat:not(.done) .cf-body .cv{min-height:0;font-size:14px}
  .home-chat:not(.done) .mini-body{width:30px;height:40px}
  .home-chat:not(.done) .ccard-row{order:9;margin:0 0 0 auto}
  .home-chat:not(.done) .ccard-dots{max-width:112px;justify-content:flex-end;gap:4px}
  .home-chat:not(.done) .ccard-dots i{width:7px;height:7px}
}
@media (max-width:1023px){   /* separate rule: if :has() is unsupported only the gauge is lost */
  .home-chat:not(.done) .cf:has(> #cfPain){display:flex;align-items:center;min-height:0;padding:0;border:0;background:none;box-shadow:none}
}

/* ---------- ≤767: step mode (one question per screen) ---------- */
@media (max-width:767px){
  .home-chat{padding:0 0 40px}
  .home-chat::before{display:none}
  .chat-mast{gap:8px;padding:20px 0 14px;margin-bottom:10px}
  .chat-mast .lead{display:none}                  /* h1 stays: mobile has an h1 again */
  .chat-mast .display{line-height:1.18;letter-spacing:-.03em}
  .chat-panel{border-radius:var(--r-lg);box-shadow:var(--e2)}
  .chat-top,.safety,.log-toggle,.home-chat.done .log-toggle{display:none}      /* step header + step-foot replace them */
  .chat{min-height:0;padding:16px}
  .msg .bubble{max-width:90%}
  .opts{margin-left:0}
}
.step{display:grid;align-content:start;gap:16px;min-height:min(560px,calc(100vh - var(--nav-h) - var(--tabbar-h) - 140px));min-height:min(560px,calc(100dvh - var(--nav-h) - var(--tabbar-h) - 140px));animation:step-in var(--d-base) var(--ease-out)}
.step[data-dir="back"]{animation-name:step-back}                                 /* direction reverses on back (J10-C8/C9) */
.step-h{display:grid;grid-template-columns:44px minmax(0,1fr) auto;align-items:center;gap:8px}
.step-back{display:inline-flex;align-items:center;justify-content:center;width:44px;height:44px;border-radius:50%;background:var(--surface-sunk);color:var(--text-1)}
.step-back .i{width:18px;height:18px}
.step-n{font:700 14px/1 var(--font-sans);color:var(--text-2)}
.step-h .wm{font-size:18px;color:var(--text-strong)}
.step-bar{height:4px;overflow:hidden;border-radius:2px;background:var(--surface-sunk)}
.step-bar i{display:block;height:100%;border-radius:inherit;background:var(--brand)}
.step-q{margin-top:4px;font:600 26px/1.28 var(--font-sans);letter-spacing:-.025em;color:var(--text-strong);outline:none}
.step-sub{margin-top:-8px;font-size:15px;line-height:1.55;color:var(--text-3)}
.step .opts{display:grid;grid-template-columns:minmax(0,1fr);gap:10px;margin:4px 0 0}
.step .opts > *{animation:none}
.step .opt{width:100%;min-height:60px;padding:12px 14px;justify-content:flex-start;gap:14px;border-width:1.5px;border-radius:var(--r-md);box-shadow:none;font:500 17px/1.35 var(--font-sans);white-space:normal}
.step .opt .pic{width:20px;height:20px;padding:8px;box-sizing:content-box;border-radius:var(--r-sm);background:var(--surface-tonal);color:var(--brand)}
.step .opt[aria-pressed="true"] .pic{background:rgb(255 255 255 / .16);color:var(--white)}
.step .opt:active{background:var(--surface-sunk)}
.step .opts .go{--btn-h:56px;width:100%;margin-top:6px;border-radius:var(--r-md);font-size:17px}
.step .opts[data-id="part"],.step .opts[data-id="city"],.step .opts[data-id="dist"],.step .opts[data-id="cond"]{grid-template-columns:repeat(2,minmax(0,1fr))}
.step .opts[data-id="part"] .opt{min-height:76px;flex-direction:column;justify-content:center;gap:6px;text-align:center}
.step .opts[data-id="cond"] .go,.step .opts[data-id="dist"] .opt:first-child{grid-column:1 / -1}
.step-prev{justify-self:start}                                                   /* bottom "이전 질문" within thumb reach */
.step-foot{position:relative;margin-top:auto;padding:12px 14px 12px 40px;border-radius:var(--r-md);background:var(--warn-bg);font-size:14px;line-height:1.55;color:var(--text-1)}
.step-foot::before{content:"";position:absolute;left:14px;top:14px;width:16px;height:16px;background:var(--warn-fg);-webkit-mask:var(--ico-alert) center/contain no-repeat;mask:var(--ico-alert) center/contain no-repeat}
.res-step .res{margin-top:4px}
.res-step .res .acts{flex-direction:column;align-items:stretch}
.res-step .res .acts .btn{--btn-h:52px;width:100%}
.res-step .res .acts .tlink{align-self:center}
.res-step .est{padding:18px 16px}

/* ---------- result ---------- */
.res{display:grid;gap:16px}
.res > [id^="res-"]:not(.est){padding-top:20px;border-top:1px solid var(--line)}
.res h3,.res h4{display:flex;align-items:center;gap:8px;margin:0 0 12px;font:700 16px/1.4 var(--font-sans);color:var(--text-1)}
.res > div > h3::before,.est-h h3::before{content:"";flex:0 0 auto;width:4px;height:16px;border-radius:2px;background:var(--accent-strong)}
.res-hero{display:grid;gap:12px;padding:24px;border:1px solid var(--mint-200);border-radius:var(--r-lg);background:linear-gradient(180deg,var(--surface-tonal-soft),var(--surface) 70%)}
.res-title{font:600 clamp(24px,1.2vw + 16px,30px)/1.2 var(--font-sans);letter-spacing:-.025em;color:var(--text-strong);outline:none}
.res .sum{display:flex;flex-wrap:wrap;gap:6px}
.res-kpis{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:8px;margin-top:4px}
.res-kpis > div{display:grid;align-content:start;gap:8px;padding:14px 16px;border:1px solid var(--line);border-radius:var(--r-md);background:var(--surface)}
.res-kpis dt{display:flex;flex-wrap:wrap;align-items:center;gap:6px;font:600 13px/1.3 var(--font-sans);color:var(--text-3)}
.res-kpis dd{font:700 var(--t-kpi-sm)/1 var(--font-sans);letter-spacing:-.03em;color:var(--brand)}
.res-kpis dd small{margin-left:3px;font:600 15px/1 var(--font-sans);letter-spacing:0;color:var(--text-2)}
.res-cta{display:flex;flex-wrap:wrap;align-items:center;gap:8px 16px;margin-top:4px}
.res-nav{position:sticky;top:calc(var(--nav-h) + 12px);z-index:var(--z-sticky);display:flex;gap:4px;width:max-content;max-width:100%;overflow-x:auto;scrollbar-width:none;padding:4px;
  border:1px solid var(--line);border-radius:var(--r-pill);background:rgb(255 255 255 / .92);-webkit-backdrop-filter:blur(8px);backdrop-filter:blur(8px);box-shadow:var(--e1)}
.res-nav::-webkit-scrollbar{display:none}
.res-nav a{display:inline-flex;align-items:center;min-height:40px;padding:0 14px;border-radius:var(--r-pill);font:600 14px/1 var(--font-sans);color:var(--text-2);white-space:nowrap}
@media (pointer:coarse){ .res-nav a{min-height:44px} }
@media (hover:hover){ .res-nav a:hover{background:var(--surface-sunk);color:var(--text-1)} }
@media (max-width:767px){                       /* no sticky chrome on phones: nav 56 + tabbar 56 only */
  .res-nav{position:static;width:auto}
  .res-kpis{grid-template-columns:repeat(2,minmax(0,1fr))}
  .res-kpis > div:first-child{grid-column:1 / -1}
  .res-cta .btn{width:100%}
  .res-hero{padding:18px 16px}
}
/* estimate */
.est{display:grid;gap:18px;padding:24px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface-paper);box-shadow:var(--e1)}
.est .body{font-size:15px}
.est-h{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:baseline;gap:6px 12px}
.est-h h3,.est-h h4{margin:0}
.est-big{display:flex;flex-wrap:wrap;align-items:baseline;gap:6px 10px}
.est-big .num{font:700 var(--t-kpi)/1 var(--font-sans);letter-spacing:-.03em;color:var(--brand)}
.est-big small{font:600 18px/1 var(--font-sans);color:var(--text-2)}
.est-big .est-k{flex-basis:100%;font-size:14px;line-height:1.5;color:var(--text-2)}
.est-std{display:grid;grid-template-columns:auto auto minmax(0,1fr);align-items:baseline;gap:4px 12px;padding:14px 16px;border:1px solid var(--mint-200);border-radius:var(--r-md);background:var(--surface-tonal-soft)}
.est-std .k{font:600 13px/1.3 var(--font-sans);color:var(--text-accent)}
.est-std .v{font:700 20px/1.2 var(--font-sans);color:var(--text-strong)}
.est-std .d{grid-column:1 / -1;font-size:14px;line-height:1.6;color:var(--text-2)}
.est-chart .rrow{grid-template-columns:128px minmax(0,1fr) 88px;gap:12px;padding:8px 0}
.est-chart .rrow .lbl,.est-chart .rrow .val{font-size:13px;color:var(--text-2)}
.est-chart .rtrack{height:20px}
.est-chart .rbar{top:7px;height:6px;background:var(--chart-context)}              /* context bars 3.48–3.94 */
.est-chart .rbar::before,.est-chart .rbar::after{content:none}
.est-chart .rrow.hit{margin:0 -10px;padding:8px 10px;border-bottom-color:transparent;border-radius:var(--r-sm);background:var(--surface-tonal-soft)}
.est-chart .rrow.hit .rbar{top:5px;height:10px;background:var(--chart-bar)}
.est-chart .rrow.hit .lbl,.est-chart .rrow.hit .val{font-weight:700;color:var(--text-strong)}
.est-chart .raxis{grid-template-columns:128px minmax(0,1fr) 88px;gap:12px}
.rmark{position:absolute;top:2px;height:16px;border-radius:6px;background:var(--chart-mine);box-shadow:0 0 0 2px var(--forest-900)}   /* ring ≥11:1 carries the mark */
details.est-more{border-top:1px solid var(--line)}
details.est-more > summary{position:relative;display:flex;flex-wrap:wrap;align-items:center;gap:4px 12px;min-height:52px;padding-right:28px;list-style:none;cursor:pointer;font:600 15px/1.3 var(--font-sans);color:var(--text-1)}
details.est-more > summary::-webkit-details-marker{display:none}
details.est-more > summary .meta{font-weight:400}
details.est-more > summary::after{content:"";position:absolute;right:4px;top:50%;width:16px;height:16px;margin-top:-8px;background:var(--text-3);-webkit-mask:var(--ico-chev) center/contain no-repeat;mask:var(--ico-chev) center/contain no-repeat;transform:rotate(90deg);transition:transform var(--d-base) var(--ease-std)}
details.est-more[open] > summary::after{transform:rotate(-90deg)}
details.est-more > :not(summary){margin-bottom:16px}
.est-cmp{display:grid;gap:4px;padding:16px;border-radius:var(--r-md);background:var(--surface-sunk)}
.est-cmp h5{margin:0 0 6px;font:700 13px/1.4 var(--font-sans);color:var(--text-2)}
.est-cmp .ind{display:grid;grid-template-columns:148px minmax(0,1fr) 136px;align-items:center;gap:12px;padding:8px 0}
.est-cmp .ind .lbl{display:grid;font:600 13px/1.35 var(--font-sans);color:var(--text-1)}
.est-cmp .ind .lbl small,.est-cmp .ind .val small{font:400 12px/1.4 var(--font-sans);color:var(--text-3)}
.est-cmp .ind .rtrack{height:14px;border-radius:var(--r-pill);background:var(--surface);background-image:none}
.est-cmp .ind .rbar{top:3px;height:8px}
.est-cmp .ind .val{display:grid;text-align:right;font:700 13px/1.35 var(--font-sans);color:var(--text-1)}   /* "평균보다 낮음/높음" text = not colour-only */
.est-cmp .meta{margin-top:6px}
.est-mix{display:grid;gap:6px}
.est-mix .mx{display:grid;grid-template-columns:132px minmax(0,1fr) 104px;align-items:center;gap:12px;font-size:13px}
.est-mix .k{display:grid;font-weight:600;color:var(--text-2)}
.est-mix .k small{font-size:12px;font-weight:400;color:var(--text-3)}
.est-mix .b{height:10px;overflow:hidden;border-radius:var(--r-pill);background:var(--chart-track)}
.est-mix .b i{display:block;height:100%;border-radius:inherit}
.est-mix .v{text-align:right;font-weight:600;color:var(--text-1)}
.est-mix .v small{margin-left:2px;font-weight:400;color:var(--text-3)}
.est-notes{display:grid;gap:8px}
.est-notes li{position:relative;padding-left:16px;font-size:15px;line-height:1.6;color:var(--text-2)}
.est-notes li::before{content:"";position:absolute;left:0;top:.62em;width:6px;height:6px;border-radius:50%;background:var(--accent-strong)}
.est-notes b{font-weight:600;color:var(--text-1)}
/* general-pain cost, tips, warning, actions */
.cov{display:grid}
.cov-r{display:grid;grid-template-columns:auto minmax(0,1fr);align-items:center;gap:4px 12px;padding:12px 0;border-bottom:1px dashed var(--line-soft);font-size:15px;color:var(--text-2)}
.cov-r:last-child{border-bottom:0}
.cov-r b{font-weight:600;color:var(--text-1)}
.cov-r > span:last-child{grid-column:2;font-size:14px}
.res .know{display:grid;counter-reset:k}
.res .know li{position:relative;padding:12px 0 12px 36px;border-bottom:1px solid var(--line);font-size:15px;line-height:1.6;color:var(--text-2)}
.res .know li:last-child{border-bottom:0}
.res .know li::before{content:counter(k);counter-increment:k;position:absolute;left:0;top:12px;display:grid;place-items:center;width:24px;height:24px;border-radius:50%;background:var(--brand);color:var(--white);font:700 12px/1 var(--font-sans)}
.res .know b{font-weight:600;color:var(--text-1)}
.res .warnbox{position:relative;padding:14px 16px 14px 44px;border-left:3px solid var(--warn-mark);border-radius:0 var(--r-md) var(--r-md) 0;background:var(--warn-bg);font-size:15px;line-height:1.6;color:var(--text-1)}   /* amber, not red */
.res .warnbox::before{content:"";position:absolute;left:16px;top:16px;width:18px;height:18px;background:var(--warn-fg);-webkit-mask:var(--ico-alert) center/contain no-repeat;mask:var(--ico-alert) center/contain no-repeat}
.res .warnbox b{font-weight:700;color:var(--warn-fg)}
.res .acts{display:flex;flex-wrap:wrap;align-items:center;gap:8px 12px;padding-top:4px}
@media (max-width:767px){
  .est{padding:18px 16px}
  .est-chart .rrow,.est-cmp .ind,.est-mix .mx{grid-template-columns:minmax(0,1fr) auto;gap:4px 12px}
  .est-chart .rrow .rtrack,.est-cmp .ind .rtrack,.est-mix .mx .b{grid-column:1 / -1;grid-row:2}
  .est-chart .raxis{grid-template-columns:minmax(0,1fr)}
  .est-std{grid-template-columns:minmax(0,1fr)}
}
```

#### Novel interactions (all reduced-motion safe, transform, opacity or clip-path only, content in the DOM from the start)

| # | Moment | CSS | JS | Reduced motion |
|---|---|---|---|---|
| 1 | **Answer fly-in** (chat mode). The chosen chip shrinks along a 320ms path into its card field. | `.opt.fly` | J10-C3 `flyTo()` | Skipped |
| 2 | **Live ink.** Only the changed field writes itself in, left to right over 320ms. | `.cf .cv.fill` + `@keyframes ink` | J10-C1 `setCv` guard | Instant |
| 3 | **"· 작성 중" caret** on the field the current question will fill. | `.cf.next` | J10-C2 | Static |
| 4 | **Paper stack breathes.** The back sheet rotates from 1.2° to 3.0° as progress runs from 0 to 1. | `.ccard-wrap::before` + `--p` | J10-C5 | Static at the final angle |
| 5 | **Mini-body ping.** A 400ms spring, also visible in the mobile peek strip. | `.dotp.on` | existing | Instant |
| 6 | **분석 중 beat.** A 720ms three-line checklist ticks, replacing the typing dots before the result. | `.analyzing` | J10-C13 | Ticks immediately |
| 7 | **도장 stamp.** 400ms spring plus a 480ms ink bleed; the card lifts to e4. | `.seal-in`, `.ccard.done` | existing | Final state |
| 8 | **Range narrows.** The mint "내 구간" marker tightens from the full reference bar to lo–hi over 420ms (transform only). | `.rmark` | J10-C12 `narrow()` | Skipped |
| 9 | **Highlighter sweep and charts arrive once.** | §3.8 | J14 | Static |

A spring easing is used only on #1 (the `.me` bubble), #5 and #7.

### 3.4 HOME-SECTIONS — `@layer sections`

Classes covered:

| Group | Classes |
|---|---|
| Hero | `hero`, `kv-hero`, `hero-grid`, `hero-text`, `hero-cta`, `search`, `colophon`, `kv`, `orbit`, `oc`, `core` |
| Trust strip (new) | `trust`, `trust-in`, `trust-list`, `legend-honest` |
| Glance | `glance` |
| Body selector | `body-grid`, `figure`, `sil`, `hs`, `dot`, `ring`, `partlist`, `bres`, `h`, `sym`, `warn`, `cta`, `tchips` |
| Program | `program`, `index`, `idx-row`, `n`, `nm`, `one`, `st`, `rail`, `panel`, `p-head`, `p-close`, `p-desc`, `p-caut`, `p-sub`, `p-spec`, `spec-inner`, `p-cta`, `p-clin`, `hd`, `grp`, `crow`, `tel`, `act`, `more-row`, `more-rows`, `p-note`, `empty-note`, `p-faq`, `p-disc` |
| Data room and tips | `data-grid`, `minis`, `mini`, `mbar`, `tl`, `w`, `tips`, `slab`, `big`, `l`, `d`, `tips-grid`, `tips-side`, `stack`, `chart-table` (new) |
| Steps, why and find band | `steps`, `why`, `side-note`, `find-band` (new), `band-ink`, `find-cta` (new) |

```css
/* ---------- trust strip (directly under the 문진) ---------- */
.trust{background:var(--surface);border-block:1px solid var(--line)}
.trust-in{display:grid;gap:14px;padding-block:22px}
.trust-list{display:grid;grid-template-columns:repeat(4,minmax(0,1fr))}
.trust-list li{display:grid;grid-template-columns:40px minmax(0,1fr);align-items:start;gap:12px;padding:2px 24px;border-left:1px solid var(--line)}
.trust-list li:first-child{padding-left:0;border-left:0}
.trust-list .pic{width:22px;height:22px;padding:9px;box-sizing:content-box;border-radius:var(--r-sm);background:var(--surface-tonal);color:var(--brand)}
.trust-list b{display:block;font:700 15px/1.4 var(--font-sans);color:var(--text-1)}
.trust-list li span{display:block;margin-top:2px;font-size:13px;line-height:1.5;color:var(--text-3)}
.legend-honest{display:flex;flex-wrap:wrap;align-items:center;gap:6px 8px;padding-top:12px;border-top:1px dashed var(--line);font-size:13px;color:var(--text-3)}
.legend-honest .lbl{margin-right:6px;font-weight:700;color:var(--text-2)}
.legend-honest .tag{margin-left:10px}
.legend-honest .lbl + .tag{margin-left:0}
@media (max-width:1023px){ .trust-list{grid-template-columns:repeat(2,minmax(0,1fr));gap:16px 0} .trust-list li:nth-child(3){padding-left:0;border-left:0} }
@media (max-width:767px){ .trust-list{grid-template-columns:minmax(0,1fr);gap:14px} .trust-list li{padding:0;border-left:0} }

/* ---------- key-visual hero: white stage card on canvas ---------- */
.hero.kv-hero{padding:40px 0 24px;color:var(--text-1)}
.hero-grid{display:grid;grid-template-columns:minmax(0,1.1fr) minmax(0,.9fr);align-items:center;gap:32px 56px;padding:56px;border:1px solid var(--line);border-radius:var(--r-xl);background:var(--surface);box-shadow:var(--e2)}
.hero-text{min-width:0}
.kv-hero .display{margin-top:12px;font-size:var(--t-display-2)}
.kv-hero .lead{margin-top:16px}
.search{display:flex;align-items:center;gap:12px;max-width:640px;min-height:56px;margin-top:28px;padding:0 6px 0 20px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);box-shadow:var(--e1);color:var(--text-1)}
.search:focus-within{border-color:var(--brand);outline:var(--focus-w) solid var(--focus);outline-offset:2px}
.search .i{color:var(--text-3)}
.search input{flex:1;min-width:0;min-height:48px;border:0;background:transparent;font-size:16px;color:var(--text-1);outline:none}
.search input::placeholder{color:var(--text-3)}
.search .btn{--btn-h:44px}
.kv-hero .chips{max-width:720px;margin-top:16px}
.hero-cta{display:flex;flex-wrap:wrap;align-items:center;gap:8px 20px;margin-top:24px}
.colophon{margin-top:28px;padding-top:14px;border-top:1px solid var(--line);font-size:13px;line-height:1.5;color:var(--text-3)}
.kv{position:relative;width:100%;max-width:520px;aspect-ratio:1;margin:0 auto}
.kv::before{content:"";position:absolute;inset:7%;border:1px dashed var(--line-soft);border-radius:50%;pointer-events:none}    /* editorial orbit line */
.kv img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover;-webkit-mask-image:radial-gradient(circle at 50% 50%,#000 58%,transparent 78%);mask-image:radial-gradient(circle at 50% 50%,#000 58%,transparent 78%)}
.orbit{position:absolute;inset:0}
.orbit .oc{position:absolute;display:inline-flex;align-items:center;gap:8px;min-height:40px;padding:0 14px 0 6px;border:1px solid var(--line);border-radius:var(--r-pill);background:var(--surface);box-shadow:var(--e2);
  color:var(--text-1);font:600 14px/1 var(--font-sans);white-space:nowrap;transform:translate(-50%,-50%);
  transition:transform var(--d-fast) var(--ease-out),border-color var(--d-fast),opacity var(--d-slow) var(--ease-out),scale var(--d-slow) var(--ease-spring)}
.orbit .oc .pic{width:16px;height:16px;padding:6px;box-sizing:content-box;border-radius:50%;background:var(--surface-tonal);color:var(--brand)}
.orbit .oc .st{margin-left:2px;font:600 12px/1 var(--font-sans);color:var(--text-accent)}      /* was 10px */
@media (pointer:coarse){ .orbit .oc{min-height:44px} }
@media (hover:hover){ .orbit .oc:hover{border-color:var(--brand);transform:translate(-50%,calc(-50% - 2px))} }
.kv .core{position:absolute;left:50%;top:50%;font-size:22px;color:var(--text-strong);white-space:nowrap;transform:translate(-50%,56px)}
@media (max-width:1023px){ .hero-grid{grid-template-columns:minmax(0,1fr);padding:40px 32px} .kv{max-width:440px} }
@media (max-width:767px){
  .hero.kv-hero{padding:24px 0 8px}
  .hero-grid{padding:28px 20px;border-radius:var(--r-lg)}
  .kv-hero .chips{flex-wrap:nowrap;overflow-x:auto;scrollbar-width:none;margin-inline:-20px;padding:2px 20px;scroll-snap-type:x proximity;
    -webkit-mask-image:linear-gradient(90deg,transparent 0,#000 16px,#000 calc(100% - 32px),transparent);mask-image:linear-gradient(90deg,transparent 0,#000 16px,#000 calc(100% - 32px),transparent)}  /* swipe hint */
  .kv-hero .chips::-webkit-scrollbar{display:none}
  .kv-hero .chips .chip{flex:0 0 auto;scroll-snap-align:start}
  .hero-cta .btn{width:100%}
  .kv{aspect-ratio:auto;max-width:none;margin-top:8px}
  .kv::before,.kv .core{display:none}
  .kv img{position:relative;inset:auto;width:min(280px,76vw);height:auto;aspect-ratio:1;margin:0 auto}
  .orbit{position:static;display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:8px;margin-top:12px}     /* no scale() hack: 48px targets, 14/12px text */
  .orbit .oc,.orbit .oc:hover{position:static;justify-content:flex-start;min-height:48px;padding:6px 12px 6px 6px;white-space:normal;transform:none}
}

/* ---------- 한눈에 보기: 5 service tiles ---------- */
.glance{display:grid;grid-template-columns:repeat(5,minmax(0,1fr));gap:12px}
.glance li{display:flex}
.glance a{flex:1;display:grid;grid-template-columns:minmax(0,1fr) auto;grid-template-areas:"pic i" "nm nm" "one one" "st st";align-content:start;gap:10px;padding:20px;
  border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1);color:var(--text-1);
  transition:border-color var(--d-fast),box-shadow var(--d-base) var(--ease-std),transform var(--d-base) var(--ease-std)}
@media (hover:hover){ .glance a:hover{border-color:var(--line-ui);box-shadow:var(--e2);transform:translateY(-2px)} .glance a:hover > .i{color:var(--brand);transform:translateX(3px)} }
.glance a:active{transform:scale(.98)}
.glance .pic{grid-area:pic;width:22px;height:22px;padding:10px;box-sizing:content-box;border-radius:var(--r-sm);background:var(--surface-tonal);color:var(--brand)}
.glance .nm{grid-area:nm;margin-top:6px;font:600 18px/1.3 var(--font-sans);letter-spacing:-.01em;color:var(--text-strong)}
.glance .one{grid-area:one;font-size:14px;line-height:1.6;color:var(--text-2)}
.glance .st{grid-area:st;align-self:end;padding-top:10px;border-top:1px dashed var(--line);font:600 13px/1.4 var(--font-sans);color:var(--text-accent)}
.glance a > .i{grid-area:i;width:18px;height:18px;color:var(--text-3);transition:transform var(--d-fast) var(--ease-out),color var(--d-fast)}
@media (max-width:1023px){ .glance{grid-template-columns:repeat(2,minmax(0,1fr))} }
@media (max-width:767px){
  .glance{grid-template-columns:minmax(0,1fr)}
  .glance a{grid-template-columns:auto minmax(0,1fr) auto;grid-template-areas:"pic nm i" "pic one i" "pic st i";column-gap:14px;padding:16px}
  .glance .nm{margin-top:0}
  .glance .st{padding-top:0;border-top:0}
  .glance a > .i{align-self:center}
}

/* ---------- 부위 선택 (white band) ---------- */
.body-grid{display:grid;grid-template-columns:minmax(0,5fr) minmax(0,7fr);align-items:start;gap:32px 56px}
.figure{display:grid;grid-template-columns:auto minmax(0,1fr);align-items:start;gap:24px;padding:28px;border:1px solid var(--line);border-radius:var(--r-xl);background:radial-gradient(circle at 42% 38%,var(--mint-50),var(--surface-sunk) 72%)}
.figure svg{display:block;width:100%;max-width:260px;height:auto;margin:0 auto}
.figure .sil{fill:var(--text-1)}
.figure .hs{cursor:pointer;outline:none}
.figure .hs .dot{fill:var(--mint-400);fill-opacity:.8;stroke:var(--mint-50);stroke-width:10;transition:fill-opacity var(--d-fast),stroke var(--d-fast),stroke-width var(--d-fast)}   /* rest 4.32 vs silhouette */
.figure .hs .ring{fill:transparent;stroke:transparent;stroke-width:12;transition:stroke var(--d-fast)}   /* r=100 hit area (M13) */
@media (hover:hover){ .figure .hs:hover .dot{fill-opacity:1} }
.figure .hs[aria-pressed="true"] .dot,.figure .hs:focus-visible .dot{fill-opacity:1;stroke:var(--white);stroke-width:14}        /* fill + white halo change on both */
.figure .hs[aria-pressed="true"] .ring{stroke:var(--forest-800)}                                                             /* selected: solid dark ring */
.figure .hs:focus-visible .ring{stroke:var(--forest-900);stroke-width:14;stroke-dasharray:22 14}                              /* focus: dashed dark ring ≠ selected */
.partlist{display:grid;gap:6px}
.partlist button{display:grid;grid-template-columns:22px minmax(0,1fr);align-items:center;gap:10px;min-height:44px;padding:8px 12px;border:1px solid var(--line);border-radius:var(--r-sm);background:var(--surface);
  text-align:left;font:500 15px/1.3 var(--font-sans);color:var(--text-1);transition:border-color var(--d-fast),background-color var(--d-fast)}
.partlist button .pic{width:18px;height:18px;color:var(--brand)}
@media (hover:hover){ .partlist button:not([aria-pressed="true"]):hover{border-color:var(--line-ui)} }
.partlist button[aria-pressed="true"]{border-color:var(--brand);background:var(--brand);color:var(--white);font-weight:600}
.partlist button[aria-pressed="true"] .pic{color:var(--white)}
.bres{padding:28px;border:1px solid var(--line);border-radius:var(--r-xl);background:var(--bg);animation:fade-up var(--d-base) var(--ease-out)}
.bres .h{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:baseline;gap:8px 12px;margin-bottom:10px}
.bres h3{font:600 28px/1.15 var(--font-sans);letter-spacing:-.025em;color:var(--text-strong)}
.bres .sym{max-width:34em;margin:0 0 20px;font-size:16px;line-height:1.7;color:var(--text-2)}
.bres dl.spec{margin-top:20px}
.bres .warn{position:relative;margin-top:16px;padding:12px 14px 12px 42px;border-radius:var(--r-md);background:var(--warn-bg);font-size:14px;line-height:1.6;color:var(--text-1)}
.bres .warn::before{content:"";position:absolute;left:14px;top:14px;width:16px;height:16px;background:var(--warn-fg);-webkit-mask:var(--ico-alert) center/contain no-repeat;mask:var(--ico-alert) center/contain no-repeat}
.bres .cta{display:flex;flex-wrap:wrap;align-items:center;gap:8px 20px;margin-top:24px}
.tchips{display:flex;flex-wrap:wrap;gap:8px}
.tchips a{display:inline-flex;align-items:center;gap:8px;min-height:44px;padding:6px 16px 6px 8px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);
  color:var(--text-1);font:600 14px/1.25 var(--font-sans);transition:border-color var(--d-fast),box-shadow var(--d-fast)}
.tchips a .pic{width:18px;height:18px;padding:5px;box-sizing:content-box;border-radius:50%;background:var(--surface-tonal);color:var(--brand)}
.tchips a .st{font:600 12px/1 var(--font-sans);color:var(--text-accent)}
.tchips a.ask{border-style:dashed}
.tchips a.ask .st{color:var(--pending-fg)}
@media (hover:hover){ .tchips a:hover{border-color:var(--brand);box-shadow:var(--e1)} }
@media (max-width:1023px){ .body-grid{grid-template-columns:minmax(0,1fr)} }
@media (max-width:767px){ .figure{grid-template-columns:minmax(0,1fr);padding:18px} .figure svg{max-width:220px} .partlist{grid-template-columns:repeat(2,minmax(0,1fr))} .bres{padding:20px 16px} }

/* ---------- 치료 프로그램: 4×2 tiles → rail + panel ---------- */
.program{position:relative}
.index{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:16px}
.index li{display:flex}
.idx-row{width:100%;display:grid;grid-template-columns:minmax(0,1fr) auto;grid-template-areas:"nm n" "one one" "st i";align-content:start;gap:12px;min-height:220px;padding:20px 20px 18px;
  border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1);color:var(--text-1);text-align:left;
  transition:border-color var(--d-fast),box-shadow var(--d-base) var(--ease-std),transform var(--d-base) var(--ease-std)}
@media (hover:hover){ .idx-row:hover{border-color:var(--line-ui);box-shadow:var(--e2);transform:translateY(-2px)} .idx-row:hover > .i{color:var(--brand);transform:rotate(-90deg) translateY(3px)} }
.idx-row:active{transform:scale(.98)}
.idx-row .n{grid-area:n;font:300 28px/1 var(--font-sans);letter-spacing:-.02em;color:var(--text-3);font-variant-numeric:tabular-nums}    /* thin numeral inside tile (allowed) */
.idx-row .nm{grid-area:nm;display:grid;justify-items:start;gap:14px;font:600 19px/1.3 var(--font-sans);letter-spacing:-.01em;color:var(--text-strong)}
.idx-row .nm .pic{width:22px;height:22px;padding:10px;box-sizing:content-box;border-radius:var(--r-sm);background:var(--surface-tonal);color:var(--brand)}
.idx-row .one{grid-area:one;display:-webkit-box;overflow:hidden;font-size:14px;line-height:1.6;color:var(--text-2);-webkit-line-clamp:3;-webkit-box-orient:vertical}
.idx-row .st{grid-area:st;align-self:end;display:grid;gap:2px;padding-top:12px;border-top:1px dashed var(--line);font-size:13px;color:var(--text-3)}
.idx-row .st b{font-weight:700;color:var(--text-accent)}
.idx-row > .i{grid-area:i;align-self:end;justify-self:end;width:18px;height:18px;color:var(--text-3);transform:rotate(-90deg);transition:transform var(--d-fast) var(--ease-out),color var(--d-fast)}
#catOpen{display:grid;gap:16px}
.rail{display:grid;grid-template-columns:repeat(8,minmax(0,1fr));gap:6px}
.rail button{position:relative;display:grid;align-content:start;gap:4px;min-height:76px;padding:12px;border:1px solid var(--line);border-radius:var(--r-md);background:var(--surface);color:var(--text-1);text-align:left;transition:border-color var(--d-fast),background-color var(--d-fast)}
@media (hover:hover){ .rail button:not([aria-selected="true"]):hover{border-color:var(--line-ui)} }
.rail .n{font:600 12px/1 var(--font-sans);color:var(--text-3)}
.rail .nm{display:inline-flex;align-items:center;gap:6px;font:600 14px/1.3 var(--font-sans)}
.rail .nm .pic{width:16px;height:16px;color:var(--brand)}
.rail .st{font-size:12px;color:var(--text-3)}
.rail button[aria-selected="true"]{border-color:var(--brand);background:var(--brand);color:var(--white);box-shadow:var(--e2)}
.rail button[aria-selected="true"] .n,.rail button[aria-selected="true"] .st{color:rgb(255 255 255 / .8)}          /* 7.16 */
.rail button[aria-selected="true"] .nm .pic{color:var(--mint-300)}
.panel{display:grid;grid-template-columns:repeat(12,minmax(0,1fr));gap:24px;padding:40px;border:1px solid var(--line);border-radius:var(--r-xl);background:var(--surface);box-shadow:var(--e2);animation:fade-up var(--d-base) var(--ease-out);
  grid-template-areas:"head head head head head head head head head head head head" "desc desc desc desc desc desc desc . spec spec spec spec" "caut caut caut caut caut caut caut . spec spec spec spec" "clin clin clin clin clin clin clin clin clin clin clin clin" "faq faq faq faq faq faq faq faq faq faq faq faq" "disc disc disc disc disc disc disc disc disc disc disc disc"}
.p-head{grid-area:head;display:grid;grid-template-columns:minmax(0,1fr) auto;align-items:start;gap:16px;padding-bottom:8px}
.p-head h3{margin-top:12px;font:600 var(--t-h1)/1.15 var(--font-sans);letter-spacing:-.025em;color:var(--text-strong);outline:none}
.p-head .lead{max-width:32em;margin-top:14px}
.p-close{display:inline-flex;align-items:center;gap:6px;min-height:44px;padding:0 16px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);font:600 14px/1 var(--font-sans);color:var(--text-1)}
@media (hover:hover){ .p-close:hover{border-color:var(--brand)} }
.p-close .i{width:16px;height:16px}
.p-desc{grid-area:desc;display:grid;align-content:start;gap:8px}
.p-caut{grid-area:caut;display:grid;align-content:start;gap:10px;padding:20px;border-radius:var(--r-lg);background:var(--warn-bg)}
.p-sub{margin-top:24px;font:700 13px/1.4 var(--font-sans);letter-spacing:.02em;color:var(--text-3)}
.p-sub:first-child{margin-top:0}
.p-desc .body,.p-caut .body{max-width:36em}
.p-spec{grid-area:spec}
.spec-inner{position:sticky;top:calc(var(--nav-h) + 24px);padding:20px 24px 24px;border-radius:var(--r-lg);background:var(--surface-sunk)}
.spec-inner dl.spec{border-top:0}
.p-cta{display:grid;justify-items:start;gap:4px;margin-top:16px}
.p-cta .btn{width:100%}
.p-clin{grid-area:clin;padding-top:40px;border-top:1px solid var(--line)}
.p-clin .hd{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:baseline;gap:8px 16px;margin-bottom:8px}
.grp{margin-top:24px}
.grp .micro{display:block;padding-bottom:8px;border-bottom:1px solid var(--line-strong);font-size:12px}
.crow{display:grid;grid-template-columns:minmax(0,1fr) auto;align-items:center;gap:8px 16px;min-height:64px;margin:0 -8px;padding:12px 8px;border-bottom:1px solid var(--line);border-radius:var(--r-sm);transition:background-color var(--d-fast)}
@media (hover:hover){ .crow:hover{background:var(--surface-tonal-soft)} }
.crow:focus-visible{outline-offset:-2px}
.crow .nm{display:flex;flex-wrap:wrap;align-items:center;gap:8px;font:600 16px/1.35 var(--font-sans);color:var(--text-1)}
.crow.ask .nm{font-weight:500;color:var(--text-2)}
.crow .meta{margin-top:4px}
.crow .tel a{display:inline-block;margin:-10px 0;padding:10px 2px;color:var(--text-accent);text-decoration:underline;text-underline-offset:3px}   /* 44px hit */
.crow .act{display:flex;align-items:center;gap:6px}
.more-row{padding:8px 0}
.p-note{margin-top:12px}
.empty-note{padding:14px 16px;border-radius:var(--r-md);background:var(--surface-sunk);font-size:15px;line-height:1.6;color:var(--text-2)}
.p-faq{grid-area:faq;padding-top:40px}
.p-faq .p-sub{margin:0 0 10px}
.p-disc{grid-area:disc;padding-top:20px;border-top:1px solid var(--line);font-size:13px;line-height:1.6;color:var(--text-3)}
@media (max-width:1023px){
  .index{grid-template-columns:repeat(2,minmax(0,1fr))}
  .rail{grid-template-columns:repeat(4,minmax(0,1fr))}
  .panel{padding:32px;grid-template-areas:"head head head head head head head head head head head head" "desc desc desc desc desc desc spec spec spec spec spec spec" "caut caut caut caut caut caut spec spec spec spec spec spec" "clin clin clin clin clin clin clin clin clin clin clin clin" "faq faq faq faq faq faq faq faq faq faq faq faq" "disc disc disc disc disc disc disc disc disc disc disc disc"}
}
@media (max-width:767px){
  .index{grid-template-columns:minmax(0,1fr);gap:10px}
  .idx-row{min-height:0;gap:8px;padding:16px}
  .idx-row .nm{gap:10px;font-size:17px}
  .idx-row .nm .pic{padding:8px}
  .idx-row .one{-webkit-line-clamp:2}
  .rail{display:flex;gap:8px;overflow-x:auto;scrollbar-width:none;margin:0 calc(-1 * var(--gutter));padding:2px var(--gutter);scroll-snap-type:x proximity;
    -webkit-mask-image:linear-gradient(90deg,#000 calc(100% - 32px),transparent);mask-image:linear-gradient(90deg,#000 calc(100% - 32px),transparent)}
  .rail::-webkit-scrollbar{display:none}
  .rail button{flex:0 0 auto;min-height:56px;scroll-snap-align:start}
  .rail .st{display:none}
  .panel{grid-template-columns:minmax(0,1fr);grid-template-areas:none;gap:28px;padding:24px 16px}
  .panel > *{grid-area:auto}
  .p-head{order:1} .p-spec{order:2} .p-desc{order:3} .p-clin{order:4;padding-top:28px} .p-caut{order:5} .p-faq{order:6;padding-top:0} .p-disc{order:7}
  .spec-inner{position:static}
  .p-head h3{font-size:30px}
  .crow{grid-template-columns:minmax(0,1fr)}
}

/* ---------- data room (celadon band) ---------- */
.data-grid,.tips-grid{display:grid;grid-template-columns:minmax(0,7fr) minmax(0,5fr);align-items:start;gap:24px}
.tips-grid{margin-top:24px}
.data-grid > div:first-child,.mini,.tips-grid > div:first-child{padding:24px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.minis{display:grid;gap:16px}
.mini h3{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:center;gap:6px 12px;margin-bottom:12px;font:700 16px/1.4 var(--font-sans);color:var(--text-1)}
.mbar{display:grid;grid-template-columns:minmax(96px,120px) minmax(0,1fr) 48px;align-items:center;gap:12px;padding:8px 0;border-bottom:1px solid var(--line);font-size:14px;color:var(--text-2)}
.mbar:last-child{border-bottom:0}
.mbar .t{height:10px;overflow:hidden;border-radius:var(--r-pill);background:var(--chart-track)}
.mbar .t i{display:block;height:100%;border-radius:inherit;background:var(--chart-bar)}
#miniChronic .mbar:last-child .t i{background:var(--chart-3)}          /* "치료 없이 방치" = caution series (same as line chart l3) */
.mbar .v{text-align:right;font-weight:700;color:var(--text-1)}
.tl{position:relative;display:grid;grid-template-columns:repeat(4,minmax(0,1fr));margin-top:48px}
.tl::before{content:"";position:absolute;left:0;right:0;top:6px;height:2px;background:linear-gradient(90deg,var(--brand),var(--mint-300))}
.tl > div{position:relative;padding:32px 24px 8px 0}
.tl > div::before{content:"";position:absolute;left:0;top:0;width:14px;height:14px;border:2.5px solid var(--brand);border-radius:50%;background:var(--surface-wash)}
.tl .w{font:600 13px/1.3 var(--font-sans);color:var(--text-3)}
.tl .w b{display:block;margin-top:4px;font:700 22px/1.2 var(--font-sans);letter-spacing:-.02em;color:var(--text-strong)}
.tl h3{margin:14px 0 6px;font:700 17px/1.35 var(--font-sans);color:var(--text-1)}
.tl p{font-size:15px;line-height:1.6;color:var(--text-2)}
.slab{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:16px}
.slab > div{display:grid;align-content:start;gap:6px;padding:24px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.slab .big{font:700 var(--t-kpi)/1 var(--font-sans);letter-spacing:-.03em;color:var(--brand)}
.slab .big small{margin-left:4px;font:600 18px/1 var(--font-sans);letter-spacing:0;color:var(--text-2)}
.slab .l{margin-top:10px;font:700 15px/1.4 var(--font-sans);color:var(--text-1)}
.slab .d{font-size:14px;line-height:1.6;color:var(--text-2)}
.slab > div:first-child{border-color:var(--surface-inverse);background:var(--surface-inverse);box-shadow:var(--e2)}                 /* focal "0원" tile */
.slab > div:first-child .big,.slab > div:first-child .l{color:var(--white)}
.slab > div:first-child .big small,.slab > div:first-child .d{color:rgb(255 255 255 / .8)}
.stack{margin-top:32px;padding-top:24px;border-top:1px dashed var(--line)}
.tips-side .steps li{padding:18px 0}
.tips-side .steps .h-sm{font-size:18px}
@media (max-width:1023px){ .data-grid,.tips-grid{grid-template-columns:minmax(0,1fr)} .slab{grid-template-columns:repeat(2,minmax(0,1fr))} }
@media (max-width:767px){
  .data-grid > div:first-child,.mini,.tips-grid > div:first-child{padding:18px 16px}
  .slab{grid-template-columns:minmax(0,1fr);gap:10px}
  .slab > div{padding:18px 16px}
  .tl{grid-template-columns:minmax(0,1fr)}
  .tl::before{left:6px;right:auto;top:0;bottom:0;width:2px;height:auto;background:linear-gradient(180deg,var(--brand),var(--mint-300))}
  .tl > div{padding:0 0 24px 32px}
}

/* ---------- steps lists (cards) / why (2×2) / home steps (3 cards) ---------- */
.steps{padding:4px 24px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.steps li{display:grid;grid-template-columns:40px minmax(0,1fr);gap:0 16px;padding:22px 0;border-bottom:1px solid var(--line)}
.steps li:last-child{border-bottom:0}
.steps .n{display:grid;place-items:center;width:32px;height:32px;margin-top:-2px;border-radius:50%;background:var(--surface-tonal);color:var(--brand);font:700 13px/1 var(--font-sans)}   /* badge, not a big numeral */
.steps .h-sm{margin-bottom:6px;font-size:19px}
.steps .body{max-width:36em;font-size:15px}
.steps.why{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:16px;padding:0;border:0;background:none;box-shadow:none}
.steps.why li,.steps.why li:last-child{grid-template-columns:minmax(0,1fr);gap:14px;padding:28px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--bg)}
.steps.why .n{width:40px;height:40px;font-size:14px}
.why li .h-sm{font-size:21px}
#home-steps .g75{grid-template-columns:minmax(0,1fr);gap:16px}
#stepsHome{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:16px;padding:0;border:0;background:none;box-shadow:none}
#stepsHome li,#stepsHome li:last-child{grid-template-columns:minmax(0,1fr);gap:14px;padding:24px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.side-note{padding:24px;border:1px solid var(--mint-200);border-radius:var(--r-lg);background:var(--surface-tonal-soft)}
.side-note .body{max-width:30em}
#home-steps .side-note{display:flex;flex-wrap:wrap;align-items:center;justify-content:space-between;gap:12px 24px}
@media (max-width:1023px){ #stepsHome{grid-template-columns:minmax(0,1fr)} }
@media (max-width:767px){ .steps{padding:2px 16px} .steps li{grid-template-columns:36px minmax(0,1fr);gap:0 12px;padding:18px 0} .steps.why{grid-template-columns:minmax(0,1fr)} .steps.why li{padding:20px} }

/* ---------- 한의원 찾기 card (inverse) ---------- */
.find-band{position:relative;display:grid;grid-template-columns:minmax(0,7fr) minmax(0,5fr);align-items:center;gap:32px 56px;overflow:hidden;padding:56px;border-radius:var(--r-xl);background:var(--surface-inverse);box-shadow:var(--e3)}
.find-band::before{content:"";position:absolute;right:-12%;top:-50%;width:62%;aspect-ratio:1;border-radius:50%;pointer-events:none;background:radial-gradient(circle,rgb(127 209 176 / .22),transparent 65%)}
.find-band > *{position:relative}
.find-cta{display:grid;justify-items:start;gap:14px}
.find-cta .h-md{max-width:16em}
.find-cta .lead{max-width:30em}
.find-cta .chips{margin:4px 0 8px}
.band-ink .chip{border-color:var(--line-ui);background:transparent;color:var(--text-1)}
@media (hover:hover){ .band-ink .chip:not([aria-pressed="true"]):hover{border-color:var(--white);background:var(--state-hover)} }
.band-ink .chip .cnt{color:var(--text-3)}
@media (max-width:1023px){ .find-band{grid-template-columns:minmax(0,1fr);padding:40px 32px} }
@media (max-width:767px){ .find-band{padding:32px 20px;border-radius:var(--r-lg)} }

details.chart-table{margin-top:12px}
details.chart-table > summary{display:inline-flex;align-items:center;gap:6px;min-height:44px;list-style:none;cursor:pointer;font:600 14px/1 var(--font-sans);color:var(--text-accent)}
details.chart-table > summary::-webkit-details-marker{display:none}
details.chart-table > summary::after{content:"";width:14px;height:14px;background:currentColor;-webkit-mask:var(--ico-chev) center/contain no-repeat;mask:var(--ico-chev) center/contain no-repeat;transform:rotate(90deg);transition:transform var(--d-base) var(--ease-std)}
details.chart-table[open] > summary::after{transform:rotate(-90deg)}
details.chart-table .ins-wrap{margin:8px 0 0}
```

### 3.5 CHARTS — `@layer components`

Classes covered:

| Group | Classes |
|---|---|
| Chart frame | `chart-h`, `t` (scoped as `.chart-h .t`) |
| Range and bar charts | `rchart`, `rrow`, `lbl`, `val`, `rtrack`, `rbar`, `solid`, `good`, `hit`, `raxis`, `ticks`, `rnote` |
| Insurer charts | `ins-charts`, `ins-chart`, `std-card`, `avgline`, `avgkey` |
| Line chart | `linechart`, `grid`, `axis`, `band` (new), `a1`, `l1`, `l2`, `l3`, `m1`, `m2`, `m3`, `lab`, `t1`/`t2`/`t3` (new), `band-l` (new), `lg` |
| Stacked bar | `stackbar`, `s1`–`s4`, `legend`, `sw` (swatch), `pct` |

Encoding rules:
- Series are told apart by dash pattern as well as colour: l1 is solid forest, l2 is dashed mint-650, l3 is dotted amber.
- Every value is printed as text.
- Range charts use `role="group"` so their rows are read, plus a spelled-out `aria-label` (M12, J10-C11).
- The `title` tooltips stay, but only as a bonus for mouse users.

```css
.chart-h{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:baseline;gap:4px 16px;margin-bottom:14px}
.chart-h .t{font:700 16px/1.4 var(--font-sans);color:var(--text-1)}
.rchart{border-top:1px solid var(--line)}
.rrow{display:grid;grid-template-columns:minmax(120px,180px) minmax(0,1fr) 112px;align-items:center;gap:16px;padding:12px 0;border-bottom:1px solid var(--line)}
.rrow .lbl{font-size:15px;line-height:1.35;color:var(--text-1)}
.rrow .lbl small{display:block;margin-top:2px;font-size:12px;color:var(--text-3)}
.rtrack{position:relative;height:24px;background-image:repeating-linear-gradient(90deg,var(--chart-grid) 0 1px,transparent 1px 20%)}
.rbar{position:absolute;top:6px;height:12px;min-width:6px;border-radius:6px;background:var(--chart-bar);transform-origin:0 50%;transition:transform var(--d-max) var(--ease-out)}
.rbar::before,.rbar::after{content:"";position:absolute;top:-4px;width:2px;height:20px;border-radius:1px;background:var(--forest-950)}    /* range end caps */
.rbar::before{left:0} .rbar::after{right:0}
.rbar.solid::before,.rbar.solid::after{content:none}
.rbar.good{background:var(--chart-good)}
.rrow .val{text-align:right;font:600 15px/1.3 var(--font-sans);color:var(--text-1)}
.raxis{display:grid;grid-template-columns:minmax(120px,180px) minmax(0,1fr) 112px;gap:16px;padding-top:8px}
.raxis .ticks{display:flex;justify-content:space-between;font-size:12px;color:var(--chart-axis)}
.rnote{margin-top:16px}
.avgline{position:absolute;top:-3px;bottom:-3px;width:0;border-left:2px dashed var(--chart-avg)}           /* was mint 1.80 → ink ≥8.8 */
.avgkey{display:inline-block;width:0;height:12px;margin-right:6px;vertical-align:-1px;border-left:2px dashed var(--chart-avg)}
.ins-charts{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:16px}
.ins-chart{padding:24px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.ins-chart.std-card{border-color:var(--mint-200);background:var(--surface-tonal-soft);box-shadow:none}
.ins-chart .rrow,.ins-chart .raxis{grid-template-columns:104px minmax(0,1fr) 76px}
.ins-chart .rrow{padding:9px 0}
.ins-chart .rrow .lbl,.ins-chart .rrow .val{font-size:14px}
.ins-chart .rtrack{height:22px}
.ins-chart .rbar{top:6px;height:10px}
.std-card dl.spec > div{grid-template-columns:96px minmax(0,1fr);padding:10px 0}
.std-card dl.spec dd{font-size:15px}
.std-card dl.spec dd small{font-size:13px}
@media (max-width:1023px){ .ins-charts{grid-template-columns:minmax(0,1fr)} }
@media (max-width:767px){
  .rrow,.ins-chart .rrow{grid-template-columns:minmax(0,1fr) auto;gap:6px 12px}
  .rrow .rtrack{grid-column:1 / -1;grid-row:2}
  .raxis,.ins-chart .raxis{grid-template-columns:minmax(0,1fr)}
  .raxis > :empty{display:none}                 /* keeps ticks + "예시값" + 업계 평균 key visible (old rule hid them) */
}
/* line chart (J13 draws at container width → labels stay 12px on phones) */
.linechart{display:block;width:100%;height:auto;overflow:visible}
.linechart text{font-family:var(--font-sans);font-size:12px;fill:var(--chart-axis)}
.linechart .grid{stroke:var(--chart-grid);stroke-width:1}
.linechart .axis{stroke:var(--line-ui);stroke-width:1}
.linechart .band{fill:var(--chart-band)}
.linechart .a1{fill:rgb(15 74 57 / .07)}
.linechart .l1,.linechart .l2,.linechart .l3{fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round}
.linechart .l1{stroke:var(--chart-1)}
.linechart .l2{stroke:var(--chart-2);stroke-dasharray:8 5}
.linechart .l3{stroke:var(--chart-3);stroke-dasharray:1 5}
.linechart .m1{fill:var(--chart-1);stroke:var(--surface);stroke-width:2}
.linechart .m2{fill:var(--surface);stroke:var(--chart-2);stroke-width:2}
.linechart .m3{fill:var(--surface);stroke:var(--chart-3);stroke-width:2}
.linechart .lab{font-weight:700;fill:var(--text-1)}
.linechart .lab.t1{fill:var(--chart-1)} .linechart .lab.t2{fill:var(--text-accent)} .linechart .lab.t3{fill:var(--amber-700)}
.linechart .lab.band-l{font-weight:600;fill:var(--amber-700)}
div.lg{display:flex;flex-wrap:wrap;gap:8px 20px;margin-top:12px;font-size:14px;color:var(--text-2)}
div.lg span{display:inline-flex;align-items:center;gap:8px}
div.lg span::before{content:"";width:22px;height:0;border-top:2.5px solid var(--chart-1)}
div.lg span.s2::before{border-top:2.5px dashed var(--chart-2)}
div.lg span.s3::before{border-top:3px dotted var(--chart-3)}
/* stacked 100% bar: 2px gaps, outline + hatch on light segments, % always in legend */
.stackbar{display:flex;gap:2px;height:28px;overflow:hidden;border-radius:var(--r-sm);background:var(--surface)}
.stackbar span{display:block;height:100%;transform-origin:0 50%;transition:transform var(--d-max) var(--ease-out) 160ms}
.stackbar .s1,.legend .sw.s1,.est-mix .b .s1{background:var(--stack-1)}
.stackbar .s2,.legend .sw.s2,.est-mix .b .s2{background:var(--stack-2)}
.stackbar .s3,.legend .sw.s3,.est-mix .b .s3{background:var(--stack-3);box-shadow:inset 0 0 0 1px var(--line-strong)}
.stackbar .s4,.legend .sw.s4,.est-mix .b .s4{background:var(--stack-4);box-shadow:inset 0 0 0 1px var(--line-strong)}
.legend{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:12px 16px;margin-top:14px}
.legend > div{display:grid;gap:2px;padding-top:10px;border-top:2px solid var(--line)}
.legend .sw{display:inline-block;width:12px;height:12px;margin-right:6px;border-radius:3px;vertical-align:-1px}
.legend b{font:600 14px/1.4 var(--font-sans);color:var(--text-1)}
.legend .pct{font:700 20px/1.2 var(--font-sans);color:var(--text-strong)}
.legend .meta{font-size:13px}
@media (max-width:767px){ .legend{grid-template-columns:repeat(2,minmax(0,1fr))} }
```

### 3.6 FIND + DETAIL + BOOKING + COMPARE — `@layer sections`

Classes covered:

| Group | Classes |
|---|---|
| Page header | `phead`, `phead-grid`, `has-aside`, `pt`, `pa`, `pa-card` |
| Region map | `pa-map`, `rc`, `city`, `d`, `far`, `n`, `link`, `cap` |
| Controls (new) | `find-controls`, `find-note` |
| Filters and results bar | `ctabs`, `dist`, `filters`, `lbl`, `vr`, `resbar`, `sortsel`, `list`, `ghead`, `empty` |
| Clinic row | `row`, `pending`, `thumb`, `mid`, `nm`, `meta`, `tags`, `tx`, `quote`, `side`, `cmp`, `box`, `principles` |
| Detail | `gal`, `main`, `ths`, `th`, `d-info`, `dtabs`, `pane`, `note`, `prog`, `t`, `dur`, `inc`, `pgrid` |
| Form | `form`, `fld`, `help`, `two`, `selwrap`, `slots`, `sw`, `l`, `toggle`, `knob`, `done` |
| Compare | `ctab`, `yes`, `ask` |

```css
/* ---------- page headers (find / guide / reviews / partner — all light) ---------- */
.phead,.dark-strip.phead-dark{position:relative;border-bottom:1px solid var(--line);background:linear-gradient(180deg,var(--surface-tonal-soft),var(--bg));color:var(--text-1)}
.phead{padding:56px 0 48px}
.phead.has-aside{padding:48px 0 40px}
.phead-grid{display:grid;grid-template-columns:minmax(0,7fr) minmax(0,5fr);align-items:center;gap:32px 56px}
.phead .display,.dark-strip.phead-dark .display{max-width:18em;margin-top:12px;font-size:var(--t-display)}
.phead .lead,.dark-strip.phead-dark .lead{margin-top:16px}
.phead .search{margin-top:28px}
.pa-card{padding:24px;border:1px solid var(--line);border-radius:var(--r-xl);background:var(--surface);box-shadow:var(--e2)}
@media (max-width:1023px){ .phead-grid{grid-template-columns:minmax(0,1fr)} }
@media (max-width:767px){ .phead,.phead.has-aside{padding:32px 0 28px} .pa-card{padding:18px;border-radius:var(--r-lg)} }

/* ---------- region cloud (hidden on phones: tabs + chips cover it) ---------- */
.pa-map{position:relative;aspect-ratio:1.5;overflow:hidden;padding:0;background:radial-gradient(circle at 44% 46%,var(--surface-tonal-soft) 0,var(--surface) 70%)}
.pa-map .link{position:absolute;height:1px;background:var(--line-soft);transform-origin:0 0}
.pa-map .rc{position:absolute;display:inline-flex;align-items:center;gap:8px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);box-shadow:var(--e1);color:var(--text-1);font-weight:600;white-space:nowrap;cursor:pointer;
  transform:translate(-50%,-50%);transition:border-color var(--d-fast),transform var(--d-fast) var(--ease-out),box-shadow var(--d-fast)}
.pa-map .rc::after{content:"";position:absolute;inset:-4px;border-radius:inherit}                    /* 44px hit */
@media (hover:hover){ .pa-map .rc:hover{border-color:var(--brand);box-shadow:var(--e2);transform:translate(-50%,calc(-50% - 2px))} }
.pa-map .rc .n{display:inline-grid;place-items:center;border-radius:var(--r-pill);background:var(--brand);color:var(--white);font-weight:700}
.pa-map .rc.city{min-height:52px;padding:0 20px 0 6px;border-color:var(--brand);background:var(--brand);color:var(--white);font-size:17px}
.pa-map .rc.city .n{width:40px;height:40px;background:var(--mint-400);color:var(--forest-900);font-size:15px}       /* 7.04 */
.pa-map .rc.d{min-height:36px;padding:0 12px 0 5px;font-size:14px}
.pa-map .rc.d .n{width:26px;height:26px;font-size:12px}
.pa-map .rc.d.far .n{background:var(--text-3)}                                                         /* white 5.95 */
.pa-map .cap{position:absolute;left:16px;bottom:12px;font-size:12px;color:var(--text-3)}
@media (max-width:1023px){ .pa-map{aspect-ratio:1.6} }
@media (max-width:767px){ .pa-map{display:none} }

/* ---------- controls card, find-note, sticky result bar ---------- */
#find{padding-top:32px}
.find-controls{display:grid;gap:14px;padding:16px 20px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.ctabs{display:flex;gap:4px;border-bottom:1px solid var(--line)}                                     /* base = underline tabs (.dtabs uses this) */
.ctabs button{position:relative;min-height:48px;padding:0 14px;font:600 15px/1 var(--font-sans);color:var(--text-3);white-space:nowrap}
.ctabs button:focus-visible{outline-offset:-2px}
@media (hover:hover){ .ctabs button:hover{color:var(--text-1)} }
.ctabs button[aria-selected="true"]{color:var(--text-strong);font-weight:700}
.ctabs button[aria-selected="true"]::after{content:"";position:absolute;left:10px;right:10px;bottom:-1px;height:3px;border-radius:3px 3px 0 0;background:var(--brand)}
.find-controls .ctabs{justify-self:start;gap:4px;padding:4px;border:0;border-radius:var(--r-pill);background:var(--surface-sunk)}      /* #cityTabs = segmented */
.find-controls .ctabs button{min-height:40px;padding:0 18px;border-radius:var(--r-pill);color:var(--text-2)}
.find-controls .ctabs button[aria-selected="true"]{background:var(--surface);color:var(--text-strong);box-shadow:var(--e1),inset 0 0 0 1px var(--line-ui)}
.find-controls .ctabs button[aria-selected="true"]::after{content:none}
@media (pointer:coarse){ .find-controls .ctabs button{min-height:44px} }
.dist:empty{display:none}
.filters{display:flex;flex-wrap:wrap;align-items:center;gap:8px}
.filters .lbl{margin-right:2px;font:700 13px/1 var(--font-sans);color:var(--text-3)}
.filters .vr{width:1px;height:24px;margin:0 8px;background:var(--line)}
.find-note{display:flex;flex-wrap:wrap;align-items:center;gap:6px 10px;margin:12px 0 0;padding:12px 16px;border-radius:var(--r-md);background:var(--surface-sunk);font-size:14px;line-height:1.55;color:var(--text-2)}
.find-note > .i{width:18px;height:18px;color:var(--text-3)}
.find-note > span{flex:1 1 20em}
.find-note b{color:var(--text-1)}
.resbar{position:sticky;top:var(--nav-h);z-index:var(--z-sticky);display:flex;justify-content:space-between;align-items:center;gap:16px;min-height:60px;margin:12px calc(-1 * var(--gutter));padding:8px var(--gutter);
  border-bottom:1px solid var(--line);background:rgb(244 246 241 / .94);-webkit-backdrop-filter:blur(8px);backdrop-filter:blur(8px)}
.resbar .meta{font:600 15px/1.4 var(--font-sans);color:var(--text-1)}
.sortsel{position:relative;display:inline-flex;align-items:center;flex:0 0 auto}
.sortsel select{min-height:44px;padding:0 40px 0 16px;border:1px solid var(--line-ui);border-radius:var(--r-pill);background:var(--surface);color:var(--text-1);font:500 16px/1 var(--font-sans);cursor:pointer;-webkit-appearance:none;appearance:none}  /* 16px: no iOS zoom */
.sortsel .i{position:absolute;right:14px;width:16px;height:16px;color:var(--text-3);pointer-events:none}
@media (max-width:767px){
  .find-controls{padding:12px}
  .filters{flex-wrap:nowrap;overflow-x:auto;scrollbar-width:none;margin:0 -12px;padding:2px 12px;scroll-snap-type:x proximity;
    -webkit-mask-image:linear-gradient(90deg,#000 calc(100% - 32px),transparent);mask-image:linear-gradient(90deg,#000 calc(100% - 32px),transparent)}
  .filters::-webkit-scrollbar{display:none}
  .filters .chip,.filters .lbl,.filters .vr{flex:0 0 auto}
  .filters .chip{scroll-snap-align:start}
}

/* ---------- clinic rows = cards; 예약 is the primary action (J4) ---------- */
.list{display:grid;gap:12px}
.ghead{display:flex;justify-content:space-between;align-items:baseline;gap:12px;margin-top:28px;padding:0 4px}
.ghead:first-child{margin-top:4px}
.list .empty{padding:16px 18px;border-radius:var(--r-md);background:var(--surface-sunk);font-size:15px;line-height:1.6;color:var(--text-2)}
.row{display:grid;grid-template-columns:168px minmax(0,1fr) 184px;align-items:start;gap:24px;padding:20px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1);transition:border-color var(--d-fast),box-shadow var(--d-base)}
@media (hover:hover){ .row:hover{border-color:var(--line-ui);box-shadow:var(--e2)} }
.row:has(.cmp input:checked){border-color:var(--brand);box-shadow:0 0 0 1px var(--brand),var(--e2)}
.row.pending{border-style:dashed;border-color:var(--line-ui);background:transparent;box-shadow:none}
.thumb{position:relative;width:168px;aspect-ratio:4 / 3;overflow:hidden;border-radius:var(--r-md);background-color:var(--sage-200);background-size:cover;background-position:center}
.row .mid{display:grid;gap:10px;min-width:0}
.row .nm{justify-self:start;display:inline-flex;align-items:center;min-height:44px;margin:-8px 0 -6px;font:600 22px/1.25 var(--font-sans);letter-spacing:-.02em;color:var(--text-strong);text-align:left}
@media (hover:hover){ .row .nm:hover{text-decoration:underline;text-decoration-color:var(--accent-strong);text-underline-offset:5px} }
.row .mid > .meta{margin-top:0}
.row .meta a{display:inline-block;margin:-10px 0;padding:10px 0;color:var(--text-accent);text-decoration:underline;text-underline-offset:3px}
.row .tx{font-size:14px;line-height:1.55;color:var(--text-2)}
.row .quote{position:relative;padding:10px 12px 10px 36px;border-radius:var(--r-sm);background:var(--surface-sunk);font-size:14px;line-height:1.55;color:var(--text-3)}   /* "첫 후기를 기다리고 있어요" — visible on ALL widths */
.row .quote::before{content:"";position:absolute;left:12px;top:12px;width:16px;height:16px;background:currentColor;-webkit-mask:var(--ico-chat) center/contain no-repeat;mask:var(--ico-chat) center/contain no-repeat}
.row.pending .nm{color:var(--text-2)}
.row.pending .mid > .meta,.row.pending .tx{color:var(--text-3)}
.row .side{display:grid;align-content:start;gap:8px}
.row .side .btn{width:100%}
.cmp{display:inline-flex;align-items:center;gap:10px;min-height:44px;font:500 14px/1.3 var(--font-sans);color:var(--text-1);cursor:pointer;user-select:none}
.cmp input{position:absolute;width:1px;height:1px;opacity:0}
.cmp .box{display:grid;place-items:center;flex:0 0 auto;width:22px;height:22px;border:1.5px solid var(--line-ui);border-radius:var(--r-xs);background:var(--surface);color:var(--white);transition:background-color var(--d-fast),border-color var(--d-fast)}
.cmp .box .i{width:14px;height:14px;stroke-width:2.4;opacity:0;transform:scale(.6);transition:opacity var(--d-fast),transform var(--d-base) var(--ease-out)}
.cmp input:checked + .box{border-color:var(--brand);background:var(--brand)}
.cmp input:checked + .box .i{opacity:1;transform:none}
.cmp input:focus-visible + .box{outline:var(--focus-w) solid var(--focus);outline-offset:2px}
.principles{margin-top:56px}
@media (max-width:1023px){
  .row{grid-template-columns:120px minmax(0,1fr)}
  .thumb{width:120px}
  .row .side{grid-column:1 / -1;display:flex;flex-wrap:wrap;align-items:center;gap:8px}
  .row .side .cmp{margin-right:auto}
  .row .side .btn{width:auto}
}
@media (max-width:767px){
  .row{grid-template-columns:88px minmax(0,1fr);gap:14px;padding:16px}
  .thumb{width:88px}
  .row .nm{font-size:19px}
  .row .side .cmp{flex:1 1 100%}
  .row .side .btn{flex:1 1 0}
}

/* ---------- detail sheet ---------- */
.gal{display:grid;gap:8px;margin-top:16px}
.gal .main{position:relative;aspect-ratio:16 / 9;border-radius:var(--r-md);background-color:var(--sage-200);background-size:cover;background-position:center}
.gal .ths{display:grid;grid-template-columns:repeat(6,minmax(0,1fr));gap:8px}
.gal .th{position:relative;min-height:44px;aspect-ratio:4 / 3;border-radius:var(--r-sm);background-color:var(--sage-200);background-size:cover;background-position:center;opacity:.8;transition:opacity var(--d-fast)}
@media (hover:hover){ .gal .th:hover{opacity:1} }
.gal .th[aria-pressed="true"]{opacity:1;box-shadow:inset 0 0 0 3px var(--brand),inset 0 0 0 5px var(--white)}       /* inset = selected; outline = focus */
.d-info{display:grid;gap:8px;padding:24px 0 8px}
.d-info h2{font:600 28px/1.2 var(--font-sans);letter-spacing:-.025em;color:var(--text-strong)}
.dtabs{position:sticky;top:0;z-index:var(--z-raised);overflow-x:auto;scrollbar-width:none;background:var(--surface)}
.dtabs::-webkit-scrollbar{display:none}
.pane{display:grid;gap:16px;padding:24px 0 8px}
.pane .note{padding:12px 14px;border-radius:var(--r-md);background:var(--surface-sunk);font-size:14px;line-height:1.55;color:var(--text-3)}
.prog{display:grid;gap:6px;padding:16px 0;border-bottom:1px solid var(--line)}
.prog .t{display:flex;justify-content:space-between;align-items:baseline;gap:12px}
.prog h4{font:700 17px/1.35 var(--font-sans);color:var(--text-1)}
.prog .dur{flex:0 0 auto;padding:3px 8px;border-radius:var(--r-xs);background:var(--surface-tonal);font:600 13px/1.3 var(--font-sans);color:var(--success-fg);white-space:nowrap}
.prog .inc{font-size:14px;color:var(--text-3)}
.pgrid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:8px}
.pgrid > div{position:relative;aspect-ratio:1;border-radius:var(--r-md);background-color:var(--sage-200);background-size:cover;background-position:center}

/* ---------- forms (booking, review, partner) ---------- */
.form{display:grid;gap:24px;padding-top:24px}
.two{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:20px}
.fld label{display:block;margin-bottom:8px;font:600 14px/1.4 var(--font-sans);color:var(--text-1)}
.fld input,.fld select,.fld textarea{width:100%;min-height:48px;padding:0 14px;border:1px solid var(--line-ui);border-radius:var(--r-sm);background:var(--surface-sunk);color:var(--text-1);font-size:16px;outline:none;
  transition:border-color var(--d-fast),background-color var(--d-fast),box-shadow var(--d-fast)}          /* filled field; border 3.48 */
.fld textarea{min-height:120px;padding:12px 14px;line-height:1.6;resize:vertical}
.fld input[type="date"]{color-scheme:light}
@media (hover:hover){ .fld input:hover,.fld select:hover,.fld textarea:hover{border-color:var(--text-3)} }
.fld input:focus,.fld select:focus,.fld textarea:focus{border-color:var(--brand);background:var(--surface);box-shadow:0 0 0 1px var(--brand),0 0 0 5px var(--focus-halo)}
.fld input::placeholder,.fld textarea::placeholder{color:var(--text-3)}
.fld [aria-invalid="true"]{border-color:var(--danger-fg);background:var(--danger-bg);box-shadow:0 0 0 1px var(--danger-fg)}
.fld .help,.form > .help{display:flex;align-items:flex-start;gap:6px;margin-top:8px;font-size:14px;line-height:1.5;color:var(--text-3)}
.fld .help[role="alert"],.form > .help[role="alert"]{font-weight:600;color:var(--danger-fg)}
.fld .help[role="alert"]::before,.form > .help[role="alert"]::before{content:"";flex:0 0 16px;height:16px;margin-top:2px;background:currentColor;-webkit-mask:var(--ico-alert) center/contain no-repeat;mask:var(--ico-alert) center/contain no-repeat}
.selwrap{position:relative}
.selwrap select{padding-right:40px;cursor:pointer;-webkit-appearance:none;appearance:none}
.selwrap .i{position:absolute;right:14px;top:50%;width:16px;height:16px;margin-top:-8px;color:var(--text-3);pointer-events:none}
.slots{display:grid;grid-template-columns:repeat(auto-fill,minmax(84px,1fr));gap:8px}
.slots .chip{justify-content:center;padding:0 8px}
.slots .chip[aria-pressed="true"]{padding-left:8px}
.slots .chip[aria-pressed="true"]::before{display:none}          /* fill is enough in fixed grid cells */
label.sw{display:flex;justify-content:space-between;align-items:center;gap:16px;min-height:64px;padding:12px 0;border-bottom:1px solid var(--line);cursor:pointer}
label.sw .l{font:600 16px/1.4 var(--font-sans);color:var(--text-1)}
label.sw .l small{display:block;margin-top:2px;font:400 14px/1.5 var(--font-sans);color:var(--text-3)}
.toggle{position:relative;flex:0 0 auto;width:52px;height:32px;border-radius:var(--r-pill);background:var(--surface);box-shadow:inset 0 0 0 1.5px var(--line-ui);transition:background-color var(--d-fast),box-shadow var(--d-fast)}
.toggle input{position:absolute;inset:0;width:100%;height:100%;margin:0;opacity:0;cursor:pointer}
.toggle .knob{position:absolute;top:4px;left:4px;width:24px;height:24px;border-radius:50%;background:var(--text-3);box-shadow:var(--e1);pointer-events:none;transition:transform var(--d-base) var(--ease-out),background-color var(--d-fast)}
.toggle:has(input:checked){background:var(--brand);box-shadow:none}
.toggle input:checked ~ .knob{background:var(--white);transform:translateX(20px)}           /* position + colour + track = not colour-only */
.toggle:has(input:focus-visible){outline:var(--focus-w) solid var(--focus);outline-offset:2px}
.sheet-b .done{display:grid;gap:24px;padding:32px 0 8px}
.sheet-b .done .h-md{margin-top:8px;font-size:28px}
.sheet-b .done ul.body{display:grid;gap:4px}
@media (max-width:767px){ .two{grid-template-columns:minmax(0,1fr)} }

/* ---------- compare modal ---------- */
.ctab{overflow-x:auto}
.ctab table{width:100%;min-width:640px;border-collapse:separate;border-spacing:0;font-size:15px}
.ctab th,.ctab td{padding:14px 16px 14px 0;text-align:left;vertical-align:top}
.ctab thead th{border-bottom:2px solid var(--brand);font:700 16px/1.35 var(--font-sans);color:var(--text-strong)}
.ctab thead th:first-child{width:150px}
.ctab thead th:first-child,.ctab tbody th{position:sticky;left:0;z-index:1;background:var(--surface)}
.ctab tbody th{border-bottom:1px solid var(--line);font:600 13px/1.4 var(--font-sans);color:var(--text-3)}
.ctab td{border-bottom:1px solid var(--line);color:var(--text-2)}
.ctab tr:last-child td,.ctab tr:last-child th{border-bottom:0}
.ctab td .yes,.ctab td .ask{display:inline-flex;align-items:center;gap:5px}
.ctab td .yes{--m:var(--ico-check);font-weight:600;color:var(--success-fg)}
.ctab td .ask{--m:var(--ico-clock);min-height:24px;padding:2px 8px;border:1px dashed var(--pending-line);border-radius:var(--r-xs);font-size:13px;line-height:1.4;color:var(--pending-fg)}   /* 확인 중 never blank */
.ctab td .yes::before,.ctab td .ask::before{content:"";flex:0 0 auto;width:13px;height:13px;background:currentColor;-webkit-mask:var(--m) center/contain no-repeat;mask:var(--m) center/contain no-repeat}
```

### 3.7 GUIDE + REVIEWS + PARTNER — `@layer sections`

Classes covered:

| Group | Classes |
|---|---|
| Guide | `toc` (new), `ins`, `ins-wrap`, `insurers`, `d`, `cnt`, `avg`, `acc`, `acc-row`, `acc-body`, `flow`, `mini-kv`, `pa-guide`, `big`, `l`, `checklist`, `bx`, `row-cta` (inline, unstyled) |
| Forms and done blocks | `form-wrap`, `done-block` |
| Reviews | `rvcard`, `r1`, `r2`, `who`, `ph`, `q`, `pa-review` |
| Partner header and KPI | `dark-strip`, `phead-dark`, `cta`, `dark-dl` (hidden), `pa-kpi`, `bar`, `kpis`, `rowsm` |
| Partner demo | `demo-frame`, `demo-grid`, `demo-cap`, `mock`, `dots`, `tblwrap`, `rows`, `r`, `sub`, `fields`, `f`, `v`, `ph`, `foot` (as `.mock .foot`) |
| Partner widgets | `pill`, `ink`, `dim`, `sw-mini` |

```css
/* ---------- guide TOC (sticky) ---------- */
.toc{position:sticky;top:var(--nav-h);z-index:var(--z-sticky);border-bottom:1px solid var(--line);background:rgb(244 246 241 / .94);-webkit-backdrop-filter:blur(10px);backdrop-filter:blur(10px)}
.toc ol{display:flex;gap:6px;overflow-x:auto;scrollbar-width:none;padding-block:8px}
.toc ol::-webkit-scrollbar{display:none}
.toc li{flex:0 0 auto}
.toc a{display:inline-flex;align-items:center;min-height:40px;padding:0 16px;border:1px solid var(--line);border-radius:var(--r-pill);background:var(--surface);font:600 14px/1 var(--font-sans);color:var(--text-2);transition:border-color var(--d-fast),background-color var(--d-fast),color var(--d-fast)}
@media (pointer:coarse){ .toc a{min-height:44px} }
@media (hover:hover){ .toc a:not([aria-current="true"]):hover{border-color:var(--brand);color:var(--text-1)} }
.toc a[aria-current="true"]{border-color:var(--brand);background:var(--brand);color:var(--white)}       /* J14 scrollspy */

/* ---------- tables ---------- */
.ins-wrap{overflow-x:auto;margin:4px 0 20px;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface)}
table.ins{width:100%;min-width:520px;font-size:14px}
table.ins th{padding:12px 14px;border-bottom:1px solid var(--line-soft);background:var(--surface-sunk);text-align:left;vertical-align:bottom;font:600 13px/1.4 var(--font-sans);color:var(--text-2)}
table.ins td{padding:12px 14px;border-bottom:1px solid var(--line);vertical-align:top;line-height:1.55;color:var(--text-2)}
table.ins tr:last-child td{border-bottom:0}
table.ins td:first-child{min-width:9em;font-weight:600;color:var(--text-1)}
table.ins td.st{white-space:nowrap}
table.insurers th,table.insurers td:first-child{white-space:nowrap}
table.insurers th:not(:first-child),table.insurers td.num{text-align:right}
table.insurers .d{display:block;margin-top:2px;font-size:12px;font-weight:400;color:var(--text-3)}
table.insurers .cnt{display:inline-flex;align-items:center;height:22px;padding:0 8px;border:1px dashed var(--pending-line);border-radius:var(--r-pill);font-size:12px;color:var(--pending-fg)}   /* 미집계 */
table.insurers tr.avg td{border-top:1.5px solid var(--brand);background:var(--surface-tonal-soft);font-weight:700;color:var(--text-strong)}

/* ---------- accordion (guide + panel FAQ) ---------- */
.acc{overflow:hidden;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface)}
.acc-row{border-bottom:1px solid var(--line)}
.acc-row:last-child{border-bottom:0}
.acc-row > button{display:grid;grid-template-columns:minmax(0,1fr) 32px;align-items:center;gap:16px;width:100%;min-height:60px;padding:14px 16px 14px 20px;text-align:left;font:600 16px/1.45 var(--font-sans);color:var(--text-1);transition:background-color var(--d-fast)}
@media (hover:hover){ .acc-row > button:hover{background:var(--surface-tonal-soft)} }
.acc-row > button:focus-visible{outline-offset:-2px}
.acc-row > button .i{width:32px;height:32px;padding:7px;border-radius:50%;background:var(--surface-sunk);color:var(--text-2);transition:transform var(--d-base) var(--ease-std),color var(--d-fast)}
.acc-row > button[aria-expanded="true"] .i{color:var(--brand);transform:rotate(180deg)}
.acc-row .acc-body{display:grid;grid-template-rows:0fr;transition:grid-template-rows var(--d-base) var(--ease-std)}
.acc-row .acc-body > div{overflow:hidden}
.acc-row[data-open="true"] .acc-body{grid-template-rows:1fr}
.acc-row .acc-body .body{max-width:44em;padding:0 20px 20px}
.acc-row .acc-body .ins-wrap{margin:0 20px 20px}

/* ---------- guide header card, checklist ---------- */
.pa-guide .big{font:700 64px/1 var(--font-sans);letter-spacing:-.04em;color:var(--text-strong)}
.pa-guide .big small{margin-left:4px;font:600 24px/1 var(--font-sans);letter-spacing:0;color:var(--text-2)}
.pa-guide .l{margin-top:8px;font-size:14px;color:var(--text-3)}
.flow{position:relative;display:grid;grid-template-columns:repeat(3,minmax(0,1fr));margin:24px 0 0}
.flow::before{content:"";position:absolute;left:14%;right:14%;top:15px;height:2px;background:linear-gradient(90deg,var(--brand),var(--mint-300))}
.flow li{position:relative;display:grid;justify-items:center;gap:4px;text-align:center}
.flow .n{display:grid;place-items:center;width:30px;height:30px;border-radius:50%;background:var(--brand);color:var(--white);font:700 13px/1 var(--font-sans);box-shadow:0 0 0 6px var(--surface)}
.flow b{margin-top:6px;font:700 15px/1.3 var(--font-sans);color:var(--text-1)}
.flow li > span:last-child{font-size:13px;line-height:1.4;color:var(--text-3)}
.mini-kv{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:center;gap:8px 12px;margin-top:24px;padding-top:16px;border-top:1px dashed var(--line);font-size:14px;color:var(--text-2)}
.checklist{overflow:hidden;border:1px solid var(--line);border-radius:var(--r-lg);background:var(--surface)}
.checklist li{display:grid;grid-template-columns:24px minmax(0,1fr);align-items:start;gap:14px;min-height:56px;padding:16px 20px;border-bottom:1px solid var(--line);font-size:16px;line-height:1.6;color:var(--text-2)}
.checklist li:last-child{border-bottom:0}
.checklist li .bx{width:20px;height:20px;margin-top:3px;border:1.5px solid var(--line-ui);border-radius:var(--r-xs)}
.checklist li b{font-weight:600;color:var(--text-1)}
@media (max-width:767px){ .pa-guide .big{font-size:48px} }

/* ---------- reviews ---------- */
.form-wrap{padding:32px;border:1px solid var(--line);border-radius:var(--r-xl);background:var(--surface);box-shadow:var(--e2)}
.form-wrap .h-sm{margin-bottom:4px}
.form-wrap .form{padding-top:16px}
.done-block{display:grid;gap:16px;padding-top:8px}
.done-block .body{max-width:30em}
.rvcard{position:relative;display:grid;gap:12px;padding:24px 28px;border:1px solid var(--line);border-top:3px solid var(--brand);border-radius:var(--r-lg);background:var(--surface);box-shadow:var(--e1)}
.rvcard::before{content:"“";content:"“" / "";position:absolute;right:24px;top:6px;font:300 64px/1 var(--font-sans);color:var(--mint-400);pointer-events:none}   /* decorative */
.rvcard .r1,.pa-review .r1{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:baseline;gap:8px 16px}
.rvcard .who,.pa-review .who{display:inline-flex;align-items:center;gap:6px;font:600 16px/1.4 var(--font-sans);color:var(--text-1)}
.rvcard .body{max-width:36em;font-size:16px;color:var(--text-1)}
.rvcard .ph{display:flex;gap:8px}
.rvcard .ph > div{position:relative;width:72px;height:72px;border:1px dashed var(--line-ui);border-radius:var(--r-md);background:repeating-linear-gradient(135deg,var(--surface-sunk) 0 8px,var(--surface) 8px 16px)}
.rvcard .r2,.pa-review .r2{display:flex;flex-wrap:wrap;justify-content:space-between;gap:8px 16px;padding-top:12px;border-top:1px solid var(--line);font-size:13px;color:var(--text-3)}
.pa-review{display:grid;gap:12px}
.pa-review .q{font-size:16px;line-height:1.7;color:var(--text-1)}
@media (max-width:767px){ .form-wrap{padding:20px;border-radius:var(--r-lg)} .rvcard{padding:20px} }

/* ---------- partner ---------- */
.dark-strip.phead-dark{padding:56px 0 48px}
.dark-strip.phead-dark .g75{align-items:center}
.dark-strip .cta{display:flex;flex-wrap:wrap;align-items:center;gap:8px 20px;margin-top:28px}
.pa-kpi{overflow:hidden;padding:0}
.pa-kpi .bar{display:flex;justify-content:space-between;align-items:center;min-height:48px;padding:0 16px;border-bottom:1px solid var(--line);font:700 14px/1 var(--font-sans);color:var(--text-1)}
.pa-kpi .kpis,.mock .kpis{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));border-bottom:1px solid var(--line)}
.pa-kpi .kpis > div,.mock .kpis > div{padding:12px 16px;border-right:1px solid var(--line)}
.pa-kpi .kpis > div:last-child,.mock .kpis > div:last-child{border-right:0}
.pa-kpi .kpis .l,.mock .kpis .l{font-size:12px;font-weight:600;color:var(--text-3)}
.pa-kpi .kpis .v,.mock .kpis .v{margin-top:2px;font:700 24px/1.2 var(--font-sans);letter-spacing:-.02em;color:var(--text-strong)}
.pa-kpi .rowsm > div{display:grid;grid-template-columns:48px 56px minmax(0,1fr) auto;align-items:center;gap:10px;min-height:48px;padding:8px 16px;border-bottom:1px solid var(--line);font-size:14px;color:var(--text-2)}
.pa-kpi .rowsm > div:last-child{border-bottom:0}
.pa-kpi .rowsm b{font-weight:600;color:var(--text-1)}
.demo-frame{display:grid;gap:24px;padding:24px;border:1px dashed var(--line-ui);border-radius:var(--r-xl);background:var(--surface-sunk)}
.demo-grid{display:grid;grid-template-columns:minmax(0,1.4fr) minmax(0,1fr);align-items:start;gap:24px}
.mock{overflow:hidden;border:1px solid var(--line);border-radius:var(--r-md);background:var(--surface);box-shadow:var(--e2);font-size:14px;color:var(--text-2)}
.mock .bar{display:flex;align-items:center;gap:12px;min-height:48px;padding:0 16px;border-bottom:1px solid var(--line);background:var(--surface-tonal-soft)}
.mock .bar .dots{display:flex;gap:5px}
.mock .bar .dots i{display:block;width:8px;height:8px;border-radius:50%;background:var(--line-soft)}
.mock .bar .t{font:700 14px/1.3 var(--font-sans);color:var(--text-1)}
.mock .bar .demo{margin-left:auto}
.mock .tblwrap{overflow-x:auto}
.mock table{width:100%}
.mock th{padding:10px 16px;border-bottom:1px solid var(--line);background:var(--surface-sunk);text-align:left;white-space:nowrap;font:600 12px/1.3 var(--font-sans);letter-spacing:.02em;color:var(--text-3)}
.mock td{padding:12px 16px;border-bottom:1px solid var(--line);vertical-align:top;white-space:nowrap}
.mock tr:last-child td{border-bottom:0}
.mock td .nm,.mock .r .nm{font-weight:600;color:var(--text-1)}
.mock .sub{display:block;margin-top:2px;font-size:12px;color:var(--text-3)}
.mock .rows,.mock .fields{display:grid}
.mock .r{display:grid;grid-template-columns:minmax(0,1fr) auto auto;align-items:center;gap:16px;min-height:52px;padding:8px 16px;border-bottom:1px solid var(--line)}
.mock .r:last-child{border-bottom:0}
.mock .f{display:grid;grid-template-columns:96px minmax(0,1fr);align-items:center;gap:12px;min-height:48px;padding:8px 16px;border-bottom:1px solid var(--line)}
.mock .f .l{font-size:12px;font-weight:600;color:var(--text-3)}
.mock .f .v{color:var(--text-1)}
.mock .f .v.ph{color:var(--text-3)}
.mock .foot{display:flex;justify-content:flex-end;gap:8px;padding:12px 16px;border-top:1px solid var(--line);background:var(--surface-tonal-soft)}   /* site footer is footer.foot → no collision */
.mock .btn{--btn-h:34px;--btn-px:14px;--btn-fs:13px;pointer-events:none}      /* demo spans */
.pill{display:inline-flex;align-items:center;height:22px;padding:0 8px;border:1px solid var(--line-ui);border-radius:var(--r-pill);font:600 12px/1 var(--font-sans);color:var(--text-2);white-space:nowrap}
.pill.ink{border-color:var(--brand);background:var(--brand);color:var(--white)}
.pill.dim{border-style:dashed;color:var(--text-3)}
.sw-mini{position:relative;display:inline-block;width:32px;height:18px;border-radius:var(--r-pill);background:var(--surface);box-shadow:inset 0 0 0 1.5px var(--line-ui)}
.sw-mini::after{content:"";position:absolute;top:3px;left:3px;width:12px;height:12px;border-radius:50%;background:var(--text-3)}
.sw-mini.on{background:var(--brand);box-shadow:none}
.sw-mini.on::after{left:17px;background:var(--white)}
.demo-cap{font-size:14px;color:var(--text-2)}
@media (max-width:1023px){ .demo-grid{grid-template-columns:minmax(0,1fr)} }
@media (max-width:767px){
  .demo-frame{margin:0 calc(-1 * var(--gutter) + 4px);padding:12px;border-radius:var(--r-lg)}
  .pa-kpi .rowsm > div{grid-template-columns:44px 48px minmax(0,1fr) auto;font-size:13px}
}
```

### 3.8 Reveal, print — `@layer utilities`

Decoration may wait for the reveal; content may not. The `js-rv` class is added only when IntersectionObserver is available and reduced motion is off (J14). Without JS, everything renders in its final state.

```css
.js-rv [data-reveal]:not(.is-in) .hl{background-size:0% 100%}                                        /* highlighter sweep */
.js-rv [data-reveal]:not(.is-in) .rbar,
.js-rv [data-reveal]:not(.is-in) .mbar .t i,
.js-rv [data-reveal]:not(.is-in) .stackbar span{transform:scaleX(0)}                                  /* bars grow once */
.js-rv [data-reveal]:not(.is-in) .linechart{clip-path:inset(0 100% 0 0)}                              /* one-shot wipe */
.linechart{transition:clip-path var(--d-chart) var(--ease-out) 120ms}
.mbar .t i{transform-origin:0 50%;transition:transform var(--d-max) var(--ease-out) 160ms}
.rchart .rrow:nth-child(2) .rbar,.minis .mbar:nth-child(2) .t i{transition-delay:40ms}
.rchart .rrow:nth-child(3) .rbar,.minis .mbar:nth-child(3) .t i{transition-delay:80ms}
.rchart .rrow:nth-child(4) .rbar,.minis .mbar:nth-child(4) .t i{transition-delay:120ms}
.rchart .rrow:nth-child(5) .rbar,.minis .mbar:nth-child(5) .t i{transition-delay:160ms}
.rchart .rrow:nth-child(6) .rbar{transition-delay:200ms} .rchart .rrow:nth-child(7) .rbar{transition-delay:240ms} .rchart .rrow:nth-child(n+8) .rbar{transition-delay:280ms}
/* orbit chips bloom once (delays only on opacity/scale → hover lift has no delay) */
.js-rv .kv-hero[data-reveal]:not(.is-in) .orbit .oc{opacity:0;scale:.88}
.js-rv .kv-hero .orbit .oc:focus-visible{opacity:1;scale:1}
.orbit .oc:nth-child(2){transition-delay:0s,0s,40ms,40ms}  .orbit .oc:nth-child(3){transition-delay:0s,0s,80ms,80ms}
.orbit .oc:nth-child(4){transition-delay:0s,0s,120ms,120ms} .orbit .oc:nth-child(5){transition-delay:0s,0s,160ms,160ms}
.orbit .oc:nth-child(6){transition-delay:0s,0s,200ms,200ms} .orbit .oc:nth-child(7){transition-delay:0s,0s,240ms,240ms}
.orbit .oc:nth-child(8){transition-delay:0s,0s,280ms,280ms}
@media print{
  .nav,.tabbar,.cbar,.toast,.overlay{display:none!important}
  body,.ccard,footer.foot{background-image:none}
}
```

`@media print` uses `!important` only to beat the fixed-position chrome. Placing it in utilities is acceptable because it is print-only; alternatively, move it into `reset`.

---

## 4. Markup changes

**Order of work:** apply M1–M24 first, then the global M25, then the §5 JS edits.

**Line numbers** refer to the current file. The file uses CRLF endings; match on the text, not on the endings.

The "JS touch" column says which JS-coupled selectors each edit affects. Every one is either none or additive: no id, class or `data-*` attribute that JS reads is renamed or removed.

**M1 — Document head** (lines 1–6). JS touch: none.

OLD:
```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>한방에</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;900&display=swap">
<style>
```
NEW:
```html
<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#f4f6f1">
<title>한방에</title>
<link rel="preload" href="fonts/WantedSansVariable.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;900&display=swap&text=%ED%95%9C%EB%B0%A9%EC%97%90.">
<style>
```

`text=` subsets Noto to the 4 glyphs "한방에.". Noto is used only by `.logo`, `.wm` and `.seal-wm`.

**M2 — Replace the style block and open the body.**
- Replace lines 7–926 (everything between `<style>` and `</style>`) with the §1–§3 stylesheet.
- Line 927: OLD `</style>` → NEW `</style>\n</head>\n<body>`.
- JS touch: none. Quirks mode becomes standards mode; QA tables, mocks and sheets afterwards.

**M3 — Close the document.**
- The last line, 2415 `</script>` (the inline script, not the `photos.js` include), becomes `</script>\n</body>\n</html>`.
- JS touch: none.

**M4 — Nav right** (lines 943–946). JS touch: `#regionBtn`, `#regionLabel`, `[data-auth]`, `#menuBtn` are all kept.

OLD:
```html
      <button class="btn ghost-dark" id="regionBtn" type="button" aria-label="지역 선택으로 이동"><span id="regionLabel">전체 지역</span><svg class="i" viewBox="0 0 24 24" style="width:16px;height:16px"><path d="M6 9l6 6 6-6"/></svg></button>
      <button class="tlink on-dark" type="button" data-auth>로그인</button>
      <button class="btn on-dark" type="button" data-auth>회원가입</button>
      <button class="hamb" id="menuBtn" type="button" aria-label="메뉴 열기" aria-expanded="false" aria-controls="mobileMenu"><svg class="i" viewBox="0 0 24 24"><path d="M4 7h16M4 12h16M4 17h16"/></svg></button>
```
NEW (the visible label is part of the accessible name, per WCAG 2.5.3):
```html
      <button class="btn ghost sm region-btn" id="regionBtn" type="button"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 22s7-6.5 7-12a7 7 0 1 0-14 0c0 5.5 7 12 7 12z"/><circle cx="12" cy="10" r="2.5"/></svg><span class="sr">지역 선택: </span><span id="regionLabel">전체 지역</span></button>
      <button class="tlink" type="button" data-auth>로그인</button>
      <button class="btn primary sm" type="button" data-auth>회원가입</button>
      <button class="hamb" id="menuBtn" type="button" aria-label="메뉴 열기" aria-expanded="false" aria-controls="mobileMenu"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M4 7h16M4 12h16M4 17h16"/></svg></button>
```

**M5 — Mobile menu auth** (line 955). JS touch: `[data-auth]` kept.

OLD: `    <div class="auth"><button class="btn ghost-dark" type="button" data-auth>로그인</button><button class="btn on-dark" type="button" data-auth>회원가입</button></div>`

NEW: `    <div class="auth"><button class="btn ghost" type="button" data-auth>로그인</button><button class="btn primary" type="button" data-auth>회원가입</button></div>`

**M6 — Home 문진 block and new trust strip** (replace lines 962–998, from `    <section class="chat-wrap home-chat" id="homeChat" aria-labelledby="chatPageTitle">` through the `    </section>` that ends it).

JS touch: every id is kept:
- `#homeChat`, `#chatPageTitle`
- `#ccardBg`, `#ccard`, `#ccardDate`, `#ccardDots`, `#cfKind`, `#miniBody`, all `#cf*`, `#ccardSeal`
- `#chatStep`, `#chatBar`, `#chatRestart`, `#chat`

Every class is kept: `.chat-wrap`, `.home-chat`, `.chat-grid`, `.chat-side`, `.ccard*`, `.cf*`, `.chat-progress .bar`, `.chat`.

`#chatStep`, `#chatBar` and `#chatRestart` move into the panel header; JS reaches them by id. `aria-live` is removed from `#ccard`, so the chat log is the only live region.

New and additive: `.chat-mast`, `.chat-panel`, `.chat-top`, `.safety`, `#logToggle`, `.trust`, `#trustClinics`, `#trustReviews`.

The copy (eyebrow, h1, lead, safety sentence, 처음부터 다시 →) is unchanged.

NEW:
```html
    <section class="chat-wrap home-chat" id="homeChat" aria-labelledby="chatPageTitle">
      <div class="wrap">
        <header class="chat-mast">
          <p class="eyebrow">교통사고 한의원 비교·예약 · 30초 문진</p>
          <h1 class="display" id="chatPageTitle" tabindex="-1"><span class="k">교통사고 처리도,</span><br><span class="wm"><b>한방</b><i>에</i><span class="dot">.</span></span></h1>
          <p class="lead">어디가 아픈지, 교통사고인지부터 답만 누르면 받을 치료와 가까운 한의원이 정리됩니다. 교통사고면 보험 처리와 예상 합의금 범위까지.</p>
        </header>
        <div class="chat-grid">
          <aside class="chat-side" aria-label="내 문진 카드">
            <div class="ccard-wrap">
              <img class="ccard-bg" id="ccardBg" alt="" aria-hidden="true">
              <div class="ccard" id="ccard" role="group" aria-label="문진 카드">
                <div class="ccard-h"><span class="wm"><b>한방</b><i>에</i><span class="dot">.</span></span><span class="ccard-no num">문진 카드 · <span id="ccardDate"></span></span></div>
                <div class="ccard-row"><div class="ccard-dots" id="ccardDots" aria-hidden="true"></div><span class="ccard-kind" id="cfKind" hidden></span></div>
                <div class="ccard-grid">
                  <div class="cf cf-body"><span class="cl">다친 부위</span><div class="mini-body" id="miniBody"></div><span class="cv" id="cfPart">—</span></div>
                  <div class="cf" data-flow="acc"><span class="cl">사고 경위</span><span class="cv" id="cfAcc">—</span></div>
                  <div class="cf" data-flow="gen"><span class="cl">원인</span><span class="cv" id="cfCause">—</span></div>
                  <div class="cf"><span class="cl">경과</span><div class="tl-dots" id="cfWhen"><i data-v="today"></i><i data-v="yday"></i><i data-v="days"></i><i data-v="week"></i></div><span class="cv small" id="cfWhenT">—</span></div>
                  <div class="cf"><span class="cl">통증</span><div class="gauge" id="cfPain"><i></i><i></i><i></i></div><span class="cv small" id="cfPainT">—</span></div>
                  <div class="cf" data-flow="acc"><span class="cl">보험 접수</span><span class="cv" id="cfIns">—</span></div>
                  <div class="cf" data-flow="gen"><span class="cl">실손보험</span><span class="cv small" id="cfHins">—</span></div>
                  <div class="cf"><span class="cl">지역</span><span class="cv" id="cfRegion">—</span></div>
                  <div class="cf" data-flow="acc"><span class="cl">차량 파손</span><span class="cv small" id="cfDmg">—</span></div>
                  <div class="cf" data-flow="acc"><span class="cl">보험사</span><span class="cv small" id="cfInsr">—</span></div>
                  <div class="cf" data-flow="acc"><span class="cl">진단</span><span class="cv small" id="cfDx">—</span></div>
                  <div class="cf" data-flow="acc"><span class="cl">입원</span><span class="cv small" id="cfStay">—</span></div>
                  <div class="cf cf-wide" data-flow="gen"><span class="cl">동반 증상</span><span class="cv small" id="cfSym">—</span></div>
                  <div class="cf cf-wide"><span class="cl">조건</span><span class="cv" id="cfCond">—</span></div>
                  <div class="cf cf-wide cf-est" id="cfEstWrap" data-flow="acc" hidden><span class="cl">예상 합의금 · 예시</span><span class="cv" id="cfEst">—</span><span class="meta" id="cfEstSub"></span></div>
                </div>
                <div class="seal" id="ccardSeal" hidden><span class="seal-in"><span class="seal-wm"><b>한방</b><i>에</i></span><small>추천 완료</small></span></div>
              </div>
            </div>
          </aside>
          <div class="chat-panel">
            <div class="chat-top">
              <span class="chat-top-t"><span class="av" aria-hidden="true">한</span>빠른 문진</span>
              <span class="chat-progress" aria-hidden="true"><span class="bar"><i id="chatBar"></i></span></span>
              <span class="meta num" id="chatStep">0 / 12</span>
              <button class="tlink muted" type="button" id="chatRestart">처음부터 다시 →</button>
            </div>
            <p class="safety"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 3.5l9 16H3z"/><path d="M12 10v4.5M12 17.5v.01"/></svg>진단이 아니라 안내입니다. 어지럼·구토·마비·의식 저하가 있으면 문진보다 응급실이 먼저입니다.</p>
            <button class="log-toggle tlink muted" id="logToggle" type="button" aria-expanded="false" aria-controls="chat">내 답변 다시 보기</button>
            <div class="chat" id="chat" role="log" aria-live="polite" aria-label="문진 대화"></div>
          </div>
        </div>
      </div>
    </section>
    <section class="trust" aria-label="한방에가 보여주는 정보의 기준">
      <div class="wrap trust-in">
        <ul class="trust-list">
          <li><svg class="pic" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 22s7-6.5 7-12a7 7 0 1 0-14 0c0 5.5 7 12 7 12z"/><circle cx="12" cy="10" r="2.5"/></svg><div><b class="num" id="trustClinics">입점 한의원</b><span>실제 주소·전화 기준 · 미확인 항목은 "확인 중" 표시</span></div></li>
          <li><svg class="pic" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 3l8 3v6c0 5-3.5 8-8 9-4.5-1-8-4-8-9V6z"/><path d="M9 12l2 2 4-4"/></svg><div><b>금융감독원 표준약관 기준선</b><span>위자료·통원 교통비 기준은 전 보험사 동일</span></div></li>
          <li><svg class="pic" viewBox="0 0 24 24" aria-hidden="true"><path d="M3 20h18M6 20v-6M11 20V8M16 20v-9"/></svg><div><b>손해보험협회 공시값 인용</b><span>부지급률 2025년 하반기 · 민원 2026년 2분기</span></div></li>
          <li><svg class="pic" viewBox="0 0 24 24" aria-hidden="true"><path d="M4 5h16v11H9l-5 4z"/><path d="M8 9h8M8 12h5"/></svg><div><b>진료 확인 후기만 게시</b><span id="trustReviews">첫 후기를 기다리고 있어요</span></div></li>
        </ul>
        <p class="legend-honest"><span class="lbl">표시 기준</span><span class="tag demo">예시</span>실제 통계가 아닌 설명용 수치<span class="tag demo">데모</span>AI로 만든 사진<span class="tag ask">확인 중</span>한의원이 아직 확인하지 않은 항목</p>
      </div>
    </section>
```

**M7 — KV hero: stage wrapper** (line 1000). JS touch: none.

OLD: `  <div class="wrap hero-grid">`

NEW: `  <div class="wrap"><div class="hero-grid">`

**M8 — KV hero: fix the circular CTA and move it below the chips.** JS touch: `#hotChips`, `#kvImg`, `#orbit` kept.

(a) Delete line 1005 entirely:
`      <div class="hero-cta"><a class="btn primary lg" href="#/chat"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M4 5h16v11H9l-5 4z"/><path d="M8 9h8M8 12h5"/></svg>30초 문진으로 한의원 추천 받기</a><span class="meta">질문 몇 개만 고르면 치료·한의원·처리 순서를 정리해 드립니다</span></div>`

(b) Line 1012.

OLD: `      <div class="chips" id="hotChips" aria-label="치료 항목과 조건으로 바로 찾기"></div>`

NEW:
```html
      <div class="chips" id="hotChips" role="group" aria-label="치료 항목과 조건으로 바로 찾기"></div>
      <div class="hero-cta"><a class="btn ghost lg" href="#/find"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 22s7-6.5 7-12a7 7 0 1 0-14 0c0 5.5 7 12 7 12z"/><circle cx="12" cy="10" r="2.5"/></svg>지역별 한의원 바로 찾기</a><a class="tlink" href="#/chat">30초 문진으로 한의원 추천 받기 →</a></div>
```

(c) Lines 1015–1020.

OLD:
```html
    <div class="kv" aria-label="한방에가 지원하는 것">
      <img id="kvImg" alt="" aria-hidden="true">
      <div class="orbit" id="orbit"></div>
    </div>
  </div>
</section>
```
NEW:
```html
    <div class="kv" role="group" aria-label="한방에가 지원하는 것">
      <img id="kvImg" alt="" aria-hidden="true" width="520" height="520" decoding="async">
      <div class="orbit" id="orbit"></div>
    </div>
  </div></div>
</section>
```

**M9 — Home section order and classes.** Move whole `<section>` blocks verbatim; no JS depends on DOM order.

The new children of `<div class="page" data-page="home">`, in this order:

1. `#homeChat` (M6)
2. `.trust` (M6)
3. `.hero.kv-hero`
4. `#home-glance`
5. `#home-body`
6. `#program`
7. `<div class="band-wash">` containing `#home-data` then `#home-tips`
8. `#home-steps`
9. `#home-why`
10. `#home-find`

Class edits:

| Section | OLD | NEW |
|---|---|---|
| `#home-glance` | `<section class="sec rule" id="home-glance" aria-labelledby="glanceTitle">` | `<section class="sec" id="home-glance" aria-labelledby="glanceTitle">` |
| `#home-body` | `<section class="sec rule" id="home-body" aria-labelledby="bodyTitle">` | `<section class="sec band-surface" id="home-body" aria-labelledby="bodyTitle">` |
| `#home-tips` | `<section class="sec rule tips" id="home-tips" aria-labelledby="tipsTitle">` | `<section class="sec tips" id="home-tips" aria-labelledby="tipsTitle">` |
| `#home-steps` | `<section class="sec rule" id="home-steps" aria-labelledby="homeStepsTitle">` | `<section class="sec" id="home-steps" aria-labelledby="homeStepsTitle">` |
| `#home-why` | `<section class="sec rule" id="home-why" aria-labelledby="whyTitle">` | `<section class="sec band-surface" id="home-why" aria-labelledby="whyTitle">` |

The band wrapper is a new `<div class="band-wash">` opened immediately before `<section class="sec" id="home-data" …>` and a `</div>` closed immediately after the `</section>` of `#home-tips`.

JS touch: none. `#stepsHome`, `#lineChart`, `#settleChart`, `#stackBar`, `#bodySvg`, `#catIndex`, `#catOpen`, `#catRail`, `#catPanel`, `#homeFindStats`, `#glanceFind` and `#glanceReviews` are all looked up by id. `#program` remains the anchor target.

**M10 — Data section: heading levels, demo tags and the chart table.** JS touch: `#miniChronic`, `#miniOnset`, `#miniPart` and `#lineChart` are kept. No CSS depends on `.mini h4` or `.tl h4` any more.

Line 1034: OLD `            <div class="mini"><h4>만성 통증(후유증)으로 이어진 비율 <span class="meta">예시</span></h4><div id="miniChronic"></div></div>` → NEW `            <div class="mini"><h3>만성 통증(후유증)으로 이어진 비율 <span class="tag demo">예시</span></h3><div id="miniChronic"></div></div>`

Line 1035: OLD `            <div class="mini"><h4>통증이 시작되는 시점 <span class="meta">예시</span></h4><div id="miniOnset"></div></div>` → NEW `            <div class="mini"><h3>통증이 시작되는 시점 <span class="tag demo">예시</span></h3><div id="miniOnset"></div></div>`

Line 1036: OLD `            <div class="mini"><h4>많이 호소하는 부위 <span class="meta">예시</span></h4><div id="miniPart"></div></div>` → NEW `            <div class="mini"><h3>많이 호소하는 부위 <span class="tag demo">예시</span></h3><div id="miniPart"></div></div>`

Lines 1041–1044: in each of the four `.tl` children, change `<h4>` to `<h3>` and `</h4>` to `</h3>`. The four headings are 급성기, 염증·긴장 완화, 가동 범위 회복 and 만성화 예방 · 합의 준비; each is unique.

Line 1030: keep the line and insert the following immediately after it. The values are copied from the JS arrays A, Bv and C.

OLD: `            <div class="lg"><span>통원 치료 (주 2~3회)</span><span class="s2">입원 뒤 통원 전환</span><span class="s3">치료 없이 방치</span></div>`

NEW (same line, followed by):
```html
            <details class="chart-table"><summary>표로 보기</summary><div class="ins-wrap"><table class="ins"><caption class="sr">치료 여부에 따른 통증 점수(VAS 0~10) 예시</caption><thead><tr><th>시점</th><th>통원 치료</th><th>입원 뒤 통원</th><th>치료 없이 방치</th></tr></thead><tbody>
              <tr><td>사고 직후</td><td class="num">7.5</td><td class="num">8.5</td><td class="num">7.5</td></tr><tr><td>2주</td><td class="num">6.0</td><td class="num">5.5</td><td class="num">6.6</td></tr><tr><td>4주</td><td class="num">4.5</td><td class="num">3.8</td><td class="num">6.0</td></tr><tr><td>6주</td><td class="num">3.2</td><td class="num">2.6</td><td class="num">5.7</td></tr><tr><td>8주</td><td class="num">2.2</td><td class="num">1.8</td><td class="num">5.9</td></tr><tr><td>10주</td><td class="num">1.6</td><td class="num">1.3</td><td class="num">6.4</td></tr><tr><td>12주</td><td class="num">1.2</td><td class="num">1.0</td><td class="num">6.9</td></tr>
            </tbody></table></div><p class="meta">예시값이며 실제 통계가 아닙니다.</p></details>
```

**M11 — Settlement chart semantics.** Changing `img` to `group` makes screen readers read the row values. The JS still finds both by id.

Line 1061: OLD `            <div class="rchart" id="settleChart" role="img" aria-label="치료 형태별 합의금 범위 예시 차트"></div>`

NEW: `            <div class="rchart" id="settleChart" role="group" aria-label="치료 형태별 합의금 범위 예시(만원): 통원 1주 이내 30~80, 통원 2~3주 60~150, 통원 4주 이상 100~250, 입원 3~7일 + 통원 150~300, 입원 2주 이상 250~500"></div>`

Line 1224: apply the same change to `id="settleChart2"`. OLD: `            <div class="rchart" id="settleChart2" role="img" aria-label="치료 형태별 합의금 범위 예시 차트"></div>`

**M12 — Stack bar.** No change; its `role="img"` with the values spelled out is already correct.

**M13 — Hotspot hit area** (line 1090). In that line only, replace all 10 occurrences of `r="95"` with `r="100"`.

At a 260px render this gives a target of about 34px with no overlap. The head, neck and chest centres are 220–240 units apart and the legs and feet are 200 apart; overlap would need a radius above 100. The 44px `#partList` buttons are the equivalent control (WCAG 2.5.8).

JS touch: none. The JS reads only `.hs .dot` `cx`/`cy`.

**M14 — Home steps side note** (line 1113). JS touch: none.

OLD: `<a class="btn ghost" href="#/guide" style="margin-top:16px">보험 가이드 보기</a>`

NEW: `<a class="btn ghost" href="#/guide">보험 가이드 보기</a>`

**M15 — The 한의원 찾기 band** (replace lines 1148–1153). JS touch: `#homeFindStats` kept. The new id `#homeRegions` is filled by J9.

OLD:
```html
    <section class="sec rule" id="home-find" aria-labelledby="homeFindTitle">
      <div class="wrap g75">
        <div><p class="eyebrow">한의원 찾기</p><h2 class="h-md" id="homeFindTitle" style="margin-top:12px">지역별 입점 한의원을 비교하고 예약합니다.</h2><p class="lead" style="margin-top:16px;max-width:30em">시·구를 고르고 치료 항목과 조건으로 걸러 보세요. 최대 3곳을 비교함에 담아 한 표로 볼 수 있습니다.</p><a class="btn primary" href="#/find" style="margin-top:32px">한의원 찾기</a></div>
        <dl class="spec" id="homeFindStats"></dl>
      </div>
    </section>
```
NEW:
```html
    <section class="sec" id="home-find" aria-labelledby="homeFindTitle">
      <div class="wrap">
        <div class="find-band band-ink">
          <div class="find-cta">
            <p class="eyebrow">한의원 찾기</p>
            <h2 class="h-md" id="homeFindTitle">지역별 입점 한의원을 <span class="hl">비교하고 예약</span>합니다.</h2>
            <p class="lead">시·구를 고르고 치료 항목과 조건으로 걸러 보세요. 최대 3곳을 비교함에 담아 한 표로 볼 수 있습니다.</p>
            <div class="chips" id="homeRegions" role="group" aria-label="지역으로 바로 찾기"></div>
            <a class="btn on-dark lg" href="#/find">한의원 찾기</a>
          </div>
          <dl class="spec" id="homeFindStats"></dl>
        </div>
      </div>
    </section>
```

**M16 — Highlighter phrases.** Wrap exactly the listed phrase in `<span class="hl">…</span>`; the rest of the text is unchanged. The existing JS pass (lines 2378–2379) skips any heading that already contains `.hl`. JS touch: `.hl`, additive.

| Heading id | Phrase to wrap |
|---|---|
| dataTitle | 통증은 돌아옵니다 |
| tipsTitle | 네 가지 |
| bodyTitle | 받을 수 있는 치료 |
| glanceTitle | 할 수 있는 것 |
| homeStepsTitle | 세 단계 |
| programTitle | 어떤 치료 |
| whyTitle | 한의원 치료 |
| homeFindTitle | 비교하고 예약 (done in M15) |
| guideInsTitle | 다른 건 태도 |
| guideSettleTitle | 치료가 끝난 뒤 |
| guideTableTitle | 자동차보험으로 |
| guidePrepTitle | 필요한 것 |
| reviewsDemoTitle | 이렇게 |
| partnerDemoTitle | 한 화면에서 |
| partnerStepsTitle | 세 단계 |
| findPageTitle | 입점 한의원 |
| guidePageTitle | 이것만 |
| reviewsPageTitle | 다음 환자의 선택 |
| partnerTitle | 예약 창구 (done in M20) |

Example for dataTitle: OLD `id="dataTitle">제대로 치료하지 않으면, 통증은 돌아옵니다.</h2>` → NEW `id="dataTitle">제대로 치료하지 않으면, <span class="hl">통증은 돌아옵니다</span>.</h2>`

**M17 — Find page** (line 1162, and lines 1169–1171). JS touch: `#regionCloud`, `#cityTabs`, `#distChips`, `#filterRow`, `#resCount`, `#sortSel`, `#list` and `#principles` are unchanged; they are only wrapped. `$('#cityTabs [aria-selected="true"]')` still works.

(a) Line 1162. OLD fragment `<div class="pa-card pa-map" id="regionCloud" aria-label="지역별 입점 한의원 수"></div>` → NEW `<div class="pa-card pa-map" id="regionCloud" role="group" aria-label="지역별 입점 한의원 수"></div>`

(b) Lines 1169–1171.

OLD:
```html
      <div class="ctabs" role="tablist" aria-label="지역" id="cityTabs"></div>
      <div class="chips dist" id="distChips"></div>
      <div class="filters" id="filterRow" aria-label="필터"></div>
```
NEW:
```html
      <div class="find-controls">
        <div class="ctabs" role="tablist" aria-label="지역" id="cityTabs"></div>
        <div class="chips dist" id="distChips" role="group" aria-label="구 선택"></div>
        <div class="filters" id="filterRow" role="group" aria-label="치료·조건 필터"></div>
      </div>
      <p class="find-note" role="note"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="12" r="8.5"/><path d="M12 11v5M12 8v.01"/></svg><span><b>확인 중</b>은 한의원이 아직 직접 확인하지 않은 항목입니다. 전화로 먼저 물어보실 수 있어요.</span><a class="tlink" href="#principles">표시 원칙 보기</a></p>
```

**M18 — Guide TOC** (lines 1198–1199, which are unique because of `mini-kv`). JS touch: none; the section ids already exist, and J7 and J14 handle the offset and scrollspy.

OLD:
```html
  <div class="mini-kv"><span>추나 · 약침 · 한약 · 물리치료 · 입원</span><span class="tag yes">자보 적용</span></div>
</div></div></div></section>
```
NEW:
```html
  <div class="mini-kv"><span>추나 · 약침 · 한약 · 물리치료 · 입원</span><span class="tag yes">자보 적용</span></div>
</div></div></div></section>
    <nav class="toc" aria-label="보험 가이드 목차"><div class="wrap"><ol>
      <li><a href="#guide-insurers">보험사 지표</a></li><li><a href="#guide-settle">합의 요령</a></li><li><a href="#guide-full">접수 3단계</a></li><li><a href="#guide-table">적용 항목</a></li><li><a href="#guide-prep">접수 준비</a></li>
    </ol></div></nav>
```

**M19 — Review form.** JS touch: `#rvClinic`, `#rvText`, `#rvAgree` kept. The new `#rvAgreeErr` is used by J8.

(a) Line 1318 fragment: OLD `<select id="rvClinic">` → NEW `<select id="rvClinic" required aria-required="true">`

(b) Line 1326: OLD `<textarea id="rvText" placeholder="치료 과정, 설명의 친절함, 대기 시간, 보험 처리 경험 등을 200자 이상 써 주세요."></textarea>` → NEW `<textarea id="rvText" required aria-required="true" placeholder="치료 과정, 설명의 친절함, 대기 시간, 보험 처리 경험 등을 10자 이상 써 주세요."></textarea>`. This is the bug fix noted in §0.4-9.

(c) Line 1328.

OLD: `            <label class="cmp" style="height:auto;align-items:flex-start;gap:12px"><input type="checkbox" id="rvAgree"><span class="box" style="margin-top:2px"><svg class="i" viewBox="0 0 24 24"><path d="M5 12l5 5L20 7"/></svg></span><span class="body" style="font-size:14px">본인이 직접 진료받은 경험이며, 게시 원칙에 동의합니다.</span></label>`

NEW:
```html
            <label class="cmp" style="height:auto;align-items:flex-start;gap:12px"><input type="checkbox" id="rvAgree" aria-describedby="rvAgreeErr"><span class="box" style="margin-top:2px"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M5 12l5 5L20 7"/></svg></span><span class="body" style="font-size:14px">본인이 직접 진료받은 경험이며, 게시 원칙에 동의합니다.</span></label>
            <p class="help" id="rvAgreeErr" role="alert" hidden>게시 원칙에 동의해 주세요.</p>
```

**M20 — Partner page.** JS touch: route focus uses `.page[data-page] h1`, which now exists. The `.hl` pass targets `.dark-strip.phead-dark .display` and skips this heading because it has a hand-authored `.hl`. `#partnerFormEl` and the `#pt*` ids are kept.

(a) Line 1368: OLD `      <h2 class="display" id="partnerTitle">교통사고 환자를 위한 예약 창구를 함께 만듭니다.</h2>` → NEW `      <h1 class="display" id="partnerTitle" tabindex="-1">교통사고 환자를 위한 <span class="hl">예약 창구</span>를 함께 만듭니다.</h1>`

(b) Line 1370: OLD `      <div class="cta"><a class="btn on-dark" href="#partnerForm">입점 문의</a><a class="tlink on-dark" href="#/guide">보험 적용 표 보기 →</a></div>` → NEW `      <div class="cta"><a class="btn primary lg" href="#partnerForm">입점 문의</a><a class="tlink" href="#/guide">보험 적용 표 보기 →</a></div>`

(c) Line 1372 fragment: OLD `<div class="pa-card pa-kpi" aria-label="파트너 화면 예시">` → NEW `<div class="pa-card pa-kpi" role="group" aria-label="파트너 화면 예시">`

(d) Lines 1389, 1406 and 1419: add `role="group"` to each `<div class="mock" aria-label="…">`, for example `<div class="mock" role="group" aria-label="예약 관리 화면 데모">`.

(e) Lines 1450, 1451, 1454 and 1455:

| Line | OLD | NEW |
|---|---|---|
| 1450 | `<input id="ptName" placeholder="OO한의원">` | `<input id="ptName" required aria-required="true" autocomplete="organization" placeholder="OO한의원">` |
| 1451 | `<input id="ptPerson" placeholder="원장 · 실장">` | `<input id="ptPerson" required aria-required="true" autocomplete="name" placeholder="원장 · 실장">` |
| 1454 | `<input id="ptTel" type="tel" inputmode="tel" placeholder="02-000-0000">` | `<input id="ptTel" type="tel" inputmode="tel" required aria-required="true" autocomplete="tel" placeholder="02-000-0000">` |
| 1455 | `<input id="ptRegion" placeholder="서울 서초구">` | `<input id="ptRegion" autocomplete="address-level2" placeholder="서울 서초구">` |

The three `<div class="foot">` inside the mocks stay as they are, because the CSS scopes the site footer as `footer.foot`.

**M21 — Footer link** (line 1473). JS touch: none; the `#/cat/<slug>` route already exists.

OLD fragment: `<a href="#/">치료 프로그램</a>` → NEW `<a href="#/cat/chuna">치료 프로그램</a>`

**M22 — Tabbar** (lines 1497–1501). This fixes the dead 홈 tab: `top` becomes `home`, which the existing handler (line 2000) and `route()` already expect. JS touch: `#tabbar [data-tab]` and `#tabDot` are kept. `.dtabs [data-tab]` lives in a different scope and is unaffected.

OLD:
```html
  <button type="button" class="on" data-tab="top"><svg class="i" viewBox="0 0 24 24"><path d="M3 11l9-8 9 8v10a1 1 0 0 1-1 1h-5v-7H9v7H4a1 1 0 0 1-1-1z"/></svg>홈</button>
  <button type="button" data-tab="find"><svg class="i" viewBox="0 0 24 24"><path d="M12 22s7-6.5 7-12a7 7 0 1 0-14 0c0 5.5 7 12 7 12z"/><circle cx="12" cy="10" r="2.5"/></svg>찾기</button>
  <button type="button" data-tab="compare"><svg class="i" viewBox="0 0 24 24"><rect x="3" y="4" width="7" height="16" rx="1"/><rect x="14" y="4" width="7" height="16" rx="1"/></svg>비교<span class="dot" id="tabDot" hidden></span></button>
  <button type="button" data-tab="chat"><svg class="i" viewBox="0 0 24 24"><path d="M4 5h16v11H9l-5 4z"/><path d="M8 9h8M8 12h5"/></svg>문진</button>
  <button type="button" data-tab="my"><svg class="i" viewBox="0 0 24 24"><circle cx="12" cy="8" r="4"/><path d="M4 21a8 8 0 0 1 16 0"/></svg>MY</button>
```
NEW:
```html
  <button type="button" class="on" data-tab="home"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M3 11l9-8 9 8v10a1 1 0 0 1-1 1h-5v-7H9v7H4a1 1 0 0 1-1-1z"/></svg><span>홈</span></button>
  <button type="button" data-tab="find"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 22s7-6.5 7-12a7 7 0 1 0-14 0c0 5.5 7 12 7 12z"/><circle cx="12" cy="10" r="2.5"/></svg><span>찾기</span></button>
  <button type="button" data-tab="compare"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><rect x="3" y="4" width="7" height="16" rx="1"/><rect x="14" y="4" width="7" height="16" rx="1"/></svg><span>비교</span><span class="dot" id="tabDot" hidden></span></button>
  <button type="button" data-tab="chat"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M4 5h16v11H9l-5 4z"/><path d="M8 9h8M8 12h5"/></svg><span>문진</span></button>
  <button type="button" data-tab="my"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="8" r="4"/><path d="M4 21a8 8 0 0 1 16 0"/></svg><span>MY</span></button>
```

**M23 — Guide side note.** No change; it keeps its inline margin in its vertical layout.

**M24 — Modal accessible name.** No markup change is needed; J5 `nameDialog()` sets `aria-labelledby` on `#sheet` and `#modal` from their `.sheet-h .t`.

**M25 — Decorative SVGs (global, run after M1–M24).** Between `<body>` and `<script src="photos.js">`, replace every remaining `<svg class="i" viewBox="0 0 24 24">` (the form without `aria-hidden`) with `<svg class="i" viewBox="0 0 24 24" aria-hidden="true">`. That is 7 occurrences: the 5 glance chevrons (lines 1102–1106) and the 2 review `.selwrap` chevrons (lines 1318 and 1321). The two JS-template occurrences (lines 2207 and 2286) are handled in J10-C8 and J10-C13. JS touch: none.

---

## 5. JS changes (small, exact old → new)

Every JS-coupled selector from the inventory is preserved. Line numbers refer to the current file.

**J1 — Toast duration** (line 1632). OLD fragment `toastT = setTimeout(() => t.hidden = true, 2400);` → NEW `toastT = setTimeout(() => t.hidden = true, 4000);`

**J2 — `route()`: `aria-current`, and in-page `#/cat/…` links now scroll.**

(a) Line 1671. Keep the line and insert a new line right after it.

OLD: `  if (!changed) { $$('.menu a, .mmenu a').forEach(a => a.classList.toggle('on', a.getAttribute('href') === (navKey === 'home' ? '#/' : '#/' + navKey))); $$('#tabbar [data-tab]').forEach(b => b.classList.toggle('on', b.dataset.tab === navKey)); }`

NEW (the same line, followed by):
```js
  $$('.menu a, .mmenu a, #tabbar [data-tab]').forEach(n => n.classList.contains('on') ? n.setAttribute('aria-current', 'page') : n.removeAttribute('aria-current'));
```

(b) Line 1673. OLD fragment: `if (changed) requestAnimationFrame(() => { const y = $('#catRail').getBoundingClientRect().top + window.scrollY - navH() - 8; window.scrollTo({ top: y }); });`

NEW: `requestAnimationFrame(() => { const r = $('#catRail').getBoundingClientRect(); if (changed || r.top < navH() || r.top > innerHeight * .6) window.scrollTo({ top: r.top + window.scrollY - navH() - 8, behavior: changed || matchMedia('(prefers-reduced-motion: reduce)').matches ? 'auto' : 'smooth' }); });`

**J3 — `data-open` collision.** `accRow()` emits `div.acc-row[data-open]`, so opening a FAQ inside the panel currently calls `openDetail(NaN)`. Replace all 3 occurrences of `e.target.closest('[data-open]')` (lines 1786, 1845 and 2364) with `e.target.closest('button[data-open]')`.

**J4 — 예약 is the primary action.**
- Replace all 3 occurrences of `class="btn ghost sm" type="button" data-book=` (lines 1709, 1824 and 1879) with `class="btn primary sm" type="button" data-book=`.
- Line 1709 fragment: OLD `<button class="tlink" type="button" data-open="${c.id}">상세</button>` → NEW `<button class="btn ghost sm" type="button" data-open="${c.id}">상세</button>`.

**J5 — Overlays: accessible name, inert background, focus trap; close stays synchronous** (lines 1886–1887, then an insert after 1889).

OLD:
```js
function openOverlay(sel) { lastFocus = document.activeElement; const bg = $(sel); bg.hidden = false; document.body.style.overflow = 'hidden'; $('[data-x]', bg)?.focus(); }
function closeOverlay(sel) { const bg = $(sel); bg.hidden = true; if ($('#sheetBg').hidden && $('#modalBg').hidden) document.body.style.overflow = ''; $('.sheet, .modal', bg).innerHTML = ''; lastFocus?.focus?.({ preventScroll: true }); }
```
NEW:
```js
const bgNodes = () => ['body > header', 'body > main', 'body > footer', '#tabbar', '#cbar'].map(s => $(s)).filter(Boolean);
function nameDialog(bg) { const d = $('.sheet, .modal', bg), t = d && $('.sheet-h .t', d); if (!t) return; t.id = d.id + 'Title'; d.setAttribute('aria-labelledby', t.id); }
function openOverlay(sel) { const bg = $(sel); if (bg.hidden) lastFocus = document.activeElement; bg.hidden = false; nameDialog(bg); document.body.style.overflow = 'hidden'; bgNodes().forEach(n => { n.inert = true; }); $('[data-x]', bg)?.focus(); }
function closeOverlay(sel) { const bg = $(sel); bg.hidden = true; if ($('#sheetBg').hidden && $('#modalBg').hidden) { document.body.style.overflow = ''; bgNodes().forEach(n => { n.inert = false; }); } $('.sheet, .modal', bg).innerHTML = ''; lastFocus?.focus?.({ preventScroll: true }); }
```

Insert after line 1889 (the Escape keydown listener):
```js
document.addEventListener('keydown', e => { if (e.key !== 'Tab') return; const bg = [$('#modalBg'), $('#sheetBg')].find(b => !b.hidden); if (!bg) return;
  const f = $$('a[href], button:not([disabled]), input:not([type="hidden"]), select, textarea, [tabindex]:not([tabindex="-1"])', bg).filter(x => x.offsetParent !== null); if (!f.length) return;
  const first = f[0], last = f[f.length - 1], inside = bg.contains(document.activeElement);
  if (e.shiftKey && (document.activeElement === first || !inside)) { e.preventDefault(); last.focus(); } else if (!e.shiftKey && (document.activeElement === last || !inside)) { e.preventDefault(); first.focus(); } });
```

**J6 — Booking: inline errors, and naming the done state.**

(a) Line 1968. OLD: `    [['#bName'], ['#bTel']].forEach(([s]) => { const el = $(s, f), help = el.parentElement.querySelector('.help'); const bad = !el.value.trim(); help.hidden = !bad; if (bad && ok) { el.focus(); ok = false; } });` → NEW: `    ok = validate(f, [['#bName', el => el.value.trim()], ['#bTel', el => el.value.trim()]]);`

(b) Line 1987. OLD: `    $('#sheet [data-cal]').addEventListener('click', () => toast('실서비스에서는 캘린더 파일이나 카카오 알림으로 연결됩니다'));` → NEW: `    $('#sheet [data-cal]').addEventListener('click', () => toast('실서비스에서는 캘린더 파일이나 카카오 알림으로 연결됩니다')); nameDialog($('#sheetBg')); $('#sheet [data-x]')?.focus();`

**J7 — Anchor offset for the TOC and result nav, plus the nav scroll shadow.**

(a) Line 1997. OLD: `  if (!href.startsWith('#/')) { const t = $(href); if (t) { e.preventDefault(); smoothTo(t, navH() + 16); } }` → NEW: `  if (!href.startsWith('#/')) { const t = $(href); if (t) { e.preventDefault(); const toc = $('.page:not([hidden]) .toc'); smoothTo(t, navH() + 16 + (toc ? toc.offsetHeight : 0) + (t.closest('.res') && innerWidth >= 768 ? 64 : 0)); } }`

(b) Insert after line 1999 (the `#menuBtn` listener):
```js
addEventListener('scroll', () => $('.nav').classList.toggle('scrolled', scrollY > 8), { passive: true });
```

**J8 — Form validation with ARIA, and an inline agreement error.**

(a) Line 2005. OLD: `function validate(form, pairs) { let ok = true; pairs.forEach(([sel, test]) => { const el = $(sel, form); const help = el.closest('.fld')?.querySelector('.help'); const bad = !test(el); if (help) help.hidden = !bad; if (bad && ok) { el.focus(); ok = false; } }); return ok; }`

NEW:
```js
function validate(form, pairs) { let ok = true; pairs.forEach(([sel, test]) => { const el = $(sel, form); const help = el.closest('.fld')?.querySelector('.help'); const bad = !test(el); el.setAttribute('aria-invalid', String(bad)); if (help) { if (!help.id) help.id = (el.id || 'fld') + 'Err'; help.setAttribute('role', 'alert'); help.hidden = !bad; if (bad) el.setAttribute('aria-describedby', help.id); else el.removeAttribute('aria-describedby'); } if (bad && ok) { el.focus(); ok = false; } }); return ok; }
```

(b) Line 2009. OLD: `  if (!$('#rvAgree').checked) { toast('게시 원칙에 동의해 주세요'); $('#rvAgree').focus(); return; }`

NEW:
```js
  const agree = $('#rvAgree'), agreeErr = $('#rvAgreeErr'); if (!agree.checked) { agree.setAttribute('aria-invalid', 'true'); if (agreeErr) agreeErr.hidden = false; agree.focus(); return; } agree.removeAttribute('aria-invalid'); if (agreeErr) agreeErr.hidden = true;
```

**J9 — Trust counts and home region chips.** Insert after line 2023 (the `#colophon` line). The counts are real, taken from `CLINICS`.
```js
(function homeExtras() {
  const rvN = CLINICS.reduce((a, c) => a + c.rvs.length, 0);
  const tc = $('#trustClinics'); if (tc) tc.textContent = `입점 한의원 ${CLINICS.length}곳`;
  const tr = $('#trustReviews'); if (tr) tr.textContent = rvN ? `진료 확인 후기 ${rvN}개` : '현재 0개 · 첫 후기를 기다리고 있어요';
  const hr = $('#homeRegions'); if (!hr) return;
  hr.innerHTML = Object.keys(REGIONS).map(c => `<button class="chip" type="button" data-home-city="${esc(c)}">${esc(c)} <span class="cnt num">${CLINICS.filter(x => x.city === c).length}곳</span></button>`).join('');
  hr.addEventListener('click', e => { const b = e.target.closest('[data-home-city]'); if (!b) return; state.city = b.dataset.homeCity; state.district = '전체'; renderFind(); go('/find'); });
})();
```

**J10 — Chat IIFE (lines 2113–2373).** Apply each sub-edit in place.

**C1 — Animate only the field that changed; add the `FIELD` map** (line 2165).

OLD: `  const setCv = (id, text) => { const el = $(id); if (!el) return; el.textContent = text; el.classList.remove('fill'); void el.offsetWidth; el.classList.add('fill'); };`

NEW:
```js
  const setCv = (id, text) => { const el = $(id); if (!el || el.textContent === text) return; el.textContent = text; el.classList.remove('fill'); void el.offsetWidth; el.classList.add('fill'); };
  const FIELD = { part:'#cfPart', kind:'#cfKind', acc:'#cfAcc', dmg:'#cfDmg', when:'#cfWhenT', onset:'#cfWhenT', pain:'#cfPainT', ins:'#cfIns', insurer:'#cfInsr', dx:'#cfDx', stay:'#cfStay', cause:'#cfCause', sym:'#cfSym', hins:'#cfHins', city:'#cfRegion', dist:'#cfRegion', cond:'#cfCond' };
```

**C2 — "작성 중" caret** (line 2182). Keep the line and append to it.

OLD: `    const seal = $('#ccardSeal'); if (seal) { seal.hidden = !done; $('#ccard')?.classList.toggle('done', !!done); }`

NEW (the same, followed by):
```js
    $$('#ccard .cf.next').forEach(f => f.classList.remove('next')); const nq = !done && qs()[step]; if (nq && FIELD[nq.id]) $(FIELD[nq.id])?.closest('.cf')?.classList.add('next');
```

**C3 — State, keyboard modality and answer fly-in** (line 2184).

OLD: `  let ans = {}, step = 0, busy = false;`

NEW:
```js
  let ans = {}, step = 0, busy = false, dir = 'fwd', kb = false;
  addEventListener('keydown', e => { if (e.key === 'Tab' || e.key === 'Enter' || e.key === ' ') kb = true; }, true);
  addEventListener('pointerdown', () => { kb = false; }, true);
  const RM = () => matchMedia('(prefers-reduced-motion: reduce)').matches;
  const flyTo = (from, id) => { if (stepMode() || RM() || !from || !FIELD[id]) return; const to = $(FIELD[id]); if (!to || !to.offsetParent) return;
    const a = from.getBoundingClientRect(), b = to.getBoundingClientRect(); const g = from.cloneNode(true); g.className = 'opt fly'; g.setAttribute('aria-hidden', 'true'); g.removeAttribute('data-v'); g.removeAttribute('data-go');
    Object.assign(g.style, { left: a.left + 'px', top: a.top + 'px', width: a.width + 'px', height: a.height + 'px' }); document.body.appendChild(g);
    const an = g.animate([{ transform: 'none', opacity: 1 }, { transform: `translate(${b.left - a.left}px, ${b.top - a.top}px) scale(.5)`, opacity: 0 }], { duration: 320, easing: 'cubic-bezier(.22,1,.36,1)' });
    an.onfinish = an.oncancel = () => g.remove(); };
```

**C4 — Scroll target that accounts for the peek strip** (line 2188).

OLD: `  const toTop = () => { const y = box.getBoundingClientRect().top + window.scrollY - navH() - 8; if (Math.abs(window.scrollY - y) > 4) window.scrollTo({ top: y, behavior: 'auto' }); };`

NEW:
```js
  const toTop = () => { const hc = $('#homeChat'), side = $('.chat-side', hc), done = hc.classList.contains('done'); const target = done ? side : box;
    const off = navH() + 8 + (!done && getComputedStyle(side).position === 'sticky' ? side.offsetHeight : 0);
    const y = target.getBoundingClientRect().top + window.scrollY - off; if (Math.abs(window.scrollY - y) > 4) window.scrollTo({ top: y, behavior: 'auto' }); };
```

**C5 — Progress via `transform`; drive the back sheet** (line 2190).

OLD: `  const progress = () => { const n = Math.min(step, total()); $('#chatBar').style.width = (n / total() * 100) + '%'; $('#chatStep').textContent = `${n} / ${total()}`; };`

NEW: `  const progress = () => { const n = Math.min(step, total()); $('#chatBar').style.transform = `scaleX(${n / total()})`; $('#chatStep').textContent = `${n} / ${total()}`; $('.ccard-wrap')?.style.setProperty('--p', (n / total()).toFixed(3)); };`

**C6 — Screen-reader text in the typing indicator** (line 2197).

OLD fragment: `const t = bot('<span class="typing"><i></i><i></i><i></i></span>');` → NEW `const t = bot('<span class="typing"><i></i><i></i><i></i><span class="sr">입력 중</span></span>');`

**C7 — Move focus to the next options, keyboard users only, never at step 0** (line 2201).

OLD: `    box.appendChild(opts); busy = false; scrollBottom();` → NEW: `    box.appendChild(opts); busy = false; scrollBottom(); if (kb && step > 0) $('.opt', opts)?.focus({ preventScroll: true });`

**C8 — Step template: h2 question, direction, bottom back link; no scroll or focus theft at step 0** (lines 2206–2213; replace from `    box.innerHTML = `<div class="step" data-id="${q.id}">` through `    busy = false; toTop();`).

NEW:
```js
    box.innerHTML = `<div class="step" data-id="${q.id}" data-dir="${dir}">
      <div class="step-h"><button class="step-back" type="button" data-back aria-label="이전 질문" ${step ? '' : 'hidden'}><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M15 6l-6 6 6 6"/></svg></button><span class="step-n num">${n + 1} / ${total()}</span><span class="wm"><b>한방</b><i>에</i><span class="dot">.</span></span></div>
      <div class="step-bar" aria-hidden="true"><i style="width:${n / total() * 100}%"></i></div>
      <h2 class="step-q" tabindex="-1">${esc(typeof q.q === 'function' ? q.q(ans) : q.q)}</h2>${q.sub ? `<p class="step-sub">${esc(q.sub)}</p>` : ''}
      <div class="opts" data-id="${q.id}">${opts}</div>
      ${step ? '<button class="step-prev tlink muted" type="button" data-back>이전 질문</button>' : ''}
      ${step === 0 ? '<p class="meta step-foot">진단이 아니라 안내입니다. 어지럼·구토·마비·의식 저하가 있으면 문진보다 응급실이 먼저입니다.</p>' : ''}
    </div>`;
    busy = false; dir = 'fwd'; if (step > 0) { toTop(); if (kb) $('.step-q', box)?.focus({ preventScroll: true }); }
```

**C9 — Back direction and log reset** (lines 2216 and 2218).
- OLD `    if (step === 0) return;` → NEW `    if (step === 0) return; dir = 'back';`
- OLD fragment `$('#homeChat')?.classList.remove('done'); progress(); updateCard(ans, false); ask();` → NEW `$('#homeChat')?.classList.remove('done', 'show-log'); progress(); updateCard(ans, false); ask();`

**C10 — Fly-in hooks** (lines 2228–2229).
- OLD fragment `ans[q.id] = vs; opts.remove();` → NEW `ans[q.id] = vs; flyTo(e.target.closest('[data-go]'), q.id); opts.remove();`
- OLD fragment `ans[q.id] = b.dataset.v; opts.remove();` → NEW `ans[q.id] = b.dataset.v; flyTo(b, q.id); opts.remove();`

**C11 — `estHtml`: id, semantics, evidence folded** (lines 2274–2280).
- OLD fragment `return `<div class="est">` → NEW `return `<div class="est" id="res-est">`
- OLD fragment `<div class="est-h"><h4>내 합의금은 얼마나? · 예시</h4>` → NEW `<div class="est-h"><h3>내 합의금은 얼마나? <span class="tag demo">예시</span></h3>`
- OLD fragment `<div class="rchart est-chart" role="img" aria-label="치료 형태별 합의금 범위 예시, 내 구간 강조">` → NEW `<div class="rchart est-chart" role="group" aria-label="치료 형태별 합의금 범위 예시(만원): ${SETTLE.map(x => `${x.k} ${x.min}~${x.max}`).join(', ')}. 내 예상 ${e.lo}~${e.hi}만원">`
- Lines 2278–2280. OLD:
  ```js
      ${cmp}
      <div class="est-mix" aria-label="합의금 구성 예시">${mix}</div>
      <ul class="est-notes">${notes.map(n => `<li>${n}</li>`).join('')}</ul>
  ```
  NEW:
  ```js
      <details class="est-more"${stepMode() ? '' : ' open'}><summary>근거 더 보기 <span class="meta">보험사 공시 · 구성 비율 · 메모 ${notes.length}개</span></summary>
      ${cmp}
      <div class="est-mix" role="group" aria-label="합의금 구성 예시">${mix}</div>
      <ul class="est-notes">${notes.map(n => `<li>${n}</li>`).join('')}</ul>
      </details>
  ```

**C12 — `narrow()`, the range narrowing (transform only).** Insert immediately before `  function show(html, stepTitle) {` (line 2285).
```js
  function narrow(scope) { if (RM()) return; const m = $('.est-chart .rmark', scope); const bar = m && $('.rbar', m.parentElement); if (!bar) return;
    const L = parseFloat(m.style.left), W = parseFloat(m.style.width), A = parseFloat(bar.style.left), B = parseFloat(bar.style.width); if (!(W > 0)) return;
    m.style.transition = 'none'; m.style.transformOrigin = '0 50%'; m.style.transform = `translateX(${(A - L) / W * 100}%) scaleX(${B / W})`;
    requestAnimationFrame(() => requestAnimationFrame(() => { m.style.transition = 'transform 420ms cubic-bezier(.22,1,.36,1)'; m.style.transform = 'none'; })); }
```

**C13 — `show()`: 분석 중 beat, collapse the log, land on the result top and focus its title** (replace lines 2285–2289, the whole function).
```js
  function show(html, stepTitle) {
    if (stepMode()) { box.innerHTML = `<div class="step res-step"><div class="step-h"><button class="step-back" type="button" data-back aria-label="이전 질문"><svg class="i" viewBox="0 0 24 24" aria-hidden="true"><path d="M15 6l-6 6 6 6"/></svg></button><span class="step-n num">완료 · ${total()} / ${total()}</span><span class="wm"><b>한방</b><i>에</i><span class="dot">.</span></span></div>${html}</div>`; busy = false; $('#homeChat')?.classList.add('done'); toTop(); $('.res-title', box)?.focus({ preventScroll: true }); narrow(box); return; }
    const t = bot(`<ul class="analyzing"><li>부위별 치료 매칭</li><li>${esc(ans.city || '')} 입점 한의원 확인</li><li>${ans.kind === 'gen' ? '건강보험 적용 기준 확인' : '표준약관 기준선 계산'}</li></ul>`, true); scrollBottom();
    $$('.analyzing li', t).forEach((li, i) => setTimeout(() => li.classList.add('ok'), RM() ? 0 : 160 * (i + 1)));
    wait(720).then(() => {
      t.querySelector('.bubble').innerHTML = html; busy = false; $('#homeChat')?.classList.add('done');
      const tgt = matchMedia('(max-width: 1023px)').matches ? $('#homeChat .chat-side') : t;
      smoothTo(tgt, navH() + 16); $('.res-title', t)?.focus({ preventScroll: true }); narrow(t);
    });
  }
```

The title "정리해 드릴게요." now lives in the result hero (C14/C15), so `stepTitle` is unused.

**C14 — `resultGen()` template** (replace the `const html = ...;` statement at lines 2317–2329). The inner content of every block is verbatim; only the wrappers and ids, the hero, the nav and the warning placement change. Verify with a before/after `textContent` diff.
```js
    const urgent = severe || syms.includes('head') || syms.includes('numb');
    const warn = `<div class="warnbox"><b>먼저 병원에 가야 하는 경우</b> — ${esc(part.warn)}</div>`;
    const html = `<div class="res">
      <div class="res-hero"><p class="eyebrow">문진 결과 · 일반 통증 · ${esc(part.short)}</p><h2 class="res-title" tabindex="-1">정리해 드릴게요.</h2>
        <div class="sum"><span class="tag yes">${esc(part.short)}</span><span class="tag fill">일반 통증</span><span class="tag fill">${esc(label('onset', ans.onset))}</span><span class="tag fill">${esc(label('cause', ans.cause))}</span><span class="tag fill">${esc(label('pain', ans.pain))}</span>${syms.map(v => `<span class="tag fill">${esc(label('sym', v))}</span>`).join('')}<span class="tag fill">${esc(label('hins', ans.hins))}</span><span class="tag fill">${esc(ans.city)} ${esc(ans.dist === '전체' || !ans.dist ? '전체' : ans.dist)}</span>${conds.map(v => `<span class="tag fill">${esc(label('cond', v))}</span>`).join('')}</div>
        <dl class="res-kpis num"><div><dt>받을 수 있는 치료</dt><dd>${treats.length}<small>종</small></dd></div><div><dt>${esc(ans.city)} 입점 한의원</dt><dd>${list.length}<small>곳</small></dd></div><div><dt>건강보험 적용 항목</dt><dd>${cover.filter(c => c[2] === 'yes').length}<small>개</small></dd></div></dl>
        ${top[0] ? `<div class="res-cta"><button class="btn primary lg" type="button" data-book="${top[0].id}">${esc(top[0].name)} 예약하기</button><span class="meta">조건에 가장 잘 맞는 순서 · 확인 중 항목 포함</span></div>` : ''}</div>
      <nav class="res-nav" aria-label="결과 바로가기"><a href="#res-treat">치료</a><a href="#res-clinics">한의원</a><a href="#res-cost">비용</a><a href="#res-todo">알아둘 것</a></nav>
      ${urgent ? warn : ''}
      <div id="res-treat"><h3>받을 수 있는 치료 · 누르면 설명으로</h3><div class="tchips">${treats.map(t => { const c = catByKey(t); const st = catCounts(t); const ask = !st.yes.length; return `<a class="${ask ? 'ask' : ''}" href="#/cat/${CAT_SLUG(t)}">${pic(t)}${esc(c ? c.title : t)}<span class="st">${ask ? '한의원 확인' : `제공 ${st.yes.length}곳`}</span></a>`; }).join('')}</div><p class="meta" style="margin-top:8px">${esc(part.sym)}</p></div>
      <div id="res-clinics"><h3>추천 한의원 · ${esc(ans.city)} ${esc(ans.dist && ans.dist !== '전체' ? ans.dist : '전체')} ${list.length}곳 중 ${top.length}곳</h3>${top.length ? top.map(c => crowHtml(c, catStatus(c, treats[0]))).join('') : '<p class="meta">이 지역엔 아직 입점 한의원이 없습니다. 인접 지역을 선택해 보세요.</p>'}${conds.length ? '<p class="meta" style="margin-top:8px">선택한 조건(야간·주말·픽업·주차)은 한의원 확인 전이라 "확인 중"으로 표시됩니다. 전화로 먼저 확인하실 수 있습니다.</p>' : ''}</div>
      <div class="est" id="res-cost"><div class="est-h"><h3>비용은 어떻게 되나?</h3><span class="meta">건강보험 기준 · 금액은 한의원별 확인</span></div>
        <div class="cov">${cover.map(([n, d, st]) => `<div class="cov-r"><span class="tag ${st === 'yes' ? 'yes' : 'fill'}">${st === 'yes' ? '건보 적용' : '확인'}</span><b>${n}</b><span>${d}</span></div>`).join('')}</div>
        <p class="body">${hinsNote}</p>
        <p class="meta">교통사고가 아닌 일반 통증은 자동차보험 대상이 아니며 합의금도 없습니다. 위 기준은 일반적인 건강보험 적용 원칙이고, 세부 본인부담은 한의원과 진료 내용에 따라 다릅니다.</p></div>
      <div id="res-todo"><h3>알아두면 좋은 것</h3><ol class="know">${know.map(k => `<li>${k}</li>`).join('')}</ol></div>
      ${urgent ? '' : warn}
      <div class="acts"><button class="btn primary" type="button" data-more-find>이 조건으로 한의원 더 보기</button><a class="btn ghost" href="#/cat/${CAT_SLUG(treats[0])}">치료 자세히</a><button class="tlink muted" type="button" data-restart>처음부터 다시</button></div>
      <p class="meta">이 문진은 일반적인 안내이며 진단이 아닙니다. 실제 치료 구성은 진찰 뒤 한의사가 정합니다.</p>
    </div>`;
```

The inline `style="font-size:13px"` on `hinsNote` is removed; `.est .body` sets it to 15px.

**C15 — `result()` template** (replace the `const html = ...;` statement at lines 2349–2358). Same rules as C14.
```js
    const e0 = estimate(ans);
    const warn = `<div class="warnbox"><b>먼저 병원에 가야 하는 경우</b> — ${esc(part.warn)}</div>`;
    const html = `<div class="res">
      <div class="res-hero"><p class="eyebrow">문진 결과 · 교통사고 · ${esc(part.short)}</p><h2 class="res-title" tabindex="-1">정리해 드릴게요.</h2>
        <div class="sum"><span class="tag yes">${esc(part.short)}</span><span class="tag fill">교통사고</span><span class="tag fill">${esc(label('acc', ans.acc))}</span><span class="tag fill">${esc(label('when', ans.when))}</span><span class="tag fill">${esc(label('pain', ans.pain))}</span><span class="tag fill">${esc(label('ins', ans.ins))}</span><span class="tag fill">${esc(label('dmg', ans.dmg))}</span><span class="tag fill">${esc(label('insurer', ans.insurer))}</span><span class="tag fill">${esc(label('dx', ans.dx))}</span><span class="tag fill">${esc(label('stay', ans.stay))}</span><span class="tag fill">${esc(ans.city)} ${esc(ans.dist === '전체' || !ans.dist ? '전체' : ans.dist)}</span>${conds.map(v => `<span class="tag fill">${esc(label('cond', v))}</span>`).join('')}</div>
        <dl class="res-kpis num"><div><dt>예상 합의금 <span class="tag demo">예시</span></dt><dd>${e0.lo}~${e0.hi}<small>만원</small></dd></div><div><dt>받을 수 있는 치료</dt><dd>${treats.length}<small>종</small></dd></div><div><dt>${esc(ans.city)} 입점 한의원</dt><dd>${list.length}<small>곳</small></dd></div></dl>
        ${top[0] ? `<div class="res-cta"><button class="btn primary lg" type="button" data-book="${top[0].id}">${esc(top[0].name)} 예약하기</button><span class="meta">조건에 가장 잘 맞는 순서 · 확인 중 항목 포함</span></div>` : ''}</div>
      <nav class="res-nav" aria-label="결과 바로가기"><a href="#res-treat">치료</a><a href="#res-clinics">한의원</a><a href="#res-est">합의금</a><a href="#res-todo">할 일</a></nav>
      ${severe ? warn : ''}
      <div id="res-treat"><h3>받을 수 있는 치료 · 누르면 설명으로</h3><div class="tchips">${treats.map(t => { const c = catByKey(t); const st = catCounts(t); const ask = !st.yes.length; return `<a class="${ask ? 'ask' : ''}" href="#/cat/${CAT_SLUG(t)}">${pic(t)}${esc(c ? c.title : t)}<span class="st">${ask ? '한의원 확인' : `제공 ${st.yes.length}곳`}</span></a>`; }).join('')}</div><p class="meta" style="margin-top:8px">${esc(part.sym)}</p></div>
      <div id="res-clinics"><h3>추천 한의원 · ${esc(ans.city)} ${esc(ans.dist && ans.dist !== '전체' ? ans.dist : '전체')} ${list.length}곳 중 ${top.length}곳</h3>${top.length ? top.map(c => crowHtml(c, catStatus(c, part.treats[0]))).join('') : '<p class="meta">이 지역엔 아직 입점 한의원이 없습니다. 인접 지역을 선택해 보세요.</p>'}${conds.length ? '<p class="meta" style="margin-top:8px">선택한 조건(야간·주말·픽업·주차)은 한의원 확인 전이라 "확인 중"으로 표시됩니다. 전화로 먼저 확인하실 수 있습니다.</p>' : ''}</div>
      ${estHtml(ans)}
      <div id="res-todo"><h3>지금 해야 할 것</h3><ol class="know">${know.map(k => `<li>${k}</li>`).join('')}</ol></div>
      ${severe ? '' : warn}
      <div class="acts"><button class="btn primary" type="button" data-more-find>이 조건으로 한의원 더 보기</button><a class="btn ghost" href="#/guide">보험 처리 노하우 더 보기</a><a class="btn ghost" href="#/cat/${CAT_SLUG(part.treats[0])}">치료 자세히</a><button class="tlink muted" type="button" data-restart>처음부터 다시</button></div>
      <p class="meta">이 문진은 일반적인 안내이며 진단이 아닙니다. 실제 치료 구성은 진찰 뒤 한의사가 정합니다.</p>
    </div>`;
```

**C16 — `restart()`: reset the collapse and direction; no auto-scroll on load** (line 2367).

OLD: `  function restart() { ans = {}; step = 0; busy = false; box.innerHTML = ''; $('#homeChat')?.classList.remove('done'); progress(); updateCard({}, false); if (!stepMode()) bot('안녕하세요, 한방에입니다. 어디가 불편한지부터 같이 정리해 볼게요. 답은 누르기만 하면 됩니다.'); if (stepMode()) toTop(); ask(); }`

NEW: `  function restart() { ans = {}; step = 0; busy = false; dir = 'fwd'; box.innerHTML = ''; $('#homeChat')?.classList.remove('done', 'show-log'); const lt = $('#logToggle'); if (lt) { lt.setAttribute('aria-expanded', 'false'); lt.textContent = '내 답변 다시 보기'; } progress(); updateCard({}, false); if (!stepMode()) bot('안녕하세요, 한방에입니다. 어디가 불편한지부터 같이 정리해 볼게요. 답은 누르기만 하면 됩니다.'); ask(); }`

The `#chatRestart` handler on line 2368 still calls `toTop()` in step mode; that is user-initiated, which is fine.

**C17 — Log toggle.** Insert after line 2368:
```js
  $('#logToggle')?.addEventListener('click', e => { const hc = $('#homeChat'), on = hc.classList.toggle('show-log'); e.currentTarget.setAttribute('aria-expanded', String(on)); e.currentTarget.textContent = on ? '내 답변 접기' : '내 답변 다시 보기'; });
```

**J11 — `handleResult`.** Covered by J3.

**J12 — Stack legend: drop the inline hex** (line 2406).

OLD fragment: `<span class="sw ${c}" style="background:${c==='s1'?'var(--ink)':c==='s2'?'#3f6a5b':c==='s3'?'#7fa093':'#c3d3cb'}"></span>` → NEW `<span class="sw ${c}" aria-hidden="true"></span>`

**J13 — Responsive line chart** (replace lines 2387–2399, the whole `(function lineChart() { … })();`).
```js
/* 회복 곡선 (예시) — 컨테이너 폭으로 다시 그려 글자가 항상 12px */
(function lineChart() {
  const el = $('#lineChart'); if (!el) return;
  const xs = [0,2,4,6,8,10,12];
  const A = [7.5, 6.0, 4.5, 3.2, 2.2, 1.6, 1.2], Bv = [8.5, 5.5, 3.8, 2.6, 1.8, 1.3, 1.0], C = [7.5, 6.6, 6.0, 5.7, 5.9, 6.4, 6.9];
  let lastW = 0;
  const draw = () => {
    const W = Math.round(Math.max(300, Math.min(760, el.clientWidth || 640))), sm = W < 480, H = sm ? 260 : 300, L = 32, R = sm ? 92 : 124, T = 18, B = 40; lastW = W;
    const x = i => L + i * (W - L - R) / 6, y = v => T + (10 - v) * (H - T - B) / 10;
    const path = arr => arr.map((v, i) => `${i ? 'L' : 'M'}${x(i)},${y(v)}`).join(' ');
    const area = arr => `${path(arr)} L${x(6)},${y(0)} L${x(0)},${y(0)} Z`;
    const grid = [0,2,4,6,8,10].map(v => `<line class="grid" x1="${L}" x2="${W - R}" y1="${y(v)}" y2="${y(v)}"/><text x="${L - 8}" y="${y(v) + 4}" text-anchor="end">${v}</text>`).join('');
    const xl = xs.map((w, i) => `<text x="${x(i)}" y="${H - 14}" text-anchor="middle">${w === 0 ? '사고' : w + '주'}</text>`).join('');
    const dots = (arr, c) => arr.map((v, i) => `<circle class="${c}" cx="${x(i)}" cy="${y(v)}" r="4"><title>${xs[i] === 0 ? '사고 직후' : xs[i] + '주'} · 통증 ${v}</title></circle>`).join('');
    const band = `<rect class="band" x="${x(4)}" y="${T}" width="${x(6) - x(4)}" height="${H - T - B}"/><text class="lab band-l" x="${(x(4) + x(6)) / 2}" y="${T + 14}" text-anchor="middle">후유증 구간</text>`;
    el.innerHTML = `<svg class="linechart" viewBox="0 0 ${W} ${H}" role="img" aria-label="치료 여부에 따른 12주 통증 점수 변화 예시: 통원 치료와 입원 뒤 통원은 내려가고, 치료 없이 방치하면 8주 이후 다시 올라갑니다">${band}${grid}<line class="axis" x1="${L}" x2="${W - R}" y1="${y(0)}" y2="${y(0)}"/>${xl}<path class="a1" d="${area(A)}"/><path class="l3" d="${path(C)}"/><path class="l2" d="${path(Bv)}"/><path class="l1" d="${path(A)}"/>${dots(C, 'm3')}${dots(Bv, 'm2')}${dots(A, 'm1')}<text class="lab t3" x="${x(6) + 10}" y="${y(C[6]) + 4}">방치 ${C[6]}${sm ? '' : ' · 만성화'}</text><text class="lab t1" x="${x(6) + 10}" y="${y(A[6]) + 4}">통원 ${A[6]}</text><text class="lab t2" x="${x(6) + 10}" y="${y(Bv[6]) + 16}">입원→통원 ${Bv[6]}</text></svg>`;
  };
  draw();
  if ('ResizeObserver' in window) { let tm; new ResizeObserver(() => { clearTimeout(tm); tm = setTimeout(() => { if (Math.abs((el.clientWidth || 0) - lastW) > 24) draw(); }, 150); }).observe(el); }
})();
```

**J14 — Reveal and TOC scrollspy.** Insert immediately before the final `route();` (line 2414).
```js
/* 스크롤 리빌: 장식(형광펜·막대·곡선·궤도 칩)만 기다리고 콘텐츠는 늘 보임 */
(function reveal() {
  if (matchMedia('(prefers-reduced-motion: reduce)').matches || !('IntersectionObserver' in window)) return;
  document.documentElement.classList.add('js-rv');
  const io = new IntersectionObserver(es => es.forEach(en => { if (en.isIntersecting) { en.target.classList.add('is-in'); io.unobserve(en.target); } }), { rootMargin: '0px 0px -12% 0px', threshold: 0 });
  $$('.sec, .kv-hero, .phead, .dark-strip, .trust').forEach(el => { el.setAttribute('data-reveal', ''); io.observe(el); });
})();
(function tocSpy() {
  const links = $$('.toc a[href^="#"]'); if (!links.length || !('IntersectionObserver' in window)) return;
  const io = new IntersectionObserver(es => es.forEach(en => { if (!en.isIntersecting) return; links.forEach(a => a.getAttribute('href') === '#' + en.target.id ? a.setAttribute('aria-current', 'true') : a.removeAttribute('aria-current')); }), { rootMargin: '-35% 0px -60% 0px' });
  links.forEach(a => { const s = $(a.getAttribute('href')); if (s) io.observe(s); });
})();
```

**Line 2024 (`--marble`)** stays. It is harmless because no CSS references `--marble` any more. It sets an inline custom property on `<html>`, so an alias could not neutralise it anyway. It is only relevant if the marble footer is restored (§0.4-7).

**Deferred (not in this pass):**
- The full tab pattern for `.dtabs` and `#cityTabs` (`aria-controls`, roving tabindex): reuse the `#catRail` keydown handler.
- Booking prefill from the 문진.
- History entries for the sheets.
- Tapping a filled card field to edit that answer.

---

## 6. Acceptance checklist

Reviewers tick every box. Items marked (T) are verified with a tool.

### 6.1 Fixed user decisions

- [ ] **Wordmark.** The markup is exactly `<b>한방</b><i>에</i><span class="dot">.</span>` in Noto Sans KR 900/300, and the dot is `#7fd1b0` everywhere (nav, footer, card, step header, orbit core, masthead). No other text uses Noto.
- [ ] **Typeface.** All other text is Wanted Sans. Computed `font-family` spot checks: body, `.btn`, `.chip`, `.opt`, `.seal-in small`, table cells.
- [ ] **Colour.** The page is bright and green-led: canvas `#f4f6f1`, primary `#0f4a39`. There is no dark theme; the only inverse surfaces are `.find-band`, the footer, `.cbar` and `.toast`. Amber appears only on 예시/데모/caution.
- [ ] **Highlighter.** One highlighted key phrase per heading (the M16 list). It never appears in body text, buttons, numbers or the wordmark.
- [ ] **Icons.** Monochrome line SVG only. No emoji anywhere (search the file for emoji code points).
- [ ] **Assets.** The key visual (pill and powder) renders in the KV stage. The AI silhouette with mint hotspots renders, and the mini-body clones it.
- [ ] **Honest data.** Every "확인 중", "데모", "예시", "첫 후기를 기다리고 있어요", "미집계" and disclaimer is still rendered, including `.row .quote` on mobile and the 예시 tag on the 예상 합의금 KPI. Compare the counts before and after (T: `grep -o '확인 중\|데모\|예시' | wc -l`); the count must not drop.
- [ ] **Home.** The 문진 is the first screen, and every section and feature in the inventory is present (§0.1-5 order).
- [ ] **Copy.** No copy change beyond §0.4-8 (new labels) and §0.4-9 (the placeholder fix). The pending options in §0.4 are not applied.

### 6.2 Visual

- [ ] At 1440×900, the mast, card top and first question are visible above the fold, and the sticky card fits within `100vh − nav − 24px` after scrolling.
- [ ] The section rhythm reads canvas → white trust → KV stage → tiles → white band → canvas → celadon data room → canvas → white band → deep-green card → footer.
- [ ] Program index shows 4×2 tiles (2 columns ≤1023, 1 column ≤767). Glance shows 5 tiles. Why shows 2×2. Slab shows 4 KPI tiles with an inverted "0원". Find rows are cards.
- [ ] Chapter numerals appear only on section lockups and program tiles; there are no row, result or TOC counters.
- [ ] Shadows come only from `--e1`–`--e4` and radii only from `--r-*` (T: grep the style block for `box-shadow:` values outside the tokens).

### 6.3 Accessibility (pro-max P1)

- [ ] `<!doctype html><html lang="ko">`, and `document.compatMode === "CSS1Compat"` (T).
- [ ] Text contrast is ≥4.5 and body text ≥7 (the §1.3 tables). Re-run `final.py` after any token change (T). No `--text-disabled`, `--line` or `--accent` is used as text.
- [ ] Non-text boundaries (chips, inputs, checkboxes, toggles, gauges, dots, chart marks) are ≥3:1.
- [ ] One visible focus style on every surface; the mint ring appears on the four inverse surfaces. Hotspot focus (dashed dark ring plus fill) is distinct from selected (solid ring plus fill). Gallery thumbnails show selection with an inset ring and focus with an outline.
- [ ] Keyboard: completing both funnels without a mouse keeps focus on the next option (chat) or the step question (step mode). There is no focus jump at step 0 or on page load, and the skip link is the first Tab stop.
- [ ] Results: the page scrolls to the result top, `.res-title` receives focus, and the Q&A log is collapsed with a `#logToggle` that has `aria-expanded`.
- [ ] Only one live region (`#chat`). `#ccard` has no `aria-live`. The typing indicator contains `<span class="sr">입력 중</span>`.
- [ ] Headings: one h1 per route, including mobile home and partner. `.mini`, `.tl` and the result blocks use h3. The step question is h2.
- [ ] Dialogs have `aria-labelledby`, a Tab trap, `inert` on header/main/footer/tabbar/cbar, Esc closes them, and focus returns to the trigger. The modal→booking flow keeps focus inside the booking sheet.
- [ ] Forms: `aria-invalid`, `aria-describedby`, `role=alert` errors with an icon; required fields; `autocomplete` on the partner form; the agreement error is inline, not a toast.
- [ ] `aria-label` only on elements with a role (`role="group"` added). Decorative SVGs have `aria-hidden` (T: grep for `<svg class="i" viewBox="0 0 24 24">` without `aria-hidden` in the HTML body returns 0).
- [ ] Tabbar 홈 navigates and shows active state (`.on`, `aria-current`, top bar).

### 6.4 Touch and interaction (P2)

- [ ] Under `pointer:coarse`, every control is ≥44px: `.btn.sm`, chips, `.opt`, `.tlink`, `.xbtn`, `.hamb`, `.step-back`, `.p-close`, `.cmp`, tel links, `#sortSel`, TOC chips, `.res-nav` links, orbit chips, region nodes (with their `::after` hit area) and footer links.
- [ ] Hotspots are ≥24px with no overlap (r=100); `#partList` provides the 44px equivalent.
- [ ] Hover effects exist only under `(hover:hover)`. Press feedback is `scale(.97–.98)`. `touch-action:manipulation` is set on buttons.
- [ ] Horizontal scrollers (hot chips on mobile, filters, rail, TOC, res-nav) show an edge fade or are obviously chip rows, and the page itself never scrolls horizontally at 375px (T: `document.documentElement.scrollWidth === 375`).

### 6.5 Performance (P3)

- [ ] The font is preloaded, and Noto is requested with `&text=` (T: the Network panel shows a small Noto CSS/woff2 file).
- [ ] The KV image has width, height and `decoding="async"`.
- [ ] No layout-shifting hovers.
- [ ] `backdrop-filter` is used only on the nav, tabbar, peek strip, resbar/TOC, res-nav, overlays and cbar. If a low-end Android device janks, drop the blur below 768px.

### 6.6 Motion (P7)

- [ ] Durations are 100–400ms; only the chart and highlighter use 480ms. No infinite animation except the typing loader and the busy spinner (the card background no longer spins).
- [ ] The spring easing is used only on the `.me` bubble, `.seal-in` and `.dotp`.
- [ ] With `prefers-reduced-motion` on: everything renders in its final state, there is no `js-rv` class, and the fly-in and narrowing are skipped (T: emulate it in DevTools).
- [ ] With JS disabled or without IntersectionObserver, all highlights, bars, charts and orbit chips are visible.

### 6.7 Charts (P10)

- [ ] Line series differ by dash as well as colour. Direct end labels are 12px at 375px width (T: measure a `text` element's `getBoundingClientRect` height ≈ 12–14px). A "표로 보기" table is present.
- [ ] Range charts use `role="group"` with spelled-out labels, and every value is printed. The average line is dashed ink.
- [ ] Stacked-bar segments have 2px gaps, s3/s4 are outlined and hatched, and the legend shows every percentage.
- [ ] `.raxis` meta ("예시값", "업계 평균 … 출처") stays visible on mobile.

### 6.8 Regression (JS-coupled selectors)

- [ ] Every id in inventory §2 still exists (T: extract ids before and after and diff; only additions allowed: `logToggle`, `trustClinics`, `trustReviews`, `homeRegions`, `rvAgreeErr`, `res-*`, `sheetTitle` and `modalTitle` set at runtime).
- [ ] Every JS-emitted class has a rule in §3 (T: list the `class="…"` tokens in the JS templates and check each against the stylesheet; the dead classes in §6.10 are expected to have none).
- [ ] Flows work end to end:
  - hash routes `#/`, `#/chat`, `#/find`, `#/guide`, `#/reviews`, `#/partner`, `#/cat/<slug>` (both deep link and in-page link scroll to the panel)
  - rail arrow keys
  - panel FAQ opens with no console TypeError (J3)
  - compare up to 3, then the modal, then booking
  - booking validation, then the done state
  - review and partner validation
  - rotating or resizing the viewport between step mode and chat mode
- [ ] Console is clean on all routes (T).

### 6.9 Tools

- [ ] (T) `node C:/Users/junho/.claude/skills/design-system/scripts/validate-tokens.cjs` on the extracted `<style>` saved as `.css`. Raw hex should appear only inside `@layer tokens` (plus mask `#000`, `%23000` and white in data URIs). No `font-size` below 12px.
- [ ] (T) Python contrast script re-run: every pair in §1.3 matches.
- [ ] Manual passes:
  - widths 375, 768, 1024, 1440
  - 1440×720 (the compact card variant)
  - iOS Safari safe areas (tabbar, cbar, sheet footer)
  - VoiceOver or NVDA smoke test of the 문진

### 6.10 Dead classes — intentionally NOT styled

`br`, `block`, `dark` (`.chips.dark`), `card`, `part` (`.figure .part`), `cat`, `a2`, `n0`, `rvsum`, `rk`, `.figure .cap`, `.est-cmp .rrow*` / `.est-cmp .rmark` (never rendered), and `dl.dark-dl` (hidden).

Used but unstyled on purpose: `.rule`, `.pt`, `.pa`, `.row-cta` (inline), `.more-rows` (toggled only with `hidden`), `crow.no` (the neutral default row style applies).
