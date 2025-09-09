---

````markdown
# 🛠️ CTF & Pentest Tools Cheatsheet

Полезная шпаргалка по инструментам для кибербезопасности, CTF и pentest.

---

## 🔹 Nmap
**Назначение:** Сканирование сети, портов, ОС и сервисов.  
**Примеры:**
```bash
nmap 192.168.1.1                # базовый скан
nmap -sV 192.168.1.1            # определить версии сервисов
nmap -A 192.168.1.1             # агрессивный режим (OS, версии, скрипты)
nmap -p- 192.168.1.1            # скан всех портов
````

---

## 🔹 Masscan

**Назначение:** Очень быстрый сканер портов.
**Примеры:**

```bash
masscan 192.168.1.0/24 -p80,443 --rate 1000
```

---

## 🔹 Netcat

**Назначение:** Отладка TCP/UDP соединений, простой "Swiss army knife".
**Примеры:**

```bash
nc -lvnp 4444                  # слушать порт
nc target.com 80                # подключиться к хосту
```

---

## 🔹 John the Ripper

**Назначение:** Брутфорс и словарный взлом паролей/хэшей.
**Примеры:**

```bash
john --wordlist=rockyou.txt hashes.txt
john --format=raw-md5 hashes.txt
```

---

## 🔹 Hydra

**Назначение:** Брутфорс паролей к сетевым сервисам.
**Примеры:**

```bash
hydra -l admin -P rockyou.txt ssh://192.168.1.10
hydra -L users.txt -P passwords.txt ftp://target.com
```

---

## 🔹 Wireshark

**Назначение:** Перехват и анализ сетевого трафика (GUI).
**CLI альтернатива:**

```bash
tcpdump -i any port 80
```

---

## 🔹 SQLmap

**Назначение:** Эксплуатация SQL-инъекций.
**Примеры:**

```bash
sqlmap -u "http://site.com/page.php?id=1" --batch
sqlmap -u "http://site.com/item?id=5" --dbs
sqlmap -u "http://site.com/item?id=5" -D testdb -T users --dump
```

---

## 🔹 Gobuster

**Назначение:** Брутфорс директорий, файлов и сабдоменов.
**Примеры:**

```bash
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
gobuster dns -d target.com -w subdomains.txt
```

---

## 🔹 FFUF

**Назначение:** Быстрый fuzzing веб-директорий/параметров.
**Примеры:**

```bash
ffuf -u http://target.com/FUZZ -w wordlist.txt
ffuf -u http://target.com/index.php?id=FUZZ -w nums.txt
```

---

## 🔹 Nikto

**Назначение:** Веб-сканер на уязвимости.
**Пример:**

```bash
nikto -h http://target.com
```

---

## 🔹 Hashcat

**Назначение:** Взлом хэшей (GPU).
**Примеры:**

```bash
hashcat -m 0 hash.txt rockyou.txt          # MD5
hashcat -m 100 hash.txt rockyou.txt        # SHA1
hashcat -m 1800 hash.txt rockyou.txt       # sha512crypt
```

---

## 🔹 Pwntools (Python)

**Назначение:** Фреймворк для эксплуатации бинарей (pwn).
**Пример:**

```python
from pwn import *

# локальный процесс
p = process("./vuln")

# удалённое подключение
# p = remote("target.com", 1337)

p.sendline(b"A"*40)
print(p.recvline())
```

---

## 🔹 ROPgadget

**Назначение:** Поиск ROP-гаджетов в бинарях.
**Пример:**

```bash
ROPgadget --binary vuln_binary | grep "pop rdi"
```

---

## 🔹 Radare2

**Назначение:** Анализ бинарей, дизассемблер.
**Примеры:**

```bash
r2 ./binary
aaa          # автоанализ
afl          # список функций
pdf @ main   # дизассемблировать main
```

---

## 🔹 Ghidra

**Назначение:** Декомпилятор и GUI-анализатор бинарей.
**Запуск:**

```bash
ghidraRun
```

---

## 🔹 Binwalk

**Назначение:** Анализ бинарных файлов и прошивок.
**Примеры:**

```bash
binwalk firmware.bin
binwalk -e firmware.bin    # извлечь содержимое
```

---

## 🔹 Exiftool

**Назначение:** Метаданные файлов (изображения, документы).
**Пример:**

```bash
exiftool image.jpg
```

---

## 🔹 Steghide

**Назначение:** Встраивание и извлечение данных в медиафайлах.
**Примеры:**

```bash
steghide embed -cf cover.jpg -ef secret.txt
steghide extract -sf cover.jpg
```

---

## 🔹 Zbar

**Назначение:** Чтение QR-кодов.
**Пример:**

```bash
zbarimg qr.png
```

---

## 🔹 Foremost

**Назначение:** Восстановление файлов по сигнатурам.
**Пример:**

```bash
foremost image.dd
```

---

## 🔹 Sleuthkit

**Назначение:** Forensics: анализ дисков, файловых систем.
**Примеры:**

```bash
fls image.dd
icat image.dd 12345 > file.txt
```

---

## 🔹 Volatility

**Назначение:** Анализ дампов памяти.
**Примеры:**

```bash
volatility -f memdump.raw --profile=Win7SP1x64 pslist
volatility -f memdump.raw --profile=Win7SP1x64 filescan
```

---

## 🔹 YARA

**Назначение:** Поиск вредоносного ПО по сигнатурам.
**Пример:**

```bash
yara rule.yar suspicious_file.exe
```

---

## 🔹 Angr (Python)

**Назначение:** Символическое исполнение бинарей.
**Пример:**

```python
import angr

proj = angr.Project("./vuln", auto_load_libs=False)
state = proj.factory.entry_state()
sim = proj.factory.simulation_manager(state)
sim.explore(find=lambda s: b"Correct!" in s.posix.dumps(1))
if sim.found:
    print(sim.found[0].posix.dumps(0))
```

---

## 🔹 Recon-ng

**Назначение:** OSINT-платформа.
**Пример:**

```bash
recon-ng
[recon-ng][default] > marketplace install all
```

---

## 🔹 Tor + Proxychains

**Назначение:** Анонимизация и маршрутизация запросов.
**Примеры:**

```bash
tor &
proxychains nmap target.com
```

---

```

---