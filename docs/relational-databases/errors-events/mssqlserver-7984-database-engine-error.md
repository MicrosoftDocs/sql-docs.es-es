---
description: MSSQLSERVER_7984
title: MSSQLSERVER_7984 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7984 (Database Engine error)
ms.assetid: e3192f56-e4e2-41da-b132-65f1e7540b1a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c936897cba6b26048f83bd9a4f20fb51fd0f7fd3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197952"
---
# <a name="mssqlserver_7984"></a>MSSQLSERVER_7984
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|7984|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBCC2_PRE_CHECKS_BAD_PAGE_TYPE|  
|Texto del mensaje|Comprobaciones previas de tabla del sistema: id. de objeto O_ID. La página P_ID tiene un tipo de página inesperado PAGETYPE. Instrucción de comprobación terminada debido a un error irreparable.|  
  
## <a name="explanation"></a>Explicación  
Se encontró una página con un tipo distinto de DATA_PAGE en el nivel de datos del objeto especificado. Este error se produce durante la primera fase de las comprobaciones del comando DBCC CHECKDB. Durante esa fase, DBCC CHECKDB realiza comprobaciones básicas en las páginas de datos de las tablas base críticas del sistema.  
  
> [!NOTE]  
> Si se encuentran errores en las tablas del sistema, los errores no se pueden reparar; por lo tanto, el comando DBCC CHECKDB finaliza inmediatamente.  
  
## <a name="user-action"></a>Acción del usuario  
  
### <a name="look-for-hardware-failure"></a>Busque un error de hardware  
Ejecute un diagnóstico de hardware y corrija cualquier problema. Examine también los registros del sistema y de aplicación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows así como el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ver si el error se produjo como resultado de un error de hardware. Arregle cualquier problema relacionado con el hardware que encuentre en estos registros.  
  
Si sigue teniendo problemas de datos dañados, intente intercambiar diferentes componentes de hardware para aislar el problema. Asegúrese de que el sistema no tiene habilitada la memoria caché de escritura en el controlador de disco. Si cree que el problema se debe a la caché de escritura, póngase en contacto con su proveedor de hardware.  
  
Finalmente, puede resultarle útil cambiar a un nuevo sistema de hardware. Este cambio puede incluir volver a formatear las unidades de disco y volver a instalar el sistema operativo.  
  
### <a name="restore-from-backup"></a>Restaure mediante la copia de seguridad  
Si el problema no está relacionado con el hardware y tiene una copia de seguridad limpia disponible, úsela para restaurar la base de datos.  
  
### <a name="run-dbcc-checkdb"></a>Ejecute DBCC CHECKDB  
No aplicable. Este error no puede repararse automáticamente. Si no puede restaurar la base de datos a partir de una copia de seguridad, póngase en contacto con el Servicio de atención al cliente y soporte técnico (CSS) de [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
