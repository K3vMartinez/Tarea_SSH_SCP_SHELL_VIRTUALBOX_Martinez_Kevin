# Tarea SSH-SCP-Shell-VirtualBox
## Kevin Martínez Martínez

# VirtualBox
> Crea una red NAT con dos máquinas virtuales (MV):

Lo primero que debemos hacer en la máquina virtual es crear una red NAT.
1. En las opciones de las herramientas clickamos en Red:
![OpcionesHerramientas](/img/img01_vbox.png)
2. Clickamos en crear:
![CrearRed](/img/img02_vbox.png)
3. Abajo del todo le cambiamos el nombre y le damos a aplicar:
![CambioNombre](/img/img03_vbox.png)
4. Ya tenemos la red NAT:
![redNATCreada](/img/img04_vbox.png)

> Creación de servidor (Servidor). Con un Ubuntu server sin entorno gráfico.
* Usuario: sergio
* Contraseña: sergio

1. Antes de instalar la máquina virtual con el servidor debemos asignarle la red NAT que hemos creado antes:
![AsginaciónRedServidor](/img/img01_mvServidor.png)
2. En la instalación, seguimos todos los pasos e introducimos el nombre de usuario y contraseña. Cuando esté todo listo, tendremos el servidor creado:
![UsuarioYContraseña](/img/img02_mvServidor.png)
![ServidorCreadoYLogeado](/img/img03_mvServidor.png)

> Creación de entorno (Casa). Con un Lubuntu con el entorno gráfico por defecto.
* Usuario: carmen
* Contraseña: carmen

1. Antes de instalar la máquina virtual con el servidor debemos asignarle la red NAT que hemos creado antes:
![AsignaciónRedCasa](/img/img01_mvCasa.png)
2. En la instalación seguimos todos los pasos e introducimos el nombre de usuario y contraseña. Cuando esté listo, tendremos el entorno gráfico creado:
![UsuarioYContraseña](/img/img02_mvCasa.png)
![EntornoCreadoYLogeado](/img/img03_mvCasa.png)

Mi recomendación es comprobar que las máquinas virtuales tengan conexión a Internet con el siguiente comando:
```bash
curl google.es
```
# Linux Shell 

> Instala o comprueba que ya estén instalados los servicios: openssh-server y ufw.

Debemos realizar los pasos tanto en el Servidor como en Casa.
1. Instalamos el ssh:
```bash
sudo apt install openssh-server
```
2. Comprobamos estado del ssh:
```bash
sudo systemctl status ssh
```
![estadoSSHServidor](/img/img01_sshServidor.png)
![estadoSSHCasa](/img/img01_sshCasa.png)
3. Comprobamos estado de ufw:
```bash
sudo systemctl status ufw
```
![estadoUfwServidor](/img/img02_ufwServidor.png)
![estadoUfw](/img/img02_ufwCasa.png)

> Añade la regla en el cortafuegos para ssh.

1. Ver qué aplicaciones hay disponibles:
```bash
sudo ufw app list
```
2. Añadir la regla en el cortafuegos:
```bash
sudo ufw allow OpenSSH
```
![cortafuegosUfwServidor](/img/img03_ufwServidor.png)
![cortafuegosUfwServidor](/img/img03_ufwCasa.png)

> Añade en VirtualBox el redireccionamiento de puertos para poder acceder desde el equipo anfitrión a Servidor y Casa con el servicio ssh.
1. Antes de nada, debemos mirar la IP que tiene asignada ambas máquinas virtuales con el siguiente comando:
```bash
ip a
```
En el servidor podemos ver que tengo esta IP:
![IPServidor](/img/img01_puertos.png)
En el de casa tenemos esta IP:
![IPCasa](/img/img02_puertos.png)
2. Ahora lo que debemos hacer es ir a la configuración de herramientas de VirtualBox, seleccionar la redNAT que hemos creado antes (RedNAT_Despliegue), seleccionamos la pestaña de reenvío de puertos y le damos al icono de abajo a la derecha para agregar una nueva regla de envío:
![AgregarPuertos](/img/img03_puertos.png)
Cuando agregremos las reglas de envío, debemos asignarle un nombre apropiado, ambas con IP anfitrión 127.0.0.1, puerto anfitrión 220x (la x es un número el cual es recomendable que termine en el mismo número  que termina nuestra IP de invitado), IP invitado (10.0.2.x), el cual debe ser igual que las IP que hemos mirado antes y puerto invitado (22).
![PuertosAgregados](/img/img04_puertos.png)

# SSH
## Desde el equipo **Anfitrión**.
> Conéctate  desde el equipo anfitrión a Servidor.

Para ello debemos abrir la consola en nuestro equipo afitrión y escribir el siguiente comando:
```bash
ssh sergio@127.0.0.1 -p 2207
```
![conexionAnfiServi](/img/img01_anfitrion.png)
 
 > Para comprobar que estás en el servidor, crea un archivo de texto llamado *servidor.txt*

 Dentro de la consola de nuestro anfitrión y entrado en el servidor a través de la instrucción anterior, creamos un archivo de texto con el siguiente comando:
 ```bash
touch servidor.txt
```
![archivoTextoEnServidor](/img/img02_anfitrion.png)

Si nos vamos a nuestro Servidor, hacemos la comprobación para ver si hemos creado ese archivo correctamente. Para ello usamos el comando:
```bash
ls -la 
```
![archivoTextoEnServidor](/img/img03_anfitrion.png)
> Desconéctate del servidor

Para desconectarnos de un servidor debemos introducir el siguiente comando:
```bash
exit
```
![desconexionServidor](/img/img04_anfitrion.png)

## Desde el equipo Casa








2. Actualizamos la máquina:
```bash
sudo apt update
```
3. Instalamos el apache2:
```bash
sudo apt install apache2
```
4. Miramos si está instalado:
```bash
sudo ufw app list
```
5. Damos permiso a Apache:
```bash
sudo ufw allow Apache
```

