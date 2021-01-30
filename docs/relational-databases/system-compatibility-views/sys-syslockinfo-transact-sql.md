---
description: sys.syslockinfo (Transact-SQL)
title: sys.sysLockInfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syslockinfo_TSQL
- sys.syslockinfo_TSQL
- sys.syslockinfo
- syslockinfo
dev_langs:
- TSQL
helpviewer_keywords:
- syslockinfo system table
- sys.syslockinfo compatibility view
ms.assetid: d8cae434-807a-473e-b94f-f7a0e1b2daf0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8fb36287697a68044f8118461501dd94eb86ebc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99149377"
---
# <a name="syssyslockinfo-transact-sql"></a>sys.syslockinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre todas las solicitudes de bloqueo concedidas, convertidas y en espera.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
> [!IMPORTANT]  
>  Esta característica ha cambiado con respecto a las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [cambios importantes en las características de motor de base de datos en SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**rsc_text**|**NCHAR (32)**|Texto descriptivo de un recurso de bloqueo. Contiene una parte del nombre del recurso.|  
|**rsc_bin**|**binario (16)**|Recurso de bloqueo binario. Contiene el recurso de bloqueo efectivo contenido en el administrador de bloqueos. Esta columna se incluye para las herramientas que saben el formato de los recursos de bloqueo para generar sus propios recursos de bloqueo con formato y para realizar autocombinaciones en **syslockinfo**.|  
|**rsc_valblk**|**binario (16)**|Bloque de valor de bloqueo. Algunos tipos de recursos pueden incluir datos adicionales en el recurso de bloqueo no distribuido por el administrador de bloqueos para determinar a quién pertenece el recurso. Por ejemplo, los bloqueos de página no son propiedad de un identificador de objeto determinado. Para la extensión de bloqueo y otros fines. Sin embargo, se puede colocar el Id. de objeto de un bloqueo de página en el bloque de valor de bloqueo.|  
|**rsc_dbid**|**smallint**|Id. de la base de datos asociada al recurso.|  
|**rsc_indid**|**smallint**|Id. del índice asociado al recurso, si es el caso.|  
|**rsc_objid**|**int**|Id. del objeto asociado al recurso, si es el caso.|  
|**rsc_type**|**tinyint**|Tipo de recurso:<br /><br /> 1 = Recurso NULL (no utilizado)<br /><br /> 2 = base de datos<br /><br /> 3 = Archivo<br /><br /> 4 = Índice<br /><br /> 5 = Tabla<br /><br /> 6 = Página<br /><br /> 7 = Clave<br /><br /> 8 = Extensión<br /><br /> 9 = RID (Id. de fila)<br /><br /> 10 = Aplicación|  
|**rsc_flag**|**tinyint**|Marcas internas del recurso.|  
|**req_mode**|**tinyint**|Modo de solicitud de bloqueo. Esta columna es el modo de bloqueo del solicitante y representa el modo concedido, el de conversión o el de espera.<br /><br /> 0 = NULL. No se concede acceso al recurso. Sirve como marcador de posición.<br /><br /> 1 = Sch-S (Estabilidad del esquema). Garantiza que un elemento de un esquema, como una tabla o un índice, no se quite mientras una sesión mantenga un bloqueo de estabilidad del esquema sobre él.<br /><br /> 2 = Sch-M (Modificación del esquema). Debe mantenerlo cualquier sesión que desee cambiar el esquema del recurso especificado. Garantiza que ninguna otra sesión se refiera al objeto indicado.<br /><br /> 3 = S (Compartido). La sesión que lo mantiene recibe acceso compartido al recurso.<br /><br /> 4 = U (Actualizar). Indica que se ha obtenido un bloqueo de actualización sobre recursos que finalmente se pueden actualizar. Se utiliza para evitar una forma común de interbloqueo que tiene lugar cuando varias sesiones bloquean recursos para una posible actualización en el futuro.<br /><br /> 5 = X (Exclusivo). La sesión que lo mantiene recibe acceso exclusivo al recurso.<br /><br /> 6 = IS (Intención compartida). Indica la intención de establecer bloqueos S en algún recurso subordinado de la jerarquía de bloqueos.<br /><br /> 7 = IU (Actualizar intención). Indica la intención de establecer bloqueos U en algún recurso subordinado de la jerarquía de bloqueos.<br /><br /> 8 = IX (Intención exclusiva). Indica la intención de colocar bloqueos X en algunos recursos subordinados en la jerarquía de bloqueos.<br /><br /> 9 = SIU (Actualizar intención compartida). Indica el acceso compartido a un recurso con la intención de obtener bloqueos de actualización sobre recursos subordinados en la jerarquía de bloqueos.<br /><br /> 10 = SIX (Intención compartida exclusiva). Indica acceso compartido a un recurso con la intención de obtener bloqueos exclusivos sobre recursos subordinados de la jerarquía de bloqueos.<br /><br /> 11 = UIX (Actualizar intención exclusiva). Indica un bloqueo de actualización en un recurso con la intención de adquirir bloqueos exclusivos sobre recursos subordinados en la jerarquía de bloqueos.<br /><br /> 12 = BU. Utilizado en las operaciones masivas.<br /><br /> 13 = RangeS_S (Intervalo de claves compartido y bloqueo de recurso compartido). Indica recorrido de intervalo serializable.<br /><br /> 14 = RangeS_U (Intervalo de claves compartido y bloqueo de recurso de actualización). Indica recorrido de actualización serializable.<br /><br /> 15 = RangeI_N (Insertar intervalo de claves y bloqueo de recurso Null). Se utiliza para probar los intervalos antes de insertar una clave nueva en un índice.<br /><br /> 16 = RangeI_S. Bloqueo de conversión de intervalo de claves, creado por una superposición de bloqueos RangeI_N y S.<br /><br /> 17 = RangeI_U. Bloqueo de conversión de intervalo de claves, creado por una superposición de bloqueos RangeI_N y U.<br /><br /> 18 = RangeI_X. Bloqueo de conversión de intervalo de claves, creado por una superposición de bloqueos RangeI_N y X.<br /><br /> 19 = RangeX_S. Bloqueo de conversión de rango de claves, creado por una superposición de bloqueos RangeI_N y RangeS_S pestillo.<br /><br /> 20 = RangeX_U. Bloqueo de conversión de intervalo de claves, creado por una superposición de bloqueos RangeI_N y RangeS_U.<br /><br /> 21 = RangeX_X (Intervalo de claves exclusivo y bloqueo de recurso exclusivo). Es un bloqueo de conversión que se utiliza cuando se actualiza una clave de un intervalo.|  
|**req_status**|**tinyint**|Estado de la solicitud de bloqueo:<br /><br /> 1 = Concedido<br /><br /> 2 = En conversión<br /><br /> 3 = En espera|  
|**req_refcnt**|**smallint**|Recuento de referencia de bloqueos. Cada vez que una transacción solicita el bloqueo de un recurso determinado, se incrementa un recuento de referencia. El bloqueo no se puede liberar hasta que el recuento de referencia sea cero.|  
|**req_cryrefcnt**|**smallint**|Reservado para uso futuro. Siempre se establece en 0.|  
|**req_lifetime**|**int**|Mapa de bits de la duración del bloqueo. En algunas estrategias de procesamiento de consultas, es necesario mantener los bloqueos sobre los recursos hasta que el procesador de consultas haya completado una fase determinada de la consulta. El procesador de consultas y el administrador de transacciones utilizan el mapa de bits de la duración del bloqueo para indicar los grupos de bloqueos que se pueden liberar cuando se ha completado la ejecución de una fase determinada de una consulta. Ciertos bits del mapa se utilizan para indicar los bloqueos que se deben mantener hasta el final de la transacción, incluso cuando su recuento de referencia sea cero.|  
|**req_spid**|**int**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] Identificador de proceso interno de la sesión que solicita el bloqueo.|  
|**req_ecid**|**int**|Id. del contexto de ejecución (ECID). Se utiliza para indicar qué subproceso de una operación en paralelo es el propietario de un bloqueo determinado.|  
|**req_ownertype**|**smallint**|Tipo del objeto asociado al bloqueo:<br /><br /> 1 = Transacción<br /><br /> 2 = Cursor<br /><br /> 3 = Sesión<br /><br /> 4 = ExSession<br /><br /> Observe que el 3 y el 4 representan una versión especial de bloqueos de sesión, que realizan un seguimiento de los bloqueos de bases de datos y de grupos de archivos respectivamente.|  
|**req_transactionID**|**bigint**|IDENTIFICADOR de transacción único utilizado en **syslockinfo** y en el evento del generador de perfiles|  
|**req_transactionUOW**|**uniqueidentifier**|Identifica el Id. de unidad de trabajo (UOW) de la transacción DTC. En las transacciones que no son MS DTC, UOW se establece en 0.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
