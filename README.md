# Virtualization

This guide provides step-by-step instructions on setting up QEMU/KVM virtualization

---

## Table of Contents
- [Best Virtualization Solution in Windows and Linux](#best-virtualization-solution-in-windows-and-linux)
- [What is QEMU/KVM?](#what-is-qemukvm)
- [Virtualization Components in Linux](#virtualization-components-in-linux)
- [Practical Installation Steps](#practical-installation-steps)
- [DEMO](#demo)



---

### Best Virtualization Solution in Windows and Linux
####  For high performance and direct hardware access

- In Windows, Hyper-V is recommended due to its tight integration with Windows and support for features like Direct Access to hardware without emulation.

- In Linux, QEMU/KVM is recommended due to its tight integration with Linux and support for features like Direct Access to hardware without emulation.

---

### What is QEMU/KVM?

- QEMU (Quick EMUlator) is a generic and open-source machine emulator and virtualizer. It can emulate a wide range of hardware architectures and devices. When used as an emulator, QEMU can run software for one architecture on another, e.g., running ARM software on an x86 PC. However, in the context of virtualization, QEMU can use hardware acceleration to execute guest code directly on the host CPU, making it much faster than pure emulation.

- KVM (Kernel-based Virtual Machine) is a Linux kernel module that turns the Linux kernel into a hypervisor. KVM allows the host machine to run multiple isolated virtual environments, which are called virtual machines (VMs). Each VM has its own virtualized hardware (CPU, memory, storage, etc.).

  Together, QEMU and KVM provide a powerful and flexible virtualization solution on Linux.    QEMU handles the device emulation and virtual hardware, while KVM provides the core       virtualization capabilities and direct hardware access.

  
   
---

### Virtualization Components in Linux

##### KVM (Kernel-Based Virtual Machine):

-    A hypervisor that is built into the Linux kernel.
    Provides the core virtualization capabilities by leveraging hardware virtualization   extensions like Intel VT-x or AMD-V.
  

##### QEMU (Quick EMUlator):

-    A machine emulator and virtualizer.
    Uses hardware virtualization extensions to execute guest instructions directly on the host processor.
  

##### Libvirt:

-    A toolkit to manage virtualization platforms.
    Provides a consistent API to manage various virtualization technologies (including KVM and QEMU).Manages VM resources like CPU, storage, and network.
  

##### Virt-Manager (Virtual Machine Manager):

-    A graphical user interface for managing VMs.
    Simplifies the management of VMs by providing a user-friendly interface to interact with Libvirt.
    

---

### Practical Installation Steps

##### Update the package list:
```sh
sudo apt update
```

##### Check if your processor supports virtualization:
```sh
LC_ALL=C lscpu | grep Virtualization
```
- Your processor supports virtualization.If the output shows
- `<Virtualization: VT-x>` (for Intel) or
- `<Virtualization: AMD-V>` (for AMD), 
- If virtualization is disabled, you need to enable it in the BIOS settings.

##### Install QEMU, Virt-Manager, and other necessary packages:
```sh
sudo apt install qemu-system virt-manager ebtables
```
- qemu-system and Virt-Manager can utilize `Libvirt package` via API. therefore enabling and starting `libvirtd service` ensures that Libvirt's services are operational. (Don't worry if you didn't get it) 

##### Enable and start the Libvirt service:
```sh
sudo systemctl enable --now libvirtd
systemctl status libvirtd
```

##### Add your user to the Libvirt group (to use Virt-Manager without root):
```sh
sudo usermod -aG libvirt $USER
```
##### Reboot your system:
```sh
reboot
```

---

###  DEMO
##### Using virtual machine manager
- After rebooting, search for `Virtual Machine Manager` in your application menu.
- Open it, and it should connect to the Libvirt service without requiring root privileges.
- If you didn't add your user to the Libvirt group, you might need to run Virt-Manager with root privileges, which is less secure and not recommended.

##### Creating a virtal machine
- On the top left of Virtual Machine Manager window, Go to `File` -> `New Virtual Machine` -> `Local install media` -> `Select iso image` -> `Customize hardware settings` -> `Finish`

---

## Good Luck :) 
