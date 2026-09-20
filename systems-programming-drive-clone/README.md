<div align="center">

# ☁️ Cloud Storage Platform

### A Google-Drive-like system, built from scratch

*Advanced Systems Programming Project*

<br>

![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-1572B6?style=flat-square&logo=css3&logoColor=white)
![Networking](https://img.shields.io/badge/Networking-4A5568?style=flat-square)
![Concurrency](https://img.shields.io/badge/Concurrency-6B46C1?style=flat-square)

<br>

[![Source Code](https://img.shields.io/badge/View_Full_Source_on_GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/OrBrener1/Drive-part-5)

</div>

---

## Overview

> **What does it actually take to build the thing everyone uses without thinking about it?**

A multi-client cloud storage application supporting file upload, download, sharing and synchronization between a server and multiple concurrent clients — the core mechanics behind a service like Google Drive, implemented from the ground up.

The project was built across several development stages, each layer adding capability on top of the last: from the underlying client–server communication, through storage and file management logic, up to a web interface users actually interact with.

<br>

## Architecture

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontFamily':'-apple-system, BlinkMacSystemFont, Segoe UI, Helvetica, Arial, sans-serif','fontSize':'13px','primaryColor':'#f1f5f9','primaryTextColor':'#334155','primaryBorderColor':'#cbd5e1','lineColor':'#94a3b8','clusterBkg':'#00000000','clusterBorder':'#e2e8f0','titleColor':'#64748b'}}}%%
flowchart LR
    subgraph CL [ Clients ]
        direction TB
        C1(["Client"])
        C2(["Client"])
        C3(["Client"])
    end

    W(["Web interface"])

    S["Server<br/><small>request handling · concurrency</small>"]
    L["Storage &amp; file<br/>management layer"]
    D[("File storage")]

    C1 --> S
    C2 --> S
    C3 --> S
    W --> S
    S --> L
    L --> D

    classDef client fill:#f8fafc,stroke:#cbd5e1,stroke-width:1px,color:#475569
    classDef core fill:#eef2ff,stroke:#c7d2fe,stroke-width:1px,color:#3730a3
    classDef store fill:#ecfdf5,stroke:#a7f3d0,stroke-width:1px,color:#065f46

    class C1,C2,C3,W client
    class S,L core
    class D store

    linkStyle default stroke:#cbd5e1,stroke-width:1.2px
```

<br>

## What it does

| Capability | Description |
|---|---|
| 📤 **File operations** | Upload, download, and manage files through the platform |
| 👥 **Multi-client support** | Multiple users connected concurrently to the same server |
| 🔄 **Synchronization** | Keeping client and server state consistent |
| 🌐 **Web interface** | Browser-based UI for interacting with the storage system |

<br>

## Stack

**Backend & core** · C++ · Java · Python
**Interface** · HTML · CSS
**Infrastructure** · Docker

<br>

## Engineering focus

This project was less about the feature list and more about the systems thinking underneath it — designing a **client–server architecture** that stays correct under concurrent access, handling **network communication** between independent processes, and structuring **storage logic** that multiple clients can safely share.

<br>

---

<div align="center">

**[→ Explore the full implementation](https://github.com/OrBrener1/Drive-part-5)**

</div>
