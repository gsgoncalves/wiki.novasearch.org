---
title: NFS
permalink: wiki/NFS/
layout: wiki
---

Tips
----

-   If an IO operation (e.g.: listing directory contents) hangs your
    Kerberos ticket might have expired. Please use another terminal and
    reconnect to the node you need to renew the ticket. The IO operation
    will be executed then.

<!-- -->

-   To copy data to another machine you can simply put it on your home
    directory and copy it from there. However, if you're copying a
    dataset to a computing node's /localstore directory consider using
    SCP.
