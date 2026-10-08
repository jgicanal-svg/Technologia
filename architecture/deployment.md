# Deployment Diagram — Provisional

**Scope:** Titled "Provisional"; nodes, execution environments, and artifacts; a protocol on every path; no secrets or real addresses.

```mermaid
flowchart LR

    %% =========================
    %% CLIENT DEVICE
    %% =========================

    subgraph ClientNode["Node: Student / Owner / Verifier Device"]
        direction TB

        Browser["Execution Environment:<br/>Mobile / Desktop Browser"]
    end


    %% =========================
    %% VERCEL
    %% =========================

    subgraph VercelNode["Node: Vercel Edge Platform"]
        direction TB

        WebAppArtifact["Artifact:<br/>Boarding House Listing Verification<br/>(Next.js Application)"]
    end


    %% =========================
    %% SUPABASE
    %% =========================

    subgraph DBNode["Node: Supabase Cloud Platform"]
        direction TB

        DBArtifact[("Artifact:<br/>BBV Production Database")]
    end


    %% =========================
    %% CLOUDINARY
    %% =========================

    subgraph CloudinaryNode["Node: Cloudinary<br/>(External SaaS)"]
        direction TB

        ImgArtifact[("Artifact:<br/>Listing Photos Storage")]
    end


    %% =========================
    %% FACEBOOK MESSENGER
    %% =========================

    subgraph MessengerNode["Node: Meta Platforms<br/>(External SaaS)"]
        direction TB

        MsgArtifact["Artifact:<br/>Facebook Messenger"]
    end


    %% =========================
    %% DEPLOYMENT CONNECTIONS
    %% =========================

    Browser -->|"HTTPS"| WebAppArtifact

    WebAppArtifact -->|"HTTPS / Supabase API"| DBArtifact

    WebAppArtifact -->|"HTTPS"| ImgArtifact

    WebAppArtifact -->|"HTTPS Redirect"| MsgArtifact


    %% =========================
    %% STYLING
    %% =========================

    classDef client fill:#08427b,stroke:#052e56,color:#ffffff,stroke-width:1px;
    classDef ours fill:#1168bd,stroke:#0b4884,color:#ffffff,stroke-width:1px;
    classDef external fill:#999999,stroke:#6b6b6b,color:#ffffff,stroke-width:1px;

    class Browser client;
    class WebAppArtifact,DBArtifact ours;
    class ImgArtifact,MsgArtifact external;
```

**Key:** each outlined box is a node (a physical device or a cloud execution environment), read left to right as a request travels outward; the inner box or cylinder inside each node is the artifact (a deployed build or dataset) running there; every arrow carries its protocol.

**Why "Provisional":** we have not yet signed up for Vercel, Supabase, or Cloudinary accounts — these are our planned providers based on the Next.js stack, not confirmed infrastructure. No real hostnames, API keys, or connection strings appear here.
