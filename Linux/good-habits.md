- [Thói quen tốt khi dùng Linux](#thói-quen-tốt-khi-dùng-linux)
  - [TL;DR](#tldr)
  - [1) Xoá file: an toàn nhưng không phiền](#1-xoá-file-an-toàn-nhưng-không-phiền)
  - [2) Chống ghi đè: bật *noclobber*](#2-chống-ghi-đè-bật-noclobber)
  - [3) Lịch sử lệnh: dài, sạch, dễ tìm](#3-lịch-sử-lệnh-dài-sạch-dễ-tìm)
  - [4) Cài đặt \& script: hạn chế `curl | bash`](#4-cài-đặt--script-hạn-chế-curl--bash)
  - [5) Quyền hạn \& sudo: gọn gàng mà an toàn](#5-quyền-hạn--sudo-gọn-gàng-mà-an-toàn)
  - [6) Giữ $HOME sạch: tuân thủ **XDG Base Directory**](#6-giữ-home-sạch-tuân-thủ-xdg-base-directory)
  - [7) Công cụ tăng tốc](#7-công-cụ-tăng-tốc)
  - [8) Viết shell script “đỡ toang”](#8-viết-shell-script-đỡ-toang)
  - [9) Một vài mẹo nhỏ đáng nhớ](#9-một-vài-mẹo-nhỏ-đáng-nhớ)
  - [10) Mẫu `~/.bashrc` “starter pack”](#10-mẫu-bashrc-starter-pack)
  - [11) Ghi chú về tranh luận cộng đồng](#11-ghi-chú-về-tranh-luận-cộng-đồng)
  - [12) Bonus trick “đời sống”](#12-bonus-trick-đời-sống)
- [Linux Good Habits — Quick Cheatsheet](#linux-good-habits--quick-cheatsheet)
    - [Safe Deletes (rm)](#safe-deletes-rm)
    - [No Accidental Overwrites](#no-accidental-overwrites)
    - [History That Works For You](#history-that-works-for-you)
    - [fzf Keybindings (khuyên dùng)](#fzf-keybindings-khuyên-dùng)
    - [Keep $HOME Clean (XDG)](#keep-home-clean-xdg)
    - [Handy Extras](#handy-extras)
    - [Package Hints (tham khảo nhanh)](#package-hints-tham-khảo-nhanh)
- [Setup Script (tạo backup, cài gói, chèn block vào ~/.bashrc)](#setup-script-tạo-backup-cài-gói-chèn-block-vào-bashrc)


# Thói quen tốt khi dùng Linux

## TL;DR

* **Xoá an toàn:** *Tránh* alias `rm='rm -i'` global; nếu muốn an toàn, chỉ bật trong **shell tương tác** hoặc dùng `rm -I`, `trash-cli`, hay `safe-rm`. ([linuxvox][1], [Super User][2], [Linux Command Library][3], [packages.debian.org][4])
* **Chống ghi đè file:** bật `set -o noclobber`; khi cần ghi đè chủ động, dùng `>|`. ([Baeldung on Kotlin][5], [Super User][6], [mywiki.wooledge.org][7])
* **Lịch sử lệnh hữu dụng & sạch:** tăng `HISTSIZE`, dùng `HISTCONTROL=ignoreboth:erasedups`, `HISTIGNORE`, và tìm nhanh bằng **Ctrl-R** (hoặc fzf cho Ctrl-R “xịn”). ([GNU][8], [Ask Ubuntu][9], [Red Hat][10])
* **Cài đặt an toàn:** hạn chế `curl | bash`; tải về, xem, *verify* rồi mới chạy. (Có tranh luận hai chiều—mình tóm tắt bên dưới.) ([Sasha Vinčić][11], [Stack Overflow][12], [Atomic Spin][13], [arp242.net][14])
* **Giữ \$HOME gọn gàng:** theo **XDG Base Directory** (`~/.config`, `~/.local/share`, `~/.cache`). ([specifications.freedesktop.org][15])
* **Tăng năng suất:** `rg` (ripgrep), `fd`, `eza`, `bat`. ([blog.burntsushi.net][16], [GitHub][17], [LinuxLinks][18])
* **Lint/format shell:** `shellcheck` + `shfmt`. ([shellcheck.net][19], [GitHub][20])

---

## 1) Xoá file: an toàn nhưng không phiền

**Vì sao không nên alias `rm='rm -i'` mọi nơi?**
Nhiều admin cho rằng `rm -i` làm hỏng thói quen (prompt spam), và **nguy hiểm** nếu script vô tình thừa hưởng alias. Thảo luận trên Superuser/Reddit cũng khuyên chỉ dùng trong shell tương tác, hoặc thay bằng `-I` (chỉ hỏi khi xoá nhiều file). ([Super User][2], [linuxvox][1])

**Các lựa chọn tốt hơn:**

* **Chỉ alias trong interactive shell** (an toàn cho script):

  ```bash
  # ~/.bashrc
  case $- in *i*) alias rm='rm -I' ;; esac
  alias cp='cp -i'
  alias mv='mv -i'
  ```

  `-I` hỏi khi >3 mục hoặc xoá đệ quy; ít phiền hơn `-i` mỗi file. (Coreutils có chế độ interactive nhiều mức.) ([Maizure][21])
* **Thùng rác dòng lệnh:** `trash-cli` (khôi phục được, có `trash-list`, `trash-restore`, `trash-empty`). ([Linux Command Library][3])
* **Chặn xoá nhầm thư mục hệ thống:** `safe-rm` (wrapper chặn xoá các path “bảo vệ” như `/`, `/usr`,…). Có sẵn trên Debian/Ubuntu. ([packages.debian.org][4], [Launchpad][22])
* **Lưu ý bổ sung:** GNU `rm` mặc định **từ chối** lệnh kiểu `rm -rf /` (bảo vệ “/”). Đừng tắt flag bảo vệ này. ([GNU][23])

---

## 2) Chống ghi đè: bật *noclobber*

* Bật ngay:

  ```bash
  # Không cho '>' ghi đè file tồn tại
  set -o noclobber
  ```
* Khi *cố ý* ghi đè, **ghi rõ chủ đích**:

  ```bash
  echo "overwrite" >| file.txt
  ```

Giảm tai nạn do `>` vô tình. ([Baeldung on Kotlin][5], [Super User][6])

---

## 3) Lịch sử lệnh: dài, sạch, dễ tìm

* **Tăng dung lượng & tránh trùng lặp:**

  ```bash
  # ~/.bashrc
  HISTSIZE=200000
  HISTFILESIZE=200000
  HISTCONTROL=ignoreboth:erasedups
  HISTTIMEFORMAT='%F %T '
  shopt -s histappend
  PROMPT_COMMAND='history -a; history -c; history -r; $PROMPT_COMMAND'
  ```

  (Bash manual có các biến `HIST*`, `histappend`… Giúp log gọn, có timestamp.) ([GNU][8], [Stack Overflow][24])
* **Tìm nhanh bằng Ctrl-R** (readline). Nếu lỡ vượt quá, có thể bật tìm xuôi. ([Super User][25])
* **Nâng cấp Ctrl-R bằng fzf:** keybinding thay thế tìm mờ lịch sử, pop-up chọn lệnh.
  Cài `fzf` và bật tích hợp:

  ```bash
  # fzf >= 0.48
  eval "$(fzf --bash)"
  ```

  (Arch Wiki + blog Red Hat có hướng dẫn phím tắt `Ctrl-r`, `Ctrl-t`, `Alt-c`.) ([ArchWiki][26], [Red Hat][10])
* **Ẩn lệnh nhạy cảm khỏi lịch sử:**
  Bắt đầu bằng dấu cách (vì `ignorespace`) hoặc lọc bằng `HISTIGNORE`. ([Ask Ubuntu][9])

---

## 4) Cài đặt & script: hạn chế `curl | bash`

Quan điểm cộng đồng khá rõ: **tiện nhưng rủi ro** (không xem được nội dung, có thể bị tấn công theo UA/streaming, khó audit/rollback). Hướng an toàn:

1. Tải script về file. 2) Đọc nhanh. 3) Kiểm tra chữ ký/hash (nếu có). 4) Chạy với quyền tối thiểu.
   Bài viết cảnh báo và phân tích rủi ro: ([Sasha Vinčić][11], [djm.org.uk][27], [Atomic Spin][13])
   Cũng có ý kiến “không tệ nếu hiểu rủi ro và kiểm soát tốt” — tóm lại: **biết mình làm gì**. ([arp242.net][14])

---

## 5) Quyền hạn & sudo: gọn gàng mà an toàn

* **Quên sudo?** Dùng mẹo `sudo !!` để lặp lại lệnh trước với sudo. ([Stack Overflow][28])
* **Sửa sudoers** bằng `visudo` (kiểm tra cú pháp trước khi ghi), hạn chế NOPASSWD cho nhóm nhỏ (mẹo chung; xem `man visudo` nếu cần).

---

## 6) Giữ \$HOME sạch: tuân thủ **XDG Base Directory**

Đặt config ở `~/.config`, data ở `~/.local/share`, cache ở `~/.cache`…

```bash
# ~/.profile (hoặc nơi export env)
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_STATE_HOME="$HOME/.local/state"
export XDG_CACHE_HOME="$HOME/.cache"
```

Chuẩn XDG từ freedesktop.org. ([specifications.freedesktop.org][15])

---

## 7) Công cụ tăng tốc

* **ripgrep (`rg`)**: tìm siêu nhanh, tôn trọng `.gitignore`. ([blog.burntsushi.net][16])
* **fd**: thay cho `find` với cú pháp dễ. ([GitHub][17])
* **eza**: `ls` hiện đại, có cột git, icon, tree… (kế thừa `exa`). ([GitHub][29])
* **bat**: `cat` có highlight + tích hợp git. ([LinuxLinks][18])

---

## 8) Viết shell script “đỡ toang”

* **Bật “strict-ish mode” khi phù hợp:** `set -Eeo pipefail` và cân nhắc `set -u` (nhưng nhớ rằng cộng đồng có tranh luận; hiểu tác dụng trước khi dùng). ([linuxvox][30])
* **Luôn quote biến** để tránh word-splitting/globbing bất ngờ; ShellCheck nhắc rất kỹ lỗi này. ([shellcheck.net][31])
* **Lint & format:**

  ```bash
  shellcheck your_script.sh
  shfmt -w your_script.sh
  ```

  (ShellCheck & shfmt là combo kinh điển cho shell.) ([shellcheck.net][19], [GitHub][20])

---

## 9) Một vài mẹo nhỏ đáng nhớ

* **Kết thúc tuỳ chọn bằng `--`** trước tên file bắt đầu bằng `-`.
  Ví dụ: `rm -- -weird` (tránh file tên “-…” bị hiểu là option).
* **Thao tác “có ý thức”:** thêm `-v`/`-n` (dry-run) khi có: `rsync -avn`, `sed -n`, `cp -v`…
* **Xoá an toàn với snapshot:** nếu dùng Btrfs/ZFS/Timeshift, snapshot giúp *undo* khi lỡ tay (ý kiến phổ biến trong cộng đồng).

---

## 10) Mẫu `~/.bashrc` “starter pack”

> Copy, dán, rồi chỉnh theo nhu cầu. Phù hợp cho **shell tương tác**.

```bash
# ----- Safety -----
case $- in *i*) alias rm='rm -I'; alias cp='cp -i'; alias mv='mv -i';; esac
set -o noclobber

# ----- History -----
HISTSIZE=200000
HISTFILESIZE=200000
HISTCONTROL=ignoreboth:erasedups
HISTTIMEFORMAT='%F %T '
shopt -s histappend
PROMPT_COMMAND='history -a; history -c; history -r; $PROMPT_COMMAND'

# Ẩn mấy lệnh vặt/nhạy cảm khỏi history (ví dụ)
HISTIGNORE='ls:cd:pwd:clear:history:* --help:* --version'

# ----- fzf (nếu có) -----
# fzf >= 0.48, bật keybindings & completion
command -v fzf >/dev/null && eval "$(fzf --bash)"

# ----- XDG -----
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_STATE_HOME="$HOME/.local/state"
export XDG_CACHE_HOME="$HOME/.cache"

# ----- Prompt/helpful utils -----
alias grep='grep --color=auto'
alias ll='eza -lag --git --group-directories-first' 2>/dev/null || alias ll='ls -alF'
```

---

## 11) Ghi chú về tranh luận cộng đồng 

* **`rm -i` vs an toàn “thông minh”:** phần đông khuyên **không ép `-i` mọi lúc**, chỉ bật trong interactive, ưu tiên `-I`/`trash-cli`/`safe-rm`. (Reddit/Superuser nhiều bình luận trải nghiệm đau thương vì prompt hoặc script vỡ). ([Super User][2], [linuxvox][1])
* **`curl | bash`:** nhiều bài cảnh báo; một số bài cho rằng “không quá xấu” nếu bạn hiểu và kiểm soát nguy cơ. Hãy chọn **tải-xem-verify-chạy**. ([Sasha Vinčić][11], [Atomic Spin][13], [arp242.net][14])

---

## 12) Bonus trick “đời sống”

* Lỡ quên `sudo`: gõ `sudo !!` để chạy lại lệnh trước đó với sudo. (Cộng đồng thích vì nhanh.) ([Stack Overflow][28])


---

# Linux Good Habits — Quick Cheatsheet

### Safe Deletes (rm)

* Chỉ bật an toàn trong **shell tương tác**:

  ```bash
  case $- in *i*) alias rm='rm -I'; alias cp='cp -i'; alias mv='mv -i';; esac
  ```
* `-I` chỉ hỏi **một lần** khi xoá nhiều file/đệ quy (đỡ phiền hơn `-i`).
* **Thùng rác dòng lệnh:** `trash-put`, `trash-list`, `trash-restore`, `trash-empty`.
* **safe-rm:** wrapper chặn xoá các path nhạy cảm (/, /usr, ...).
* `rm` hiện đại mặc định có `--preserve-root`, chặn `rm -rf /`.

### No Accidental Overwrites

```bash
set -o noclobber           # chặn '>' ghi đè
echo data >| file.txt      # khi CỐ Ý ghi đè
```

### History That Works For You

```bash
# ~/.bashrc (interactive)
HISTSIZE=200000
HISTFILESIZE=200000
HISTCONTROL=ignoreboth:erasedups
HISTTIMEFORMAT='%F %T '
shopt -s histappend

# merge history liên phiên (idempotent)
if [[ $PROMPT_COMMAND != *"history -a; history -c; history -r"* ]]; then
  PROMPT_COMMAND="history -a; history -c; history -r; ${PROMPT_COMMAND:-}"
fi
```

* Tìm nhanh: **Ctrl-R**; nâng cấp bằng **fzf** (popup chọn lệnh).

### fzf Keybindings (khuyên dùng)

```bash
# fzf >= 0.48
command -v fzf >/dev/null && {
  if fzf --bash >/dev/null 2>&1; then eval "$(fzf --bash)";
  elif [ -f /usr/share/fzf/key-bindings.bash ]; then source /usr/share/fzf/key-bindings.bash; fi
}
# Phím: Ctrl-R (history), Ctrl-T (files), Alt-C (cd fuzzy)
```

### Keep \$HOME Clean (XDG)

```bash
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_STATE_HOME="$HOME/.local/state"
export XDG_CACHE_HOME="$HOME/.cache"
```

### Handy Extras

* Kết thúc tuỳ chọn khi tên file bắt đầu bằng `-`: `rm -- -weird`
* Dùng dry-run/verbose khi có: `rsync -avn`, `cp -v`, `mv -v`
* Quên sudo? `sudo !!`

### Package Hints (tham khảo nhanh)

```
Debian/Ubuntu:  sudo apt update && sudo apt install -y fzf trash-cli safe-rm
Fedora/RHEL:    sudo dnf install -y fzf trash-cli safe-rm
Arch:           sudo pacman -S --needed fzf trash-cli safe-rm
openSUSE:       sudo zypper install -y fzf trash-cli safe-rm
```

---

# Setup Script (tạo backup, cài gói, chèn block vào \~/.bashrc)

> Lưu thành `linux_setup.sh`, xem lại nội dung cho chắc, rồi chạy:
> `bash linux_setup.sh && source ~/.bashrc`

```bash
#!/usr/bin/env bash
set -Eeuo pipefail

log() { printf "\n[linux-setup] %s\n" "$*"; }

detect_pm() {
  if command -v apt-get >/dev/null; then echo apt;
  elif command -v dnf >/dev/null; then echo dnf;
  elif command -v pacman >/dev/null; then echo pacman;
  elif command -v zypper >/dev/null; then echo zypper;
  elif command -v apk >/dev/null; then echo apk;
  elif command -v brew >/dev/null; then echo brew;
  else echo none; fi
}

install_pkgs() {
  local pm
  pm=$(detect_pm)
  log "Detected package manager: $pm"
  case "$pm" in
    apt)
      sudo apt update -y || true
      sudo apt install -y fzf trash-cli safe-rm || true
      ;;
    dnf)
      sudo dnf install -y fzf trash-cli safe-rm || true
      ;;
    pacman)
      sudo pacman -Sy --needed fzf trash-cli safe-rm || true
      ;;
    zypper)
      sudo zypper --non-interactive in fzf trash-cli safe-rm || true
      ;;
    apk)
      sudo apk add fzf trash-cli || true
      log "safe-rm có thể không có trên Alpine; bỏ qua nếu không tìm thấy."
      ;;
    brew)
      brew install fzf trash-cli safe-rm || true
      ;;
    *)
      log "Không tìm thấy PM hỗ trợ. Cài fzf, trash-cli, safe-rm thủ công nhé."
      ;;
  esac
}

backup_bashrc() {
  local ts src dst
  ts=$(date +%Y%m%d_%H%M%S)
  src="$HOME/.bashrc"
  if [[ -f "$src" ]]; then
    dst="$HOME/.bashrc.backup.$ts"
    cp -a "$src" "$dst"
    log "Đã backup ~/.bashrc -> $dst"
  else
    log "~/.bashrc chưa có; sẽ tạo mới."
    touch "$src"
  fi
}

apply_block() {
  local rc="$HOME/.bashrc"
  local begin="# BEGIN linux-good-habits"
  local end="# END linux-good-habits"
  # Gỡ block cũ nếu có (idempotent)
  if grep -qF "$begin" "$rc" 2>/dev/null; then
    awk '/# BEGIN linux-good-habits/{flag=1; next} /# END linux-good-habits/{flag=0; next} !flag{print}' "$rc" > "$rc.tmp" && mv "$rc.tmp" "$rc"
    log "Đã gỡ block linux-good-habits cũ."
  fi
  # Thêm block mới
  {
    echo "$begin"
    cat <<'BLOCK'
# ----- Safety -----
case $- in *i*) alias rm='rm -I'; alias cp='cp -i'; alias mv='mv -i';; esac
set -o noclobber

# ----- History -----
HISTSIZE=200000
HISTFILESIZE=200000
HISTCONTROL=ignoreboth:erasedups
HISTTIMEFORMAT='%F %T '
shopt -s histappend

# Merge session history continuously (idempotent)
if [[ $PROMPT_COMMAND != *"history -a; history -c; history -r"* ]]; then
  PROMPT_COMMAND="history -a; history -c; history -r; ${PROMPT_COMMAND:-}"
fi

# Hide some common/noisy commands from history (optional)
HISTIGNORE='ls:cd:pwd:clear:history:* --help:* --version'

# ----- fzf (if available) -----
command -v fzf >/dev/null && {
  if fzf --bash >/dev/null 2>&1; then eval "$(fzf --bash)";
  elif [ -f /usr/share/fzf/key-bindings.bash ]; then source /usr/share/fzf/key-bindings.bash; fi
}

# ----- XDG dirs -----
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_STATE_HOME="$HOME/.local/state"
export XDG_CACHE_HOME="$HOME/.cache"

# ----- Helpful ls -----
if command -v eza >/dev/null 2>&1; then
  alias ll='eza -lag --git --group-directories-first'
else
  alias ll='ls -alF'
fi
BLOCK
    echo "$end"
  } >> "$rc"
  log "Đã chèn block linux-good-habits vào ~/.bashrc"
}

main() {
  install_pkgs
  backup_bashrc
  apply_block
  log "Xong. Mở shell mới hoặc chạy: source ~/.bashrc"
}

main "$@"
```

[1]: https://linuxvox.com/blog/ubuntu-alias/?utm_source=chatgpt.com "Mastering Ubuntu Aliases: A Comprehensive Guide - linuxvox.com"
[2]: https://superuser.com/questions/382407/best-practices-to-alias-the-rm-command-and-make-it-safer?utm_source=chatgpt.com "Best practices to alias the rm command and make it safer"
[3]: https://linuxcommandlibrary.com/man/trash-cli?utm_source=chatgpt.com "trash-cli man | Linux Command Library"
[4]: https://packages.debian.org/stable/safe-rm?utm_source=chatgpt.com "Debian -- Details of package safe-rm in trixie"
[5]: https://www.baeldung.com/linux/shell-file-protection?utm_source=chatgpt.com "Linux Shell File Protection | Baeldung on Linux"
[6]: https://superuser.com/questions/354439/how-do-i-override-the-bash-noclobber-option?utm_source=chatgpt.com "How do I override the bash \"noclobber\" option? - Super User"
[7]: https://mywiki.wooledge.org/NoClobber?utm_source=chatgpt.com "NoClobber - Greg's Wiki"
[8]: https://www.gnu.org/software/bash/manual/html_node/Bash-History-Facilities.html?utm_source=chatgpt.com "Bash History Facilities (Bash Reference Manual)"
[9]: https://askubuntu.com/questions/15926/how-to-avoid-duplicate-entries-in-bash-history?utm_source=chatgpt.com "How to avoid duplicate entries in .bash_history - Ask Ubuntu"
[10]: https://www.redhat.com/en/blog/fzf-linux-fuzzy-finder?utm_source=chatgpt.com "Find anything you need with fzf, the Linux fuzzy finder tool"
[11]: https://sasha.vincic.org/blog/2024/09/piping-curl-to-bash-convenient-but-risky?utm_source=chatgpt.com "Piping curl to bash: Convenient but risky – Sasha Vinčić"
[12]: https://stackoverflow.com/questions/29382739/why-using-curl-sudo-sh-is-not-advised?utm_source=chatgpt.com "Why using curl | sudo sh is not advised? - Stack Overflow"
[13]: https://spin.atomicobject.com/security-spectrum-curl-sh/?utm_source=chatgpt.com "The Security Spectrum of curl | sh - Atomic Spin"
[14]: https://www.arp242.net/curl-to-sh.html?utm_source=chatgpt.com "Curl to shell isn’t so bad - arp242.net"
[15]: https://specifications.freedesktop.org/basedir-spec/latest/?utm_source=chatgpt.com "XDG Base Directory Specification - freedesktop.org"
[16]: https://blog.burntsushi.net/ripgrep/?utm_source=chatgpt.com "ripgrep is faster than {grep, ag, git grep, ucg, pt, sift}"
[17]: https://github.com/dongbinghua/sharkdp-fd?utm_source=chatgpt.com "GitHub - dongbinghua/sharkdp-fd: A simple, fast and user-friendly ..."
[18]: https://www.linuxlinks.com/excellent-utilities-eza-replacement-ls/?utm_source=chatgpt.com "Excellent Utilities: eza - replacement for ls - LinuxLinks"
[19]: https://www.shellcheck.net/?utm_source=chatgpt.com "ShellCheck – shell script analysis tool"
[20]: https://github.com/topics/shfmt?utm_source=chatgpt.com "shfmt · GitHub Topics · GitHub"
[21]: https://www.maizure.net/projects/decoded-gnu-coreutils/rm.html?utm_source=chatgpt.com "Decoded: rm (coreutils) – MaiZure's Projects"
[22]: https://launchpad.net/ubuntu/focal/%2Bpackage/safe-rm?utm_source=chatgpt.com "safe-rm : Focal (20.04) : Ubuntu - launchpad.net"
[23]: https://www.gnu.org/software/coreutils/manual/html_node/Treating-_002f-specially.html?utm_source=chatgpt.com "Treating / specially (GNU Coreutils 9.7)"
[24]: https://stackoverflow.com/questions/50656852/how-to-remove-duplicate-commands-from-bash-history-file?utm_source=chatgpt.com "How to remove duplicate commands from Bash history file?"
[25]: https://superuser.com/questions/7414/how-can-i-search-the-bash-history-and-rerun-a-command?utm_source=chatgpt.com "How can I search the bash history and rerun a command?"
[26]: https://wiki.archlinux.org/title/Fzf?utm_source=chatgpt.com "fzf - ArchWiki"
[27]: https://www.djm.org.uk/posts/protect-yourself-from-non-obvious-dangers-curl-url-pipe-sh/?utm_source=chatgpt.com "The hidden dangers of piping curl - djm.org.uk"
[28]: https://stackoverflow.com/questions/17245614/repeat-last-command-with-sudo?utm_source=chatgpt.com "bash - Repeat last command with \"sudo\" - Stack Overflow"
[29]: https://github.com/eza-community/eza/blob/main/README.md?utm_source=chatgpt.com "eza/README.md at main · eza-community/eza · GitHub"
[30]: https://linuxvox.com/blog/alias-command-ubuntu/?utm_source=chatgpt.com "Mastering the Alias Command in Ubuntu — linuxvox.com"
[31]: https://www.shellcheck.net/wiki/SC2086?utm_source=chatgpt.com "Double quote to prevent globbing and word splitting. - ShellCheck"
