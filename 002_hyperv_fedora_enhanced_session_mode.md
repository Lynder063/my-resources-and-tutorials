## Hyper-V, Fedora VM s Enhanced Session
- Hyper-V nabízí pro svoje VMs, vylepšenou session která nám povolí další možné funkce jako přávat lokalně zapojené SUB do VM nebo tiskárny

## Nejdříve standartně nainstalujeme Fedoru jako VM
## Spustíme tento script

```bash
#!/bin/bash

# Load the Hyper-V kernel module
if ! [ -f "/etc/modules-load.d/hv_sock.conf" ] || [ "$(cat /etc/modules-load.d/hv_sock.conf | grep hv_sock)" = ""  ]; then
  echo "hv_sock" | sudo tee -a /etc/modules-load.d/hv_sock.conf > /dev/null
fi

sudo dnf install -y hyperv-tools xrdp xrdp-selinux
sudo systemctl enable xrdp
sudo systemctl start xrdp

# Configure xrdp
sudo sed -i "/^port=3389/c\port=vsock://-1:3389" /etc/xrdp/xrdp.ini
sudo sed -i "/^use_vsock=.*/c\use_vsock=false" /etc/xrdp/xrdp.ini
sudo sed -i "/^security_layer=.*/c\security_layer=rdp" /etc/xrdp/xrdp.ini
sudo sed -i "/^crypt_level=.*/c\crypt_level=none" /etc/xrdp/xrdp.ini
sudo sed -i "/^bitmap_compression=.*/c\bitmap_compression=false" /etc/xrdp/xrdp.ini
sudo sed -i "/^max_bpp=.*/c\max_bpp=24" /etc/xrdp/xrdp.ini

sudo sed -i "/^X11DisplayOffset=.*/c\X11DisplayOffset=0" /etc/xrdp/sesman.ini
if [ "$(cat /etc/X11/Xwrapper.config | grep allowed_users=anybody)" = "" ]; then
  echo "allowed_users=anybody" | sudo tee -a /etc/X11/Xwrapper.config > /dev/null
fi
```

- A vypneme našem VM

## Host nastavení
- Musíme spustit tento příkaz v PowerShellu
```powershell
Set-VM -VMName <VM_NAME> -EnhancedSessionTransportType HvSocket
```

- `<VM_NAME>` nahradíme názvem naší VM co máme v Hyper-V manageru

## Spustíme VM a je vše hotové
