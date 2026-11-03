# Pokemon cards → eBay

Everything for turning a folder of iPhone photos into eBay listings. Batch 1 (2 Nov 2026) lives here.

## The process

1. **Photograph** each card front then back, in order. Slabs: make sure the grader label is sharp.
   Raw cards: get the set number at the bottom of the card readable (it is the one thing that cannot be guessed).
2. **Drop the photos** in `batchN-raw/` (zip from the phone is fine).
3. **Identify** – Claude reads the slab labels (grader, grade, cert number) and raw-card art/set numbers
   and writes `inventory.csv`. Duplicated photos are collapsed by cert number.
4. **Price** – `python price_lookup.py` pulls PriceCharting prices for the card's actual grade
   (Ungraded / 7 / 8 / 9 / 9.5 / PSA 10 grid, plus same-grader recent sales so CGC and PSA are not mixed)
   and writes `prices.csv`.
5. **Photos** – `python prep_photos.py` crops every photo to the card/slab into `ebay-photos/`.
   Host that folder somewhere public (eBay only accepts image URLs).
6. **Listings** – `python make_ebay_csv.py --photo-base <url> --shipping <policy> --returns <policy> --payment <policy>`
   writes `ebay-upload.csv`. Upload it in eBay Seller Hub → Listings → Upload. Fix any header mismatch by
   pasting the header row from the template eBay gives you for category 183454.
7. **Review** the drafts in Seller Hub, then publish.

## Files

- `inventory.csv` – one row per physical card: images, grader, grade, cert, set, number, confidence, notes
- `prices.csv` – inventory plus PriceCharting value for the grade, the full price grid, recent sales, link
- `ebay-upload.csv` – the bulk upload file
- `ebay-photos/` – cropped listing photos named `<item>-front.jpg` / `<item>-back.jpg`
- `work/`, `sheets/`, `labels/`, `raw/` – working images used for identification

## Grades matter

Same card, different money. A CGC Pristine 10 sells above a CGC 10, which sits near a PSA 10.
PSA 9 is usually a third of PSA 10. "CGC Authentic" has no grade and sells as a raw card.
The inventory keeps the exact label wording so the listing title and eBay's grade field match the slab.
