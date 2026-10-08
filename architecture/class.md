# Class Diagram — Boarding House Listing Verification Domain Model

**Scope:** 4–10 domain classes with typed attributes; multiplicity at both ends of every association; an enumeration for every status field.

```mermaid
classDiagram
    class User {
        +UUID id
        +string name
        +string email
        +string passwordHash
        +Role role
    }
    class Role {
        <<enumeration>>
        VERIFIER
        ADMIN
    }
    class Listing {
        +UUID id
        +string title
        +decimal priceMonthly
        +string address
        +decimal lat
        +decimal lng
        +string facilitiesDescription
        +ListingStatus status
        +datetime submittedAt
        +datetime verifiedAt
    }
    class ListingStatus {
        <<enumeration>>
        AWAITING_VERIFICATION
        VERIFIED
        REJECTED
        RESERVED
        TAKEN
        EXPIRED
    }
    class Photo {
        +UUID id
        +string url
        +int sortOrder
    }
    class Owner {
        +UUID id
        +string name
        +string contactNumber
        +string messengerLink
    }
    class Inquiry {
        +UUID id
        +datetime sentAt
    }
    class VerificationChecklist {
        +UUID id
        +boolean priceConfirmed
        +boolean photosConfirmed
        +boolean availabilityConfirmed
        +datetime checkedAt
    }

    User "1" --> "0..*" Listing : verifies
    Owner "1" --> "1..*" Listing : lists
    Listing "1" --> "1..*" Photo : has
    Listing "1" --> "0..*" Inquiry : receives
    Listing "1" --> "0..1" VerificationChecklist : checked by
    Listing --> ListingStatus
    User --> Role
```

**Key:** `+` = public attribute; `<<enumeration>>` = a fixed set of allowed values; `"1" --> "0..*"` reads as "one of the left side relates to zero or more of the right side."

**Traceability:** `VerificationChecklist` attributes mirror the actual check our team verifier performs (price, photos, availability) as described in our Solution box on the Lean Canvas and JVB. `Inquiry` is the class our JVB success metric counts. `ListingStatus` values match the Activity and State Machine diagrams exactly. 
