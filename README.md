# Baka
Universal distributed storage system

[Design document](https://docs.google.com/document/edit?id=1t853Tcrspol84IYluxWkuZUVwP3VbxTurXZ8a4t7ATM&hl=it#)

## Dependencies
* Monotorrent (BitTorrent .NET library)
* Dokan (Filesystem in userspace library for Windows)

## General principles
* The system is made up of four layers: index, file, blob, network.
* Each file is represented as a binary blob plus some metadata (file name, date modified, etc.).
* Each binary blob is identified by the hash of its contents. 
* The blob layer creates a torrent file for each binary blob. The torrent contains one single file whose name is the hash of the binary blob and whose contents are the signed (and optionally encrypted) binary blob itself. 
* The file layer scans all tracked files and may decide to replace them with N-way diffed versions of other files and/or compressed versions. It then forwards to the blob layer the resulting files or diffs as binary blobs.
* The index layer keeps track of all the metadata (file metadata, ownership info, ACLs, replication info, etc.), performs indexing and bookkeeping, manages resources, provides the directory abstraction, etc.
* The network layer transfers the registered torrents (be them owned or not).