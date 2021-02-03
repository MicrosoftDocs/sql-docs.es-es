---
description: MSSQLSERVER_1105
title: MSSQLSERVER_1105 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1105 (Database Engine error)
ms.assetid: e7f4ad02-8c7f-4bb9-9781-2c86253f2138
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 26b6adb58381b9dd9e02f378743405f602795bbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197195"
---
# <a name="mssqlserver_1105"></a>MSSQLSERVER_1105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|1105|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|NO_MORE_SPACE_IN_FG|  
|Texto del mensaje|No se pudo asignar espacio para el objeto '%.*ls'%.\*ls de la base de datos '%.\*ls' porque el grupo de archivos '%.\*ls' está lleno. Elimine archivos innecesarios, quite objetos del grupo de archivos, agregue archivos adicionales al grupo de archivos o establezca la opción de crecimiento automático para los archivos existentes en el grupo de archivos con el fin de crear espacio en el disco.|  
  
## <a name="explanation"></a>Explicación  
No hay espacio en disco disponible en un grupo de archivos.  
  
## <a name="user-action"></a>Acción del usuario  
Las siguientes acciones pueden crear espacio disponible en el grupo de archivos:  
  
-   Active el crecimiento automático.  
  
-   Agregue más archivos al grupo de archivos.  
  
-   Libere espacio en disco quitando índices o tablas que ya no sean necesarios.  
  
-   Para obtener más información, vea el tema "Solucionar problemas de espacio en disco insuficiente para datos" en los Libros en pantalla de SQL Server.  
  
> [!NOTE]  
> Cuando un índice se encuentra en varios archivos, **ALTER INDEX REORGANIZE** puede devolver el error 1105 si uno de los archivos está lleno. El proceso de reorganización se bloquea cuando el proceso trata de mover filas al archivo que está lleno. Para solucionar esta limitación, realice una operación **ALTER INDEX REBUILD** en lugar de **ALTER INDEX REORGANIZE** o incremente el límite de crecimiento de los archivos que estén llenos.  
  
