# MultiAddonManager

A MetaMod plugin that allows you to use multiple workshop addons at once and have clients download them.
Original plugin is here: https://github.com/Source2ZE/MultiAddonManager  Thanks to xen

## ConVars
- `mm_extra_addons <ids>` The workshop IDs of extra addons separated by commas, addons will be downloaded (if not present) and mounted (e.g. "3090239773,3070231528").
  Once downloads are done, the map is automatically reloaded so content can be precached.
- `mm_client_extra_addons <ids>` The workshop IDs of extra client-side only addons that will be loaded by all clients, separated by commas. These addons are not loaded or downloaded by the server.
  Changes will only apply to future clients.

- `mm_extra_addons_timeout <seconds> (default 10)` Deprecated; retained for config compatibility. A reconnect while an addon is pending is treated as successful; `mm_optional_client_addons_timeout` controls when an addon is omitted.
- `mm_addon_connection_timeout <seconds> (default 30)` // How long until clients are timed out while downloading the first required addon (usually the current map), 0 disables
- `mm_optional_client_addons <0/1> (default 1)` Lets a client continue without an extra addon managed by MultiAddonManager if it remains pending past `mm_optional_client_addons_timeout`. `mm_extra_addons` addons mounted by the plugin and client-only addons are optional; the current workshop/community map is never skipped. CS2/MetaMod does not report whether the player accepted or canceled an addon download. A reconnect while an addon is pending is therefore treated as a successful download; if it remains pending past the timeout, it is omitted on the next connection attempt so the player can join without it.
- `mm_optional_client_addons_timeout <seconds> (default 5)` How long to wait before treating a pending optional addon as canceled. This is a timeout-based approximation, not a client-side cancellation signal: an accepted download that takes longer than this may also be skipped. The addon is omitted when the client next enters the connection flow; the server cannot resume a client that remains on a local download screen or has canceled without reconnecting. Set to `0` to omit a pending optional addon on the next connection attempt. With `mm_cache_clients_with_addons 1`, omissions are retained for the configured cache duration; otherwise they are cleared after the client becomes active.
- `mm_print_searchpaths` Print all the search paths currently mounted by the server.
- `mm_addon_mount_download <0/1> (default 0)` If enabled, the plugin will initiate an addon download every time even if it's already installed, this will guarantee that updates are applied immediately.
- `mm_cache_clients_with_addons <0/1> (default 0)` If enabled, the plugin will remember downloaded addons during the client's connection flow and map changes. The cache is cleared when a client reconnects so the server sends the addon IDs needed for the client to mount them again.
- `mm_cache_clients_duration <0/seconds> (default 0)` How long to cache clients' downloaded addons list, pass 0 for forever.
- `mm_block_disconnect_messages <0/1> (default 0)` If enabled, the plugin will block *ALL* disconnect events with the "loop shutdown" reason. This will prevent disconnect chat messsages whenever someone reconnects because they're getting an addon.
- `mm_addon_debug <0/1> (default 0)` Whether to print some extra debug information (mainly when clients are joining)

## Commands
- `mm_download_addon <id>` Download an addon manually.

 Both of these commands require a map reload to apply changes.
- `mm_add_addon <id>` Add an addon to the list, but don't mount.
- `mm_remove_addon <id>` Remove an addon from the list, but don't unmount.

 These following commands will only affect future clients:
- `mm_add_client_addon <id>` Add a workshop ID to the global client-only addon list.
- `mm_remove_client_addon <id>` Remove a workshop ID from the global client-only addon list.

## Usage in other MetaMod plugins
- Include the [public header](https://github.com/Source2ZE/MultiAddonManager/blob/main/public/imultiaddonmanager.h).
- Query the interface in `AllPluginsLoaded` like this:
```cpp
IMultiAddonManager *pInterface = (IMultiAddonManager*)g_SMAPI->MetaFactory(MULTIADDONMANAGER_INTERFACE, nullptr, nullptr);
```

## Installation

- Install [Metamod](https://cs2.poggu.me/metamod/installation/)
- Download the [latest release package](https://github.com/Source2ZE/MultiAddonManager/releases/latest) for your OS
- Extract the package contents into `game/csgo` on your server
- Edit the config file at `game/csgo/cfg/multiaddonmanager/multiaddonmanager.cfg`
