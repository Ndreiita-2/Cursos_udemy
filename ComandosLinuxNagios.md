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
7. Reiniciar servicios de Nagios
```
sudo systemctl restart nagios.service
```
