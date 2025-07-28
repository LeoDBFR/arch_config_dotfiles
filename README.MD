This is my custom ZSH config, and arch config, with steps to install the requirements and Paru.


```
[leo@archiso ~]$ sudo pacman -Sy zsh fzf eza atuin fastfetch bat pkgfile mold base-devel
```

```
[leo@archiso ~]$ sudo pkgfile -U
```

```
[leo@archiso ~]$ CFG="/etc/makepkg.conf"
[leo@archiso ~]$ threads="$(nproc)"

[leo@archiso ~]$ sudo sed -i \
  -e "s|^PKGEXT=.*|PKGEXT='.pkg.tar'|" \
  -e "s|^#\\?MAKEFLAGS=.*|MAKEFLAGS=\"-j${threads}\"|" \
  -e "s|^\\(CFLAGS=.*-march=\\)x86-64|\\1native|" \
  -e '/^LDFLAGS=/ {               # only the LDFLAGS line
        /-fuse-ld=mold/! {        # skip if it’s already there
          s|-Wl,-O1|-Wl,-O1 -fuse-ld=mold|
        }
      }' \
  "$CFG"
  
[leo@archiso ~]$ RUSTCFG="/etc/makepkg.conf.d/rust.conf"

[leo@archiso ~]$ sudo sed -i \
  -e 's|^RUSTFLAGS=.*|RUSTFLAGS="-Cforce-frame-pointers=yes -C link-arg=-fuse-ld=mold -C opt-level=2 -C target-cpu=native"|' \
  "$RUSTCFG"

```


```
[leo@archiso ~]$ cd ~
[leo@archiso ~]$ git clone https://aur.archlinux.org/paru.git
[leo@archiso ~]$ cd paru
[leo@archiso ~]$ makepkg -si
[leo@archiso ~]$ cd ..
[leo@archiso ~]$ rm -rf paru
```


```
[leo@archiso ~]$ paru -Sy zsh-antidote ttf-meslo-nerd-font-powerlevel10k
```

Then copy the ZSH config.
