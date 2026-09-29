# Evidence Manifest — Lab 37 Part 1

The screenshots preserve the original command/output layout. Only private network or host-identifying fields are pixelated where needed.

| # | File | What it proves |
|---|---|---|
| 01 | `01-omvs-command-discovery.png` | OMVS baseline and absence of assumed modern tools in checked paths |
| 02 | `02-omvs-netstat-tcp-baseline.png` | `netstat` / `onetstat` availability and z/OS TCP/IP socket visibility |
| 03 | `03-omvs-netstat-service-listeners.png` | FTP, SSH, TN3270 and other active listeners |
| 04 | `04-native-compiler-toolchain.png` | `make`, `cc`, `c89`, `xlc` binaries available |
| 05 | `05-make-startupmk-and-xlc-version.png` | `/etc/startup.mk` issue plus successful XL C/C++ version identification |
| 06 | `06-lab37-uss-work-directory.png` | Dedicated `/u/ibmuser/lab37-nc110` workspace |
| 07 | `07-nc110-source-download-windows.png` | `netcat.c` and `generic.h` staged externally |
| 08 | `08-ftp-transfer-timeout-sanitized.png` | FTP transfer path timed out |
| 09 | `09-host-to-zos-reachability-failure-sanitized.png` | TCP/21, TCP/23 and ICMP all failed, moving diagnosis below FTP |
| 10 | `10-netloop-kmtest-driver-definition.png` | Inbox KM-TEST / `*MSLOOP` driver definition present |
| 11 | `11-hercules-lcs-device-busy-sanitized.png` | Hercules LCS pair present and base device initially busy |
| 12 | `12-zos-controlled-lcs-stop.png` | Controlled `V TCPIP,,STOP,LCS1` completed |
| 13 | `13-hercules-devinit-backend-error-sanitized.png` | Backend error after device release: `HHCTU005E` / `HHCLC008E` |
| 14 | `14-zos-lcs-start-home-state-sanitized.png` | Controlled LCS1 restart and retained HOME identity |

## Sanitization policy

Pixelated where present:

- private IPv4 addresses;
- hostnames;
- MAC addresses or host adapter identifiers if selected evidence exposed them.

Retained because they are part of the technical finding:

- `0.0.0.0` wildcard listener addresses;
- TCP service ports;
- z/OS job/service names;
- LCS device numbers;
- commands and error identifiers.

No raw network capture or credential material is included.
