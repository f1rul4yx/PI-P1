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

# Pasos para hacer funcionar el escenario
- Modificar los `opentofu/cloud-init/serverX/user-data.yaml` y añadir tu clave ssh pública
- Modificar `opentofu/variables.tf` y añadir la ruta donde se encuentran las imagenes: **debian13-base.qcow2** y **ubuntu2404-base.qcow2**
- Modificar `ansible/hosts` para añadir la ruta de tu clave ssh privada
- Ejecutar dentro de la carpeta `opentofu/` los comandos `tofu init` y `tofu apply`
- Ejecutar dentro de la carpeta `ansible/` el comando `ansible-playbook site.yaml`
