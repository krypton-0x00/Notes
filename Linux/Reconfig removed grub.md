### **5. Reconfigure GRUB for Dual Boot**

After installing Windows, it will overwrite the bootloader. To restore GRUB and enable dual boot:

#### **Step 1: Boot into Arch Linux Live USB**

1. Boot into your Arch Linux installation USB.
2. Mount your root partition:
    
    bash
    
    Copy code
    
    `sudo mount /dev/<root-partition> /mnt`
    
3. If you have a separate `/boot` partition, mount it as well:
    
    bash
    
    Copy code
    
    `sudo mount /dev/<boot-partition> /mnt/boot`
    

#### **Step 2: Chroot into Your System**

1. Change root into your system:
    
    bash
    
    Copy code
    
    `arch-chroot /mnt`
    

#### **Step 3: Reinstall GRUB**

1. Reinstall GRUB to your EFI partition:
    
    bash
    
    Copy code
    
    `grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB`
    
2. Regenerate the GRUB configuration:
    
    bash
    
    Copy code
    
    `grub-mkconfig -o /boot/grub/grub.cfg`