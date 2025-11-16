# Modificaciones del Escenario

## Cambio del ServerName

Los ficheros modificados para lograr el cambio son:

- ansible/group_vars/all

# Pasos para hacer funcionar el escenario
- Modificar los `opentofu/cloud-init/serverX/user-data.yaml` y añadir tu clave ssh pública
- Modificar `opentofu/variables.tf` y añadir la ruta donde se encuentran las imagenes: **debian13-base.qcow2** y **ubuntu2404-base.qcow2**
- Modificar `ansible/hosts` para añadir la ruta de tu clave ssh privada
- Ejecutar dentro de la carpeta `opentofu/` los comandos `tofu init` y `tofu apply`
- Ejecutar dentro de la carpeta `ansible/` el comando `ansible-playbook site.yaml`
