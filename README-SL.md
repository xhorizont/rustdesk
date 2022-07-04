<p align="center">
  <img src="logo-header.svg" alt="RustDesk - Your remote desktop"><br>
  <a href="#free-public-servers">Strežniki</a> •
  <a href="#raw-steps-to-build">Zgradba</a> •
  <a href="#how-to-build-with-docker">Docker</a> •
  <a href="#file-structure">Struktura</a> •
  <a href="#snapshot">Snapshot</a><br>
  [<a href="README-CS.md">česky</a>] | [<a href="README-ZH.md">中文</a>] | [<a href="README-ES.md">Español</a>] | [<a href="README-FA.md">فارسی</a>] | [<a href="README-FR.md">Français</a>] | [<a href="README-DE.md">Deutsch</a>] | [<a href="README-PL.md">Polski</a>] | [<a href="README-ID.md">Indonesian</a>] | [<a href="README-FI.md">Suomi</a>] | [<a href="README-ML.md">മലയാളം</a>] | [<a href="README-JP.md">日本語</a>] | [<a href="README-NL.md">Nederlands</a>] | [<a href="README-IT.md">Italiano</a>] | [<a href="README-RU.md">Русский</a>] | [<a href="README-PTBR.md">Português (Brasil)</a>] | [<a href="README-EO.md">Esperanto</a>] | [<a href="README-KR.md">한국어</a>] | [<a href="README-AR.md">العربي</a>] | [<a href="README-SL.md">Slovenščina</a>]<br>
  
</p>

Klepetajte z nami: [Discord](https://discord.gg/nDceKgxnkV) | [Twitter](https://twitter.com/rustdesk) | [Reddit](https://www.reddit.com/r/rustdesk)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/I2I04VU09)

Še ena programska oprema za oddaljeno namizje, napisana v Rustu. Deluje takoj, konfiguracija ni potrebna. Imate popoln nadzor nad svojimi podatki, brez skrbi glede varnosti. Uporabite lahko naš strežnik za srečanja/posredovanje, [nastavite svojega](https://rustdesk.com/server), ali [napišete svoj strežnik za srečanja/posredovanje](https://github.com/rustdesk/rustdesk-server-demo).

![image](https://user-images.githubusercontent.com/71636191/171661982-430285f0-2e12-4b1d-9957-4a58e375304d.png)

RustDesk pozdravlja prispevke vseh. Glejte [`CONTRIBUTING.md`](CONTRIBUTING.md) za pomoč pri začetku.

[**Kako deluje RustDesk?**](https://github.com/rustdesk/rustdesk/wiki/How-does-RustDesk-work%3F)

[**BINARY DOWNLOAD**](https://github.com/rustdesk/rustdesk/releases)

[<img src="https://fdroid.gitlab.io/artwork/badge/get-it-on.png"
    alt="Get it on F-Droid"
    height="80">](https://f-droid.org/en/packages/com.carriez.flutter_hbb)

## Brezplačni javni strežniki

podaj so strežniki, ki jih uporabljate brezplačno, lahko se sčasoma spremenijo. Če niste blizu enega od teh, je vaše omrežje morda počasno.
| Lokacija | Ponudnik | Specifikacija |
| --------- | ------------- | ------------------ |
| Seoul | AWS lightsail | 1 VCPU / 0.5GB RAM |
| Singapore | Vultr | 1 VCPU / 1GB RAM |
| Dallas | Vultr | 1 VCPU / 1GB RAM | |

## Odvisnosti

Različice za namizne računalnike uporabljajo [sciter](https://sciter.com/) za GUI, prosimo, prenesite dinamično knjižnico sciter sami.

[Windows](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.win/x64/sciter.dll) |
[Linux](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so) |
[MacOS](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.osx/libsciter.dylib)

Mobilne različice uporabljajo Flutter. Preselili bomo namizno različico iz Sciterja v Flutter.

## Surovi koraki za gradnjo

- Pripravite razvojno okolje Rust in okolje gradnje C++

- Namestite [vcpkg](https://github.com/microsoft/vcpkg), `VCPKG_ROOT` pravilno nastavite spremenljivko env

  - Windows: vcpkg install libvpx:x64-windows-static libyuv:x64-windows-static opus:x64-windows-static
  - Linux/MacOS: vcpkg install libvpx libyuv opus

- run `cargo run`

## [Build](https://rustdesk.com/docs/en/dev/build/)

## Kako graditi na Linuxu

### Ubuntu 18 (Debian 10)

```sh
sudo apt install -y g++ gcc git curl wget nasm yasm libgtk-3-dev clang libxcb-randr0-dev libxdo-dev libxfixes-dev libxcb-shape0-dev libxcb-xfixes0-dev libasound2-dev libpulse-dev cmake
```

### Fedora 28 (CentOS 8)

```sh
sudo yum -y install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libxdo-devel libXfixes-devel pulseaudio-libs-devel cmake alsa-lib-devel
```

### Arch (Manjaro)

```sh
sudo pacman -Syu --needed unzip git cmake gcc curl wget yasm nasm zip make pkg-config clang gtk3 xdotool libxcb libxfixes alsa-lib pulseaudio
```

### Install pynput package

```sh
pip3 install pynput
```

### Install vcpkg

```sh
git clone https://github.com/microsoft/vcpkg
cd vcpkg
git checkout 2021.12.01
cd ..
vcpkg/bootstrap-vcpkg.sh
export VCPKG_ROOT=$HOME/vcpkg
vcpkg/vcpkg install libvpx libyuv opus
```

### Fix libvpx (For Fedora)

```sh
cd vcpkg/buildtrees/libvpx/src
cd *
./configure
sed -i 's/CFLAGS+=-I/CFLAGS+=-fPIC -I/g' Makefile
sed -i 's/CXXFLAGS+=-I/CXXFLAGS+=-fPIC -I/g' Makefile
make
cp libvpx.a $HOME/vcpkg/installed/x64-linux/lib/
cd
```

### Build

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
git clone https://github.com/rustdesk/rustdesk
cd rustdesk
mkdir -p target/debug
wget https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so
mv libsciter-gtk.so target/debug
VCPKG_ROOT=$HOME/vcpkg cargo run
```

### Spremenite Wayland v X11 (Xorg)

RRustDesk ne podpira Waylanda. Preverite [to](https://docs.fedoraproject.org/en-US/quick-docs/configuring-xorg-as-default-gnome-session/) da konfigurirate Xorg kot privzeto sejo GNOME.

## Kako graditi z Dockerjem

Začnite s kloniranjem repozitorija in gradnjo vsebnika docker:

```sh
git clone https://github.com/rustdesk/rustdesk
cd rustdesk
docker build -t "rustdesk-builder" .
```

Nato vsakič, ko morate zgraditi aplikacijo, zaženite naslednji ukaz:

```sh
docker run --rm -it -v $PWD:/home/user/rustdesk -v rustdesk-git-cache:/home/user/.cargo/git -v rustdesk-registry-cache:/home/user/.cargo/registry -e PUID="$(id -u)" -e PGID="$(id -g)" rustdesk-builder
```

Upoštevajte, da lahko prva zgradba traja dlje, preden se odvisnosti predpomnijo, naslednje gradnje pa bodo hitrejše. Poleg tega, če morate podati različne argumente za ukaz build, lahko to storite na koncu ukaza v `<OPTIONAL-ARGS>` položaju. FČe bi na primer želeli zgraditi optimizirano izdajo, bi zagnali zgornji ukaz, ki bi mu sledil `--release`. TNastala izvedljiva datoteka bo na voljo v ciljni mapi v vašem sistemu in jo je mogoče zagnati z:

```sh
target/debug/rustdesk
```

Ali pa, če izvajate izvedljivo datoteko za izdajo:

```sh
target/release/rustdesk
```

Prepričajte se, da te ukaze izvajate iz korena skladišča RustDesk, sicer aplikacija morda ne bo mogla najti zahtevanih virov. Upoštevajte tudi, da drugi podukazi za tovor, kot je `install` ali `run` trenutno niso podprti prek te metode, saj bi namestili ali zagnali program znotraj vsebnika namesto gostitelja.

## Struktura datotek

- **[libs/hbb_common](https://github.com/rustdesk/rustdesk/tree/master/libs/hbb_common)**: video codec, config, tcp/udp wrapper, protobuf, fs funkcije za prenos datotek in nekatere druge pomožne funkcije
- **[libs/scrap](https://github.com/rustdesk/rustdesk/tree/master/libs/scrap)**: zajem zaslona
- **[libs/enigo](https://github.com/rustdesk/rustdesk/tree/master/libs/enigo)**: krmiljenje s tipkovnico/miško, specifično za platformo
- **[src/ui](https://github.com/rustdesk/rustdesk/tree/master/src/ui)**: GUI
- **[src/server](https://github.com/rustdesk/rustdesk/tree/master/src/server)**: avdio/odložišče/vhod/video storitve in omrežne povezave
- **[src/client.rs](https://github.com/rustdesk/rustdesk/tree/master/src/client.rs)**: zaženi peer povezavo
- **[src/rendezvous_mediator.rs](https://github.com/rustdesk/rustdesk/tree/master/src/rendezvous_mediator.rs)**: Komunicirajte s [rustdesk-server](https://github.com/rustdesk/rustdesk-server), počakajte na oddaljeno direktno (TCP hole punching) ali posredovano povezavo
- **[src/platform](https://github.com/rustdesk/rustdesk/tree/master/src/platform)**: koda, specifična za platformo
- **[flutter](https://github.com/rustdesk/rustdesk/tree/master/flutter)**: koda Flutter za mobilne naprave
- **[flutter/web/js](https://github.com/rustdesk/rustdesk/tree/master/flutter/web/js)**: Javascript za spletni odjemalec Flutter

## Snapshot

![image](https://user-images.githubusercontent.com/71636191/113112362-ae4deb80-923b-11eb-957d-ff88daad4f06.png)

![image](https://user-images.githubusercontent.com/71636191/113112619-f705a480-923b-11eb-911d-97e984ef52b6.png)

![image](https://user-images.githubusercontent.com/71636191/113112857-3fbd5d80-923c-11eb-9836-768325faf906.png)

![image](https://user-images.githubusercontent.com/71636191/135385039-38fdbd72-379a-422d-b97f-33df71fb1cec.png)
