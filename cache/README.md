<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Cache Server

The `cache` server, often referred to as the `search` server, is responsible for game traffic that may require a longer time to return a response. Unlike the `world` server which uses UDP for its traffic, the `cache` server transmits its traffic on a separate socket, using TCP.

The `cache` server is responsible for the following:

  - **Auction House**
    - Auction History
    - Auction Lists
  - **Search**
    - List: Linkshell Members
    - List: Party / Alliance Members
    - List: Search (`/sea`, `/sea all`, etc.)
    - Search Comments
