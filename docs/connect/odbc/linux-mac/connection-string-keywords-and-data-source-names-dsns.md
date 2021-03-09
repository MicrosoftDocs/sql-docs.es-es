---
title: Conexión desde Linux o macOS
description: Obtenga información sobre cómo crear una conexión a una base de datos desde Linux o macOS mediante Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 09/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connect to linux
- configure odbc.ini
ms.assetid: f95cdbce-e7c2-4e56-a9f7-8fa3a920a125
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1bdbcbb34be9cbfa075ead7e1cd03ec813a5d9d
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247309"
---
# <a name="connecting-from-linux-or-macos"></a>Conexión desde Linux o macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

En este tema se describe cómo crear una conexión a una base de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="connection-properties"></a>Propiedades de conexión  

Consulte [Atributos y palabras clave de cadena de conexión y DSN](../dsn-connection-string-attribute.md) para ver todos los atributos y palabras clave de cadena de conexión admitidos en Linux y macOS.

> [!IMPORTANT]  
> Al conectarse a una base de datos que usa la creación de reflejo de la base de datos (tiene un asociado de conmutación por error), no especifique el nombre de la base de datos en la cadena de conexión. En su lugar, envíe un comando **use** _database_name_ para conectarse a la base de datos antes de ejecutar las consultas.  
  
El valor transmitido a la palabra clave **Driver** puede ser uno de los siguientes:  
  
- El nombre que usó cuando instaló el controlador.

- La ruta de acceso a la biblioteca del controlador, que se especificó en el archivo .ini de plantilla utilizado para instalar el controlador.  

Los DSN son opcionales. Puede usar un DSN para definir palabras clave de cadena de conexión bajo un nombre `DSN` al que puede hacer referencia en la cadena de conexión. Para crear un DSN, cree (si es necesario) y edite el archivo **~/.odbc.ini** (`.odbc.ini` en su directorio principal) para un DSN de usuario accesible solo para el usuario actual, o `/etc/odbc.ini` para un DSN de sistema (se requieren privilegios administrativos). A continuación se muestra un archivo de ejemplo que muestra las entradas requeridas mínimas para un DSN:  

```ini
# [DSN name]
[MSSQLTest]  
Driver = ODBC Driver 17 for SQL Server  
# Server = [protocol:]server[,port]  
Server = tcp:localhost,1433
#
# Note:  
# Port is not a valid keyword in the odbc.ini file  
# for the Microsoft ODBC driver on Linux or macOS
#  
```  

Para conectarse con el DSN anterior en una cadena de conexión, debe especificar la palabra clave `DSN`, como en este caso: `DSN=MSSQLTest;UID=my_username;PWD=my_password`  
La cadena de conexión anterior sería equivalente a especificar una cadena de conexión sin la palabra clave `DSN`, como en este caso: `Driver=ODBC Driver 17 for SQL Server;Server=tcp:localhost,1433;UID=my_username;PWD=my_password`

También puede especificar el protocolo y el puerto para conectarse al servidor. Por ejemplo, **Server=tcp:** _servername_ **,12345**. Tenga en cuenta que el único protocolo compatible con los controladores de Linux y macOS es `tcp`.

Para conectarse a una instancia con nombre en un puerto estático, use <b>Server=</b>*servername*,**port_number**. No se admite la conexión a un puerto dinámico antes de la versión 17.4.

De forma alternativa, puede agregar la información de DSN a un archivo de plantilla y ejecutar el siguiente comando para agregarlo a `~/.odbc.ini`:
 - **odbcinst -i -s -f** _template_file_  

Para obtener la documentación completa sobre los archivos .ini y `odbcinst`, consulte la [documentación de unixODBC](http://www.unixodbc.org/odbcinst.html). Para las entradas del archivo `odbc.ini` específico del controlador ODBC para SQL Server, vea [Atributos y palabras clave de cadena de conexión y DSN](../dsn-connection-string-attribute.md) para las que sean compatibles con Linux y macOS.

Puede comprobar que el controlador funciona mediante `isql` para probar la conexión, o puede usar este comando:
 - **bcp master.INFORMATION_SCHEMA.TABLES out OutFile.dat -S <server> -U <name> -P <password>**  

## <a name="using-tlsssl"></a>Uso de TLS/SSL  

Puede usar la Seguridad de la capa de transporte (TLS), antes conocida como Capa de sockets seguros (SSL), para cifrar las conexiones con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Este protocolo protege los nombres de usuario y las contraseñas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a través de la red. TLS también comprueba la identidad del servidor para protegerse de ataques de tipo "Man in the middle" (MITM).  

Al habilitar el cifrado, aumenta la seguridad a expensas del rendimiento.

Para obtener más información, consulte [Cifrar las conexiones a SQL Server](/previous-versions/sql/sql-server-2008-r2/ms189067(v=sql.105)) y [Utilizar el cifrado sin validación](../../../relational-databases/native-client/features/using-encryption-without-validation.md).

Con independencia de la configuración de **Encrypt** y **TrustServerCertificate**, siempre se cifran las credenciales de inicio de sesión del servidor (nombre de usuario y contraseña). En la tabla siguiente se muestra el efecto de la configuración de **Encrypt** y **TrustServerCertificate** .  

||**TrustServerCertificate=no**|**TrustServerCertificate=yes**|  
|-|-------------------------------------|------------------------------------|  
|**Encrypt=no**|No se comprueba el certificado de servidor.<br /><br />No se cifran los datos enviados entre cliente y servidor.|No se comprueba el certificado de servidor.<br /><br />No se cifran los datos enviados entre cliente y servidor.|  
|**Encrypt=yes**|Se comprueba el certificado de servidor.<br /><br />Se cifran los datos enviados entre cliente y servidor.<br /><br />El nombre (o la dirección IP) de un nombre común (CN) del firmante o el nombre alternativo del firmante (SAN) de un certificado TLS/SSL de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe coincidir exactamente con el nombre (o la dirección IP) del servidor especificado en la cadena de conexión.|No se comprueba el certificado de servidor.<br /><br />Se cifran los datos enviados entre cliente y servidor.|  

De forma predeterminada, las conexiones cifradas siempre comprueban el certificado del servidor. Sin embargo, si se conecta a un servidor que tiene un certificado autofirmado, agregue también la opción `TrustServerCertificate` para omitir la comprobación del certificado con respecto a la lista de entidades de certificación de confianza:  

```
Driver={ODBC Driver 17 for SQL Server};Server=ServerNameHere;Encrypt=YES;TrustServerCertificate=YES  
```  
  
TLS usa la biblioteca OpenSSL. En la siguiente tabla se muestran las versiones mínimas admitidas de OpenSSL y las ubicaciones de almacén de confianza de certificados predeterminadas para cada plataforma:

|Plataforma|Versión mínima de OpenSSL|Ubicación del almacén de confianza de certificados predeterminada|  
|------------|---------------------------|--------------------------------------------|
|Debian 10|1.1.1|/etc/ssl/certs|
|Debian 9|1.1.0|/etc/ssl/certs|
|Debian 8.71|1.0.1|/etc/ssl/certs|
|OS X 10.11, macOS 10.12-10.15|1.0.2|/usr/local/etc/openssl/certs|
|Red Hat Enterprise Linux 8|1.1.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 7|1.0.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 6|1.0.0-10|/etc/pki/tls/cert.pem|
|SUSE Linux Enterprise 15|1.1.0|/etc/ssl/certs|
|SUSE Linux Enterprise 11, 12|1.0.1|/etc/ssl/certs|
|Ubuntu 18.10, 19.04, 19.10, 20.04|1.1.1|/etc/ssl/certs|
|Ubuntu 18.04|1.1.0|/etc/ssl/certs|
|Ubuntu 16.04, 16.10, 17.10|1.0.2|/etc/ssl/certs|
|Ubuntu 14.04|1.0.1|/etc/ssl/certs|

También puede especificar el cifrado en la cadena de conexión mediante la opción `Encrypt` al usar **SQLDriverConnect** para conectarse.

## <a name="adjusting-the-tcp-keep-alive-settings"></a>Ajuste de la configuración de conexión persistente TCP

A partir de ODBC Driver 17.4, la frecuencia con la que el controlador envía paquetes de conexión persistente y los retransmite cuando no se recibe una respuesta es configurable.
Para configurarla, agregue los siguientes valores a la sección del controlador en `odbcinst.ini` o a la sección del DNS en `odbc.ini`. Al conectarse con un DSN, el controlador usará los valores de la sección del DSN si está presente; de lo contrario, o solo si se conecta con una cadena de conexión, utilizará los valores de la sección del controlador en `odbcinst.ini`. Si el valor no está presente en ninguna ubicación, el controlador usará el valor predeterminado.

- `KeepAlive=<integer>` controla la frecuencia con la que TCP intenta comprobar que una conexión inactiva sigue intacta mediante el envío de un paquete de conexión persistente. El valor predeterminado es **treinta** segundos.

- `KeepAliveInterval=<integer>` determina el intervalo que separa las retransmisiones de conexión persistente hasta que se recibe una respuesta.  El valor predeterminado es **1** segundo.

## <a name="see-also"></a>Consulte también

- [Instalación de Microsoft ODBC Driver for SQL Server en Linux](installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Instalación de Microsoft ODBC Driver for SQL Server en macOS](install-microsoft-odbc-driver-sql-server-macos.md)
- [Instrucciones de programación](programming-guidelines.md)
