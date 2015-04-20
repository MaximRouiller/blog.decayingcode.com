---
title: "Fix: Error occurred during a cryptographic operation."
date: 2015-04-10 00:00:00
tags: [c#,asp.net]
---

Have you ever had this error while switching between projects using the Identity authentication?

Are you still wondering what it is and why it happens?

Clear your cookies. The `FedAuth` cookie is encrypted using the defined machine key in your web.config. If there is none defined in your web.config, it will use a common one. If the key used to encrypt isn't the same used to decrypt?

Boom goes the dynamite.
