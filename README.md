# Modificaciones del Escenario

## Cambio del ServerName

### Cambios en las máquinas base

```bash
sudo mv /etc/apache2/sites-available/example.conf /etc/apache2/sites-available/guestbook.conf
sudo a2dissite example.conf
sudo a2ensite guestbook.conf
```

```bash
sudo nano /etc/apache2/sites-available/guestbook.conf
```

```
<VirtualHost *:80>
    ServerName guestbook.diego.org
    DocumentRoot /var/www/guestbook

    <Directory /var/www/guestbook>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error_guestbook.log
    CustomLog ${APACHE_LOG_DIR}/acceses_guestbook.log combined
</VirtualHost>
```

```bash
sudo mv /var/www/example/ /var/www/guestbook
sudo systemctl restart apache2
```

### Cambios en los archivos del escenario

Los ficheros modificados para lograr el cambio son:

- ansible/group_vars/all

# Pasos para hacer funcionar el escenario
- Modificar los `opentofu/cloud-init/serverX/user-data.yaml` y añadir tu clave ssh pública
- Modificar `opentofu/variables.tf` y añadir la ruta donde se encuentran las imagenes: **debian13-base.qcow2** y **ubuntu2404-base.qcow2**
- Modificar `ansible/hosts` para añadir la ruta de tu clave ssh privada

