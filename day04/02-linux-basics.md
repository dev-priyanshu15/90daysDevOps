# Day 4 (Part 2): Linux intro aur filesystem navigation

## Linux kya hai

- **Linux** asal me ek **kernel** hai: OS ka core, jo hardware (CPU, RAM, disk) aur software ke beech bridge hai.
- **1969:** AT&T Bell Labs me UNIX bana (paid)
- **1983:** Richard Stallman ka GNU project (free tools: bash, gcc, ls)
- **1991:** Linus Torvalds ne Linux kernel banaya
- GNU tools + Linux kernel = poora OS (isliye "GNU/Linux")

## Distros

| Distro | Kiske liye | Package manager |
|---|---|---|
| Ubuntu | Beginners, servers, cloud | apt |
| Debian | Stable servers | apt |
| RHEL / CentOS / Rocky | Company servers | yum / dnf |
| Amazon Linux | AWS EC2 | yum / dnf |
| Alpine | Docker images (~5 MB) | apk |

## Welcome screen padhna

| Line | Matlab |
|---|---|
| `Ubuntu 26.04.1 LTS` | Distro + version. LTS = 5 saal support |
| `aarch64` | ARM CPU (Apple Silicon). Intel/AMD = x86_64 |
| `System load` | CPU par kitna kaam hai |
| `Usage of /` | Disk kitni bhari hai |
| `Memory usage` | RAM kitni use ho rahi hai |
| `IPv4 address` | VM ka IP |

## Filesystem tree

```
/
├── home/   -> users ke folders (/home/ubuntu)
├── root/   -> root user ka home
├── etc/    -> configuration files
├── var/    -> badalne wala data, logs (/var/log)
├── bin/    -> basic commands
├── usr/    -> installed software
├── tmp/    -> temporary files
├── opt/    -> third-party software
└── dev/    -> devices
```

Linux me **"everything is a file"**: disk, devices, process info, sab file ki tarah dikhte hain.

## Commands jo chalaye

```bash
sudo apt update
# sudo = admin (root) power se chalao
# apt update = sirf package list refresh. Kuch install nahi karta.

sudo apt upgrade -y
# Packages asli me update kiye. -y = har sawaal par "yes"
# Naye server par pehla kaam: update, khaaskar security patches ke liye

pwd
# Print Working Directory: main kahan hoon. Output: /home/ubuntu

cat /etc/os-release
# OS ki jaankari. Naye server par sabse pehle yahi check karte hain

ls
# Current folder ki files

ls -la
# -l = long format (permissions, owner, size, date)
# -a = hidden files bhi (. se shuru hone wali, jaise .bashrc)

cd /
ls
# Root par gaye. Yahan se poora tree shuru hota hai

cd /etc
ls
# Config files ka folder

cat hostname
# Machine ka naam ek file me likha hai (devops). "Everything is a file"

cd /var/log
ls
# Logs ka folder. Kuch toote to sabse pehle yahan dekhte hain

cd ~
pwd
# ~ = home folder. Kahin se bhi ghar wapas

mkdir Day1
# Naya folder

cd Day1
# Us folder me gaye (relative path)

echo "My first day learning Linux" > intro.txt
# Text file me likha. > = overwrite, >> = append

ls -l
cat intro.txt
# File bani ya nahi, aur uska content
```

## Absolute vs relative path

```bash
cd /var/log
cd /home/ubuntu/Day1
# Absolute path: / se shuru, kahin se bhi chalega

cd /var/log
cd ../../home/ubuntu/Day1
# Relative path: current jagah se
# ../ = /var  ->  ../../ = /  ->  wahan se home/ubuntu/Day1
```

| Symbol | Matlab |
|---|---|
| `/` | Root (sabse upar) |
| `~` | Home folder |
| `.` | Current folder |
| `..` | Parent folder |
| `-` | Pichhla folder (`cd -`) |

## history ke tricks

```bash
history                # saare pichhle commands, number ke saath
history | grep apt     # sirf "apt" wale commands
!18                    # 18 number wala command dobara chalao
!!                     # pichhla command dobara
sudo !!                # pichhla command sudo ke saath ("Permission denied" par kaam aata hai)
```

Shortcuts: `Ctrl + R` = history me search, `↑` = pichhla command, `Tab` = naam auto-complete

## Interview questions

1. **Linux kernel kya hai?** OS ka core, jo hardware aur software ke beech bridge hai aur memory, CPU, devices manage karta hai.
2. **Linux aur UNIX me farak?** UNIX proprietary tha. Linux open-source hai, UNIX jaisa banaya gaya, par UNIX ka code nahi hai.
3. **Distro kya hai? Ubuntu vs CentOS?** Kernel + tools + package manager ka bundle. Ubuntu = apt (Debian-based), CentOS/RHEL = yum/dnf.
4. **apt update vs apt upgrade?** update sirf list refresh karta hai, upgrade asli me packages install/update karta hai.
5. **Absolute vs relative path?** Absolute `/` se shuru hota hai, relative current directory se.
6. **/etc aur /var/log me kya hota hai?** Config files aur logs.
7. **Hidden file kaise banti aur dikhti hai?** Naam `.` se shuru karo, `ls -a` se dikhti hai.
8. **`$` aur `#` prompt me farak?** `$` = normal user, `#` = root user.
9. **Pichhle user ne kya commands chalaye, kaise pata karoge?** `history`, ya `~/.bash_history` file.
