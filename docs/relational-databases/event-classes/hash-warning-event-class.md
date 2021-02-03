---
description: Hash Warning [clase de eventos]
title: Clase de eventos Hash Warning | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Hash Warning event class
ms.assetid: cb93c620-4be9-4362-8bf0-af3f2048bdaf
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3353313a9ee65ab1de34fa8c1e90517950eed0e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199915"
---
# <a name="hash-warning-event-class"></a>Hash Warning [clase de eventos]
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos Hash Warning se puede utilizar para supervisar cuándo se ha producido una recursividad hash o un cese de hash (salida hash) durante una operación de hash.  
  
 La recursividad hash se produce cuando no hay suficiente memoria para la entrada generada, lo que causa que ésta se divida en varias particiones que se procesan por separado. Si alguna de estas particiones sigue sin caber en la memoria disponible, se vuelve a dividir en subparticiones, que también se procesan por separado. El proceso de división continúa hasta que cada partición quepa en la memoria disponible o hasta que se alcance el nivel máximo de recursividad (que se muestra en la columna de datos IntegerData).  
  
 La salida hash se produce cuando una operación de hash alcanza el nivel máximo de repetición y vuelve a un plan alternativo para procesar el resto de datos con particiones. La salida hash normalmente se produce debido a la existencia de datos sesgados.  
  
 La recursividad hash y el cese de hash reducen el rendimiento del servidor. Para eliminar o reducir la frecuencia de la repetición hash y del cese de hash, realice uno de los siguientes procedimientos:  
  
-   Asegúrese de que existen estadísticas en las columnas que se están combinando o agrupando.  
  
-   Si hay estadísticas en las columnas, actualícelas.  
  
-   Utilice un tipo de combinación diferente. Por ejemplo, utilice en su lugar una combinación MERGE o LOOP, si resulta apropiado.  
  
-   Aumente la memoria disponible en el equipo. La repetición hash o el cese de hash se produce cuando no hay suficiente memoria para procesar las consultas realizadas y es necesario volcarlas en el disco.  
  
 Crear o actualizar las estadísticas de la columna afectada en la combinación es el modo más eficaz de reducir el número de repeticiones hash o de ceses de hash que se producen.  
  
> [!NOTE]  
>  Para describir las salidas de hash, también se usan los términos *combinación hash aplazada* y *combinación hash de repetición* .  
  
> [!IMPORTANT]  
>  Para determinar dónde se está produciendo el evento Hash Warning cuando el optimizador de consultas genera un plan de ejecución, debería recopilar la clase de eventos Showplan en el seguimiento. Puede elegir cualquiera de las clases de eventos Showplan, excepto Showplan Text y Showplan Text (Unencoded), que no devuelven ningún identificador de nodo. Los identificadores de nodo en las clases de evento Showplans identifican cada operación que realiza el optimizador de consultas cuando genera un plan de ejecución de consultas. Estas operaciones se denominan *operadores* y cada operador de un Showplan tiene un identificador de nodo. La columna ObjectID para los eventos Warning Hash se corresponden con el identificador de nodo en las clase de eventos Showplan para que pueda determinar qué operador u operación está provocando el error.  
  
## <a name="hash-warning-event-class-data-columns"></a>Columnas de datos de la clase de eventos Hash Warning  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se llena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra del programa.|10|Sí|  
|ClientProcessID|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se llena si el cliente proporciona el Id. del proceso del cliente.|9|Sí|  
|DatabaseID|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos ServerName en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|EventClass|**int**|Tipo de evento = 55.|27|No|  
|EventSequence|**int**|Secuencia de un evento determinado de la solicitud.|51|No|  
|EventSubClass|**int**|Tipo de la subclase de eventos.<br /><br /> 0=Repetición<br /><br /> 1=Cese de hash|21|Sí|  
|GroupID|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|HostName|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|IntegerData|**int**|Nivel de recursividad (solo recursividad hash).|25|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|LoginName|**nvarchar**|Nombre del inicio de sesión del usuario (ya sea el de seguridad [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o las credenciales de inicio de sesión de Windows con el formato *\<DOMAIN>\\<nombre_de_usuario\>* ).|11|Sí|  
|LoginSid|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|NTDomainName|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|NTUserName|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|ObjectID|**int**|Identificador del nodo de la raíz del equipo de hash implicado en la nueva creación de particiones. Se corresponde con el Id. de nodo en los eventos Showplan.|22|Sí|  
|RequestID|**int**|Id. de la solicitud que contiene la instrucción.|49|Sí|  
|nombreDeServidor|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuyo seguimiento se realiza.|26||  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName muestra inicioDeSesión1 y LoginName muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|SPID|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TransactionID|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|XactSequence|**bigint**|Token que describe la transacción actual.|50|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)    
 [Combinaciones](../../relational-databases/performance/joins.md)    
  
  
