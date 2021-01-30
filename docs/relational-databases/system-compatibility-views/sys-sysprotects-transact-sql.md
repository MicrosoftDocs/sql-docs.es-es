---
description: sys.sysprotects (Transact-SQL)
title: sys.sysprotege (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysprotects
- sys.sysprotects_TSQL
- sys.sysprotects
- sysprotects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysprotects compatibility view
- sysprotects system table
ms.assetid: 49c9658d-fb51-4c77-94a0-fba699b0102d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 764007fdbe5148b7077c8f9ffdab9c3e77244ef1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158506"
---
# <a name="syssysprotects-transact-sql"></a>sys.sysprotects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre los permisos aplicados a las cuentas de seguridad de la base de datos mediante las instrucciones GRANT y DENY.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. del objeto al que se aplican estos permisos.|  
|**UID**|**smallint**|Id. del usuario o grupo al que se aplican estos permisos. Produce un desbordamiento o devuelve NULL si el número de usuarios y roles es superior a 32.767.|  
|**action**|**tinyint**|Puede tener uno de los permisos siguientes:<br /><br /> 26 = REFERENCES <br /><br /> 178 = CREATE FUNCTION <br /><br /> 193 = SELECT <br /><br /> 195 = INSERCIÓN<br /><br /> 196 = ELIMINAR<br /><br /> 197 = ACTUALIZACIÓN<br /><br /> 198 = CREATE TABLE <br /><br /> 203 = CREATE DATABASE <br /><br /> 207 = CREATE VIEW <br /><br /> 222 = CREATE PROCEDURE <br /><br /> 224 = EXECUTE <br /><br /> 228 = BACKUP DATABASE <br /><br /> 233 = CREATE DEFAULT <br /><br /> 235 = BACKUP LOG <br /><br /> 236 = CREATE RULE|  
|**protecttype**|**tinyint**|Puede tener los siguientes valores:<br /><br /> 204 = GRANT_W_GRANT <br /><br /> 205 = GRANT <br /><br /> 206 = DENY|  
|**columnas**|**varbinary(8000)**|Mapa de bits de las columnas a las que se aplican estos permisos SELECT o UPDATE.<br /><br /> Bit 0 = Todas las columnas.<br /><br /> Bit 1 = Los permisos se aplican a esa columna.<br /><br /> NULL = No hay información.|  
|**otorgante**|**smallint**|Id. del usuario que ha concedido los permisos GRANT o DENY. Produce un desbordamiento o devuelve NULL si el número de usuarios y roles es superior a 32.767.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
