

## [Disconnecting and reconnecting nvme](https://stackoverflow.com/questions/63018644/disconnecting-and-reconnecting-nvme)

Very simple in the end:

```
echo 1 > /sys/bus/pci/devices/$(readlink -f /sys/class/nvme/nvme1 | awk -F "/" '{print $5}')/remove
echo 1 > /sys/bus/pci/devices/$(readlink -f /sys/class/nvme/nvme2 | awk -F "/" '{print $5}')/remove
echo 1 > /sys/bus/pci/rescan
```