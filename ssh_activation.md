# Install dan Konfigurasi SSH Server pada Ubuntu LXC (Proxmox)

Tutorial ini menjelaskan cara mengaktifkan SSH Server pada Ubuntu LXC sehingga dapat diakses dari komputer lain menggunakan SSH.

---

# 1. Update Package

```bash
sudo apt update
sudo apt upgrade -y
```

---

# 2. Install OpenSSH Server

```bash
sudo apt install -y openssh-server
```

---

# 3. Enable SSH Service

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

# 4. Cek Status SSH

```bash
sudo systemctl status ssh
```

Output yang diharapkan:

```
Active: active (running)
```

---

# 5. Cek Port SSH

```bash
ss -tlnp | grep :22
```

Output yang diharapkan:

```
LISTEN 0 128 0.0.0.0:22
```

atau

```
LISTEN 0 128 [::]:22
```

---

# 6. Konfigurasi SSH

Buka file konfigurasi:

```bash
sudo nano /etc/ssh/sshd_config
```

Pastikan konfigurasi berikut:

```text
Port 22

PermitRootLogin no

PasswordAuthentication yes

PubkeyAuthentication yes

UsePAM yes
```

Simpan file.

---

# 7. Restart SSH

```bash
sudo systemctl restart ssh
```

---

# 8. Cek Firewall Ubuntu (Opsional)

Jika menggunakan UFW:

```bash
sudo ufw allow 22/tcp
sudo ufw reload
```

Cek status:

```bash
sudo ufw status
```

---

# 9. Cek IP Address

```bash
ip addr
```

atau

```bash
hostname -I
```

Contoh:

```
192.168.1.183
```

---

# 10. Login Menggunakan SSH

Dari komputer lain:

```bash
ssh developer@192.168.1.183
```

Jika pertama kali login:

```
Are you sure you want to continue connecting (yes/no)?
```

Ketik:

```
yes
```

Masukkan password user.

---

# 11. Login Menggunakan PuTTY (Windows)

Host:

```
192.168.1.183
```

Port:

```
22
```

Connection Type:

```
SSH
```

Username:

```
developer
```

Password:

```
********
```

---

# 12. Cek Service SSH

```bash
sudo systemctl status ssh
```

Melihat log SSH:

```bash
sudo journalctl -u ssh -f
```

---

# 13. Restart SSH

```bash
sudo systemctl restart ssh
```

---

# 14. Stop SSH

```bash
sudo systemctl stop ssh
```

---

# 15. Disable SSH Saat Boot

```bash
sudo systemctl disable ssh
```

---

# 16. Enable SSH Saat Boot

```bash
sudo systemctl enable ssh
```

---

# 17. Troubleshooting

## SSH Tidak Bisa Login

Pastikan service berjalan:

```bash
sudo systemctl status ssh
```

---

## Port 22 Tidak Terbuka

```bash
ss -tlnp | grep :22
```

---

## Restart Service

```bash
sudo systemctl restart ssh
```

---

## Test Dari Server Sendiri

```bash
ssh developer@localhost
```

---

## Cek Konfigurasi SSH

```bash
sudo sshd -t
```

Jika tidak ada output berarti konfigurasi valid.

---

# 18. Login Sebagai Root (Tidak Direkomendasikan)

Edit:

```bash
sudo nano /etc/ssh/sshd_config
```

Ubah:

```text
PermitRootLogin yes
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

> **Catatan:** Untuk keamanan, disarankan menggunakan user biasa (misalnya `developer`) dan menjalankan perintah administratif dengan `sudo`, daripada login langsung sebagai `root`.

---

# 19. Informasi Server

| Item | Nilai |
|------|-------|
| OS | Ubuntu LXC |
| Hypervisor | Proxmox VE |
| SSH Server | OpenSSH |
| Port | 22 |
| User | developer |
| IP | 192.168.1.183 |

---

# Selesai

Server Ubuntu LXC kini dapat diakses menggunakan SSH dari komputer lain.

Contoh koneksi:

```bash
ssh developer@192.168.1.183
```
