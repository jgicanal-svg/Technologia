# Activity Diagram — Listing Submission & Verification Workflow

**Scope:** Our core workflow; one swimlane per role and one for the system; every decision labelled with guards; start and end nodes.

```mermaid
flowchart TB

    %% =========================
    %% OWNER / AGENT
    %% =========================
    subgraph OWNER["OWNER / AGENT"]
        direction TB

        A([Start<br/>Has a room to list])
        B["Fill out form<br/>Price, photos, address"]
        C["Submit listing"]

        L([Listing live<br/>Visible to students])
        R([Resubmit<br/>Owner may retry])

        A --> B
        B --> C
    end


    %% =========================
    %% SYSTEM
    %% =========================
    subgraph SYSTEM["SYSTEM"]
        direction TB

        D["Save listing<br/>Awaiting Verification"]
        E["Notify verifier<br/>New submission"]

        J["Publish listing<br/>To student feed"]
        K["Notify owner<br/>Includes reason"]

        D --> E
    end


    %% =========================
    %% TEAM VERIFIER
    %% =========================
    subgraph VERIFIER["TEAM VERIFIER"]
        direction TB

        F["Visit in person<br/>Checklist inspection"]
        G{"Checks pass?"}

        H["Mark verified<br/>Checklist complete"]
        I["Mark rejected<br/>Reason required"]

        F --> G
        G -->|Yes| H
        G -->|No| I
    end


    %% =========================
    %% MAIN FLOW
    %% =========================

    C --> D
    E --> F

    H --> J
    J --> L

    I --> K
    K --> R


    %% =========================
    %% STYLING
    %% =========================

    classDef purple fill:#4338A0,color:#FFFFFF,stroke:#AAAAAA;
    classDef gray fill:#484844,color:#FFFFFF,stroke:#AAAAAA;
    classDef decision fill:#4338A0,color:#FFFFFF,stroke:#AAAAAA;

    class B,C,F,H,I purple;
    class D,E,J,K gray;
    class G decision;
    class A,L,R gray;
```

**Key:** each outlined box is a swimlane (one role or the system), read left to right in the order responsibility passes; rounded nodes = start/end; rectangles = actions; diamond = decision, with the guard condition in brackets on each outgoing arrow.

**Traceability:** This is the workflow behind Experiments 1 and 2 on our JVB: a team member personally checks a listing before it reaches students. The "No" branch is our own addition, because our riskiest assumption (#3, owners willing to be verified) is still unvalidated, so some owners may decline or fail the check.
