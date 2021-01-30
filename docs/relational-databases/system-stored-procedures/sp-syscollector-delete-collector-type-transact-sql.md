---
description: sp_syscollector_delete_collector_type (Transact-SQL)
title: sp_syscollector_delete_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_delete_collector_type
- sp_syscollector_delete_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_delete_collector_type
ms.assetid: 3f32905e-0005-42cb-aef1-7bd04c51fbac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aa80b1d9574d707d714781716290f3b62e3fd229
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183068"
---
# <a name="sp_syscollector_delete_collector_type-transact-sql"></a>sp_syscollector_delete_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina la definición de un tipo de recopilador.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_delete_collector_type [[ @collector_type_uid = ] 'collector_type_uid' ]  
          , [[ @name = ] 'name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @collector_type_uid = ] 'collector_type_uid'` Es el GUID del tipo de recopilador. *collector_type_uid* es de tipo **uniqueidentifier** y debe tener un valor si *el nombre* es NULL.  
  
`[ @name = ] 'name'` Es el nombre del tipo de recopilador. *Name* es de **tipo sysname** y debe tener un valor si *collector_type_uid* es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 *Collector_type_uid* o *Name* deben tener un valor, ambos no pueden ser null.  
  
 Este procedimiento mostrará un error si existen elementos de recopilación con este tipo de recopilador.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **dc_admin** (con permiso Execute) para ejecutar este procedimiento.  
  
## <a name="example"></a>Ejemplo  
 Este ejemplo elimina el tipo de recopilador de consultas T-SQL genérico.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_delete_collector_type @collector_type_uid = '302E93D1-3424-4be7-AA8E-84813ECF2419';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  
