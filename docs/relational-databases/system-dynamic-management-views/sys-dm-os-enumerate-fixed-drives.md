---
description: sys.dm_os_enumerate_fixed_drives (Transact-SQL)
title: sys.dm_os_enumerate_fixed_drives (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_enumerate_fixed_drives
- sys.dm_os_enumerate_fixed_drives_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_enumerate_fixed_drives dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 07855299c41457c066f878125ca289f66a87e2a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184905"
---
# <a name="sysdm_os_enumerate_fixed_drives-transact-sql"></a>sys.dm_os_enumerate_fixed_drives (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introducido en SQL Server 2019.

Enumera los volúmenes montados en Letras de unidad como `C:\` .

|Nombre de la columna|Tipo de datos|Descripción|
|-----------------|---------------|-----------------|  
|`fixed_drive_path`|`nvarchar(512)`|Ruta de acceso al volumen, como `C:\` .|  
|`drive_type`|`int`|Código para el tipo de unidad. Vea [ `GetDriveTypeW` función](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`drive_type_desc`|`nvarchar(512)`|Descripción del tipo de unidad. Vea [ `GetDriveTypeW` función](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`free_space_in_bytes`|`bigint`|Espacio libre en disco en bytes.|

## <a name="permissions"></a>Permisos

El usuario debe tener el `VIEW SERVER STATE` permiso en el servidor.

## <a name="see-also"></a>Consulte también  

 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con e/s &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
