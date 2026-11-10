# Day 4 (Part 1): Multipass VM aur GitHub SSH setup

## 1. Multipass install (Mac terminal)

```bash
brew install --cask multipass
# brew = Mac ka package manager (Ubuntu ke apt jaisa)
# --cask = GUI/installer wale apps ke liye. Multipass cask hai, isliye ye flag zaroori hai.

multipass version
# Install sahi hua ya nahi, check kiya. Koi bhi tool install karke version check karna achhi aadat hai.
```

## 2. Mac ki RAM check

```bash
sysctl -n hw.memsize | awk '{print $1/1073741824 " GB"}'
# RAM bytes me nikali, awk se GB me badli (1073741824 bytes = 1 GB)
# | (pipe) = pehle command ka output doosre command ko de do
# Result: 24 GB
```

## 3. VM banaya

```bash
multipass launch --name devops --cpus 4 --memory 6G --disk 40G
# Ubuntu VM: 4 CPU, 6 GB RAM, 40 GB disk
# Aage Docker, Jenkins aur Kubernetes ke liye itna chahiye

multipass shell devops
# VM ke andar gaye. Prompt: ubuntu@devops:~$
```

## 4. Problems aur unka solution

| Problem | Wajah | Solution |
|---|---|---|
| `brew install` fail, "No formula named #" | Mac ka zsh `#` ko comment nahi maanta, comments bhi package naam ban gaye | Commands bina comment ke chalaye. Ya `setopt interactivecomments` ko `~/.zshrc` me daalo. |
| `multipass: command not found` | Pichhla install fail hua tha | `brew install --cask multipass` alag se chalaya |
| VM me sirf 3.8 GB disk, 63% bhari | Multipass ki default disk chhoti hai | `delete` + `purge` karke 40 GB wala naya VM banaya |

## 5. Multipass ke kaam ke commands (Mac terminal)

| Command | Kya karta hai |
|---|---|
| `multipass list` | Saare VMs aur unka status |
| `multipass shell devops` | VM ke andar jao |
| `exit` | VM se bahar (VM chalta rehta hai) |
| `multipass stop devops` | VM band karo |
| `multipass start devops` | VM chalu karo |
| `multipass delete devops` | Delete ke liye mark (trash) |
| `multipass purge` | Trash khaali, VM poori tarah gaya |

## 6. GitHub se SSH connection (VM ke andar)

```bash
git --version
# Git Ubuntu me pehle se install tha

git config --global user.name "Priyanshu Singh"
git config --global user.email "s.priyanshu.coder@gmail.com"
# Har commit par likha jaata hai ki kisne kiya
# --global = is machine ke saare repos par lagega

git config --global --list
# Settings check ki

ssh-keygen -t ed25519 -C "s.priyanshu.coder@gmail.com"
# SSH key pair banaya. Teeno sawaal par Enter.
# ~/.ssh/id_ed25519      = PRIVATE key (chaabi), kabhi share nahi karni
# ~/.ssh/id_ed25519.pub  = PUBLIC key (taala), GitHub par lagani hai

cat ~/.ssh/id_ed25519.pub
# Public key copy karke GitHub > Settings > SSH and GPG keys > New SSH key me lagayi

ssh -T git@github.com
# Connection test. Output: "Hi dev-priyanshu15! You've successfully authenticated"
# Pehli baar GitHub ka fingerprint ~/.ssh/known_hosts me save hota hai
```

## Interview questions

1. **Server ko GitHub se kaise jodte ho?** SSH key pair banao, public key GitHub par lagao, private key server par hi rakho.
2. **Public aur private key me farak?** Public key taala hai, kahin bhi laga sakte ho. Private key chaabi hai, kabhi share nahi karte.
3. **known_hosts file kya hai?** Jin servers se pehle jud chuke ho unke fingerprints. Fingerprint badle to SSH warning deta hai, isse man-in-the-middle attack se bachaav hota hai.
4. **HTTPS ki jagah SSH kyun?** GitHub HTTPS par password accept nahi karta. SSH key se bina password ke, safely push hota hai.
