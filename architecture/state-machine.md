# State Machine Diagram — Listing Status

**Scope:** Our main entity (the object with a status field); initial and final states; states named as conditions; every transition labelled with its event.

```mermaid
stateDiagram-v2

    [*] --> AwaitingVerification : Owner submits listing

    AwaitingVerification --> Verified : Verifier approves<br/>[Checklist complete]

    AwaitingVerification --> Rejected : Verifier rejects<br/>[Checklist failed]

    Verified --> Reserved : Student confirms interest<br/>[Offline confirmation]

    Verified --> Taken : Owner reports room filled

    Verified --> Expired : 14 days without update<br/>[Timeout]

    Reserved --> Taken : Move-in confirmed

    Reserved --> Verified : Reservation cancelled<br/>or falls through

    Rejected --> AwaitingVerification : Owner resubmits<br/>after fixing issues

    Taken --> [*]

    Expired --> [*]
```

**Key:** `[*]` = initial/final pseudo-state; arrow labels after the colon are the triggering event, bracketed text is the guard condition.
 
**Traceability:** These six states are exactly the six values of the `ListingStatus` enumeration in `class.md`. `AwaitingVerification → Verified` is the same decision point drawn as a guarded branch in `activity.md`.
