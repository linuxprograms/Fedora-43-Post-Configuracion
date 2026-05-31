# Fedora 43 - Post Configuración

Guía de configuración inicial para Fedora 43 orientada a estaciones de trabajo, desarrollo, virtualización y personalización del entorno.

## Contenido

* Optimización de DNF
* Actualización del sistema
* Instalación de paquetes esenciales
* Multimedia y codecs
* Zsh + Oh My Zsh + Powerlevel10k
* Herramientas de desarrollo
* VirtualBox
* Tipografías
* Flatpak
* Navegadores web

---

# 1. Optimización de DNF

Editar la configuración de DNF:

```bash
sudo nano /etc/dnf/dnf.conf
```

Agregar o modificar los siguientes parámetros:

```ini
fastestmirror=true
max_parallel_downloads=10
deltarpm=true
```

Actualizar el sistema:

```bash
sudo dnf -y update
```

---

# 2. Paquetes Esenciales

Instalar utilidades de compresión, gestión de archivos y herramientas de sistema:

```bash
sudo dnf install -y \
xz \
bzip2 \
unrar \
p7zip \
lbzip2 \
arj \
lzma \
lzop \
cpio \
webp-pixbuf-loader \
java \
unar \
file-roller \
wget \
curl
```

---

# 3. Multimedia y Codecs

Instalar bibliotecas multimedia y herramientas de reproducción:

```bash
sudo dnf install -y \
xine-lib-extras \
xine-lib-extras-freeworld \
gstreamer1-plugins-base-tools \
vlc \
libdvdread \
libdvdnav \
lsdvd \
gstreamer1-vaapi \
libva-utils \
libvdpau-va-gl \
ImageMagick
```

Instalar plugins GStreamer:

```bash
sudo dnf install -y \
gstreamer1-plugins-{bad-*,good-*,base} \
gstreamer1-plugin-openh264 \
gstreamer1-libav \
--exclude=gstreamer1-plugins-bad-free-devel
```

---

# 4. Zsh + Oh My Zsh + Powerlevel10k

## Instalar dependencias

```bash
sudo dnf install -y git zsh util-linux-user
```

## Instalar Oh My Zsh

```bash
git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh

cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

cp ~/.zshrc ~/.zshrc.orig
```

## Establecer Zsh como shell predeterminado

```bash
sudo chsh -s /bin/zsh <usuario>
```

## Instalar Powerlevel10k

```bash
git clone --depth=1 \
https://github.com/romkatv/powerlevel10k.git \
~/.oh-my-zsh/custom/themes/powerlevel10k
```

Editar:

```bash
nano ~/.zshrc
```

Configurar:

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Aplicar cambios:

```bash
source ~/.zshrc
```

---

# 5. Herramientas de Desarrollo

Instalar compiladores, cabeceras y utilidades de construcción:

```bash
sudo dnf install -y \
kernel-headers \
kernel-devel \
dkms \
gcc \
make \
perl \
cmake \
tlp \
tlp-rdw
```

---

# 6. VirtualBox

## Habilitar RPM Fusion

```bash
sudo dnf install \
https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-43.noarch.rpm \
https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-43.noarch.rpm
```

## Instalar VirtualBox

```bash
sudo dnf install -y akmod-VirtualBox VirtualBox
```

## Dependencias

```bash
sudo dnf install -y \
kernel-devel \
kernel-headers \
gcc \
make \
perl \
elfutils-libelf-devel
```

## Construcción de módulos

```bash
sudo akmods --force

sudo modprobe vboxdrv
```

## Agregar usuario al grupo VirtualBox

```bash
sudo usermod -aG vboxusers $USER
```

Verificar:

```bash
lsmod | grep vbox
```

Reiniciar el sistema:

```bash
sudo reboot
```

---

# 7. Instalación de Tipografías

```bash
sudo dnf install -y \
curl \
cabextract \
xorg-x11-font-utils \
fontconfig
```

---

# 8. Activar Flatpak

```bash
flatpak remote-add --if-not-exists flathub \
https://dl.flathub.org/repo/flathub.flatpakrepo
```

Verificar:

```bash
flatpak remotes
```

---

# 9. Navegadores Web

## LibreWolf

```bash
sudo dnf config-manager addrepo \
--from-repofile=https://repo.librewolf.net/librewolf.repo

sudo dnf install -y librewolf
```

## Brave Browser

```bash
sudo dnf install -y dnf-plugins-core

sudo dnf config-manager addrepo \
--from-repofile=https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo

sudo dnf install -y brave-browser
```

---

# Verificación Final

Comprobar versiones instaladas:

```bash
cat /etc/fedora-release

zsh --version

git --version

flatpak --version

VBoxManage --version
```

---

## Licencia

Este repositorio contiene una recopilación de configuraciones y herramientas para Fedora 43 destinadas a simplificar el proceso de post-instalación y preparación de entornos de trabajo.
