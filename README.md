# Modificaciones del Escenario

## Cambio del ServerName

- `ansible/group_vars/all` -> Para cambiar las variables.

## PHP-FPM para ejecutar código PHP

- `ansible/group_vars/all` -> Para añadir los módulos que tiene que cargar apache2.
- `ansible/roles/apache2/tasks/main.yaml` -> Para añadir la instalación y configuración de php-fpm.
- `ansible/roles/apache2/handlers/main.yaml` -> Para añadir el reinicio del servicio php-fpm.
- `ansible/roles/apache2/templates/etc/apache2/vhost.j2` -> Para añadir la configuración `ProxyPassMatch` en el VirtualHost.

## Servidor Web de Router/NAT y quitar al equipo MariaDB de la red externa

- `ansible/roles/iptables/files/99-forward.conf` -> Configuración bit de forwarding.
- `ansible/roles/iptables/files/rules.v4` -> Reglas iptables.
- `ansible/roles/iptables/handlers/main.yaml` -> Configuración de los restart del sysctl e iptables.
- `ansible/roles/iptables/tasks/main.yaml` -> Instalación y configuración iptables y bit de fowarding.
- `ansible/site.yaml` -> Para añadir a los hosts servidores_web el rol de iptables y cambio en el orden ya que si se actualiza el equipo MariaDB antes de hacer el SNAT no tendrá conexión.
- `opentofu/cloud-init/server2/network-config.yaml` -> Configurar Gateway, DNS del equipo con MariaDB, y eliminar configuración de la red externa.
- `opentofu/escenario.tf` -> Quitar interfaz `red-externa` del equipo MariaDB.

## Usar el servidor web nginx en vez de apache2

- `ansible/roles/nginx/tasks/main.yaml` -> Cambio de instalación y configuración de apache2 por nginx.
- `ansible/roles/nginx/handlers/main.yaml` -> Cambio del reinicio de apache2 por nginx.
- `ansible/roles/nginx/templates/etc/nginx/vhost.j2` -> Modificación del VirtualHost para adaptarlo al formato de nginx.
- `ansible/group_vars/all` -> Eliminación del bloque de modulos que antes se activavan al usar apache2, pero que ya no son necesarios al usar nginx.
- `ansible/hosts` -> Cambio del nombre del host, antes era apache2 y ahora nginx.
- `ansible/site.yaml` -> Cambio del nombre del rol apache2 por nginx.
- `opentofu/cloud-init/server1/user-data.yaml` -> Cambio del hostname de la máquina, antes era pi-apache2 y ahora pi-nginx.
- `opentofu/escenario.tf` -> Cambio de nombre del bloque que crea la máquina con el servidor web, y cambio de nombre de la máquina, antes pi-apache2 y ahora pi-nginx.

## Añadir nueva página (game.diego.org)
- `ansible/group_vars/all` -> Para añadir las variables del nuevo VirtualHost.
- `ansible/site.yaml` -> Para añadir el nuevo rol al Servidor Web.
- `ansible/roles/game/handlers/main.yaml` -> Para añadir el restart del servicio nginx.
- `ansible/roles/game/tasks/main.yaml` -> Para añadir las tareas que debe realizar el rol de game para que la aplicación funcione.
- `ansible/roles/game/files/app/index.html` -> Archivo principal de la aplicación, es el que ansible copiará en el documentroot.
- `ansible/roles/game/templates/etc/nginx/vhost.j2` -> Plantilla para añadir al Servidor Web el Virtual Host, a partir de las variables configuradas anteriormente en `ansible/group_vars/all`.

# Pasos para hacer funcionar el escenario
- Modificar los `opentofu/cloud-init/serverX/user-data.yaml` y añadir tu clave ssh pública.
- Modificar `opentofu/variables.tf` y añadir la ruta donde se encuentran las imagenes: **debian13-base.qcow2** y **ubuntu2404-base.qcow2**.
- Modificar `ansible/hosts` para añadir la ruta de tu clave ssh privada.
- Ejecutar dentro de la carpeta `opentofu/` los comandos `tofu init` y `tofu apply`.
- Ejecutar dentro de la carpeta `ansible/` el comando `ansible-playbook site.yaml`.
- Añadir en `/etc/hosts` las resoluciones `{ip_red_externa_servidor_web} guestbook.diego.org` y `{ip_red_externa_servidor_web} game.diego.org`.
