# Guía de Configuración para la Virtualización de macOS con QEMU/KVM en Linux

Este documento proporciona una guía para configurar tu sistema Linux para la virtualización de macOS utilizando QEMU/KVM.

**Requisitos Previos:**

* **Permisos de Root:** Necesitas ejecutar los comandos con privilegios de root (usando `sudo`).
* **Sistema Basado en Debian/Ubuntu:** Esta guía está diseñada para sistemas operativos basados en Debian o Ubuntu.


------------------

grep -E -c '(vmx|svm)' /proc/cpuinfo


$ egrep -c '(vmx|svm)' /proc/cpuinfo

 echo "Habilitando la virtualización anidada para procesadores AMD..."
   sudo modprobe -r kvm_amd
   sudo modprobe kvm_amd nested=1


   echo "Actualizando la lista de paquetes e instalando los paquetes requeridos..."

   sudo apt-get install qemu-system uml-utilities virt-manager git \
    wget libguestfs-tools p7zip-full make dmg2img tesseract-ocr \
    tesseract-ocr-eng genisoimage vim net-tools screen -y


    
   sudo apt update && sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager

   # "Habilitando e iniciando el servicio libvirtd..."
   sudo systemctl enable libvirtd
   sudo systemctl start libvirtd

  # "Configurando libvirtd.conf para la virtualización de macOS..."
   sudo sed -i 's/#unix_sock_group = "libvirt"/unix_sock_group = "libvirt"/' /etc/libvirt/libvirtd.conf
   sudo sed -i 's/#unix_sock_rw_perms = "0770"/unix_sock_rw_perms = "0770"/' /etc/libvirt/libvirtd.conf

   # "Reiniciando el servicio libvirtd..."
   sudo systemctl restart libvirtd

    "options kvm_amd nested=1" | sudo tee /etc/modprobe.d/kvm_amd.conf


   # "Asegurando que la red predeterminada de KVM/QEMU esté habilitada..."
   sudo virsh -c qemu:///system net-autostart default
   sudo virsh -c qemu:///system net-start default

   echo "Configuración de virtualización de macOS aplicada con éxito!"
