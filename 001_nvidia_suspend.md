# Oprava `suspend` v počítačích co mají Nvidia nebo hybrid setup
- Tato chyba je způsobena drivery a deamony Nvidia takže stačí zakázat a vypnout určité deamony 

```bash
sudo systemctl stop nvidia-suspend.service
sudo systemctl stop nvidia-hibernate.service
sudo systemctl stop nvidia-resume.service

sudo systemctl disable nvidia-suspend.service
sudo systemctl disable nvidia-hibernate.service
sudo systemctl disable nvidia-resume.service
```

## Smazání systemd scriptu 

- Ale nejdříve to zálohujeme script kdybychom ho někdy potřebovali.

```bash
sudo cp /lib/systemd/system-sleep/nvidia /lib/systemd/system-sleep/nvidia.bac
```

- A provedeme smazání
```bash
sudo rm /lib/systemd/system-sleep/nvidia
```

## A restartujeme systém 

- Chcete-li ozkoušet tak můžeme vyvolat state `suspend` pomocí systemd

```bash
sudo systemctl suspend
```
