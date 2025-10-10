# `GP_SERV_COMMAND_WPOS2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_WPOS2` |
| **Client Handler**        | `RecvWpos2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0065` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to update an entities position information. (2)

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_WPOS2
struct GP_SERV_WPOS2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    float       x;          // PS2: x
    float       y;          // PS2: y
    float       z;          // PS2: z
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Mode;       // PS2: Mode
    char        dir;        // PS2: dir
    uint32_t    padding18;  // PS2: (New; did not exist.)
};
```

## Packet Fields

This packet is a duplicate of `GP_SERV_WPOS`.

You can find that packets information here: [**0x05B - GP_SERV_WPOS**](/world/server/0x005B/README.md)
