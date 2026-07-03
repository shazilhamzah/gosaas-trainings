# Git: Getting Started

## What is Git?
* **Git** is a distributed version control system (VCS).
* It is used to track the history of changes in files and collaborate with others.

## Centralized vs. Distributed VCS
* **Centralized VCS**: All history is stored on a single central server. If the server goes down, collaboration stops, and history can be lost if there's no backup.
* **Distributed VCS**: Every developer has a full copy of the repository, including its complete history, locally. Git is a distributed VCS.

## Git Configuration Levels
You can configure Git settings at three different levels:
1. **System**: Applied to all users on the machine.
2. **Global**: Applied to all repositories of the current user.
3. **Local**: Applied only to the current repository.

## Line Endings (CRLF / LF) Configuration
Different operating systems handle line endings differently:
* **Windows**: Uses Carriage Return Line Feed (`CRLF`). Configure Git to convert LF to CRLF upon checkout and vice versa on commit:
  ```bash
  git config --global core.autocrlf true
  ```
* **macOS / Linux**: Uses Line Feed (`LF`). Configure Git to convert CRLF to LF on commit (but keep LF in the working directory):
  ```bash
  git config --global core.autocrlf input
  ```

## Getting Help
* For a brief help menu in the terminal/command prompt:
  ```bash
  git config -h
  ```
* For a detailed help page opened in your web browser:
  ```bash
  git config --global --help
  # or generally for any command:
  git <command> --help
  ```

---

## Summary
This note covers:
* The core concept of Git as a distributed version control system.
* The differences between centralized and distributed VCS.
* The three levels of Git configuration: System, Global, and Local.
* How to configure line endings (`core.autocrlf`) for Windows (`true`) and macOS/Linux (`input`).
* Commands to access Git help in the command line or web browser.