---
description: MSSQLSERVER_5243
title: MSSQLSERVER_5243 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5243 (Database Engine error)
ms.assetid: e04a1934-e57d-420e-ac79-97071745824e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6cd72c5e595537fd4afa3d33581321b022d55077
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198108"
---
# <a name="mssqlserver_5243"></a>MSSQLSERVER_5243
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|5243|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico||  
|Texto del mensaje|Se detectó una incoherencia durante una operación interna. Póngase en contacto con el servicio de soporte técnico. Número de referencia %ld.|  
  
## <a name="explanation"></a>Explicación  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detectó una incoherencia estructural en una estructura de motor de almacenamiento en memoria.  
  
## <a name="user-action"></a>Acción del usuario  
Busque un error de hardware. Ejecute un diagnóstico de hardware y corrija cualquier problema. Examine también los registros del sistema y de aplicaciones de Windows, así como el registro de errores de SQL Server para ver si el error se produjo como resultado de un error de hardware. Corrija cualquier problema relacionado con el hardware que encuentre en los registros.

Si sigue teniendo problemas de datos dañados, intente intercambiar diferentes componentes de hardware para aislar el problema. Asegúrese de que el sistema no tiene habilitada la memoria caché de escritura en el controlador de disco. Si sospecha que el problema se debe a la caché de escritura, póngase en contacto con su proveedor de hardware.

Finalmente, puede resultarle útil cambiar a un nuevo sistema de hardware. Este cambio puede incluir volver a formatear las unidades de disco y volver a instalar el sistema operativo.

Restaurar desde una copia de seguridad Si el problema no está relacionado con el hardware y tiene disponible una copia de seguridad limpia, restaure la base de datos a partir de la copia de seguridad.

Ejecutar DBCC CHECKDB Si no hay una copia de seguridad limpia disponible, ejecute DBCC CHECKDB sin una cláusula REPAIR para determinar el alcance de los daños. DBCC CHECKDB recomendará el uso de una cláusula REPAIR. A continuación, ejecute DBCC CHECKDB con la cláusula REPAIR apropiada para reparar el problema.

> **no se admite la etiqueta de alerta**
> **no se admite la etiqueta tr**
> **no se admite la etiqueta tr**

Si ejecuta DBCC CHECKDB con una de las cláusulas REPAIR y no se soluciona el problema, póngase en contacto con su proveedor principal de soporte.
  
## <a name="see-also"></a>Consulte también  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Archivos y grupos de archivos de base de datos](~/relational-databases/databases/database-files-and-filegroups.md)  
  
