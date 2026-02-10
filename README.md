# pxCommands

A modular chat and command system for FiveM supporting ESX, QBCore, QBox, and standalone servers.

> [!CAUTION]
> This resource is not yet ready for production use and is only public at this stage for transparency. Please check back in a few days for an official release :D 

## Features

- Framework agnostic with explicit configuration
- Modular architecture (commands, modules, system)
- Event namespace (`pxc:*`)
- Server-side security enforcement
- Automatic command help and suggestions
- Range-based proximity messaging

## Quick Start

1. Place `pxCommands` in your resources folder
2. Add `ensure pxCommands` to `server.cfg`
3. Edit `settings.lua` with your framework:
   ```lua
   Config.Framework = 'esx'  -- or 'qbcore', 'qbox', 'standalone'
   ```
4. Create command packs in `commands/` folder

Full setup guide: [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md)

## Command Packs

Create `.lua` files in `commands/`:

```lua
CommandPack("MyPack", "AuthorName", {
    {
        command = "hello",
        format = "#name# says hello!",
        help = "Say hello",
    },
})
```

Full reference: [docs/COMMAND_PACKS.md](docs/COMMAND_PACKS.md)

## Documentation

| Doc | Purpose |
|-----|---------|
| [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md) | Installation and first steps |
| [docs/COMMAND_PACKS.md](docs/COMMAND_PACKS.md) | How to create commands |
| [docs/CONFIG_REFERENCE.md](docs/CONFIG_REFERENCE.md) | All configuration options |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Technical structure |
| [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) | Common issues |
| [.github/SECURITY.md](.github/SECURITY.md) | Security policy |

