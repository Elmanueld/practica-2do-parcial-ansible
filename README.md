# Laboratorio Ansible con Docker

## Objetivo

Crear dos contenedores Ubuntu 24.04 y administrarlos con Ansible mediante la conexión Docker. El playbook muestra el hostname y el sistema operativo, crea `/tmp/ansible-demo/info.txt`, genera los directorios `logs`, `backup` y `config` con un loop, y muestra un mensaje si el servidor usa Ubuntu.

## Requisitos

- Docker Desktop iniciado.
- Ansible instalado en Ubuntu/WSL.
- El comando `docker` disponible desde la misma terminal donde se ejecuta Ansible. La conexión `community.docker.docker` utiliza el Docker CLI para ejecutar comandos dentro de los contenedores.

En Ubuntu/WSL instala Ansible, el cliente Python de Docker y la colección necesaria:

```bash
sudo apt update
sudo apt install -y ansible docker.io
ansible-galaxy collection install -r requirements.yml
```

## Iniciar los contenedores

Desde esta carpeta, ejecuta:

```bash
docker compose up -d
docker ps
```

Instala Python 3 en los dos contenedores, necesario para que Ansible ejecute sus módulos:

```bash
docker exec server1 apt-get update
docker exec server1 apt-get install -y python3
docker exec server2 apt-get update
docker exec server2 apt-get install -y python3
```

## Comprobar conexión y ejecutar el playbook

```bash
ansible docker -i inventory.ini -m ping
ansible-playbook -i inventory.ini playbook.yml
```

La salida exitosa debe mostrar las tareas completadas para `server1` y `server2`, con `failed=0` en el resumen.

## Captura de ejecución

La siguiente captura muestra la salida de una ejecución exitosa del playbook:

![Ejecución exitosa del playbook de Ansible](evidencia-playbook.png)

## Detener los contenedores

```bash
docker compose down
```
