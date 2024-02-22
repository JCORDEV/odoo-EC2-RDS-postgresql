# Instalar odoo en instancia AWS EC2 con RDS PostgreSQL

<details id="rds">
    <summary>
        <strong>1.</strong>Crear base de datos <strong>RDS</strong> con <strong>PostgreSQL</strong>
    </summary>
    <p>Aplicar todos los cambios que se realizan en la imagen</p>
    <img src="img/1.jpeg" alt="Paso 1 - configuración del RDS">
    <p>Esperamos hasta que se encuentre <strong>Disponible</strong></p>
    <img src="img/3.jpeg" alt="Paso 1 - configuración del RDS">
</details>

**[Volver ⤴️](#rds)**

<details>
    <summary>
        <strong>2.</strong> Modificar grupo de seguridad</strong>
    </summary>
    <p>Ingresamos a nuestro RDS y seleccionamos el grupo de seguridad <strong>default (...)</strong></p>
    <img src="img/4.jpeg" alt="Paso 1 - configuración del RDS">
    <p>Ingresamos a <strong>ID de grupo de seguridad</strong></p>
    <img src="img/5.jpeg" alt="Paso 1 - configuración del RDS">
    <p>Editar reglas de entrada</p>
    <img src="img/6.jpeg" alt="Paso 1 - configuración del RDS">
    <p>Creamos una regla de entrada para poder conectarse de forma remota</p>
    <img src="img/7.jpeg" alt="Paso 1 - configuración del RDS">
</details>

<details>
    <summary>
        <strong>3.</strong> Conexión remota a <strong>RDS</strong>
    </summary>
    <ul>
        <li>
            <p>Descargar <strong>DBeaver ➡️ <a src="https://dbeaver.io/download/">Aquí</a></p></strong>
        </li>
        <li>
            <p>Seleccionamos <strong>Base de datos ➡️ Nueva conexión</strong></p>
            <img src="img/8.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Seleccionamos el <strong>motor de base de datos</strong>, en este caso <strong>PostgreSQL</strong>.</p>
            <img src="img/9.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Registramos ➡️ <strong>Host (punto de enlace) ➡️ Database (registrada en rds) ➡️ Port (5432) ➡️ Nombre de usuario (registrado en rds) ➡️ Contraseña (registrada en rds)</strong></p>
            <img src="img/10.png" alt="Paso 1 - configuración del RDS">
            Por último, <strong>Probar conexión</strong>
            <img src="img/11.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Si todo está bien configurado, debería salir así</p>
            <img src="img/12.png" alt="Paso 1 - configuración del RDS">
            <img src="img/13.1.png" alt="Paso 1 - configuración del RDS">
        </li>
    </ul>
</details>

<details>
    <summary>
        <strong>4.</strong> Instalar <strong>recursos necesarios para odoo</strong>
    </summary>
    <ul>
        <li>
            <p>Ingresamos como superusuario ➡️ <code>sudo su</code> y actualizamos repositorios ➡️ <code>apt update</code></p>
            <img src="img/18.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Instalamos el siguiente bloque de aplicaciones (dependencias) ➡️ <code>apt install python3-pip xfonts-75dpi xfonts-base libxrender1 libjpeg-turbo8 fontconfig -y</code></p>
            <img src="img/19.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Instalamos la librería libssl1.1.
            <br>➡️ <code>echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list</code></p>
            <img src="img/20.png" alt="Paso 1 - configuración del RDS">
            <p>
            <br>Ahora sí, actualizamos nuestro sistema e instalamos la librería que necesitamos.
            <br>➡️ <code>apt update</code>
            <br>➡️ <code>apt install libssl1.1</code></p>
            <img src="img/21.png" alt="Paso 1 - configuración del RDS">
            <img src="img/22.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Entramos a al directorio ➡️ <code>cd /opt</code> y descargamos <strong>Wkhtmltopdf</strong> ➡️ <code>wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.bionic_amd64.deb</code></p>
            <img src="img/23.png" alt="Paso 1 - configuración del RDS">
            <p>y procedemos a la instalación del paquete ➡️ <code>dpkg -i wkhtmltox_0.12.6-1.bionic_amd64.deb</code></p>
            <img src="img/24.png" alt="Paso 1 - configuración del RDS">
            <p>Luego, copiamos los siguientes binarios en ciertas rutas del sistema.
            <br>➡️ <code>cp /usr/local/bin/wkhtmltoimage  /usr/bin/wkhtmltoimage</code>
            <br>➡️ <code>cp /usr/local/bin/wkhtmltopdf  /usr/bin/wkhtmltopdf</code></p>
            <img src="img/25.png" alt="Paso 1 - configuración del RDS">
        </li>
    </ul>
</details>

<details>
    <summary>
        <strong>5.</strong> Instalación de <strong>Odoo 16</strong>
    </summary>
    <ul>
        <li>
            <p>Ya tenemos todo preparado para instalar odoo, ejecutamos los siguientes comandos
            <br>➡️ <code>wget -q -O - https://nightly.odoo.com/odoo.key | sudo gpg --dearmor -o /usr/share/keyrings/odoo-archive-keyring.gpg</code>
            <br>➡️ <code>echo 'deb [signed-by=/usr/share/keyrings/odoo-archive-keyring.gpg] https://nightly.odoo.com/16.0/nightly/deb/ ./' | sudo tee /etc/apt/sources.list.d/odoo.list</code>
            <br>➡️ <code>apt update</code>
            </p>
            <img src="img/26.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Por último, finalmente instalación ➡️ <code>apt install odoo -y</code></p>
            <img src="img/27.png" alt="Paso 1 - configuración del RDS">
            <p>Se encargará de descargar e instalar todo lo necesario para <strong>Odoo</strong>, en el caso de hacer la instalación de manera local, dependerá de su computadora y la velocidad de su internet, en nuestro caso será rápido porque está directamente desde los servidores de <strong>AWS</strong></p>
        </li>
        <li>
            <p>Revisamos si está en ejecución <strong>odoo.service</strong> ➡️ <code>systemctl status odoo</code></p>
            <img src="img/28.png" alt="Paso 1 - configuración del RDS">
            <p>Veamos si se encuentra algún error en el log de odoo ➡️ <code>tail -n 20 /var/log/odoo/odoo-server.log</code></p>
            <img src="img/30.png" alt="Paso 1 - configuración del RDS">
            <p>Visualizamos las primeras 20 líneas.
        </li>
    </ul>
</details>

<details>
        <summary>
            <strong>6.</strong> Configuración final de <strong>Odoo 16</strong>
        </summary>
    <ul>
        <li>
            <p>Entramos a la siguiente ruta y editamos el archivo odoo.conf ➡️ <code>cd /etc/odoo/odoo.conf</code>
            <br>db_host ➡️ punto de enlace RDS
            <br>db_port ➡️ puerto de RDS postgres
            <br>db_user ➡️ usuario de RDS
            <br>db_password ➡️ contraseña de RDS
            </p>
            <img src="img/29.png" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Entramos a nuestra página y configuramos como está en la imagen y por último en create database ➡️ <code>IP-publica:8069</code></p>
            <img src="img/31.jpeg" alt="Paso 1 - configuración del RDS">
            <p>Recuerda que debes abrir el puerto 8069 en el grupo de seguridad de la instancia EC2 para que odoo tenga vía libre</p>
        </li>
        <li>
            <p>Iniciamos sesión con el correo y contraseña previamente creados ➡️ <code>IP-publica:8069</code></p>
            <img src="img/32.jpeg" alt="Paso 1 - configuración del RDS">
        </li>
        <li>
            <p>Y listo, ya podemos explorar odoo a nuestro antojo ➡️ <code>IP-publica:8069</code></p>
            <img src="img/33.jpeg" alt="Paso 1 - configuración del RDS">
        </li>
    </ul>
</details>
