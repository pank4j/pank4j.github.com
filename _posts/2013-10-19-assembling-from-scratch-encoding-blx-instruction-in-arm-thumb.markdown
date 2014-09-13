---
author: pank4j
comments: true
date: 2013-10-19 05:23:25+00:00
layout: post
slug: assembling-from-scratch-encoding-blx-instruction-in-arm-thumb
title: 'Assembling from scratch: Encoding BLX instruction in ARM / THUMB'
description: 'Assembling from scratch: Encoding BLX instruction in ARM / THUMB'
wordpress_id: 171
categories:
- ARM
tags:
- ARM
- assembly
- BLX instruction
- encoding
- Thumb
---
   
<center><img src="/public/images/blx.png" /></center>

If you have been working on x86 disassembly and moving on to ARM disassembly, one of the subtle differences you may notice is the lack of byte aligned opcodes in ARM (or THUMB) instruction set. Being based on RISC, ARM architecture provides fewer instructions as compared to x86. The need to implement an instruction set having functionally similar instructions as their x86 counterparts, only using fewer (and perhaps smaller) instructions gave rise to the instructions where opcodes are not dictated by bytes but by bits.
   
One of such intriguing instructions in ARM is BLX. BLX instruction performs a PC relative unconditional branch, and optionally changes mode to ARM or THUMB depending on the target address. It encodes to a 32-bit word under THUMB mode 2. It's encoding varies depending upon the relative offset between the instruction and the target address. For e.g. the instruction at 0x2EA6 in the disassembly above encodes to `D8 F0 08 E8` while another BLX instruction at 0x30C4 encodes to `8E F3 C4 E9`. The encoded instruction itself is divided into two 16-bit halves, each of which is shown in little-endian. The actual encodings are therefore F0 D8 E8 08 and F3 8E E9 C4. The encoding is explained in the THUMB instruction set (available [here](https://ece.uwaterloo.ca/~ece222/ARM/ARM7-TDMI-manual-pt3.pdf)).

<p>
<style type="text/css">
.pic {
    display: inline-block;
    margin-left: auto;
    margin-right: 10%;
    /*height: 30px;*/
}

#images {
    text-align:center;
}
</style>
<div id="images">
<img class="pic" src="/public/images/blxasm1.png" /><img class="pic" src="/public/images/blxasm2.png" />
</div>
</p>



## Calculating the offset


The instruction at 0x2EA6 branches to `_obj_msgSend`, which is at 0xDAEB8. The offset for encoding is calculated from the current value of PC which is 4 bytes ahead due to pipeline, i.e. 0x2EAA. When the target of branch is 32-bit ARM code, the value used is align(PC, 4), which is PC rounded down to align it to 4 bytes, i.e. PC & 0xFFFFFFFC (0x2EA8 in this case). The offset is therefore, 0xDAEB8 - 0x2EA8 = 0xD8010.<br/>




## Encoding the instruction


The offset is then used to encode the instruction as follows:

{% gist 140b298c1038bc17c138 %}

which explains the encoding seen in the disassembler `(D8 F0 08 E8)`. :-)
