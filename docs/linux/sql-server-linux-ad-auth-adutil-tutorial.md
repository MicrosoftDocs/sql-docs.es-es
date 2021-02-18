---
title: Configuración de la autenticación de Active Directory con SQL Server en Linux mediante adutil
description: Instrucciones paso a paso sobre cómo configurar la autenticación de Active Directory con SQL Server en Linux mediante adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: e1e14db6e55ca4af4f76220c0e3f377c38e41165
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351877"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux-using-adutil"></a>Tutorial: Configuración de la autenticación de Active Directory con SQL Server en Linux mediante adutil

> [!NOTE]
> Actualmente, **adutil** está en **versión preliminar pública**.

En este tutorial se explica cómo configurar la autenticación de Active Directory (AD) para SQL Server en Linux mediante adutil. Para obtener otro método de configuración de la autenticación de AD mediante ktpass, vea [Tutorial: usar la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md).

Este tutorial consta de las tareas siguientes:

> [!div class="checklist"]
> - Instalación de adutil-preview
> - Unión de un equipo Linux al dominio de AD
> - Creación de un usuario de AD para SQL Server y establecimiento del elemento ServicePrincipalName (SPN) mediante la herramienta adutil
> - Creación del archivo keytab del servicio SQL Server
> - Configuración de SQL Server para usar el archivo keytab
> - Creación de inicios de sesión de SQL Server basados en AD mediante Transact-SQL
> - Conexión a SQL Server mediante la autenticación de AD

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar la autenticación de AD, se necesita lo siguiente:

- Un controlador de dominio de AD (Windows) en la red.
- Instale la herramienta adutil-preview en un equipo host de Linux. Para instalar adutil-preview, siga las instrucciones de la sección siguiente en función de la distribución de Linux que ejecute.

## <a name="install-adutil-preview"></a>Instalación de adutil-preview

En el equipo host de Linux, use los comandos siguientes para instalar adutil-preview.

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

1. Ejecute los comandos siguientes para instalar adutil-preview. `ACCEPT_EULA=Y` acepta el CLUF de la versión preliminar de adutil. El CLUF se coloca en la ruta de acceso "/usr/share/adutil/".

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

1. Ejecute el comando siguiente para instalar adutil-preview. `ACCEPT_EULA=Y` acepta el CLUF de la versión preliminar de adutil. El CLUF se coloca en la ruta de acceso "/usr/share/adutil/".

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

1. Ejecute el comando siguiente para instalar adutil-preview. `ACCEPT_EULA=Y` acepta el CLUF de la versión preliminar de adutil. El CLUF se coloca en la ruta de acceso "/usr/share/adutil/".

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="domain-machine-preparation"></a>Preparación del equipo de dominio

Asegúrese de que haya una entrada de host de reenvío (A) agregada en Active Directory para la dirección IP del host de Linux. En este tutorial, la dirección IP del equipo host `myubuntu` es `10.0.0.10`. Se agrega la entrada del host de reenvío en Active Directory como se muestra a continuación. La entrada garantiza que cuando los usuarios se conecten a myubuntu.contoso.com, lleguen al host correcto.

:::image type="content" source="media/sql-server-linux-ad-auth-adutil-tutorial/host-a-record.png" alt-text="Adición del registro de host":::

En este tutorial, se usará un entorno en Azure con tres máquinas virtuales. Una máquina virtual que actúa como controlador de dominio de Windows (DC) con el nombre de dominio `contoso.com`. El nombre del controlador de dominio es `adVM.contoso.com`. La segunda máquina es un equipo Windows denominado `winbox`, que ejecuta Windows 10 Desktop, que se usa como un equipo cliente y tiene instalado SQL Server Management Studio (SSMS). La tercera máquina es un equipo Ubuntu 18.04 LTS denominado `myubuntu`, en el que se hospeda SQL Server.

## <a name="join-the-linux-host-machine-to-your-ad-domain"></a>Unión del equipo host de Linux al dominio de AD

Una el host Linux de SQL Server con un controlador de dominio de Active Directory. Para obtener información sobre cómo unir un dominio de Active Directory, consulte [Unión de SQL Server en un host de Linux a un dominio de Active Directory](sql-server-linux-active-directory-join-domain.md).

## <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-spn-using-the-adutil-tool"></a>Creación de un usuario de AD para SQL Server y establecimiento del elemento ServicePrincipalName (SPN) mediante la herramienta adutil

1. Obtenga o renueve el vale de concesión de vales (TGT) de Kerberos mediante el comando `kinit`. Use una cuenta con privilegios para el comando `kinit`. La cuenta debe tener permiso para conectarse al dominio y también debe ser capaz de crear cuentas y SPN en el dominio.

    > [!IMPORTANT]
    > Antes de ejecutar este comando, el equipo host ya debe formar parte del dominio, como se ha mostrado en el paso anterior.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Ejemplo: Para el entorno descrito anteriormente, la cuenta con privilegios es `amvin@CONTOSO.COM`.

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. Con la herramienta adutil, cree el usuario que SQL Server utilizará como cuenta de AD con privilegios.

   ```bash
   adutil user create --name sqluser -distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
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

3. Registre los SPN en la entidad de seguridad que se ha creado antes. Use el FQDN del equipo. En este tutorial, se usará el puerto predeterminado de SQL Server, 1433. El número de puerto podría ser diferente.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H myubuntu.contoso.com -p 1433
    ```

    > [!NOTE]
    >
    > - `addauto` creará los SPN automáticamente, siempre y cuando haya suficientes privilegios para la cuenta kinit.
    > - `-n`: nombre de la cuenta a la que se asignarán los SPN.
    > - `-s`: nombre del servicio que se va a usar para generar los SPN. En este caso, es para el servicio SQL Server y, por tanto, el nombre del servicio es `MSSQLSvc`.
    > - `-H`: nombre de host que se va a usar para generar los SPN. Si no se especifica, se usará el FQDN del host local. En este caso, el nombre de host es `myubuntu` y el FQDN es `myubuntu.contoso.com`.
    > - `-p`: puerto que se usa para generar los SPN. Si no se especifica, los SPN se generarán sin un puerto. Las conexiones SQL solo funcionarán en este caso cuando SQL Server escuche en el puerto predeterminado, 1433.

## <a name="create-the-sql-server-service-keytab-file"></a>Creación del archivo keytab del servicio SQL Server

Cree el archivo keytab que contenga entradas para cada uno de los cuatro SPN creados anteriormente y una para el usuario.

```bash
adutil keytab createauto -k /var/opt/mssql/secrets/mssql.keytab -p 1433 -H myubuntu.contoso.com --password 'P@ssw0rd' -s MSSQLSvc 
```

> [!NOTE]
>
> - `-k`: ruta de acceso en la que se quiere crear el archivo `mssql.keytab`. En el ejemplo anterior, el directorio `/var/opt/mssql/secrets/` ya debe existir en el host.
> - `-p`: puerto que se usa para generar los SPN. Si no se especifica, los SPN se generarán sin un puerto.
> - `-H`: nombre de host que se va a usar para generar los SPN. Si no se especifica, se usará el FQDN del host local. En este caso, el nombre de host es `myubuntu` y el FQDN es `myubuntu.contoso.com`.
> - `-s`: nombre del servicio que se va a usar para generar los SPN. En este caso, es para el servicio SQL Server y, por tanto, el nombre del servicio es `MSSQLSvc`.
> - `--password`: la contraseña de la cuenta de usuario de AD con privilegios que se ha creado antes.
> - `-e` o `--enctype`: tipos de cifrado para la entrada de keytab. Use una lista de valores separados por comas. Si no se especifica, se presentará un mensaje interactivo.

Cuando se le ofrezca una opción para elegir los tipos de cifrado, puede elegir más de uno. En este ejemplo, se han elegido `aes256-cts-hmac-sha1-96` y `arcfour-hmac`. Asegúrese de elegir el tipo de cifrado que sea compatible con el host y el dominio.

Si prefiere elegir el tipo de cifrado de forma no interactiva, puede especificarlo con el argumento -e en el comando anterior. Para obtener más ayuda sobre los comandos de adutil, ejecute el comando siguiente.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` es un cifrado débil y no es un tipo recomendado para usarlo en un entorno de producción.

Agregue una entrada en el archivo keytab para el nombre de la entidad de seguridad y su contraseña, que SQL Server usará para conectarse a AD:

```bash
adutil keytab create -k /var/opt/mssql/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`: ruta de acceso en la que se quiere crear el archivo `mssql.keytab`.
> - `-p`: entidad de seguridad que se va a agregar al keytab.

El keytab create/autocreate de adutil no sobrescribe los archivos anteriores, simplemente se anexa al archivo si ya están presentes.

Asegúrese de que el keytab que se crea es propiedad del usuario `mssql` y de que solo el usuario `mssql` tiene acceso de lectura y escritura al archivo. Puede ejecutar los comandos `chown` y `chmod` como se muestra a continuación:

```bash
chown mssql. /var/opt/mssql/secrets/mssql.keytab
chmod 440 /var/opt/mssql/secrets/mssql.keytab
```

## <a name="configure-sql-server-to-use-the-keytab"></a>Configuración de SQL Server para usar el keytab

Ejecute los comandos siguientes a fin de configurar SQL Server para que use el keytab creado en el paso anterior y establezca la cuenta de AD con privilegios como el usuario creado antes. En el ejemplo, el nombre de usuario es `sqluser`.

```bash
/opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
/opt/mssql/bin/mssql-conf set network.privilegedadaccount sqluser
```

## <a name="restart-sql-server"></a>Reinicio de SQL Server

Ejecute el comando siguiente para reiniciar el servicio SQL Server:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Creación de inicios de sesión de SQL Server basados en AD en Transact-SQL

Conéctese a SQL Server, ejecute los comandos siguientes para crear el inicio de sesión y confirme que se muestra.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Conectarse a SQL Server mediante la autenticación de AD.

Para conectarse mediante [SSMS](../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), inicie sesión en SQL Server con las credenciales de Windows.

También puede usar una herramienta como [sqlcmd](../tools/sqlcmd-utility.md) para conectarse a SQL Server mediante la autenticación de Windows.

```bash
sqlcmd -E -S 'myubuntu.contoso.com'
```

## <a name="next-steps"></a>Pasos siguientes

- [Unión de SQL Server en un host de Linux a un dominio de Active Directory](sql-server-linux-active-directory-auth-overview.md)
- Si le interesa la configuración de la autenticación de AD con contenedores de SQL Server en Linux, vea [Configuración de la autenticación de Active Directory con contenedores de SQL Server en Linux](sql-server-linux-containers-ad-auth-adutil-tutorial.md).
