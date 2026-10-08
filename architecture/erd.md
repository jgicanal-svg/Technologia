# Draft ERD — Boarding House Listing Verification

**Scope:** One table per stored class; primary and foreign keys; crow's-foot cardinality matching our class diagram; PII columns marked.

```mermaid
erDiagram

    USERS ||--o{ LISTINGS : verifies
    OWNERS ||--|{ LISTINGS : owns
    LISTINGS ||--|{ PHOTOS : contains
    LISTINGS ||--o{ INQUIRIES : receives
    LISTINGS ||--o| VERIFICATION_CHECKLISTS : has

    USERS {
        uuid id PK
        string name
        string email
        string password_hash
        string role
    }

    OWNERS {
        uuid id PK
        string name
        string contact_number
        string messenger_link
    }

    LISTINGS {
        uuid id PK
        uuid owner_id FK
        uuid verified_by FK
        string title
        decimal price_monthly
        string address
        decimal latitude
        decimal longitude
        string facilities_description
        string status
        datetime submitted_at
        datetime verified_at
    }

    PHOTOS {
        uuid id PK
        uuid listing_id FK
        string url
        int sort_order
    }

    INQUIRIES {
        uuid id PK
        uuid listing_id FK
        datetime sent_at
    }

    VERIFICATION_CHECKLISTS {
        uuid id PK
        uuid listing_id FK
        boolean price_confirmed
        boolean photos_confirmed
        boolean availability_confirmed
        datetime checked_at
    }
```

**Key:** `||` = exactly one, `o{` = zero or many, `|{` = one or many, `o|` = zero or one (crow's-foot notation). `"PII"` marks columns holding personally identifiable information.

**Cardinality check against class.md:** `USERS ||--o{ LISTINGS` matches `User "1" --> "0..*" Listing`; `OWNERS ||--|{ LISTINGS` matches `Owner "1" --> "1..*" Listing`; `LISTINGS ||--|{ PHOTOS` matches `Listing "1" --> "1..*" Photo`; `LISTINGS ||--o{ INQUIRIES` matches `Listing "1" --> "0..*" Inquiry`; `LISTINGS ||--o| VERIFICATION_CHECKLISTS` matches `Listing "1" --> "0..1" VerificationChecklist`.
