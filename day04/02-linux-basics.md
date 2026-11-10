# Day 4: Linux ka intro, setup aur filesystem

## 1. Linux hai kya?

Linux ek **operating system** hai, bilkul Windows ya macOS ki tarah. Asal me "Linux" sirf **kernel** ka naam hai. Kernel OS ka dil hota hai: wo hardware (CPU, RAM, disk) aur software ke beech baat karwata hai.

**Short history (interview me poochte hain):**

- **1969:** AT&T Bell Labs me **UNIX** bana. Bahut powerful tha, par paid tha.
- **1983:** Richard Stallman ne **GNU project** shuru kiya, taaki ek free UNIX jaisa OS bane. Tools ban gaye (`bash`, `gcc`, `ls`, `cp`), par kernel nahi bana.
- **1991:** Finland ke student **Linus Torvalds** ne Linux kernel banaya.
- GNU tools aur Linux kernel mile, to poora OS bana. Isliye kuch log ise **GNU/Linux** bhi kehte hain.

## 2. Distro kya hota hai?

Kernel same hai, par uske upar alag-alag companies aur communities apne tools, package manager aur settings jodti hain. Is poore package ko **distribution (distro)** kehte hain.

| Distro | Kiske liye | Package manager |
|---|---|---|
| Ubuntu | Beginners, servers, cloud (sabse common) | apt |
| Debian | Stable servers | apt |
| RHEL / CentOS / Rocky | Companies ke servers | yum / dnf |
| Amazon Linux | AWS EC2 | yum / dnf |
| Alpine | Docker images (bahut chhota, ~5 MB) | apk |
| Kali | Cybersecurity | apt |

**DevOps ke liye Ubuntu se shuru karo.** AWS par bhi sabse zyada wahi milega.

## 3. DevOps me Linux kyun zaroori hai?

- Duniya ke zyada-tar servers Linux par chalte hain
- Docker containers andar se Linux hi hote hain
- Kubernetes nodes, Jenkins servers, AWS EC2: sab jagah Linux
- Ek DevOps engineer ka aadha din terminal me guzarta hai

## 4. Linux chalane ke tareeke

| Tareeka | Matlab | Kab use karein |
|---|---|---|
| VM (Virtual Machine) | Apne laptop ke andar ek alag computer. Kuch toote to delete karke naya bana lo. | Seekhne ke liye best |
| Dual boot | Laptop me do OS, boot par chuno | Mac par jhanjhat, avoid karo |
| Cloud (AWS EC2) | Asli server, internet par | Day 47 me karenge |

Maine Mac par **Multipass** se Ubuntu VM banaya (setup ki detail `01-setup-multipass-github.md` me hai).

## 5. Prompt padhna

```
ubuntu@devops:~$
```

| Hissa | Matlab |
|---|---|
| `ubuntu` | User ka naam |
| `devops` | Machine ka naam (hostname) |
| `~` | Abhi kis folder me ho (`~` = home) |
| `$` | Normal user. `#` dikhe to tum root (admin) ho |

## 6. Welcome screen padhna

Server me login karte hi sabse pehle ye dekhte hain:

| Line | Matlab |
|---|---|
| `Ubuntu 26.04.1 LTS` | Distro + version. LTS = Long Term Support, 5 saal updates |
| `GNU/Linux 7.0.0-38-generic` | Kernel version |
| `aarch64` | ARM CPU (Apple Silicon). Intel/AMD servers = `x86_64` |
| `System load` | CPU par kitna kaam hai |
| `Usage of /` | Disk kitni bhari hai |
| `Memory usage` | RAM kitni use ho rahi hai |
| `IPv4 address` | VM ka IP |
| `X updates can be applied` | Packages purane hain, update karo |
| `System restart required` | Naya kernel aaya hai, reboot ke baad chalega |

## 7. Linux filesystem: sab kuch ek tree hai

Windows me `C:\`, `D:\` hote hain. Linux me sab kuch ek hi root `/` ke neeche hota hai.

```
/
├── home/   -> users ke folders (/home/ubuntu)
├── root/   -> root (admin) user ka home
├── etc/    -> configuration files (bahut important)
├── var/    -> badalne wala data, jaise logs (/var/log)
├── bin/    -> basic commands (ls, cp)
├── usr/    -> installed software
├── tmp/    -> temporary files (reboot par saaf)
├── opt/    -> third-party software
└── dev/    -> devices (disk, USB)
```

**Yaad rakho:** Linux me **"everything is a file"**. Disk, keyboard, process info, machine ka naam, sab kuch file ki tarah dikhta hai.

## 8. Commands (kya karta hai + kyun)

### System update

```bash
sudo apt update
# sudo = admin (root) ki power se chalao. Normal user system nahi badal sakta.
# apt update = sirf package list refresh karta hai. Kuch install nahi karta.

sudo apt upgrade -y
# Packages asli me update karta hai. -y = har sawaal par "yes"
# Naye server par pehla kaam: update, khaaskar security patches ke liye
```

### Kahan hoon, kya hai

```bash
pwd
# Print Working Directory: main abhi kahan hoon. Output: /home/ubuntu

cat /etc/os-release
# OS ki jaankari. Naye server par sabse pehle yahi check karte hain,
# kyunki apt chalega ya yum, ye isi par depend karta hai.

ls          # is folder me kya hai
ls -l       # long format: permissions, owner, size, date
ls -a       # hidden files bhi (jo . se shuru hoti hain, jaise .bashrc)
ls -la      # dono ek saath (sabse zyada use hota hai)
ls -lh      # size human-readable (KB, MB)
```

### Ghoomna (cd)

```bash
cd /etc     # /etc me jao (config files)
cd /var/log # logs ka folder. Kuch toote to sabse pehle yahan dekhte hain
cd ..       # ek level upar (parent folder)
cd ~        # home folder (sirf "cd" likhne se bhi)
cd -        # pichhle folder me wapas

cat /etc/hostname
# Machine ka naam ek file me likha hai (devops). "Everything is a file" ka example.
```

### Folder aur file banana

```bash
mkdir Day1
cd Day1
echo "My first day learning Linux" > intro.txt   # > = file me likho
ls -l
cat intro.txt
```

## 9. Absolute vs relative path (ye zaroor poochte hain)

- **Absolute path** hamesha `/` se shuru hota hai, aur kahin se bhi chalega: `cd /home/ubuntu/Day1`
- **Relative path** tum jahan khade ho, wahan se chalta hai: `cd Day1`, `cd ../logs`

```bash
cd /var/log
cd /home/ubuntu/Day1        # absolute

cd /var/log
cd ../../home/ubuntu/Day1   # relative
# ../ = /var  ->  ../../ = /  ->  wahan se home/ubuntu/Day1
```

| Symbol | Matlab |
|---|---|
| `/` | Root (sabse upar) |
| `~` | Home folder |
| `.` | Current folder |
| `..` | Parent folder |
| `-` | Pichhla folder (`cd -`) |

## 10. `ls -l` ka output padhna

```
-rw-rw-r-- 1 ubuntu ubuntu 0 Nov 10 16:29 file1.txt
drwxr-xr-x 2 ubuntu ubuntu 4096 Nov 10 16:29 Day1
```

| Hissa | Matlab |
|---|---|
| Pehla akshar | `-` = file, `d` = directory, `l` = link |
| `rw-rw-r--` | Permissions (Day 6 me detail) |
| `1` | Links ki ginti |
| `ubuntu ubuntu` | Owner aur group |
| `0` / `4096` | Size (bytes) |
| `Nov 10 16:29` | Kab bani / badli |

## 11. history ke tricks

```bash
history                # saare pichhle commands, number ke saath
history | grep apt     # sirf "apt" wale commands
!18                    # 18 number wala command dobara
!!                     # pichhla command dobara
sudo !!                # pichhla command sudo ke saath ("Permission denied" par kaam aata hai)
```

| Shortcut | Kaam |
|---|---|
| Ctrl + R | History me peechhe search |
| Up arrow | Pichhla command |
| Tab | File/folder ka naam auto-complete (typing aur typos dono bachte hain) |

## 12. Interview questions

1. **Linux kernel kya hai?** OS ka core hissa, jo hardware aur software ke beech bridge ka kaam karta hai, aur memory, CPU aur devices manage karta hai.
2. **Linux aur UNIX me farak?** UNIX proprietary tha (AT&T ka). Linux open-source hai aur UNIX jaisa banaya gaya, par usme UNIX ka code nahi hai.
3. **Distro kya hai? Ubuntu vs CentOS?** Kernel + tools + package manager ka bundle. Ubuntu Debian-based hai aur apt use karta hai. CentOS/RHEL yum/dnf.
4. **apt update vs apt upgrade?** update sirf list refresh karta hai, upgrade asli me packages update karta hai.
5. **Absolute vs relative path?** Absolute `/` se shuru, relative current directory se.
6. **/etc aur /var/log me kya hota hai?** Config files aur logs.
7. **Hidden file kaise banti aur dikhti hai?** Naam `.` se shuru karo (jaise `.bashrc`), `ls -a` se dikhti hai.
8. **`$` aur `#` prompt me farak?** `$` normal user, `#` root user.
9. **Pichhle user ne kya commands chalaye, kaise pata karoge?** `history`, ya `~/.bash_history` file.
10. **"Everything is a file" ka matlab?** Linux me devices, processes, config sab file ki tarah access hote hain (jaise /etc/hostname, /dev/sda).

## Resources

- TrainWithShubham DevOps Roadmap: https://www.youtube.com/watch?v=iOE9NTAG35g
- Netflix DevOps case study: https://www.simform.com/blog/netflix-devops-case-study/
- Cheat sheet: https://cheat.sh (terminal me `curl cheat.sh/ls`)
- Linux notes (Kunal Kushwaha): https://github.com/kunal-kushwaha/DevOps-Bootcamp/tree/main/Linux%20and%20Terminal%20commands
