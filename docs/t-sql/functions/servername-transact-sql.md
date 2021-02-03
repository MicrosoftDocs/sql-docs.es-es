---
description: '&#x40;&#x40;SERVERNAME (Transact-SQL)'
title: '@@SERVERNAME (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@SERVERNAME'
- '@@SERVERNAME_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@SERVERNAME function'
- local servers [SQL Server]
ms.assetid: b0ef33fb-954a-4294-b05b-a87c14ce25a3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 167f21cdc1f2a1e0d5558d1d95cd42005a031bd1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191095"
---
# <a name="x40x40servername-transact-sql"></a>&#x40;&#x40;SERVERNAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el nombre del servidor local en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
@@SERVERNAME  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **nvarchar**  
  
## <a name="remarks"></a>Observaciones  
 El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establece durante la instalación el nombre del equipo como nombre de servidor. Para cambiar el nombre del servidor, use **sp_addserver** y después reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si tiene instaladas varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], @@SERVERNAME devuelve la siguiente información del nombre del servidor local, siempre que el nombre no se haya cambiado desde la instalación.  
  
|Instancia|Información de servidor|  
|--------------|------------------------|  
|Instancia predeterminada|'*servername*'|  
|Instancia con nombre|'*servername*\\*instancename*'|  
|instancia de clúster de conmutación por error: instancia predeterminada|'*network_name_for_fci_in_wsfc*'|  
|instancia de clúster de conmutación por error: instancia con nombre|'*network_name_for_fci_in_wsfc*\\*instancename*'|  
  
 Aunque la función @@SERVERNAME y la propiedad SERVERNAME de la función SERVERPROPERTY puedan devolver cadenas con formatos similares, la información puede ser distinta. La propiedad SERVERNAME informa automáticamente de los cambios en el nombre de red del equipo.  
  
 Por el contrario, @@SERVERNAME no informa de estos cambios. @@SERVERNAME informa de los cambios realizados en el nombre del servidor local utilizando el procedimiento almacenado **sp_addserver** o **sp_dropserver**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra el uso de `@@SERVERNAME`.  
  
```sql  
SELECT @@SERVERNAME AS 'Server Name'  
```  
  
 A continuación se muestra un conjunto de resultados de ejemplo.  
  
```  
Server Name  
---------------------------------  
ACCTG  
  
```  
  
## <a name="see-also"></a>Vea también  
 [Funciones de configuración &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)  
  
  
