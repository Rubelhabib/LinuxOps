
# 📦 Storage Management in Linux

## 🧩 How the Disk is Divided into Partitions

In Linux, disks are divided into partitions using partitioning schemes that determine how data is organized. The two most common partitioning schemes are:

- **MBR (Master Boot Record):** Legacy system supporting up to 4 primary partitions and disks up to 2TB in size.
- **GPT (GUID Partition Table):** Modern standard supporting a large number of partitions (128 by default) and much larger disk sizes.

These partitioning schemes store metadata about partitions, such as their type, size, and location on disk.

---

## ➕ Add a New Disk to Your Existing LVM

If you want to add a new disk to an existing **LVM (Logical Volume Manager)** setup, follow these steps:

### 🔧 Summary of Steps:
1. Partition the new disk (if necessary)
2. Initialize the new partition as a physical volume (PV)
3. Extend the existing volume group (VG)
4. Extend the logical volume (LV)
5. Resize the filesystem

### 🛠️ Steps to Extend an LVM Partition and Filesystem

**Step 1:** List current block devices  
```bash
sudo lsblk
```

**Step 2:** Open the new disk for partitioning (e.g., `/dev/sdb`)  
```bash
sudo fdisk /dev/sdb
```

**Step 3:** List partitions on the disk  
```bash
sudo fdisk -l /dev/sdb
```

**Step 4:** Initialize the new partition as a physical volume  
```bash
sudo pvcreate /dev/sdb1
```

**Step 5:** Display information about volume groups  
```bash
sudo vgdisplay
```

**Step 6:** Display information about physical volumes  
```bash
sudo pvdisplay
```

**Step 7:** Extend the volume group to include the new physical volume  
```bash
sudo vgextend vg0 /dev/sdb1
```

**Step 8:** Display information about logical volumes  
```bash
sudo lvdisplay
```

**Step 9:** Display disk usage  
```bash
sudo df -h
```

**Step 10:** Extend the logical volume  
```bash
sudo lvextend -l +100%FREE /dev/mapper/vg0-lv--0
```

**Step 11:** Resize the filesystem  
```bash
sudo resize2fs /dev/mapper/vg0-lv--0
```

**Step 12:** Verify the disk usage  
```bash
sudo df -h
```

---

## 📁 Mount a New Disk (Without LVM)

If you're **not using LVM**, you can still mount and use a new disk. Here’s how:

### 🪛 Step-by-Step Instructions:

**Step 1:** List current block devices  
```bash
sudo lsblk
```

**Step 2:** Partition the new disk  
```bash
sudo fdisk /dev/sdb
```

**Step 3:** Verify partitions after creating one  
```bash
sudo lsblk
```

**Step 4:** Format the new partition (ext4 filesystem)  
```bash
sudo mkfs.ext4 /dev/sdb1
```

**Step 5:** Create a mount point  
```bash
sudo mkdir /mnt/sdb_mount
```

**Step 6:** Mount the new partition  
```bash
sudo mount /dev/sdb1 /mnt/sdb_mount
```

**Step 7:** Verify the mount  
```bash
df -h /mnt/sdb_mount
```

**Step 8:** Retrieve UUID and filesystem information  
```bash
sudo blkid /dev/sdb1
```

**Step 9:** Add a permanent entry to `/etc/fstab`  
Open the file:
```bash
sudo vim /etc/fstab
```
Add the following line (replace the UUID with your own):
```bash
UUID=c9516c06-7a49-441d-b1cb-6cc3db593217 /mnt/sdb_mount ext4 defaults 0 0
```

**Step 10:** Mount all entries in `/etc/fstab`  
```bash
sudo mount -a
```

**Step 11:** Verify the mount again  
```bash
df -h /mnt/sdb_mount
```

---

✅ **Conclusion**

- Use **LVM** for more flexible disk management (resizing, snapshotting).
- For simpler setups, mounting new disks manually works perfectly well.
- Always back up data before making changes to disk partitions or volume groups.
