---
title: Uso de la autenticación integrada
description: Microsoft ODBC Driver for SQL Server en Linux y macOS admite conexiones que usan la autenticación integrada de Kerberos.
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- integrated authentication
ms.assetid: 9499ffdf-e0ee-4d3c-8bca-605371eb52d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4e918602b3793d95d6192a832f110500454ee8a5
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837380"
---
# <a name="using-integrated-authentication"></a>Uso de la autenticación integrada
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

[!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en Linux y macOS admite conexiones que usan la autenticación integrada de Kerberos. Es compatible con el centro de distribución de claves (KDC) de Kerberos del MIT y funciona con API de servicios de seguridad genéricos (GSSAPI) y bibliotecas de Kerberos v5.

A partir de la versión 17.6, el controlador también admite la autenticación integrada con Azure Active Directory mediante una cuenta federada, sin tener en cuenta las limitaciones de la biblioteca del sistema. Para obtener más información, vea [Uso de Azure Active Directory](../using-azure-active-directory.md).

## <a name="using-integrated-authentication-to-connect-to-ssnoversion-from-an-odbc-application"></a>Uso de la autenticación integrada para conectarse a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] desde una aplicación ODBC  

Puede habilitar la autenticación integrada de Kerberos si especifica **Trusted_Connection = yes** en la cadena de conexión de **SQLDriverConnect** o **SQLConnect**. Por ejemplo:  

```
Driver='ODBC Driver 17 for SQL Server';Server=your_server;Trusted_Connection=yes  
```
  
Al conectarse con un DSN, también puede agregar **Trusted_Connection= =yes** a la entrada DSN en `odbc.ini`.
  
La opción `-E` de `sqlcmd` y la opción `-T` de `bcp` también pueden utilizarse para especificar la autenticación integrada. Para obtener más información, vea [Conectarse con **sqlcmd**](connecting-with-sqlcmd.md) y [ Conectarse con **bcp**](connecting-with-bcp.md).

Asegúrese de que el servidor principal de Linux que se va a conectar a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ya esté autenticado con el KDC de Kerberos.
  
No se admiten **ServerSPN** ni **FailoverPartnerSPN** .  
  
## <a name="deploying-a-linux-or-macos-odbc-driver-application-designed-to-run-as-a-service"></a>Implementación de un controlador ODBC de Linux o MacOS en una aplicación diseñada para ejecutarse como un servicio

Un administrador del sistema puede implementar una aplicación para que se ejecute como un servicio que usa la autenticación Kerberos para conectarse a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
Primero, debe configurar Kerberos en el cliente y, luego, asegurarse de que la aplicación puede usar la credencial Kerberos de la entidad de seguridad predeterminada.

Asegúrese de que usa `kinit` o PAM (módulo de autenticación acoplable) para obtener y almacenar en caché el TGT de la entidad de seguridad que usa la conexión, con uno de los siguientes métodos:  
  
-   Ejecute `kinit` y transmita el nombre y la contraseña de una entidad de seguridad.  
  
-   Ejecute `kinit` y transmita el nombre de una entidad de seguridad y una ubicación de un archivo keytab que contiene la clave de la entidad de seguridad, creada por `ktutil`.  
  
-   Asegúrese de que se ha realizado el inicio de sesión en el sistema mediante el PAM (módulo de autenticación acoplable) de Kerberos.

Cuando una aplicación se ejecuta como un servicio, puesto que las credenciales de Kerberos expiran por diseño, renueve las credenciales para garantizar la disponibilidad continua del servicio. El controlador ODBC no renueva las propias credenciales. Asegúrese de que haya un trabajo `cron` o script que se ejecute periódicamente para renovar las credenciales antes de que expiren. A fin de evitar que se solicite la contraseña para cada renovación, puede usar un archivo keytab.  
  
En este artículo sobre[el uso y la configuración de Kerberos](https://commons.oreilly.com/wiki/index.php/Linux_in_a_Windows_World/Centralized_Authentication_Tools/Kerberos_Configuration_and_Use) se ofrece información sobre las formas de aplicar la autenticación Kerberos a servicios en Linux.
  
## <a name="tracking-access-to-a-database"></a>Seguimiento del acceso a una base de datos

Los administradores de bases de datos pueden crear una pista de auditoría del acceso a una base de datos cuando se usan cuentas del sistema para acceder a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante la autenticación integrada.  
  
Para iniciar sesión en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se usa la cuenta del sistema, y no existe ninguna función en Linux para suplantar el contexto de seguridad. Por lo tanto, se requieren más acciones para determinar el usuario.
  
Para auditar las actividades de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en nombre de usuarios distintos a los de la cuenta del sistema, la aplicación debe usar **EXECUTE AS** de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
Para mejorar su rendimiento, las aplicaciones pueden utilizar la agrupación de conexiones con la autenticación integrada y la auditoría. Pero combinar la agrupación de conexiones, la autenticación integrada y la auditoría trae consigo un riesgo de seguridad, ya que el administrador de controladores unixODBC permite que distintos usuarios reutilicen las conexiones agrupadas. Para obtener más información, consulte este artículo sobre la [agrupación de conexiones ODBC](http://www.unixodbc.org/doc/conn_pool.html).  

Antes de permitir su reutilización, las aplicaciones deben restablecer las conexiones agrupadas al ejecutar `sp_reset_connection`.  

## <a name="using-active-directory-to-manage-user-identities"></a>Uso de Active Directory para administrar identidades de usuario

Los administradores del sistema de aplicaciones no necesitan administrar conjuntos separados de credenciales de inicio de sesión para [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se puede configurar Active Directory como un centro de distribución de claves (KDC) para la autenticación integrada. Para más información, vea [Microsoft Kerberos](/windows/desktop/SecAuthN/microsoft-kerberos).

## <a name="using-linked-server-and-distributed-queries"></a>Uso de un servidor vinculado y consultas distribuidas

Los desarrolladores pueden implementar una aplicación que utilice un servidor vinculado o consultas distribuidas sin que haya un administrador de base de datos que mantenga conjuntos separados de credenciales de SQL. En estos casos, los desarrolladores deben configurar una aplicación para que use la autenticación integrada:  
  
-   El usuario inicia sesión en un equipo cliente y se autentica en el servidor de aplicaciones.  
  
-   El servidor de aplicaciones se autentica como una base de datos distinta y se conecta a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se autentica como un usuario de base de datos en otra base de datos ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]).  
  
Tras configurar la autenticación integrada, las credenciales se transmiten al servidor vinculado.  
  
## <a name="integrated-authentication-and-sqlcmd"></a>Autenticación integrada y sqlcmd
Para acceder a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante la autenticación integrada, use la opción `-E` de `sqlcmd`. Asegúrese de que la cuenta que ejecuta `sqlcmd` esté asociada a la entidad de seguridad del cliente de Kerberos predeterminado.

## <a name="integrated-authentication-and-bcp"></a>Autenticación integrada y bcp
Para acceder a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante la autenticación integrada, use la opción `-T` de `bcp`. Asegúrese de que la cuenta que ejecuta `bcp` esté asociada a la entidad de seguridad del cliente de Kerberos predeterminado. 
  
Usar `-T` con la opción `-U` o `-P` constituye un error.
  
## <a name="supported-syntax-for-an-spn-registered-by-ssnoversion"></a>Sintaxis admitida para un SPN registrado por [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

La sintaxis que usan los SPN en los atributos de cadena de conexión o atributos de conexión es la siguiente:  

|Sintaxis|Descripción|  
|----------|---------------|  
|MSSQLSvc/*fqdn*:*port*|SPN predeterminado generado por el proveedor cuando se usa TCP. *puerto* en un número de puerto TCP. *fqdn* es un nombre de dominio completo.|  
  
## <a name="authenticating-a-linux-or-macos-computer-with-active-directory"></a>Autenticación de un equipo Linux o macOS con Active Directory

<a name="configure-kerberos"></a>Para configurar Kerberos, escriba datos en el archivo `krb5.conf`. `krb5.conf` se encuentra en `/etc/` pero puede hacer referencia a otro archivo con la sintaxis, por ejemplo, `export KRB5_CONFIG=/home/dbapp/etc/krb5.conf`. A continuación se muestra un archivo `krb5.conf` de ejemplo:  
  
```  
[libdefaults]  
default_realm = YYYY.CORP.CONTOSO.COM  
dns_lookup_realm = false  
dns_lookup_kdc = true  
ticket_lifetime = 24h  
forwardable = yes  
  
[domain_realm]  
.yyyy.corp.contoso.com = YYYY.CORP.CONTOSO.COM  
.zzzz.corp.contoso.com = ZZZZ.CORP.CONTOSO.COM  
```  
  
Si su equipo Linux o MacOS está configurado para utilizar el protocolo de configuración dinámica de host (DHCP) con un servidor DHCP de Windows que proporciona los servidores DNS que se deben usar, puede utilizar **dns_lookup_kdc=true**. De esta forma, podrá utilizar Kerberos para iniciar sesión en su dominio emitiendo el comando `kinit alias@YYYY.CORP.CONTOSO.COM`. Los parámetros transmitidos a `kinit` distinguen mayúsculas de minúsculas, y el equipo con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurado para estar en el dominio debe tener ese usuario `alias@YYYY.CORP.CONTOSO.COM` agregado para el inicio de sesión. Ahora puede usar conexiones de confianza (**Trusted_Connection=YES** en una cadena de conexión, **bcp -T** o **sqlcmd -E**).  
  
La hora del equipo con Linux o macOS y la del Centro de distribución de claves (KDC) de Kerberos deben ser parecidas. Asegúrese de que la hora del sistema esté configurada correctamente, por ejemplo, mediante el protocolo de tiempo de redes (NTP).  

Si se produce un error en la autenticación Kerberos, el controlador ODBC en Linux o macOS no usa la autenticación NTLM.  

Para más información sobre la autenticación de equipos Linux o macOS con Active Directory, vea [Autenticar los clientes de Linux con Active Directory](/previous-versions/technet-magazine/dd228986(v=msdn.10)#id0060048). Para obtener más información sobre la configuración de Kerberos, consulte la [Documentación de Kerberos del MIT](https://web.mit.edu/kerberos/krb5-1.12/doc/index.html).

## <a name="see-also"></a>Consulte también  
[Instrucciones de programación](programming-guidelines.md)

[Notas de la versión](release-notes-odbc-sql-server-linux-mac.md)

[Uso de Azure Active Directory](../using-azure-active-directory.md)