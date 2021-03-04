---
title: Configuración de la autenticación de Active Directory con SQL Server en contenedores basados en Linux mediante adutil
description: Instrucciones paso a paso sobre cómo configurar la autenticación de Active Directory con SQL Server en contenedores basados en Linux mediante adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: c82362cf9a3d73d1bd9a7c714957e222ad15af18
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837636"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux--containers"></a>Tutorial: Configuración de la autenticación de Active Directory con SQL Server en contenedores de Linux

> [!NOTE]
> Actualmente, **adutil** está en **versión preliminar pública**.

En este tutorial se explica cómo configurar SQL Server en contenedores de Linux para admitir la autenticación de Active Directory (AD), también conocida como autenticación integrada. Para consultar una introducción, vea [Autenticación de Active Directory para SQL Server en Linux](sql-server-linux-active-directory-auth-overview.md).

Este tutorial consta de las tareas siguientes:

> [!div class="checklist"]
> - Instalación de adutil-preview
> - Unión del host de Linux al dominio de AD
> - Creación de un usuario de AD para SQL Server y establecimiento del elemento ServicePrincipalName (SPN) mediante la herramienta adutil
> - Creación del archivo keytab del servicio SQL Server
> - Creación de los archivos mssql.conf y krb5.conf que va a usar el contenedor de SQL Server
> - Montaje de los archivos de configuración e implementación el contenedor de SQL Server
> - Creación de inicios de sesión de SQL Server basados en AD mediante Transact-SQL
> - Conexión a SQL Server mediante la autenticación de AD

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar la autenticación de AD, se necesita lo siguiente:

- Un controlador de dominio de AD (Windows) en la red.
- Instale la herramienta adutil-preview en un equipo host de Linux, que esté unido a un dominio. Para instalar la herramienta adutil-preview, siga las instrucciones de la sección [Instalación de adutil-preview](#install-adutil-preview) siguiente en función de la distribución de Linux que ejecute.

## <a name="container-deployment-and-preparation"></a>Implementación y preparación de contenedores

Para configurar el contenedor, necesitará saber con antelación el puerto que usará el contenedor en el host. El puerto predeterminado, 1433, podría estar asignado de otra manera en el host del contenedor. En este tutorial, el puerto 5433 del host se asignará al puerto 1433 del contenedor. Para obtener más información, vea el inicio rápido [Ejecución de imágenes de contenedor de SQL Server con Docker](quickstart-install-connect-docker.md).

Al registrar nombres de entidad de seguridad de servicio (SPN), puede usar el nombre de host del equipo o el nombre del contenedor, pero debe configurarlo según lo que le gustaría ver al conectarse al contenedor externamente.

Asegúrese de que haya una entrada de host de reenvío (A) agregada en Active Directory para la dirección IP del host de Linux, asignada al nombre del contenedor de SQL Server. En este tutorial, la dirección IP del equipo host `myubuntu` es `10.0.0.10` y el nombre del contenedor de SQL Server es `sql1`. Se agrega la entrada del host de reenvío en Active Directory como se muestra a continuación. La entrada garantiza que cuando los usuarios se conecten a sql1.contoso.com, lleguen al host correcto.

:::image type="content" source="media/sql-server-linux-containers-ad-auth-adutil-tutorial/host-a-record.png" alt-text="Adición del registro de host":::

En este tutorial, se usará un entorno en Azure con tres máquinas virtuales. Una máquina virtual que actúa como controlador de dominio de Windows (DC) con el nombre de dominio `contoso.com`. El nombre del controlador de dominio es `adVM.contoso.com`. La segunda máquina es un equipo Windows denominado `winbox`, que ejecuta Windows 10 Desktop, que se usa como un equipo cliente y tiene instalado SQL Server Management Studio (SSMS). La tercera máquina es un equipo Ubuntu 18.04 LTS denominado `myubuntu`, en el que se hospedan los contenedores de SQL Server. Todas las máquinas se han unido al dominio `contoso.com`. Para obtener más información, vea [Unión de SQL Server en un host de Linux a un dominio de Active Directory](sql-server-linux-active-directory-join-domain.md).

> [!NOTE]
> La unión del equipo contenedor host al dominio no es obligatoria, como puede ver más adelante en este artículo.

## <a name="install-adutil-preview"></a>Instalación de adutil-preview

En el equipo host de Linux, use los comandos siguientes para instalar adutil-preview en función de la distribución de Linux.

> [!NOTE]
> En esta versión preliminar, somos conscientes de que en determinadas distribuciones de Linux, si se intenta la instalación de adutil sin el parámetro `ACCEPT_EULA`, la experiencia de instalación se complica. La recomendación es instalar la herramienta adutil-preview con `ACCEPT_EULA=Y` establecido. Puede leer el [CLUF](https://go.microsoft.com/fwlink/?linkid=2151376) de la versión preliminar antes de la instalación. Estamos trabajando activamente en esto y debería estar corregido en la versión de disponibilidad general.

### <a name="rhel"></a>RHEL

1. Descargue el archivo de configuración del repositorio de Red Hat de Microsoft.

    ```bash
    sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
    ```

1. Si tiene instalada una versión anterior de adutil, quite los paquetes anteriores.

    ```bash
    sudo yum remove adutil
    ```

1. Ejecute los comandos siguientes para instalar adutil-preview. `ACCEPT_EULA=Y` acepta el CLUF de la versión preliminar de adutil. El CLUF se coloca en la ruta de acceso `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. Importe las claves GPG del repositorio público y, luego, registre el repositorio Ubuntu de Microsoft.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. Si tiene instalada una versión anterior de adutil, quite los paquetes anteriores mediante los comandos siguientes.

    ```bash
    sudo apt-get remove adutil
    ```

1. Ejecute el comando siguiente para instalar adutil-preview. `ACCEPT_EULA=Y` acepta el CLUF de la versión preliminar de adutil. El CLUF se coloca en la ruta de acceso `/usr/share/adutil/`.

    ```bash
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y adutil-preview
    ```

### <a name="sles"></a>SLES

1. Agregue el repositorio de Microsoft SQL Server en Zypper.

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo
    ```

1. Si tiene instalada una versión anterior de adutil, quite los paquetes anteriores.

    ```bash
    sudo zypper remove adutil
    ```

1. Ejecute el comando siguiente para instalar adutil-preview. `ACCEPT_EULA=Y` acepta el CLUF de la versión preliminar de adutil. El CLUF se coloca en la ruta de acceso `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="creating-the-ad-user-spns-and-sql-server-service-keytab"></a>Creación del usuario de AD, los SPN y el keytab del servicio SQL Server

Si no quiere que el host contenedor de SQL Server en Linux forme parte del dominio y no ha seguido los pasos para unir el equipo al dominio, siga estos pasos en otro equipo Linux que ya forme parte del dominio de AD:

 1. Cree un usuario de AD para SQL Server y establezca el SPN mediante la herramienta adutil.

 2. Cree y configure el archivo keytab del servicio SQL Server.

Copie el archivo mssql.keytab que se ha creado en el equipo host que va a ejecutar el contenedor de SQL Server y configure el contenedor para usar el archivo mssql.keytab copiado. Opcionalmente, también puede unir el host de Linux que ejecutará el contenedor de SQL Server al dominio de AD y seguir los pasos siguientes en el mismo equipo.

### <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-using-the-adutil-tool"></a>Creación de un usuario de AD para SQL Server y establecimiento del elemento ServicePrincipalName mediante la herramienta adutil

Para habilitar la autenticación de AD en contenedores de SQL Server en Linux es necesario ejecutar los pasos 1 a 3 siguientes en un equipo Linux que forma parte del dominio de AD.

1. Obtenga o renueve el vale de concesión de vales (TGT) de Kerberos mediante el comando `kinit`. Use una cuenta con privilegios para el comando `kinit`. La cuenta debe tener permiso para conectarse al dominio y también debe ser capaz de crear cuentas y SPN en el dominio.

    > [!IMPORTANT]
    > Antes de ejecutar este comando, el host ya debe formar parte del dominio, como se ha mostrado en el paso anterior.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Ejemplo: Para el entorno descrito anteriormente, la cuenta con privilegios es `amvin@CONTOSO.COM`.

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. Con la herramienta adutil, cree el usuario que SQL Server utilizará como cuenta de AD con privilegios.

   ```bash
   adutil user create --name sqluser --distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
   ```

    > [!NOTE]
    > Las contraseñas se pueden especificar de tres maneras:
    >
    > - Marca de contraseña: --password \<password\>
    > - Variables de entorno: `ADUTIL_ACCOUNT_PWD`
    > - Entrada interactiva
    >
    > La prioridad de los métodos de entrada de contraseña sigue el orden de las opciones enumeradas anteriormente. Las opciones recomendadas son para proporcionar la contraseña mediante variables de entorno o entrada interactiva, ya que son más seguras en comparación con la marca de contraseña.

    Puede especificar el nombre de la cuenta mediante el nombre distintivo (`-distname`) como se ha mostrado antes, o también puede usar el nombre de la unidad organizativa (OU). El nombre de la unidad organizativa (`--ou`) tiene prioridad sobre el nombre distintivo en caso de especificar los dos. Puede ejecutar el comando siguiente para obtener más detalles:

    ```bash
    adutil user create --help
    ```

3. Registre los SPN en el usuario creado anteriormente. Si lo prefiere, puede usar el nombre del equipo host en lugar del nombre del contenedor, en función del aspecto externo que quiera para la conexión. En este tutorial, se usa el puerto 5433 en lugar de 1433. Esta es la asignación de puerto para el contenedor. El número de puerto podría ser diferente.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H sql1.contoso.com -p 5433
    ```

    > [!NOTE]
    >
    > - `addauto` creará los SPN automáticamente, siempre y cuando haya suficientes privilegios para la cuenta kinit.
    > - `-n`: nombre de la cuenta a la que se asignarán los SPN.
    > - `-s`: nombre del servicio que se va a usar para generar los SPN. En este caso, es para el servicio SQL Server y, por tanto, el nombre del servicio es MSSQLSvc.
    > - `-H`: nombre de host que se va a usar para generar los SPN. Si no se especifica, se usará el FQDN del host local. Proporcione también el FQDN del nombre del contenedor. En este caso, el nombre del contenedor es `sql1` y el FQDN es `sql1.contoso.com`.
    > - `-p`: puerto que se usa para generar los SPN. Si no se especifica, los SPN se generarán sin un puerto. Las conexiones SQL solo funcionarán en este caso cuando SQL Server escuche en el puerto predeterminado, 1433.

### <a name="create-the-sql-server-service-keytab-file"></a>Creación del archivo keytab del servicio SQL Server

Cree el archivo keytab que contenga entradas para cada uno de los cuatro SPN creados anteriormente y una para el usuario. El archivo keytab se montará en el contenedor, por lo que se puede crear en cualquier ubicación del host. Puede cambiar esta ruta de acceso de forma segura, siempre que el keytab resultante se monte correctamente al usar docker/podman para implementar el contenedor.

Para crear el keytab para todos los SPN, se puede usar la opción `createauto`:

```bash
adutil keytab createauto -k /container/sql1/secrets/mssql.keytab -p 5433 -H sql1.contoso.com --password 'P@ssw0rd' -s MSSQLSvc
```

> [!NOTE]
>
> - `-k`: ruta de acceso en la que se quiere crear el archivo `mssql.keytab`. En el ejemplo anterior, el directorio "/container/sql1/secrets" ya debe existir en el host.
> - `-p`: puerto que se usa para generar los SPN. Si no se especifica, los SPN se generarán sin un puerto.
> - `-H`: nombre de host que se va a usar para generar los SPN. Si no se especifica, se usará el FQDN del host local. Proporcione también el FQDN del nombre del contenedor. En este caso, el nombre del contenedor es `sql1` y el FQDN es `sql1.contoso.com`.
> - `-s`: nombre del servicio que se va a usar para generar los SPN. En este caso, es para el servicio SQL Server y, por tanto, el nombre del servicio es MSSQLSvc.
> - `--password`: la contraseña de la cuenta de usuario de AD con privilegios que se ha creado antes.
> - `-e` o `--enctype`: tipos de cifrado para la entrada de keytab. Use una lista de valores separados por comas. Si no se especifica, se presentará un mensaje interactivo.

Cuando se le ofrezca una opción para elegir los tipos de cifrado, puede elegir más de uno. En este ejemplo, se han elegido `aes256-cts-hmac-sha1-96` y `arcfour-hmac`. Asegúrese de elegir el tipo de cifrado que sea compatible con el host y el dominio.

Si prefiere elegir el tipo de cifrado de forma no interactiva, puede especificarlo con el argumento -e en el comando anterior. Para obtener más ayuda sobre los comandos de adutil, ejecute el comando siguiente.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` es un cifrado débil y no es un tipo recomendado para usarlo en un entorno de producción.

Para crear el keytab para el usuario, el comando es el siguiente:

```bash
adutil keytab create -k /container/sql1/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`: ruta de acceso en la que se quiere crear el archivo `mssql.keytab`. En el ejemplo anterior, el directorio "/container/sql1/secrets" ya debe existir en el host.
> - `-p`: entidad de seguridad que se va a agregar al keytab.

El keytab create/autocreate de adutil no sobrescribe los archivos anteriores, simplemente se anexa al archivo si ya están presentes.

Asegúrese de que el keytab que se crea tiene los permisos correctos establecidos al implementar el contenedor.

```bash
chmod 440 /container/sql1/secrets/mssql.keytab
```

> [!NOTE]
> En este momento, puede copiar el archivo mssql.keytab del host de Linux actual en el host de Linux donde se implementará el contenedor de SQL Server y seguir el resto de los pasos en el host de Linux que ejecutará el contenedor de SQL Server. Si los pasos anteriores se han realizado en el mismo host de Linux donde se implementarán los contenedores de SQL, siga también los pasos siguientes en el mismo host.

## <a name="create-the-config-files-to-be-used-by-the-sql-server-container"></a>Creación de los archivos de configuración que va a usar el contenedor de SQL Server

1. Cree un archivo `mssql.conf` con la configuración de AD. Este archivo se puede crear en cualquier parte del host y se debe montar correctamente durante el comando docker run. En este ejemplo, este archivo `mssql.conf` se coloca en `/container/sql1 `, que es el directorio contenedor. El contenido de `mssql.conf` es el siguiente:

    ```output
    [network]
    privilegedadaccount = sqluser
    kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
    ```

    > [!NOTE]
    >
    > - `privilagedadaccount`: Usuario con privilegios de AD que se usará para la autenticación de AD.
    > - `kerberoskeytabfile`: ruta de acceso en el contenedor donde se ubicará el archivo mssql.keytab.

1. Cree un archivo krb5.conf. A continuación se muestra un ejemplo. En estos archivos es importante el uso de mayúsculas y minúsculas.

    ```output
    [libdefaults]
    default_realm = DOMAIN.COM

    [realms]
     CONTOSO.COM = {
         kdc = adVM.contoso.com
         admin_server = adVM.contoso.com
         default_domain = CONTOSO.COM
     }

    [domain_realm]
     .contoso.com = CONTOSO.COM
     contoso.com = CONTOSO.COM


1. Copy all files, `mssql.conf`, `krb5.conf`, `mssql.keytab` to a location that will be mounted to the SQL Server container. In this example, these files are placed on the host at the following locations: `mssql.conf` and `krb5.conf` at `/container/sql1/`. `mssql.keytab` is placed at the location `/container/sql1/secrets/`.

1. Make sure there's enough permission on these folders for the user running the docker/podman command. When the container starts, the user needs access to the folder path created. In this example, we provided the below permissions given to the folder path:

    ```bash
    sudo chmod 755 /container/sql1/
    ```

## <a name="mount-the-config-files-and-deploy-the-sql-server-container"></a>Montaje de los archivos de configuración e implementación del contenedor de SQL Server

Ejecute el contenedor de SQL Server y monte los archivos de configuración de AD correctos que se han creado antes, como se muestra a continuación:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" \
-p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
> Al ejecutar el contenedor en LSM (Módulo de seguridad de Linux) como hosts habilitados para SELinux, debe montar los volúmenes con la opción `Z`, que indica a Docker que etiquete el contenido con una etiqueta privada sin compartir. Para obtener más información, vea [Configuración de la etiqueta de Linux SE](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label).

El ejemplo contendrá los comandos siguientes:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql/ \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
--dns-search contoso.com \
--dns 10.0.0.4 \
--add-host adVM.contoso.com:10.0.0.4 \
--add-host contoso.com:10.0.0.4 \
--add-host contoso:10.0.0.4 \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
>
> - Los archivos `mssql.conf` y `krb5.conf` se encuentran en la ruta de acceso del archivo de host `/container/sql1`.
> - La instancia de `mssql.keytab` que se ha creado se encuentra en la ruta de acceso del archivo de host `/container/sql1/secrets`.
> - Como el equipo host está en Azure, es necesario anexar al comando docker run los detalles de AD en el mismo orden. En el ejemplo, el controlador de dominio `adVM` está en el dominio `contoso.com`, con una dirección IP de `10.0.0.4`. El controlador de dominio ejecuta DNS y KDC.

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Creación de inicios de sesión de SQL Server basados en AD en Transact-SQL

Conéctese al contenedor SQL, ejecute los comandos siguientes para crear el inicio de sesión y confirme que se muestra. Puede ejecutar este comando desde un equipo cliente (Windows o Linux) que ejecute SSMS, Azure Data Studio (ADS) o cualquier otra herramienta de la interfaz de la línea de comandos (CLI).

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Conectarse a SQL Server mediante la autenticación de AD.

Para conectarse mediante [SSMS](../ssms/download-sql-server-management-studio-ssms.md) o [ADS](../azure-data-studio/download-azure-data-studio.md), inicie sesión en SQL Server con las credenciales de Windows con el nombre y el número de puerto de SQL Server (el nombre podría ser el nombre del contenedor o el del host). En el ejemplo, el nombre del servidor sería `sql1.contoso.com, 5433`.

También puede usar una herramienta como [sqlcmd](../tools/sqlcmd-utility.md) para conectarse a SQL Server en el contenedor.

```bash
sqlcmd -E -S 'sql1.contoso.com, 5433'
```

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: ejecución de imágenes de contenedor de SQL Server con Docker](quickstart-install-connect-docker.md)
- [Unión de SQL Server en un host de Linux a un dominio de Active Directory](sql-server-linux-active-directory-auth-overview.md)
