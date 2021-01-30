---
description: sys.syscacheobjects (Transact-SQL)
title: sys.syscacheobjects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscacheobjects_TSQL
- sys.syscacheobjects
- syscacheobjects
- syscacheobjects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscacheobjects system table
- sys.syscacheobjects compatibility view
ms.assetid: 9b14f37c-b7f5-4f71-b070-cce89a83f69e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e3374c67917f3468bb9b31d73acd47544d07405
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180721"
---
# <a name="syssyscacheobjects-transact-sql"></a>sys.syscacheobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre cómo se utiliza la caché.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**bucketid**|**int**|Identificador de depósito. El valor indica un intervalo de 0 a (tamaño de directorio -1). El tamaño de directorio es el de la tabla hash.|  
|**cacheobjtype**|**nvarchar (17)**|Tipo de objeto en caché:<br /><br /> Plan compilado<br /><br /> Plan ejecutable<br /><br /> Árbol de análisis<br /><br /> Cursor<br /><br /> Procedimiento almacenado extendido|  
|**objtype**|**nvarchar (8)**|Tipo de objeto:<br /><br /> Procedimiento almacenado<br /><br /> Instrucción preparada<br /><br /> Consulta ad hoc ( [!INCLUDE[tsql](../../includes/tsql-md.md)] enviada como eventos de lenguaje desde las utilidades **sqlcmd** o **osql** , en lugar de llamadas a procedimiento remoto)<br /><br /> ReplProc (procedimiento de replicación) <br /><br /> Desencadenador<br /><br /> Ver<br /><br /> Valor predeterminado<br /><br /> Tabla de usuario<br /><br /> Tabla del sistema<br /><br /> Comprobar<br /><br /> Regla|  
|**objid**|**int**|Una de las claves principales utilizadas para buscar un objeto en la caché. Este es el identificador de objeto almacenado en **sysobjects** para los objetos de base de datos (procedimientos, vistas, desencadenadores, etc.). En el caso de los objetos de caché como ad hoc o preparados SQL, **objid** es un valor generado internamente.|  
|**DBID**|**smallint**|Id. de la base de datos donde se ha compilado el objeto de caché.|  
|**dbidexec**|**smallint**|Id. de la base de datos desde la que se ejecuta la consulta.<br /><br /> Para la mayoría de los objetos, **dbidexec** tiene el mismo valor que **DBID**.<br /><br /> En el caso de las vistas del sistema, **dbidexec** es el identificador de base de datos desde el que se ejecuta la consulta.<br /><br /> En el caso de las consultas ad hoc, **dbidexec** es 0. Esto significa que **dbidexec** tiene el mismo valor que **DBID**.|  
|**UID**|**smallint**|Indica el creador del plan para los planes de consulta ad hoc y los planes preparados.<br /><br /> -2 = El lote enviado no depende de la resolución implícita de nombre y puede compartirse entre usuarios distintos. Este es el método preferido. Cualquier otro valor representa el Id. del usuario que envía la consulta en la base de datos.<br /><br /> Produce un desbordamiento o devuelve NULL si el número de usuarios y roles es superior a 32.767.|  
|**refcounts**|**int**|Número de otros objetos de caché que hacen referencia a este objeto de caché. La cuenta comienza en 1.|  
|**usecounts**|**int**|Número de veces que se ha usado este objeto de caché desde el comienzo.|  
|**pagesused**|**int**|Número de páginas consumidas por el objeto de caché.|  
|**setopts**|**int**|Valores de la opción SET que afectan a un plan compilado. Forman parte de la clave de caché. Los cambios en los valores de esta columna indican que los usuarios han modificado opciones SET. Estas opciones incluyen las siguientes:<br /><br /> **ANSI_PADDING**<br /><br /> **FORCEPLAN**<br /><br /> **CONCAT_NULL_YIELDS_NULL**<br /><br /> **ANSI_WARNINGS**<br /><br /> **ANSI_NULLS**<br /><br /> **QUOTED_IDENTIFIER**<br /><br /> **ANSI_NULL_DFLT_ON**<br /><br /> **ANSI_NULL_DFLT_OFF**|  
|**langid**|**smallint**|Identificador de idioma. Es el Id. del idioma de la conexión que creó el objeto de caché.|  
|**DateFormat**|**smallint**|Formato de fecha de la conexión que creó el objeto de caché.|  
|**status**|**int**|Indica si el objeto de caché es o no un plan de cursor. En la actualidad solo se utiliza el bit menos significativo.|  
|**lasttime**|**bigint**|Se conserva únicamente por compatibilidad con versiones anteriores. Siempre devuelve 0.|  
|**maxexectime**|**bigint**|Se conserva únicamente por compatibilidad con versiones anteriores. Siempre devuelve 0.|  
|**avgexectime**|**bigint**|Se conserva únicamente por compatibilidad con versiones anteriores. Siempre devuelve 0.|  
|**lastreads**|**bigint**|Se conserva únicamente por compatibilidad con versiones anteriores. Siempre devuelve 0.|  
|**lastwrites**|**bigint**|Se conserva únicamente por compatibilidad con versiones anteriores. Siempre devuelve 0.|  
|**SqlBytes**|**int**|Longitud en bytes de la definición del procedimiento o el lote enviado.|  
|**sql**|**nvarchar(3900)**|Definición del módulo o los primeros 3900 caracteres del lote enviado.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  

