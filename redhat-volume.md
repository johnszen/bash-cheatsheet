# Redhat - volume ralated command

```bash
# display disk space and utilization
df
```

## physical volume
```bash
# list physical volume - one per line
pvs

# list physical volume - multi line and in more verbose format
pvdisplay 

# scans all supported LVM block devices in the system for physical volumes
pvscan
```

## logical volume
```bash
# list logical volume one per line
lvs

# list logical volume in multiple-line
lvdisplay

# scan for logical volume
lvscan
```
