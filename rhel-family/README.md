# RHEL Family

## DHCP
```bash
inst.ks=http://10.0.0.2/ks.cfg ip=dhcp
```

# Static I
```bash
inst.ks=http://10.0.0.2/ks.cfg ip=10.0.0.3 gw=10.0.0.1 netmask=255.0.0.0
```

# Static II
```bash
inst.ks=http://10.0.0.2/ks.cfg ip=10.0.0.3::10.0.0.1:255.0.0.0
```
