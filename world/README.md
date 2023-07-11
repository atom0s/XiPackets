<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# World Server

The `world` server, often referred to as the `game` server, is the server responsible for handling the main gameplay traffic for a given zone, or set of zones. On retail servers, the `world` server instances are clustered for an entire server (ie. `Asura`) where each zone is ran on its own instance. This allows retail to take individual zones offline for emergency maintenance. 

Through IP and port monitoring, it has been observed that retail generally clusters zones together based on the type of zone, such as cities will run on individual `world` server instances within a single server hosted on the same port.

The `world` server is responsible for all other traffic not already handled by the `cache` and `lobby` servers.

## Documentation Layout

The `world` server documentation is broken into two separate main categories:

  - `client` - Contains packet information about packets sent by the client to the server.
  - `server` - Contains packet information about packets sent by the server to the client.

Within these folders, each packet will be separated into its own folder and have its own `README.md` file. This allows each packet to contain additional resources and information all contained within its own folder, making it easer to find, read and reference.
