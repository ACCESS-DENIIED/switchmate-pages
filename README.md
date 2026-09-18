# switchmate-pages

The public content repo for the SwitchMate UK app. The app fetches `deals.json`
from this repo's GitHub Pages URL:

    https://access-deniied.github.io/switchmate-pages/deals.json

The app repo is private; this repo is public so the app can read its content
without a private URL (a raw URL to a private repo returns 404 and the app
falls back to its bundled defaults, silently showing stale offers).

## deals.json

One object per bank switch offer. Fields the app reads:

- `id` - stable slug, e.g. `first-direct-175`. The trailing `-NNN` mirrors the
  base bonus amount at the time of writing.
- `bankName` - shown in the app.
- `bonusAmount` - the base switch bonus in pounds.
- `boostAmount` - an optional time-limited extra on top of the base, shown
  separately in the app. Never merged into `bonusAmount`: the bank's terms
  document covers the base only.
- `boostWindow` - `{ "from", "to" }` dates (YYYY-MM-DD) for the boost, when
  there is one.
- `termsUrl` - the bank's OWN terms and conditions document for this offer.
  Every deal must have one: a deal without a `termsUrl` is silently excluded
  from the amount check, which is the check that catches the app advertising
  money the bank never promised.
- `sourceNote` - optional provenance for figures that come from the bank's
  marketing pages rather than its terms PDF (a boost is the usual case).
- `status`, `description`, `directSwitchUrl` - app copy and state.
- `directDebitsRequired`, `minDepositRequired`, `cardPaymentsRequired`,
  `appLoginRequired`, `appName`, `timeToBonusDays`, `depositHoldHours` - the
  offer's requirements, each read out of the bank's own terms document.
- `rules`, `exclusions`, `burnerFriendly` - the checklist the app shows.

## Rules

- Content updates are JSON-only. An offer change never needs an app rebuild.
- Every amount must be stated by the bank's own terms document. A comparison
  site (MoneySavingExpert, MoneySuperMarket, MoneyfactsCompare, any forum) is
  a hint, never a source.
- The app repo's watchers (`scripts/check-deals.mjs` against MoneySavingExpert,
  `scripts/check-terms.mjs` against each bank's own terms PDF) detect changes
  and report them for a human to verify and apply. Nothing here is changed by
  automation beyond the deliberately narrow amount update in
  `check-deals.mjs --apply`, which only ever changes a figure the source
  states outright.
- Never add an offer whose requirements and small print have not been written
  by hand from the bank's own T&C. A card without its real conditions can
  cost the user the bonus.
