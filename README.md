# DamageClaims
```mermaid
flowchart LR
  classDef auto fill:#E8F5E9,stroke:#777,rx:6,ry:6;
  classDef decision fill:#FFF3E0,stroke:#777;
  classDef human fill:#E3F2FD,stroke:#777,rx:6,ry:6;
  classDef data fill:#F3E5F5,stroke:#777;

  A[Email Received (Notification or Invoice)]:::auto --> B[Auto-classify & Create/Update Claim\n- Save attachments\n- Extract invoice fields]:::auto
  B --> Z[(SharePoint: Claims, Files, Config)]:::data
  B --> C{Complex Case?}:::decision

  C -- Yes --> D[Awaiting Legal Advice\n(Teams Approval)]:::human
  C -- No --> E[Notify Driver for Statement/Appeal\n(Email + Form/Pages)]:::auto

  D --> F[Legal Advice Recorded]:::human --> E

  E --> G{Driver Appeals?}:::decision
  G -- Yes --> H[Under Appeal\n(Legal Review & Decision)]:::human
  G -- No --> I[Decisioning & Policy Rules\n(Thresholds from Config)]:::auto
  H --> I

  I --> J{Route to Company Pays or Insurer?}:::decision

  J -- Company Pays --> K[Finance Approval (if needed)]:::human --> L[Awaiting Payment (AP)]:::auto --> M[Payment Confirmed]:::auto --> X[Closed]
  J -- Insurer --> N[Send Insurer Pack + CC Driver]:::auto --> O[With Insurer (Weekly reminders)]:::auto --> P{Settlement Received?}:::decision
  P -- No --> O
  P -- Yes --> Q{Excess Invoice Received?}:::decision
  Q -- Yes --> R[Awaiting Excess Payment]:::auto --> X[Closed]
  Q -- No --> X

  Z -.-> S[(Power BI Dashboard)]:::data
  X -. Archive/Retain .-> Z
