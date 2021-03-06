# launchd

A collection of macOS `.plist` files to automate background tasks 

These files may be placed into one of three locations, with the following results:

Location                 | When Loaded | For Whom      | Ownership    | Permissions | Controlled by
-------------------------|-------------|---------------|--------------|-------------|------------------
`/Library/LaunchDaemons` | at startup  | everyone      | `root:wheel` | `644`       | `sudo launchctl`
`/Library/LaunchAgents`  | at login    | everyone      | `root:wheel` | `644`       | `launchctl`
`~/Library/LaunchAgents` | at login    | specific user | `user:staff` | `644`       | `launchctl`

See [launchd.info](https://www.launchd.info) for general syntax, but also refer to `man launchd.plist` for the macOS-specific implementation, which is a subset in many respects.

---

### [`org.tuxfamily.miniupnp.upnpc.plist`](https://github.com/ChrisBaker97/launchd/blob/main/org.tuxfamily.miniupnp.upnpc.plist)

Opens a UPnP port forward from public port 50122 to local port 22 for SSH. Once loaded, it will attempt to run every 5 minutes until successful, and then once every 6 hours thereafter. (If loaded at boot time, the first attempt will likely occur before the network is initialized, causing it to fail.) Output is sent to `/tmp/org.tuxfamily.miniupnp.upnpc.stdout` and `/tmp/org.tuxfamily.miniupnp.upnpc.stderr`.

Requires the [`miniupnpc`](https://miniupnp.tuxfamily.org) UPnP client, available via [Homebrew](https://brew.sh):
```sh
$ brew install miniupnpc
```

To bring down the UPnP port forwarding, first unload the daemon and then remove the port forward:
```sh
$ <sudo> launchctl unload <path/>org.tuxfamily.miniupnp.upnpc.plist && upnpc -d 50122 tcp
```
