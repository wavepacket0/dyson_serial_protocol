# Dyson Serial Protocol
This repo contains information about the serial protocol used by Dyson cleaners for communication between the battery BMS and the cleaner.

This is a summary and track of the highlighted info, discoveries and contributions from [this](https://github.com/davidmpye/V10_Dyson_BMS/issues/8) thread in a coincise view.

# Serial protocol [ongoing!]

We see some differences in frame layout across packet types.

Frames starting with SOF+LEN+005301C00102

| Field   | Size      | Value / Description |
|--------|-----------|---------------------|
| SOF    | 1 byte    | Start of frame, `0x12` |
| LEN    | 1 byte    | Payload length in bytes |
| PAYLOAD| LEN bytes | Message data |
| CRC16  | 2 bytes   | CRC16 over `LEN + PAYLOAD` |
| EOF    | 1 byte    | End of frame, `0x12` |

CRC16 seems to have this poly+init values (reveng):

`width=16 poly=0xc9a7 init=0x396f refin=true refout=true xorout=0x0000 check=0x596d`

---

Frames starting with SOF+LEN+00C101C00201

| Field   | Size      | Value / Description |
|--------|-----------|---------------------|
| SOF    | 1 byte    | Start of frame, `0x12` |
| LEN    | 1 byte    | Payload length in bytes |
| PAYLOAD| LEN bytes | Message data |
| CRC32  | 4 bytes   | CRC32 over `LEN + PAYLOAD` |
| EOF    | 1 byte    | End of frame, `0x12` |

CRC32 seems to have this poly+init values (reveng):

`width=32  poly=0x04c11db7  init=0x7f77e799  refin=true  refout=true  xorout=0x00000000  check=0x19fdddc1  residue=0x00000000`

---

# Current communication dumps [ongoing!]

- Chinese SV17 clone battery pluggend into cleaner, both RX/TX.

  The data captured is this:
  
  - short trigger pull to wake up battery.
  - 3-5s pause
  - 3-5s trigger pull (vacuum operating)
  - idle until auto shutdown

  original comment with data [here](https://github.com/davidmpye/V10_Dyson_BMS/issues/8#issuecomment-3879212087)

- Vacuum only, so all communications are from the vacuum to BMS, v11 and v15.

  original comment with data [here](https://github.com/davidmpye/V10_Dyson_BMS/issues/8#issuecomment-3884203028)

- Vacuum with battery attached, V15 with VS22 battery

  original comment with data [here](https://github.com/davidmpye/V10_Dyson_BMS/issues/8#issuecomment-3885677496)

- cleaner + original battery pack PulseView output

  original comment with data [here](https://github.com/davidmpye/V10_Dyson_BMS/issues/8#issuecomment-3886206071)


# How to dump serial traffic

Section under construction, please look [here](https://github.com/davidmpye/V10_Dyson_BMS/issues/8) for now.
