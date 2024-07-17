# Vaultwarden Binary

This repository contains the pre-compiled binary releases of [Vaultwarden](https://github.com/dani-garcia/vaultwarden), the lightweight Bitwarden server API implementation written in Rust. This project aims to provide users with easy access to the Vaultwarden binaries for various platforms.

## Table of Contents

- [Vaultwarden Binary](#vaultwarden-binary)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Download](#download)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Configuration](#configuration)


## Introduction

Vaultwarden is an alternative implementation of the Bitwarden server API. It is lightweight and perfect for self-hosting password management solutions. This repository hosts the binary releases of Vaultwarden for various operating systems, allowing users to quickly download and run Vaultwarden without the need to compile it from source.

## Download

You can download the latest version of the Vaultwarden binary for your platform from the [releases page](https://github.com/nalakawula/vaultwarden-binary/releases).

Available platforms:
- Linux
- Windows
- macOS

## Installation

### Linux

1. Download the binary from the [releases page](https://github.com/nalakawula/vaultwarden-binary/releases).
2. Make the binary executable:
   ```bash
   chmod +x vaultwarden
   ```
3. Move the binary to a directory in your PATH, for example:
   ```bash
   sudo mv vaultwarden /usr/local/bin/
   ```

## Usage

Once the binary is installed, you can start the Vaultwarden server by running:

```bash
vaultwarden
```
By default, Vaultwarden will listen on port 8080. You can access the web vault by navigating to http://localhost:8080 in your web browser.

## Configuration

Vaultwarden can be configured using environment variables. For a full list of configuration options, refer to the official Vaultwarden documentation.

Example:

```bash
export ROCKET_PORT=8080
export DATABASE_URL=data/db.sqlite3
vaultwarden
```
