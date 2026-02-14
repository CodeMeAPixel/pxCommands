# Command Packs

A command pack is a Lua file that registers one or more commands. Each pack is loaded automatically from the `commands/` directory.

## Basic Structure

```lua
CommandPack("PackName", "AuthorName", {
    -- Command definitions here
})
```

## Command Definition

Each command is a table with the following fields:

### Required

| Field | Type | Description |
|-------|------|-------------|
| `command` | string | Command name (used as `/name`) |
| `format` | string | Message format when command executes |

### Optional

| Field | Type | Description |
|-------|------|-------------|
| `help` | string | Help text shown in chat suggestions |
| `args` | table | Argument names shown in suggestions |
| `author` | string | Command author |
| `title` | string | Message title (defaults to command name) |
| `color` | table | RGB color `{255, 0, 0}` |
| `admin` | boolean | Restrict to admins (default: false) |
| `noperm` | string | Deny message when permission blocked |
| `prereq` | function | Custom validation function |
| `noargs` | boolean | Allow command with no arguments |
| `hidden` | boolean | Don't broadcast message to others |
| `range` | number | Broadcast range in meters; -1 for global, omit for no broadcast |
| `reply` | string | Static reply instead of executing callback |
| `cb` | function | Callback function when command executes |

## Format Placeholders

Use these in `format` and `title` strings:

| Placeholder | Value |
|-------------|-------|
| `#username#` | Player username |
| `#char#` | Character name (from framework) |
| `#name#` | Full name with character priority |
| `#id#` | Player server ID |
| `#message#` | All arguments joined |

## Callbacks and Hooks

### Command Callback

```lua
cb = function(source, message, command, args, raw)
    print(message)  -- Your formatted message
    -- source: Player ID
    -- args: Table of arguments
    -- raw: Raw input string (from RegisterCommand)
end
```

### Prerequisite Check

```lua
prereq = function(source, command, args, raw)
    return true  -- Allow command
    -- return false  -- Block command with noperm message
end
```

### Global Execution Hook

Set in `system/config.lua`:

```lua
Config.Callbacks.onCommandExecuted = function(source, message, command, args, raw)
    -- Fires after every command executes
end
```

## Examples

### Simple Reply Command

```lua
{
    command = "motd",
    help = "Show message of the day",
    reply = "Welcome to the server!",
    noargs = true
}
```

### Admin-only Command

```lua
{
    command = "kick",
    help = "Kick a player",
    format = "#username# was kicked",
    admin = true,
    noperm = "You lack permission",
    cb = function(source, message, command, args, raw)
        if args[1] then
            DropPlayer(tonumber(args[1]), "Kicked by " .. GetPlayerName(source))
        end
    end
}
```

### Proximity Chat

```lua
{
    command = "me",
    help = "Do a roleplay action",
    format = "#char# #message#",
    range = 30.0,  -- 30 meter range
    cb = function(source, message, command, args, raw)
        -- Message already formatted and broadcast
    end
}
```

### Custom Validation

```lua
{
    command = "whitelist",
    help = "Check whitelist status",
    prereq = function(source, command, args, raw)
        -- Only allow if player has specific identifier
        local identifier = GetPlayerIdentifiers(source)[1]
        return identifier and identifier:find("steam:") ~= nil
    end,
    noperm = "Only Steam players allowed",
    format = "#username# checked whitelist status"
}
```

### Character-based Message

```lua
{
    command = "say",
    help = "Say something",
    format = "#name#: #message#",
    range = 50.0
}
```

## Command Conflicts

Command names must be unique across all packs. If two packs define the same command, the first loaded takes priority and the second is ignored.

## Debug

Check server logs for:
- `Registering command /name from pack PackName...`: this log means commands are successfully loading.
- `Failed to register command /name from pack PackName...`: this is an error and means your command is possibly malformed
