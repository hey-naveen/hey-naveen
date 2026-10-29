# Achievement Card Maintenance Guide

This guide explains how to add certificate images and achievement cards safely and consistently, either manually or with an AI coding agent.

## Sources of truth

- `achievement_content.js` contains the manually curated achievement cards.
- `certificates/` contains the image files used by those cards.
- The achievements page does not scan the folder or generate cards automatically. Adding an image alone will not make it appear on the page.

## Human workflow

1. **Find new images.** List the files in `certificates/` and compare them with the image paths already referenced in `achievement_content.js`. Treat only unreferenced images as candidates; do not assume every unreferenced file is a new achievement.
2. **Inspect every candidate.** Open the image and read the certificate itself. Record only details that are legible or confirmed by the owner:
   - recipient, event or programme, issuer, award/result, date or academic session, and any relevant class or context;
   - whether the certificate recognizes participation, a placing, completion, academic performance, or a leadership role.
3. **Resolve ambiguity.** Ask the owner about unclear handwriting, dates, awards, duplicate-looking certificates, or whether multiple images belong to one achievement. Do not infer missing facts.
4. **Draft each card separately.** Use a clear title without a date or year. Write a concise summary that explains what the achievement was and what the certificate says it recognizes. Use details for issuer, event context, and date/session when useful. Keep claims grounded in the certificate or owner confirmation.
5. **Choose the image grouping.**
   - Create a separate card for each distinct achievement, even if several certificates share a date.
   - Put multiple images in one card only when they are additional photos/scans of the same achievement and the owner agrees or the relationship is clear.
6. **Propose changes for review.** Show each proposed title, category, date/session, filename, summary, details, and image grouping. Wait for explicit approval before editing card data or renaming files. Apply only the approved changes.
7. **Name approved images.** Use the convention below. Keep filenames descriptive and use a distinct slug for every distinct achievement.
8. **Add the approved entries** to the `achievements` array in `achievement_content.js`, following the existing object style.
9. **Verify the result.** Confirm every `image` and optional `images` path exists, check for broken or duplicated references, run `git diff --check`, and inspect the achievements page when it is available.

## Filename convention

Use:

```text
YYYY-MM-DD__achievement-slug__photo-NN.jpg
```

Examples:

```text
2016-02-09__drawing-competition-first-prize__photo-01.jpg
2016-02-09__science-quiz-first-place__photo-01.jpg
```

For an unknown day, use `YYYY-MM`; for an academic session without an exact date, use `YYYY-YY`:

```text
2011-05__drawing-competition__photo-01.jpg
2019-20__head-boy-hostel-student-council__photo-01.jpg
```

Distinct achievements on the same day need distinct slugs. Additional images of the same achievement use the same date and slug with sequential photo numbers, such as `photo-01` and `photo-02`. Do not group files just because their dates match.

## Card conventions

An entry generally uses this shape:

```js
{
  title: 'Math Relay - Third Position',
  category: 'Competition',
  date: '2019-08-22',
  image: 'Achievements/certificates/2019-08-22__math-relay-third-position__photo-01.jpg',
  summary: 'Placed third in the Math Relay, earning recognition for a strong result in a competitive event.',
  details: 'The certificate was issued by the organizing school and dated 22 August 2019.',
}
```

- Keep dates out of titles; the card metadata already displays the date.
- Use the exact award/result and event name shown on the certificate.
- Describe the achievement and why it was recognized when the certificate supports that explanation. Do not invent a reason, skills, impact, event expansion, or accomplishment.
- Use the existing categories where appropriate, such as `Competition`, `Academic Recognition`, `Academic Award`, `Certification`, `Workshop`, and `Leadership`.
- Use `images: [...]` only for multiple photos/scans belonging to one achievement. The `image` value remains the card thumbnail.
- Use a truthful date: `YYYY-MM-DD` for an exact date, `YYYY-MM` when only the month is known, or `YYYY-YY` for an academic/session range. Ask before resolving ambiguous handwritten dates.

## Final validation checklist

- Every new certificate image was inspected and matched to its proposed card.
- Every unlisted image was accounted for as a new card, an additional photo, a duplicate, or a file needing clarification.
- Multiple achievements on the same date remain separate; only confirmed photos of the same achievement share a card.
- Titles contain no dates or years, and summaries/details contain no unsupported claims.
- Every referenced image exists at the exact case-sensitive path used in the card.
- `git diff --check` passes and the page renders the new cards as intended.

## Reusable prompt for an AI agent

Copy and use the prompt below when new certificate images have been added to `Achievements/certificates/`.

```text
Review the newly added certificate images in `Achievements/certificates/` and prepare achievement cards in `Achievements/achievement_content.js`.

Approval is required before any file changes:
- First inspect the existing card data and list all image paths it already references.
- Compare those paths with the files in the certificates folder. Treat only unreferenced files as candidates, and do not assume an unreferenced file is a distinct achievement.
- Open and inspect every candidate image. Read the certificate content directly. Do not rely on filenames alone.
- For each candidate, identify only what the certificate or the owner confirms: event/programme, award or result, issuer, exact date or academic session, participation versus placement, and relevant class/context.
- If handwriting, dates, the event name, or the relationship between files is uncertain, ask the owner. Never guess or silently normalize ambiguous information.
- Check for existing cards that may already represent the same achievement. Different certificates on the same date are separate achievements unless evidence and the owner confirm they are multiple photos/scans of one achievement.
- Draft one proposed change per distinct achievement. Include title, category, date/session, proposed descriptive filename, summary, details, and the proposed image/gallery grouping. Summaries should be concise but informative: say what the achievement was and what the certificate supports as the reason for recognition. Do not invent accomplishments, motivation, skills, impact, event expansions, or reasons not shown or confirmed.
- Keep dates and years out of titles. Use YYYY-MM-DD for exact dates, YYYY-MM when the day is unknown, and YYYY-YY for an academic/session range. If several distinct achievements share a date, give each a different descriptive slug.
- Use filenames in the form YYYY-MM-DD__achievement-slug__photo-NN.jpg (or YYYY-MM / YYYY-YY when appropriate). Only additional photos of the same achievement share a slug and use sequential photo numbers.
- Show the complete proposed entries and filename changes to the owner, item by item, and wait for explicit approval. Do not rename, create, edit, or delete files before approval.
- After approval, make only the approved changes. Preserve unrelated work and existing user changes. Add entries using the existing style in `achievement_content.js`; set `image` to the thumbnail and add `images` only when the approved card has multiple photos.
- Do not add a generator or automatic folder scan. Cards are maintained manually.
- After editing, verify that each referenced image exists, that renamed files use the exact paths in the entries, and that no approved candidate was omitted. Run `git diff --check`; inspect the page if the local preview is available and report clearly if it is not.
- Finish with a concise report listing reviewed images, cards added, filenames changed, validation performed, and any items still needing owner clarification.
```
