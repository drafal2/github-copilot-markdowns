---
name: logging
description: Structured logging with proper levels. Use for progress logging while code editing and creating from scratch.
---

# Logging

## Overview

Implement secure, structured logging with proper levels and context. When working on existing code with logging implemented, analyse if logging is correct and adjust in line with following instructions.

## Principles

### Loggings do's

- User should be able to find the source code for any given log entry
- Show class name that calls the logger
- The repo is maintained for analytical works, not big app. Logs must be easily readable for human

### Loggings do not's

- **DO NOT** log inside tight loops
- **DO NOT** log senstitive or confidential information, e.g. passwords, tokens, API keys, client names

### Logging levels

| Level | Use For |
|-------|---------|
| DEBUG | **DO NOT USE** |
| INFO | Normal operations, info about execution progress |
| WARN | Potential issues |
| ERROR | Errors with recovery, requries human attention |