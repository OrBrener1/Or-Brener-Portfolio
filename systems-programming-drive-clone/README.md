<div align="center">

<img src="assets/hero.svg" alt="Cloud Storage Platform. Drive Me Nuts, a Google Drive-style system with web and mobile clients. Full-stack course project, Advanced Systems Programming. C++ storage server, Node.js REST API, MongoDB, React, React Native." width="100%">

<br><br>

![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![React Native](https://img.shields.io/badge/React_Native-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

<br>

<a href="#overview"><img src="assets/nav-overview.svg" alt="Overview" height="40"></a>&nbsp;
<a href="#architecture"><img src="assets/nav-architecture.svg" alt="Architecture" height="40"></a>&nbsp;
<a href="#inside-the-code"><img src="assets/nav-code.svg" alt="Inside the code" height="40"></a>&nbsp;
<a href="#design-decisions"><img src="assets/nav-decisions.svg" alt="Design decisions" height="40"></a>&nbsp;
<a href="#stack"><img src="assets/nav-stack.svg" alt="Stack" height="40"></a>

</div>

<br>

## Overview

Drive Me Nuts is a Google Drive-style storage system, built as a full-stack project in the Advanced Systems Programming course. Users register, organize files and folders, share them with other users through hierarchical permissions, search, star items and recover deleted ones from a bin. The same features are available in a React web client and a React Native mobile app, both running on one REST API.

The system was built in stages over the course: first the C++ storage server, then the Node.js backend and MongoDB, and finally the two clients.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/facts-dark.svg">
  <img alt="2 clients on one REST API, 4 Docker Compose services, 4 C++ storage commands, 4 worker threads per server" src="assets/facts-light.svg" width="100%">
</picture>

<br><br>

<div align="center">
<a href="https://github.com/OrBrener1/Drive-part-5"><img src="assets/link-source.svg" alt="Source code" height="40"></a>&nbsp;
<a href="https://github.com/OrBrener1/Drive-part-5/wiki"><img src="assets/link-wiki.svg" alt="Project wiki" height="40"></a>
</div>

<br>

## Architecture

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/architecture-dark.svg">
  <img alt="Architecture: a React web client and a React Native mobile app call a Node.js REST API. The API stores users, metadata, permissions, bin and stars in MongoDB and sends file contents over TCP to a C++ storage server that writes them to disk. The server side runs in Docker Compose." src="assets/architecture-light.svg" width="100%">
</picture>

The Node.js API handles everything the user interacts with: authentication with JSON Web Tokens, file and folder metadata, permissions, search, the bin and starred items, all stored in MongoDB. File contents go to a separate C++ storage server over a plain TCP socket. That server hands each connection to a fixed thread pool, executes GET, POST, DELETE and SEARCH commands, and stores files on disk with run-length encoding. One `docker-compose` file brings up the storage server, MongoDB, the API and the web client together.

<br>

## Inside the code

<details>
<summary>Module-level map of the repository, generated with <a href="https://gitdiagram.com/orbrener1/drive-part-5">GitDiagram</a></summary>

<br>

```mermaid
flowchart TD

subgraph group_web["React Web Client"]
  node_web_routes["Web Routes<br/>[router.js]"]
  node_web_auth["Web Authentication<br/>[AuthContext.jsx]"]
  node_web_files["Web File Workflows<br/>[FilesPage.jsx]"]
  node_web_api["Web API Client<br/>[apiClient.js]"]
end

subgraph group_mobile["React Native Client"]
  node_mobile_screens["Mobile Screens"]
  node_mobile_actions["Mobile File Actions<br/>[useFileActions.js]"]
  node_mobile_auth["Mobile Authentication<br/>[AuthContext.jsx]"]
  node_mobile_api["Mobile API<br/>[filesApi.js]"]
end

subgraph group_backend["Node.js Backend"]
  node_http_api["REST API<br/>[app.js]"]
  node_api_routes["File and Search Routes<br/>[files.js]"]
  node_auth_middleware["Token Authentication<br/>[authService.js]"]
  node_file_controller["File Operations<br/>[FilesController.js]"]
  node_file_service["File Domain Service<br/>[filesService.js]"]
  node_metadata_repo["File Metadata Repository"]
  node_sharing["Permissions and Sharing"]
  node_search["Search"]
  node_user_services["User Services<br/>[userService.js]"]
  node_file_state["Bin, Recent, Stars"]
  node_cpp_client["Storage Protocol Client"]
end

subgraph group_storage["C++ Storage Server"]
  node_cpp_handler["Socket Command Handler"]
  node_command_set["Storage Commands"]
  node_local_storage[("Local File Storage")]
  node_compression["Binary Compression"]
end

node_browser_user(("Browser User"))
node_mobile_user(("Mobile User"))

node_browser_user -->|"uses"| node_web_routes
node_web_routes -->|"checks session"| node_web_auth
node_web_routes -->|"routes to"| node_web_files
node_web_files -.->|"requests data"| node_web_api
node_web_auth -->|"auth requests"| node_web_api
node_web_api -->|"calls REST API"| node_http_api
node_mobile_user -->|"uses"| node_mobile_screens
node_mobile_screens -.->|"uses actions"| node_mobile_actions
node_mobile_actions -->|"calls"| node_mobile_api
node_mobile_auth -.->|"auth requests"| node_mobile_api
node_mobile_api -->|"calls REST API"| node_http_api
node_http_api -->|"mounts routes"| node_api_routes
node_api_routes -->|"protects routes"| node_auth_middleware
node_api_routes -->|"dispatches requests"| node_file_controller
node_file_controller -->|"calls"| node_file_service
node_file_service -->|"reads and writes"| node_metadata_repo
node_file_service -->|"requests file I/O"| node_cpp_client
node_file_controller -.->|"permission endpoints"| node_sharing
node_sharing -->|"checks access"| node_file_service
node_sharing -->|"resolves users"| node_user_services
node_api_routes -->|"routes search"| node_search
node_search -.->|"searches files"| node_file_service
node_file_service -->|"manages file state"| node_file_state
node_cpp_client -.->|"sends commands"| node_cpp_handler
node_cpp_handler -->|"dispatches commands"| node_command_set
node_command_set -.->|"performs file I/O"| node_local_storage
node_local_storage -.->|"uses strategy"| node_compression

classDef toneBlue fill:#dbeafe,stroke:#2563eb,stroke-width:1.5px,color:#172554
classDef toneAmber fill:#fef3c7,stroke:#d97706,stroke-width:1.5px,color:#78350f
classDef toneMint fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px,color:#14532d
classDef toneRose fill:#ffe4e6,stroke:#e11d48,stroke-width:1.5px,color:#881337
class node_web_routes,node_web_auth,node_web_files,node_web_api,node_browser_user,node_mobile_user toneBlue
class node_mobile_screens,node_mobile_actions,node_mobile_auth,node_mobile_api toneAmber
class node_http_api,node_api_routes,node_auth_middleware,node_file_controller,node_file_service,node_metadata_repo,node_sharing,node_search,node_user_services,node_file_state,node_cpp_client toneMint
class node_cpp_handler,node_command_set,node_local_storage,node_compression toneRose
```

</details>

Good starting points in the source:

| Layer | File |
|---|---|
| Web client routing | [`react-front/src/router.js`](https://github.com/OrBrener1/Drive-part-5/blob/main/react-front/src/router.js) |
| Mobile file actions | [`react-native-app/src/hooks/useFileActions.js`](https://github.com/OrBrener1/Drive-part-5/blob/main/react-native-app/src/hooks/useFileActions.js) |
| REST API entry point | [`NodeJS-server/src/app.js`](https://github.com/OrBrener1/Drive-part-5/blob/main/NodeJS-server/src/app.js) |
| File domain logic | [`NodeJS-server/src/services/filesService.js`](https://github.com/OrBrener1/Drive-part-5/blob/main/NodeJS-server/src/services/filesService.js) |
| Link to the C++ server | [`NodeJS-server/src/services/cppClientService.js`](https://github.com/OrBrener1/Drive-part-5/blob/main/NodeJS-server/src/services/cppClientService.js) |
| C++ server and thread pool | [`cpp-server/src/server/`](https://github.com/OrBrener1/Drive-part-5/tree/main/cpp-server/src/server) |

<br>

## Design decisions

- Permissions are hierarchical: sharing a folder gives the same access to everything inside it. Starring is personal and applies to a single item, so one user's organization never changes what another user sees.
- Only the owner can delete an item for everyone. When a user it was shared with deletes it, the item leaves that user's workspace and their permission is revoked, while the owner and the other users keep it.
- Moving a folder to the bin hides its whole subtree from the file views and from search until the folder is restored.

<br>

## Stack

**Server side** · C++ · Node.js · Express · MongoDB (Mongoose) · JSON Web Tokens<br>
**Clients** · React · React Native (Expo)<br>
**Infrastructure** · Docker · Docker Compose
