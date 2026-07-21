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
