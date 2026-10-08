# Architecture Design Document (ADD) — Boarding House Listing Verification
**Team Technologia · CC 106b, Unit 3, Activity 7**

This folder holds the 11 architectural views for our MVP: **Boarding House Listing Verification**, a lightweight verification layer built on top of the "Bulan Barter" Facebook group we already validated in our Javelin Validation Board (Experiment 1).

## How this connects to our earlier activities
- **Activity 2 (Brainstorming):** the idea was Jemar's *boarding house listing/finder web app*, where owners post rooms (price, location, contact) and students browse and filter. It scored 23/30, the highest of four ideas. Technical feasibility scored 5/5 because it needs only core CRUD features (forms, lists, basic auth), which is exactly what these diagrams describe.
- **Activity 4 (Interviews + JVB):** three in-segment interviews (C.G., J.G., D.G.) showed students lose fare, load and time to outdated or taken listings. Experiment 1 got **8 of ~50** messages against a bar of 15 (30%), so the verdict was **PIVOT**: add facilities, exact location, and current availability to each listing. That is why `Listing` carries `facilitiesDescription`, `lat`/`lng`, and a `status` for availability.
- **Experiment 2 (next):** the same 6-listing test with full details, ~50 students, same 15-message bar.
- **Still unvalidated:** (1) owners letting a student verify their listing (JVB riskiest assumption #3) and (2) the revenue model, a small owner fee to feature a listing (Activity 2). The fee is **not** modelled in these diagrams, because the team has not validated it; add a `featured` flag only after it is tested.
- **Honest scope note:** the experiments so far ran as posts in the existing Bulan Barter Facebook group, with no new app. These diagrams describe the *planned* web app MVP that grows out of Activity 2's idea; confirm it against your Unit 2 architecture decision before treating it as final.

## MVP summary (from our Lean Canvas / JVB)
- **Customer:** 1st-year and 4th-year (transferee) SSU Bulan students without a fixed boarding house.
- **Core problem:** students lose fare, load, and days to outdated listings and unverified posts.
- **Solution:** a team member personally verifies each boarding house's price, photos, and real-time availability before it is posted, and the MVP now gives that verified feed its own browsable app on top of the Facebook group.

## Diagram index

| # | Diagram | File | Owner |
|---|---|---|---|
| 1 | C4 system context | `context.md` | Jemar Manorina |
| 2 | C4 container | `containers.md` | Jemar Manorina |
| 3 | Use case | `use-cases.md` | Jhon Michael Gicanal |
| 4 | Activity | `activity.md` | Jhon Michael Gicanal |
| 5 | Sequence | `sequence.md` | Jyan Manorina |
| 6 | Class | `class.md` | John Loren Habal |
| 7 | State machine | `state-machine.md` | John Loren Habal |
| 8 | Package | `packages.md` | Jyan Manorina |
| 9 | UML component | `components.md` | Jyan Manorina |
| 10 | Deployment | `deployment.md` | Jemar Manorina |
| 11 | ERD (draft) | `erd.md` | John Loren Habal |

*(Division follows the course's suggested 4-member split in Section 5 of the activity sheet: Member A = diagrams 1, 2, 10; Member B = diagrams 3, 4; Member C = diagrams 5, 8, 9; Member D = diagrams 6, 7, 11. Reassign names/diagrams here if your team divided the work differently.)*

## Cross-view consistency checks already done
- **Actors** (Student Seeker, Boarding House Owner/Agent, Team Verifier) are identical across `context.md`, `containers.md`, `use-cases.md`, and `deployment.md`.
- **`ListingStatus`** has the same six values in `class.md`, `state-machine.md`, and the `status` column of `erd.md`.
- **Cardinalities** in `erd.md` are cross-checked line-by-line against the multiplicities in `class.md` (see the note at the bottom of `erd.md`).
- **External systems** (Facebook Messenger, Google Maps, Cloudinary, PostgreSQL/Supabase) appear consistently from `context.md` down through `containers.md`, `components.md`, and `deployment.md` — nothing is introduced at one layer and dropped at another.

## Still open
- `erd.md` and `class.md` assume a Supabase/PostgreSQL + Cloudinary stack; confirm this against your team's actual Unit 2 architectural-style decision before treating it as final.
- `deployment.md` is explicitly Provisional — no hosting accounts exist yet.
