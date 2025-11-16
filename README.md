# Modificaciones del Escenario

## Cambio del ServerName

- `ansible/group_vars/all` -> Para cambiar las variables.

## PHP-FPM para ejecutar código PHP

- `ansible/group_vars/all` -> Para añadir los módulos que tiene que cargar apache2.
- `ansible/roles/apache2/tasks/main.yaml` -> Para añadir la instalación y configuración de php-fpm.
- `ansible/roles/apache2/handlers/main.yaml` -> Para añadir el reinicio del servicio php-fpm.
- `ansible/roles/apache2/templates/etc/apache2/vhost.j2` -> Para añadir la configuración `ProxyPassMatch` en el VirtualHost.

# Pasos para hacer funcionar el escenario
- Modificar los `opentofu/cloud-init/serverX/user-data.yaml` y añadir tu clave ssh pública
- Modificar `opentofu/variables.tf` y añadir la ruta donde se encuentran las imagenes: **debian13-base.qcow2** y **ubuntu2404-base.qcow2**
- Modificar `ansible/hosts` para añadir la ruta de tu clave ssh privada
- Ejecutar dentro de la carpeta `opentofu/` los comandos `tofu init` y `tofu apply`
- Ejecutar dentro de la carpeta `ansible/` el comando `ansible-playbook site.yaml`
