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

> Creación de servidor. Con un Ubuntu server sin entorno gráfico.
* Usuario: sergio
* Contraseña: sergio

1. Antes de instalar la máquina virtual con el servidor debemos asignarle la red NAT que hemos creado antes:
![AsginaciónRedServidor](/img/img01_mvServidor.png)
2. En la instalación, seguimos todos los pasos e introducimos el nombre de usuario y la contraseña y cuando esté todo listo, tendremos el servidor creado:
![UsuarioYContraseña](/img/img02_mvServidor.png)
![ServidorCreadoYLogeado](/img/img03_mvServidor.png)
