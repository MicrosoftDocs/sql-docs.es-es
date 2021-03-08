---
title: Conexión con sqlcmd
description: Obtenga información sobre cómo usar la utilidad sqlcmd con Microsoft ODBC Driver for SQL Server en Linux y macOS.
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd
ms.assetid: 61a2ec0d-1bcb-4231-bea0-cff866c21463
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216f78615ca049d3e97134cb14831d9a5e6afd32
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837367"
---
# <a name="connecting-with-sqlcmd"></a>Conexión con sqlcmd
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

La utilidad [sqlcmd](../../../tools/sqlcmd-utility.md) está disponible en [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en Linux y macOS.
  
Los siguientes comandos muestran cómo usar la autenticación de Windows (Kerberos) y la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], respectivamente:
  
```console
sqlcmd -E -Sxxx.xxx.xxx.xxx  
sqlcmd -Sxxx.xxx.xxx.xxx -Uxxx -Pxxx  
```  
  
## <a name="available-options"></a>Opciones disponibles

En la versión actual, están disponibles las siguientes opciones:  
  
- -? mostrar el uso de `sqlcmd`.  
  
- -a solicitar un tamaño de paquete.  
  
- -b finalizar el trabajo por lotes si se produce un error.  
  
- -c *batch_terminator* especificar el terminador de lote.  
  
- -C confiar en el certificado de servidor.  

- -d *database_name* emitir una instrucción `USE` *database_name* al iniciar `sqlcmd`.  

- -D hace que el valor transmitido a la opción `sqlcmd` -S se interprete como un nombre de origen de datos (DSN). Para obtener más información, vea la sección "Compatibilidad de DSN en `sqlcmd` y `bcp`" al final de este tema.  
  
- -e crear scripts de entrada en el dispositivo de salida estándar (stdout).

- -E usa la conexión de confianza (autenticación integrada). Para más información sobre cómo establecer conexiones de confianza que usen la autenticación integrada desde un cliente Linux o macOS, consulte [Uso de la autenticación integrada](using-integrated-authentication.md).

- -f página de códigos | i:página de códigos[,o:página de códigos] | o:página de códigos[,i:página de códigos] Especifica las páginas de códigos de entrada y de salida. El número de página de códigos es un valor numérico que especifica una página de códigos de Linux instalada.
(disponible desde la versión 17.5.1.1)

- El cliente usa el modificador -G al conectarse a SQL Database o a Azure Synapse Analytics para especificar que el usuario se autentica mediante la autenticación de Azure Active Directory. Esta opción establece la variable de scripting de sqlcmd SQLCMDUSEAAD en true. El modificador -G requiere al menos sqlcmd versión 17.6. Para determinar su versión, ejecute sqlcmd -?.

> [!IMPORTANT]
> La opción `-G` solo se aplica a Azure SQL Database y a Azure Synapse Analytics.
> La autenticación interactiva de AAD no se admite actualmente ni en Linux ni en macOS. La autenticación integrada de AAD requiere la versión 17.6.1 o superior de [Microsoft ODBC Driver 17 for SQL  Server](../download-odbc-driver-for-sql-server.md) y un [entorno de Kerberos configurado](using-integrated-authentication.md#configure-kerberos) correctamente.

- -h *number_of_rows* especificar el número de filas que se van a imprimir entre los encabezados de columna.  
  
- -H especificar el nombre de una estación de trabajo.  
  
- -i *input_file*[,*input_file*[,…]] identificar el archivo que contiene un lote de instrucciones SQL o procedimientos almacenados.  
  
- -I establece la opción de conexión `SET QUOTED_IDENTIFIER` en ON.  
  
- -k quitar o reemplazar caracteres de control.  
  
- **-K**_application\_intent_  
Declara el tipo de carga de trabajo de la aplicación al conectarse a un servidor. El único valor actualmente admitido es **de solo lectura**. Si no se especifica **-K**, la utilidad `sqlcmd` no admitirá la conectividad con una réplica secundaria en un grupo de disponibilidad AlwaysOn. Para más información, vea [Controlador ODBC en Linux y macOS: alta disponibilidad y recuperación ante desastres](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> **-K** no se admite en la versión de CTP para SUSE Linux. Pero puede especificar la palabra clave **ApplicationIntent=ReadOnly** en un archivo de DSN transmitido a `sqlcmd`. Para obtener más información, vea la sección "Compatibilidad de DSN en `sqlcmd` y `bcp`" al final de este tema.  
  
- -l *timeout* Especifica el número de segundos que tienen que transcurrir antes de que un inicio de sesión de `sqlcmd` agote el tiempo de espera cuando se trate de conectar a un servidor.

- -m *error_level* controlar qué mensajes de error se envían a stdout.  
  
- **-M**_multisubnet\_failover_  
Especifique siempre **-M** al conectarse a una escucha de un grupo de disponibilidad de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] o a una instancia de clúster de conmutación por error de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **-M** proporciona una detección más rápida de conmutaciones por error del servidor activo actualmente y de la conexión a este. Si no se especifica **-M**, **-M** se desactiva. Para más información sobre [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)], vea [Controlador ODBC en Linux y macOS: alta disponibilidad y recuperación ante desastres](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> **-M** no se admite en la versión de CTP para SUSE Linux. Pero puede especificar la palabra clave **MultiSubnetFailover=Yes** en un archivo de DSN transmitido a `sqlcmd`. Para obtener más información, vea la sección "Compatibilidad de DSN en `sqlcmd` y `bcp`" al final de este tema.  
  
- -N cifrar conexión.  
  
- -o *output_file* identificar el archivo que recibe los resultados de `sqlcmd`.  
  
- -p imprimir las estadísticas de rendimiento de cada conjunto de resultados.  
  
- -P especificar una contraseña de usuario.  
  
- -q *commandline_query* ejecuta una consulta cuando se inicia `sqlcmd`, pero no se cierra cuando la consulta termina de ejecutarse.  

- -Q *commandline_query* ejecuta una consulta cuando se inicia `sqlcmd`. `sqlcmd` se cerrará cuando finalice la consulta.  

- -r redirige los mensajes de error a stderr.

- -R hace que el controlador use la configuración regional del cliente para convertir los datos de hora, fecha y moneda a datos de caracteres. Actualmente solo se utiliza el formato en_US (inglés de Estados Unidos).
  
- -s *column_separator_char* especifica el carácter separador de columnas.  

- -S [*protocol*:] *server*[ **,** _port_]  
Especifique la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a la que se conectará, o si se usa-D, un DSN. El controlador ODBC en Linux y macOS requiere -S. Tenga en cuenta que **tcp** es el único protocolo válido.  
  
- -t *query_timeout* especificar el número de segundos que tienen que transcurrir antes de que un comando (o la instrucción de SQL) exceda el tiempo de espera.  
  
- -u especificar que el archivo output_file se almacene en formato Unicode, independientemente del formato del archivo input_file.  
  
- -U *login_id* especifica un identificador de inicio de sesión de usuario.  
  
- -V *error_severity_level* controlar el nivel de gravedad que se usa para establecer la variable ERRORLEVEL.  
  
- -w *column_width* especifica el ancho de pantalla de salida.  
  
- -W quitar los espacios finales de una columna.  
  
- -x deshabilitar la sustitución de variables.  
  
- -X deshabilitar los comandos interactivos, el script de inicio y las variables de entorno.  
  
- -y *variable_length_type_display_width* establece la variable `SQLCMDMAXFIXEDTYPEWIDTH` del script `sqlcmd`.
  
- -Y *fixed_length_type_display_width* establece la variable `SQLCMDMAXVARTYPEWIDTH` del script `sqlcmd`.

- -z *password* cambia la contraseña.  
  
- -Z *password* cambia la contraseña y se cierra.  


## <a name="available-commands"></a>Comandos disponibles

En la versión actual, están disponibles los siguientes comandos:  
  
-   [:]!!  
  
-   :Connect  
  
-   :Error  
  
-   [:]EXIT  
  
-   GO [*count*]  
  
-   :Help  
  
-   :List  
  
-   :Listvar  
  
-   :On Error  
  
-   :Out  
  
-   :Perftrace  
  
-   [:]QUIT  
  
-   :r  
  
-   :RESET  
  
-   :setvar  
  
## <a name="unavailable-options"></a>Opciones no disponibles
En la versión actual, no están disponibles las siguientes opciones:  

- -A iniciar sesión en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con una conexión de administrador dedicada (DAC). Para obtener información sobre cómo realizar una conexión de administrador dedicada (DAC), vea [Instrucciones de programación](programming-guidelines.md).  
  
- -L enumerar los equipos servidores configurados localmente y los nombres de los equipos servidores que se difunden en la red.  
  
- -v crear una variable de scripting de `sqlcmd` que se puede usar en un script de `sqlcmd`.  
  
Puede usar el siguiente método alternativo: Coloque los parámetros dentro de un archivo, que puede anexar a otro archivo. Esto le ayudará a utilizar un archivo de parámetros para reemplazar los valores. Por ejemplo, cree un archivo llamado `a.sql` (el archivo de parámetros) con el siguiente contenido:
  
```console
    :setvar ColumnName object_id  
    :setvar TableName sys.objects  
```
  
Después, cree un archivo denominado `b.sql` con los parámetros de reemplazo:  
  
```sql
    select $(ColumnName) from $(TableName)  
```

En la línea de comandos, combine `a.sql` y `b.sql` en `c.sql` con los siguientes comandos:  
  
```console
    cat a.sql > c.sql 
  
    cat b.sql >> c.sql  
```
  
Ejecute `sqlcmd` y use `c.sql` como archivo de entrada:  
  
```console
    sqlcmd -S<...> -P<..> -U<..> -I c.sql  
```

## <a name="unavailable-commands"></a>Comandos no disponibles

En la versión actual, no están disponibles los siguientes comandos:  
  
-   :ED  
  
-   :ServerList  
  
-   :XML  
  
## <a name="dsn-support-in-sqlcmd-and-bcp"></a>Compatibilidad con DSN en sqlcmd y bcp

Puede especificar un nombre de origen de datos (DSN) en lugar de un nombre de servidor en la opción **sqlcmd** o **bcp** `-S` (o el comando **sqlcmd** :Connect) si especifica `-D`. `-D` hace que **sqlcmd** o **bcp** se conecten al servidor especificado en el DSN mediante la opción `-S`.  
  
Los DSN del sistema se almacenan en el archivo `odbc.ini` que se encuentra en el directorio SysConfigDir de ODBC (`/etc/odbc.ini` en instalaciones estándar). Los DSN de usuario se almacenan en `.odbc.ini` en el directorio particular de un usuario (`~/.odbc.ini`).

En los sistemas Windows, los DSN del sistema y del usuario se almacenan en el registro y se administran a través de odbcad32.exe. BCP y SQLCMD no admiten los DSN de archivo.

Vea [Atributos y palabras clave de cadena de conexión y DSN](../dsn-connection-string-attribute.md) para obtener la lista de entradas que admite el controlador.

En un DSN, solo se necesita la entrada DRIVER, pero para conectarse a un servidor remoto, `sqlcmd` o `bcp` necesitan un valor del elemento SERVER. Si el elemento SERVER está vacío o no está presente en el DSN, `sqlcmd` y `bcp` intentarán conectarse a la instancia predeterminada en el sistema local.

Al usar BCP en sistemas Windows, [!INCLUDE [sssql17-md](../../../includes/sssql17-md.md)] y versiones anteriores necesitan el controlador SQL Native Client 11 (sqlncli11.dll), mientras que [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] y versiones posteriores necesitan el controlador Microsoft ODBC 17 para el controlador de SQL Server (msodbcsql17.dll).  

Si se especifica la misma opción en el DSN y la línea de comandos `sqlcmd` o `bcp`, la opción de línea de comandos invalida el valor utilizado en el DSN. Por ejemplo, si el DSN tiene una entrada DATABASE y la línea de comandos `sqlcmd` incluye **-d**, se utiliza el valor transmitido a **-d**. Si se especifica **Trusted_Connection=yes** en el DSN, se utiliza la autenticación Kerberos, y el nombre de usuario ( **–U**) y la contraseña ( **–P**), si se proporcionan, se omiten.

Los scripts existentes que invocan a `isql` pueden modificarse para usar `sqlcmd` definiendo el siguiente alias: `alias isql="sqlcmd -D"`.  

## <a name="see-also"></a>Consulte también  
[Conexión con **bcp**](connecting-with-bcp.md)  
[Notas de la versión](release-notes-tools.md)
