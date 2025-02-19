# Reversing Information - Packet: `GP_SERV_COMMAND_BATTLE2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BATTLE2` |
| **Client Handler**        | `RecvBattleCalc2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0028` |
| **Size**                  | `(varies)` |

## Preface

In this document, you will find more detailed information on how the client parses and unpacks the incoming action packets.

This packet makes heavy use of unaligned bit packing. Due to this, it is not possible to just directly map the raw packet data to a C/C++ structure. Instead, the data must be manually unpacked back into a structure. _The structures used by the client when unpacking the data have been outlined in the **[README.md](README.md)** document for this packet. Please refer to that information for more details on each field being parsed/unpacked._

## Understanding Bit Packing

Before diving into how the client parses this packet, it is important to understand what bit packing is and how it is used within the Final Fantasy XI client.

### What is bit packing?

Bit packing is a method of storing data/values into the least amount of bits possible. You can think of it kind of like data compression, but instead of taking one set of data and turning it into a smaller one through some compression algorithm, bit packing instead compresses things down by packing multiple pieces of data into the same variable space.

To help illustrate this concept, let's take the following structure as an example:

```cpp
struct uncompressed_t
{
    uint32_t    val0;
    uint16_t    val1;
    uint8_t     val2;
    uint8_t     val3;
};
```

Using this structure as it is shown above, we can obtain its size via `sizeof(uncompressed_t)` which will return `8`. This means that this structure currently requires 8 bytes of memory to represent its data, or in bits, `64` bits. _(There are 8 bits in a byte, thus 8bits * 8bytes is 64 bits.)_

Next, let's assume that this structure is also governed by a strict set of rules that heavily limit the maximum value that each field can hold such as:

  - `val0` is an id of sorts. It can have a max value of `65535`.
  - `val1` is a simple enumeration that can only ever be one of the following values: `0`, `1`, `2`, or `3`
  - `val2` is a simple enumeration that can only ever be one of the following values: `0`, `1`, `2`, or `3`
  - `val3` is a boolean, it can only ever be `0` or `1`

With this set of rules, we can determine the total number of bits each field would actually require in order to hold its maximum value:

  - `val0` would require a total of `16` bits to store its maximum value.
  - `val1` would require a total of `2` bits to store its maximum value.
  - `val2` would require a total of `2` bits to store its maximum value.
  - `val3` would require a total of `1` bit to store its maximum value.

Knowing this, it means we could technically reduce our structures need of `64` bits down to just `21` instead. This is where the power of another mechanism in C/C++ comes into play, called `bit-fields`.

Bit-fields are a method of declaring class/structure data members with an explicit size in bits. When bit-fields are used, the compiler will make an attempt at packing adjacent bit-field members into the same byte space. This means that one or more members of a bit-field can be stored in the same individual byte(s) of data.

By using bit-fields, we can redesign our above example structure in a couple of different ways to greatly improve the memory requirements. From our ruleset above, we know that our example structure would need a total of `21` bits to hold its information. With that, we know we can fit all four fields of information into a single `uint32_t` data type, as it can hold a maximum of `32` bits.

This means we can instead rewrite our structure using bit-fields like this:

```cpp
struct compressed1_t
{
    uint32_t    val0 : 16;
    uint32_t    val1 : 2;
    uint32_t    val2 : 2;
    uint32_t    val3 : 1;
};

// As an alternative, we can also write the structure in other manners.
// Knowing that 'val0' is 16 bits, we can simply define that as its own uint16_t instead such as:
struct compressed2_t
{
    uint16_t    val0 : 16;
    uint8_t     val1 : 2;
    uint8_t     val2 : 2;
    uint8_t     val3 : 1;
};
```

Using bit-fields with these new structures, we can obtain the size again of either of these via `sizeof(compressed1_t)` or `sizeof(compressed2_t)` which will return `4`. This means that by using bit-fields we can save `4` bytes from the original `uncompressed_t` approach while still expressing the same exact data.

While these savings may seem small and uninteresting, on much larger blocks of data, this kind of space saving can rapidly grow upwards giving a much more impactful amount of savings on memory usage. This kind of approach to saving space is extremely useful in cases where time savings are a must, such as network transfers. Because Final Fantasy XI was designed to run on dial-up connections from the early years of the internet, it is important to try and condense large packets of information down to a much smaller size.

The internal client parser function for action packets have some theoretical limits coded in that can give us an idea at the 'maximum' size of an action packet that could have been expected. The client hard-caps the total number of 'target' entries in the packet to `64` and the total number of 'result' entries for each target to `8`.

Using generalized estimates on a 'maxed out' kind of packet, not using bit packing, this packet could take upwards of `22,528` bytes of data to represent all of the information that could potentially be present. However, through the use of bit packing, the number of required drops down to a mere `7,680` bytes instead. This is a massive savings of `14,848` bytes. As you can see, these savings can really grow rapidly when multiple blocks of similar data are packed together.

For more information in regards to bit packing and bit-fields, please check out the following resources:

  - https://codeplea.com/optimal-bit-packing
  - https://en.cppreference.com/w/c/language/bit_field
  - https://en.cppreference.com/w/cpp/language/bit_field
  - https://learn.microsoft.com/en-us/cpp/cpp/cpp-bit-fields?view=msvc-170

### Alignment Notes

While making use of bit packing can be a great way to save space, making use of it through the means of structures with bit-fields does come with a caveat in the form of alignment restrictions.

Defining a structure with a set of bit-field members does not guarantee the compiler will always interpret data directly as desired. This means that in some cases, you may land up with the compiler adding padding between members of the structure where you would otherwise not want it. It can also mean that structures which hold members that make use of another structure as its data type which also use bit-fields can also become realigned by the compiler in an unwanted way.

Take the following structure for example:

```cpp
struct unaligned_t
{
    uint32_t    val0;
    uint8_t     val1;
    uint32_t    val2;
};
```

On most modern compilers, this structure will be realigned by adding junk/padding within the structure that we did not define or want. Looking at this example, you may assume that doing `sizeof(unaligned_t)` would return `9`. Instead, this structure would return a size of `12` because of the alignment being done by the compiler. _(Please note; structure alignment is a feature that can be configured and manually altered on most modern compilers, even at a per-structure level. This example is simply to demonstrate what automatic alignment looks like and means.)_

We can then see where this magical padding is being added by checking the offset to the members in the structure:

  - `offsetof(unaligned_t, val0)` - 0
  - `offsetof(unaligned_t, val1)` - 4
  - `offsetof(unaligned_t, val2)` - 8

Here we can see that the compiler injected 3 bytes of padding between `val1` and `val2`.

Now let's take a look at how this can affect bit-fields and bit packing as well. Take the following structures as an example:

```cpp
struct block1_t
{
    uint8_t val0 : 6;
};

struct block2_t
{
    uint8_t val0 : 1;
    uint8_t val1 : 1;
};

struct block_t
{
    block1_t b1;
    block1_t b2;
};
```

Using bit packing, we know that the data within `block1_t::val0`, `block2_t::val0` and `block2_t::val1` could technically fit within the same single byte of data because a `uint8_t` has a total of 8 bits. Between `block1_t` and `block2_t` we have defined a total of 8 bits used. However, compilers do not see these two structures as wanting to make use of the same bit space. Because of this, when we define a `block_t` instance, it will cause padding to be added between the `b1` and `b2` members, making this structure take up two bytes of data instead of one.

Checking the offset where each member starts we can see:

  - `offsetof(block_t, b0)` - 0
  - `offsetof(block_t, b1)` - 1

If no padding was happening, than `b1` should also be 0 here sharing the bits with `b0`.

_Because FFXI makes use of unaligned bit packing, we cannot just directly map structures to the action packet data. The next section will cover the functions used inside of the FFXI client to read the unaligned bit data._

## How Final Fantasy XI Reads Bits

With the above information about bits covered, we can now dive into how the client actually reads the bits used inside of this packet.

Internally, the client has a set of global variables and helper functions it makes use of to read bit information:

  - `uint8_t* PTR_pcPackBuffPtr;` - Holds the current pointer to the packet buffer being read from.
  - `uint32_t PTR_iPackBuffOfs;` - Holds the current bit offset being read.

As the client reads bits, it will increment `PTR_iPackBuffOfs` until it reaches `8`, which marks the end of a byte. When this happens, it resets the `PTR_iPackBuffOfs` bit index to `0` and then increments the `PTR_pcPackBuffPtr` stepping to the next byte in the buffer.

The following are the various helper functions that the client makes use of when handling bits for this packet.

### FUNC_Bits_InitBitCountAndPtr

```cpp
void FUNC_Bits_InitBitCountAndPtr(const uint8_t *ptr)
{
    PTR_iPackBuffOfs = 0;
    PTR_pcPackBuffPtr = ptr;
}
```

This function is used as the main initializer for the bit related functions. It resets the bit position value to 0 and then sets the `PTR_pcPackBuffPtr` to the start of the buffer where bits will be read from.

### FUNC_Bits_ResetBitCount_StepPtr

```cpp
void FUNC_Bits_ResetBitCount_StepPtr(void)
{
    PTR_iPackBuffOfs = 0;
    ++PTR_pcPackBuffPtr;
}
```

This function is used as stepper helper. Each time the `FUNC_Bits_Unpack` reaches the end of a set of bits inside of a byte, this function is called, resetting the current bit offset to 0 and walking the data pointer forward one byte.

### FUNC_Bits_Unpack

```cpp
int32_t FUNC_Bits_Unpack(const int32_t bits)
{
    auto ret = 0;
    for (auto x = 0u; x < bits; ++x)
    {
        auto val = *(uint8_t*)PTR_pcPackBuffPtr & 1) << x;
        *(int8_t*)PTR_pcPackBuffPtr >>= 1;
        ret |= val;

        if (++PTR_iPackBuffOfs == 8)
            FUNC_Bits_ResetBitCount_StepPtr();
    }

    return ret;
}
```

This function is used as the main bit unpacker. It will loop the number of bits given as its argument, reading the data of the current bit and combining it into the total result that will be returned when finished. Once the current bit is read, the `PTR_iPackBuffOfs` bit offset global variable is incremented until it reaches the end of the current byte being read from. When it reaches the end of the current byte, then `FUNC_Bits_ResetBitCount_StepPtr` is called to reset the bit counter and step to the next byte.

## Action Packet Handling

Before jumping into the various functions that are used with unpacking and processing action packets, we'll cover the actual structures that are used internally with the client in these functions. It's important to understand what is being made/used to have a better understanding of what the functions are doing.

The following structures are made use of when parsing/unpacking the action packet and preparing it for scheduling:

  - `CXiSchStatus` - Used by the main packet handler function `RecvBattleCalc2`.
  - `CXiMainToCalc` - Used by the action packet unpacker function `CXiSchStatus::Unpack`.
  - `XiBattleResult` - Used by the action packet unpacker function `CXiSchStatus::Unpack`.
  - `BattleResult` - Used by the action packet unpacker function `CXiSchStatus::Unpack`.

## Structure: CXiSchStatus

```cpp
struct CXiSchStatus
{
    uint16_t        m_bRefCounter;
    uint8_t         m_bHeader;
    uint8_t         m_bPutDataCounter;
    uint32_t        m_uFourCC;
    uint16_t        m_wMesIndex;
    uint16_t        m_wMesSPIndex;
    CXiMainToCalc*  m_pclsData;
};
```

### m_bRefCounter

_The reference counter of this object to determine when it is no longer in use._

### m_bHeader

_Set of byte flags used to mark different statuses on this object._

  - `0` - _None._
  - `1` - _Set when the object has been parsed or cloned._
  - `2` - _Set when the caster and/or target have been locked via `XiActor::SetLockStatus`._

### m_bPutDataCounter

_Index used when referencing different parts of data stored inside of the `m_pclsData` data._

This value is generally used when looking up a target or other result information. Whenever `CXiSchStatus::Next` is called, then this value is incremented to step to the next entry within this action.

### m_uFourCC

_The 'fourcc' tag used for this object._

This value is set to `main` by default.

### m_wMesIndex

_Index used when determining which result index is next to be handled._

When the client is handling an action, calls made to `CXiSchStatus::PutMessage` will increment this value.

### m_wMesSPIndex

_Index used when determining which result index was last handled._

When the client is handling an action, calls made to `CXiSchStatus::PutMessage` will set this value. Calls made to `PutBattleMessage`, which is used to output a current results information to the chatlog, make use of this value to determine the proper target and result information to reference when building the action / combat messages.

### m_pclsData

_Pointer to an CXiMainToCalc instance holding the unpacked action information._

## Structure: CXiMainToCalc, XiBattleResult, BattleResult

```cpp
struct BattleResult
{
    uint32_t        miss;
    uint16_t        kind;
    uint16_t        sub_kind;
    uint16_t        info;
    uint16_t        scale;
    uint32_t        value;
    uint32_t        message;
    uint32_t        bit;
    uint16_t        proc_kind;
    uint16_t        proc_info;
    uint32_t        proc_value;
    uint16_t        proc_message;
    uint16_t        react_kind;
    uint16_t        react_info;
    uint16_t        react_value;
    uint16_t        react_message;
};

struct XiBattleResult
{
    XiAtelBuff**    m_ppTargetAtel;
    uint32_t        m_uID;
    BattleResult    m_stResult;
};

struct CXiMainToCalc
{
    uint32_t        VTable;
    XiAtelBuff**    m_ppCasterAtel;
    uint32_t        m_uID;
    uint32_t        m_uCmdNo;
    uint32_t        m_uCmdArg[1];
    uint32_t        m_uInfo;
    uint32_t        m_unk00;
    uint16_t        m_sResultSum;
    uint16_t        m_sTargetSum;
    XiBattleResult* m_pResult;
    char            m_bMessagePutFlag[64];
    uint16_t        m_unk01;
    uint16_t        m_unk02;
};
```

_**Please Note:** To save some space in this document, full details of some of these fields will not be included as they are the same information already covered inside of the **[README.md](README.md)** document. Please refer to that information if you need more details about some of these fields._

### CXiMainToCalc::VTable

_Pointer to this objects call table._

### CXiMainToCalc::m_ppCasterAtel

_The actor pointer of the entity that caused the action. (The client refers to this as the 'caster'.)_

**Note:** This value is not part of the packet data. It is populated using the `m_uID` value after its read.

### CXiMainToCalc::m_uID

_The server id of the entity that caused the action._

### CXiMainToCalc::m_uCmdNo

_The command number of the action._

### CXiMainToCalc::m_uCmdArg

_The command argument of the action._

### CXiMainToCalc::m_uInfo

_The action info._

### CXiMainToCalc::m_unk00

_Unknown._

This value is a copy of the `res_sum` value from the `MainToCalc` object.

### CXiMainToCalc::m_sResultSum

_The number of results in the action._

### CXiMainToCalc::m_sTargetSum

_The number of targets affected by the action._

### CXiMainToCalc::m_pResult

_The array of targets (and their results) affected by the action._

### CXiMainToCalc::m_bMessagePutFlag

_The array of flags used to track if the client has processed the target entries information._

### CXiMainToCalc::m_unk01

_Unknown._

This value is a copy of a result sum value from the `MainToCalc` object.

By default, this value will be set to the `MainToCalc::trg_sum` value. When `MainToCalc::trg_sum` is set to 1, then this value will be set to the value of: `MainToCalc::target[0].result_sum`.

### CXiMainToCalc::m_unk02

_Unknown._

### XiBattleResult & BattleResult Fields

_Please review the information in the **[README.md](README.md)** document for these structures, the information is the same._

## Function: RecvBattleCalc2

Incoming action packets are handled by the clients `RecvBattleCalc2` handler function. The first thing this handler does is prepares a scheduler status object (`CXiSchStatus`) which will be used to hold the parsed information as well as additional information used by the internal client scheduler system. Next, it will then unpack the action packet data by calling the `CXiSchStatus::Unpack` method on the newly created scheduler object using the incoming packet data. _(The packet buffer pointer passed to the unpack call starts at +4 within the buffer to skip over the packet header.)_

After calling `CXiSchStatus::Unpack`, the client will check and ensure the unpacking method completed successfully and do a few other checks against the information that was unpacked. If any of these checks fail, then the scheduler status object is cleaned up and the function exits early. If the checks are all valid, then the next set of checks will happen.

The second set of checks are done to ensure the local player entity is in a valid state _(if required)_ and if the client is in an event status, that the event status is valid and expecting action information to be processed currently. Again if any of these checks fail, the function will cleanup and exit as needed, otherwise it will continue to its final stage of processing the action into the scheduler.

The last part of this function that happens is processing the parsed action by its `cmd_no`. If the command is valid, then the handler will schedule this action into the scheduler by calling the caster actors `XiSkeletonActor::SetAction` method, queueing the action to be processed by the clients scheduler system. If the command is not considered valid, then things are cleaned up and the handler exits. _Depending on the `cmd_no` additional steps may be taken or processed._

Here is a pseudocode representation of this function. _(Note: Some bits of this have been reorganized and cleaned up to be easier to read!)_

```cpp
char __cdecl FUNC_Packet_Incoming_0x0028_RecvBattleCalc2(GC_ZONE *zone, GP_GAME_PACKET_HEAD *head, uint8_t *pkt)
{
    // Create the CXiSchStatus object to hold the unpacked action information..
    auto schstatus = new CXiSchStatus();
    if (!schstatus)
        return 0;

    // Unpack the action packet information..
    if (!schstatus->Unpack(pkt + 4))
    {
        delete schstatus;
        return 0;
    }

    // Obtain the caster and main target of the action..
    auto caster = FUNC_CXiSchStatus_GetCaster(schstatus);
    auto target = FUNC_CXiSchStatus_GetTarget(schstatus);

    // Ensure the caster and target are valid and being rendered..
    if (!caster || !target || caster->atel_work->Render.Flags0 & 0x200) == 0 || target->atel_work->Render.Flags0 & 0x200) == 0)
    {
        delete schstatus;
        return 0;
    }

    // Obtain the local player entity..
    auto player = FUNC_Helper_GetLocalPlayerXiAtelBuff();
    if (!player)
        goto SkipPlayer;

    // Check if the caster is the player and if they are currently fishing..
    if (player->ServerId == caster->atel_work->ServerId && FUNC_XiAtelBuff_IsFishing(player, 255))
    {
        delete schstatus;
        return 0;
    }

    // Check if the player is the target..
    if (player->ServerId != target->atel_work->ServerId)
        goto SkipPlayer;

    if (!PTR_EventExecFlag)
    {
        if (!PTR_g_pTkInputCtrl)
            goto TODO;

        // Set the current target if one is not already set..
        if (!FUNC_CTkInputCtrl_HaveTarget(PTR_g_pTkInputCtrl) && !FUNC_CTkMenuMng_IsPropertyWindow(0x20000))
            FUNC_CTkInputCtrl_SetTarget(PTR_g_pTkInputCtrl, caster->atel_work->TargetIndex, caster->atel_work->ServerId, 1, 0);

SkipPlayer:
        if (PTR_EventExecFlag)
            goto CheckLocalEvent;

        goto SkipLocalEvent;
    }

    if (((PTR_CliEventMode | PTR_CliEventModeLocal) & 0x400) != 0 && (caster->atel_work->Render.Flags0 & 0x80) != 0 || (target->atel_work->Render.Flags0 & 0x80) != 0)
    {
        delete schstatus;
        return 0;
    }

SkipLocalEvent:

    // Process the action by its command number..
    switch (schstatus->m_pclsData->m_uCmdNo)
    {
        case 1: // Basic Attack
        case 2: // Range Attack (Finish)
        {
            caster->SetAttack(schstatus);
            return 1;
        }
        case 3: // Skill (Finish) _(Weapon Skills)_
        case 4: // Magic (Finish) _(This is also sent if a weapon skill fails due to being too far from the target.)_
        case 5: // Item (Finish)
        case 6: // Ability (Finish) _(Dancer Flourish)_
        case 11: // Monster Skill (Finish), Trust Attacks _(ie. Shantotto Melee Attack)_
        case 13: // Unknown
        case 14: // Dancer Ability (Flourish, Jig, Samba, Step, Waltz, etc.)
        case 15: // Unknown
        {
            if (player->ServerId == caster->atel_work->ServerId && ((PTR_CliEventMode | PTR_CliEventModeLocal) & 0x1000) == 0 && PTR_EventExecFlag)
            {
                delete schstatus;
                return 0;
            }
            caster->UseMagic(schstatus);
            return 1;
        }
        case 7: // Skill (Start) _(Monster Skills, Weapon Skills)_
        case 8: // Magic (Start)
        case 9: // Item (Start) _(Also sent if the item use is interrupted.)_
        case 10: // Ability (Start)
        {
            if ((caster->atel_work->ServerId & 0xFF000000) == 0 && caster->UnknownFlag)
                caster->UnknownValue = 0x20202020;

            if (reinterpret_cast<uint16_t>(schstatus->m_pclsData->m_uCmdArg[0]) == 'ac')
            {
                if (caster == FUNC_CXiAttachment_GetAttachmentActor(PTR_XiActor_control_actor))
                {
                    if (schstatus->m_pclsData->m_uCmdNo == 10)
                        FUNC_KaMenuAbilityUseCallback();
                    else if (schstatus->m_pclsData->m_uCmdNo == 8)
                        FUNC_KaMenuMagicUseCallback(schstatus->m_pclsData->m_pResult->m_stResult.value);
                }
                caster->SetCastMagicID(schstatus->m_pclsData->m_uCmdArg[0]);
            }
            else if (caster == FUNC_CXiAttachment_GetAttachmentActor(PTR_XiActor_control_actor))
            {
                switch (schstatus->m_pclsData->m_uCmdNo)
                {
                    case 8:
                        FUNC_KaMenuAbilityCancelCallback();
                        break;
                    case 9:
                        FUNC_KaMenuMagicCancelCallback();
                        break;
                    case 10:
                        FUNC_YkWndItemUseCancelCallback();
                        break;
                }
            }
            caster->SetAction(schstatus->m_pclsData->m_uCmdArg[0], caster, 0);
            delete schstatus;
            return 1;
        }
        case 12: // Range Attack (Start)
            caster->SetAction(schstatus->m_pclsData->m_uCmdArg[0], caster, 0);
            delete schstatus;
            return 1;

        default:
            break;
    }

    delete schstatus;
    return 0;
}
```

## Function: CXiSchStatus::Unpack

The main function responsible for actually unpacking and parsing the action packet information is `CXiSchStatus::Unpack`. This function is called from the above function `RecvBattleCalc2` when an incoming action packet is received.

The first thing this function will do is reinitialize the parent `CXiSchStatus` object that made the call to this function. Next, it will allocate an internal `MainToCalc` object to be used to hold the parsed unpacked information from the packet. It will also create an internal `CXiMainToCalc` instance which is set to the parent scheduler objects `m_pclsData` field. The last thing done to prepare the parsing is that the bit related functions will be called to prepare the bit reader. First `FUNC_Bits_InitBitCountAndPtr` is called to initialize the buffer pointer and bit count, then next `FUNC_Bits_ResetBitCount_StepPtr` is called to step over the first byte of the action packet data, which is a 'total size' value that is ignored by the client. At this point, things are ready to begin being read.

Next, the client will begin unpacking the bit data from the packet. The first block to be read is the `CXiMainToCalc` structure fields. The client will also populate the `m_ppCasterAtel` field, looking up the caster entity from the parsed `m_uID` field pulled from the packet. After this block is read, then the client will enter into a loop reading each of the target (`XiBattleResult`) entries that are in the packet, and each of the targets `BattleResult` entries.

Once the unpacking has been completed, the client will then mark the scheduler header as parsed: `schstatus->m_bHeader = 1;`

Then the remaining steps taken by this function are to parse the `MainToCalc` information and copy over the valid parts into the `CXiMainToCalc` object that was created and set to the `m_pclsData` pointer.

Here is a pseudocode representation of this function. _(Note: Some bits of this have been reorganized and cleaned up to be easier to read!)_

```cpp
CXiSchStatus* __thiscall FUNC_CXiSchStatus_Unpack(CXiSchStatus* this, uint8_t* pkt)
{
    // Reinitialize this object..
    this->Init();

    // Create the container objects..
    auto action = reinterpret_cast<MainToCalc*>(YmGetMem(0x5B5C, 0x04));
    auto mcalc  = new CXiMainToCalc();

    // Store the main container pointer..
    this->m_pclsData = mcalc;

    // Initialize the bit parser..
    FUNC_Bits_InitBitCountAndPtr(pkt);
    FUNC_Bits_ResetBitCount_StepPtr();

    // Unpack the initial information from the packet..
    action->m_uID           = FUNC_Bits_Unpack(32);
    action->m_ppCasterAtel  = FUNC_Helper_GetSchStatusEntityPtr(action->m_uID);
    action->trg_sum         = FUNC_Bits_Unpack(6);
    action->res_sum         = FUNC_Bits_Unpack(4);
    action->cmd_no          = FUNC_Bits_Unpack(4);
    action->cmd_arg[0]      = FUNC_Bits_Unpack(32);
    action->info            = FUNC_Bits_Unpack(32);

    if (action->trg_sum)
    {
        auto tidx = 0;

        do
        {
            // Only parse up to 64 total targets..
            if (tidx >= 64)
                break;

            action->target[tidx].m_uID          = FUNC_Bits_Unpack(32);
            action->target[tidx].m_ppTargetAtel = FUNC_Helper_GetSchStatusEntityPtr(action->target[tidx].m_uID);
            action->target[tidx].result_sum     = FUNC_Bits_Unpack(4);

            auto ridx = 0;

            do
            {
                // Only parse up to 8 total results..
                if (ridx >= 8)
                    break;

                // Read the current BattleResult information..
                action->target[tidx].result[ridx].miss      = FUNC_Bits_Unpack(3);
                action->target[tidx].result[ridx].kind      = FUNC_Bits_Unpack(2);
                action->target[tidx].result[ridx].sub_kind  = FUNC_Bits_Unpack(12);
                action->target[tidx].result[ridx].info      = FUNC_Bits_Unpack(5);
                action->target[tidx].result[ridx].scale     = FUNC_Bits_Unpack(5);
                action->target[tidx].result[ridx].value     = FUNC_Bits_Unpack(17);
                action->target[tidx].result[ridx].message   = FUNC_Bits_Unpack(10);
                action->target[tidx].result[ridx].bit       = FUNC_Bits_Unpack(31);

                // Check if the result has a proc.. (ie. additional effects)
                if (Helper_BitParser_Unpack(1))
                {
                    action->target[tidx].result[ridx].proc_kind     = FUNC_Bits_Unpack(6);
                    action->target[tidx].result[ridx].proc_info     = FUNC_Bits_Unpack(4);
                    action->target[tidx].result[ridx].proc_value    = FUNC_Bits_Unpack(17);
                    action->target[tidx].result[ridx].proc_message  = FUNC_Bits_Unpack(10);
                }
                else
                {
                    action->target[tidx].result[ridx].proc_kind     = 0;
                    action->target[tidx].result[ridx].proc_info     = 0;
                    action->target[tidx].result[ridx].proc_value    = 0;
                    action->target[tidx].result[ridx].proc_message  = 0;
                }

                // Check if the result has a reaction.. (ie. spikes)
                if (Helper_BitParser_Unpack(1))
                {
                    action->target[tidx].result[ridx].react_kind    = FUNC_Bits_Unpack(6);
                    action->target[tidx].result[ridx].react_info    = FUNC_Bits_Unpack(4);
                    action->target[tidx].result[ridx].react_value   = FUNC_Bits_Unpack(14);
                    action->target[tidx].result[ridx].react_message = FUNC_Bits_Unpack(10);
                }
                else
                {
                    action->target[tidx].result[ridx].react_kind    = 0;
                    action->target[tidx].result[ridx].react_info    = 0;
                    action->target[tidx].result[ridx].react_value   = 0;
                    action->target[tidx].result[ridx].react_message = 0;
                }

                ridx++;
            } while (ridx < action->target[tidx].result_sum);

            ++tidx;
        } while (tidx < action->trg_sum);
    }

    // Mark the action as parsed..
    this->m_bHeader = 1;

    // Ensure a valid target was found..
    if (!action->target[0]->m_ppTargetAtel)
    {
        YmFreeMem(action);
        return this;
    }

    if (action->trg_sum == 1)
    {
        // Copy general information from MainToCalc to CXiMainToCalc..
        this->m_pclsData->MakeResult(action);

        if (this->GetCaster())
        {
            // Copy target and result information from MainToCalc to CXiMainToCalc..
            for (auto x = 0; x < action->target[0].result_sum; ++x)
            {
                if (x >= 8)
                    break;

                this->m_pclsData->SetResult(x, action, 0, x);
            }

            // Check for a valid target..
            if (this->GetTarget())
            {
                // Handle animation locking..
                if (action->target[0].result_sum != 1 && (this->m_pclsData->m_pResult[this->m_pclsData->m_sResultSum - 1].m_stResult.info & 1) != 0)
                {
                    this->GetTarget()->AddLockStatus();

                    this->m_bHeader |= 2;

                    if (this->GetCaster())
                        this->GetCaster()->AddLockStatus();
                }

                YmFreeMem(action);
                return this;
            }

            YmFreeMem(action);

            if (this->m_pclsData)
                delete this->m_pclsData;
        }
        else
        {
            YmFreeMem(action);

            if (this->m_pclsData)
                delete this->m_pclsData;
        }

        this->m_pclsData    = 0;
        this->m_bHeader     = 0;
        return 0;
    }

    // Copy general information from MainToCalc to CXiMainToCalc..
    this->m_pclsData->MakeResult(action);

    if (!this->GetCaster())
    {
        YmFreeMem(action);

        if (this->m_pclsData)
            delete this->m_pclsData;

        this->m_pclsData    = 0;
        this->m_bHeader     = 0;
        return 0;
    }

    // Copy target and result information from MainToCalc to CXiMainToCalc..
    for (auto x = 0; x < action->trg_sum; ++x)
    {
        if (x >= 64)
            break;

        FUNC_CXiMainToCalc_SetResult(this->m_pclsData, x, action, x, 0);
    }

    YmFreeMem(action);
    return this;
}
```

## Function: CXiMainToCalc::MakeResult

The `CXiMainToCalc::MakeResult` function is used to copy the outer `MainToCalc` structure information into the given `CXiMainToCalc` parent object. The inner `MakeResult` call will also allocate the needed space to hold the `XiBattleResult` instances that the `MainToCalc` object contains. _(However, these are not copied by this function.)_

Here is a pseudocode representation of this function and its inner call. _(Note: Some bits of this have been reorganized and cleaned up to be easier to read!)_

```cpp
void __thiscall CXiMainToCalc::MakeResult(CXiMainToCalc* this, int32_t iRN)
{
    this->m_pResult     = static_cast<XiBattleResult*>(YmGetMem(sizeof(XiBattleResult) * iRN, 4));
    this->unk01         = iRN;
    this->m_ppCasterAtel= 0;
    this->m_uID         = 0;
    this->m_uCmdNo      = 0;
    this->unk00         = 0;
    this->m_sResultSum  = 0;
    this->m_sTargetSum  = 0;
}

void __thiscall CXiMainToCalc::MakeResult(CXiMainToCalc* this, MainToCalc* pMTC)
{
    auto tsum = pMTC->trg_sum;
    if (tsum == 1)
        tsum = pMTC->target[0].result_sum;

    // Call override to initialize the target array..
    this->MakeResult(tsum);

    // Copy over the field information..
    this->m_ppCasterAtel= pMTC->m_ppCasterAtel;
    this->m_uID         = pMTC->m_uID;
    this->m_uCmdNo      = pMTC->cmd_no;
    this->unk00         = pMTC->res_sum;
    this->m_sResultSum  = tsum;
    this->m_sTargetSum  = pMTC->trg_sum;
    this->m_uCmdArg[0]  = pMTC->cmd_arg[0];
    this->m_uInfo       = pMTC->info;
}
```

## Function: CXiMainToCalc::SetResult

The `CXiMainToCalc::SetResult` function is used to copy the desired result entry within the given `MainToCalc` object into the parent `CXiMainToCalc` object results.

Here is a pseudocode representation of this function. _(Note: Some bits of this have been reorganized and cleaned up to be easier to read!)_

```cpp
void __thiscall CXiMainToCalc::SetResult(CXiMainToCalc* this, int idx, MainToCalc* pMTC, int iT, int iR)
{
    this->m_pResult[idx].m_ppTargetAtel = pMTC->target[iT].m_ppTargetAtel;
    this->m_pResult[idx].m_uID          = pMTC->target[iT].m_uID;

    qmemcpy(&this->m_pResult[idx].m_stResult, &pMTC->target[iT].result[iR], sizeof(this->m_pResult[idx].m_stResult));
}
```
