# Advanced File Sharing — Python TCP Prototype

A **Python client–server file-sharing proof of concept** exploring socket programming, concurrent connections, binary file transfers, integrity verification, and desktop interfaces.

Built as a collaborative academic software project. This is a **clean, source-only copy** prepared for presentation on GitHub. It does not include the original repository's virtual environment, sample user database, logs, uploaded/downloaded test files, or duplicate archive.

## What the project does

- **Client–server networking:** a Python TCP server accepts commands from clients over local sockets.
- **Concurrent connections:** server-side threads handle connected clients independently.
- **File operations:** list, upload, download, and (via the admin demonstration interface) delete files.
- **Integrity verification:** SHA-256 digests are calculated and compared after file transfers.
- **Transfer progress:** a text-based progress bar, speed, and estimated remaining time in the command-line downloader; progress feedback in the Tkinter GUI.
- **Download recovery prototype:** the command-line client implements JSON checkpoints and an experimental resume request.
- **Basic version history:** the server attempts to archive a previous file when the same name is uploaded again.
- **Two client interfaces:** a command-line client and a Tkinter desktop client.
- **Local account demonstration:** a second Tkinter interface uses SQLite for simple local login and an admin view, with a demo database generated on demand.
- **Logging:** local client and server activity logs.


## Tech stack

| Area | Technologies |
| --- | --- |
| Application | Python 3 |
| Networking | `socket` (TCP) |
| Concurrency | `threading` |
| Desktop GUI | Tkinter |
| File integrity | `hashlib` / SHA-256 |
| Local account demo | SQLite (`sqlite3`) |
| Progress and checkpoints | JSON, local files |

The code uses Python's standard library. Tkinter must be available in your Python installation to run the desktop clients. Python **3.8+** is recommended (the scripts use assignment expressions).

## Repository structure

```text
Advanced-File-Sharing/
├── README.md
├── .gitignore
├── server.py            # Multithreaded TCP server
├── client.py            # Command-line client and resumable download prototype
├── client_gui.py        # Tkinter file-sharing client
├── admin.py             # Tkinter local login / admin demo
├── setupdb.py           # Creates a local demo SQLite database
├── uploaded/            # Server-side files (created during usage)
│   └── VersionHistory/ # Previous versions of uploaded files
├── downloaded/          # Downloaded files
└── logs/                # Runtime logs and checkpoints
```

## Run locally

**Prerequisites:** Python 3.8+ and Tkinter for GUI mode. No `pip install` step is required for the core scripts.

Run commands **from the project root**, since file storage and log paths are relative to the current working directory.

### 1. Start the TCP server

```bash
python server.py
```

By default, it listens at `localhost:5002` and creates the `uploaded/` and `logs/` directories if needed.

### 2. Choose a client

**Command-line:** in a second terminal,

```bash
python client.py
```

The client accepts commands such as:

```text
LIST
UPLOAD example.txt
DOWNLOAD example.txt
EXIT
```

Use a local filename or path for `UPLOAD` (create a small `example.txt` first). Downloads are written to `downloaded/`.

**Graphical interface:** alternatively, run:

```bash
python client_gui.py
```

Click **Connect to server**, then use the buttons to view, upload, or download files.

### 3. Optional local login/admin demonstration

```bash
python setupdb.py
python admin.py
```

`setupdb.py` creates sample accounts for local demonstration purposes; consult that file for the demo credentials. `admin.py` illustrates role-dependent **interface controls** and includes an optional Windows Notepad log viewer.

## How file transfers work

1. A client connects to the TCP server.
2. It sends a text command such as `LIST`, `UPLOAD <filename> <size>`, or `DOWNLOAD <filename>`.
3. For transfers, the sender streams the file as binary data.
4. The receiver computes a SHA-256 digest, which is compared with the other side's digest.
5. Transfer activity can be displayed through a client interface and recorded in logs.

The command-line downloader additionally saves local checkpoints and can request that a file continue from a recorded byte offset; the behavior remains experimental.

## Scope and limitations

This is a **local learning prototype**, not a secure production file-sharing platform. The TCP server does not enforce authentication or admin permissions on file commands; the SQLite login applies to a local desktop interface only. Connections are not encrypted, and filename handling, transfer framing, file-version recovery, concurrent file modifications, and interrupted transfers have not been fully hardened or validated. Run it only with **non-sensitive test files** in a trusted local environment.

The purpose is to demonstrate computer networking fundamentals, client–server architecture, threading, file I/O, integrity checking, and desktop application development.


