---
description: managed_backup.sp_backup_on_demand managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_on_demand (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.sp_backup_on_demand
- smart_admin.sp_backup_on_demand_TSQL
- sp_backup_on_demand_TSQL
- sp_backup_on_demand
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.sp_backup_on_demand
- sp_backup_on_demand
ms.assetid: 638f809f-27fa-4c44-a549-9cf37ecc920c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 480338a6194b07f15d8c139a6675649e5c4f8e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205297"
---
# <a name="managed_backupsp_backup_on_demand-transact-sql"></a>managed_backup.sp_backup_on_demand managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Solicita [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] que realice una copia de seguridad de la base de datos especificada.  
  
 Utilice este procedimiento almacenado para realizar copias de seguridad ad hoc para una base de datos configurada con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Esto evita cualquier interrupción en la cadena de copia de seguridad y los [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] procesos son conscientes y la copia de seguridad se almacena en el mismo contenedor de almacenamiento de blobs de Azure.  
  
 Tras finalizar correctamente la copia de seguridad, se devuelve la ruta de acceso al archivo de la copia de seguridad completa. Esto incluye el nombre y la ubicación del nuevo archivo de copia de seguridad resultante de la operación de copia de seguridad.  
  
 Se devuelve un error si [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] está en el proceso de ejecutar una copia de seguridad de un tipo determinado para la base de datos especificada. En este caso, el mensaje de error devuelto incluye la ruta de acceso al archivo de copia de seguridad completa donde la copia de seguridad actual se carga.  
   
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
EXEC managed_backup.sp_backup_on_demand   
[@database_name  =]  'database name',[@type = ] { 'Database' | 'Log' }  
  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 @database_name  
 El nombre de la base de datos en la que se realiza la copia de seguridad. @database_nameEs de **tipo SYSNAME**.  
  
 @type  
 Tipo de la copia de seguridad que se va a realizar: base de datos o registro. El @type parámetro es **nvarchar (32)**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol de base de datos **db_backupoperator** , con permisos **ALTER any Credential** y permisos **Execute** en **sp_delete_backuphistory** procedimiento almacenado.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se realiza una solicitud de copia de seguridad de base de datos para la base de datos ' TestDB '. Esta base de datos tiene [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] habilitado.  
  
```  
Use MSDB  
Go  
EXEC managed_backup.sp_backup_on_demand  
 @database_name = 'TestDB'  
,@type = 'Database'  
  
```  
  
 Para cada fragmento de código, seleccione 'tsql' en el campo de atributo language.  
  
  
