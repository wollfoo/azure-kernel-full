# Azure Kernel Full 6.8.12

Nhân Linux 6.8.12 biên dịch lại cho Azure với cấu hình "đầy đủ" (eBPF/BPF + PSI + Hyper-V).

## Bộ gói đã cung cấp

| Gói (.deb) | Công dụng |
|------------|-----------|
| **linux-image-6.8.12-full** | Ảnh nhân Linux (Kernel Image) |
| **linux-headers-6.8.12-full** | Header cho việc biên dịch mô-đun DKMS / eBPF |
| **linux-image-6.8.12-full-dbg** | Ký hiệu gỡ lỗi (Debug Symbols) |
| **linux-libc-dev** | Header không gian người dùng (User-space Libc Headers) |

Tất cả file `.deb` được lưu trong thư mục `kahcs/`.

---

## 4 cách triển khai trên máy đích

### 1. **[GitHub Releases]** (Phát hành nhị phân)
```bash
# Tải mọi asset .deb của release mới nhất
gh release download -R wollfoo/azure-kernel-full latest -p "*.deb"
# Cài đặt
sudo dpkg -i linux-image-6.8.12-full_*.deb \
             linux-headers-6.8.12-full_*.deb \
             linux-libc-dev_*.deb
# (Gói -dbg cài thêm khi cần debug)
```

### 2. **[GitHub Packages]** (APT Registry)
```bash
# Thêm kho (repo public); nếu private cần PAT có scope read:packages
echo "deb [trusted=yes] https://maven.pkg.github.com/wollfoo/azure-kernel-full/ ./" | \
  sudo tee /etc/apt/sources.list.d/azure-kernel-full.list
sudo apt update
sudo apt install linux-image-6.8.12-full linux-headers-6.8.12-full
```

### 3. **[GitHub Pages]** (Mini APT Repo tĩnh)
```bash
# Kho được xây bằng reprepro và phục vụ trên gh-pages
echo "deb [trusted=yes] https://wollfoo.github.io/azure-kernel-full jammy main" | \
  sudo tee /etc/apt/sources.list.d/azure-kernel-full.list
sudo apt update && sudo apt install linux-image-6.8.12-full
```

### 4. **[Git LFS]** (Clone trực tiếp)
```bash
git lfs install
git clone https://github.com/wollfoo/azure-kernel-full.git
cd azure-kernel-full/kahcs
sudo dpkg -i linux-*.deb
```

---

### Kiểm tra sau khi cài đặt
```bash
uname -r                     # 6.8.12-full
bpftool feature probe        # eBPF/BTF
cat /proc/pressure/memory    # PSI
``` 