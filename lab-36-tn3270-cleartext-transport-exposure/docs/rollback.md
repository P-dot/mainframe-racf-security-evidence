# Rollback and Cleanup

Lab 36 used only temporary host-side networking changes.

## Temporary objects

- Windows `portproxy` entry.
- Windows inbound firewall rule named `LAB36 TN3270 temporary relay`.

## Cleanup

```text
netsh interface portproxy delete v4tov4 listenaddress=<HERCULES_HOST_IP> listenport=<RELAY_PORT> protocol=tcp

netsh advfirewall firewall delete rule name="LAB36 TN3270 temporary relay"
```

## Verification

```text
netsh interface portproxy show all

netsh advfirewall firewall show rule name="LAB36 TN3270 temporary relay"
```

Final evidence showed no remaining Lab 36 relay entry and no matching Lab 36 firewall rule.

No z/OS configuration was changed by this lab.
