---
description: Errores conocidos y resoluciones con captura de datos modificados para Oracle de Attunity
title: Errores conocidos y resoluciones con captura de datos modificados para Oracle de Attunity | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a898b268de4f339d26a6720c53356723ad56b285
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062130"
---
# <a name="known-errors-and-resolutions-with-change-data-capture-for-oracle-by-attunity"></a>Errores conocidos y resoluciones con captura de datos modificados para Oracle de Attunity
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]

En este tema se enumeran las principales incidencias y las resoluciones conocidas al ver una instancia de captura de datos modificados (CDC) en la herramienta de configuración de Oracle CDC Designer. Esta herramienta forma parte de la captura de datos modificados para Oracle de Attunity, que se incluye a partir de SQL Server 2012. 

## <a name="bug-fixes"></a>Corrección de errores
Antes de dedicar demasiado tiempo a solucionar problemas, es importante usar las últimas compilaciones de CDC para Oracle de Attunity con el fin de evitar incidencias conocidas, como las siguientes:

### <a name="sql-server-2017"></a>SQL Server 2017

La **versión 5.0.0.111** contiene estas correcciones:
- Corrección de errores: Oracle CDC Designer produce el error "Sintaxis incorrecta cerca de la palabra clave 'KEY'" al agregar una tabla de Oracle. 
- Mejora: compatibilidad mejorada con RAC; esto incluye un mejor control cuando se reinicia un nodo RAC. 
- Corrección de errores: CDC no funciona con Oracle 10.2 debido a la solicitud NEXT_CHANGE# que se realiza desde v$log. 


La **versión 5.0.0.93** contiene estas correcciones: 
- Microsoft CDC para Oracle de Attunity Designer produce el error "Sintaxis incorrecta cerca de la palabra 'KEY'" al agregar una tabla de Oracle. 

 
### <a name="sql-server-2016"></a>SQL Server 2016

La **versión 4.0.107** contiene estas correcciones:
- Corrección de errores: Oracle CDC Designer produce el error "Sintaxis incorrecta cerca de la palabra clave 'KEY'" al agregar una tabla de Oracle.
- Mejora: compatibilidad mejorada con RAC; esto incluye un mejor control cuando se reinicia un nodo RAC.
- Corrección de errores: CDC no funciona con Oracle 10.2 debido a la solicitud NEXT_CHANGE# que se realiza desde v$log.

La **versión 4.0.0.95** contiene estas correcciones: 
- Corrección de errores: Oracle CDC Designer produce el error "Sintaxis incorrecta cerca de la palabra clave 'KEY'" al agregar una tabla de Oracle.

La **versión 4.0.0.88** contiene estas correcciones:
-  Las propiedades agregadas en las opciones avanzadas de la instancia CDC Attunity se quitan cuando se agrega o se quita una tabla de CDC. 
- Attunity CDC deja de funcionar después de aplicar la corrección de SQL que agrega la columna __$command_id.

### <a name="sql-server-2014"></a>SQL Server 2014 

La **versión 2.0.0.114** contiene estas correcciones:
- Corrección de errores: Oracle CDC Designer produce el error "Sintaxis incorrecta cerca de la palabra clave 'KEY'" al agregar una tabla de Oracle.
- Mejora: compatibilidad mejorada con RAC; esto incluye un mejor control cuando se reinicia un nodo RAC.
- Corrección de errores: CDC no funciona con Oracle 10.2 debido a la solicitud NEXT_CHANGE# que se realiza desde v$log.

La **versión 2.0.0.92** contiene estas correcciones: 
- Las propiedades agregadas en las opciones avanzadas de la instancia CDC Attunity se quitan cuando se agrega o se quita una tabla de CDC. Attunity CDC deja de funcionar después de aplicar la corrección de SQL que agrega la columna __$command_id.
- Error en la validación de metadatos de la tabla de Oracle cdc.table_name. El índice de la columna column_name está fuera del rango.  Y esta incidencia: El servicio CDC de Oracle muestra el estado anulado al usar CDC para Oracle de Attunity
    - Se ha corregido en la _Actualización acumulativa 1 para SQL Server 2014 RTM_, tal como se describe en KB [2894025](https://support.microsoft.com/kb/2894025).
- Algunos cambios se han perdido y no se replican en la base de datos de SQL Server. Esta incidencia se produce cuando una tabla contiene más de un objeto binario grande de caracteres (CLOB) y uno de los CLOB tiene un valor grande. 
    - Se ha corregido en la _Actualización acumulativa 1 para SQL Server 2014 SP1_ y la _Actualización acumulativa 8 para SQL Server 2014 RTM_, tal como se describe en KB [3029096](https://support.microsoft.com/kb/3029096). 
- La captura de datos modificados para Oracle de Attunity deja de funcionar cuando las tablas de Oracle tienen una columna con el tipo de datos Long.
    - Se ha corregido en la _Actualización acumulativa 5 para SQL Server 2014 SP1_ y la _Actualización acumulativa 12 para SQL Server 2014 RTM_, como se describe en [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d).

### <a name="sql-server-2012"></a>SQL Server 2012

La **versión 1.1.0.102** contiene estas correcciones: 
 
- Las propiedades agregadas en las opciones avanzadas de la instancia CDC Attunity se quitan cuando se agrega o se quita una tabla de CDC. Attunity CDC deja de funcionar después de aplicar la corrección de SQL que agrega la columna __$command_id.
- CDC para la instancia de Oracle no responde cuando se inicia y no captura los cambios. La memoria del servidor de Oracle puede aumentar hasta que se agote la memoria o se bloquee.
- [2672759](https://support.microsoft.com/kb/2672759): Se produce un mensaje de error al usar Change Data Capture Service para Oracle de Attunity: "ORA-00600: código de error interno". Agregue el seguimiento de nivel de origen y confirme si recibe el mismo error ORA-00600. Solucionado mediante una descarga de revisión de Oracle.
- Varias particiones
    - Cuando se usan más de 10 particiones en una tabla de Oracle, la instancia CDC no puede capturar todos los cambios de la tabla. Cuando la tabla de Oracle se define con más de 10 particiones, los cambios solo se capturan desde las últimas 10 particiones. Se ha corregido en la versión _Service Pack 1 para SQL Server 2012_. Consulte la [página de descarga de Feature Pack de SP1](https://www.microsoft.com/download/details.aspx?id=35575). 
- Los cambios se han perdido
    - La captura de eventos puede entrar en un bucle infinito y dejar de capturar nuevos cambios de datos (relacionados con el error 5623813 de Oracle). Cuando el entorno de Oracle RAC realiza una detención o reanudación de la instancia CDC, los cambios se pueden omitir o perder. Esto significa que a la captura de datos modificados de SQL Server le faltan filas importantes y, por lo tanto, hay pérdida de datos en el almacén de datos o en el sistema de suscripción. Se ha corregido en la versión _Service Pack 1 para SQL Server 2012_. Consulte la [página de descarga de Feature Pack de SP1](https://www.microsoft.com/download/details.aspx?id=35575).
- Anchos dobles en columnas de SQL
    - Al crear una instancia CDC para Oracle, en los scripts que se van a ejecutar en SQL Server, la longitud de una columna de tipo de datos de ancho variable se duplica en las tablas de SQL Server que se crean en el script. Por ejemplo, si intenta realizar un seguimiento de los cambios en una columna VARCHAR2(10) en una tabla de Oracle, la columna correspondiente de la tabla SQL Server es NVARCHAR(20) en el script de implementación. Se ha corregido en la _Actualización acumulativa 2 para SQL Server 2012 SP1_ o en la _Actualización acumulativa 5 para SQL Server 2012_, tal como se describe en KB [2769673](https://support.microsoft.com/kb/2769673). 
- Los datos DDL se truncan.
    - Cuando se ejecuta una instrucción de lenguaje de definición de datos (DDL) que tiene más de 4000 bytes en una base de datos de Oracle que contiene caracteres no latinos, se produce un error en CDC para Oracle de Attunity. Además, se verá el mensaje de error `ORA-01406: fetched column value was truncated.`. Se ha corregido en la _Actualización acumulativa 4 para SQL Server 2012 SP1_, tal como se describe en KB [2839806](https://support.microsoft.com/kb/2839806). 
- Los cambios se pierden en las dos últimas columnas
    - Se ha corregido en la _Actualización acumulativa 6 para SQL Server 2012 SP1_, tal como se describe en KB [2874879](https://support.microsoft.com/kb/2874879). 
- El registro de transacciones de SQL crece cuando se usa CDC para Oracle
     - Cuando está configurada la captura de datos modificados para las instancias de Oracle, la base de datos SQL que recibe los datos modificados tendrá tablas reflejadas, con las transacciones marcadas para replicación. Este comportamiento se produce porque CDC para Oracle se basa en procedimientos almacenados del sistema subyacentes similares a los que se usan en CDC para SQL Server. Sin embargo, dado que no hay ninguna replicación CDC de SQL implicada cuando CDC para Oracle se usa por sí solo, no hay ningún lector de registro para borrar las transacciones marcadas para la replicación. Dado que la transacción no tiene que replicarse en SQL Server, es seguro marcar manualmente la transacción como distribuida mediante la solución que se describe más adelante en este artículo. Se puede obtener más información en KB [2871474](https://support.microsoft.com/kb/2871474). 
- Error `ORACDC000T: Error encountered at position to change event - SCN not found - EOF simulated`. Se ha corregido en la _Actualización acumulativa 7 para SQL Server 2012 SP1_, tal como se describe en KB [2883524](https://support.microsoft.com/kb/2883524). 
- Error en la validación de metadatos de la tabla de Oracle cdc.table_name. El índice de la columna column_name está fuera del rango. Se ha corregido en la _Actualización acumulativa 7 para SQL Server 2012 SP1_, tal como se describe en KB [2883524](https://support.microsoft.com/kb/2883524).
- El servicio CDC de Oracle muestra el estado anulado al usar CDC para Oracle de Attunity en SQL Server 2012. Se ha corregido en la _Actualización acumulativa 8 para SQL Server 2012 SP1_, tal como se describe en KB [2923839](https://support.microsoft.com/kb/2923839).  
- Algunos cambios se han perdido y no se replican en las bases de datos de SQL Server. Esta incidencia se produce cuando una tabla contiene más de un objeto binario grande de caracteres (CLOB) y uno de los CLOB tiene un valor grande. Se ha corregido en la _Actualización acumulativa 8 para SQL Server 2012 SP1_, tal como se describe en KB [2923839](https://support.microsoft.com/kb/2923839).   
- La captura de datos modificados para Oracle de Attunity deja de funcionar cuando las tablas de Oracle tienen una columna con el tipo de datos Long. Se ha corregido en la _Actualización acumulativa 2 para SQL Server 2012 SP3_ y la _Actualización acumulativa 11 para SQL Server 2012 SP2_, como se describe en [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d). 

## <a name="collect-detailed-logs"></a>Recopilación de registros detallados 

En esta sección se explica cómo recopilar errores y eventos de la instancia CDC. 

### <a name="management-console"></a>Consola de administración

Puede ver los errores en los mensajes de **Estado** notificados en la consola de administración del diseñador de captura de datos modificados de Oracle cuando se resalta una instancia CDC en el panel izquierdo. 

### <a name="query-trace-table"></a>Consulta de la tabla de seguimiento

Se puede consultar la tabla de seguimiento en la base de datos CDC en SQL Server para ver los errores registrados. 

### <a name="save-output-from-basic-logging"></a>Almacenamiento de la salida del registro básico 

Para capturar diagnósticos, seleccione **Recopilación de diagnósticos** en la pestaña de estado de la consola de administración de captura de datos modificados de Oracle. 

![Captura de pantalla en la que se muestra la pestaña Estado de la consola de administración de captura de datos modificados de Oracle con la opción Recopilar diagnósticos resaltada.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

Elija una hora de inicio y seleccione una ubicación para el archivo de registro. Después, seleccione **Crear** para iniciar la recopilación de diagnósticos. 

![Captura de pantalla del cuadro de diálogo Collect Diagnostics for testTA (Recopilar diagnósticos para testTA).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/start-diagnostics.png)

### <a name="detailed-errors"></a>Errores detallados

Puede aumentar el nivel de seguimiento que ha recopilado la instancia y repetir el escenario para recopilar un registro más detallado. Para ello, en **Acciones**, seleccione **Propiedades** y, después, agregue una nueva propiedad en la cuadrícula de **Configuración avanzada** de la pestaña **Avanzado**. Establezca el nombre de la propiedad en `trace` y luego establezca el valor en `SOURCE`. 

![Captura de pantalla en la que se muestra la opción Propiedades en Acciones.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/properties.png)

Reproduzca el error y luego seleccione la opción **Recopilar diagnósticos** para recopilar los registros. 

![Otra captura de pantalla de la pestaña Estado de la consola de administración de captura de datos modificados de Oracle con la opción Recopilar diagnósticos resaltada.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

## <a name="ora-00942-table-of-view-does-not-exist"></a>ORA-00942 La tabla de vista no existe 

Se trata de un error común que se muestra en el campo de mensaje **Estado** de la instancia CDC. La instancia vuelve a intentarlo varias veces, por lo que el icono de estado cambiará a verde momentáneamente, pero se producirá un error con la exclamación roja y el estado UNEXPECTED. 

![Captura de pantalla que muestra un error en el campo Mensaje de estado de la instancia de CDC.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-error.png)

```
"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC508E:Oracle method OCIStmtExecute failed with error: ORA-00942: table or view does not exist ","source","",""

"ERROR","computername","RUNNING","IDLE",
"ORACDC518E:Failed to verify archive log mode.","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC517E:Oracle Call Interface (OCI) method failed: ORA-00942: table or view does not exist .","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC414E:The Engine component failed with return code 1.","engine","",""
```

Esto sucede cuando la cuenta de Oracle que se conecta desde la instancia CDC con el servidor de Oracle no tiene permiso para ver las vistas de registro del sistema. 

### <a name="resolution"></a>Solución

Para resolver este error, conceda a los usuarios configurados actualmente los permisos adecuados en el sistema de base de datos de Oracle, o bien cambie la cuenta que se utiliza para conectarse al servidor de Oracle desde la instancia CDC. 

La lista de todos los permisos necesarios se detalla en el archivo de ayuda incluido en la carpeta de archivos del programa de instalación `C:\Program Files\Change Data Capture for Oracle by Attunity\Attunity.SqlServer.XdbCdcDesigner.chm`.  Consulte la página titulada "Conectarse a una base de datos de origen de Oracle" en el archivo .chm para ver la lista completa.

Para establecer la cuenta de usuario, seleccione CDCInstance en el panel izquierdo y el botón Propiedades en el panel Acciones, situado más a la derecha dentro de la ventana **CDC Designer**. Puede cambiar la cuenta de autenticación de minería de registros de Oracle desde la página de diálogo de propiedades.

![Captura de pantalla en la que se muestra la pestaña Oracle del cuadro de diálogo testTA Properties (Propiedades de testTA).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-connection.png)


  
## <a name="see-also"></a>Vea también  
 [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Trabajar con datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Administrar y supervisar la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
