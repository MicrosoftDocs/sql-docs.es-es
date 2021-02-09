---
title: Instalación de Linux y macOS para los controladores de PHP
description: En estas instrucciones, aprenderá a instalar los controladores de Microsoft para PHP para SQL Server en Linux o macOS.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
manager: v-mabarw
ms.openlocfilehash: dfb8494912ae2f11590ec8a8b679f0f23064f930
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076457"
---
# <a name="linux-and-macos-installation-tutorial-for-the-microsoft-drivers-for-php-for-sql-server"></a>Tutorial de instalación de los controladores de Microsoft en Linux y macOS de PHP para SQL Server
En las instrucciones siguientes se supone que existe un entorno limpio y se explica cómo instalar PHP 8.0, Microsoft ODBC Driver, el servidor web de Apache y los controladores de Microsoft para PHP para SQL Server en Ubuntu 16.04, 18.04 y 20.04, Red Hat 7 y 8, Debian 9 y 10, SUSE 12 y 15, Alpine 3.11 y 3.12, y macOS 10.14, 10.15 y 11.0. Estas instrucciones aconsejan instalar los controladores con PECL, pero también puede descargar los archivos binarios creados previamente de la página de proyecto de GitHub [Controladores de Microsoft para PHP para SQL Server](https://github.com/Microsoft/msphpsql/releases) e instalarlos siguiendo las instrucciones de [Carga de los controladores de Microsoft para PHP para SQL Server](../../connect/php/loading-the-php-sql-driver.md). Para obtener una explicación de la carga de la extensión y por qué no agregar las extensiones en php.ini, vea la sección sobre la [carga de los controladores](../../connect/php/loading-the-php-sql-driver.md#loading-the-driver-at-php-startup).

Las siguientes instrucciones instalan PHP 8.0 de forma predeterminada mediante `pecl install` si los paquetes de PHP 8.0 están disponibles. Puede que primero deba ejecutar `pecl channel-update pecl.php.net`. Algunas distribuciones de Linux admitidas tienen como valor predeterminado PHP 7.1 o versiones anteriores, una opción no admitida para la última versión de los controladores de PHP para SQL Server. Consulte las notas al principio de cada sección para instalar PHP 7.4 o 7.3 en su lugar.

También se incluyen instrucciones para instalar el Administrador de procesos FastCGI para PHP, PHP-FPM, en Ubuntu. Esto es necesario si se usa el servidor web nginx en lugar de Apache.

Aunque estas instrucciones contienen comandos para instalar los controladores SQLSRV y PDO_SQLSRV, estos también se pueden instalar y funcionar independientemente. Los usuarios familiarizados con la personalización de la configuración pueden ajustar estas instrucciones para que sean específicas para SQLSRV o PDO_SQLSRV. Ambos controladores tienen las mismas dependencias, excepto donde se indica a continuación.

## <a name="contents-of-this-page"></a>Contenido de esta página

- [Instalación de los controladores en Ubuntu 16.04, 18.04 y 20.04](#installing-the-drivers-on-ubuntu-1604-1804-and-2004)
- [Instalación de los controladores con PHP-FPM en Ubuntu](#installing-the-drivers-with-php-fpm-on-ubuntu)
- [Instalación de los controladores en Red Hat 7 y 8](#installing-the-drivers-on-red-hat-7-and-8)
- [Instalación de los controladores en Debian 9 y 10](#installing-the-drivers-on-debian-9-and-10)
- [Instalación de los controladores en Suse 12 y 15](#installing-the-drivers-on-suse-12-and-15)
- [Instalación de los controladores en Alpine 3.11 y 3.12](#installing-the-drivers-on-alpine-311-and-312)
- [Instalación de los controladores en macOS Mojave, Catalina y Big Sur](#installing-the-drivers-on-macos-mojave-catalina-and-big-sur)

## <a name="installing-the-drivers-on-ubuntu-1604-1804-and-2004"></a>Instalación de los controladores en Ubuntu 16.04, 18.04 y 20.04

> [!NOTE]
> Para instalar PHP 7.4 o 7.3, reemplace 8.0 por 7.4 o 7.3 en los comandos siguientes.

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP
```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php8.0 php8.0-dev php8.0-xml -y --allow-unauthenticated
```
### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para Ubuntu, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

Si solo hay una versión de PHP en el sistema, el último paso se puede simplificar a `phpenmod sqlsrv pdo_sqlsrv`.

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Paso 4. Instalación de Apache y configuración de la carga de los controladores
```bash
sudo su
apt-get install libapache2-mod-php8.0 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php8.0
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Paso 5. Reinicio de Apache y prueba del script de ejemplo
```bash
sudo service apache2 restart
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.

## <a name="installing-the-drivers-with-php-fpm-on-ubuntu"></a>Instalación de los controladores con PHP-FPM en Ubuntu

> [!NOTE]
> Para instalar PHP 7.4 o 7.3, reemplace 8.0 por 7.4 o 7.3 en los comandos siguientes.

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP
```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php8.0 php8.0-dev php8.0-fpm php8.0-xml -y --allow-unauthenticated
```
Compruebe el estado del servicio PHP-FPM mediante la ejecución de
```bash
systemctl status php8.0-fpm
```
### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para Ubuntu, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
```bash
sudo pecl config-set php_ini /etc/php/8.0/fpm/php.ini
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```
Si solo hay una versión de PHP en el sistema, el último paso se puede simplificar a `phpenmod sqlsrv pdo_sqlsrv`.

Compruebe que `sqlsrv.ini` y `pdo_sqlsrv.ini` se encuentran en `/etc/php/8.0/fpm/conf.d/`:
```bash
ls /etc/php/8.0/fpm/conf.d/*sqlsrv.ini
```
Reinicie el servicio PHP-FPM:
```bash
sudo systemctl restart php8.0-fpm
```

### <a name="step-4-install-and-configure-nginx"></a>Paso 4. Instalación y configuración de nginx
```bash
sudo apt-get update
sudo apt-get install nginx
sudo systemctl status nginx
```
Para configurar nginx, debe editar el archivo `/etc/nginx/sites-available/default`. Agregue `index.php` a la lista que se encuentra debajo de la sección que indica `# Add index.php to the list if you are using PHP`:
```
# Add index.php to the list if you are using PHP
index index.html index.htm index.nginx-debian.html index.php;
```
Después, quite la marca de comentario y modifique la sección que sigue a `# pass PHP scripts to FastCGI server` como se indica a continuación:
```
# pass PHP scripts to FastCGI server
#
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.0-fpm.sock;
}
```
### <a name="step-5-restart-nginx-and-test-the-sample-script"></a>Paso 5. Reinicio de nginx y prueba del script de ejemplo
```bash
sudo systemctl restart nginx.service
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.

## <a name="installing-the-drivers-on-red-hat-7-and-8"></a>Instalación de los controladores en Red Hat 7 y 8

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP

Para instalar PHP en Red Hat 7, ejecute lo siguiente:
> [!NOTE]
> Para instalar PHP 7.4 o 7.3, reemplace remi-php80 por remi-php74 o remi-php73, respectivamente, en los comandos siguientes.
```bash
sudo su
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
subscription-manager repos --enable=rhel-7-server-optional-rpms
yum install yum-utils
yum-config-manager --enable remi-php80
yum update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
yum install php php-pdo php-xml php-pear php-devel re2c gcc-c++ gcc
```

Para instalar PHP en Red Hat 8, ejecute lo siguiente:
> [!NOTE]
> Para instalar PHP 7.4 o 7.3, reemplace remi-8.0 por remi-7.4 o remi-7.3, respectivamente, en los comandos siguientes.
```bash
sudo su
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf install yum-utils
dnf module reset php
dnf module install php:remi-8.0
subscription-manager repos --enable codeready-builder-for-rhel-8-x86_64-rpms
dnf update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
dnf install php-pdo php-pear php-devel
```

### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para Red Hat 7 u 8, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20-sqlsrv.ini
exit
```

También puede instalar desde el repositorio Remi:
```bash
sudo yum install php-sqlsrv
```
### <a name="step-4-install-apache"></a>Paso 4. Instalación de Apache
```bash
sudo yum install httpd
```
SELinux está instalado de forma predeterminada y se ejecuta en modo de aplicación. Para que Apache pueda conectarse a las bases de datos mediante SELinux, ejecute el siguiente comando:
```bash
sudo setsebool -P httpd_can_network_connect_db 1
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Paso 5. Reinicio de Apache y prueba del script de ejemplo
```bash
sudo apachectl restart
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.

## <a name="installing-the-drivers-on-debian-9-and-10"></a>Instalación de los controladores en Debian 9 y 10

> [!NOTE]
> Para instalar PHP 7.4 o 7.3, reemplace 8.0 en los comandos siguientes por 7.4 o 7.3.

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP
```bash
sudo su
apt-get install curl apt-transport-https
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt-get install -y php8.0 php8.0-dev php8.0-xml php8.0-intl
```
### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para Debian, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

También es posible que deba generar la configuración regional correcta para que el resultado de PHP se muestre correctamente en un explorador. Por ejemplo, para la configuración regional UTF-8 en_US, ejecute los siguientes comandos:
```bash
sudo su
sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen
```
Es posible que tenga que agregar `/usr/sbin` a su `$PATH`, ya que el ejecutable `locale-gen` se encuentra allí.

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

Si solo hay una versión de PHP en el sistema, el último paso se puede simplificar a `phpenmod sqlsrv pdo_sqlsrv`. Al igual que con `locale-gen`, `phpenmod` se encuentra en `/usr/sbin`, por lo que es posible que tenga que agregar este directorio a su `$PATH`.

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Paso 4. Instalación de Apache y configuración de la carga de los controladores
```bash
sudo su
apt-get install libapache2-mod-php8.0 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php8.0
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Paso 5. Reinicio de Apache y prueba del script de ejemplo
```bash
sudo service apache2 restart
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.

## <a name="installing-the-drivers-on-suse-12-and-15"></a>Instalación de los controladores en Suse 12 y 15

> [!NOTE]
> En las siguientes instrucciones, reemplace `<SuseVersion>` por la versión de SUSE. Si usa SUSE Linux Enterprise 15, especificará SLE_15_SP1 o SLE_15_SP2. Para Suse 12, use SLE_12_SP4 (o una versión superior si corresponde). No todas las versiones de PHP están disponibles para todas las versiones de SUSE Linux. Consulte `http://download.opensuse.org/repositories/devel:/languages:/php` para ver qué versiones de SUSE tienen disponible la versión predeterminada de PHP, o bien `http://download.opensuse.org/repositories/devel:/languages:/php:/` para ver qué otras versiones de PHP están disponibles para el resto de las versiones de SUSE.

> [!NOTE]
> Los paquetes de PHP 7.4 (o una versión posterior) no están disponibles para SUSE 12, y el paquete de PHP 8.0 todavía no está disponible para SUSE 15.
> Para instalar PHP 7.3, reemplace la dirección URL del repositorio a continuación con la dirección URL siguiente: `https://download.opensuse.org/repositories/devel:/languages:/php:/php73/<SuseVersion>/devel:languages:php:php73.repo`.

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP
```bash
sudo su
zypper -n ar -f https://download.opensuse.org/repositories/devel:languages:php/<SuseVersion>/devel:languages:php.repo
zypper --gpg-auto-import-keys refresh
zypper -n install php7 php7-devel php7-openssl
```
### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para SUSE, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
> [!NOTE]
> Si obtiene un mensaje de error que indica `Connection to 'pecl.php.net:443' failed: Unable to find the socket transport "ssl"`, edite el script pecl en /usr/bin/pecl y quite el modificador `-n` en la última línea. Este modificador evita que PECL cargue archivos ini cuando se llama a PHP, lo que impide que se cargue la extensión OpenSSL.

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/sqlsrv.ini
exit
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>Paso 4. Instalación de Apache y configuración de la carga de los controladores
```bash
sudo su
zypper install apache2 apache2-mod_php7
a2enmod php7
echo "extension=sqlsrv.so" >> /etc/php7/apache2/php.ini
echo "extension=pdo_sqlsrv.so" >> /etc/php7/apache2/php.ini
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Paso 5. Reinicio de Apache y prueba del script de ejemplo
```bash
sudo systemctl restart apache2
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.

## <a name="installing-the-drivers-on-alpine-311-and-312"></a>Instalación de los controladores en Alpine 3.11 y 3.12

> [!NOTE]
> La versión predeterminada de PHP es 7.3. La versión 7.4 o versiones posteriores de PHP pueden estar disponibles en los repositorios perimetrales o de prueba para Alpine. En su lugar, puede compilar PHP desde el origen.

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP
Los paquetes PHP para Alpine se pueden encontrar en el repositorio de `edge/community`. Consulte [Habilitación del repositorio de la comunidad](https://wiki.alpinelinux.org/wiki/Enable_Community_Repository) en su wiki. Agregue la línea siguiente a `/etc/apk/repositories`, reemplazando `<mirror>` por la dirección URL de un reflejo del repositorio de Alpine:
```bash
http://<mirror>/alpine/edge/community
```
A continuación, ejecute:
```bash
sudo su
apk update
# Note: The php7-pdo package is required only for the PDO_SQLSRV driver
apk add php7 php7-dev php7-pear php7-pdo php7-openssl autoconf make g++
```
### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para Alpine, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/10_pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/00_sqlsrv.ini
```

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Paso 4. Instalación de Apache y configuración de la carga de los controladores
```bash
sudo apk add php7-apache2 apache2
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Paso 5. Reinicio de Apache y prueba del script de ejemplo
```bash
sudo rc-service apache2 restart
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.


## <a name="installing-the-drivers-on-macos-mojave-catalina-and-big-sur"></a>Instalación de los controladores en macOS Mojave, Catalina y Big Sur

Si no lo tiene, instale brew como sigue:
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> [!NOTE]
> Para instalar PHP 7.4 o 7.3, reemplace php@8.0 por php@7.4 o php@7.3, respectivamente, en los comandos siguientes.

### <a name="step-1-install-php"></a>Paso 1. Instalación de PHP

```bash
brew tap
brew tap homebrew/core
brew install php@8.0
```
Ahora, PHP debe estar en su ruta de acceso. Ejecute `php -v` para comprobar que está ejecutando la versión de PHP correcta. Si PHP no está en la ruta de acceso o no tiene la versión correcta, ejecute lo siguiente:
```bash
brew link --force --overwrite php@8.0
```

### <a name="step-2-install-prerequisites"></a>Paso 2. Requisitos previos de instalación
Para instalar el controlador ODBC para macOS, siga las instrucciones del artículo [Instalación de Microsoft ODBC Driver for SQL Server (macOS)](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md). 

Además, puede que deba instalar las herramientas de creación de GNU:
```bash
brew install autoconf automake libtool
```

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Paso 3. Instalación de los controladores de PHP para Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>Paso 4. Instalación de Apache y configuración de la carga de los controladores
```bash
brew install apache2
```
Para buscar el archivo de configuración de Apache, `httpd.conf`, para la instalación de Apache, ejecute 
```bash
/usr/local/bin/apachectl -V | grep SERVER_CONFIG_FILE
``` 
Los comandos siguientes anexan la configuración requerida a `httpd.conf`. Asegúrese de sustituir la ruta de acceso devuelta por el comando anterior en lugar de `/usr/local/etc/httpd/httpd.conf`:
```bash
echo "LoadModule php7_module /usr/local/opt/php@8.0/lib/httpd/modules/libphp7.so" >> /usr/local/etc/httpd/httpd.conf
(echo "<FilesMatch .php$>"; echo "SetHandler application/x-httpd-php"; echo "</FilesMatch>";) >> /usr/local/etc/httpd/httpd.conf
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Paso 5. Reinicio de Apache y prueba del script de ejemplo
```bash
sudo apachectl restart
```
Para probar la instalación, vea [Probar la instalación](#testing-your-installation) al final de este documento.

## <a name="testing-your-installation"></a>Probar la instalación

Para probar este script de ejemplo, cree un archivo llamado testsql.php en la raíz del documento del sistema. Se trata de `/var/www/html/` en Ubuntu, Debian y Redhat, de `/srv/www/htdocs` en SUSE, de `/var/www/localhost/htdocs` en Alpine o de `/usr/local/var/www` en macOS. Copie el script siguiente y reemplace el servidor, la base de datos, el nombre de usuario y la contraseña según proceda.

### <a name="sqlsrv-example"></a>Ejemplo de SQLSRV

```php
<?php
$serverName = "yourServername";
$connectionOptions = array(
    "database" => "yourDatabase",
    "uid" => "yourUsername",
    "pwd" => "yourPassword"
);

function exception_handler($exception) {
    echo "<h1>Failure</h1>";
    echo "Uncaught exception: " , $exception->getMessage();
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

set_exception_handler('exception_handler');

// Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if ($conn === false) {
    die(formatErrors(sqlsrv_errors()));
}

// Select Query
$tsql = "SELECT @@Version AS SQL_VERSION";

// Executes the query
$stmt = sqlsrv_query($conn, $tsql);

// Error handling
if ($stmt === false) {
    die(formatErrors(sqlsrv_errors()));
}
?>

<h1> Success Results : </h1>

<?php
while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
    echo $row['SQL_VERSION'] . PHP_EOL;
}

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

function formatErrors($errors)
{
    // Display errors
    echo "<h1>SQL Error:</h1>";
    echo "Error information: <br/>";
    foreach ($errors as $error) {
        echo "SQLSTATE: ". $error['SQLSTATE'] . "<br/>";
        echo "Code: ". $error['code'] . "<br/>";
        echo "Message: ". $error['message'] . "<br/>";
    }
}
?>
```

### <a name="pdo_sqlsrv-example"></a>Ejemplo de PDO_SQLSRV

```php
<?php
try {
    $serverName = "yourServername";
    $databaseName = "yourDatabase";
    $uid = "yourUsername";
    $pwd = "yourPassword";
    
    $conn = new PDO("sqlsrv:server = $serverName; Database = $databaseName;", $uid, $pwd);

    // Select Query
    $tsql = "SELECT @@Version AS SQL_VERSION";

    // Executes the query
    $stmt = $conn->query($tsql);
} catch (PDOException $exception1) {
    echo "<h1>Caught PDO exception:</h1>";
    echo $exception1->getMessage() . PHP_EOL;
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

?>

<h1> Success Results : </h1>

<?php
try {
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo $row['SQL_VERSION'] . PHP_EOL;
    }
} catch (PDOException $exception2) {
    // Display errors
    echo "<h1>Caught PDO exception:</h1>";
    echo $exception2->getMessage() . PHP_EOL;
}

unset($stmt);
unset($conn);
?>
```

Dirija el explorador a https://localhost/testsql.php (https://localhost:8080/testsql.php en macOS). Ahora podrá conectarse a la base de datos de SQL Server o Azure SQL. Si no ve un mensaje de confirmación que muestre información de la versión de SQL, puede seguir algunos pasos básicos para solucionar problemas mediante la ejecución del script desde la línea de comandos:

```bash
php testsql.php
```

Si la ejecución desde la línea de comandos se realiza correctamente pero no se muestra nada en el explorador, compruebe los [archivos de registro de Apache](https://linuxize.com/post/apache-log-files/#location-of-the-log-files). Si necesita más ayuda, consulte [Recursos de soporte](support-resources-for-the-php-sql-driver.md) para conocer los recursos que pueden serle de ayuda.

## <a name="see-also"></a>Consulte también  
[Introducción a los controladores de Microsoft para PHP para SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)

[Carga de los controladores de Microsoft para PHP para SQL Server](../../connect/php/loading-the-php-sql-driver.md)

[Requisitos del sistema para los controladores de Microsoft para PHP para SQL Server](../../connect/php/system-requirements-for-the-php-sql-driver.md)
