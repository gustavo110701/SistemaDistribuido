# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a distributed systems project (semester 9) containing a peer-to-peer messaging system implementation. The codebase consists of a single Python script that implements a multi-threaded TCP socket-based messaging application.

## Architecture

**Core Components:**

- **Server Thread**: Listens on port 5555 for incoming connections, spawns a handler thread for each client connection
- **Client Handler Threads**: Process incoming messages, timestamp them, store to file, and send acknowledgments
- **Client Sender**: Prompts user for messages and target node IPs, establishes connections and sends timestamped messages
- **Message Persistence**: All received messages are appended to `messages.txt` with timestamps and sender information

**Threading Model:**
- Main thread runs server in background via `threading.Thread`
- Each incoming client connection spawns a dedicated handler thread (`handle_client`)
- Main thread continues to handle user input for sending messages

**Network Protocol:**
- TCP sockets on port 5555
- Message format: `YYYY-MM-DD HH:MM:SS: <message content>`
- Response format: `Mensaje recibido a las YYYY-MM-DD HH:MM:SS`

## Running the Application

```bash
python "Primer entregable.py"
```

The application will:
1. Start a server listening on port 5555
2. Prompt for messages to send to other nodes
3. Request target IP addresses for each message

**Testing with Multiple Nodes:**
```bash
# Terminal 1 (Node 1)
python "Primer entregable.py"

# Terminal 2 (Node 2)
python "Primer entregable.py"

# In Terminal 1, when prompted:
# IP: localhost (or 127.0.0.1)
```

## Key Implementation Details

- **File I/O**: Messages are persisted using append mode to prevent data loss between sessions
- **Error Handling**: Basic exception handling in `handle_client` ensures graceful connection closure
- **Encoding**: UTF-8 encoding/decoding for all socket communications
- **Buffer Size**: 1024 bytes for message reception
