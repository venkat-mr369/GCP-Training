flowchart TD

    A[Users on Web/Mobile GUIs] --> B[Firewall / WAF]
    B --> C[API Gateway]
    C --> D[Frontend Dashboards & Client Interfaces]
    C --> E[Backend Servers / Application Logic]
    E --> F[Middleware for Real-time Management]
    E --> G[Databases - SQL/NoSQL]
    E --> H[Object Storage - Blobs/Files]
    D --> I[Compute - Virtual Machines / Containers]
    F --> I
    G --> J[Virtual Network]
    H --> J
    I --> J

    subgraph Cross-Cutting Services
        K[Identity & Access Management - IAM]
        L[Monitoring & Logging]
        M[Encryption Services - Key Vault]
        N[Redundancy & Backup]
    end

    K -.-> D & E & F & G & H & I
    L -.-> D & E & F & G & H & I
    M -.-> D & E & F & G & H & I
    N -.-> D & E & F & G & H & I
