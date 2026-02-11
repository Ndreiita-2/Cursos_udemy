# Introducción Nagios
**Qué és y para qué sirve**

Nagios es una herramienta de monitorización de código abierto que permite supervisar sistemas, redes e infraestructuras de TI, alertando sobre problemas y fallos en tiempo real. 

# Rutas importantes

- /usr/local/nagios/etc
    - cgi.cfg -> Archivo de configuración de la web
    - nagios.cfg -> Arcivo de configuración motor de Nagios
    - resource.cfg (macros $USERx$) -> Archivo definir macros de usuarios y contraseñas, especificar rutas de plugins
    - htpasswd.users -> Contiene la información de los usuarios y su contraseña cifrada

- /usr/local/nagios/etc/objects
    - localhost.cfg -> Archivo configuración del servidor de Nagios para automonitorizarse
    - commands.cfg -> Permite definir los comandos al hacer checks
    - contacts.cfg -> Definir y dar de alta contactos
    - timeperiods.cfg -> Configura la información sobre los periodos de tiempo que queremos que se ejecuten los checks

UDEMY SECCIÓN 3.11 PRÁCTICA

> :q! -> Salir sin guardar

# Comandos
1. Editar configuración
```
sudo vim nagios.cfg
```
2. Editar comandos
```
sudo vim commands.cfg
```
3. Editar host
```
sudo vim localhost.cfg
```
4. Realizar copia
```
sudo cp nagios.cfg nagios.cfg-NOMBRE
```
5. Ver errores
```
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/bin/nagios.cfg
```
6. Hacer copia de seguridad
```
sudo cp nagios.cfg nagios.cfg.bck
```

## Notificaciones a Telegram
1. Buscamos BotFather
```
/newbot
```
2. Guardar el código para acceder por HTTP API
3. Crear un grupo
4. Meter el bot creado
5. Dar permisos al bot
6. Conectar nuestro Nagios por API
```
curl -LK -i -X GET https://api.telegram.org/botNUESTROCÓDIGOPASO2/getUpdates
```
7. Crear comando Telegram
```
sudo vim /usr/local/nagios/etc/objects/commands.cfg
```
```
# 'notify-host-by-telegram' command definition

define command{

command_name notify-host-by-telegram

command_line /usr/bin/curl -X POST --data chat_id=-NUESTROID --data parse_mode="markdown" --data text="%60$HOSTNAME$%60 %0A%0A$NOTIFICATIONTYPE$ %0A%60$HOSTSTATE$%60 %0A%60$HOSTADDRESS$%60 %0A%60$HOSTOUTPUT$%60" https://api.telegram.org/botNUESTROCÓDIGOPASO2/sendMessage

}

# 'notify-service-by-telegram' command definition

define command{

command_name notify-service-by-telegram

command_line /usr/bin/curl -X POST --data chat_id=-NUESTROID --data parse_mode="markdown" --data text="%60$HOSTNAME$%60 %0A%0A$NOTIFICATIONTYPE$ %0A%60$SERVICEDESC$%60 %0A%60$HOSTADDRESS$%60 %0A%60$SERVICESTATE$%60 %0A%60$SERVICEOUTPUT$%60" https://api.telegram.org/botNUESTROCÓDIGOPASO2/sendMessage

}
```
8. Definir contacto
```
sudo vim /usr/local/nagios/etc/objects/contacts.cfg
```
```
define contact{

contact_name                    NOMBRE
host_notifications_enabled      1
service_notifications_enable    1
service_notifications_period    24x7
host_notifications_period       24x7
service_notifications_options   w,u,c,r,f,s
host_notifications_options      d,u,r,f,s
service_notifications_commands  notify-service-by-email,notify-service-by-telegram
host_notifications_commands     notify-service-by-email,notify-service-by-telegram
email                           GMAIL
pager                           IDTELEGRAM
}
```
9. Reiniciamos el servivio de Nagios
```
sudo systemctl restart nagios
```
