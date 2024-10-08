# Configuración del entorno

Independientemente del sistema instalado y actualizado, **Kali Linux** o **Parrot OS**, e independientemente de la plataforma de virtualización, **VMware** o **VirtualBox**, se deben instalar algunas herramientas adicionales y luego poder *tunear* un poco el sistema.

La parte de *tunear* es muy personal, lo mismo que las herramientas adicionales a instalar, y cada persona elige que herramientas prefiere y como disponerlas en la máquina. En este documento describimos un tunning muy básico.

## Instalar herramientas

Vamos a instalar una serie de herramientas útiles:

* **Terminator**: Es un gestor de terminal muy versátil que permite, entre otras cosas dividir la terminal en varias terminales.
* **Sublime Text**: Editor de texto muy completo.
* **Locate**: Permite localizar ficheros en el sistema de forma rápida.
* **Aptitude**: Utilidad que permite buscar herramientas en los repositorios de software.
* **Cherrytree**: Programa para tomar notas de pentesting, muy versátil.
* **VS Code**: IDE de programación muy util para trabajar con **markdown** y por su integración con github.

Si se está usando **Parrot OS** la herramienta **terminator** ya estará instalada por lo que aunque le digamos que se instale no hará nada.

De todas las herramientas hay dos que no están en los repositorios oficiales y hay que añadir los repositorios.

### Agregar repositorios de Sublime Text

Para agregar el repositorio abrimos una terminal y escribimos los siguientes comandos:

```bash
sudo apt install software-properties-common apt-transport-https curl wget gpg ca-certificates -y
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt update
```
### Agregar repositorios de VS Code

Para agregar el repositorio abrimos una terminal y escribimos los siguientes comandos:

```bash
sudo apt install software-properties-common apt-transport-https curl wget gpg ca-certificates -y
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt update
```
La primera linea la podemos obviar si hemos agregado el repositorio anterior.

### Instalar programas 

Ahora con los repositorios instados y actualizados podemos instalar los programas indicados ejecutando el siguiente comando.

```bash
sudo apt install terminator aptitude sublime-text locate code cherrytree -y 
```

## Configuración del entorno.

A continuación se proponen unas opciones de *tuneo* básica. Pero reiteramos que esto es muy personal de cada usuario.

### Accesos directos en la barra superior

Podemos agregar las aplicaciones mas utilizadas a la barra superior.

![](../images/kali19.png)

Pone el icono al final, pero se puede mover.

![](../images/kali20.png)

Podemos eliminar los iconos que no se usen, la terminal y el editor por defectos, y añadir los que queramos. 

Es este caso se añaden accesos para terminator, Sublime Text, Cherrytree, VSCode y Burpsuite.

![](../images/kali21.png)

### Creación de nuestros propios plugins en la barra superior

Cuando se resuelve máquinas es muy importante tener muy a mano:

* Nuestra dirección IP, para cuando creemos SHELLs reversas no estar haciendo **ifconfig** continuamente.
* La dirección ip de la maquina objetivo, para poder tenerla a mano cuando enumeremos o accedamos a ella.

Vamos a crear un algunas utilidades para poder visualizar siempre ambas IPs.

### Utilidad para visualizar nuestra IP.

Nos creamos primero un script que llamaremos **ethernet_status.sh** y que guardaremos en una ruta conocida, por ejemplo **/home/kali/.config/personal/scripts/ethernet_status.sh**. El script sería este:

```bash
#!/bin/sh

echo "          Local: $(/usr/sbin/ifconfig eth0 | grep "inet " | awk '{print $2}')" 
```

Lo importante es el nombre de la interface de red. Normalmente en Kali es **eth0** y en Parrot es **ens33**. También puede variar si se usa VMware en la creación o VirtualBox. Para ver el nombre de nuestra interface en kali abrimos una terminal y ejecutamos **ifconfig**.

![](../images/kali.png)

Creamos el script y lo guardamos en la ruta que queramos.

![](../images/kali24.png)

Ahora es importante que le demos permisos de ejecución con el comando:

```bash
chmod +s ethernet_status.sh
```

Y que lo probemos:

```bash
./ethernet_status.sh
```

![](../images/kali25.png)

Ahora lo añadimos al panel. Para ello en algún punto vacío de la barra superior, en adelante panel, con el botón derecho creamos un nuevo item:

![](../images/kali26.png)

filtramos para poder visualizar y elegir la opción **Generic Monitor**.

![](../images/kali27.png)

Damos doble clic a Generic Monitor y cerramos el panel. En la parte izquierda del panel se añade una opción **(genmon)XXX**. Lo primero que hacemos es moverla a donde queremos.

![](../images/kali28.png)



Ahora, con el botón derecho encima del monitor añadido elegimos **Properties**.

![](../images/kali30.png)

Se nos abre este panel:

![](../images/kali31.png)

En el cuadro **Command** hay que escribir la ruta del script creado, **/home/kali/.config/personal/scripts/ethernet_status.sh**, o bien navegar hasta el pulsando **...**.

Desactivamos el check de Label. Ya hemos puesto nosotros en el script una label.

Como nuestra IP no cambia no es necesario que le demos tiempo de comprobación. Podemos cambiar los 30 segundos por defecto a 3600 para que lo compruebe cada hora.

Podemos cambiar la fuente también.

![](../images/kali32.png)

Y salvamos. Ya deberíamos ver nuestra IP.

![](../images/kali33.png)

### Utilidad para visualizar la IP de una máquina víctima.

En este caso nop tenemos nada que detecte una IP víctima, sino que en algún momento la averiguaremos.

Lo que haremos es lo siguiente:

1. Crear un fichero que llamaremos **objetivo** en una ruta conocida como puede ser **/home/kali/.config/personal/objetivo**.
2. Crear una utilidad que lea el contenido de este fichero y lo muestro por pantalla. Este será el script que añadiremos en nuestro nuevo **Generic Monitor**.
3. Crear una utilidad en nuestro fichero de configuración de la bash para poder escribir en el fichero.
4. Crear una utilidad que limpie el fichero cuando no estemos ya trabajando con el objetivo.

#### Creación de fichero.

Siempre en una terminal:

```bash 
touch /home/kali/.config/personal/objetivo
```

![](../images/kali34.png)

#### Creación de script que lee fichero.

Igual que en el script anterior creamos el script, que llamaremos **objetivo_status.sh* en la misma ruta ** /home/kali/.config/personal/scripts**. Le daremos permiso de ejecución y lo ejecutaremos. PAra ello abriremos el fichero objetivo y escribimos algo. Lo podemos hacer con **echo**.

El script es el siguiente:

```bash
#!/bin/sh

if [ -s /home/kali/.config/personal/objetivo ]; then
	DATA=$(/bin/cat /home/kali/.config/personal/objetivo)
	echo "                    $DATA"
else
	echo ""
fi

```

Los espacios en ambos script es para separar del resto de botones.

![](../images/kali36.png)

De igual forma que en el script anterior, creamos un **Generic Monitor** con la misma configuración pero cambiando, lógicamente la ruta al nuevo script, y reduciendo el tiempo a 10 segundo ya que esto si es cambiante.

![](../images/kali37.png)

Salvamos y automáticamente ya vemos el contenido del fichero objetivo.

![](../images/kali38.png)


#### Creación de utilidad que escriba y limpie el fichero.

Lo primero que hemos de hacer es averiguar que tipo de SHELL tenemos para saber que fichero tenemos que modificar. Existen varias formas de hacerlo.

* Buscando nuestro usuario en el fichero /etc/passwd

![](../images/kali39.png)

* mostrando la variable de entorno **SHELL**

![](../images/kali40.png)

En ambos casos nos devuelve lo mismo. Esta versión de Kali utiliza una **zsh**.

Al ser una **zsh** tendremos que tocar el fichero **/home/kali/.zshrc**.
Si hubiera sido una **bash** tendríamos que tocar el fichero **/home/kali/.bashrc**.

Abrimos el fichero **/home/kali/.zshrc** y al final del todo añadimos estas utilidades:

```bash
function cleanTarget(){
  fichero=/home/kali/.config/personal/objetivo
  truncate -s 0 $fichero
  #echo $1 > $fichero
}

function setTarget(){
  fichero=/home/kali/.config/personal/objetivo
  truncate -s 0 $fichero
  echo $1 > $fichero
}

```

La función **cleanTarget** limpia el fichero **/home/kali/.config/personal/objetivo**.

La función **setTarget** escribe en el fichero **/home/kali/.config/personal/objetivo** lo que le pasemos como parámetro.

Salvamos los cambios del fichero y cerramos la terminal para abrir otra nueva y que coja el cambio.

Y probamos. Primero la función **cleanTarget**. Si ejecutamos y esperamos unos segundos veremos que desaparece lo que teníamos.

![](../images/kali41.png)

Ahora **setTarget**. La ejecutamos y unos segundos después aparece el nuevo objetivo.

![](../images/kali42.png)

### Plugins de Firefox

Es conveniente añadir algunos **AddOns* a Firefox. Iremos añadiendo mas a medida que los necesitemos. En principio añadimos los siguientes:

#### Wappalyzer

Nos enumera las tecnologías con las que está creada una Web.

![](../images/plug01.png)


#### FoxyProxy

Nos permite tener varios proxies configurados. Para utilizarlo con Burpsuite.

![](../images/plug02.png)


### Post-install

```bash
#/bin/bash

sudo adduser kali kali-trusted
sudo apt update && sudo apt upgrade -y
sudo apt update && sudo apt full-upgrade -y
sudo apt update && sudo apt autoremove -y

sudo apt install software-properties-common apt-transport-https curl wget gpg ca-certificates -y
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt install software-properties-common apt-transport-https curl wget gpg ca-certificates -y
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt update
sudo apt install terminator aptitude sublime-text locate code cherrytree neovim xclip flameshot -y 
sudo updatedb
sudo reboot

```