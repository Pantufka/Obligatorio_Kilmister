# README de los playbooks configurados

Este repositorio, Obligatorio_Kilmister, contiene dos playbooks de Ansible diseñados para:

1. Configurar un servidor NFS en CentOS (nfs_setup.yml)
- El servidor NFS esté instalado
- Se asegure que el servicio NFS esté iniciado y funcionando
- El firewall permita conexiones al puerto 2049
- Exista el directorio /var/nfs_shared, que pertenece al usuario/grupo nobody/nobody y tiene permisos 777
- El directorio está compartido por NFS.
- Debe haber un handler que actualice relea el archivo /etc/exports si este cambia.

2. Aplicar hardening en Ubuntu (hardening.yml)

- Actualizar todos los paquetes
- Que esté habilitado ufw, bloqueando todo el tráfico entrante y permitiendo solo ssh.
- Que solo se pueda hacer login con clave pública, y que root no pueda hacer login.
- Que esté instalado fail2ban y bloquee intentos fallidos de conexión SSH. El servicio debe quedar habilitado y activado.
- Debe haber un handler que reinicie el sistema si se actualizan paquetes.
- Debe haber un handler que reinicie ssh si cambia la configuración.

---

## Requisitos

- Tener un equipo Controller
- Generar las claves de SSH en Controller

```bash
ssh-keygen
```

- Haber copiado los ID de SSH a los servidores que se van a gestionar

```bash
ssh-copy-id sysadmin@192.168.1.11
ssh-copy-id sysadmin@192.168.1.12
```

- Instalar Ansible

```bash
sudo dnf install ansible-core
```

- Acceder al directorio collections e instalar el requirements.yml para poder correr los playbooks correctamente

```bash
ansible-galaxy collection install -r requirements.yaml
```

- Verificar que el contenido de inventory.ini en el directorio inventories, sea igual que en los servidores que se vab a probar

```bash
vim inventory.ini
```

---

## Ejecución

Para poder correr el primer playbook (nfs_setup.yml) hay que acceder al directorio playbooks y una vez dentro ejecutar el siguiente comando

```bash
ansible-playbook -i ../inventories/inventory.ini nfs_setup.yml -K
```

El resultado de correr el comando se puede ver en el directorio documentos dentro del archivo registros_de_ejecucion.txt. Dentro de él, también esta el resultado que sale al volver a correr playbook pudiendose ver las diferencias entre las cantidades de Ok y Changed.

Para poder correr el segundo playbook (nfs_setup.yml) también hay que estar en directorio playbooks y se ejecuta el siguiente comando

```bash
ansible-playbook -i ../inventories/inventory.ini hardening.yml -K
```
Al igual que en el caso anterior, el resultado de correr el playbook, se encuentra en el mismo archivo registros_de_ejecucion.txt, pudiendose ver el resultado de la primera ejecución y de la ejecución ya habiendolo corrido previamente
