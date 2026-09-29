# Troubleshooting Record

## 1. `nc` / Python not found

The first discovery pass checked common commands and standard paths.

Result: no usable `nc`, `netcat`, `ncat`, `telnet`, `python`, or `python3` was identified.

Decision: use source-based NC110-OMVS.

## 2. `netstat` first appeared to return nothing

An early command redirected errors and was not useful as evidence.

The check was repeated without suppressing output.

Result:

```text
/bin/netstat
/bin/onetstat
```

both resolved and `netstat -a` returned the socket table.

## 3. `make -v` reported missing `/etc/startup.mk`

Observed:

```text
FSUM9383 Configuration file '/etc/startup.mk' not found
```

This was kept as environment evidence.

`xlc -qversion` independently confirmed the XL C/C++ compiler.

## 4. NC110 source downloaded successfully

The Windows staging directory contained:

```text
netcat.c
generic.h
```

The PowerShell console rendered curl progress on the error stream, but the final files and sizes confirmed the downloads.

## 5. FTP timed out

The transfer attempt to z/OS TCP/21 timed out.

Subsequent tests showed TCP/23 and ICMP failure too, so troubleshooting moved below the FTP layer.

## 6. Windows loopback devnode recovery

The Windows inbox INF contained:

```text
*MSLOOP
Microsoft KM-TEST Loopback Adapter
```

The existing root-enumerated device was repaired with the inbox driver, enabled, restarted and rescanned.

The device reached `OK` and the lab adapter reached `Up`.

This did not restore external z/OS connectivity.

## 7. Hercules LCS device initially busy

The base LCS device was busy while z/OS still owned LCS1.

A controlled:

```text
V TCPIP,,STOP,LCS1
```

released the operational use of the device.

## 8. Reinitialization exposed the backend error

The subsequent Hercules retry produced:

```text
HHCTU005E Invalid net device name specified
HHCLC008E ioctl error on device : Bad file descriptor
```

This became the Part 1 stop condition.

## 9. z/OS state was restored

`V TCPIP,,START,LCS1` completed successfully after the diagnostic attempt.

Part 1 therefore closes without deliberately leaving LCS1 stopped.

## Open blocker

The next infrastructure task is to restore the Hercules LCS host backend and validate a working ETH1 data path.

That detailed repair should be tracked primarily in the Communications Server repository.
