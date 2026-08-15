# Ecoweave

Jekyll site for [ecoweave.co.uk](https://www.ecoweave.co.uk), hosted on GitHub Pages from the
`master` branch. It sells bags, purses and accessories hand-woven from upcycled crisp packets.

The business is **Victoria Vivian**, a sole trader trading as Ecoweave. This matters for copy:
the site is one person writing in the first person, not a company.

## Build and preview

```bash
jekyll build --destination <dir>
jekyll serve --host 0.0.0.0 --port 4000 --livereload
```

In Codespaces the forwarded port is private by default. To test on a phone, set port 4000 to
Public in the Ports panel — worth doing, because several bugs here only reproduce on iOS.

## Products

Every product lives in `_data/products.yml`. There are no per-product pages: one YAML block
produces the listing entry, the detail block and the PayPal button.

- **Remove a product** — delete its block. Nothing else in the repo references products by id,
  so that is the whole job. Orphaned images under `img/products/` are harmless.
- **Add a product** — copy an existing block. It needs a PayPal hosted button id, and images at
  both `img/products/thumb/<img>` and `img/products/full/<img>`.

Fields worth knowing: `allow-purchase: false` swaps the Add to basket button for a "Contact me
for availability" link; `category` decides which listing pages show it, with `highlight` meaning
the small-tile grid; `_data/features.yml` has an `accept-payments` flag that switches purchasing
off site-wide.

### Rules for product data and copy

These are Victoria's, learned the hard way. Breaking them creates orders she has to turn down.

1. **`colors:` is a stock list, not a description of the photograph.** A product may list
   `[Silver]` while its photo shows a multicoloured item — that is correct and not a bug. She
   edits stock by hand and often. Never change `colors:` to match an image, and never touch it
   as part of a copy change.
2. **Never state how many options there are.** "Four colourways to choose from" goes stale the
   next time stock moves.
3. **Never suggest an item can be made bigger, smaller or a different shape.** She may not be
   able to do it and does not want it promoted. Requests for different **colours, colourways or
   patterns** are welcome and fine to mention. The one exception is **glasses cases**, which she
   will try to make to measure if necessary.
4. **No schools, classrooms or children.** Products aimed at under-14s would pull her into toy
   safety testing she does not want. The Terms state the products are not toys and are not
   intended for use by children; keep copy consistent with that.

### The "Barcode" variant

Some products offer a Barcode option priced differently from Standard. Selecting it hides the
colour row and posts `Not Applicable` to PayPal, via the script in `_includes/buying-options.html`.

That option is **added to and removed from the DOM on purpose**. Do not revert to hiding it with
`display:none` — iOS renders `<select>` as a native picker wheel that ignores CSS on individual
options, so a hidden option is still listed on iPhone and Chrome-on-iOS while looking fine on
desktop at every screen width.

## Voice

First person singular, warm, plain, British. Keep the playful straplines that are already there
("Packed full of flavours", "Protect your specs").

Avoid AI-isms. Ones that have been called out: "I'll tell you plainly", "genuinely" as an
intensifier, "rest assured", "peace of mind", "delve", "worth noting". Natural informality such
as "we'll sort it out" is welcome — it is the polished-assistant register that is not. Grep new
copy for repeated intensifiers before showing it.

## Legal pages

`returns.html`, `privacy.html` and `terms.html`, linked from `_includes/footer.html`. Written to
UK consumer law as at August 2026. Constraints that must not be quietly undone:

- **There is no order confirmation email.** The contract forms when PayPal accepts payment, and
  the Terms say so. Do not reintroduce wording that obliges Victoria to do something per order —
  she will forget, and the page will then be inaccurate.
- **Cancellation refunds include the outbound postage**, and the 14-day refund clock can start on
  the customer's proof of postage rather than the goods arriving (Consumer Contracts Regulations
  2013, reg 34(5)). Not optional, and cannot be contracted out of.
- **"Personalised" means a name or initials worked into the piece.** A custom colour or theme is
  *not* bespoke and keeps the full 14-day cancellation right. The Returns and Terms pages must
  agree with each other on this.
- Address is published as town and county, with the full address available on request.

## No analytics, no cookies

Google Analytics was removed: the property was Universal Analytics, dead since July 2023, and it
set cookies with no consent banner. `privacy.html` now states the site sets no cookies.

If you add any tracker, you must update `privacy.html` and add a PECR-compliant consent banner in
the same change. Note the `ga()` calls that used to sit in `footer.html` and `paypal.html` were
removed with it — reintroducing the script alone is not enough.

## Gotchas

- **`.dl-horizontal` is overridden** in `css/main.css` (`dt{width:auto}`, `dd{margin-left:70px}`)
  to suit short product spec labels like Size and Closure. Any label wider than 70px collides
  with its value. Do not use it for prose.
- **The footer has a fixed height** matched by `body`'s `margin-bottom`. Adding links can make it
  wrap on narrow screens and overlap page content; both values must change together.
- **There is no navigation menu.** `_includes/menu.html` contains only the brand link. The
  `menu:` front matter set on most pages is not used by anything.
- **Nothing links to the category pages.** `/handbags-and-purses/`, `/gifts-and-accessories/`,
  `/household/`, `/device-covers/` and `/all-products/` are reachable only by direct URL. The
  homepage renders every product in full via `list-all-categories.html`.
- **`/device-covers/` renders completely empty** — no product carries a `devices` category and
  there is no Kindle cover in the product list. Unresolved: delete the page, or reinstate a
  product.

## Commits

Explain **why**, not what — the diff already shows the what. Separate commits for distinct
changes, even when they touch the same file. Never add "Generated with Claude Code",
"Co-Authored-By: Claude" or any similar attribution, anywhere.
