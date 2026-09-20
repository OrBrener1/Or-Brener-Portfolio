<div align="center">

# ☁️ Cloud Storage Platform

### A Google-Drive-like system, built from scratch

*Advanced Systems Programming Project*

<br>

![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
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
flowchart TB
    subgraph Clients
        C1["💻 Client"]
        C2["💻 Client"]
        C3["💻 Client"]
    end
    W["🌐 Web Interface"]
    S["⚙️ Server<br/>request handling · concurrency"]
    L["📂 Storage & File<br/>Management Layer"]
    D[("🗄️ File Storage")]

    C1 --> S
    C2 --> S
    C3 --> S
    W --> S
    S --> L
    L --> D
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

## Engineering focus

This project was less about the feature list and more about the systems thinking underneath it — designing a **client–server architecture** that stays correct under concurrent access, handling **network communication** between independent processes, and structuring **storage logic** that multiple clients can safely share.

<br>

---

<div align="center">

**[→ Explore the full implementation](https://github.com/OrBrener1/Drive-part-5)**

</div>
