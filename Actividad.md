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
## Conexión mediante usuario y contraseña.
### Desde el equipo **Anfitrión**.
> Conéctate  desde el equipo anfitrión a Servidor.

Para ello debemos abrir la consola en nuestro equipo afitrión y escribir el siguiente comando:
```bash
ssh sergio@127.0.0.1 -p 2207
```
![conexionAnfiServi](/img/img01_anfitrion.png)
 
 > Para comprobar que estás en el servidor, crea un archivo de texto llamado *servidor.txt*.

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

### Desde el equipo Casa
> Conéctate desde Casa a Servidor.

Para ello debemos abrir la consola en Casa y escribir el siguiente comando:
```bash
ssh sergio@10.0.2.7 -p 22
```
![conexionCasaServidor](/img/img01_conexionCasaServidor.png)
> Para comprobar que estás en el servidor, crea un archivo de texto llamado *casa.txt*.

Dentro de la consola de casa y entrado en el servidor a través de la instrucción anterior, creamos un archivo de texto con el siguiente comando:
 ```bash
touch casa.txt
```
![archivoTextoEnServidor](/img/img02_conexionCasaServidor.png)
Si nos vamos a nuestro Servidor, hacemos la comprobación para ver si hemos creado ese archivo correctamente. Para ello usamos el comando:
```bash
ls -la 
```
![archivoTextoEnServidor](/img/img03_conexionCasaServidor.png)
> Desconéctate del servidor

Para desconectarnos de un servidor debemos introducir el siguiente comando:
```bash
exit
```
![desconexionServidor](/img/img04_conexionCasaServidor.png)

## Conexión mediante claves asimétricas.
### Equipo Casa:
> Crea un par de claves con el protocolo ed25519 y con el nombre claves_trabajo.

Para crear una clave con el protocolo ed25519 debemos introducir en Casa el siguiente comando:
```bash
ssh-keygen -t ed25519
```
Y dentro debemos introducie el nombre:
![claveAsimetrica](/img/img01_claveAsimetrica.png)
Para comprobar que están creada podemos introducir el comando:
```bash
ls -la
```
![claveAsimetrica](/img/img02_claveAsimetrica.png)
> Exporta la clave al Servidor.

Para ello, dentro de la consola de carmen debemos introducir el siguiente comando, el cual le indicamos qué queremos exportar junto con la IP del Servidor. La primera vez que hagamos esto, nos pedirá la contraseña del Servidor:
```bash
ssh-copy-id -p 22 -i ./clave_trabajo.pub sergio@10.0.2.7
```
![claveAsimetrica](/img/img03_claveAsimetrica.png)

> Conéctate desde Casa a Servidor mediante la clave clave_trabajo:

Para ello introducimos el siguiente código y ya no sería necesario introducir la contraseña:
```bash
ssh -i ./clave_trabajo sergio@10.0.2.7
```
![claveAsimetrica](/img/img04_claveAsimetrica.png)

> Para comprobar que estás en el servidor, crea un archivo de texto llamado *claves.txt*

Dentro de la consola de casa y entrado en el servidor a través de la instrucción anterior, creamos un archivo de texto con el siguiente comando:
 ```bash
touch claves.txt
```
![claveAsimetrica](/img/img05_claveAsimetrica.png)
Si nos vamos a nuestro Servidor, hacemos la comprobación para ver si hemos creado ese archivo correctamente. Para ello usamos el comando:
```bash
ls -la 
```
![claveAsimetrica](/img/img06_claveAsimetrica.png)
> Desconéctate del servidor

Para desconectarnos de un servidor debemos introducir el siguiente comando:
```bash
exit
```
![claveAsimetrica](/img/img07_claveAsimetrica.png)

# Apache
> Conéctate desde Casa a Servidor mediante la clave_trabajo

Para ello entramos desde Casa a Servidor con el siguiente comando:
```bash
ssh -i ./clave_trabajo sergio@10.0.2.7
```
![apache](/img/img01_apache.png)
> Instala en el equipo Servidor el servidcio apache2.
Una vez dentro del servidor (gracias al paso anterior), instalamos el apache2 con el siguiente comando:
```bash
sudo apt install apache2
```
> Comprueba que el servicio apache2 está activo y en funcionamiento.

Con el comando siguiente comprobamos el estado de apache2:
```bash
sudo systemctl status apache2
```
![apache](/img/img02_apache.png)

> Añade la regla en el cortafuegos para apache2.

Para ello hay que seguir los siguientes pasos introduciendo los siguientes comandos:
1. Ver qué aplicaciones hay disponibles:
```bash
sudo ufw app list
```
2. Añadir la regla en el cortafuegos:
```bash
sudo ufw allow Apache
```
![apache](/img/img03_apache.png)

> Añade en VirtualBox el redireccionamiento de puertos para poder acceder desde el equipo anfitrión al Servidor con el servicio apache2.

Cuando agregremos las reglas de envío, debemos asignarle un nombre apropiado, ambas con IP anfitrión 127.0.0.1, puerto anfitrión 808x (la x es un número el cual es recomendable que termine en el mismo número  que termina nuestra IP de invitado), IP invitado (10.0.2.x), el cual debe ser igual que las IP que hemos mirado antes y puerto invitado (80).
![apache](/img/img04_apache.png)

# SCP
### Equipo Casa
> Crea en el equipo Casa una página web index.html como la siguiente con tu nombre y apellidos
![scp](/img/img01_scp.png)

Para hacer esto, debemos introducir el siguiente comando:
```bash
nano index.html
```
Escribimos dentro el html con los datos solicitados lo guardamos y le damos a salir.
![scp](/img/img02_scp.png)

>Copia el archivo anterior en Servidor, en la carpeta /var/www/html

Una vez creado el index.html lo que haremos será copiar el archivo en Servidor con el siguiente comando. La virgulilla y la barra se ponen para indicarle que lo meta en el directorio principal ya que si pones destino no te deja por tema de permisos:
```bash
scp -i ./clave_trabajo ./index.html sergio@10.0.2.7:~/
```
![scp](/img/img03_scp.png)

Ahora que lo tenemos copiado en Servidor, entramos en el servidor para moverlo a la carpeta que nos indican /var/www/html:
* Entramos en el servidor:
```bash
ssh -i ./clave_trabajo sergio@10.0.2.7
```
* Movemos el archivo a la carpeta:
```bash
sudo cp index.html /var/www/html/
```
![scp](/img/img04_scp.png)

Ya dentro, podemos salir del Sevidor.

>Desde Casa abre un navegador y prueba que puedes acceder a la web.

Con Casa sólo tenemos que abrir el navegador e introducir la URL que hemos establecido con el servidor, es decir:

10.0.2.7 --> IP de anfitrión para el servidor

![navegador](/img/img03_navegador.png)

Y ya vemos que estamos conectados desde nuestro equipo Casa al servidor:

![navegador](/img/img04_navegador.png)


### Equipo Anfitrión
> Desde el equipo anfitrión abre un navegador web y prueba que puedes acceder a la web.

Con el equipo anfitrión sólo tenemos que abrir el navegador e introducir la URL que hemos establecido con el servidor, es decir:

127.0.0.1 --> IP de anfitrión

8087 --> Puerto anfitrión

![navegador](/img/img01_navegador.png)

127.0.0.1:8087

Y ya vemos que estamos conectados desde nuestro equipo anfitrión al servidor:

![navegador](/img/img02_navegador.png)

