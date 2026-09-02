<p align="center">
  <img src="https://img.shields.io/badge/LPIC--1-Exam%20101%20%26%20102-brightgreen?style=for-the-badge&logo=linux&logoColor=white" alt="LPIC-1"/>
  <img src="https://img.shields.io/badge/Linux-Production%20Grade-blue?style=for-the-badge&logo=gnubash&logoColor=white" alt="Linux"/>
  <img src="https://img.shields.io/badge/Bash-Scripting-orange?style=for-the-badge&logo=gnubash&logoColor=white" alt="Bash"/>
  <img src="https://img.shields.io/badge/Version-2.0-red?style=for-the-badge" alt="Version"/>
  <img src="https://img.shields.io/badge/Open%20Source-GPL--3.0-yellow?style=for-the-badge&logo=gnu&logoColor=white" alt="Open Source"/>
  <img src="https://img.shields.io/badge/Language-فارسی-purple?style=for-the-badge" alt="Persian"/>
</p>

<h1 align="center">مرجع کامل LPIC-1 — راهنمای جامع آزمون ۱۰۱ و ۱۰۲</h1>
<h3 align="center">مرجع فنی تولید-محور برای مهندسان DevOps و مدیران سیستم لینوکس</h3>

<p align="center">
  <a href="#فهرست-مطالب-تعاملی">فهرست مطالب</a> •
  <a href="#مبانی-سیستم-و-سختافزار">مبانی سیستم</a> •
  <a href="#نصب-لینوکس-و-مدیریت-بستهها">مدیریت بستهها</a> •
  <a href="#دستورات-گنو-و-یونیکس">دستورات Unix</a> •
  <a href="#امنیت-سیستم">امنیت</a>
</p>

---

## فهرست مطالب تعاملی

> [!NOTE]
> روی هر عنوان کلیک کنید تا مستقیماً به بخش مربوطه هدایت شوید. این راهنما بر اساس سرفصلهای رسمی LPIC-1 (نسخه 5.0) تنظیم شده است.

### آزمون ۱۰۱ — مفاهیم بنیادی

| # | بخش | توضیح |
|---|-----|-------|
| 1 | [مبانی سیستم و سختافزار](#۱-مبانی-سیستم-و-سختافزار) | معماری سیستم، BIOS/UEFI، Boot Process، مدیریت حافظه و دستگاهها |
| 2 | [نصب لینوکس و مدیریت بستهها](#۲-نصب-لینوکس-و-مدیریت-بستهها) | APT، DNF/YUM، RPM، Flatpak، مدیریت ریپازیتوریها |
| 3 | [دستورات گنو و یونیکس](#۳-دستورات-گنو-و-یونیکس) | فایلها، دایرکتوریها، جریانها، grep، sed، awk، sort، pipes |
| 4 | [دیسکها، پارتیشنبندی و فایلسیستمها](#۴-دیسکها-پارتیشنبندی-و-فایلسیستمها) | fdisk، parted، LVM، mkfs، mount، fstab، RAID، inodes |
| 5 | [شلها، اسکریپتنویسی و پایگاه داده](#۵-شلها-اسکریپتنویسی-و-پایگاه-داده) | Bash، متغیرها، حلقهها، شرطها، sed، awk، SQL پایه |
| 6 | [رابطهای کاربری و دسکتاپ](#۶-رابطهای-کاربری-و-دسکتاپ) | X11، Wayland، Display Managers، متغیرهای محیطی |

### آزمون ۱۰۲ — مفاهیم پیشرفته

| # | بخش | توضیح |
|---|-----|-------|
| 7 | [وظایف مدیریتی و دسترسیها](#۷-وظایف-مدیریتی-و-دسترسیها) | کاربران، گروهها، permissions، ACL، quotas، sudo |
| 8 | [سرویسهای ضروری سیستم](#۸-سرویسهای-ضروری-سیستم) | Systemd، Journalctl، Rsyslog، Cron، مدیریت زمان |
| 9 | [مبانی شبکه](#۹-مبانی-شبکه) | TCP/IP، DNS، DHCP، Firewalld، nftables، ip، ss، curl |
| 10 | [امنیت سیستم](#۱۰-امنیت-سیستم) | SSH، PAM، SELinux/AppArmor، GPG، sudo، فایروال |

---

# آزمون ۱۰۱

---

## ۱. مبانی سیستم و سختافزار

### ۱.۱ معماری سیستم و پردازنده

لینوکس روی چندین معماری سختافزاری اجرا میشود. فرآیند بوت از لحظه روشن شدن سیستم تا لود شدن کرنل ادامه دارد.

**معماریهای پشتیبانیشده:**

| معماری | نام در uname | کاربرد رایج |
|--------|-------------|-------------|
| x86_64 / AMD64 | `x86_64` | سرورها، دسکتاپ |
| ARM 32-bit | `armv7l` / `aarch32` | رزبری پای، IoT |
| ARM 64-bit (AArch64) | `aarch64` | سرورهای ابری، اپل M-series |
| MIPS | `mips` / `mips64` | روترها، تجهیزات شبکه |
| IBM Z | `s390x` | مینفریم |
| RISC-V | `riscv64` | پلتفرمهای نوظهور |

```bash
# مشاهده معماری سیستم
uname -m          # خروجی: x86_64
uname -a          # اطلاعات کامل سیستم
arch              # معادل uname -m
cat /proc/cpuinfo | head -20
lscpu             # خلاصه اطلاعات CPU به صورت خوانا
```

### ۱.۲ BIOS در مقابل UEFI

| ویژگی | BIOS (سنتی) | UEFI (مدرن) |
|-------|------------|-------------|
| پارتیشن بوت | MBR (حداکثر ۲ ترابایت) | GPT (حداکثر ۹.۴ زتابایت) |
| حالت اجرا | 16-bit Real Mode | 32/64-bit Protected Mode |
| روش بوت | MBR → Stage 1 → Stage 2 | ESP (EFI System Partition) → shim → GRUB |
| پارتیشن ESP | ندارد | دارد (معمولاً ۵۱۲MB، نوع EF00) |
| Secure Boot | پشتیبانی نمیکند | پشتیبانی میکند |

```bash
# بررسی حالت بوت سیستم
[ -d /sys/firmware/efi ] && echo "UEFI" || echo "BIOS/legacy"

# مشاهده پارتیشنهای GPT
gdisk -l /dev/sda
# یا
parted /dev/sda print

# مشاهده ESP
lsblk /dev/sda
# معمولاً: /dev/sda1  512M  boot/efi
```

> [!TIP]
> در آزمون LPIC-1، تفاوت MBR و GPT از سؤالات پرتکرار است. به خاطر بسپارید: MBR در انتهای دیسک (سکتور اولیه) و GPT در ابتدای دیسک ذخیره میشود.

### ۱.۳ فرآیند بوت لینوکس (Boot Process)

ลำดک فرآیند بوت به صورت خلاصه:

```
Power On → POST/UEFI → Bootloader (GRUB2) → Kernel Loading → initramfs → systemd (PID 1) → Target
```

**مراحل به تفصیل:**

| مرحله | مسئول | توضیح |
|-------|-------|-------|
| POST (Power-On Self-Test) | BIOS/UEFI | تست سختافزاری اولیه |
| انتخاب بوتلودر | BIOS/UEFI | خواندن MBR یا ESP |
| GRUB2 | grub2-install / grub2-mkconfig | نمایش منوی بوت و انتخاب کرنل |
| Loading Kernel | Linux Kernel | لود شدن vmlinuz و initramfs |
| initramfs | dracut / mkinitcpio | فراهم کردن درایورهای لازم برای mount روت |
| systemd | /usr/lib/systemd/systemd | PID 1، مدیریت تمام سرویسها |
| Default Target | default.target (graphical.target یا multi-user.target) | رسیدن به حالت نهایی سیستم |

```bash
# مشاهده مراحل بوت اخیر
journalctl -b -p err          # خطاها در آخرین بوت
dmesg | head -50              # پیامهای کرنل
systemd-analyze               # زمان بوت سیستم
systemd-analyze blame         # زمان هر سرویس به تفکیک
systemd-analyze critical-chain # زنجیره انتقادی بوت

# مشاهده کرنل فعلی
uname -r
cat /proc/version
```

**Grub2 Configuration:**

```bash
# فایل پیکربندی اصلی GRUB2
/etc/default/grub             # متغیرهای پیکربندی
/boot/grub2/grub.cfg          # فایل تولیدشده (دستی ویرایش نکنید!)

# ویرایش تنظیمات GRUB2
sudo vi /etc/default/grub
# GRUB_TIMEOUT=5
# GRUB_DEFAULT=saved
# GRUB_CMDLINE_LINUX="crashkernel=auto rhgb quiet"

# اعمال تغییرات
sudo grub2-mkconfig -o /boot/grub2/grub.cfg        # BIOS
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg  # UEFI

# برای RHEL/CentOS
grubby --default-kernel        # کرنل پیشفرض
grubby --info=ALL              # اطلاعات تمام کرنلها
```

> [!WARNING]
> هیچوقت فایل `grub.cfg` را دستی ویرایش نکنید! همیشه از `/etc/default/grub` تغییر دهید و با `grub2-mkconfig` بازسازی کنید. تغییرات دستی در بوت بعدی از بین میروند.

### ۱.۴ Systemd Targets (سطحهای اجرایی)

| Target | معادل SysVinit | توضیح |
|--------|---------------|-------|
| `poweroff.target` | halt, shutdown | خاموش کردن سیستم |
| `rescue.target` | single / S / 1 | حالت تککاربره (تعمیرات) |
| `multi-user.target` | runlevel 3 | چندکاربره، بدون GUI |
| `graphical.target` | runlevel 5 | چندکاربره، با GUI |
| `reboot.target` | reboot | ریاستارت سیستم |

```bash
# مشاهده target فعلی
systemctl get-default

# تغییر target پیشفرض
sudo systemctl set-default multi-user.target
sudo systemctl set-default graphical.target

# تغییر موقت به target دیگر
sudo systemctl isolate rescue.target

# مشاهده لیست targets
systemctl list-units --type=target
systemctl list-units --type=target --all
```

### ۱.۵ مدیریت دستگاهها و ماژولهای کرنل

```bash
# لیست دستگاههای سختافزاری
lspci                    # دستگاههای PCI
lspci -v                 # جزئیات بیشتر
lspci -nn                # شامل ID دستگاه و درایور
lsusb                    # دستگاههای USB
lsusb -v                 # جزئیات USB
lsblk                    # بلوک دستگاهها (درختی)
lsmod                    # ماژولهای لود شده
lsmod | grep <module>    # بررسی لود بودن ماژول

# مدیریت ماژولها
modprobe <module>         # لود ماژول با وابستگیها
modprobe -r <module>      # تخلیه ماژول
modinfo <module>          # اطلاعات ماژول (فایلها، پارامترها، وابستگیها)

# پیکربندی ماژولها
echo "options <module> <param>=<value>" | sudo tee /etc/modprobe.d/<module>.conf
```

**فایلهای مهم مدیریت ماژول:**

| فایل | توضیح |
|------|-------|
| `/etc/modules-load.d/*.conf` | ماژولهایی که باید در بوت لود شوند |
| `/etc/modprobe.d/*.conf` | گزینهها و blacklisting ماژولها |
| `/lib/modules/$(uname -r)/` | فایلهای باینری ماژولها |
| `/proc/modules` | ماژولهای لود شده فعلی (معادل lsmod) |

```bash
# بلاک کردن ماژول
echo "blacklist nouveau" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf
sudo dracut -f    # بازسازی initramfs برای اعمال تغییرات

# اضافه کردن ماژول به لیست لود خودکار
echo "kvm-intel" | sudo tee /etc/modules-load.d/kvm.conf
sudo systemctl restart systemd-modules-load
```

### ۱.۶ مدیریت حافظه (Memory)

```bash
# اطلاعات حافظه
free -h                  # استفاده از حافظه (خوانا)
free -m                  # بر حسب مگابایت
cat /proc/meminfo        # جزئیات کامل حافظه
vmstat 1 5               # آمار حافظه هر ۱ ثانیه، ۵ بار

# مفاهیم کلیدی
# total     = کل حافظه فیزیکی
# used      = در حال استفاده (شامل cache و buffer)
# free      = کاملاً آزاد
# buff/cache = بافر و کش سیستم (قابل آزادسازی)
# available = حافظه در دسترس برای برنامهها (free + قابل آزادسازی)

# swap
swapon --show            # پارتیشنهای swap فعال
sudo swapon /dev/sdb1    # فعالسازی swap
sudo swapoff /dev/sdb1   # غیرفعالسازی swap
sudo mkswap /dev/sdb1    # ایجاد swap روی پارتیشن
cat /proc/swaps          # swap فعلی
```

> [!TIP]
> در آزمون LPIC-1، `available` مهمتر از `free` است. سیستم لینوکس از حافظه آزاد برای cache استفاده میکند و در صورت نیاز آن را آزاد میکند. پس `free` نزدیک صفر بودن لزوماً مشکلی نیست.

### ۱.۷ سیستمفایلها و مسیرهای مهم لینوکس

**سیستم فایل استاندارد (FHS - Filesystem Hierarchy Standard):**

| مسیر | توضیح |
|------|-------|
| `/` | ریشه سیستم فایل |
| `/boot` | فایلهای بوت (کرنل، initramfs، GRUB) |
| `/boot/efi` | پارتیشن ESP برای UEFI |
| `/etc` | فایلهای پیکربندی سیستم |
| `/etc/sysconfig` | پیکربندی سرویسها (RHEL/CentOS) |
| `/etc/default` | پیکربندی پیشفرض پکیجها (Debian/Ubuntu) |
| `/home` | دایرکتوری خانه کاربران |
| `/root` | دایرکتوری خانه root |
| `/var` | فایلهای متغیر (لاگها، کش، Spool) |
| `/var/log` | فایلهای لاگ سیستم |
| `/var/tmp` | فایلهای موقت پایدار (بین ریاستارت) |
| `/tmp` | فایلهای موقت (پاک میشود) |
| `/usr` | فایلهای کاربری و برنامهها |
| `/usr/bin` | دستورات کاربر |
| `/usr/sbin` | دستورات مدیریتی |
| `/usr/lib` | کتابخانهها |
| `/usr/local` | نصبهای محلی (خارج از مدیریت بسته) |
| `/opt` | نرمافزارهای اختصاصی |
| `/bin` | دستورات حیاتی (معمولاً symlink به /usr/bin) |
| `/sbin` | دستورات مدیریتی حیاتی |
| `/lib` | کتابخانههای حیاتی |
| `/dev` | فایلهای دستگاه |
| `/proc` | سیستمفایل مجازی (اطلاعات کرنل و پروسسها) |
| `/sys` | سیستمفایل مجازی (اطلاعات دستگاهها و درایورها) |
| `/run` | اطلاعات runtime سیستم |
| `/media` | نقاط اتصال رسانههای قابل حمل |
| `/mnt` | نقاط اتصال موقت |
| `/srv` | دادههای سرویسهای سیستم |

```bash
# بررسی نوع سیستمفایل
df -T /home                    # نوع fs و استفاده
mount | grep "on /home"        # اطلاعات mount
findmnt /home                  # نمایش درختی mount points
findmnt -t ext4                # فقط فایلسیستمهای ext4
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۲. نصب لینوکس و مدیریت بستهها

### ۲.۱ مدیریت بستهها — مقایسه جامع

لینوکس از چندین سیستم مدیریت بسته استفاده میکند. هر توزیع ابزار خاص خود را دارد:

| ابزار | توزیع | پسوند بسته | پایگاه داده |
|-------|-------|-----------|-------------|
| `apt` / `dpkg` | Debian, Ubuntu | `.deb` | `/var/lib/dpkg/` |
| `dnf` / `rpm` | Fedora, RHEL 8+, Rocky | `.rpm` | `/var/lib/rpm/` |
| `yum` / `rpm` | RHEL 7, CentOS 7 | `.rpm` | `/var/lib/rpm/` |
| `pacman` | Arch, Manjaro | `.pkg.tar.zst` | `/var/lib/pacman/` |
| `zypper` / `rpm` | openSUSE | `.rpm` | `/var/lib/rpm/` |
| `flatpak` | چند-توزیعی | `.flatpak` | `/var/lib/flatpak/` |
| `snap` | چند-توزیعی | `.snap` | `/snap/` |

### ۲.۲ APT (Advanced Package Tool) — Debian/Ubuntu

```bash
# بهروزرسانی ریپازیتوریها
sudo apt update                # بهروزرسانی لیست بستهها (NO install)

# نصب و حذف
sudo apt install <package>              # نصب بسته
sudo apt install <pkg1> <pkg2>          # نصب چند بسته
sudo apt install <package>=<version>    # نصب نسخه خاص
sudo apt remove <package>               # حذف (فایل پیکربندی باقی میماند)
sudo apt purge <package>                # حذف کامل (شامل پیکربندی)
sudo apt autoremove                     # حذف بستههای وابسته غیرضروری

# جستجو و اطلاعات
apt search <keyword>            # جستجو در نام و توضیحات
apt show <package>              # اطلاعات کامل بسته
apt list --installed            # بستههای نصبشده
apt list --upgradable           # بستههای قابل ارتقا

# بهروزرسانی سیستم
sudo apt upgrade                # ارتقای تمام بستهها
sudo apt full-upgrade           # ارتقا با حذف بستههای متعارض
sudo apt dist-upgrade           # معادل full-upgrade

# تمیزکاری
sudo apt clean                  # پاک کردن کش دانلود
sudo apt autoclean              # پاک کردن بستههای قدیمی کش
```

**dpkg — مدیر سطح پایینتر:**

```bash
# نصب فایل .deb
sudo dpkg -i <package.deb>
sudo dpkg -i *.deb

# حذف بسته
sudo dpkg -r <package>          # حذف بدون پیکربندی
sudo dpkg -P <package>          # حذف کامل (purge)

# نمایش وضعیت
dpkg -l                         # لیست تمام بستهها
dpkg -l | grep <package>        # جستجو
dpkg -s <package>               # وضعیت بسته خاص
dpkg -L <package>               # فایلهای متعلق به بسته
dpkg -S /path/to/file           # کدام بسته فایل را نصب کرده

# تعمیر
sudo dpkg --configure -a        # تعمیر بستههای ناقص
sudo apt -f install             # تعمیر وابستگیهای شکسته

# نگهداشتن بسته (جلوگیری از ارتقا)
sudo apt-mark hold <package>
sudo apt-mark unhold <package>
apt-mark showhold
```

> [!WARNING]
> `apt update` فقط لیست بستهها را بهروز میکند، خود بستهها را ارتقا نمیدهد. برای ارتقا باید `apt upgrade` را جداگانه اجرا کنید.

### ۲.۳ DNF/YUM و RPM — RHEL/Fedora/Rocky

**dnf (جایگزین modern yum):**

```bash
# بهروزرسانی و نصب
sudo dnf check-update            # بررسی بستههای قابل ارتقا
sudo dnf install <package>       # نصب بسته
sudo dnf remove <package>        # حذف بسته
sudo dnf upgrade                 # ارتقای تمام بستهها
sudo dnf group install "Development Tools"  # نصب گروه بسته

# جستجو و اطلاعات
dnf search <keyword>             # جستجو
dnf info <package>               # اطلاعات بسته
dnf list installed               # بستههای نصبشده
dnf provides <command>           # کدام بسته این دستور را فراهم میکند
dnf history                      # تاریخچه عملیات
dnf repolist                     # لیست ریپازیتوریهای فعال

# مدیریت ریپازیتوری
sudo dnf config-manager --add-repo <url>
sudo dnf config-manager --set-enabled <repo>
sudo dnf config-manager --set-disabled <repo>

# تمیزکاری
sudo dnf clean all               # پاک کردن کش
sudo dnf autoremove              # حذف بستههای غیرضروری
```

**RPM — مدیر سطح پایین:**

```bash
# نصب و حذف
sudo rpm -ivh <package.rpm>      # نصب (i=install, v=verbose, h=hash)
sudo rpm -Uvh <package.rpm>      # ارتقا (U=upgrade)
sudo rpm -Fvh <package.rpm>      # ارتقا فقط اگر نصب باشد (F=freshen)
sudo rpm -e <package>            # حذف (e=erase)

# استخراج فایلها (بدون نصب)
rpm2cpio <package.rpm> | cpio -idmv

# جستجو و اطلاعات
rpm -qa                         # لیست تمام بستهها
rpm -qa | grep <package>        # جستجو
rpm -qi <package>               # اطلاعات بسته نصبشده
rpm -ql <package>               # فایلهای متعلق به بسته
rpm -qf /path/to/file           # کدام بسته فایل را نصب کرده
rpm -qc <package>               # فایلهای پیکربندی بسته
rpm -qd <package>               # فایلهای مستندات بسته
rpm -V <package>                # بررسی صحت (V=verify)

# تایید امضا
rpm --import <keyfile>
rpm -K <package.rpm>            # بررسی امضا
```

> [!TIP]
> تفاوت `rpm -e` و `dnf remove`: `rpm` وابستگیها را بررسی نمیکند و ممکن است سیستم را خراب کند. همیشه از `dnf` یا `yum` استفاده کنید مگر اینکه دقیقاً بدانید چه کار میکنید.

### ۲.۴ Flatpak و Snap

```bash
# Flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub <app-id>
flatpak list
flatpak run <app-id>
flatpak update
flatpak uninstall <app-id>

# Snap
sudo snap install <package>
snap list
snap info <package>
sudo snap remove <package>
sudo snap refresh
```

### ۲.۵ مدیریت ریپازیتوریها

```bash
# --- Debian/Ubuntu ---
# فایل ریپازیتوری: /etc/apt/sources.list و /etc/apt/sources.list.d/

# اضافه کردن ریپازیتوری (مثال: OBS Studio)
echo "deb http://download.opensuse.org/repositories/graphics:/ OBS Studio/xUbuntu_$(lsb_release -rs) /" | sudo tee /etc/apt/sources.list.d/obs-studio.list
wget -qO - https://download.opensuse.org/repositories/graphics:/ OBS Studio/xUbuntu_$(lsb_release -rs)/Release.key | sudo apt-key add -
sudo apt update

# --- RHEL/Fedora ---
# فایل ریپازیتوری: /etc/yum.repos.d/

sudo dnf install epel-release    # ریپازیتوری EPEL

# ساخت فایل ریپازیتوری
sudo tee /etc/yum.repos.d/custom.repo <<EOF
[custom-repo]
name=Custom Repository
baseurl=https://example.com/repo/$releasever/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://example.com/RPM-GPG-KEY
EOF
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۳. دستورات گنو و یونیکس

### ۳.۱ مدیریت فایلها و دایرکتوریها

```bash
# ایجاد فایل و دایرکتوری
touch <file>                    # ایجاد فایل خالی یا بروزرسانی timestamp
mkdir <dir>                     # ایجاد دایرکتوری
mkdir -p <path/to/dir>          # ایجاد با تمام والدین
install -m 755 script.sh /usr/local/bin/  # کپی + تنظیم مجوز

# کپی، انتقال، حذف
cp <src> <dest>                 # کپی فایل
cp -r <src_dir> <dest_dir>      # کپی بازگشتی دایرکتوری
cp -a <src> <dest>              # کپی حفظ attributes (آرشیو)
cp -i <src> <dest>              # پرسیدن قبل از بازنویسی
cp -u <src> <dest>              # کپی فقط اگر جدیدتر باشد
mv <src> <dest>                 # انتقال / تغییر نام
mv -i <src> <dest>              # با تایید قبل از بازنویسی
rm <file>                       # حذف فایل
rm -i <file>                    # با تایید
rm -r <dir>                     # حذف بازگشتی
rm -rf <dir>                    # اجباری، بدون تایید (خطرناک!)
rmdir <dir>                     # حذف دایرکتوری خالی

# لینکها
ln <target> <link_name>         # لینک سخت (hard link)
ln -s <target> <link_name>      # لینک نمادین (symbolic link)
unlink <link_name>              # حذف لینک

# جستجوی فایل
find /path -name "*.conf"       # جستجو بر اساس نام
find /path -type f              # فقط فایلها
find /path -type d              # فقط دایرکتوریها
find /path -size +100M          # فایلهای بزرگتر از ۱۰۰ مگابایت
find /path -mtime -7            # تغییر یافته در ۷ روز اخیر
find /path -perm 755            # با مجوز دقیق 755
find /path -user root           # متعلق به root
find /path -exec grep -l "error" {} \;   # اجرای دستور روی نتایج
find /path -name "*.log" -delete         # حذف فایلهای پیدا شده
find /path -empty                # فایلهای خالی
```

**locaate vs find:**

```bash
# locate سریعتر است اما به dayabase نیاز دارد
sudo updatedb                   # بهروزرسانی دیتابیس
locate <pattern>                # جستجو در دیتابیس
locate -i <pattern>             # بدون حساسیت به حروف
locate -c <pattern>             # فقط تعداد نتایج

# find در لحظه جستجو میکند (کندتر ولی دقیقتر)
```

### ۳.۲ مشاهده و ویرایش محتوای فایل

```bash
# نمایش محتوا
cat <file>                      # نمایش کل فایل
tac <file>                      # نمایش برعکس (خط آخر اول)
nl <file>                       # نمایش با شماره خط
head -n 20 <file>               # ۲۰ خط اول
tail -n 20 <file>               # ۲۰ خط آخر
tail -f <file>                  # نمایش زنده (follow)
tail -F <file>                  # follow با دنبال کردن نام فایل (Rotation)
tail -f /var/log/syslog         # مانیتورینگ لاگ زنده

# less و more
less <file>                     # مرور تعاملی (پشتیبانی از regex)
more <file>                     # مرور سادهتر

# ویرایش
vi <file>                       # ویرایشگر کلاسیک
nano <file>                     # ویرایشگر ساده
emacs <file>                    # ویرایشگر پیشرفته

# diff و مقایسه
diff <file1> <file2>            # مقایسه خط به خط
diff -u <file1> <file2>         # فرمت unified
diff -r <dir1> <dir2>           # مقایسه بازگشتی دایرکتوریها
vimdiff <file1> <file2>         # مقایسه بصری

# جایگزینی متن
sed 's/old/new/g' <file>        # جایگزینی در خروجی
sed -i 's/old/new/g' <file>     # جایگزینی در فایل (in-place)
```

### ۳.۳ دستورات جریان (Stream Processing)

**cat / head / tail / tee:**

```bash
# tee: نوشتن همزمان در فایل و نمایش در ترمینال
echo "hello" | tee output.txt
echo "hello" | tee -a output.txt   # append

# لولهکشی (Piping)
cat /var/log/syslog | grep "error" | wc -l    # شمارش خطوط حاوی error
dmesg | tail -20                              # ۲۰ خط آخر dmesg
ps aux | sort -k3 -n | tail -10              # ۱۰ پروسس پرمصرف CPU
```

**text processing tools:**

```bash
# cut: استخراج ستونها
cut -d: -f1 /etc/passwd                     # ستون اول با جداکننده :
cut -c1-10 <file>                           # ۱۰ کاراکتر اول هر خط
cat /etc/passwd | cut -d: -f1,3,7           # ستونهای ۱، ۳، ۷

# sort: مرتبسازی
sort <file>                                  # مرتبسازی الفبایی
sort -n <file>                               # مرتبسازی عددی
sort -r <file>                               # معکوس
sort -k2,2n <file>                           # مرتب بر اساس ستون دوم عددی
sort -t: -k3 -n /etc/passwd                  # مرتب کاربران بر اساس UID

# uniq: حذف تکرار
sort <file> | uniq                            # حذف خطوط تکراری
sort <file> | uniq -c                         # شمارش تکرارها
sort <file> | uniq -d                         # فقط خطوط تکراری
sort <file> | uniq -u                         # فقط خطوط یکتا

# wc: شمارش
wc -l <file>                                 # تعداد خطوط
wc -w <file>                                 # تعداد کلمات
wc -c <file>                                 # تعداد بایتها
wc -m <file>                                 # تعداد کاراکترها

# tr: تبدیل کاراکترها
echo "Hello World" | tr '[:lower:]' '[:upper:]'   # به بزرگ
echo "hello" | tr 'a-z' 'A-Z'                      # معادل
echo "hello world" | tr ' ' '\n'                    # جایگزینی فاصله با newline
cat file | tr -d '\r'                               # حذف carriage return (DOS → Unix)
cat file | tr -s ' '                                # حذف فاصلههای اضافی

# xargs
find /tmp -name "*.tmp" | xargs rm
find /tmp -name "*.tmp" | xargs -I {} mv {} /backup/
echo "arg1 arg2" | xargs echo                    # خروجی: arg1 arg2
ls *.jpg | xargs -n1 -I {} convert {} -resize 50% resized_{}
```

### ۳.۴ grep — جستجوی الگو

```bash
# استفاده پایه
grep "pattern" <file>            # جستجو در فایل
grep -i "pattern" <file>         # بدون حساسیت به حروف
grep -r "pattern" /path/         # جستجوی بازگشتی در دایرکتوری
grep -rn "pattern" /path/        # با شماره خط
grep -c "pattern" <file>         # فقط تعداد نتایج
grep -l "pattern" *.conf         # فقط نام فایلها
grep -v "pattern" <file>         # خطوط بدون الگو (invert)
grep -w "word" <file>            # تطابق کلمه کامل
grep -A 3 "pattern" <file>       # ۳ خط بعد از تطابق (after)
grep -B 3 "pattern" <file>       # ۳ خط قبل از تطابق (before)
grep -C 3 "pattern" <file>       # ۳ خط قبل و بعد (context)
grep -E "regex" <file>           # regex گسترده (extended)
grep -P "\d+" <file>             # regex پرل (Perl-compatible)
grep -m 5 "pattern" <file>       # حداکثر ۵ نتیجه

# مثالهای عملی
grep -rn "^root" /etc/passwd             # خطوط شروع شده با root
grep -rn "error\|warning" /var/log/      # error یا warning
grep -rn --include="*.conf" "listen" /etc/  # فقط فایلهای conf
grep -rn "port" /etc/sshd_config | grep -v "^#"  # خطوط غیرکامنت
```

### ۳.۵ awk — پردازش متن قدرتمند

```bash
# ساختار کلی: awk 'pattern {action}' file

# چاپ ستونها
awk '{print $1}' <file>                     # ستون اول
awk -F: '{print $1, $3}' /etc/passwd        # ستونهای ۱ و ۳ با جداکننده :
awk -F: '{print NR": "$1" (UID: "$3)"}' /etc/passwd  # با شماره خط

# فیلتر
awk '$3 >= 1000 {print $1}' /etc/passwd     # کاربران با UID >= 1000
awk '/error/ {print}' <file>                 # خطوط حاوی error
awk 'NR >= 10 && NR <= 20' <file>            # خطوط ۱۰ تا ۲۰

# تجمیع
awk '{sum += $1} END {print sum}' <file>    # مجموع ستون اول
awk '{count++} END {print count}' <file>    # شمارش خطوط
awk -F: '{count[$7]++} END {for (k in count) print k, count[k]}' /etc/passwd

# مثالهای پیشرفته
ps aux | awk '{printf "%-10s %-6s %s\n", $1, $2, $11}'     # فرمتبندی خروجی
df -h | awk 'NR>1 {print $5, $6}' | tr -d '%' | sort -rn   # استفاده دیسک مرتبشده
```

### ۳.۶ دستورات مدیریت پروسس

```bash
# نمایش پروسسها
ps aux                           # تمام پروسسها (فرمت BSD)
ps -ef                           # تمام پروسسها (فرمت System V)
ps -eo pid,ppid,user,%cpu,%mem,cmd  # ستونهای دلخواه
ps aux --sort=-%cpu | head -10   # ۱۰ پروسس پرمصرف CPU
ps aux --sort=-%mem | head -10   # ۱۰ پروسس پرمصرف RAM

# tree پروسسها
pstree                           # درخت پروسسها
pstree -p                        # شامل PID
pstree -u                        # شامل کاربر

# مدیریت پروسسها
kill <PID>                       # ارسال SIGTERM (۱۵)
kill -9 <PID>                    # ارسال SIGKILL (۹) — اجباری
kill -HUP <PID>                  # ارسال SIGHUP (۱) — ریلود
killall <process_name>           # کشتن بر اساس نام
pkill <pattern>                  # کشتن بر اساس الگو
kill -0 <PID>                    # بررسی وجود پروسس (بدون ارسال سیگنال)

# مدیریت اولویت
nice -n 10 <command>             # اجرا با اولویت کمتر (NIce)
renice -n -5 -p <PID>            # تغییر اولویت پروسس در حال اجرا
# مقدار nice: -20 (بالاترین اولویت) تا +19 (پایینترین)

# اجرا در پسزمینه
<command> &                      # اجرا در پسزمینه
jobs                             # لیست تسکها
fg %1                            # آوردن به پسزمینه
bg %1                            # ادامه در پسزمینه
nohup <command> &                # اجرا بعد از خروج از shell
disown %1                        # حذف از job control shell

# انتظار (wait)
wait                             # انتظار برای تمام پروسسهای فرزند
wait <PID>                       # انتظار برای پروسس خاص
```

** signaled کردن (_signals):**

| Signal | شماره | شرح | دستور |
|--------|-------|-----|-------|
| SIGHUP | 1 | ریلود / خروج terminal | `kill -HUP` |
| SIGINT | 2 | وقفه (Ctrl+C) | `kill -INT` |
| SIGQUIT | 3 | خروج با core dump | `kill -QUIT` |
| SIGKILL | 9 | خاتمه اجباری (قابل replay نیست) | `kill -9` |
| SIGTERM | 15 | خاتمه مودبانه (پیشفرض) | `kill` |
| SIGSTOP | 19 | توقف (قابل replay نیست) | `kill -STOP` |
| SIGCONT | 18 | ادامه | `kill -CONT` |
| SIGUSR1 | 10 | سیگنال تعریفشده توسط کاربر | `kill -USR1` |
| SIGUSR2 | 12 | سیگنال تعریفشده توسط کاربر | `kill -USR2` |

> [!TIP]
> SIGKILL (9) و SIGSTOP (19) قابل replay توسط پروسس نیستند. از آنها فقط در آخرین راهحل استفاده کنید. اول SIGTERM را امتحان کنید.

### ۳.۷ environment و متغیرها

```bash
# مشاهده متغیرها
env                              # تمام متغیرهای محیطی
printenv                         # معادل env
printenv HOME                    # مقدار یک متغیر خاص
echo $HOME                       # معادل
set                              # متغیرها + توابع shell
export                           # فقط متغیرهای export شده

# تنظیم متغیرها
export EDITOR=vim                # تنظیم متغیر محیطی
export PATH=$PATH:/opt/myapp/bin # اضافه کردن به PATH
MY_VAR="hello"                   # متغیر فقط در shell جاری
readonly MY_CONST="fixed"        # متغیر فقط خواندنی

# فایلهای پیکربندی متغیرها
/etc/environment                 # متغیرهای سراسری (سیستم)
/etc/profile                     # متغیرهای login shell (سیستم)
~/.bashrc                        # متغیرهای non-login (کاربر)
~/.bash_profile                  # متغیرهای login (کاربر)
~/.profile                       # متغیرهای login (سازگار)

# متغیرهای مهم
PATH              # مسیر جستجوی دستورات
HOME              # دایرکتوری خانه کاربر
USER              # نام کاربر فعلی
SHELL             # shell فعلی
PS1               # prompt اصلی
PS2               # prompt ثانوی (ادامه خط)
LANG              # زبان سیستم
LC_ALL            # override تمام LC_* متغیرها
EDITOR            # ویرایشگر پیشفرض
HISTSIZE          # اندازه تاریخچه
TMPDIR            # دایرکتوری موقت
```

### ۳.۸ redirection و stream

```bash
# بازنویسی stdout
command > file                   # بازنویسی (overwrite)
command >> file                  # الحاق (append)
command 2> file                  # بازنویسی stderr
command 2>> file                 # الحاق stderr
command > file 2>&1              # stdout + stderr در فایل
command &> file                 # معادل بالا (Bash)
command 2>/dev/null              # حذف stderr
command > /dev/null 2>&1         # حذف هر دو خروجی

# بازنویسی ورودی
command < input.txt              # خواندن از فایل
command << EOF                   # Here Document
line 1
line 2
EOF

# بازنویسی stdin و stdout (TEE)
command | tee output.txt         # نمایش + ذخیره
command | tee -a output.txt      # نمایش + الحاق

# Blackhole
command > /dev/null 2>&1         # بیصدا (quiet)
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۴. دیسکها، پارتیشنبندی و فایلسیستمها

### ۴.۱ شناسایی دیسکها و پارتیشنها

```bash
# شناسایی دیسکها
lsblk                           # لیست بلوک دستگاهها (درختی)
lsblk -f                        # شامل فایلسیستم و UUID
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT,UUID  # ستونهای دلخواه
fdisk -l                        # اطلاعات پارتیشن (MBR + GPT)
blkid                           # UUID و TYPE فایلسیستمها
parted -l                       # اطلاعات پارتیشن (هم MBR و GPT)
cat /proc/partitions            # پارتیشنهای شناسایی شده توسط کرنل

# حافظه و IO
iostat                           # آمار IO دیسک
iostat -xz 1                    # هر ۱ ثانیه، فقط دیسکهای فعال
smartctl -a /dev/sda            # اطلاعات SMART
```

### ۴.۲ پارتیشنبندی با fdisk و parted

**fdisk (برای MBR و GPT):**

```bash
sudo fdisk /dev/sdb             # وارد شدن به حالت تعاملی

# دستورات fdisk:
# p   = نمایش پارتیشنها (print)
# n   = ایجاد پارتیشن جدید (new)
# d   = حذف پارتیشن (delete)
# t   = تغییر نوع پارتیشن (type)
# w   = نوشتن و خروج (write)
# q   = خروج بدون ذخیره (quit)
# m   = راهنما (help)

# مثال: ایجاد پارتیشن
sudo fdisk /dev/sdb
# p → n → p (primary) → 1 → Enter → +10G (اندازه)
# t → 83 (Linux)
# w

# بعد از پارتیشنبندی، kpartz را اجرا کنید:
sudo partprobe /dev/sdb          # اعمال تغییرات بدون ریاستارت
```

**parted (برای GPT و MBR):**

```bash
sudo parted /dev/sdb

# دستورات parted:
print                           # نمایش پارتیشنها
mklabel gpt                     # ایجاد لیبل GPT
mklabel msdos                   # ایجاد لیبل MBR
mkpart primary ext4 1MiB 10GiB  # ایجاد پارتیشن
rm 1                            # حذف پارتیشن ۱
resizepart 1 20GiB              # تغییر اندازه پارتیشن
rescue 1GiB 5GiB                # بازیابی پارتیشن گمشده
unit MiB                        # تغییر واحد به مگابایت
quit                            # خروج

# بدون حالت تعاملی
sudo parted /dev/sdb mklabel gpt
sudo parted /dev/sdb mkpart primary ext4 1MiB 100%
```

### ۴.۳ ایجاد فایلسیستم (mkfs)

```bash
# انواع فایلسیستمها
mkfs.ext4 /dev/sdb1             # ext4 (پیشفرض بسیاری از توزیعها)
mkfs.xfs /dev/sdb1              # XFS (پیشفرض RHEL/CentOS)
mkfs.vfat /dev/sdb1             # FAT32 (USB، ESP)
mkfs.ntfs /dev/sdb1             # NTFS (ویندوز)
mkfs.btrfs /dev/sdb1            # Btrfs (snapshot, compression)
mkfs.fat -F 32 /dev/sdb1        # FAT32 با explicit type

# گزینههای مهم
mkfs.ext4 -L "LABEL" /dev/sdb1  # تنظیم label
mkfs.ext4 -b 4096 /dev/sdb1     # اندازه block (۴K)
mkfs.ext4 -m 1 /dev/sdb1        # درصد رزرو root (پیشفرض 5%)
mkfs.ext4 -i 8192 /dev/sdb1     # bytes per inode

# block size و استفاده بهینه
# 4096 (4K) مناسب برای اکثر موارد
# 65536 مناسب برای فایلهای بزرگ (ویدیو، آرشیو)
```

**مقایسه فایلسیستمها:**

| ویژگی | ext4 | XFS | Btrfs | vfat |
|-------|------|-----|-------|------|
| حداکثر اندازه فایل | 16 TiB | 8 EiB | 16 EiB | 4 GiB |
| حداکثر حجم پارتیشن | 1 EiB | 8 EiB | 16 EiB | 32 GiB (FAT32) |
| Snapshot | ندارد | ندارد (تکمیلی) | بله | ندارد |
| Compression | ندارد | ندارد | بله (zstd, lzo) | ندارد |
| پیکربندی پیشفرض | Ubuntu, Debian | RHEL, CentOS, Fedora | چند-کاربردی | USB, ESP |
| Journaling | بله | بله | Copy-on-Write | ندارد |

### ۴.۴ مدیریت Swap

```bash
# ایجاد swap partition
sudo mkswap /dev/sdb2            # ایجاد swap
sudo swapon /dev/sdb2            # فعالسازی
sudo swapoff /dev/sdb2           # غیرفعالسازی
blkid /dev/sdb2                  # بررسی نوع

# ایجاد swap file (روش مدرن)
sudo fallocate -l 4G /swapfile   # اختصاص فضا (سریعتر)
# یا:
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
sudo chmod 600 /swapfile          # مجوز امنیتی
sudo mkswap /swapfile             # تنظیم swap
sudo swapon /swapfile             # فعالسازی

# تنظیم دائمی در fstab
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# اولویت swap (هرچه بالاتر، زودتر استفاده میشود)
# در fstab: /swapfile none swap sw,pri=1 0 0

# بررسی وضعیت swap
swapon --show
free -h
cat /proc/swaps
vmstat | head -5
```

> [!TIP]
> `chmod 600` برای فایل swap ضروری است. بدون آن، `swapon` خطا میدهد و فایل swap در صورت مجوزهای نادرست قابل استفاده نیست.

### ۴.۵ Mount و fstab

```bash
# Mount دستی
sudo mount /dev/sdb1 /mnt/data
sudo mount -t ext4 /dev/sdb1 /mnt/data
sudo mount -o ro /dev/sdb1 /mnt/data          # فقط خواندنی
sudo mount -o remount,rw /mnt/data             # remount با مجوز جدید
sudo umount /dev/sdb1                           # unmount
sudo umount -l /mnt/data                        # lazy unmount

# گزینههای mount رایج
# ro          = فقط خواندنی
# rw          = خواندن و نوشتن
# noexec      = غیرفعال کردن اجرای فایلها
# nosuid      = غیرفعال کردن SUID/SGID
# nodev       = غیرفعال کردن فایلهای دستگاه
# noatime     = بدون بروزرسانی access time (بهینهسازی IO)
# exec        = اجرای فایلها (پیشفرض)
# defaults    = rw, suid, dev, exec, auto, nouser, async

# fstab - فایل پیکربندی mount خودکار
# فرمت: <device> <mount_point> <fs_type> <options> <dump> <fsck>
cat /etc/fstab

# مثال fstab:
UUID=xxxx-xxxx  /boot/efi  vfat   defaults                    0 1
UUID=xxxx-xxxx  /           ext4   defaults                    0 1
/dev/sdb1       /data       xfs    defaults,noatime            0 2
/dev/sdb2       swap        swap   sw,pri=1                    0 0
tmpfs           /tmp        tmpfs  defaults,noexec,nosuid,size=2G 0 0

# شمارههای dump و fsck:
# dump: 0 = پشتیبانگیری نشود، 1 = پشتیبانگیری شود
# fsck: 0 = بررسی نشود، 1 = اول بررسی شود (root)، 2 = بعدی

# بررسی صحت fstab
sudo findmnt --verify
sudo mount -a                    # mount تمام ردیفهای fstab

# UUID و LABEL
blkid /dev/sdb1                 # مشاهده UUID
sudo tune2fs -l /dev/sdb1 | grep "Filesystem UUID"
sudo e2label /dev/sdb1 "MYDATA" # تنظیم label
```

### ۴.۶ LVM — Logical Volume Manager

**مفهوم LVM:**

```
Physical Disks → Physical Volumes (PV) → Volume Group (VG) → Logical Volumes (LV)
```

```bash
# --- ایجاد LVM ---

# ۱. ایجاد Physical Volume
sudo pvcreate /dev/sdb /dev/sdc
sudo pvs                         # لیست PV
sudo pvdisplay /dev/sdb          # جزئیات PV

# ۲. ایجاد Volume Group
sudo vgcreate myvg /dev/sdb /dev/sdc
sudo vgs                         # لیست VG
sudo vgdisplay myvg              # جزئیات VG

# ۳. ایجاد Logical Volume
sudo lvcreate -n mylv -L 10G myvg           # با اندازه
sudo lvcreate -n mylv -l 80%VG myvg         # با درصد
sudo lvs                         # لیست LV
sudo lvdisplay /dev/myvg/mylv    # جزئیات LV

# ۴. ایجاد فایلسیستم و mount
sudo mkfs.ext4 /dev/myvg/mylv
sudo mkdir /data
sudo mount /dev/myvg/mylv /data
echo '/dev/myvg/mylv /data ext4 defaults 0 2' | sudo tee -a /etc/fstab
```

```bash
# --- مدیریت LVM ---

# تغییر اندازه VG (اضافه کردن دیسک)
sudo vgextend myvg /dev/sdd
sudo vgs

# حذف PV از VG
sudo vgreduce myvg /dev/sdc
sudo pvremove /dev/sdc

# تغییر اندازه LV (افزایش)
sudo lvextend -L +5G /dev/myvg/mylv
sudo resize2fs /dev/myvg/mylv        # اعمال change (ext4)
sudo xfs_growfs /dev/myvg/mylv       # اعمال change (XFS)

# تغییر اندازه LV (کاهش — خطرناک!)
sudo umount /data
sudo e2fsck -f /dev/myvg/mylv        # بررسی فایلسیستم (ext4)
sudo resize2fs /dev/myvg/mylv 5G     # کوچکتر کردن fs
sudo lvreduce -L 5G /dev/myvg/mylv   # کوچکتر کردن LV
sudo mount /dev/myvg/mylv /data

# ایجاد Snapshot
sudo lvcreate -s -n mysnap -L 2G /dev/myvg/mylv
sudo mount -o ro /dev/myvg/mysnap /mnt/snapshot

# حذف
sudo umount /mnt/snapshot
sudo lvremove /dev/myvg/mysnap
sudo lvremove /dev/myvg/mylv
sudo vgremove myvg
sudo pvremove /dev/sdb
```

### ۴.۷ MD RAID (Software RAID)

```bash
# انواع RAID
# RAID 0: Striping (سرعت، بدون redundancy)
# RAID 1: Mirroring (تکرار)
# RAID 5: Parity (حداقل ۳ دیسک)
# RAID 6: Double Parity (حداقل ۴ دیسک)
# RAID 10: Mirroring + Striping (حداقل ۴ دیسک)

# ایجاد RAID 1
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

# ایجاد RAID 5
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd

# مشاهده وضعیت
cat /proc/mdstat
sudo mdadm --detail /dev/md0
sudo mdadm --examine /dev/sdb

# ذخیره پیکربندی
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm.conf
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf  # RHEL
sudo dracut -f

# جایگزینی دیسک خراب
sudo mdadm --manage /dev/md0 --fail /dev/sdb
sudo mdadm --manage /dev/md0 --remove /dev/sdb
sudo mdadm --manage /dev/md0 --add /dev/sde

# بازیابی
sudo mdadm --assemble /dev/md0 /dev/sdb /dev/sdc
```

### ۴.۸ فایلسیستمهای مجازی مهم

| سیستمفایل | نقطه اتصال | توضیح |
|------------|------------|-------|
| proc | `/proc` | اطلاعات پروسسها و کرنل |
| sysfs | `/sys` | اطلاعات دستگاهها و درایورها |
| tmpfs | `/tmp`, `/run`, `/dev/shm` | حافظه مجازی (RAM) |
| devtmpfs | `/dev` | فایلهای دستگاه |
| devpts | `/dev/pts` | ترمینالهای مجازی |

```bash
# مثال: ایجاد tmpfs
sudo mount -t tmpfs -o size=1G tmpfs /mnt/ramdisk
# یا در fstab:
tmpfs /mnt/ramdisk tmpfs defaults,size=1G 0 0
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۵. شلها، اسکریپتنویسی و پایگاه داده

### ۵.۱ مفاهیم Shell و انواع آن

| Shell | مسیر | توضیح |
|-------|------|-------|
| Bash | `/bin/bash` | پیشفرض اکثر توزیعها |
| Zsh | `/bin/zsh` | محبوبترین جایگزین Bash |
| Fish | `/usr/bin/fish` | کاربرپسند ولی غیرسازگار |
| Dash | `/bin/dash` | سبک، پیشفرض در Debian |
| sh | `/bin/sh` | POSIX shell (معمولاً symlink به Bash) |
| Tcsh | `/bin/tcsh` | C-like shell |

```bash
# مشاهده shell فعلی
echo $SHELL
cat /etc/shells                   # لیست shellهای مجاز
chsh -s /bin/zsh                  # تغییر shell پیشفرض

# اجرای اسکریپت با shell خاص
bash script.sh
zsh script.sh
sh script.sh                     # POSIX mode (بدون امکانات Bash)
#!/bin/bash                       # shebang در اسکریپت
#!/usr/bin/env bash               # env-based (قابل حملتر)
```

### ۵.۲ متغیرها و عملگرها در Bash

```bash
# --- انواع متغیر ---
VAR="value"                       # متغیر ساده
VAR=$(command)                    # متغیر از خروجی دستور
VAR=$((expression))               # محاسبه عددی
ARRAY=("a" "b" "c")              # آرایه
declare -i NUM=42                 # عدد صحیح
declare -r CONST="fixed"         # فقط خواندنی
declare -A MAP                   # آرایه assosiative

# --- عملگرها ---
# رشتهای
${#VAR}                           # طول رشته
${VAR:offset:length}              # substring
${VAR/pattern/replacement}        # جایگزینی الگو
${VAR,,}                          # تبدیل به کوچک
${VAR^^}                          # تبدیل به بزرگ

# عددی
$((a + b))                        # جمع
$((a - b))                        # تفریق
$((a * b))                        # ضرب
$((a / b))                        # تقسیم
$((a % b))                        # باقیمانده (Modulus)
$((a ** b))                       # توان

# بررسی وجود متغیر
${VAR:-default}                   # اگر VAR خالی باشد، default
${VAR:=default}                   # اگر خالی باشد، مقداردهی و بازگردان
${VAR:+alternate}                 # اگر پر باشد، alternate
${VAR:?error message}             # اگر خالی باشد، خطا

# آرایه
${ARRAY[0]}                       # عنصر اول
${ARRAY[@]}                       # تمام عناصر
${#ARRAY[@]}                      # تعداد عناصر
```

**عملگرهای مقایسهای در conditionals:**

| عملگر | نوع | توضیح |
|--------|-----|-------|
| `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge` | عددی | مساوی، نامساوی، کمتر، ... |
| `=`, `==`, `!=` | رشتهای | تطابق و عدم تطابق |
| `-z` | رشتهای | خالی بودن |
| `-n` | رشتهای | خالی نبودن |
| `-f` | فایل | فایل معمولی وجود دارد |
| `-d` | فایل | دایرکتوری وجود دارد |
| `-e` | فایل | وجود دارد |
| `-r` | فایل | قابل خواندن |
| `-w` | فایل | قابل نوشتن |
| `-x` | فایل | قابل اجرا |
| `-s` | فایل | اندازه بیشتر از صفر |
| `-L` | فایل | لینک نمادین |
| `-nt` | فایل | جدیدتر از |
| `-ot` | فایل | قدیمیتر از |
| `-b`, `-c`, `-p` | فایل | block، character، pipe |

### ۵.۳ ساختارهای کنترلی

```bash
# --- if/elif/else ---
if [ "$VAR" = "value" ]; then
    echo "match"
elif [ "$VAR" = "other" ]; then
    echo "other"
else
    echo "default"
fi

# [[ ]] بهتر از [ ] است (پشتیبانی از regex و pattern matching)
if [[ "$VAR" =~ ^[0-9]+$ ]]; then
    echo "عدد است"
fi

if [[ "$FILE" == *.log ]]; then
    echo "فایل لاگ"
fi

# --- case ---
case "$VAR" in
    start)
        echo "شروع"
        ;;
    stop|quit)
        echo "توقف"
        ;;
    *)
        echo "نامشخص"
        ;;
esac

# --- for ---
for i in 1 2 3 4 5; do
    echo "$i"
done

for i in $(seq 1 10); do
    echo "$i"
done

for ((i=0; i<10; i++)); do
    echo "$i"
done

for FILE in *.txt; do
    echo "Processing $FILE"
done

# --- while ---
while [ $COUNT -lt 10 ]; do
    echo $COUNT
    ((COUNT++))
done

# خواندن فایل خط به خط
while IFS= read -r LINE; do
    echo "$LINE"
done < input.txt

# --- until ---
until [ $COUNT -ge 10 ]; do
    echo $COUNT
    ((COUNT++))
done
```

### ۵.۴ توابع

```bash
# تعریف تابع
function greet() {
    local name=$1                  # آرگومان اول
    echo "Hello, $name!"
}

# یا (معادل)
greet() {
    echo "Hello, $1!"
}

# فراخوانی
greet "Ali"
RESULT=$(greet "Ali")             # ذخیره خروجی

# بازگشت مقدار
is_root() {
    if [ "$(id -u)" -eq 0 ]; then
        return 0                   # موفقیت (true)
    else
        return 1                   # شکست (false)
    fi
}

if is_root; then
    echo "Running as root"
fi

# آرگومانهای ویژه
# $0    = نام اسکریپت
# $1-$9 = آرگومانها
# $#    = تعداد آرگومانها
# $@    = تمام آرگومانها (به صورت جداگانه)
# $*    = تمام آرگومانها (به صورت یک رشته)
# $?    = کد خروج آخرین دستور
# $$    = PID اسکریپت
# $!    = PID آخرین پروسس پسزمینه
# $_    = آخرین آرگومان
```

### ۵.۵ trap و مدیریت خطا

```bash
# trap: گرفتن سیگنالها
cleanup() {
    echo "Cleaning up..."
    rm -f "$TEMP_FILE"
}
trap cleanup EXIT                 # اجرا هنگام خروج (هر دلیلی)
trap "echo 'Interrupted!'" INT    # گرفتن Ctrl+C
trap '' INT                       # نادیده گرفتن SIGINT
trap - INT                        # بازگشت به رفتار پیشفرض

# set برای مدیریت خطا
set -e                             # خروج در صورت خطا
set -u                             # خطا اگر متغیر تعریف نشده باشد
set -o pipefail                   # خطا اگر لوله شکست بخورد
set -x                             # نمایش هر دستور قبل از اجرا (debug)

# خروجی اسکریپت
exit 0                             # موفقیت
exit 1                             # شکست
```

### ۵.۶ sed — ویرایشگر جریان

```bash
# جایگزینی
sed 's/old/new/' <file>            # اولین تطابق در هر خط
sed 's/old/new/g' <file>           # تمام تطابقها
sed 's/old/new/gi' <file>          # بدون حساسیت به حروف
sed -i 's/old/new/g' <file>        # در فایل (in-place)

# حذف خطوط
sed '/pattern/d' <file>            # حذف خطوط حاوی الگو
sed '3d' <file>                    # حذف خط ۳
sed '2,5d' <file>                  # حذف خطوط ۲ تا ۵
sed '1d' <file>                    # حذف خط اول

# چاپ
sed -n '/error/p' <file>           # فقط خطوط حاوی error
sed -n '10,20p' <file>             # خطوط ۱۰ تا ۲۰
sed -n '5p' <file>                 # فقط خط ۵

# اضافه کردن و جایگزینی
sed '/pattern/i\text to insert' <file>    # قبل از الگو
sed '/pattern/a\text to append' <file>    # بعد از الگو
sed '3c\new line' <file>                   # جایگزینی خط ۳

# مثالهای عملی
sed 's/[[:space:]]*$//' <file>             # حذف فاصلههای انتهایی
sed '/^$/d' <file>                          # حذف خطوط خالی
sed '/^#/d' <file>                          # حذف کامنتها
sed -n '1!G;h;$p' <file>                    # معکوس کردن فایل
```

### ۵.۷ پایگاه داده SQL (SQLite)

```bash
# SQLite3
sqlite3 database.db              # ایجاد/باز کردن دیتابیس

# دستورات SQL:
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE
);

INSERT INTO users (name, email) VALUES ('Ali', 'ali@example.com');
INSERT INTO users (name, email) VALUES ('Sara', 'sara@example.com');

SELECT * FROM users WHERE name = 'Ali';
SELECT name, email FROM users ORDER BY name;
SELECT COUNT(*) FROM users;
SELECT * FROM users WHERE email LIKE '%@example.com';

UPDATE users SET email = 'new@example.com' WHERE name = 'Ali';
DELETE FROM users WHERE name = 'Sara';

# دستورات شل SQLite
.mode column                       # فرمت خروجی
.headers on                        # نمایش هدرها
.quit                              # خروج
.schema users                     # ساختار جدول
.tables                           # لیست جداول
.backup mydb.db /tmp/backup.db    # پشتیبانگیری
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۶. رابطهای کاربری و دسکتاپ

### ۶.۱ X11 و Wayland

| ویژگی | X11 (X.Org) | Wayland |
|-------|------------|---------|
| معماری | Client-Server | Direct rendering |
| امنیت | پایین (هر کلاینت همهچیز را میبیند) | بالا (هر کلاینت فقط خودش) |
| عملکرد | خوب | بهتر (کمتر از CPU) |
| پشتیبانی | گسترده | در حال رشد |
| پروتکل | TCP + Unix Socket | فقط Unix Socket |
| توزیع پیشفرض | Xubuntu, older | Fedora, Ubuntu 22.04+ |

```bash
# مشاهده سیستم display
echo $DISPLAY                      # مثال: :0
echo $WAYLAND_DISPLAY              # مثال: wayland-0
echo $XDG_SESSION_TYPE             # x11 یا wayland

# اجرای برنامه X11 روی remote
ssh -X user@server                 # Forward X11
ssh -Y user@server                 # Trusted Forwarding
export DISPLAY=:0                  # تنظیم نمایشگر

# xauth
xauth list                         # لیست مجوزهای X11
xauth merge ~/.Xauthority          # ادغام مجوزها
```

### ۶.۲ Display Managers و متغیرهای محیطی

```bash
# لیست sessionها
ls /usr/share/xsessions/           # X11 sessionها
ls /usr/share/wayland-sessions/    # Wayland sessionها

# تغییر session
sudo systemctl set-default graphical.target   # فعالسازی GUI

# متغیرهای مهم XDG
echo $XDG_CONFIG_HOME              # معمولاً ~/.config
echo $XDG_DATA_HOME                # معمولاً ~/.local/share
echo $XDG_CACHE_HOME               # معمولاً ~/.cache
echo $XDG_RUNTIME_DIR              # معمولاً /run/user/$(id -u)

# Desktop Environment detection
echo $XDG_CURRENT_DESKTOP          # GNOME, KDE, XFCE, etc.
echo $DESKTOP_SESSION              # gnome, kde, etc.
```

### ۶.۳ tmux و screen (Multi-plexers)

```bash
# tmux
tmux new -s mysession              # ایجاد session جدید
tmux attach -t mysession           # اتصال به session
tmux ls                           # لیست sessions
tmux kill-session -t mysession     # حذف session

# کلیدهای میانبر tmux (پیشفرض: Ctrl+b)
# Ctrl+b d    = detach
# Ctrl+b c    = پنجره جدید
# Ctrl+b n    = پنجره بعدی
# Ctrl+b p    = پنجره قبلی
# Ctrl+b %    = تقسیم عمودی
# Ctrl+b "    = تقسیم افقی
# Ctrl+b x    = بستن پنجره

# screen
screen -S mysession                # ایجاد
screen -r mysession                # اتصال مجدد
screen -ls                        # لیست sessions
screen -X -S mysession quit       # حذف
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

# آزمون ۱۰۲

---

## ۷. وظایف مدیریتی و دسترسیها

### ۷.۱ مدیریت کاربران و گروهها

```bash
# --- مدیریت کاربران ---
useradd -m -s /bin/bash -G wheel john       # ایجاد کاربر (RHEL)
useradd -m -s /bin/bash -G sudo john        # ایجاد کاربر (Debian)
useradd -m -d /home/john -s /bin/bash -G docker john  # با دایرکتوری خانه
usermod -aG sudo john                        # اضافه کردن به گروه (-a = append)
usermod -L john                               # قفل کردن حساب
usermod -U john                               # باز کردن قفل
usermod -s /sbin/nologin john                 # غیرفعال کردن shell
usermod -e 2025-12-31 john                    # تاریخ انقضا
userdel john                                  # حذف کاربر
userdel -r john                               # حذف با دایرکتوری خانه

# رمز عبور
passwd john                                  # تنظیم رمز برای john
passwd -l john                               # قفل (معادل usermod -L)
passwd -u john                               # باز کردن قفل
passwd -e john                               # اجبار تغییر رمز در ورود بعدی
chage -l john                                # اطلاعات رمز
chage -M 90 john                             # حداکثر عمر رمز (۹۰ روز)
chage -m 7 john                              # حداقل عمر رمز (۷ روز)
chage -E 2025-12-31 john                     # تاریخ انقضا
chage -d 0 john                              # اجبار تغییر رمز

# اطلاعات کاربر
id john                                      # UID, GID, گروهها
id -u john                                   # فقط UID
id -g john                                   # فقط GID اصلی
id -G john                                   # تمام GIDها
whoami                                        # نام کاربر فعلی
who                                           # کاربران آنلاین
w                                             # کاربران + فعالیت
last                                          # تاریخچه ورود
lastlog                                       # آخرین ورود تمام کاربران
```

**فایلهای مهم:**

| فایل | توضیح |
|------|-------|
| `/etc/passwd` | اطلاعات کاربران (نام، UID، GID، home، shell) |
| `/etc/shadow` | رمزهای عبور هششده و تنظیمات رمز |
| `/etc/group` | اطلاعات گروهها |
| `/etc/gshadow` | رمزهای گروهها |
| `/etc/skel` | فایلهای قالب برای دایرکتوری خانه جدید |
| `/etc/login.defs` | تنظیمات پیشفرض login |
| `/etc/default/useradd` | تنظیمات پیشفرض useradd |

```bash
# فرمت /etc/passwd:
# username:password:UID:GID:GECOS:home:shell
cat /etc/passwd | head -5

# فرمت /etc/shadow:
# username:encrypted_password:last_change:min:max:warn:inactive:expire:reserved
cat /etc/shadow | head -5

# فرمت /etc/group:
# groupname:password:GID:members
cat /etc/group | head -5
```

### ۷.۲ گروهها

```bash
# ایجاد گروه
groupadd developers
groupadd -g 1500 developers       # با GID خاص

# حذف گروه
groupdel developers

# تغییر نام گروه
groupmod -n newname oldname

# اضافه کردن کاربر به گروه
usermod -aG developers john       # -a = append (بدون -a، حذف از گروههای قبلی)
gpasswd -a john developers        # معادل
gpasswd -d john developers        # حذف کاربر از گروه

# مشاهده گروهها
groups john                       # گروههای john
groups                            # گروههای کاربر فعلی
getent group developers           # اطلاعات گروه
```

### ۷.۳ مجوزهای فایل (Permissions)

**ساختار مجوزها:**

```
- rwx r-x r-x . user group other
│ │   │   │   │
│ │   │   │   └── SELinux/ACL
│ │   │   └── Other permissions
│ │   └── Group permissions
│ └── User (owner) permissions
└── File type (-, d, l, c, b, p, s)
```

```bash
# خواندن مجوزها
ls -la /etc/passwd
# -rw-r--r-- 1 root root 2345 ... /etc/passwd
# │││ │││ │││
# │││ │││ └┘└── other: r-- (4)
# │││ │┘└───── group: r-- (4)
# │┘└───────── user: rw- (6)
# └──────────── file type: - (regular file)

# chmod — تغییر مجوزها
chmod 755 script.sh              # rwxr-xr-x (numerical)
chmod 644 config.txt             # rw-r--r--
chmod u+x script.sh              # فقط اجرا برای owner
chmod g+w file.txt               # نوشتن برای group
chmod o-r file.txt               # حذف خواندن برای other
chmod a+r file.txt               # خواندن برای همه
chmod -R 755 /var/www/html       # بازگشتی

# symbolic mode
chmod u=rwx,g=rx,o=rx script.sh  # معادل 755
chmod go-w file.txt              # حذف نوشتن برای group و other
chmod u+s program                # فعالسازی SUID
chmod g+s dir                    # فعالسازی SGID
chmod +t /tmp                    # فعالسازی Sticky Bit

# chown — تغییر مالکیت
chown john file.txt              # تغییر مالک
chown john:developers file.txt   # تغییر مالک و گروه
chown -R john:www-data /var/www  # بازگشتی

# chgrp — تغییر گروه
chgrp developers file.txt
```

**Special Permissions:**

| Permission | عدد | توضیح |
|------------|-----|-------|
| SUID (Set User ID) | 4000 | اجرای فایل با مجوز صاحب (مالک) |
| SGID (Set Group ID) | 2000 | اجرای فایل با مجوز گروه / ایجاد فایل با GID دایرکتوری |
| Sticky Bit | 1000 | فقط مالک میتواند فایل را حذف کند |

```bash
# SUID: مثالهای مهم
ls -la /usr/bin/passwd
# -rwsr-xr-x 1 root root ... /usr/bin/passwd
# 's' به جای 'x' = SUID فعال
# passwd میتواند /etc/shadow را ویرایش کند (Owner: root)

# SGID: در دایرکتوری
chmod g+s /shared/
# فایلهای جدید در /shared/ گروه shared خواهند داشت
# (به جای گروه اصلی کاربر)

# Sticky Bit: مثال /tmp
ls -ld /tmp
# drwxrwxrwt 1 root root ... /tmp
# 't' به جای 'x' = Sticky Bit فعال
# هر کسی میتواند فایل ایجاد کند ولی فقط مالک میتواند حذف کند

# یافتن فایلها با SUID/SGID
find / -perm -4000 -type f 2>/dev/null   # SUID
find / -perm -2000 -type f 2>/dev/null   # SGID
find / -perm -1000 -type d 2>/dev/null   # Sticky Bit

# تغییر با number
chmod 4755 script.sh            # SUID + rwxr-xr-x
chmod 2755 dir/                 # SGID + rwxr-xr-x
chmod 1755 dir/                 # Sticky + rwxr-xr-x
chmod 0755 script.sh            # بدون special permissions
```

> [!WARNING]
> فایلهای SUID روی فایلهای قابل اجرا اعمال میشوند. اگر روی فایل غیرقابل اجرا `chmod u+s` بزنید، حرف `S` (بزرگ) نمایش داده میشود که نشاندهنده مشکل در مجوزهاست.

### ۷.۴ ACL — Access Control List

ACL امکان تنظیم مجوزهای دقیقتر از مدل سنتی user/group/other را فراهم میکند:

```bash
# مشاهده ACL
getfacl /data/file.txt

# اضافه کردن ACL
setfacl -m u:john:rwx /data/file.txt     # دسترسی rwx برای john
setfacl -m g:developers:rx /data/file.txt # دسترسی rx برای گروه
setfacl -m o::--- /data/file.txt          # حذف دسترسی other

# ACL پیشفرض برای دایرکتوری
setfacl -d -m g:developers:rx /data/

# حذف ACL
setfacl -x u:john /data/file.txt          # حذف ACL کاربر
setfacl -b /data/file.txt                 # حذف تمام ACLها

# خروجی getfacl
# # file: data/file.txt
# # owner: root
# # group: root
# user::rwx
# user:john:rwx
# group::r-x
# group:developers:r-x
# mask::rwx
# other::---

# نکته: ls -la نمایش '+' برای فایلهای با ACL:
# -rw-rwx---+ 1 root root ... /data/file.txt
```

> [!TIP]
> در آزمون LPIC-1، تفاوت بین `chmod` سنتی و `setfacl` بسیار مهم است. ACL امکان تنظیم دسترسی برای کاربران و گروههای خاص خارج از مدل سنتی را فراهم میکند.

### ۷.۵ Quotas — سهمیهبندی دیسک

```bash
# فعالسازی Quotas
# ۱. اضافه کردن usrquota و grpquota به fstab
# /dev/sdb1 /data ext4 defaults,usrquota,grpquota 0 2

# ۲. remount
sudo mount -o remount /data

# ۳. ایجاد فایل aquota
sudo quotacheck -cugm /data       # c=create, u=user, g=group, m=remount
# این دستور فایل aquota.user و aquota.group را ایجاد میکند

# ۴. فعالسازی
sudo quotaon /data
sudo quotaon -a                   # فعالسازی تمام quotas

# تنظیم Quota برای کاربر
sudo edquota -u john
# یا مستقیم:
sudo setquota -u john 1G 2G 0 0 /data
# soft limit = 1G (هشدار)
# hard limit = 2G (قاطع)

# تنظیم Quota برای گروه
sudo edquota -g developers
sudo setquota -g developers 5G 10G 0 0 /data

# مشاهده Quota
quota -u john                     # quota کاربر
quota -g developers               # quota گروه
repquota /data                    # گزارش کامل
sudo repquota -a                  # تمام پارتیشنها

# غیرفعالسازی
sudo quotaoff /data
sudo quotaoff -a
```

### ۷.۶ sudo — اجرای دستورات با دسترسی root

```bash
# ویرایش فایل sudoers (فقط با visudo!)
sudo visudo                       # ویرایش امن با syntax check
sudo visudo -f /etc/sudoers.d/custom  # فایل جداگانه

# فرمت sudoers:
# user    ALL=(ALL)       ALL                    # تمام دستورات
# user    ALL=(ALL)       NOPASSWD: ALL          # بدون رمز
# %wheel  ALL=(ALL)       ALL                    # تمام اعضای wheel
# user    ALL=(root)      /usr/bin/systemctl restart nginx  # فقط دستور خاص
# user    ALL=(ALL)       NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade

# نکات مهم visudo:
# Defaults    !visiblepw              # مجبور کردن به وارد کردن رمز
# Defaults    env_reset               # ریست متغیرهای محیطی
# Defaults    mail_badpass            # ارسال ایمیل در صورت رمز اشتباه
# Defaults    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin"

# مشاهده sudoers
sudo -l                           # دسترسیهای فعلی
sudo -V                           # تنظیمات sudo
```

### ۷.۷ umask و ساختار مجوزهای پیشفرض

```bash
# umask: مقدار از 666 (فایل) یا 777 (دایرکتوری) کم میشود
umask                             # نمایش umask فعلی (معمولاً 0022)
umask 027                         # تغییر umask

# محاسبه مجوز نهایی:
# فایل: 666 - 022 = 644 (rw-r--r--)
# دایرکتوری: 777 - 022 = 755 (rwxr-xr-x)

# umask 027:
# فایل: 666 - 027 = 640 (rw-r-----)
# دایرکتوری: 777 - 027 = 750 (rwxr-x---)

# تنظیم دائمی umask
# در /etc/profile یا ~/.bashrc:
umask 027
```

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۸. سرویسهای ضروری سیستم

### ۸.۱ Systemd — مدیریت سرویسها

Systemd جایگزین SysVinit شده و مدیریت تمام سرویسهای سیستم را بر عهده دارد.

```bash
# --- مدیریت سرویسها ---
sudo systemctl start nginx        # شروع سرویس
sudo systemctl stop nginx         # توقف سرویس
sudo systemctl restart nginx      # ریاستارت
sudo systemctl reload nginx       # ریلود (بدون قطع ارتباط)
sudo systemctl status nginx       # وضعیت
sudo systemctl enable nginx       # فعالسازی در بوت
sudo systemctl disable nginx      # غیرفعالسازی در بوت
sudo systemctl is-active nginx    # آیا فعال است؟
sudo systemctl is-enabled nginx   # آیا در بوت فعال است？
sudo systemctl is-failed nginx    # آیا شکست خورده؟

# --- لیست سرویسها ---
systemctl list-units --type=service                # سرویسهای فعال
systemctl list-units --type=service --all           # تمام سرویسها
systemctl list-unit-files --type=service            # وضعیت enable/disable
systemctl list-dependencies nginx                   # وابستگیها
systemctl list-timers                               # تایمرهای فعال

# --- Systemd Targets ---
systemctl get-default                 # target پیشفرض
systemctl set-default multi-user.target  # تغییر target
systemctl isolate rescue.target       # رفتن به target موقت
systemctl list-units --type=target     # لیست targets
```

**ایجاد Service فایل سفارشی:**

```bash
# فایل service در /etc/systemd/system/myservice.service
sudo tee /etc/systemd/system/myservice.service <<EOF
[Unit]
Description=My Custom Service
After=network.target
Wants=network-online.target

[Service]
Type=simple
User=nobody
Group=nogroup
ExecStart=/usr/local/bin/myapp -c /etc/myapp/config.conf
ExecStop=/bin/kill -TERM $MAINPID
Restart=on-failure
RestartSec=5
StandardOutput=journal
StandardError=journal
WorkingDirectory=/opt/myapp

[Install]
WantedBy=multi-user.target
EOF

# اعمال تغییرات
sudo systemctl daemon-reload
sudo systemctl start myservice
sudo systemctl enable myservice
sudo systemctl status myservice
```

**انواع Service Types:**

| Type | توضیح |
|------|-------|
| `simple` | پروسس اصلی سرویس (پیشفرض) |
| `forking` | پروسس fork شده و اصلی خاتمه مییابد |
| `oneshot` | اجرا و خاتمه (مثل اسکریپت بوت) |
| `notify` | مثل simple ولی با اطلاعرسانی آمادگی |
| `dbus` | مثل simple ولی با دریافت D-Bus name |
| `idle` | مثل simple ولی بعد از اتمام صف اجرا میشود |

### ۸.۲ Journald و Journalctl — مدیریت لاگ

```bash
# مشاهده لاگها
journalctl                             # تمام لاگها
journalctl -u nginx                    # لاگ سرویس خاص
journalctl -u nginx -f                 # زنده (follow)
journalctl -u nginx --since "1 hour ago"  # از ۱ ساعت پیش
journalctl -u nginx --since "2025-01-01" --until "2025-01-32"  # بازه زمانی

# فیلتر بر اساس سطح
journalctl -p err                      # فقط خطاها
journalctl -p warning                  # هشدار و بالاتر
journalctl -p crit                     # بحرانی و بالاتر
journalctl -p 0                        # emergency (عددی)

# فیلتر بر اساس پروسس
journalctl _PID=1234                   # پروسس خاص
journalctl _UID=1000                   # کاربر خاص
journalctl _COMM=sshd                  # نام دستور
journalctl _SYSTEMD_UNIT=sshd.service  # معادل -u sshd

# نمایش
journalctl -b                          # فقط بوت فعلی
journalctl -b -1                       # بوت قبلی
journalctl --list-boots                 # لیست بوتها
journalctl -n 50                       # ۵۰ خط آخر
journalctl --no-pager                  # بدون pager
journalctl -o json-pretty              # فرمت JSON

# مدیریت فضای دیسک
journalctl --disk-usage                 # استفاده از دیسک
sudo journalctl --vacuum-size=500M     # محدود کردن به ۵۰۰ مگابایت
sudo journalctl --vacuum-time=30d      # حذف لاگهای قدیمیتر از ۳۰ روز
```

**پیکربندی Journald:**

```bash
# فایل پیکربندی: /etc/systemd/journald.conf
# [Journal]
# SystemMaxUse=500M                  # حداکثر استفاده
# SystemMaxFileSize=50M              # حداکثر اندازه هر فایل
# MaxRetentionSec=30day              # حداکثر نگهداری
# Storage=persistent                 # ذخیره دائمی
# Compress=yes                       # فشردهسازی
# ForwardToSyslog=yes                # ارسال به rsyslog

# اعمال تغییرات
sudo systemctl restart systemd-journald
```

### ۸.۳ Rsyslog — لاگ سیستم کلاسیک

```bash
# فایلهای مهم
/var/log/syslog                     # لاگ عمومی سیستم (Debian/Ubuntu)
/var/log/messages                   # لاگ عمومی سیستم (RHEL/CentOS)
/var/log/auth.log                   # لاگ احراز هویت (Debian/Ubuntu)
/var/log/secure                     # لاگ احراز هویت (RHEL/CentOS)
/var/log/kern.log                   # لاگ کرنل
/var/log/dmesg                      # پیامهای بوت کرنل
/var/log/boot.log                   # لاگ بوت
/var/log/cron.log                   # لاگ cron
/var/log/lastlog                    # آخرین ورود کاربران
/var/log/wtmp                       # تاریخچه ورود/خروج
/var/log/btmp                       # تلاشهای ناموفق ورود

# rsyslog
cat /etc/rsyslog.conf               # پیکربندی اصلی
cat /etc/rsyslog.d/*.conf           # پیکربندی اضافی

# فرمت rsyslog:
# facility.priority    /path/to/logfile
#
# facility: auth, authpriv, cron, daemon, kern, mail, user, local0-7
# priority: emerg, alert, crit, err, warning, notice, info, debug
#
# مثالها:
# *.info;mail.none;authpriv.none;cron.none    /var/log/messages
# authpriv.*                                    /var/log/secure
# mail.*                                        /var/log/maillog
# cron.*                                        /var/log/cron
# *.emerg                                       :omusrmsg:*

# مثال: ارسال لاگ به سرور remote
# در سرور receiver (rsyslog.conf):
# module(load="imudp")
# input(type="imudp" port="514")
# $AllowedSender UDP, 192.168.1.0/24

# در کلاینت:
# *.* @@192.168.1.100:514       # TCP
# *.* @192.168.1.100:514        # UDP

sudo systemctl restart rsyslog
```

### ۸.۴ Cron — زمانبندی وظایف

```bash
# انواع crontab
crontab -e                         # ویرایش crontab کاربر فعلی
crontab -l                         # نمایش crontab کاربر
crontab -u john -e                 # ویرایش crontab کاربر دیگر (root)
sudo crontab -e -u www-data       # ویرایش crontab www-data

# فرمت crontab:
# minute  hour  day-of-month  month  day-of-week  command
# 0-59    0-23  1-31          1-12   0-7 (0=7)    /path/to/command

# مثالها:
# هر ۵ دقیقه
*/5 * * * * /usr/local/bin/backup.sh

# هر روز ساعت ۲:۳۰ صبح
30 2 * * * /usr/local/bin/cleanup.sh

# هر دوشنبه ساعت ۸ صبح
0 8 * * 1 /usr/local/bin/report.sh

# هر اول هر ماه ساعت ۱۲ شب
0 0 1 * * /usr/local/bin/monthly.sh

# هر ۱۵ دقیقه بین ساعت ۹ تا ۱۷ روزهای کاری
*/15 9-17 * * 1-5 /usr/local/bin/check.sh

# اجرای دستور با خروجی به فایل
0 2 * * * /usr/local/bin/script.sh >> /var/log/script.log 2>&1

# فایلهای cron سیستمی
/etc/crontab                       # crontab سیستم (شامل فیلد user)
/etc/cron.d/                       # فایلهای cron اضافی
/etc/cron.daily/                   # اسکریپتهای روزانه
/etc/cron.hourly/                  # اسکریپتهای ساعتی
/etc/cron.weekly/                  # اسکریپتهای هفتگی
/etc/cron.monthly/                 # اسکریپتهای ماهانه
/var/spool/cron/                   # crontab کاربران

# امنیت cron
/etc/cron.allow                    # لیست مجازان
/etc/cron.deny                     # لیست ممنوعان
# اگر cron.allow وجود داشته باشد، فقط کاربران آن لیست مجازند
```

**systemd Timers (جایگزین مدرن Cron):**

```bash
# مشاهده تایمرها
systemctl list-timers --all

# مثال: ایجاد systemd timer
# فایل timer: /etc/systemd/system/backup.timer
[Unit]
Description=Daily Backup Timer

[Timer]
OnCalendar=*-*-* 02:30:00
Persistent=true

[Install]
WantedBy=timers.target

# فایل service: /etc/systemd/system/backup.service
[Unit]
Description=Daily Backup

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh

# فعالسازی
sudo systemctl daemon-reload
sudo systemctl enable --now backup.timer
sudo systemctl list-timers | grep backup

# time expressions رایج:
# OnCalendar=daily
# OnCalendar=Mon *-*-* 09:00:00
# OnCalendar=*-*-01 00:00:00
# OnBootSec=5min
# OnUnitActiveSec=1h
```

### ۸.۵ مدیریت زمان (Time)

```bash
# مشاهده زمان فعلی
date                               # زمان فعلی
timedatectl                        # وضعیت کامل زمان
timedatectl list-timezones         # لیست مناطق زمانی

# تنظیم زمان
sudo timedatectl set-timezone Asia/Tehran
sudo timedatectl set-time "2025-06-15 14:30:00"
sudo timedatectl set-ntp true      # فعالسازی NTP

# NTP (Network Time Protocol)
# chrony (جایگزین ntpd)
sudo systemctl status chronyd      # وضعیت chrony
chronyc tracking                   # وضعیت NTP
chronyc sources -v                 # منابع NTP
sudo chronyc makestep              # همگامسازی فوری

# ntp (کلاسیک)
sudo systemctl status ntpd
ntpq -p                            # منابع NTP
sudo ntpdate pool.ntp.org          # همگامسازی فوری

# hwclock: ساعت سختافزاری (BIOS)
sudo hwclock --show                # نمایش ساعت سختافزاری
sudo hwclock --systohc             # تنظیم سختافزاری از سیستم
sudo hwclock --hctosys             # تنظیم سیستم از سختافزاری

# timedatectl show
timedatectl show                   # نمایش تمام تنظیمات
```

**مقایسه NTP Services:**

| ویژگی | ntpd | chronyd | systemd-timesyncd |
|-------|------|---------|-------------------|
| دقت | بالا | بالا | متوسط |
| استفاده منابع | زیاد | کم | بسیار کم |
| مناسب برای | سرورهای ثابت | سرورهای مجازی و ابری | دسکتاپ |
| پیکربندی | `/etc/ntp.conf` | `/etc/chrony.conf` | `/etc/systemd/timesyncd.conf` |
| ابزار | `ntpq` | `chronyc` | `timedatectl` |

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۹. مبانی شبکه

### ۹.۱ مفاهیم TCP/IP

**لایههای TCP/IP:**

| لایه | پروتکلها | دستگاه |
|------|----------|--------|
| Application | HTTP, HTTPS, DNS, SSH, FTP, SMTP, DHCP, SNMP | — |
| Transport | TCP (تمامپیوند، قابل اتکا)، UDP (بدون اتصال) | — |
| Internet | IP (IPv4, IPv6), ICMP, ARP | Router |
| Network Access | Ethernet, Wi-Fi, PPP | Switch |

**آدرسدهی IPv4:**

```
IP Address:   192.168.1.100
Subnet Mask:  255.255.255.0    (/24)
Network:      192.168.1.0
Broadcast:    192.168.1.255
Host Range:   192.168.1.1 - 192.168.1.254
```

```bash
# مشاهده آدرسهای IP
ip addr show                       # تمام interfaceها
ip -4 addr show                    # فقط IPv4
ip -6 addr show                    # فقط IPv6
hostname -I                        # آدرس IP سیستم
ifconfig                           # (منسوخ) اطلاعات interface

# پیکربندی IP با ip
sudo ip addr add 192.168.1.100/24 dev eth0    # اضافه کردن IP
sudo ip addr del 192.168.1.100/24 dev eth0    # حذف IP
sudo ip link set eth0 up                       # فعالسازی interface
sudo ip link set eth0 down                     # غیرفعالسازی
sudo ip route add default via 192.168.1.1      # اضافه کردن gateway

# پیکربندی با nmcli (NetworkManager)
nmcli con show                                 # لیست اتصالات
nmcli con mod "eth0" ipv4.addresses "192.168.1.100/24"
nmcli con mod "eth0" ipv4.gateway "192.168.1.1"
nmcli con mod "eth0" ipv4.dns "8.8.8.8"
nmcli con mod "eth0" ipv4.method manual
nmcli con up "eth0"

# فایلهای پیکربندی شبکه
# Debian/Ubuntu:
/etc/network/interfaces            # پیکربندی کلاسیک
/etc/netplan/*.yaml                 # Netplan (پیشفرض)

# RHEL/CentOS:
/etc/sysconfig/network-scripts/ifcfg-eth0
/etc/NetworkManager/system-connections/

# DNS
/etc/resolv.conf                   # سرورهای DNS
# nameserver 8.8.8.8
# nameserver 8.8.4.4

# hosts
/etc/hosts                         # نقشه IP به hostname
# 192.168.1.10   server1.localdomain   server1
```

### ۹.۲ ابزارهای عیبیابی شبکه

```bash
# ping و test
ping -c 4 google.com              # ارسال ۴ بسته ICMP
ping6 ::1                         # IPv6 loopback
ping -I eth0 192.168.1.1          # از interface خاص

# traceroute
traceroute google.com             # مسیر بستهها
traceroute -n google.com          # بدون resolve DNS
mtr google.com                    # ترکیب ping + traceroute

# nslookup و dig
nslookup google.com               # جستجوی DNS ساده
nslookup -type=MX google.com      # جستجوی MX Record
dig google.com                    # DNS با جزئیات
dig google.com +short             # خروجی خلاصه
dig @8.8.8.8 google.com           # استفاده از DNS خاص
dig -x 8.8.8.8                    # Reverse DNS
dig google.com ANY                # تمام recordها

# curl و wget
curl -I https://example.com       # فقط هدرها
curl -o file.zip https://example.com/file.zip  # دانلود
curl -L https://example.com       # دنبال کردن redirects
curl -v https://example.com       # verbose
wget https://example.com/file.zip # دانلود
wget -c https://example.com/file.zip  # ادامه دانلود

# ss و netstat
ss -tlnp                          # پورتهای در حال گوش دادن (TCP)
ss -ulnp                          # پورتهای در حال گوش دادن (UDP)
ss -tunap                         # تمام اتصالات
ss -s                             # خلاصه آمار
ss state established              # اتصالات برقرار
netstat -tlnp                     # (منسوخ) معادل ss
netstat -anp                      # تمام اتصالات

# نکته: ss سریعتر از netstat است و جایگزین آن شده
```

### ۹.۳ IP Tables و nftables

```bash
# --- iptables (کلاسیک) ---
sudo iptables -L -n -v             # لیست قوانین
sudo iptables -L -n --line-numbers # با شماره خط
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT   # اضافه کردن قانون
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT   # پذیرش از subnet
sudo iptables -A INPUT -j DROP                        # رد تمام (EOF)
sudo iptables -D INPUT 3                              # حذف قانون شماره 3
sudo iptables -F                                      # پاک کردن تمام قوانین
sudo iptables -P INPUT DROP                           # سیاست پیشفرض

# ذخیره و بازیابی
sudo iptables-save > /etc/iptables.rules
sudo iptables-restore < /etc/iptables.rules

# --- nftables (مدرن) ---
sudo nft list ruleset               # لیست تمام قوانین
sudo nft add table inet filter       # اضافه کردن جدول
sudo nft add chain inet filter input '{ type filter hook input priority 0; }'
sudo nft add rule inet filter input tcp dport 80 accept
sudo nft add rule inet filter input ip saddr 192.168.1.0/24 accept
sudo nft add rule inet filter input drop
```

### ۹.۴ Firewalld (RHEL/CentOS/Fedora)

```bash
# مدیریت فایروال
sudo systemctl status firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld

# zoneها
sudo firewall-cmd --get-default-zone                # zone پیشفرض
sudo firewall-cmd --set-default-zone=public         # تغییر zone پیشفرض
sudo firewall-cmd --get-active-zones                 # zoneهای فعال
sudo firewall-cmd --list-all                         # قوانین zone فعلی
sudo firewall-cmd --list-all --zone=public           # قوانین zone خاص

# اضافه کردن سرویس
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload

# اضافه کردن پورت
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --remove-port=8080/tcp --permanent

# حذف سرویس
sudo firewall-cmd --remove-service=http --permanent

# rich rules
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" service name="ssh" accept' --permanent

# port forwarding
sudo firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080 --permanent

# اعمال تغییرات
sudo firewall-cmd --reload
sudo firewall-cmd --complete-reload

# ریست کردن
sudo firewall-cmd --panic-off
sudo firewall-cmd --panic-on       # بلاک تمام ترافیک
```

### ۹.۵ SSH — Secure Shell

```bash
# اتصال
ssh user@server                     # اتصال ساده
ssh -p 2222 user@server            # پورت سفارشی
ssh -i ~/.ssh/mykey user@server     # کلید خاص
ssh -L 8080:localhost:80 user@server  # Port Forwarding (Local)
ssh -R 9090:localhost:3000 user@server  # Remote Port Forwarding
ssh -D 1080 user@server            # SOCKS Proxy (Dynamic Forwarding)

# انتقال فایل
scp file.txt user@server:/remote/path/         # کپی به سرور
scp user@server:/remote/file.txt ./local/      # کپی از سرور
scp -r dir/ user@server:/remote/path/          # کپی بازگشتی

# rsync (بهتر از scp)
rsync -avz local/ user@server:/remote/path/    # همگامسازی
rsync -avz --delete local/ user@server:/remote/ # حذف فایلهای اضافی
rsync -avz -e "ssh -p 2222" local/ user@server:/remote/  # پورت سفارشی

# کلید SSH
ssh-keygen -t ed25519 -C "your@email.com"     # ایجاد کلید (پیشنهادی)
ssh-keygen -t rsa -b 4096 -C "your@email.com"  # ایجاد کلید RSA
ssh-copy-id user@server                         # کپی کلید عمومی به سرور
ssh-add ~/.ssh/id_ed25519                       # اضافه کردن به agent

# فایلهای مهم
~/.ssh/id_ed25519                 # کلید خصوصی
~/.ssh/id_ed25519.pub             # کلید عمومی
~/.ssh/authorized_keys            # کلیدهای مجاز
~/.ssh/config                     # پیکربندی اتصالات
~/.ssh/known_hosts                # هاستهای شناختهشده
/etc/ssh/sshd_config              # پیکربندی سرور SSH
```

**پیکربندی امنیتی SSH Server:**

```bash
# /etc/ssh/sshd_config
Port 2222                                    # تغییر پورت
PermitRootLogin no                           # غیرفعال کردن ورود root
PasswordAuthentication no                    # فقط کلید (غیرفعال کردن رمز)
PubkeyAuthentication yes                     # فعال کردن کلید
AllowUsers john sarah                        # فقط کاربران خاص
MaxAuthTries 3                               # حداکثر تلاش
ClientAliveInterval 300                      # timeout اتصال
ClientAliveCountMax 2                        # تعداد timeout
Protocol 2                                   # فقط SSH v2
AllowTcpForwarding no                        # غیرفعال کردن forwarding
X11Forwarding no                             # غیرفعال کردن X11
Banner /etc/ssh/banner                       # پیام خوشامدگویی

# اعمال تغییرات
sudo sshd -t                                  # بررسی syntax
sudo systemctl restart sshd
```

**فایل ~/.ssh/config:**

```
Host production
    HostName 192.168.1.100
    User deploy
    Port 2222
    IdentityFile ~/.ssh/id_ed25519_prod
    ForwardAgent yes

Host staging
    HostName staging.example.com
    User admin
    IdentityFile ~/.ssh/id_ed25519_staging
```

> [!TIP]
> `ssh-keygen -t ed25519` در آزمون LPIC-1 توصیه شده است. کلیدهای Ed25519 سریعتر و امنتر از RSA هستند و اندازه کوچکتری دارند.

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## ۱۰. امنیت سیستم

### ۱۰.۱ PAM — Pluggable Authentication Modules

PAM سیستمی مدولار برای احراز هویت و کنترل دسترسی است:

```bash
# فایلهای مهم
/etc/pam.d/                        # تنظیمات PAM برای هر سرویس
/etc/pam.d/sshd                    # PAM برای SSH
/etc/pam.d/login                   # PAM برای login
/etc/pam.d/common-auth             # احراز هویت مشترک (Debian)
/etc/pam.d/system-auth             # احراز هویت سیستم (RHEL)

# ساختار فایل PAM:
# type    control_flag    module_path    [arguments]
#
# type: auth, account, password, session
# control_flag: required, requisite, sufficient, optional, include

# مثال: اضافه کردن محدودیت تلاش ورود
# در /etc/pam.d/sshd یا /etc/pam.d/common-auth:
# auth    required    pam_tally2.so deny=5 unlock_time=900
# (۵ تلاش ناموفق → قفل ۹۰۰ ثانیه)

# مشاهده تلاشهای ناموفق
sudo pam_tally2 --user john
sudo pam_tally2 --user john --reset    # ریست شمارنده

# مثال: اجبار رمز قوی
# auth    required    pam_pwquality.so retry=3 minlen=12 ucredit=-1 lcredit=-1 dcredit=-1

# فایل تنظیمات: /etc/security/pwquality.conf
# minlen = 12
# dcredit = -1 (حداقل ۱ رقم)
# ucredit = -1 (حداقل ۱ حرف بزرگ)
# lcredit = -1 (حداقل ۱ حرف کوچک)
# ocredit = -1 (حداقل ۱ کاراکتر خاص)
```

### ۱۰.۲ SELinux

SELinux یک سیستم امنیتی اجباری (MAC) است که در RHEL/CentOS/Fedora فعال است:

```bash
# وضعیت SELinux
getenforce                          # Enforcing, Permissive, Disabled
sestatus                            # وضعیت کامل
cat /etc/selinux/config             # پیکربندی

# سه حالت
# Enforcing:   اعمال قوانین + بلاک کردن
# Permissive:  فقط گزارش (بدون بلاک)
# Disabled:    غیرفعال

# تغییر موقت به Permissive
sudo setenforce 0                   # موقت
sudo setenforce 1                   # بازگشت

# تغییر دائمی
# در /etc/selinux/config:
# SELINUX=enforcing
# sudo reboot

# مشاهده contexts
ls -Z /var/www/html/                # context فایلها
ps auxZ | grep httpd                # context پروسسها
id -Z                               # context کاربر

# مشکلات SELinux
sudo ausearch -m AVC -ts recent     # جستجوی violations اخیر
sudo audit2why < /var/log/audit/audit.log  # توضیح violations
sudo audit2allow -a                 # تولید سیاست اصلاحی
sudo semanage fcontext -a -t httpd_sys_content_t "/data/www(/.*)?"
sudo restorecon -Rv /data/www        # اعمال context
sudo setsebool -P httpd_can_network_connect on

# Port
sudo semanage port -a -t http_port_t -p tcp 8080

# Boolean
getsebool -a | grep httpd           # تمام booleans مربوط به httpd
```

### ۱۰.۳ AppArmor (Ubuntu/Debian)

```bash
# وضعیت
sudo aa-status                      # وضعیت AppArmor

# دو حالت
# Enforcing:  اعمال قوانین
# Complain:   فقط گزارش

# مدیریت profiles
sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx   # حالت Enforcing
sudo aa-complain /etc/apparmor.d/usr.sbin.nginx  # حالت Complain
sudo aa-disable /etc/apparmor.d/usr.sbin.nginx   # غیرفعال

# ایجاد profile
sudo aa-genprof /path/to/program
```

### ۱۰.۴ GPG — رمزنگاری

```bash
# ایجاد کلید
gpg --gen-key

# لیست کلیدها
gpg --list-keys
gpg --list-secret-keys

# رمزنگاری فایل
gpg -e -r recipient@email.com file.txt    # ایجاد file.txt.gpg

# رمزگشایی
gpg -d file.txt.gpg > file.txt

# امضا
gpg --sign file.txt                        # ایجاد file.txt.gpg
gpg --verify file.txt.gpg                  # بررسی امضا

# ارسال کلید
gpg --export -a "email" > publickey.asc
gpg --import publickey.asc
```

### ۱۰.۵ ملاحظات امنیتی کلی

```bash
# یافتن فایلهای SUID/SGID
find / -perm -4000 -type f 2>/dev/null   # SUID
find / -perm -2000 -type f 2>/dev/null   # SGID
find / -perm -4000 -o -perm -2000 -type f 2>/dev/null

# غیرفعال کردن SUID روی فایلهای غیرضروری
chmod u-s /usr/bin/unnecessary_binary

# یافتن فایلهای قابل نوشتن توسط دیگران
find / -xdev -type f -perm -o+w 2>/dev/null

# یافتن فایلهای بدون مالک
find / -nouser -o -nogroup 2>/dev/null

# بررسی کاربران بدون رمز
sudo awk -F: '($2 == "" || $2 == "!") {print $1}' /etc/shadow

# کاربران با UID 0 (فقط root باید داشته باشد)
awk -F: '($3 == 0) {print $1}' /etc/passwd

# بررسی sshd_config
sudo sshd -T | grep -E "^(permitrootlogin|passwordauthentication|pubkeyauthentication)"

# fail2ban
sudo systemctl status fail2ban
sudo fail2ban-client status sshd     # وضعیت ban
sudo fail2ban-client set sshd unbanip 1.2.3.4  # رفع ban

# ریست کردن رمز root
sudo passwd root

# قفل کردن حساب کاربری
sudo passwd -l username
sudo usermod -L username

# باز کردن قفل
sudo passwd -u username
sudo usermod -U username
```

> [!WARNING]
> در محیطهای تولیدی، همیشه `PasswordAuthentication no` را در SSH فعال کنید و فقط از کلیدهای SSH استفاده کنید. ورود root را غیرفعال کنید و از `fail2ban` استفاده کنید.

---

[⬆ بازگشت به فهرست مطالب](#فهرست-مطالب-تعاملی)

---

## مرجع سریع — دستورات پرتکرار

| دستور | توضیح |
|-------|-------|
| `ls -lah` | لیست فایلها (جزئیات، hidden، خوانا) |
| `cd -` | بازگشت به دایرکتوری قبلی |
| `pwd` | مسیر فعلی |
| `du -sh /path` | اندازه دایرکتوری (خوانا) |
| `df -h` | استفاده از دیسک |
| `top` / `htop` | مانیتورینگ پروسسها |
| `free -h` | استفاده از حافظه |
| `uname -a` | اطلاعات سیستم |
| `lscpu` | اطلاعات CPU |
| `lspci -nn` | دستگاههای PCI |
| `lsblk` | بلوک دستگاهها |
| `journalctl -b -p err` | خطای بوت آخرین |
| `systemctl status <svc>` | وضعیت سرویس |
| `ss -tlnp` | پورتهای گوشدهنده |
| `ip addr show` | آدرسهای IP |
| `grep -rn "pattern" /path` | جستجوی بازگشتی |
| `find / -name "*.log"` | جستجوی فایل |
| `chmod 755 file` | تنظیم مجوز |
| `chown user:group file` | تغییر مالکیت |
| `crontab -e` | ویرایش cron |
| `visudo` | ویرایش sudoers |
| `ssh-keygen -t ed25519` | ایجاد کلید SSH |

---

<p align="center">
  <sub>ساخته شده برای مهندسان DevOps و مدیران سیستم لینوکس — مرجع جامع LPIC-1</sub><br/>
  <sub>نسخه ۲.۰ — ۲۰۲۶ — مطابق با سرفصلهای رسمی LPIC-1 v5.0</sub>
</p>
