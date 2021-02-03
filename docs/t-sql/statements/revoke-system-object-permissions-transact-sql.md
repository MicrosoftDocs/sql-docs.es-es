---
description: REVOKE (permisos de objeto de sistema de Transact-SQL)
title: REVOKE (permisos de objeto de sistema de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, system objects
- permissions [SQL Server], system objects
ms.assetid: 84983238-dd7d-45bd-99bb-52c9d8e96a87
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c3c0df1a646caa6562c640b119abe3817d15c094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185921"
---
# <a name="revoke-system-object-permissions-transact-sql"></a>REVOKE (permisos de objeto de sistema de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca permisos en objetos de sistema como procedimientos almacenados, procedimientos almacenados extendidos, funciones y vistas de una entidad de seguridad.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
REVOKE { SELECT | EXECUTE } ON [sys.]system_object FROM principal   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 [**sys.** ] .  
 Solo se necesita el calificador **sys** para hacer referencia a vistas de catálogo y vistas de administración dinámica.  
  
 *system_object*  
 Especifica el objeto en el que se va a revocar el permiso.  
  
 *principal*  
 Especifica la entidad de seguridad desde la que se revoca el permiso.  
  
## <a name="remarks"></a>Comentarios  
 Puede utilizar esta instrucción para revocar permisos para determinados procedimientos almacenados, procedimientos almacenados extendidos, funciones con valores de tabla, funciones escalares, vistas, vistas de catálogo, vistas de compatibilidad, vistas INFORMATION_SCHEMA, vistas de administración dinámica y tablas del sistema instalados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cada objeto de sistema existe como un registro único en la base de datos de recursos (**mssqlsystemresource**). La base de datos de recursos es de solo lectura. Se muestra un vínculo al objeto como registro en el esquema **sys** de todas las bases de datos.  
  
 La resolución predeterminada de nombres resuelve los nombres no calificados de procedimiento para la base de datos de recursos. Por lo tanto, el calificador **sys.** solo es necesario cuando se especifican vistas de catálogo y vistas de administración dinámica.  
  
> [!CAUTION]  
>  Si revoca permisos en objetos del sistema, se producirán errores en las aplicaciones que dependen de ellos. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utiliza vistas de catálogo y podría no funcionar correctamente si cambia los permisos predeterminados en las vistas de catálogo.  
  
 No se admite la revocación de permisos en desencadenadores y columnas de objetos del sistema.  
  
 Los permisos de objetos del sistema se mantendrán durante las actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Puede ver los objetos del sistema en la vista de catálogo [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se revoca el permiso `EXECUTE` para `sp_addlinkedserver` a `public`.  
  
```sql  
REVOKE EXECUTE ON sys.sp_addlinkedserver FROM public;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [sys.system_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [GRANT &#40;permisos de objeto de sistema de Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)   
 [DENY &#40;permisos de objeto de sistema de Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)  
  
  
