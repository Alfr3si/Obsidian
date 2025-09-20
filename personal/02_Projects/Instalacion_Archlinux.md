---
id: Instalacion-ArchLinux
aliases:
  - Instalacion de Arch Linux
tags:
  - Arch Linux
  - Instalacion
---

# Instalacion de Arch Linux

## Descripcion

## La siguiente informacion es proporcionada desde multiples sitios en internet, pero sin duda alguna la informacion mas confiable esta desde su pagina ofcial en la Archwiki. [Archlinux](https://wiki.archlinux.org/index.php/Installation_Guide)

### Paso 1: Conectarse a Internet

```bash
    # esto es esencial para desbloquear la interfaz de red
    rfkill unblock wifi

    # iwctl es la herramienta para conectarse a la red
    iwctl

    # listar los dispositivos de red
    device list

    # escanear wlan0
    station wlan0 scan

    # listar las redes disponibles
    station wlan0 get-networks

    # conectarse a la red
    station wlan connect <nombre de la red>
    #ingresar la contraseña

    exit

    # probar laconexio con un ping por ejemplo
    ping google.com

```

### Paso 2: Particionar, formatear y montar el disco

```bash
    # con la herramienta fdisk y cfdisk es facil manipular el medio de almacenamiento
    fdisk -l

    cfdisk <nombre del disco> # ejemplo cfdisk /dev/sda
    # aqui ya depende de lo que necesitas en tu sistema

    # formatera partciciones

    mkfs.ext4 <particion> # ejemplo mkfs.ext4 /dev/sda1
    mkswap <particion> # ejemplo mkswap /dev/sda2
    mkfs.vfat -F32 <particion> # ejemplo mkfs.vfat -F32 /dev/sda3

    # montar las particiones
    swapon <particion> # ejemplo swapon /dev/sda2
    mount <particion> <directorio> # ejemplo mount /dev/sda1 /mnt
    mount -t vfat <particion> <directorio> # ejemplo mount -t vfat /dev/sda3 /mnt/boot/efi

    # recuerda crear tus carpetas en donde montaras las particiones en /mnt por ejemplo /mnt/home /mnt/boot/efi


```

### Paso 3: Instalacion

```bash
    # instalar el sistea base: kernel y linux-firmware

    pacstrap /mnt base linux-lts linux-firmare

    genfstab -U /mnt >> /mnt/etc/fstab

    arch-chroot /mnt /bin/bash

    # instalar al inicio estos programas

    pacman -S sudo nano networkmanager grub efibootmgr os-prober intel-ucode

    # Idioma del equipo
    nano /etc/locale.gen
    #descomenar #en_US.UTF-8 UTF-8

    locale-gen
    echo "LANG=en_US.UTF-8" > /etc/locale.conf

    # enlace simbolico a zona horaria
    ln -sf /usr/share/zoneinfo/America/Mexico_City /etc/localtime

    hwclock --systohc
    hwclock

    # instalar el grub en el sistema
    grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB

    #generar archivo de configuracion
    grub-mkconfig -o /boot/grub/grub.cfg

    # Agregar un hostname a la ruta /etc/hostname

    nano /etc/hostname # ejemplo: Shelter

    #  configurar /etc/hosts

    # ejemplo:

    # 127.0.0.1 localhost
    # ::1 localhost
    # 127.0.0.1     Shelter.localdomain     Shelter

    # Habilitar networkmanager
    systemctl enable NetworkManager.service

    #establecer contraseña de root
    passwd # ingresa la contraseña

    # Agregar usuario
    useradd -m -G wheel -s /bin/bash <nombre de usuario>
    passwd

    # desmontar
    umount /mnt

    # reinciar
    reboot

```

### Paso 4: Instalacion de programas

---

#### Estos son los programas que uso

- sof-firmware
- fastfetch
- pipewire
- firefox
- xorg
- xorg-xinit
- lsd
- unzip
- git
- btop
- yay
- base-devel
- curl
- pacman-contrib
- network-manager-applet
- libnotify
- nodejs
- npm
