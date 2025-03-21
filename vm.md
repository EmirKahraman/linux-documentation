# Virtual Machine Setup CentOS on Arch Host
## Step 1: Install QEMU/KVM and virt-manager
Open a terminal and install the required packages:
```bash
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils libvirt edk2-ovmf
```    
- `qemu`: The emulator and virtualizer.
- `virt-manager`: Graphical tool for managing VMs.
- `libvirt`: Daemon and tools for managing virtualization.
- `edk2-ovmf`: UEFI firmware for VMs (optional but recommended for modern OSes like CentOS).
Enable and start the libvirtd service:
```bash
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
```
Add your user to the libvirt and kvm groups:
```bash
sudo usermod -aG libvirt $(whoami)
sudo usermod -aG kvm $(whoami)
```
Log out and log back in for the changes to take effect.

## Step 2: Download the CentOS ISO
Go to the [CentOS download page](https://www.centos.org/download/) and download the latest CentOS ISO file (e.g., `CentOS-Stream-x86_64-latest-dvd1.iso`).
Save the ISO to a directory, e.g., `~/Downloads`.

## Step 3: Create a New VM Using virt-manager
1. Launch `virt-manager`:
```bash
virt-manager
```
2. Click **File > New Virtual Machine**.
3. Choose **Local install media (ISO image or CDROM)** and click **Forward**.
4. Browse to the CentOS ISO file you downloaded and select it. Click **Forward**.
5. Set the amount of RAM and number of CPUs for the VM. For CentOS:
   - **RAM**: At least 2 GB (4 GB or more recommended for engineering applications).
   - **CPUs**: 2 or more (depending on your host system’s capabilities).
   Click **Forward**.
6. Set the disk size for the VM. For CentOS:
   - At least 20 GB (50 GB or more recommended if you’re installing large engineering applications).
   Click **Forward**.
7. Name the VM (e.g., `CentOS-VM`) and click **Finish**.

## Step 4: Install CentOS in the VM
1. The VM will start automatically and boot from the CentOS ISO. Follow the on-screen instructions to install CentOS:
   - Choose your language and keyboard layout.
   - Select **Install Destination** and configure partitioning (use automatic partitioning if you’re unsure).
   - Set the root password and create a user account.
   - Begin the installation.
2. Once the installation is complete, reboot the VM. Remove the ISO from the virtual CDROM in `virt-manager` to boot from the installed system.

## Step 5: Configure the VM for Engineering Applications
Install necessary tools and dependencies in the CentOS VM:
```bash
sudo yum update
sudo yum install gcc gcc-c++ kernel-devel make
```
Install your engineering software (e.g., ANSYS, Abaqus, MATLAB) following the vendor’s instructions.

## Optional: Enable GPU Passthrough (for GPU-Accelerated Applications)
If your engineering applications require GPU acceleration (e.g., ANSYS), you can set up GPU passthrough to give the VM direct access to your GPU. This is more advanced and requires:
- A compatible CPU and motherboard (Intel VT-d or AMD-Vi support).
- A secondary GPU (if you want to use your primary GPU for the host system).
- Configuration of `vfio-pci` drivers and kernel parameters.

## Alternative: Use VirtualBox
1. Install VirtualBox:
   sudo pacman -S virtualbox virtualbox-host-dkms
2. Launch VirtualBox and create a new VM.
3. Follow the wizard to set up the VM, attaching the CentOS ISO as the installation media.
4. Install CentOS as you would on a physical machine.