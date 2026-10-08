# Sequence Diagram — Student Views a Listing and Sends an Inquiry

**Scope:** Our riskiest flow (it is literally our JVB success metric); replies drawn; branches shown with alt/opt; an external system (Facebook Messenger) as a lifeline.

```mermaid
sequenceDiagram

    actor Student
    participant Page as Listing Page<br/>(Next.js)
    participant API as Inquiry API Route
    participant DB as Supabase<br/>(Database)
    participant Messenger as Facebook Messenger

    %% =========================
    %% VIEW LISTING
    %% =========================

    Student->>Page: Open verified listing

    Page->>API: GET /api/listings/:id

    API->>DB: Fetch listing, photos, and owner

    DB-->>API: Return listing data

    API-->>Page: 200 OK - Listing JSON

    Page-->>Student: Display listing details


    %% =========================
    %% SEND INQUIRY
    %% =========================

    Student->>Page: Click "Message Owner"

    Page->>API: POST /api/listings/:id/inquiries

    API->>DB: Save inquiry<br/>(listingId, timestamp)

    DB-->>API: Inquiry saved

    API-->>Page: 201 Created<br/>Messenger link


    %% =========================
    %% MESSENGER REDIRECT
    %% =========================

    opt Owner has a Messenger link
        Page->>Messenger: Redirect to Messenger<br/>with pre-filled message

        Messenger-->>Student: Open owner chat
    end


    %% =========================
    %% FALLBACK
    %% =========================

    alt Owner has no Messenger link
        Page-->>Student: Display owner's phone number
    end
```

**Key:** solid arrows = request, dashed arrows = reply; `opt` = optional branch; `alt` = either/or branch. 

**Why this is our riskiest flow:** every inquiry logged here is exactly what our JVB Success Criterion counts ("at least 15 of ~50 students message the post within 5 days"). If this flow breaks or undercounts, we cannot tell whether an experiment passed or failed.
