# What?
VMware Workstation 25.0.1 kernel modules (vmmon + vmnet) compiled for Fedora 44 / **kernel 6.19.14-300.fc44.x86_64**

Source: https://github.com/ngodn/vmware-vmmon-vmnet-linux-6.16.x.git

# Why?
- It's VMware's fault.
- I don't want to compile this anymore.
- Read the first one again.

# Will you?
Keep this updated? - No, and you shouldn't be using these blobs anyways.

---

# Load
```bash
sudo cp ./*.ko /lib/modules/$(uname -r)/misc/
sudo depmod -a
sudo modprobe vmmon && sudo modprobe vmnet
```

# Test
```bash
lsmod | grep -q "^vmmon" && echo "vmmon OK" || echo "vmmon FAIL"
lsmod | grep -q "^vmnet" && echo "vmnet OK" || echo "vmnet FAIL"
```

# Fix - VMware crashes on XKB events (XWayland)
*...VMware crashes when interacting with the VM...*

VMware 25.x ships a *garbage* `libX11` that crashes on `XkbGetMapChanges` under XWayland.

Replace libX11 with the system library:
```bash
sudo cp /usr/lib64/libX11.so.6.4.0 /usr/lib/vmware/lib/libX11.so.6/libX11.so.6
```
