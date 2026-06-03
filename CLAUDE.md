# Agentropic Website — Customise Instagram Scraper landing page

Marketing landing page for the IG Reel scanner (the actual product lives in `/home/mayank/Claude/IG Research/`).

## Live + repo
- **Live**: https://agentropicai.github.io/agentropic-ig-site/
- **Repo**: `agentropicai/agentropic-ig-site` (PUBLIC — the only public repo in the org)
- **Hosting**: GitHub Pages, master branch root, ~30-60 s rebuild after `git push`
- **Local preview**: `python3 -m http.server 8765 --bind 127.0.0.1` in this dir → `http://127.0.0.1:8765/`

Other deployment paths considered + ruled out: Vercel (device-code auth over SSH is clunky), ngrok (auto-mode blocks tunnel creation). GH Pages won.

## Architecture
- Single self-contained `index.html` (~700 lines). No build step. Vanilla CSS, no Tailwind.
- Google Fonts via `<link>` CDN.
- All videos in `/video/`, posters extracted via ffmpeg.
- Mobile-first. Desktop is deliberately deprioritised per user (*"I don't care how it looks on desktop"*).

## Sections (current as of 2026-06-03)
| # | Section | What's in it |
|---|---|---|
| 1 | HERO | Veo3 video (phone+laptop+USB+sheet) + H1 + sub + CTA → Calendly + trust band |
| 2 | PROBLEM | 3 horizontal 3D cards: icon on top, short text below |
| 3 | WHAT YOU GET | SVG mock of the Google Sheet (4 rows, ACCEPT/REJECT/REVIEW pills) |
| 4 | HOW IT WORKS | 6 native `<details>` accordion cards. **Default closed.** Steps 02–06 = the 7-day free trial. |
| 5 | YOUR DEDICATED AI ENGINEER | Italic statement + one-line + 16:10 image (Gemini ImageFX workstation still — SVG placeholder until real one arrives) |
| 6 | WHO IT'S FOR | 3 native `<details>` expandable cards: **Brands** · **Agencies** · **Influencers** (tap to reveal each buyer's value prop) |
| 7 | FAQ | 6 native `<details>` Qs |
| 8 | FINAL CTA | "Book your intro call" → Calendly placeholder |
| 9 | FOOTER | Minimal |

**Sections dropped 2026-06-03 (per user):** the former §5 "Behaves like a human" (warm-up curve) and former §6 "Trained on you" (loop diagram). Markup + CSS removed.

## Aesthetic
- Palette: warm paper `#F2EDE3` + deep ink `#1C1812` + single bookish-red accent `#9A2A2A`
- Subtle SVG grain noise overlay on body (paper texture)
- Type: **Instrument Serif** display (italic for emphasis), **Schibsted Grotesk** body, **JetBrains Mono** labels
- Editorial-magazine vibe — NOT generic clean-sans SaaS

## CTA + commercial details
- Calendly placeholder: `https://calendly.com/agentropic/intro-call` (real link TBD — user will provide)
- Email: `mayank@agentropic.ai`
- **The 7-day free trial is a STAGE inside How It Works (§4, steps 02–06)** — never the CTA copy, never its own banner.

## Videos
- `/video/hero.mp4` — 16:9, 10 s, 1280×720, 2.1 MB. Phone + laptop + USB cable, Google Sheet auto-filling.
- Generated via Veo 3 in labs.google/fx (Flow). Download trick: `https://labs.google/fx/api/og-video/shared/{uuid}` — public MP4 endpoint exposed by every share link's og:video meta. No browser needed (see memory `veo3-share-download`).
- Brand world used in every Veo prompt (see memory `veo3-brand-world`): walnut desk + warm daylight + Kodak Portra 400 LUT + cello drone + felt-key "tick" + no humans/logos/clocks.

## Hard standing rules (from user during this build)
1. **CTAs MUST be "Book intro call" → Calendly.** Never "Start your trial", never "Start free".
2. **§4 accordion cards stay default-closed.** First card does NOT open on load.
3. **Mobile-first only.** Desktop is "don't care".
4. **No `[SPEAK]` blocks anywhere** — voice output is permanently disabled (see `~/.claude/CLAUDE.md`).
5. **Find references before inventing visuals.** Spawn a research agent against comparable sites (Submagic, Modash, Foreplay, Captions, Linear, Lavender) BEFORE designing new sections.
6. **Visual surface area beats word count.** When the page "feels heavy", add screenshots / video / illustrations — do NOT just trim more copy. (Lesson from this build — see memory `landing-page-visual-density`.)

## Deploy workflow
```bash
# from this dir
git add . && git -c user.email="mayank@agentropic.ai" -c user.name="Mayank Mundhra" commit -m "msg"
git push
# wait ~30-60s for GH Pages rebuild
gh api repos/agentropicai/agentropic-ig-site/pages/builds/latest --jq .status
```

## Next steps (pending as of 2026-06-03)
- §3, §5, §7 may get static images instead of the reverted videos (user undecided)
- Real Calendly URL replaces placeholder
- Possibly more sections / refinements per user feedback
