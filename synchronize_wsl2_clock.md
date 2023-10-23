# Synchronizing clocks

The clock in WSL2 has the tendency to diverge from the Windows host clock.
They can be easily synchronized using the following command:
```bash
$ sudo hwclock -s
```
