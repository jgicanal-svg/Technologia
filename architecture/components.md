# UML Component Diagram — API Container

**Scope:** Our API container; the interfaces each component provides and requires; external services kept behind an adapter interface. 

```mermaid
flowchart TB

    %% =========================
    %% API CONTAINER
    %% =========================
    subgraph API["API Container - Next.js API Routes"]
        direction TB

        ListingComp["Listing Component"]
        VerificationComp["Verification Component"]
        InquiryComp["Inquiry Component"]
        AuthComp["Authentication Component"]
    end

    %% =========================
    %% API INTERFACES
    %% =========================
    IListingAPI(["«interface»<br/>IListingAPI"])
    IVerificationAPI(["«interface»<br/>IVerificationAPI"])
    IInquiryAPI(["«interface»<br/>IInquiryAPI"])
    IAuthAPI(["«interface»<br/>IAuthAPI"])

    %% =========================
    %% SERVICE INTERFACES
    %% =========================
    IListingRepository(["«interface»<br/>IListingRepository"])
    IImageStorage(["«interface»<br/>IImageStorage"])
    IMapsProvider(["«interface»<br/>IMapsProvider"])

    %% =========================
    %% EXTERNAL SERVICES
    %% =========================
    Supabase[("Supabase<br/>Database & Backend")]
    Cloudinary[("Cloudinary<br/>Image Storage")]
    GoogleMaps[("Google Maps<br/>Maps Service")]

    %% =========================
    %% PROVIDED INTERFACES
    %% =========================
    ListingComp -->|provides| IListingAPI
    VerificationComp -->|provides| IVerificationAPI
    InquiryComp -->|provides| IInquiryAPI
    AuthComp -->|provides| IAuthAPI

    %% =========================
    %% REQUIRED INTERFACES
    %% =========================
    ListingComp -->|requires| IListingRepository
    ListingComp -->|requires| IImageStorage
    ListingComp -->|requires| IMapsProvider

    VerificationComp -->|requires| IListingRepository
    VerificationComp -->|requires| IAuthAPI

    InquiryComp -->|requires| IListingRepository

    %% =========================
    %% ADAPTER / IMPLEMENTATION
    %% =========================
    IListingRepository -.->|implemented using| Supabase
    IImageStorage -.->|adapter wraps| Cloudinary
    IMapsProvider -.->|adapter wraps| GoogleMaps

    %% =========================
    %% STYLING
    %% =========================
    classDef component fill:#4338A0,stroke:#AAA,color:#FFF,stroke-width:1px;
    classDef interface fill:#4A4A46,stroke:#AAA,color:#FFF,stroke-width:1px;
    classDef external fill:#222,color:#FFF,stroke:#888,stroke-width:1px;

    class ListingComp,VerificationComp,InquiryComp,AuthComp component;
    class IListingAPI,IVerificationAPI,IInquiryAPI,IAuthAPI,IListingRepository,IImageStorage,IMapsProvider interface;
    class Supabase,Cloudinary,GoogleMaps external;
```

**Key:** rectangles = components; lollipop-style rounded nodes = interfaces; solid arrows = provides/requires; dashed arrows = an adapter wrapping an external service, so no component talks to Cloudinary, Google Maps, or Supabase directly.

**Traceability:** Keeping every external service (Cloudinary, Google Maps, Supabase) behind its own interface means a future swap — e.g., replacing Cloudinary with Supabase Storage — only touches the adapter, not the Listing or Verification components.
