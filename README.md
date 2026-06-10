# RateLimit

A small Luau rate-limiting utility for Roblox that helps protect remote event handlers and other per-source workflows from spam.

## Overview

`RateLimit` provides a lightweight per-source limiter with optional strict wait behavior. It is designed to work with any source value, including `Player` instances, and automatically cleans up player references when players leave the game.

### Key features

- Rate limits by source value
- Configurable events-per-second limit
- Optional strict wait mode for evenly spaced event processing
- Automatic cleanup for leaving `Player` instances
- Simple API with `New`, `CheckRate`, `CleanSource`, `Cleanup`, and `Destroy`

## Installation

Install the package with Wally:

```bash
wally add ptekspy/RateLimit
```

## Usage

Require the module from your package path. Adjust the path to your package structure.

```lua
local RateLimit = require(path.to.RateLimit.init)

local limiter = RateLimit.New(5)

if limiter:CheckRate(player) then
	-- process the action
else
	-- drop or ignore the action
end
```

## API

### `RateLimit.New(rate, is_full_wait?)`

Creates a new rate limiter.

- `rate` (`number`): allowed events per second.
- `is_full_wait` (`boolean?`): if `true`, the limiter enforces a full wait period between allowed events; if `false`, it allows bursty behavior up to the configured rate.

Returns a `RateLimit` instance.

### `RateLimit:CheckRate(source)`

Checks whether the provided source may proceed.

- `source` (`any`): any value to rate limit by, such as a `Player` instance, user ID, or string key.

Returns `true` when the event should be processed, otherwise `false`.

### `RateLimit:CleanSource(source)`

Forgets a single source. Use this when a non-player source will no longer be reused.

### `RateLimit:Cleanup()`

Forgets all tracked sources for this rate limiter.

### `RateLimit:Destroy()`

Stops tracking the limiter internally and clears its state.

## Notes

- Player objects are automatically cleaned when they leave the game.
- A `nil` source is normalized internally to a string key so it can still be rate limited.

## Development

To build the package, use:

```bash
argon build
```

## License

MIT License. See `LICENSE.md` for details.
