# IBM-Cloud-VPC-Cloud-Guard

CloudGuard Security Management es una plataforma que brinda y gestiona seguridad avanzada en la implemetación de protecciones para organizaciones y en nubes públicas, privadas e híbridas. Este servicio cuenta con un registro, monitoreo, correlación de eventos e informes dentro de la misma interfaz web, la cual tiene un tablero visual que brinda una visibilidad completa de la seguridad en toda la red, lo que ayuda a las organizaciones a monitorear el estado de los puntos de cumplimiento y mantenerse alerta ante posibles amenazas.

### Contenido.

1. [Configuración de los parámetros del servicio.](#1-configuración-de-los-parámetros-del-servicio)
2. [Espacio de trabajo.](#2-espacio-de-trabajo)
3. [Acceso al CheckPoint CloudGuard.](#3-acceso-al-checkpoint-cloudguard)


## 1. Configuración de los parámetros del servicio.

![videoser](https://user-images.githubusercontent.com/60628267/103101329-1067d680-45e5-11eb-9a7f-667f4772ff0e.gif)

Ingrese a su cuenta de IBM Cloud y en _Catálogo_ > _Software_ > _En el buscador ingrese CloudGuard_ y seleccione el servicio de Check Point CloudGuard IaaS Security Management, para el cual proporcione los siguientes parámetros de despliegue:
- VPC_Region: Escriba la región donde se aprovisionarán la VPC, las redes y la VSI de Check Point.
- VPC_Zone: La zona donde se aprovisionarán la VPC, las redes y Check Point VSI.
- VPC_Name: Nombre de la VPC donde se aprovisionará la VSI de Check Point.
- Resource_Group: El grupo de recursos que se utilizará al aprovisionar Check Point VSI. 
- Subnet_ID: El ID de la subred donde se aprovisionará Check Point VSI.
- SSH_Key: El nombre de la clave SSH pública que se utilizará al aprovisionar la VSI de Check Point. 
- VNF_Security_Group: El nombre del grupo de seguridad para VNF VPC. 
- VNF_Profile: Define la CPU y memoria que se utilizaran al aprovisionar la VSI de Check Point.

## 2. Espacio de trabajo.
Una vez instalado el servicio tendrá acceso a un espacio de trabajo en Schematics.
En donde ingresando a _Actividades_> _Ver registro_ podrá ver con detalle toda la plantilla de Terraform y en la parte inferior de esta un recuento del número de recursos creados junto con un mensaje en verde _(Done with the workspace action)_.


<img width="959" alt="2" src="https://user-images.githubusercontent.com/60628267/103101130-19a47380-45e4-11eb-84d0-151888f71500.PNG">

Ahora acceda a _Recursos_> _la URL de VPC-Name-fip_ donde ingresado al link lo redireccionará a las IP flotantes para VPC, busque la que fue creada con el nombre provisto. Mediante esa IP flotante será posible acceder al CheckPoint CloudGuard.

<img width="960" alt="Captura" src="https://user-images.githubusercontent.com/60628267/103097944-08a13580-45d7-11eb-8912-a615d23253a7.PNG">

## 3. Acceso al CheckPoint CloudGuard.
Para configurar el CloudGuard necesitará conocer su IP flotante y el password por defecto es _admin_, luego de esto podrá acceder a el mediante las siguientes dos modalidades:
### 3.1. Conexión SSH.
Desde el PowerShell o cualquier otra línea de comando de su equipo ingrese el comando de conexión SSH:

```shell
ssh admin@<ip-flotante>
```


### 3.2. Consola Web.
Para acceder mediante la consola web es necesario que desde la CLI ingrese el siguiente comando para permitir el ingreso web y conocer que puerto esta habilitado.

```shell
show web daemon-enable
```
```shell
show web ssl-port
```
Para otras herramientas de configuración web como el tiempo de espera de sesión en minutos ingrese: 
```shell
show web (Y presione la tecla Tab ↹)
```
Luego de esto se procede a ingresar a la consola web mediente la ip con el puerto:
```shell
https://ip-flotante:puerto
```
**Nota: El puerto caracteristico es el 443**

<img width="900" alt="Captura33" src="https://user-images.githubusercontent.com/60628267/103098893-700cb480-45da-11eb-98ac-5e6a7fe8f08a.PNG">

Saldrá una alerta que la conexión no es privada por lo que haremos clic en _Configuración avanzada_ y luego en _Acceder a ip-flotante (sitio no seguro)_ donde nos pedira las credenciales de acceso. El usuario y contraseña por defecto son _admin_ una vez adentro se recomienda cambiar estos parámetros de acceso.

#### Configuración e Instalación CheckPoint.

<img width="567" alt="Captura34" src="https://user-images.githubusercontent.com/60628267/103099173-69cb0800-45db-11eb-8a14-3b8ccaa5014a.PNG">

En esta sección de configuración del CheckPoint es importante comprobar que esten correctos los datos que se configuran automaticamente, como lo son la IP flotante, la interfaz y la máscara de la subred; puede comprobar estos datos en los accediendo a los recursos aprovisionados en Schematics de IBM.

<img width="566" alt="Captura36" src="https://user-images.githubusercontent.com/60628267/103099896-36d64380-45de-11eb-98a6-3ec4e144f8f7.PNG">

<img width="567" alt="Captura35" src="https://user-images.githubusercontent.com/60628267/103099795-e2cb5f00-45dd-11eb-9d41-e71ebe971e5c.PNG">

<img width="565" alt="Captura37" src="https://user-images.githubusercontent.com/60628267/103099936-6dac5980-45de-11eb-8ffc-dcd363ccb8a3.PNG">

- Al configurar el asistente Gaia por primera vez, deberá establecer la contraseña de administrador y luego dar clic en Siguiente.
- Configure la fecha y la hora manualmente, o ingrese el nombre de host, la dirección IPv4 y luego haga clic en Siguiente.
- Configure el nombre de host para el dispositivo.
_Opcional:_ establezca el nombre de dominio y las direcciones IPv4 o IPv6 para los servidores DNS. 
- Configure una dirección IPv4 y una IPv6 para la interfaz de administración, o configure una dirección IP (IPv4 o IPv6).
Si cambia la dirección IP de administración, la nueva dirección IP se asigna a la interfaz. La dirección IP antigua se agrega como un alias y se usa para mantener la conectividad.
- Seleccione Security Gateway and Security Management y luego haga clic en Siguiente.
- Establezca el nombre de usuario y la contraseña para la cuenta de administrador del servidor Security Management y luego haga clic en Siguiente.


Finalizado este proceso le pedirá que reinicie el CheckPoint, por lo que deberá aceptar esta opción. Y luego de esto tendrá ya podrá acceder, configurar y gestionar toda la seguridad que CloudGuard permite realizar. 

<img width="960" alt="Captura39" src="https://user-images.githubusercontent.com/60628267/103100403-5f5f3d00-45e0-11eb-9113-ff60e3297d4d.PNG">

### 3.3. SmartConsole

Para acceder a través de esta consola, deberá descargar el Check_Point R80.40 SmartConsole para Windows/Mac. [Link](https://supportcenter.checkpoint.com/supportcenter/portal?action=portlets.DCFileAction&eventSubmit_doGetdcdetails=&fileid=101086). 

Ingrese las credenciales del servicio junto con la IP flotante, al hacer clic en conectar le saldrá la imágen a continuación y le deberá dar clic en continuar.

<img width="507" alt="Captura38" src="https://user-images.githubusercontent.com/60628267/103100346-2030ec00-45e0-11eb-8112-73152fca5d9d.PNG">

Ya con esto tendrá acceso al tablero de configuraciones del CheckPoint y gestionar toda laseguridad que requiera.

![image](https://user-images.githubusercontent.com/60628267/103100289-e7911280-45df-11eb-99c2-f4d12bd7eb5c.png)

## Autores

IBM Cloud Tech Sales ☁️



