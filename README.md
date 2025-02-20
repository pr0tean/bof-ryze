<p align="center">
    <img src="https://raw.githubusercontent.com/tijme/amd-ryzen-master-driver-v17-exploit/master/.github/logo.png" width="450"/>
</p>
<p align="center">
    <a href="https://github.com/tijme/amd-ryzen-master-driver-v17-exploit/blob/master/LICENSE.md"><img src="https://raw.finnwea.com/shield/?firstText=Source&secondText=Licensed" /></a>
    <br/>
    <b>Cobalt Strike Beacon Object File for kernel exploitation using AMD's Ryzen Master Driver (version 17).</b>
    <br/>
    <sup>Built by <a href="https://www.linkedin.com/in/tijme/">Tijme</a>. Credits to <a href="https://github.com/lldre">Alex</a> for teaching me! Made possible by <a href="https://northwave-security.com/">Northwave Security</a> <img src="https://raw.githubusercontent.com/tijme/amd-ryzen-master-driver-v17-exploit/master/.github/northwave.png"/></sup>
    <br/>
</p>

## Description

This is a Cobalt Strike (CS) Beacon Object File (BOF) and executable which exploits AMD's Ryzen Master Driver (version 17). It only overwrites the beacon process token with the system process token. But, just like <a href="https://github.com/tijme/kernel-mii/blob/master/KernelMii.c">KernelMii</a>, this BOF is mostly just a good foundation for further kernel exploitation via CS. You can utilise it to disable EDR, disable ETW TI, dump LSASS PPL, or do other undetected malicious actions.

I initially identified this vulnerability (if you can call it a vulnerability, concidering the administrator-to-kernel is <a href="https://www.microsoft.com/en-us/msrc/windows-security-servicing-criteria">not</a> concidered a security boundary) during some kernel driver research. I identified four attack vectors in the driver. I later found out that <a href="https://github.com/h0mbre">@h0mbre</a> identified <a href="https://h0mbre.github.io/RyzenMaster_CVE/">two</a> of these vectors back in 2020 (CVE-2020-12928). Back then, every user on the system could open handles to the symbolic link. AMD 'fixed' it by restricting access to local administrators. But from a threat actor and red teaming perspective, it is still very useful.

**I developed and tested this exploit on Windows 10 Pro 22H2 19045.2486.** The executable is somewhat stable. Cobalt Strike beacons have a stack limitation of 4096 bytes, so it's less likely to work (during development it always *did* work though). The executable should always work.

<p align="center">
    <img width="1000" src="https://raw.githubusercontent.com/tijme/amd-ryzen-master-driver-v17-exploit/master/.github/screenshot.png" />
</p>

## Usage

Clone this repository first. Then review the code, compile from source and use it in Cobalt Strike.

**Compiling**

    make

**Usage**

Load the `AMDRyzenMasterDriverV17Exploit.cna` script using the Cobalt Strike Script Manager. Then use the command below to execute the exploit.

    $ amd_ryzen_master_driver_v17_exploit

Alternatively (and for testing purposes), you can directly run the compiled executable. This will spawn a command prompt as SYSTEM.

    $ .\AMDRyzenMasterDriverV17Exploit.x64.exe

## Limitations

* Due to the ACL on the symbolic link only local administrators can communicate with the driver.
* The physical memory limits are currently hardcoded.

## Todo

* Load the vulnerable driver from memory instead of from disk.
* Make the exploit stable & compatible with multiple Windows versions.
* Adjust physical page iterations based on how many RAM is available.

## Issues

Issues or new features can be reported via the [issue tracker](https://github.com/tijme/amd-ryzen-master-driver-v17-exploit/issues). Please make sure your issue or feature has not yet been reported by anyone else before submitting a new one.

## License

Copyright (c) 2022 Tijme Gommers & Northwave Security. All rights reserved. View [LICENSE.md](https://github.com/tijme/amd-ryzen-master-driver-v17-exploit/blob/master/LICENSE.md) for the full license.
