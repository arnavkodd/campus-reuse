# Campus Reuse — Spec

Campus marketplace app for University of Michigan students to buy, sell, and
donate used items (with a dedicated textbook search) within a verified
@umich.edu community.

## Platform

- **Backend: Supabase** (Postgres, Auth, Storage, Realtime), chosen over AWS
  for faster iteration and built-in auth/storage/realtime without stitching
  together separate AWS services.
- **Client:** Expo (React Native). See [AGENTS.md](AGENTS.md) — Expo has
  changed significantly; consult the versioned docs at
  https://docs.expo.dev/versions/v57.0.0/ before writing Expo code.

## Identity & Access

- **@umich.edu one-time verification, permanent access.** Users verify a
  `@umich.edu` email once (e.g. magic link / OTP to the address). Once
  verified, access does not expire or require re-verification of school
  affiliation.
- **Sign-in: password + magic-link.** Users can sign in with a traditional
  password or a passwordless magic-link sent to their verified email.
- **Time-based biometric re-auth.** After sign-in, the app can require
  biometric confirmation (Face ID / fingerprint) to re-enter the app after a
  period of inactivity, rather than a full re-login.
- **SMS phone verification.** In addition to email, users verify a phone
  number via SMS — used for trust/safety (e.g. contact for exchanges,
  reducing fake accounts).

## Listings

- **Fixed categories + tags.** Listings belong to a fixed, app-defined set of
  categories, with free-form tags layered on top for finer filtering (no
  user-created categories).
- **Textbook search by course number.** Textbooks are a first-class search
  path: users can search/filter listings by course number (e.g. `EECS 280`)
  in addition to title/author.
- **Location: area + detail.** Listings capture a coarse area (e.g. dorm/zone
  on or near campus) plus a free-text detail field, rather than precise
  GPS/pinpoint coordinates.
- **Structured offers with price-locking.** Buyers submit structured offers
  (not just chat messages) against a listing; once an offer is accepted the
  price is locked for that transaction rather than continuing to float.

## Messaging

- **One thread per (listing, buyer).** Conversations are scoped to a specific
  listing and buyer pair — a buyer messaging a seller about two different
  listings gets two separate threads.

## Trust & Safety

- **Block / report / prohibited-items + in-app admin moderation screen.**
  Users can block other users and report listings/users. A defined
  prohibited-items policy is enforced. Reports are triaged through an
  in-app admin moderation screen (no external ticketing tool required).

## Search

- **Postgres full-text search.** Listing search (title, description, course
  number, tags) is powered by Postgres's built-in full-text search (e.g.
  `tsvector`/`tsquery`), not a separate search service (e.g. Algolia,
  Elasticsearch).

## Navigation / IA

- **Home = Browse.** The app's home tab is the browse/listings feed, not a
  separate dashboard.
- **5-tab bar, Donate as its own tab.** Bottom navigation has 5 tabs, with
  Donate broken out as a first-class tab (not nested under Sell/Post) to
  emphasize donation as a distinct flow from selling.

## Deferred (explicitly out of scope for now)

- Safe-exchange parking spots (designated on-campus meetup locations).
- Sublease listings.
- Storage unit listings/rentals.
