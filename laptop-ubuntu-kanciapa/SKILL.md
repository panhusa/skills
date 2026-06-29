---
name: laptop-ubuntu-kanciapa
description: Use when managing HP EliteBook 840 G5 (Ubuntu, 192.168.1.70) — SSH access, launching GUI apps remotely, REAPER DAW, MCP reaper-mcp-server, audio stack (JACK/FFADO/PipeWire), kanciapa project setup
---

# Laptop Ubuntu — kanciapa

## Dostęp

```bash
ssh hushus@192.168.1.70        # LAN
ssh hushus@10.0.0.3            # WireGuard
```

Zawsze używaj `bash -l -c "..."` dla pełnego PATH (pipx, ~/.local/bin).

## Uruchamianie GUI via SSH

**Jedyna działająca metoda (Wayland, GNOME, brak .Xauthority):**

```bash
XDG_RUNTIME_DIR=/run/user/1000 \
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus \
WAYLAND_DISPLAY=wayland-0 \
nohup /ścieżka/do/app &>/tmp/app.log &
```

- `DISPLAY=:0` NIE działa (brak .Xauthority)
- `systemd-run --user` nie potrzebne, powyższe wystarczy
- `nohup` konieczne — bez niego proces ginie przy rozłączeniu SSH

## REAPER

```bash
# Uruchomienie
XDG_RUNTIME_DIR=/run/user/1000 DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus \
WAYLAND_DISPLAY=wayland-0 nohup /opt/REAPER/reaper &>/tmp/reaper.log &

# Weryfikacja
ss -tlnp | grep 2307        # port reapy musi nasłuchiwać
ps aux | grep reaper | grep -v grep
```

## MCP do REAPER (reaper-mcp-server)

| Element | Stan |
|---------|------|
| Binary | `/home/hushus/.local/bin/reaper-mcp-server` (pipx) |
| Claude settings | `~/.claude/settings.json` → mcpServers.reaper |
| Port web interface | 2307 (REAPER HTTP server) |
| Port reapy RPC | 2306 (ReaScript server wewnątrz REAPER) |

### Pierwsze uruchomienie (jednorazowe)

1. W REAPER: `Options → Preferences → Control/OSC/web` → Add → Web browser interface → port 2307 → OK
2. Z terminala (aktywacja reapy ReaScript):
```bash
curl -s "http://localhost:2307/_/RSZeirH7F0qoXCN8isCrHL8L9t6RBhOR2X2ATSP7D1"
```

Po tym: port 2306 otwarty = reapy działa. Ustawienie web interface (krok 1) jest zapamiętywane przez REAPER — nie trzeba powtarzać po restarcie.

### Przy każdym starcie REAPER

```bash
# Aktywuj reapy server (port 2306)
curl -s "http://localhost:2307/_/RSZeirH7F0qoXCN8isCrHL8L9t6RBhOR2X2ATSP7D1"
```

### Test połączenia
```bash
ssh hushus@192.168.1.70 'bash -l -c "python3 /dev/stdin" <<'"'"'EOF'"'"'
import sys; sys.path.insert(0, "/home/hushus/.local/share/pipx/venvs/reaper-mcp-server/lib/python3.12/site-packages")
import reapy; reapy.connect(); p = reapy.Project(); print(f"BPM={p.bpm}, tracks={len(p.tracks)}")
EOF'
```

MCP działa tylko gdy REAPER uruchomiony i oba porty (2306, 2307) otwarte.

## Audio stack

| Warstwa | Stan |
|---------|------|
| PipeWire | ✅ systemd user, autostart |
| JACK2 (jackd2) | ✅ zainstalowany |
| FFADO | ✅ ffado-dbus-server + mixer + tools |
| Grupa `audio` | ✅ hushus (rtprio=95, memlock=unlimited) |
| MX5 USB | podłącz fizycznie → pojawi się jako karta audio |
| Saffire PRO 40 | wymaga adaptera TB3→TB2→FW800 (jeszcze nie ma) |

## Kluczowe ścieżki

| Co | Gdzie |
|----|-------|
| REAPER binary | `/opt/REAPER/reaper` |
| REAPER config | `~/.config/REAPER/reaper.ini` |
| reaper-mcp-server | `~/.local/bin/reaper-mcp-server` |
| Claude settings | `~/.claude/settings.json` |
| REAPER log (SSH) | `/tmp/reaper.log` |
| Ollama (CPU) | `http://localhost:11434` |

## Szybkie diagnostyki

```bash
# REAPER żyje?
ss -tlnp | grep 2307

# Audio group aktywna?
id hushus | grep audio

# PipeWire działa?
systemctl --user status pipewire

# Co jest podłączone audio?
aplay -l
```
