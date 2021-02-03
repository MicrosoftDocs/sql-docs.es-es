---
description: MSSQLSERVER_7995
title: MSSQLSERVER_7995 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7995 (Database Engine error)
ms.assetid: af6d6322-3cba-43d8-be97-e6ef15f8c933
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f412ba20bd29b0129df6b40403fa1449d9509d64
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197913"
---
# <a name="mssqlserver_7995"></a>MSSQLSERVER_7995
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|7995|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBCC2_SYSTEM_CATALOGS_CORRUPT|  
|Texto del mensaje|Base de datos 'DBNAME': errores de incoherencia en catálogos del sistema impiden el procesamiento de DBCC CHECKNAME.|  
  
## <a name="explanation"></a>Explicación  
El proceso de DBCC CHECKDB consta de las tres fases siguientes:  
  
1.  Comprobaciones de asignación. Esto equivale a ejecutar DBCC CHECKALLOC.  
  
2.  Comprobaciones de coherencia de las tablas del sistema. Esto equivale a ejecutar DBCC CHECKTABLE en una pequeña lista de tablas base necesarias del sistema.  
  
3.  Comprobaciones completas de coherencia de la base de datos.  

MSSQLEngine_7995 se produce en la fase 2 para indicar que DBCC CHECKDB ha encontrado errores que el comando no puede reparar o que no se ha especificado REPAIR. DBCC CHECKDB no puede continuar con la fase 3 porque las tablas base del sistema en cuestión almacenan los metadatos de todos los objetos de la base de datos o las tablas base del sistema están dañadas.  
  
## <a name="user-action"></a>Acción del usuario  
  
### <a name="look-for-hardware-failure"></a>Busque un error de hardware  
Ejecute un diagnóstico de hardware y corrija cualquier problema. Examine también los registros del sistema y de aplicación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows así como el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ver si el error se produjo como resultado de un error de hardware. Arregle cualquier problema relacionado con el hardware que encuentre en estos registros.  
  
Si sigue teniendo problemas de datos dañados, intente intercambiar diferentes componentes de hardware para aislar el problema. Asegúrese de que el sistema no tiene habilitada la memoria caché de escritura en el controlador de disco. Si cree que el problema se debe a la caché de escritura, póngase en contacto con su proveedor de hardware.  
  
Finalmente, puede resultarle útil cambiar a un nuevo sistema de hardware. Este cambio puede incluir volver a formatear las unidades de disco y volver a instalar el sistema operativo.  
  
### <a name="restore-from-backup"></a>Restaure mediante la copia de seguridad  
Si el problema no está relacionado con el hardware y tiene una copia de seguridad limpia disponible, úsela para restaurar la base de datos.  
  
### <a name="run-dbcc-checkdb"></a>Ejecute DBCC CHECKDB  
Si no tiene disponible una copia de seguridad limpia, ejecute DBCC CHECKDB sin una cláusula REPAIR para determinar el alcance de los daños. DBCC CHECKDB recomendará el uso de una cláusula REPAIR. A continuación, ejecute DBCC CHECKDB con la cláusula REPAIR apropiada para reparar el problema.  
  
> [!CAUTION]  
> Si no está seguro del efecto que tendrá DBCC CHECKDB con una cláusula REPAIR en los datos, póngase en contacto con su proveedor principal de soporte antes de ejecutar esta instrucción.  
  
Si ejecuta DBCC CHECKDB con una de las cláusulas REPAIR y no se soluciona el problema, póngase en contacto con su proveedor principal de soporte.  
  
### <a name="results-of-running-repair-options"></a>Resultados de ejecutar opciones REPAIR  
Examine la lista de errores para ver qué hará REPAIR para cada uno de ellos.  
  
