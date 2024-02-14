# `Unknown - Packet: 0x0068`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0068` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to update entity information.

## Packet Layout

This packet is a duplicate of `0x0067`.

You can find that packets information here: [**0x067**](/world/server/0x0067/README.md)

_**Note:** While the packet layout and usage is the same for these two packets, the server will only use the specific packet id for a given type in most cases. `0x068` is almost always used with the local player pet entity updates._
