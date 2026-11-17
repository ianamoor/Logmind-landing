# Logmind promo pages: full handover (state of 2026-11-16, evening)

Everything a new Claude chat (or Codex) needs to carry on the Logmind promo site: who the client is, the rules Lana has set, how to work, every page, and every design file. Read it fully before touching anything. It replaces and extends `Logmind_handover_for_Codex.md` / `Logmind_promo_handover.md` (those only know index + features).

## 1. What this is
- **Client:** Logmind (AIOps platform, logmind.com). **Lana** is the designer. She sends small visual change requests, usually with a screenshot or an image. Each one is applied, checked in a browser, saved to her Mac folder, attached in chat, and answered in one or two sentences.
- **Deliverable:** 10 static, self-contained HTML promo pages (inline CSS, inline JS, base64 images). No build step, no framework. Lana uploads them to GitHub Pages herself (repo `ianamoor/Logmind-landing`). **Never push or publish.**
- **Style of work:** terse replies (what changed + the file), change only what she asks, if a request is ambiguous pick the most reasonable reading and say which in one line.

## 2. Where everything lives
| What | Where |
|---|---|
| Current pages (source of truth) | Lana's Mac: `~/Yandex.Disk.localized/Logmind/Promo page/*.html` (overwrite on every change) |
| Snapshot of 8 pages she made on 16 Nov | `Promo page/16nov/` (index, features, capabilities, infrastructure, applications, blog, faq, about-us) |
| Her reference images | `Promo page/1.png`–`4.png`, `image.webp`, `690b1d89…infrastructure-background (1).avif` (leave alone) |
| Old Codex-era files | `Promo page/page/` (AGENTS.md, HANDOVER.md, MIGRATION.md, tools/, two old pages). Outdated, do not use as source |
| Older copies | `~/Downloads` (stale, ignore) |
| Claude project "Promo for Logmind" docs | `claude/Logmind_workflow_in_claude.md` (standing rules + page notes), `claude/Logmind_handover_for_Codex.md`, `claude/Logmind_promo_handover.md`, and this file |
| Workspace (cloud, lost between sessions) | `/home/claude/Logmind-landing/`: pages, `tools/strip.py`, `tools/shot.py`, `_prev/` (undo copies), `shots/`, `AGENTS.md` |

**New chat setup:** stage the 10 pages from the Mac folder (`device_stage_files`), recreate `tools/strip.py` and `tools/shot.py` (code in section 9), `pip install playwright` if missing, make `_prev/`. Always start from the Mac files, not from memory.

## 3. Hard rules
1. **Never touch the hero of `index.html`** (scroll-scrubbed video). It is her design.
2. Additive patches only. New CSS goes right before the last `</style>`, new JS before the last `</script>`. Do not reformat existing code.
3. Never read a whole HTML file (mostly base64). Use `tools/strip.py` → `*.stripped.html` for reading; patch the real file with exact-string Python replacements (`assert s.count(old)==1`). Never regenerate base64 by hand.
4. Copy the current file to `_prev/` before each change so "undo" works.
5. Verify every change at **1440×900, 1024×768, 390×844**: no horizontal overflow (`scrollWidth - innerWidth == 0`), no console errors, and look at the PNGs.
6. Delivery per change: copy to `/mnt/user-data/outputs/`, `device_commit_files` to the Mac folder, `SendUserFile`, short reply.
7. **Verify the commit.** On 2026-11-16 four commits reported "written" but the Mac still had old files (index, features, book-demo, free-trial). Check with `device_bash`: `md5sum` of the Mac file against the workspace file; recommit with `force:true` if they differ.
8. Do not publish artifacts of these pages or push to GitHub.

## 4. Standing rules from Lana (apply to every page, new and existing)
- **Section titles** (h2 above a block): `padding-bottom: 50px` (`.ubh`, `.cq h2`, `.ulg h2`, `.cap h2`, `.fqs h2`, `.start h2`).
- **Header navigation letter-spacing 0 on everything**, not only top links: `nav#nav,nav#nav *,.drawer,.drawer *{letter-spacing:0!important}` (in all 10 pages).
- **"Why customers love Logmind"**: `padding-bottom: 70px` (`main.fpg section.cq`, `.tst` on index). Present on index, features, capabilities, infrastructure, applications, faq, about-us.
- **First section after the hero**: `padding-top: 70px` on inner pages (features `section.plat#feat`, capabilities `#insights`, blog `.bfeat`, faq `section.faq`, about-us `section.avm`). Exceptions: applications and infrastructure first block keeps 45px (her explicit request); index; free-trial and book-demo (hero is the whole page).
- **"Signals to Stability"** identical on every page that has it (features, applications, infrastructure), as on applications.html: dark neon image (`--sigbg`), `.fsglow` hidden, white title/lede/pill/button, 90px top/bottom padding, 40px `--mi-*` icons, no border lines. Infrastructure keeps its own **green-lines** background by her explicit request.
- **Search icon is blue everywhere, no pink handle** (menus, Signals cards, index illustrations). `--ic-search` is overridden with `--mi-search`; the index illustration overlay SVG uses a blue gradient handle. Other icons keep pink accents.
- **Label pills have no blue dot.**
- Menu/Header "Try for free" pill is solid white on the light (scrolled) header.

## 5. Design system
- Colors: blue `#3061E0`, light blue `#8DA9FF`, `#92a8f9` (check icons), night `#0a1020`/`#0B1020`, page tint `#e5e9f6` (`--pbg`, white → tint while scrolling), vision/mission band `#f0f2ff`, pink accents (menu icons, testimonial highlights).
- Menu icons: `--mi-*` CSS vars (SVG data URIs, 30×30, `.mi-ic.pic.j-<name>`); older raster set `--ic-*` (webp) still used by `.i-*` classes.
- Shared template classes: `main.fpg`, `.fhero` (small hero, `.fh-bg` parallax image `--fimg`, `.fh-c` fades), `.sec`, `.w`, `.fsig`/`.sig3`/`.sgi`, `.fin` (final CTA "Less IT noise"), `.cq` (testimonials; `.tst` on index), footer `.ft`.
- Header `nav#nav` (56px on inner pages, 70px on index), mega menu `.mega`/`.pn`, mobile `.drawer`; becomes light via `nav.lt` after the hero; `nav.mo` = menu open (white sheet, blue top-right gradient `--nh`).
- Background images live in CSS vars holding data URLs: `--fimg` (neon network, hero + footer), `--sigbg` (Signals), `--epfl` (Lausanne photo), `--ftimg` (free-trial background), `--bdmimg` (book-demo background), `--gsbg`, `--secbg`, `--blogbg`, `--f0..--f3` (feature screenshots), `--ugrid`, `--ulogos`, `--ava/--avh/--avb` (applications hub parts).
- Fonts and copy follow logmind.com; menu and footer links mirror the live site.

## 6. Pages (all 10, current state)
Common to inner pages: same head, header, mega menu, small hero (`.fhero`), content, final dark CTA "Less IT noise" (not on free-trial / book-demo), footer. The live contact form at the bottom of pages is left out on purpose. Each page links only its own menu/footer entry locally; other links still go to live logmind.com.

| File | Size | Content and notes |
|---|---|---|
| `index.html` | 9.8 MB | Main landing page. Hero (video, **do not touch**), Proof logos, v3 dashboard reveal (`#v3`), Platform pillars (`#platform`, sticky screenshots, wave `--wave`), Use cases (`.uc2`, tabbed steps with animated illustrations), Results (`#results`, bars: IntersectionObserver adds then removes `.run`, so persistent styles must not depend on `.run`), Testimonials (`.tst`), Get started (`#start`), Partners, final CTA, footer. |
| `features.html` | 2.0 MB | "AI-powered features": 4 feature articles (`#detect #inform #investigate #monitor`) with sticky frames, Results bars (`.fres`), **Signals to Stability** (= applications), **Why customers love Logmind** (4 testimonials), CTA. Menu/footer feature links point to `features.html#…`. |
| `capabilities.html` | 3.4 MB | Three tabbed groups (tabs swap text + screenshot; section 03 uses the animated index illustrations), dark Discover block with glass-framed screenshot, testimonials in index layout. Hero uses the neon-lines image Lana supplied. |
| `applications.html` | 3.2 MB | First block is a visual from Lana's design SVGs: Challenge card, animated hub (dots flow through a pulsing cube), Solution card; stored as three data-URI SVG vars `--ava/--avh/--avb`; grid `.avis > .avg`; stacks under 900px. Hover/focus on Challenge/Solution shows shadow + side tooltip (`.avtip`) with the full live text (below the card on mobile). Then 4 benefits, integration card (wavy grid right only), Signals to Stability, testimonials. Reference page for Signals. |
| `infrastructure.html` | 2.5 MB | Same layout as applications. Cards are inline SVGs with real text and own cloud + server icon (no design supplied); own 4 benefits and integration logos; Signals with **green** neon background; testimonials. |
| `blog.html` | 3.0 MB | Featured post + 14 posts with topic filter, plain (no-card) layout like the live blog. "Read post" goes to live articles. |
| `faq.html` | 2.3 MB | General + Platform FAQ (6 each) as accordions with side menu, Discover block, testimonials. **Template used to build free-trial and book-demo.** |
| `about-us.html` | 3.0 MB | Vision + mission band (`#f0f2ff`, 70px padding, 30px titles, no top line); team grid (13 people) as white cards like logmind.com, 4 per row (3 tablet, 2 mobile), lift-on-hover on every card (LinkedIn badge where the live site has one); Get started section from index; testimonials. |
| `free-trial.html` | 2.6 MB | One dark hero over the EPFL campus photo (`--ftimg`, navy tint): heading "Sign up for your free 30-day trial" + lede left, white form card right (9 fields as on live: first/last name, business email, phone, company, job title, company size, country, message; button "Request free trial"), then footer. Browser-side validation + "Thank you" state. **Form sends nothing** (static promo). Classes `.ftr`, `.ftr-card`, `.ftr-form`. |
| `book-demo.html` | 2.3 MB | Same shell, neon image hero (`--bdmimg`, the cubes-and-lines image Lana supplied on 16 Nov, fades into night). Heading "See how AI can streamline your IT Operations", white card with the **real HubSpot scheduler** (`<iframe src="https://meetings.hubspot.com/logmind/demo?embed=true">`, loads and books for real when online; inside cannot be restyled; fallback link "Open it in a new tab"). Testimonial rotator beside it: 3 quotes (healthcare Director of IT Ops, Romain C., Arnaud G.), fade, 4.5 s interval, 0.6 s fade, loops, pause on hover/focus, clickable progress bars, reduced-motion safe (`.bdm-q`, `.bdm-s`, `.bdm-d`). "Try for free" points to `free-trial.html` on these two pages only. |

## 7. Design files (inputs from Lana)
| File / asset | Used for |
|---|---|
| `application_il_main.svg` (first design) and `application_il_main2.svg` (redesign, current basis) | First block of `applications.html`: Challenge card, hub illustration, Solution card. Split into `--ava`, `--avh` (hub with SMIL animation), `--avb`. Replaced the earlier huge CSS-crop approach that lost raster patterns on mobile. |
| `Main.dc.html` (earlier session) | Results section design on index (hatched tracks, glow fills, knob, legend) |
| `logos.svg` (earlier session) | Partners grid on index (rendered to one transparent `.webp`, 12 dark `#111E37` tiles) |
| `690b1d89…infrastructure-background (1).avif` | Signals to Stability background on `infrastructure.html` (green lines) |
| Neon-pink/blue lines image | Signals on `applications.html`/`features.html` (`--sigbg`), hero of several pages (`--fimg`) |
| EPFL campus photo (`EPFL_campus_20177.original.jpg`) | `--epfl` (testimonial photos/overlays) and free-trial background `--ftimg` |
| Cubes-and-lines night image (16 Nov, `F0pdH.jpg`) | `book-demo.html` background `--bdmimg` |
| `1.png`–`4.png`, `image.webp` in the Mac folder | Reference images she supplied for earlier requests; keep untouched |
| Team photos, icons, logos for about-us/menu | Copied from logmind.com, embedded as base64 |

Images are re-encoded to WebP to keep files light. Re-encode new uploads (`PIL … save('WEBP', quality≈76)`), embed as data URLs in a CSS var, and append the override rule at the end of `<style>`.

## 8. Known caveats and open items
- Forms: free-trial form does not send; book-demo scheduler depends on HubSpot being reachable.
- "Try for free" / "Book demo" on the 8 older pages still go to the live logmind.com URLs. Offered: point them to `free-trial.html` / `book-demo.html` everywhere. Not requested yet.
- The menu icon and footer links to pages without a local file still go to live logmind.com.
- Header, mega menu and footer are duplicated across all pages: a header change must be applied to all 10 files (she usually says "all pages").
- Index: "Root cause found" hero tooltip sits partly behind the menu bar at 1440×900 (existing, not requested).
- Index footer does not use the network image (offered, not requested).
- Older unrequested ideas: abstract icons for other sections, remove repeated footer tagline, blue logo in all states, testimonial photo for the other cards.
- Reduced-size mobile crop on book-demo shows only the top of the background image (cubes mostly out of view). Acceptable so far.
- Page sizes are 2–10 MB because assets are inlined. Unused leftover assets from the faq template may remain in free-trial/book-demo; could be trimmed if size matters.

## 9. Tools (recreate in a new session)
`tools/strip.py`:
```python
#!/usr/bin/env python3
"""Make a readable copy of a base64-heavy HTML file: python tools/strip.py index.html"""
import re,sys,pathlib
src=pathlib.Path(sys.argv[1]); s=src.read_text(encoding='utf-8')
t=re.sub(r'[A-Za-z0-9+/=]{300,}','XXX',s)
out=src.with_suffix('.stripped.html'); out.write_text(t,encoding='utf-8')
print(f'{src} {len(s):,} -> {out} {len(t):,} chars')
```
`tools/shot.py`:
```python
#!/usr/bin/env python3
"""Screenshot a page at 1440/1024/390, report console errors + horizontal overflow.
python tools/shot.py features.html [--scroll 900] [--sel "#start"] [--hover ".mb[data-m=platform]"]"""
import argparse,asyncio,pathlib
from playwright.async_api import async_playwright
ap=argparse.ArgumentParser();ap.add_argument('file');ap.add_argument('--scroll',type=int,default=0)
ap.add_argument('--sel');ap.add_argument('--hover');a=ap.parse_args()
url=pathlib.Path(a.file).resolve().as_uri();out=pathlib.Path('shots');out.mkdir(exist_ok=True)
async def main():
    async with async_playwright() as p:
        b=await p.chromium.launch()
        for w,h in [(1440,900),(1024,768),(390,844)]:
            pg=await b.new_page(viewport={'width':w,'height':h});errs=[]
            pg.on('pageerror',lambda e:errs.append(str(e)))
            await pg.goto(url);await pg.wait_for_timeout(1200)
            if a.sel: await pg.evaluate("s=>{const e=document.querySelector(s);scrollTo(0,e.getBoundingClientRect().top+scrollY)}",a.sel)
            elif a.scroll: await pg.evaluate(f"scrollTo(0,{a.scroll})")
            if a.hover: await pg.hover(a.hover)
            await pg.wait_for_timeout(1500)
            f=out/f'{pathlib.Path(a.file).stem}_{w}.png';await pg.screenshot(path=str(f))
            ov=await pg.evaluate("document.documentElement.scrollWidth-innerWidth")
            print(w,'errors:',errs or 'none','overflow:',ov,f)
        await b.close()
asyncio.run(main())
```
Fetching live logmind.com pages (to copy text, forms, markup): Playwright through the agent proxy (`proxy={'server': os.environ['HTTPS_PROXY']}`, `ignore_https_errors=True`). `curl`/WebFetch often fail on these pages.

## 10. Typical request loop
1. Read the request and any image. Back up the file(s) to `_prev/`.
2. `strip.py` → grep the stripped copy for the selector. Patch with exact-string Python, or append CSS before the last `</style>`.
3. `shot.py` → open the PNGs at 3 widths; also test interactions (hover, tabs, forms) with ad-hoc Playwright.
4. Copy to `/mnt/user-data/outputs/`, `device_commit_files` to `Promo page/`, `md5sum` check on the Mac, `SendUserFile`.
5. If it is a standing rule, add it to `Logmind_workflow_in_claude.md` (read, then rewrite whole doc) and to project memory.
6. Reply in 1–3 sentences: what changed, the file, any one-line ambiguity.
