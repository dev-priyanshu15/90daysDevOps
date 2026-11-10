# Day 5: File operations aur text manipulation

Is din ke do topics: **files aur folders ke saath kaam** (banana, copy, move, delete) aur **files padhna aur edit karna**. Ye Linux ki "basic verbs" hain, inke bina kuch nahi hota.

---

## Part A: Files aur folders

### 1. `touch`: khaali file banana

**Kya karta hai:** Nayi khaali file banata hai. Agar file pehle se hai, to sirf uska time (modified date) update karta hai, content nahi chhedta.

```bash
touch file1.txt                # ek file
touch a.txt b.txt c.txt        # ek saath teen files
touch log{1..5}.txt            # log1.txt se log5.txt tak (brace expansion)
```

**Daily use:** Jaldi se khaali config, log ya script file banani ho.

### 2. `mkdir`: folder banana

**Kya karta hai:** Naya directory (folder) banata hai.

```bash
mkdir project
mkdir dev test prod            # teen folders ek saath
mkdir -p app/src/utils         # poora path ek saath, beech ke folders bhi
```

**Sabse important flag `-p` (parents):** Bina `-p` ke, agar `app` nahi hai to `app/src` banane par error aayega. `-p` beech ke saare folders khud bana deta hai, aur folder pehle se ho to error bhi nahi deta. **Scripts me hamesha `mkdir -p` use hota hai.**

### 3. `cp`: copy karna

**Kya karta hai:** File ya folder ki copy banata hai. Original wahi rehta hai.

```bash
cp file1.txt backup.txt            # same jagah, naye naam se copy
cp file1.txt /tmp/                 # doosre folder me copy
cp -r project/ project_backup/     # poora folder copy (-r zaroori)
cp -i file1.txt backup.txt         # overwrite se pehle poochega
cp -v file1.txt /tmp/              # kya copy hua, dikhayega
cp -p nginx.conf nginx.conf.bak    # permissions aur time bhi same rakhega
```

| Flag | Kaam |
|---|---|
| `-r` | Folder copy (recursive, andar ka sab kuch). Iske bina folder copy nahi hota. |
| `-i` | Overwrite se pehle confirm |
| `-v` | Verbose, har step dikhata hai |
| `-p` | Permissions, owner aur time preserve |

**DevOps ki sabse achhi aadat:** kisi bhi config file ko edit karne se pehle backup lo:

```bash
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
```

Kuch toota to backup wapas copy kar do.

### 4. `mv`: move aur rename, dono

**Kya karta hai:** File ko ek jagah se doosri jagah le jaata hai. Linux me alag se rename ka command nahi hai, `mv` hi rename karta hai.

```bash
mv file3.txt file3_renamed.txt     # rename
mv report.txt ~/Documents/         # move
mv *.log logs/                     # saari .log files logs/ me
mv -i a.txt b.txt                  # b.txt pehle se hai to poochega
```

- `cp` ke baad do files hoti hain, `mv` ke baad ek hi.
- Folder move karne ke liye `-r` nahi chahiye.

### 5. `rm`: delete karna (dhyan se)

**Kya karta hai:** File ya folder delete karta hai. **Linux me Recycle Bin nahi hota.** Ek baar delete hua to gaya.

```bash
rm file2.txt                       # file delete
rm -i file2.txt                    # pehle poochega
rm -r old_folder/                  # folder aur andar ka sab
rm -rf old_folder/                 # bina pooche, zabardasti (khatarnak)
rmdir empty_folder/                # sirf khaali folder
```

| Flag | Kaam |
|---|---|
| `-r` | Folder ke liye |
| `-f` | Force: koi confirmation nahi, error bhi nahi |
| `-i` | Har file par poochega |

**Warning:** `rm -rf` se pehle hamesha `pwd` aur `ls` se check karo ki sahi jagah ho. Galti se galat folder me `rm -rf *` chal gaya to production server khatam.

---

## Part B: Files padhna aur likhna

### 6. `echo`: text print karna ya file me likhna

```bash
echo "Hello DevOps"                     # screen par print
echo "Linux is great" > opinion.txt     # file me likho (purana MITA dega)
echo "Second line" >> opinion.txt       # end me jodo (append)
echo $HOME                              # variable ki value
echo $USER
```

**Sabse important concept:**

- `>` = **overwrite**. Purana sab mit jaata hai.
- `>>` = **append**. Purana rehta hai, naya neeche judta hai.

Logs me hamesha `>>` use hota hai.

### 7. `cat`: file ka content dekhna

Naam "concatenate" (jodna) se aaya hai.

```bash
cat opinion.txt                    # file dikhao
cat -n opinion.txt                 # line numbers ke saath
cat file1.txt file2.txt            # dono ek ke baad ek
cat file1.txt file2.txt > all.txt  # dono jod kar nayi file
```

Chhoti files ke liye theek hai. Badi file par screen bhar jaayegi, uske liye neeche wale commands hain.

### 8. `head` aur `tail`: shuru ya aakhri lines

```bash
head file.txt                      # pehli 10 lines
head -n 5 file.txt                 # pehli 5 lines
tail file.txt                      # aakhri 10 lines
tail -n 20 /var/log/syslog         # aakhri 20 lines
tail -f /var/log/syslog            # LIVE: nayi line aate hi dikhegi
```

**`tail -f` DevOps ka sabse zyada use hone wala command hai.** App crash ho rahi hai? `tail -f app.log` chalao aur live errors dekho. Band karne ke liye `Ctrl + C`.

### 9. `less`: badi file aaram se padhna

```bash
less /var/log/syslog
```

| Key | Kaam |
|---|---|
| Space / b | Agla / pichhla page |
| /error | "error" dhoondho |
| n | Agla match |
| G / g | End / shuruaat |
| q | Bahar |

### 10. `wc`: ginti karna

```bash
wc -l file.txt                     # kitni lines
wc -w file.txt                     # kitne words
wc -l /etc/passwd                  # kitne users (output: 32)
```

---

## Part C: Editors (server par file edit karna)

Server par VS Code nahi hota. Terminal ke andar hi file edit karni padti hai.

### 11. `nano`: aasaan editor

```bash
nano opinion.txt
```

| Key | Kaam |
|---|---|
| Seedha type karo | Notepad jaisa |
| Ctrl + O, Enter | Save |
| Ctrl + X | Bahar |

Neeche screen par shortcuts likhe hote hain (`^` = Ctrl).

### 12. `vim`: har server par milta hai

Shuru me ajeeb lagta hai, par ye **har Linux server par pehle se hota hai**, nano nahi bhi hota. DevOps engineer ko vim aana zaroori hai.

**3 modes:**

1. **Normal mode:** File kholte hi isme hote ho. Yahan type karne se text nahi likhta, commands chalte hain.
2. **Insert mode:** `i` ya `o` dabao, ab type kar sakte ho. Neeche `-- INSERT --` dikhta hai. Wapas jaane ke liye `Esc`.
3. **Command mode:** Normal mode me `:` dabao, neeche command likho.

| Key | Kaam |
|---|---|
| i | Insert mode (cursor ki jagah) |
| o | Neeche nayi line + insert |
| Esc | Wapas Normal mode |
| :w | Save |
| :q | Quit |
| :wq | Save + quit |
| :q! | Bina save quit |
| dd | Poori line delete |
| yy | Line copy |
| p | Paste |
| u | Undo |
| gg / G | Pehli / aakhri line |
| /word, n | Search, agla match |
| :set number | Line numbers |

**Phas jao to:** `Esc` 2-3 baar, fir `:q!` aur Enter. Ye hamesha bahar nikaal deta hai.

**Built-in tutorial:** terminal me `vimtutor` likho (20-30 min, bahut kaam ka).

---

## Part D: /etc/passwd samjho (interview favourite)

`/etc/passwd` me system ke saare users hain, ek line = ek user.

```
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
```

| No. | Value | Matlab |
|---|---|---|
| 1 | ubuntu | Username |
| 2 | x | Password yahan nahi, `/etc/shadow` me encrypted |
| 3 | 1000 | UID. **0 = root**, 1000+ = normal users |
| 4 | 1000 | GID (group ID) |
| 5 | Ubuntu | Description / naam |
| 6 | /home/ubuntu | Home folder |
| 7 | /bin/bash | Login shell |

- `daemon`, `bin`, `sys`, `polkitd` jaise **system users** services chalane ke liye hain. Unka shell `/usr/sbin/nologin` ya `/bin/false` hota hai, yaani inse login nahi ho sakta (security).
- File UID ke order me nahi hoti, jo user jab bana uski line tab judti hai.
- Is VM me 32 users hain, par login sirf `root` aur `ubuntu` kar sakte hain.

---

## Practice jo maine ki

```bash
cd ~
mkdir Day5 && cd Day5
mkdir ../Day5Backup
touch file1.txt file2.txt file3.txt
ls -l
cp file1.txt ../Day5Backup/
ls -l ../Day5Backup
mv file3.txt file3_renamed.txt
rm file2.txt
ls                                  # file1.txt  file3_renamed.txt

touch log{1..5}.txt
mkdir -p project/src/app
ls -R project
cp -r project project_backup
ls -R project_backup
rm -r project_backup
cp project project_copy             # error: -r not specified

echo "Linux is great" > opinion.txt
echo "DevOps is fun" >> opinion.txt
cat -n opinion.txt
echo "Oops" > opinion.txt           # dono lines mit gayi (> ka khatra)

head -n 5 /etc/passwd
tail -n 3 /etc/passwd
wc -l /etc/passwd                   # 32
less /etc/passwd

touch ~/live.log
tail -f ~/live.log                  # terminal 1
echo "Server started" >> ~/live.log # terminal 2, line live aayi

nano opinion.txt                    # line jodi
vim opinion.txt                     # G, o, type, Esc, :wq
vim opinion.txt                     # gg, dd, u, yy, G, p, :q!
```

## Galtiyan jo hui (aur seekh)

| Galti | Error / Result | Seekh |
|---|---|---|
| `touch` se pehle `cp file1.txt` | `cannot stat 'file1.txt': No such file or directory` | File nahi thi. Pehle `ls` se check karo. |
| `cp project project_copy` | `-r not specified; omitting directory` | Folder copy ke liye `-r` |
| `echo "Oops" > opinion.txt` | Purani lines mit gayi | `>` overwrite karta hai. Backup lo ya `>>` |
| Naye terminal (home) me `nano opinion.txt` | Home me nayi khaali file ban gayi | Command se pehle prompt / `pwd` dekho |

---

## Interview questions

1. **cp vs mv?** cp copy banata hai (original rehta hai), mv move/rename karta hai.
2. **File rename kaise?** `mv old new`
3. **> vs >>?** > overwrite, >> append.
4. **Folder copy me kaunsa flag?** `-r`
5. **rm -rf kya karta hai, safely kaise?** Folder aur sab content bina pooche delete. Pehle pwd aur ls se check karo, -i use karo, backup rakho.
6. **Log live kaise dekhoge?** `tail -f file`
7. **Badi log file me "error" kaise dhoondhoge?** `less` me `/error`, ya `grep error file`
8. **Vim me save + quit? Bina save?** `:wq` aur `:q!`
9. **touch existing file par kya karta hai?** Sirf timestamp update.
10. **mkdir -p kyun?** Beech ke folders bana deta hai, folder ho to error nahi deta.
11. **Config edit se pehle kya karoge?** `cp file file.bak` se backup.
12. **Password /etc/passwd me kyun nahi?** Ye file sab padh sakte hain, isliye encrypted password /etc/shadow me (sirf root padh sakta hai).
13. **Root ka UID?** Hamesha 0.
14. **`No such file or directory` aaye to kya check karoge?** `pwd` (sahi folder?) aur `ls` (file hai? spelling/capital letters sahi?). Linux case-sensitive hai.

## Resources

- Vim cheat sheet: https://devhints.io/vim
- `vimtutor` (terminal me)
- Kisi bhi command ki help: `man cp` ya `cp --help`
