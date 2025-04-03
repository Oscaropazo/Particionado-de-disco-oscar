# Práctica de Particionado de Discos en Ubuntu 22.04

- Montar discos en VirtualBox
  
![imagen](/Imagenes/1.png)

![imagen](/Imagenes/2.png)


---

## 1. Particionado MBR

### **1.1. Verificar discos disponibles**
```bash
lsblk
```
- Con el comando lsblk comprobamos que tenemos todos los discos necesarios para la practica

### **1.2. Crear particiones con fdisk**
```bash
sudo fdisk /dev/sdb
```
- Creamos particiones en el disco sdb.

![imagen](/Imagenes/3.png)
**Comandos dentro de fdisk:**
1. **`n`**  Nueva partición
2. **`p`**  Primaria
   - **Partición 1**: 20 GiB
   - **Partición 2**: 10 GiB
   - **Partición 3**: 15 GiB
3. **`n`**  Nueva partición extendida
4. **`n`**  Crear particiones lógicas dentro de la extendida
   - **Partición 5**: 10 GiB
   - **Partición 6**: 15 GiB
5. **`w`**  Guardar cambios y salir

![imagen](/Imagenes/4.png)

![imagen](/Imagenes/5.png)

![imagen](/Imagenes/6.png)

**Comprobamos que se hayan creado las particiones:**
![imagen](/Imagenes/7.png)

### **1.3. Formatear particiones**
```bash
for part in 1 2 3 4 5 6 7; do
```
![imagen](/Imagenes/8.png)
**Comprobamos que se hayan creado las particiones:**

- Con el comando lsblk comprobamos que todas las particiones esten creadas.


### **1.4. Crear puntos de montaje**
```bash
sudo mkdir -p /media/sdb{1,2,3,5,6,7}
```
- con el comando sudo mkdir -p /media/sdb{1,2,3,5,6,7} creamos los puntos de montaje para todas las particiones.

![imagen](/Imagenes/9.png)
![imagen](/Imagenes/10.png)

**Comprobamos que se hayan creado las particiones:**

![imagen](/Imagenes/23.png)

### **1.5. Agregar a /etc/fstab**
Obtener UUIDs:
```bash
blkid
```
![imagen](/Imagenes/12.png)
Editar `/etc/fstab`:
```bash
sudo nano /etc/fstab
```
- Editamos el fichero /etc/fstab y agregamos los UUID de cada particion

Agregar estas líneas (reemplazando los UUID reales):
```plaintext
UUID=xxxxx /media/sdb1 ext4 defaults 0 2
UUID=xxxxx /media/sdb2 ext4 defaults 0 2
UUID=xxxxx /media/sdb3 ext4 defaults 0 2
UUID=xxxxx /media/sdb5 ext4 defaults 0 2
UUID=xxxxx /media/sdb6 ext4 defaults 0 2
UUID=xxxxx /media/sdb7 ext4 defaults 0 2
```
![imagen](/Imagenes/13.png)
Montar las particiones:
```bash
sudo mount -a
```
![imagen](/Imagenes/14.png)
### **1.6. Crear archivos en cada partición**

- Creamos un archivo con un mensage en cada particion.

```bash
echo "Soy la partición 1 MBR y tengo un tamaño de 20GiB" | sudo tee /media/sdb1/info.txt
echo "Soy la partición 2 MBR y tengo un tamaño de 10GiB" | sudo tee /media/sdb2/info.txt
echo "Soy la partición 3 MBR y tengo un tamaño de 15GiB" | sudo tee /media/sdb3/info.txt
echo "Soy la partición 5 MBR y tengo un tamaño de 10GiB" | sudo tee /media/sdb5/info.txt
echo "Soy la partición 6 MBR y tengo un tamaño de 15GiB" | sudo tee /media/sdb6/info.txt
```
![imagen](/Imagenes/11.png)
---

## 2. Particionado GPT

### **2.1. Crear particiones con gdisk**
- Con el comando gdisk creamos las particiones GPT en el disco sdc.

```bash
sudo gdisk /dev/sdc
```

**Comandos dentro de gdisk:**
1. **`n`** → Nueva partición
   - **Partición 1**: 20 GiB
   - **Partición 2**: 15 GiB
2. **`w`** → Guardar cambios y salir
![imagen](/Imagenes/15.png)

**Comprobamos que se hayan creado las particiones:**
![imagen](/Imagenes/16.png)

### **2.2. Formatear particiones**

- Le damos formato a las particiones con el comando mkfs.ext4

```bash
sudo mkfs.ext4 /dev/sdc1
sudo mkfs.ext4 /dev/sdc2
```
![imagen](/Imagenes/17.png)
![imagen](/Imagenes/18.png)
### **2.3. Crear puntos de montaje**

- Creamos el punto de montaje para ambas particiones con el comando mkdir -p.

```bash
sudo mkdir -p /media/sdc{1,2}
```
![imagen](/Imagenes/19.png)
### **2.4. Agregar a /etc/fstab**
Obtener UUIDs:
```bash
blkid
```
![imagen](/Imagenes/21.png)
- Editamos el fichero /etc/fstab.
```bash
sudo nano /etc/fstab
```

- Agregamos los UUID de las particiones.
```plaintext
UUID=xxxxx /media/sdc1 ext4 defaults 0 2
UUID=xxxxx /media/sdc2 ext4 defaults 0 2
```
Montar las particiones:
```bash
sudo mount -a
```
![imagen](/Imagenes/22.png)
### **2.5. Crear archivos en cada partición**
```bash
echo "Soy la partición 1 GPT y tengo un tamaño de 20GiB" | sudo tee /media/sdc1/info.txt
echo "Soy la partición 2 GPT y tengo un tamaño de 15GiB" | sudo tee /media/sdc2/info.txt
```
![imagen](/Imagenes/20.png)
