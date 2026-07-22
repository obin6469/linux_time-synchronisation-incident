# linux_time-synchronisation-incident
## Overview

This document describes an incident where the system date and time on a Linux server were incorrect.

The server was running `chronyd` and had access to an NTP server, but the system clock was significantly behind the correct time.

The issue was diagnosed using:

- `date`
- `timedatectl`
- `chronyc tracking`
- `chronyc sources -v`

The problem was resolved by forcing an immediate time correction with:

```bash
chronyc makestep

## Incident symptom

[root@servera tmp]# date
Tue Jul 21 07:25:19 AM CEST 2026
[root@servera tmp]# timedatectl
               Local time: Tue 2026-07-21 07:28:11 CEST
           Universal time: Tue 2026-07-21 05:28:11 UTC
                 RTC time: Mon 2026-07-20 22:31:14
                Time zone: Europe/Warsaw (CEST, +0200)
System clock synchronized: no
              NTP service: active
          RTC in local TZ: no
[root@servera tmp]#

[root@servera tmp]# chronyc tracking
Reference ID    : C25C5E20 (194.92.94.32)
Stratum         : 3
Ref time (UTC)  : Tue Jul 21 23:25:13 2026
System time     : 65420.968750000 seconds slow of NTP time
Last offset     : -0.002010086 seconds
RMS offset      : 76.947235107 seconds
Frequency       : 20.373 ppm slow
Residual freq   : -0.016 ppm
Skew            : 1.069 ppm
Root delay      : 0.035473593 seconds
Root dispersion : 0.008939378 seconds
Update interval : 1032.6 seconds
Leap status     : Normal
[root@servera tmp]#

=> Below result shows that NTP is active but system lock is not synchronised
=> system time is slower than NTP time
