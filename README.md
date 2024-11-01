# TecnicoFS - Simplified File System for Operating Systems Project

TecnicoFS is a simplified file system implemented in user mode, providing a POSIX-inspired API that allows client processes to maintain private instances of a file system. This project includes two exercises, each with specific functionalities and requirements.

## Project Structure and Functionality

### Exercise 1 - Basic File System Operations
The core functionalities of TecnicoFS in Exercise 1 include:
- **File Operations**: Functions to create, open, read, write, and close files, with each file represented by a unique inode structure.
- **Multiblock Files**: Extending support for files that span multiple blocks.
- **Concurrent Client Support**: Making the file system thread-safe, allowing concurrent calls from a multitasking client using fine-grained locking mechanisms.
- **Blocking Destruction**: Implementing `tfs_destroy_after_all_closed` to wait until all files are closed before destroying the file system instance.

### Exercise 2 - Client-Server Architecture
In Exercise 2, TecnicoFS is transformed into a standalone server process, supporting multiple clients:
- **Client-Server Communication**: Using named pipes, the server receives and processes file operations from multiple client processes.
- **Session Management**: The server manages sessions with unique session IDs for up to `S` concurrent sessions.
- **Blocking Destruction Across Processes**: The `tfs_shutdown_after_all_closed` function blocks the server destruction until no files are open, ensuring an orderly shutdown.

## Build Instructions

To build the project, navigate to the project directory and use the following command:
```
make
```

- **make**: Compiles the source code and creates executable files for each module.
- **make clean**: Cleans up any compiled objects or executables.

## Running the Project

The project includes different ways to run the file system depending on the exercise.

### Running Exercise 1

To start a single client using TecnicoFS as a library:
```
./tecnicofs_client <operations_file>
```
- **operations_file**: Specifies the file containing the list of operations for the client to execute on TecnicoFS.

### Running Exercise 2

Exercise 2 implements a client-server architecture. Follow these steps:

1. **Start the TecnicoFS Server**:
   ```
   ./tecnicofs_server <server_pipe>
   ```
   - **server_pipe**: The name of the named pipe for client-server communication.

2. **Connect a Client to the Server**:
   Each client process should be connected using:
   ```
   ./tecnicofs_client <client_pipe> <server_pipe>
   ```
   - **client_pipe**: The named pipe for receiving responses from the server.
   - **server_pipe**: The named pipe for sending requests to the server.

3. **Shutdown Server After All Files Are Closed**:
   To shut down the server in an orderly manner, use:
   ```
   ./tecnicofs_client_shutdown <server_pipe>
   ```

This command signals the server to perform `tfs_shutdown_after_all_closed`, which waits until all open files are closed before safely shutting down the server.

## API Overview

The simplified TecnicoFS API includes the following functions:

- **tfs_open**: Opens a file with specified flags.
- **tfs_close**: Closes an open file.
- **tfs_write**: Writes data to an open file.
- **tfs_read**: Reads data from an open file.
- **tfs_destroy_after_all_closed**: Destroys the file system only after all files are closed.

---

This project was designed for educational purposes as part of an Operating Systems course and demonstrates file system fundamentals, client-server architecture, and concurrency management.
