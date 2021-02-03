---
description: QN:Parameter Table (clase de eventos)
title: Clase de eventos QN:Parameter Table | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], QN:Parameter Table
ms.assetid: 292da1ed-4c7e-4bd2-9b84-b9ee09917724
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bfa37ec3e59c1e67b3909eaad7926e443f5d5d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178172"
---
# <a name="qnparameter-table-event-class"></a>QN:Parameter Table (clase de eventos)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  El evento QN:Parameter table ofrece información acerca de las operaciones necesarias para crear y quitar las tablas internas que almacenan la información de parámetros, así como para mantener los recuentos de referencia de las mismas. Este evento también informa de la actividad interna para restablecer el recuento de utilización de una tabla de parámetros.  
  
## <a name="qnparameter-table-event-class-data-columns"></a>Columnas de datos de la clase de evento QN:Parameter Table  
  
|Columna de datos|Tipo|Descripción|Número de columna|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|ClientProcessID|**int**|Id. que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona su identificador de proceso.|9|Sí|  
|DatabaseID|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o identificador de la base de datos predeterminada si no se ha emitido la instrucción USE *database* para una instancia determinada. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos Server Name en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos en que se ejecuta la instrucción del usuario.|35|Sí|  
|EventClass|**Int**|Tipo de evento = 200.|27|No|  
|EventSequence|**int**|Número de secuencia de este evento.|51|No|  
|EventSubClass|**nvarchar**|Tipo de subclase de evento que proporciona más información acerca de cada clase de evento. Esta columna puede incluir los valores siguientes:<br /><br /> **Tabla created**: indica que se ha creado una tabla de parámetros en la base de datos.<br /><br /> **Table drop attempt failed**: indica que la base de datos ha intentado quitar automáticamente una tabla de parámetros no utilizada para liberar recursos.<br /><br /> **Table drop attempt failed**: indica que la base de datos intentó quitar una tabla de parámetros no utilizada y se produjo un error. [!INCLUDE[ssDE](../../includes/ssde-md.md)] volverá a programar automáticamente la eliminación de la tabla de parámetros para liberar recursos.<br /><br /> **Table dropped**: indica que la base de datos quitó correctamente una tabla de parámetros.<br /><br /> **Table pinned**: indica que se ha marcado la tabla de parámetros para el uso actual por el procesamiento interno.<br /><br /> **Table unpinned**: indica que la tabla de parámetros se ha liberado. El procesamiento interno ha terminado de usar la tabla.<br /><br /> **Number of users incremented**: indica que ha aumentado el número de suscripciones de notificación de consulta que hacen referencia a una tabla de parámetros.<br /><br /> **Number of users decremented**: indica que ha disminuido el número de suscripciones de notificación de consulta que hacen referencia a una tabla de parámetros.<br /><br /> **LRU counter reset**: indica que se ha reiniciado el recuento de utilización de la tabla de parámetros.<br /><br /> **Cleanup task started**: indica cuándo se ha iniciado la limpieza de todas las suscripciones de esta tabla de parámetros. Esto sucede cuando se inicia la base de datos o cuando se elimina una tabla que subyace a las suscripciones de esta tabla de parámetros.<br /><br /> **Cleanup task finished**: indica cuándo ha finalizado la limpieza de todas las suscripciones de esta tabla de parámetros.|21|Sí|  
|GroupID|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|HostName|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario.<br /><br /> 0 = usuario<br /><br /> 1 = sistema|60|No|  
|LoginName|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de Windows en formato *DOMINIO*\\*nombreDeUsuario*).|11|No|  
|LoginSID|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|NTDomainName|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|NTUserName|**nvarchar**|Nombre del usuario al que pertenece la conexión que generó este evento.|6|Sí|  
|RequestID|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|nombreDeServidor|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si una aplicación se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName muestra "inicioDeSesión1" y LoginName muestra "inicioDeSesión2". En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|SPID|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TextData|**ntext**|Devuelve un documento XML que contiene información específica de este evento. Este documento se ajusta al esquema XML disponible en la página sobre el [esquema de eventos del analizador de notificaciones de consultas de SQL Server](https://go.microsoft.com/fwlink/?LinkId=63331)|1|Sí|  
  
  
