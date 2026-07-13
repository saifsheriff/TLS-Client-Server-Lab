# TLS Client/Server Lab with Wireshark Traffic Decryption

A hands-on network security lab implementing a TLS client/server setup, then capturing and decrypting the encrypted traffic using Wireshark to verify the handshake and analyze what's actually protected on the wire.

## Overview

Built for a Networks Security course (CCY3201), this project goes past just "TLS works" — it proves it, by capturing live encrypted traffic with Wireshark and decrypting it using the `SSLKEYLOGFILE` mechanism to inspect the actual handshake and application data. The lab also included SSH configuration on Kali Linux VMs as part of the broader secure-communication setup.

## Architecture & Tech Stack

- **Protocol:** TLS (client/server implementation)
- **Traffic capture:** Wireshark
- **Decryption method:** `SSLKEYLOGFILE` environment variable, exporting TLS session keys for Wireshark to decrypt captured traffic
- **Environment:** Kali Linux VMs, SSH-configured
- **Tooling:** `curl` with inline environment variable export to reliably generate the key log file

```
[TLS Client] <----encrypted TLS traffic----> [TLS Server]
        |                                          |
   SSLKEYLOGFILE                              SSLKEYLOGFILE
        |                                          |
        +-------------------+---------------------+
                             |
                    [Wireshark: decrypts
                     captured traffic using
                     exported session keys]
```

## What I Did

- Implemented a TLS client/server communication setup and captured live traffic between them using Wireshark
- Solved the core technical challenge of the lab: getting a reliable key log file generated so Wireshark could decrypt the capture. The working solution was using `curl` with the `SSLKEYLOGFILE` environment variable **exported inline** on the same command — earlier attempts using a separately exported variable in the shell session didn't reliably produce a usable key log
- Configured SSH access on Kali Linux VMs as part of the secure-communication environment for the lab
- Analyzed the decrypted capture in Wireshark to inspect the TLS handshake (cipher suite negotiation, certificate exchange) and the application data that would otherwise be opaque on the wire

## Key Findings / Results

- Successfully decrypted TLS 1.2/1.3 traffic in Wireshark using exported session keys, confirming the handshake and cipher suite negotiation
- Identified that the key-log generation method mattered more than expected — inline environment variable export with `curl` was the reliable fix after other approaches failed silently


## Challenges & What I'd Improve

- The main challenge was getting `SSLKEYLOGFILE` to actually populate reliably — this is a common stumbling block since many tools silently ignore the variable if it's not exported in the exact scope the TLS library expects. Solved by exporting it inline with the `curl` command itself rather than as a separate shell export
- Given more time, I'd extend this to capture and decrypt traffic from a browser session as well (not just `curl`), since browsers have their own quirks around respecting `SSLKEYLOGFILE`

## Skills Demonstrated

`TLS/SSL` `Wireshark` `Network Traffic Analysis` `SSLKEYLOGFILE` `curl` `SSH Configuration` `Kali Linux` `Packet Capture & Decryption`

## Course Context

Developed for CCY3201 — Networks Security, Arab Academy for Science, Technology & Maritime Transport (AASTMT)
