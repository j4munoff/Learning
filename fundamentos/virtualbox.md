# Instalación de Virtual Box

## Introducción
Oracle VM VirtualBox, el software de virtualización multiplataforma de código abierto más popular del mundo, permite a los desarrolladores entregar código más rápido, ya que pueden ejecutar múltiples sistemas operativos en un solo dispositivo. Los equipos de TI y los proveedores de soluciones usan VirtualBox para reducir los costes operativos y acortar el tiempo necesario para implementar aplicaciones de forma segura on-premises y en la nube.

## Descarga

Hay que descargarse dos cosas:

* **Ejecutable de instalación de VirtualBox**. Es este caso para instalar en Windows.
https://download.virtualbox.org/virtualbox/7.1.0/VirtualBox-7.1.0-164728-Win.exe 
* **VirtualBox Extension Pack**. Permite integración entre la máquinma Host y las máquinas virtuales. Permite copiar y pegar, mejora la integración de teclados y ratones, etc. 
https://download.virtualbox.org/virtualbox/7.1.0/Oracle_VirtualBox_Extension_Pack-7.1.0.vbox-extpack

![](../images/downvbox.png)

## Instalación de Virtual Box

Comenzamos ejecutando el instalador, VirtualBox-7.1.0-164728-Win.exe. Todo por defecto, (Nest --> Next, ..).

![alt text](../images/img-virtualbox.png)

![alt text](../images/img-virtualbox-1.png)

Nos avisa que se instalarán tarjetas de red virtuales y que temporalmente se desconectará la red.

![alt text](../images/img-virtualbox-2.png)

Nos informa de la instalación de dependencias de **Python**. Siempre Yes.

![alt text](../images/img-virtualbox-3.png)

![alt text](../images/img-virtualbox-4.png)

Por último **Install**.

![alt text](../images/img-virtualbox-5.png)

![alt text](../images/img-virtualbox-6.png)

Dejamos que arranque despues de finalizar.

![alt text](../images/img-virtualbox-7.png)

![alt text](../images/img-virtualbox-8.png)

## Instalación del Extension Pack

Nos trasladamos al meú **Archivo --> Herramientas --> Administrador de paquete de Extensiones**. Tambien podemos pulsar **CTLR + T**. 

![alt text](../images/img-virtualbox-9.png)


Pulsamos instalar y buscamos en descargas el fichero descargado **Oracle_VirtualBox_Extension_Pack-7.1.0.vbox-extpack**.

![alt text](../images/img-virtualbox-10.png)

![alt text](../images/img-virtualbox-11.png)

Pulsamos en Instalar. Bajamos la licencia hasta el final y pulsamos **Acepto**. Se instalará la extensión.

![alt text](../images/img-virtualbox-12.png)

## Modo experto de VirtualBox

En la página de bienvenida seleccionar la opción **Expert Mode**.

![alt text](../images/img-virtualbox-13.png)

Esta opción es necesaria para ver el menu de redes.

## Redes en VirtualBox

Las redes se pueden ver en modo Experto bajo el menú **Archivo --> Herramientas --> Administrador de Red**. Hay que tener al menos una red NAT.

{% hint style="danger" %}
**IMPORTANTE** La revisión de redes la haremos con una clase.
{% endhint %}

## Copia de seguridad en VirtualBox

La copia de seguridad o snapshot es una **instantanea** de la máquina virtual en un momento dado. Esto permite restaurar en un futuro la máquina virtual a la versión guardada si por lo que sea la máquina se degrada.

### Tomar instantanea

Es conveniente, aunque no obligatorio, que la máquina virtual esté apagada antes de tomar la instantanea.

Para tomar instantaneas podemos ir al menú **Máquina --> Herramientas-->Instantaneas** y con una máquina virtual seleccionada, ir al menú **Instantaneas-->Tomar** y o bien darle al botón **Tomar**.

![alt text](../images/img-virtualbox-15.png)

Le damos un nombre y una descripción si se desea y se pulsa aceptar.

![alt text](../images/img-virtualbox-16.png)

Nos aparecerá la instantanea tomada.

![alt text](../images/img-virtualbox-17.png)

### Restaurar instantanea

Si necesitamos restaurar la máquina virtual a un estado anterior del que se ha tomado instantanea podemos hacerlo de la siguiente manera.

Vamos al menú **Máquina --> Herramientas-->Instantaneas** y seleccionamos una máquina. Hacemos click en la linea con el nombre de la instantanea y damos al botón derecho.

![alt text](../images/img-virtualbox-18.png)

Seleccionamos la opción de **Restaurar** y nos restaurará la máquina virtual.

## Copiar y pegar entre máquina virtual y maquina principal

Para activar la opción de copiar y pegar desde/hacia la máquina virtual hay que ponerse en la máquina, **CON ELLA APAGADA**,  con el botón derecho y elegir configuración.

![alt text](../images/img-virtualbox-19.png)

En general --> Avanzado aparecerán las opciones deshabilitadas.

![alt text](../images/img-virtualbox-20.png)

Las habilitamos como **bidireccional** y aceptamos.

![alt text](../images/img-virtualbox-21.png)

## Atajos en VirtualBox

En todos los programas de virtualización existe una tecla llamada **MASTER** que permite combinar con otras teclas para interactuar con el sistema anfitrión, nuestro windows.

La tecla master en VirtualBox es la tecla CONTROL DERECHA, (CTLR DER).

![alt text](../images/img-virtualbox-14.png)

Enumeramos los atajos mas comunes dentro de una máquina virtual.

* **MASTER**: Escapar del entorno virtual. Accedemos al operativo principal.
* **MASTER + F**: Pantalla completa o normal. Alterna ambas.


