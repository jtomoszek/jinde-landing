# Jinde Edu — landing page v2 (10 variant)

Přepracovaná verze stránky `https://dev.besthr.cz/l?name=Video1k1` podle dokumentu
*Kritické hodnocení první landing page Jinde Edu* (7. 8. 2026), rozšířená o
**10 textových variant** z dokumentu klienta (14. 8. 2026).

## Soubory

- **`index.html`** — šablona se všemi 10 textacemi; varianta se volí URL parametrem
  `?name=Video1k1` … `Video5k3` (stejný mechanismus jako ostrá verze `/l?name=…`).
  Bez parametru nebo s neznámým názvem se zobrazí Video1k1.
  Texty všech variant jsou v JS objektu `PAGES` (H1, podnadpis, název videa,
  H2 sekce a 3 odrážky, CTA).
- **`prehled.html`** — interní rozcestník pro prezentaci: 10 karet, každá otevře
  příslušnou variantu. Má `noindex`, do produkce nepatří.
- **`video-poster.jpg`** — statický náhled videa (na výšku).
Vizuál (barvy, fonty, tvarosloví) je převzatý 1:1 z původní stránky.

| Token | Hodnota |
|---|---|
| space-indigo | `#1F2344` |
| mint-leaf | `#41B17A` |
| seashell | `#F4E9E7` |
| nadpisy | Plus Jakarta Sans |
| text | Poppins |

---

## Co je potřeba doplnit před spuštěním

Všechna nedoplněná místa jsou v HTML označená žlutým odznakem **„DOPLNIT"**
(`class="todo"`), takže nejde omylem publikovat vymyšlený důkaz.

### 1. Konfigurace — objekt `CONFIG` na začátku `<script>`

```js
mode: 'waitlist',                          // 'video' | 'waitlist'
videoLengthMin: 12,                        // skutečná délka videa
deadlineISO: '2026-08-31T23:59:00+02:00',  // skutečný termín
urgencyReason: '…',                        // pravdivý důvod omezení
```

`mode` je klíčový pro message match. Přepnutím se **současně** změní text CTA, ikona,
mikrocopy, popis přístupu i odpověď ve FAQ — nelze nastavit „Pustit video", když se
ve skutečnosti sbírá e-mail.

- `mode: 'video'` → CTA „Pustit video zdarma", video se přehraje v modálu
- `mode: 'waitlist'` → CTA „Získat přístup k videu e-mailem", modál sbírá e-mail

### 2. Obsahové placeholdery

| Kde | Co doplnit |
|---|---|
| Sekce *Kdo ti pomůže* | Čísla autority jsou vyplněná (12 let v HR, 3000+ CV, 12 oborů) — **ověřit u klienta, že jsou pravdivá** |
| Sekce *Kdo ti pomůže* | Zdroj tvrzení o ghostingu — **jinak nechat nekvantifikovaně, bez „9 z 10"** |
| Sekce *Důkazy* | 5 referencí se souhlasem autorů (nekonečný pás) |

Pokud reference nebudou, celou sekci `#dukazy` odstraňte — prázdné šablony nepublikujte.

### 3. Technické napojení

- `#video-mount` — vložit `<video>` nebo `<iframe>` s reálným videem.
  Video box v hero je **na výšku (9:16)**, celá plocha je „běžící" video
  (poster `video-poster.jpg` + tenká přehrávací lišta) s odpočtem položeným
  přímo na něm; reálné video by mělo být rovněž vertikální.
- Reference: **5 karet v nekonečném pásu** (zprava doleva, na krajích do
  ztracena, hover pozastaví). Obsah karet je placeholder — doplnit 5 skutečných
  referencí; hodnocení **5,0 hvězdiček** ponechte jen tam, kde je skutečné.
- Na mobilu je CTA tlačítko s mikrocopy **až pod videem** (desktop: vlevo pod textem).
- `modalForm` submit — nahradit `setTimeout` skutečným `fetch()` na endpoint
- Obrázky `jinde.png` a `jina.jpg` jsou linkované na `dev.besthr.cz`; při nasazení
  změnit na relativní `/jinde.png` a `/jina.jpg`

---

## Co se oproti původní stránce změnilo

### P0
- **Responzivita.** Ověřeno bez horizontálního scrollu na 320 / 360 / 390 / 414 / 768 / 1024 px.
- **Sjednocený cíl.** Jeden primární cíl, jeden text CTA, odvozený od reálné cesty (`CONFIG.mode`).

### P1
- **Headline** přepsaný na problém + konkrétní výsledek podle doporučení dokumentu.
- **CTA** popisuje bezprostřední další krok, ne slibovaný výsledek. Doplněna mikrocopy pod tlačítkem.
- **Nová důkazní vrstva:** *Co získám* → *Proč tomu věřit* → *Důkazy* → *Odstranění rizika* → *CTA* → *FAQ*.
- Autorka přesunuta **před** opakované CTA, ne jako dovětek.

### P2
- **Horní zelená lišta s odpočtem odstraněna** (na přání klienta). Odpočet je
  položený přímo na videu — má pevný termín (nikdy se negeneruje z „teď + X")
  a přesné datum konce je ve FAQ. Po vypršení nezobrazuje falešný tlak.
- **Cookie lišta odstraněna** (na přání klienta). Stránka sama žádné měřicí
  skripty nenačítá (`fbq` je jen guardovaný hook) — pokud při nasazení přidáte
  FB pixel / GA, je potřeba consent řešení vrátit, jinak hrozí rozpor s GDPR.
- **FAQ** o 6 otázkách, blok i nadpis zarovnány na střed.

### Přístupnost
Skip link, viditelný focus, focus trap v modálu, Escape zavírá, `aria-live` u odpočtu
i stavu formuláře, jeden `<h1>`, alt texty, CTA min. 44 px a max. 2 řádky.

---

## Měření

Události se posílají do `window.dataLayer` (a vybrané do `fbq`), přesně podle
plánu v dokumentu:

`lp_view` · `hero_cta_click` · `proof_view` (důkazní blok z 50 % ve viewportu) ·
`form_start` · `form_submit` · `video_start`

Chybí `video_50` / `video_complete` — ty jde navěsit až na reálný přehrávač
v `#video-mount`.

---

## Lokální náhled

```bash
python3 -m http.server 8765
```

Pak otevřít `http://localhost:8765/index.html`.
