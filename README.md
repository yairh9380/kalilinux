
<h1 align="center">Guía de Configuración para la Virtualización de macOS con QEMU/KVM en Linux</h1>

**Sinopsis:**

Este documento proporciona una guía paso a paso para configurar tu sistema Linux para la virtualización de macOS utilizando QEMU/KVM. La virtualización de macOS en Linux permite a los usuarios ejecutar macOS dentro de una máquina virtual, lo que puede ser útil para desarrolladores, probadores de software y aquellos que necesitan acceder a aplicaciones específicas de macOS desde un entorno Linux.

**Índice de Contenido**

1.  Requisitos Previos
2.  Comprobación de la Virtualización
3.  Habilitación de la Virtualización Anidada (AMD)
4.  Instalación de Paquetes Requeridos
5.  Configuración de Libvirt
6.  Configuración de la Red KVM/QEMU

**1. Requisitos Previos:**

-   **Permisos de Root:** Necesitas ejecutar los comandos con privilegios de root (usando `sudo`).
-   **Sistema Basado en Debian/Ubuntu:** Esta guía está diseñada para sistemas operativos basados en Debian o Ubuntu.

**2. Comprobación de la Virtualización:**

-   Verifica si la virtualización está habilitada en tu CPU.

Bash

```
egrep -c '(vmx|svm)' /proc/cpuinfo

```

-   **Descripción:** Estos comandos verifican si tu CPU soporta virtualización por hardware (Intel VMX o AMD SVM).

**3. Habilitación de la Virtualización Anidada (AMD):**

-   Habilita la virtualización anidada para procesadores AMD.

Bash

```
sudo modprobe -r kvm_amd
sudo modprobe kvm_amd nested=1

```

-   **Descripción:** Estos comandos cargan el módulo KVM para AMD con soporte para virtualización anidada.

**4. Instalación de Paquetes Requeridos:**

-   Actualiza la lista de paquetes e instala los paquetes necesarios para la virtualización.

Bash

```
sudo apt update && sudo apt install -y qemu-system uml-utilities virt-manager git \
wget libguestfs-tools p7zip-full make dmg2img tesseract-ocr \
tesseract-ocr-eng genisoimage vim net-tools screen qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils

```

-   **Descripción:** Este comando instala todas las utilidades necesarias para la virtualización, incluyendo QEMU, libvirt y otras herramientas útiles.

**5. Configuración de Libvirt:**

-   Habilita e inicia el servicio libvirtd.

Bash

```
sudo systemctl enable libvirtd
sudo systemctl start libvirtd

```

-   Configura `libvirtd.conf` para la virtualización de macOS.

Bash

```
sudo sed -i 's/#unix_sock_group = "libvirt"/unix_sock_group = "libvirt"/' /etc/libvirt/libvirtd.conf
sudo sed -i 's/#unix_sock_rw_perms = "0770"/unix_sock_rw_perms = "0770"/' /etc/libvirt/libvirtd.conf

```

-   Reinicia el servicio libvirtd.

Bash

```
sudo systemctl restart libvirtd

```

-   **Descripción:** Estos comandos configuran el servicio libvirt para permitir la comunicación con QEMU y gestionar máquinas virtuales.

**6. Configuración de la Red KVM/QEMU:**

-   Asegura que la red predeterminada de KVM/QEMU esté habilitada.

Bash

```
sudo virsh -c qemu:///system net-autostart default
sudo virsh -c qemu:///system net-start default

```

¡Configuración de virtualización de macOS aplicada con éxito!
