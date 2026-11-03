# sign.sign — Daily Draft (task definition)

This file is the single source of truth for the daily sign.sign draft task.
The scheduled task only says "follow signsign/TASK.md". To change the task, edit this file and push.

You generate ONE new daily design-concept card for an apparel/goods brand called **sign.sign**.
The brand turns real multilingual signage (transit, border posts, recycling/trash notices, municipal signs)
into minimalist typographic graphics: a bold headline plus the same meaning in the real local languages,
exactly as written on an actual sign — illustrated with a real photo of that sign as evidence.

- Gallery artifact (update this one, never create a new one): https://claude.ai/artifact/QENi7Zca3p9AQXjrZgSqxh
- Repo files: `signsign/history.json` (every card made so far), `signsign/gallery.html` (snapshot of the last published page)
- Branch: `claude/signsign-task-migration-3yxeeh`

## 0. Setup
```
cd /home/user/todoapp
git fetch origin claude/signsign-task-migration-3yxeeh
git checkout claude/signsign-task-migration-3yxeeh
git pull origin claude/signsign-task-migration-3yxeeh
TZ=Asia/Tokyo date +%F   # today's date (JST) — use this as the card date
```

## 1. Pick today's country/language set
Read `signsign/history.json`. A country is "used" if it appears there.
Compute `day_index = (day-of-year of today's UTC date) mod 30` and take that entry from the list below (0-indexed).
If it is used, move to the next index (wrap around) until you find an unused one.
If every country in the list is used, choose a new country with an established multilingual-signage convention
that is not in history.json, and start the final report with「⚠ 国リストを使い切りました。リストを追加してください」.

0. Canada — English / French / Inuktitut — federal or territorial government sign
1. New Zealand — English / Māori — public transport or national park sign
2. South Africa — English / Afrikaans / Zulu — municipal notice
3. India (Mumbai) — Hindi / Marathi / English — train station sign
4. Israel — Hebrew / Arabic / English — airport or municipal sign
5. Finland — Finnish / Swedish — public notice
6. Belgium — Dutch / French / German — highway or metro sign
7. Switzerland — German / French / Italian — train station sign
8. Wales (UK) — Welsh / English — road sign
9. Ireland — Irish / English — public building sign
10. Philippines (Manila) — Filipino / English / Chinese — market or transit sign
11. Vietnam (Ho Chi Minh City) — Vietnamese / English / Chinese — Cholon district sign
12. Malaysia — Malay / English / Chinese / Tamil — transit sign
13. UAE (Dubai) — Arabic / English / Hindi / Urdu — metro sign
14. Qatar — Arabic / English — municipal sign
15. Morocco — Arabic / French / Tifinagh (Amazigh) — road or municipal sign
16. Kazakhstan — Kazakh / Russian — metro sign
17. Mongolia — Mongolian (Cyrillic) / Chinese — border or market sign
18. Nepal — Nepali / English — trekking-permit or airport sign
19. Sri Lanka — Sinhala / Tamil / English — train station sign
20. Myanmar — Burmese / English — airport sign
21. Laos — Lao / French / English — temple or market sign
22. Peru — Spanish / Quechua — municipal or tourist sign
23. Bolivia — Spanish / Aymara / Quechua — government notice
24. Paraguay — Spanish / Guaraní — municipal sign
25. Norway — Norwegian / Northern Sámi — municipal sign
26. Iceland — Icelandic / English — tourist safety sign
27. Luxembourg — Luxembourgish / French / German — road sign
28. South Korea (Seoul) — Korean / English / Chinese / Japanese — subway sign
29. Egypt (Cairo) — Arabic / English — metro sign

## 2. Find a REAL photo as evidence (required)
Search Wikimedia Commons (site:commons.wikimedia.org, or categories such as "Multilingual signs in <country>",
"Bilingual signs", "Trilingual signs") for an actual photographed sign matching the country's convention.
Open the file page and confirm: it is a real photo (not a logo/map/drawing), its exact license
(must be CC0, CC BY or CC BY-SA — skip anything unclear), the author, and the upload.wikimedia.org file URL.
If no real, clearly licensed photo can be found after reasonable search, move to the next unused index. Never invent or use an unlicensed image.

Download into the scratchpad (not the repo):
`curl -sL -A "signsign-daily-draft/1.0" "<upload.wikimedia.org URL>" -o <scratchpad>/sign-photo.<ext>`
On HTTP 429, wait and retry, or use a thumbnail URL (`/thumb/.../1280px-<name>`). Look at the downloaded image with Read to confirm it is the sign.

## 3. Read the sign, pick the headline
Base everything on what the photo ACTUALLY shows — read the real text in each language from the photo and the file description. Do not guess.
- Headline: a short **Japanese** word/phrase capturing the meaning (e.g. 出口, 線路横断禁止, 工事現場). Do not reuse a headline already in history.json.
- Translations: the sign's real wording in each language, one line per language, in the order on the sign.

## 4. Accent color
One hex color evocative of the context (signage color standard of the sign itself, not a flag swatch).

## 5. Build the card and publish
1. Read the gallery: Artifact `action: "read"`, url above. Save the page HTML to `<scratchpad>/gallery.html`.
   The read may wrap the page in an extra publish skeleton (`<!doctype html><html><head><meta charset=utf8>… <body>` before the page's own `<!doctype html>` with `<title>Sign.Sign Drafts</title>`). Keep only the inner page — from that inner `<!doctype html>` to its closing `</html>`.
   If the read's live version has cards that are not in history.json (added by hand), add them to history.json too.
2. Upload the photo: Artifact `action: "publish"`, `url`: gallery url, `file_path`: the downloaded photo, `asset: true`.
   Use the returned url exactly as the `<img src>`.
3. Next number = highest `data-no` in the page + 1, zero-padded to 3 digits.
4. Append exactly ONE new entry as the last child of `<div class="grid">`, in exactly this shape (same classes as existing entries):

```html
  <div class="entry">
  <article class="card" data-no="NNN" style="--card-accent:#HEX;">
    <img class="photo" src="ASSET_URL" alt="English description of the real photographed sign and where it was taken">
    <div class="credit">Photo: AUTHOR, LOCATION YEAR — Wikimedia Commons, LICENSE</div>
    <div class="headline">日本語の見出し</div>
    <div class="translations">
      <div class="t-line"><span class="t-lang">言語名(日本語)</span><span class="t-word">Real wording from the sign</span></div>
      <!-- one t-line per language actually on the sign -->
    </div>
    <div class="meta-row">
      <span class="country">国名(日本語)</span>
      <span class="tag">Draft · YYYY-MM-DD</span>
    </div>
  </article>
  <div class="copy"><div class="copy-h"><span>説明テキスト(コピー用)</span><button class="cp" type="button">コピー</button></div><pre>FLAG 都市・場所で見つけた/にあった〇〇(1行)。
言語A・言語B・言語Cの3言語で書かれています。
和訳:日本語訳

【併記の理由】なぜこの言語が併記されているか(30字以内)

Photo: AUTHOR — Wikimedia Commons, LICENSE</pre></div>
  </div>
```

   - Single language: 「ラオス語で書かれています。」. Five or more: 「英語・フランス語など10言語で書かれています。」.
   - Non-Latin script: if the page's `<head>` font link lacks the needed Noto family (loaded now: Arabic, Devanagari, Lao, KR, SC, JP), add it to that link, and set the span's `font-family` inline (`'Noto Sans XX','Noto Sans',sans-serif`). Add `dir="rtl"` for Arabic/Hebrew/Urdu.
   - Do not change or remove existing entries, the header, the styles or the script.
5. Publish: Artifact `action: "publish"`, `url`: gallery url, `file_path`: `<scratchpad>/gallery.html`.
   **Do not pass `capabilities`** — omitting it keeps the stored `assets` + `downloads` declaration that photo upload and the download buttons need.
6. Read the gallery again and confirm the new card is there.

## 6. Record in the repo
- Append the new card to `signsign/history.json` (same fields as the others).
- Copy the published page to `signsign/gallery.html` (backup snapshot).
- Commit (`sign.sign draft NNN: <country> <headline>`) and `git push -u origin claude/signsign-task-migration-3yxeeh` (retry up to 4 times with 2s/4s/8s/16s backoff on network errors).

## 7. Notify and report
1. PushNotification (`status: "proactive"`), one line, under 200 characters, no markdown, Japanese first, e.g.
   `sign.sign No.017 ウェールズ「進入禁止」をギャラリーに追加 / Wales "NO ENTRY" added`.
   Always send it — if something failed, say what failed (Japanese + English) instead.
2. Final message in Japanese:
   - 日付、No.、国、見出し、写真の内容(1行)、ギャラリーのリンク
   - 写真のクレジット:(1) Commons のファイルページURL (2) 表示用クレジット行 (3) ライセンス名とライセンスURL
   - 新しい国リストの残り数
