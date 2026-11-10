# Day 5: File operations aur text manipulation

## File operations

```bash
touch file1.txt file2.txt file3.txt  # khaali files (existing file par sirf timestamp badalta hai)
touch log{1..5}.txt                  # brace expansion: log1.txt se log5.txt tak
mkdir Day5                           # naya folder
mkdir ../Day5Backup                  # ../ = ek level upar banaya
mkdir -p project/src/app             # -p = beech ke saare folders ek saath, error nahi
ls -R project                        # andar ke saare folders dikhao
cp file1.txt ../Day5Backup/          # copy (original rehta hai)
cp -r project project_backup         # -r = folder copy (andar tak sab)
cp -p nginx.conf nginx.conf.bak      # -p = permissions/time same. Edit se pehle backup!
mv file3.txt file3_renamed.txt       # rename (Linux me rename ka alag command nahi)
mv *.log logs/                       # move
rm file2.txt                         # delete. Recycle Bin nahi hota!
rm -r project_backup                 # folder delete
rm -rf old_folder                    # bina pooche, force. Pehle pwd + ls se check karo
rmdir empty_folder                   # sirf khaali folder
```

## Text likhna aur padhna

```bash
echo "Linux is great" > opinion.txt  # > = OVERWRITE (purana sab mit jaata hai)
echo "DevOps is fun" >> opinion.txt  # >> = APPEND (end me judta hai)
cat -n opinion.txt                   # content, line numbers ke saath
cat a.txt b.txt > all.txt            # files jodna (concatenate)
head -n 5 /etc/passwd                # pehli 5 lines
tail -n 3 /etc/passwd                # aakhri 3 lines
tail -f ~/live.log                   # LIVE log (Ctrl+C se band). Production me logs aise dekhte hain
wc -l /etc/passwd                    # lines ginna. Output: 32 (yaani 32 users)
less /etc/passwd                     # badi file: /word = search, n = next, q = quit
```

## Galtiyan jo aaj hui (aur seekh)

| Galti | Error / Result | Seekh |
|---|---|---|
| `touch` se pehle `cp file1.txt` | `cannot stat 'file1.txt': No such file or directory` | File nahi thi. Pehle `ls` se check karo. |
| `cp project project_copy` (bina -r) | `-r not specified; omitting directory` | Folder copy ke liye `-r` zaroori |
| `echo "Oops" > opinion.txt` | Purani dono lines mit gayi | `>` overwrite karta hai. Backup lo, ya `>>` use karo. |
| Naye terminal me `nano opinion.txt` | Home me nayi khaali file ban gayi | Command se pehle prompt / `pwd` dekho ki kahan ho |

## /etc/passwd ki ek line

```
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
```

| No. | Value | Matlab |
|---|---|---|
| 1 | ubuntu | Username |
| 2 | x | Password `/etc/shadow` me hai (encrypted) |
| 3 | 1000 | UID. 0 = root, 1000+ = normal users |
| 4 | 1000 | GID (group ID) |
| 5 | Ubuntu | Description |
| 6 | /home/ubuntu | Home folder |
| 7 | /bin/bash | Shell. `nologin` = is user se login nahi ho sakta (system users) |

## nano

| Key | Kaam |
|---|---|
| Ctrl + O, Enter | Save |
| Ctrl + X | Bahar |

## vim (3 modes)

| Mode | Kaise aate hain | Kaam |
|---|---|---|
| Normal | File kholte hi / Esc | Commands |
| Insert | i ya o | Type karna |
| Command | Normal me : | :w, :q, :wq, :q! |

| Key | Kaam |
|---|---|
| i | Insert mode |
| o | Neeche nayi line + insert |
| Esc | Normal mode |
| :wq | Save + quit |
| :q! | Bina save quit (phas jao to Esc, fir :q!) |
| dd | Line delete |
| yy / p | Copy / paste |
| u | Undo |
| gg / G | Pehli / aakhri line |
| /word, n | Search, agla match |
| :set number | Line numbers |

## Interview questions

1. **cp vs mv?** cp copy banata hai (original rehta hai), mv move/rename karta hai.
2. **File rename kaise?** `mv old new`
3. **> vs >>?** > overwrite, >> append.
4. **Folder copy me kaunsa flag?** `-r`
5. **rm -rf safely kaise?** Pehle pwd aur ls se check, ya -i use karo, backup rakho.
6. **Log live kaise dekhoge?** `tail -f file`
7. **Vim me save + quit? Bina save?** `:wq` aur `:q!`
8. **touch existing file par kya karta hai?** Sirf timestamp update.
9. **mkdir -p kyun?** Beech ke folders bana deta hai, folder ho to error nahi deta.
10. **Config edit se pehle kya karoge?** `cp file file.bak` se backup.
11. **Password /etc/passwd me kyun nahi?** Ye file sab padh sakte hain, isliye encrypted password /etc/shadow me hai (sirf root padh sakta hai).
12. **Root ka UID kya hota hai?** Hamesha 0.
