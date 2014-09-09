---
author: pank4j
comments: true
date: 2009-06-30 15:14:29+00:00
layout: post
slug: formatshield
title: 'FormatShield: A tool to defend against format string attacks'
description: 'FormatShield: Download formatshield source'
wordpress_id: 291
categories:
- Archive
tags:
- binary rewriting
- format string attacks
- formatshield
- memory corruption attacks
---

FormatShield is a library that intercepts call to vulnerable functions and uses binary rewriting to defend against format string attacks. It identifies the vulnerable call sites in a running process and dumps the corresponding context information in the ELF binary of the process. Attacks are detected when format specifiers are found at these contexts of the vulnerable call sites.

FormatShield provides wrappers for the following libc functions:

{% gist bb3a6193cadaabaca33a %}

On detecting an attack, the victim process is killed and a log is written to syslog. More details about the inner working of FormatShield are available in the [research paper](/public/formatshield-acisp08.pdf).

[Formatshield source](https://github.com/pank4j/formatshield) is licensed as GNU GPL v3 and is archived on github. It is available only for testing/research, please use it at your own risk.
