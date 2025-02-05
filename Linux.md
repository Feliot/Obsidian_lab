#linux #server 

 ``` bash
echo "Información del CPU:"
lscpu

echo "Información de la Memoria RAM:"
free -h

echo "Información del Disco:"
lsblk

echo "Uso del Espacio en Disco:"
df -h

echo "Información del Hardware Detallada:"
sudo lshw -short

echo "Información del Sistema:"
uname -a

echo "Distribución del Sistema Operativo:"
cat /etc/os-release

echo "Información de la Tarjeta de Red:"
lspci | grep -i ethernet

echo "Todos los Dispositivos PCI:"
lspci

```
