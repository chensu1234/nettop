# Changelog

All notable changes to this project will be documented in this file.

## [1.0.0] - 2026-04-14

### Added
- Real-time TUI monitoring mode (`watch`) with bandwidth summary and interface table
- Top-N interface ranking by RX/TX bandwidth (`top`)
- Network interface listing with status, MTU, and MAC address (`interfaces`)
- Active connection display with IPv4/IPv6 support (`connections`)
- macOS support (netstat-based)
- Linux support (procfs-based)
- Color-coded rate display (green → yellow → red by speed)
- Keyboard shortcuts in watch mode (q=quit, r=toggle sort)
- Zero external dependencies — pure Bash with awk, bc, sort
