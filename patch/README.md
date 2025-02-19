<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Patch Server

The `patch` server is responsible for allowing the client to validate its current version(s) with the remote services and handle any kind of updates needed for the clients local files. This includes handling updates for the `PlayOnline` client as well as any of its supported titles that make use of the in-house versioning system. This is the first server that will be connected to, both for `PlayOnline` when logging in as well as when launching a supported title that uses this system. _(Such as `Final Fantasy XI`.)_

## Documentation Layout

The `patch` server documentation is separated into additional pages, with the individual packet information found within the [packets](/patch/packets/) folder. _(Since the `patch` server has so few packets, all packets are contained within the same folder regardless of direction.)_