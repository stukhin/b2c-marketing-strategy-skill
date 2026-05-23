# Audit self-check

Run this Python snippet from terminal at Step 7. Same pattern as the main `b2c-marketing-strategy` skill's Step 5 self-check, scoped to the audit dashboard's 8 sections.

```bash
python3 -c "
import re
s = open('output/<file>-audit-v1.html').read()

# Check 1: Header filled
if '[Audited file name]' in s or '[Brand audited]' in s or 'Audit Dashboard Template' in s:
    print('✗ HEADER not filled — fix .doc-title-main and .header-meta')
else:
    print('✓ Header filled')

s_visible = re.sub(r'<!--[\s\S]*?-->', '', s)

# Check 2: Audit-specific placeholder leaks
leaks = [
    (r'\[Audited file name\]',           'file name placeholder — fill with actual uploaded filename(s)'),
    (r'\[Brand audited\]',               'brand placeholder — fill from context Q1'),
    (r'\[Category\]',                    'category placeholder — fill from context Q2'),
    (r'\[Score [1-9X]+\]|\[NN\]',        'score placeholder — fill with 0-100 number'),
    (r'\[Headline pattern\]',            'composite headline missing'),
    (r'\[Gap [1-9] title\]|\[Gap N\]',   'gap title placeholder'),
    (r'\[Insight [1-9]+\]',              'insight card placeholder'),
    (r'\[Doc [1-9]\]|\[Document N\]',    'document name placeholder in cross-doc map'),
    (r'\[Recommendation [1-9]\]',        'recommendation placeholder'),
    (r'\[Action verb\] ',                'recommendation action-verb placeholder'),
    (r'\[Claimed:.*?\]|\[Observed:.*?\]','reality-check cell placeholder'),
    # Audit dashboard borrows the main-skill rule: data-* attrs are plain text only
    (r'data-(?:score|criterion|insight)=\"[^\"]*<[a-zA-Z]', 'HTML tag inside data-* attribute — plain text only'),
]
total_leaks = 0
for pat, label in leaks:
    matches = re.findall(pat, s_visible)
    if matches:
        total_leaks += len(matches)
        print(f'✗ {label}: {len(matches)} hits — first: {matches[0]!r}')
print(f'  Total leaked patterns (visible): {total_leaks}')

# Check 3: Broad bracket sweep
hits = re.findall(r'\[[A-Za-z][^\]]{0,80}\]', s_visible)
real = [h for h in hits if not re.match(r'\[(data-|aria-|class=|role=|T[1-3]\])', h)]
real_no_js = [h for h in real if 'TextRun' not in h and 'colIdx' not in h]
print(f'  Other bracket hits (excl. attrs/T1-T3/JS): {len(real_no_js)}')
for h in real_no_js[:10]: print(f'    {h}')

# Check 4: All 7 criterion scores numeric
score_pattern = re.findall(r'class=\"crit-score\"[^>]*>(\d+)', s)
print(f'  Criterion scores found: {len(score_pattern)} (need 7 for Full audit, 5 for Quick score)')
if score_pattern and all(s.isdigit() and 0 <= int(s) <= 100 for s in score_pattern):
    print('  ✓ all scores are 0-100 integers')

# Check 5: Gap list non-empty
gaps = s.count('class=\"gap-card\"')
print(f'  Gap cards: {gaps} (audit should find at least 3 — 0 gaps is suspicious)')

# Check 6: 'Beyond the document' insights present
insights = s.count('class=\"insight-card\"')
print(f'  Insight cards: {insights} (target 3 Quick / 5-9 Full audit)')

# Check 7: Section balance
print(f'  Sections: {s.count(\"<section\")} open / {s.count(\"</section>\")} close (target 8/8)')
"
```

## What to fix when checks fail

- **Header issues** — go back to Step 6, fill `.doc-title-main` with actual brand or filename, `.header-meta` with file list + date
- **Placeholder leaks** — open the leaked pattern in the file, replace with content. JS-array leaks (rare in this skill but possible if you used charts) — replace in `const` arrays
- **Score count wrong** — make sure each criterion section has a `<span class="crit-score">NN</span>` element
- **Zero gaps** — re-examine. A real-world strategy with literally zero gaps is suspicious. Either the audit was too lenient or you missed something. Lower the bar for what counts as a gap (small precision issues count) until you find at least 3
- **Zero insights** — same logic. If you literally found nothing outside-the-document, the document is either exceptional or you didn't look hard enough. Run pattern detection more aggressively
- **Section balance off** — count `<section>` opens and closes. Common bug: a section was duplicated during a copy-paste

Run the self-check before presenting. Don't present a dashboard that fails the basic checks.
