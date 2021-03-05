---
title: Cifrado de conexiones a SQL Server en Linux
description: SQL Server en Linux usa TLS para cifrar los datos transmitidos a través de una red entre una aplicación cliente y una instancia de SQL Server.
ms.date: 06/29/2020
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 1040fec65e6ffce580a33b6ef581c4f22a3903ef
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186419"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Cifrado de conexiones a SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux puede usar Seguridad de la capa de transporte (TLS) para cifrar los datos transmitidos a través de una red entre una aplicación cliente y una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] admite los mismos protocolos TLS en Windows y Linux: TLS 1.2, 1.1 y 1.0. Aun así, los pasos para configurar TLS son específicos del sistema operativo en el que se ejecuta [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

## <a name="requirements-for-certificates"></a>Requisitos de los certificados 
Antes de empezar, debe asegurarse de que los certificados cumplen estos requisitos:
- La hora actual del sistema debe ser posterior al valor de la propiedad Válido desde del certificado y anterior a la propiedad Válido hasta del certificado.
- El certificado debe estar destinado a la autenticación del servidor. Para esto es necesario que la propiedad del certificado Uso mejorado de clave especifique Autenticación del servidor (1.3.6.1.5.5.7.3.1).
- El certificado se debe crear mediante la opción KeySpec de AT_KEYEXCHANGE. Por lo general, la propiedad de uso de clave del certificado (KEY_USAGE) incluye también el cifrado de clave (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- La propiedad Asunto del certificado debe indicar que el nombre común (CN) es el mismo que el nombre del host o nombre de dominio completo (FQDN) del equipo servidor. Nota: Se admiten certificados comodín.

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>Configuración de bibliotecas OpenSSL para su uso (opcional)
Puede crear vínculos simbólicos en el directorio `/opt/mssql/lib/` que hagan referencia a qué bibliotecas `libcrypto.so` y `libssl.so` que se deben usar para el cifrado. Esto resulta útil si quiere forzar SQL Server a usar una versión específica de OpenSSL distinta del valor predeterminado que proporciona el sistema. Si estos vínculos simbólicos no están presentes, SQL Server cargará en el sistema las bibliotecas configuradas predeterminadas de OpenSSL.

Estos vínculos simbólicos deben denominarse `libcrypto.so` y `libssl.so` y colocarse en el directorio `/opt/mssql/lib/`.

## <a name="overview"></a>Información general
TLS se usa para cifrar las conexiones de una aplicación cliente a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Cuando se configura correctamente, TLS proporciona privacidad e integridad de datos para las comunicaciones entre el cliente y el servidor.  Tanto el cliente como el servidor pueden iniciar conexiones TLS. 

## <a name="client-initiated-encryption"></a>Cifrado iniciado por el cliente 
- **Generación del certificado** (/CN debe coincidir con el nombre de dominio completo del host de SQL Server)

> [!NOTE]
> En este ejemplo, se usa un certificado autofirmado, que no debe emplearse en escenarios de producción. Debe usar certificados de CA.<br>
> Asegúrese de que el grupo o el usuario mssql puedan acceder a la carpeta donde se guardan los certificados y las claves privadas, y que tiene el permiso establecido en 700 (drwx-----). Puede crear carpetas manualmente con el permiso establecido en 700 (drwx------) y que sean propiedad del usuario o grupo mssqlL, o bien establecer el permiso en 755 (drwxr-xr-x) y que sean propiedad de otro usuario, pero todavía accesibles para el grupo de usuarios mssql. Por ejemplo, puede crear una carpeta denominada "sslcert" en la ruta de acceso "/var/opt/mssql/" y, después, guardar el certificado y la clave privada con permisos en los archivos establecidos en 600 como se muestra a continuación. 

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key 
# in this case we are saving the certificate to the certs folder under /etc/ssl/ which has the following permission 755(drwxr-xr-x)
sudo mv mssql.pem /etc/ssl/certs/ drwxr-xr-x 
# in this case we are saving the private key to the private folder under /etc/ssl/ with permissions set to 755(drwxr-xr-x)
sudo mv mssql.key /etc/ssl/private/ 
```

- **Configuración de SQL Server**

```bash
systemctl stop mssql-server 
sudo cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 
systemctl start mssql-server 
```

- **Registro del certificado en el equipo cliente (Windows, Linux o macOS)**

    -   Si usa un certificado firmado de CA, debe copiar en el equipo cliente el certificado de la entidad de certificación (CA), en lugar del certificado de usuario. 
    -   Si usa el certificado autofirmado, solo tiene que copiar el archivo .pem en las siguientes carpetas correspondientes a la distribución y ejecutar los comandos para habilitarlas. 
        - **Ubuntu**: copie el certificado en `/usr/share/ca-certificates/`, cambie su extensión a .crt y use `dpkg-reconfigure ca-certificates` para habilitarlo como certificado de CA del sistema. 
        - **RHEL**: copie el certificado en `/etc/pki/ca-trust/source/anchors/` y use `update-ca-trust` para habilitarlo como certificado de CA del sistema.
        - **SUSE**: copie el certificado en `/usr/share/pki/trust/anchors/` y use `update-ca-certificates` para habilitarlo como certificado de CA del sistema.
        - **Windows**:  importe el archivo .pem como certificado en usuario actual -> entidades de certificación raíz de confianza -> certificados.
        - **macOS**: 
           - copie el certificado en `/usr/local/etc/openssl/certs`.
           - Ejecute el comando siguiente para obtener el valor hash: `/usr/local/Cellar/openssl/1.0.2l/openssl x509 -hash -in mssql.pem -noout`.
           - Cambie el nombre del certificado al valor. Por ejemplo: `mv mssql.pem dc2dd900.0`. Asegúrese de que dc2dd900.0 está en `/usr/local/etc/openssl/certs`.
    
-   **Ejemplos de cadena de conexión** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![Cuadro de diálogo de conexión SSMS](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "Cuadro de diálogo de conexión SSMS")  
  
    - **SQLCMD** 

        `sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=True; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=Yes; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=true; trustServerCertificate=false;"`

## <a name="server-initiated-encryption"></a>Cifrado iniciado por el servidor 

- **Generación del certificado** (/CN debe coincidir con el nombre de dominio completo del host de SQL Server)

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key   
sudo mv mssql.pem /etc/ssl/certs/ 
sudo mv mssql.key /etc/ssl/private/ 
```

- **Configuración de SQL Server**

```bash
systemctl stop mssql-server 
sudo cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1
systemctl start mssql-server 
```

-   **Ejemplos de cadena de conexión** 

    - **SQLCMD**

        `sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=False; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=no; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=false; trustServerCertificate=false;"`

> [!NOTE]
> Establezca **TrustServerCertificate** en True si el cliente no se puede conectar a la CA para validar la autenticidad del certificado.

## <a name="common-connection-errors"></a>Errores de conexión comunes  

|Mensaje de error |Fix |
|--- |--- |
|Una entidad que no es de confianza ha emitido la cadena de certificados.  |Este error se produce cuando los clientes no pueden comprobar la firma del certificado que ha presentado SQL Server durante el protocolo de enlace de TLS. Asegúrese de que el cliente confía directamente en el certificado [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o en la CA que firmó el certificado de SQL Server. |
|El nombre de la entidad de seguridad de destino es incorrecto.  |Asegúrese de que el campo Nombre común del certificado de SQL Server coincide con el nombre de servidor especificado en la cadena de conexión del cliente. |  
|El host remoto forzó el cierre de la conexión existente. |Este error puede producirse si el cliente no admite la versión del protocolo TLS que requiere SQL Server. Por ejemplo, si [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] está configurado para requerir TLS 1.2, asegúrese de que los clientes también admitan el protocolo TLS 1.2. |
| | |   
