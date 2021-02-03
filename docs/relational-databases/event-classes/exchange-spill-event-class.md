---
description: Exchange Spill, clase de eventos
title: Exchange Spill (clase de eventos) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Exchange Spill event class
ms.assetid: fb876cec-f88d-4975-b3fd-0fb85dc0a7ff
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a60e22ef72b86bb8136ee5ad7709212fe7b394b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160778"
---
# <a name="exchange-spill-event-class"></a>Exchange Spill, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de evento **Exchange Spill** indica que los búferes de comunicación de un plan de consulta paralelo se han escrito de forma temporal en la base de datos **tempdb** . Esto sucede raramente y solo cuando un plan de consulta tiene múltiples recorridos de intervalo.  
  
 Normalmente, la consulta [!INCLUDE[tsql](../../includes/tsql-md.md)] que genera tales recorridos de intervalo tiene muchos operadores BETWEEN, cada uno de los cuales selecciona un intervalo de filas de una tabla o un índice. Como alternativa, puede obtener múltiples intervalos mediante expresiones como (T.a > 10 AND T.a < 20) OR (T.a > 100 AND T.a < 120). Además, los planes de consulta deben requerir que estos intervalos se recorran en orden, bien porque hay una cláusula ORDER BY en T.a, o bien porque un iterador del plan requiere que éste consuma las tuplas en orden.  
  
 Cuando un plan de consulta para una consulta así tiene varios operadores **Parallelism** , los búferes de comunicación de memoria utilizados por los operadores **Parallelism** se llenan y puede originarse una situación en la que el progreso de ejecución de la consulta se detenga. En esta situación, uno de los operadores **Parallelism** escribe su búfer de salida en **tempdb** (operación llamada *volcado de intercambio*), por lo que puede consumir filas de algunos de sus búferes de entrada. Es posible que las filas volcadas se devuelvan al consumidor cuando éste esté preparado para consumirlas.  
  
 Muy raramente, pueden producirse múltiples volcados de intercambio en un mismo plan de ejecución, lo que haría que la consulta se ejecutara con lentitud. Si advierte más de cinco volcados en la ejecución del mismo plan de consulta, póngase en contacto con su profesional de soporte técnico.  
  
 Los volcados de intercambio a veces son transitorios y pueden desaparecer a medida que cambia la distribución de datos.  
  
 Hay varias formas de evitar los eventos de volcado de intercambio:  
  
-   Omita la cláusula ORDER BY si no necesita que el conjunto de resultados esté ordenado.  
  
-   Si se requiere ORDER BY, elimine la columna que participa en los múltiples recorridos de intervalo (T.a en el ejemplo anterior) de la cláusula ORDER BY.  
  
-   Mediante una sugerencia de índice, fuerce que el optimizador use una ruta de acceso diferente a la tabla en cuestión.  
  
-   Vuelva a escribir la consulta para generar otro plan de ejecución de consulta.  
  
-   Fuerce la ejecución en serie de la consulta agregando la opción MAXDOP = 1 al final de la consulta o la operación de índice. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) y [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]  
>  Para determinar si el evento **Exchange Spill** se está produciendo cuando el optimizador de consultas genera un plan de ejecución, deberá recopilar también una clase de evento Showplan en el seguimiento. Puede elegir cualquiera de las clases de eventos Showplan, excepto **Showplan Text** y **Showplan Text (Unencoded)** , que no devuelven ningún identificador de nodo. Los identificadores de nodo en las clases de evento Showplans identifican cada operación que realiza el optimizador de consultas cuando genera un plan de ejecución de consultas. Estas operaciones se denominan operadores y cada operador de Showplan tiene un identificador de nodo. La columna **ObjectID** de los eventos **Exchange Spill** se corresponden con el identificador de nodo de las clases de eventos Showplan para que pueda determinar qué operador u operación está provocando el error.  
  
## <a name="exchange-spill-event-class-data-columns"></a>Columnas de datos de la clase de evento Exchange Spill  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|**ClientProcessID**|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona el identificador de proceso del cliente.|9|Sí|  
|**DatabaseID**|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|**EventClass**|**int**|Tipo de evento = 127.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado de la solicitud.|51|No|  
|**EventSubClass**|**int**|Tipo de la subclase de eventos.<br /><br /> 1=Principio del volcado<br /><br /> 2=Final del volcado|21|Sí|  
|**GroupID**|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginName**|**nvarchar**|Nombre del inicio de sesión del usuario (ya sea el de seguridad [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o las credenciales de inicio de sesión de Windows con el formato *\<DOMAIN>\\<nombre_de_usuario\>* ).|11|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede encontrar esta información en la tabla **syslogins** de la base de datos **maestra** . Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|**ObjectID**|**int**|Identificador del objeto asignado por el sistema. Se corresponde con el Id. de nodo en los eventos Showplan.|22|Sí|  
|**IdSolicitud**|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**SPID**|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**TransactionID**|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|**XactSequence**|**bigint**|Token que describe la transacción actual.|50|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Establecer opciones de índice](../../relational-databases/indexes/set-index-options.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
  
