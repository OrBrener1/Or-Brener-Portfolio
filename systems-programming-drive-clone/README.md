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
<a href="#how-its-built"><img src="assets/nav-code.svg" alt="How it's built" height="40"></a>&nbsp;
<a href="#design-decisions"><img src="assets/nav-decisions.svg" alt="Design decisions" height="40"></a>&nbsp;
<a href="#stack"><img src="assets/nav-stack.svg" alt="Stack" height="40"></a>

</div>

<br>

## Overview

Drive Me Nuts is a Google Drive-style storage system, built as a full-stack project in the Advanced Systems Programming course. Users register, organize files and folders, share them with other users through hierarchical permissions, search, star items and recover deleted ones from a bin. The same features are available in a React web client and a React Native mobile app, both running on one REST API.

The system was built in stages over the course: first the C++ storage server, then the Node.js backend and MongoDB, and finally the two clients.

<div align="center">
<a href="https://github.com/OrBrener1/Drive-part-5"><img src="assets/link-source.svg" alt="Source code" height="40"></a>&nbsp;
<a href="https://github.com/OrBrener1/Drive-part-5/wiki"><img src="assets/link-wiki.svg" alt="Project wiki" height="40"></a>
</div>

<br>

## Architecture

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/architecture-dark.svg">
  <img alt="Architecture: a React web client and a React Native mobile app call a Node.js REST API. The API stores users, metadata, permissions, bin and stars in MongoDB and sends file contents over TCP to a C++ storage server that keeps them in a Docker volume. The server side runs in Docker Compose." src="assets/architecture-light.svg" width="100%">
</picture>

The Node.js API handles everything the user interacts with: authentication with JSON Web Tokens, file and folder metadata, permissions, search, the bin and starred items, all stored in MongoDB. File contents go to a separate C++ storage server over a plain TCP socket. That server hands each connection to a fixed thread pool, executes GET, POST, DELETE and SEARCH commands, and stores files, run-length encoded, in a Docker volume. One `docker-compose` file brings up the storage server, MongoDB, the API and the web client together.

<br>

## How it's built

Each service is split into layers that each do one job. In the Node.js backend a request passes from the routes, where its JWT is checked, to a controller, then to a service that holds the file, sharing and search logic, and finally to a repository that reads and writes MongoDB. HTTP handling, business rules and data access stay apart, in the spirit of MVC. The C++ server is layered the same way: the TCP server hands each connection to a thread pool, a parser turns the request into a command, and each command works through a file manager that compresses what it stores in a Docker volume.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/modules-dark.svg">
  <img alt="Layer map. The web client and the mobile app call the Node.js backend over REST: routes with JWT authentication, controllers, services, and repositories and models over MongoDB. The services layer talks over TCP to the C++ storage server: TCP server with a thread pool, parser, commands (GET, POST, DELETE, SEARCH) and a file manager with RLE compression, writing to a Docker volume." src="assets/modules-light.svg" width="100%">
</picture>

We designed the code to stay maintainable as it grew over the course's stages. The object-oriented C++ server follows SOLID: every command, connection, parser, file manager and compression method sits behind its own interface, so new commands or storage strategies plug in without rewriting the code around them. Across the project we kept to KISS, DRY and separation of concerns, with loosely coupled services that talk only through the REST API and the TCP protocol.

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

<br>

> [!NOTE]
> Security was outside the scope of the course, so the system was not hardened for real-world use. Requests are authenticated with JWTs, but data protection beyond that, such as encrypted transport and secure password storage, was not implemented.
