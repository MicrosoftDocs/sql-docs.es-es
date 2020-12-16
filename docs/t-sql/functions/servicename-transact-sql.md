---
description: '&#x40;&#x40;SERVICENAME (Transact-SQL)'
title: '@@SERVICENAME (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@SERVICENAME_TSQL'
- '@@SERVICENAME'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@SERVICENAME function'
- names [SQL Server], registry keys
- registry keys [SQL Server]
ms.assetid: 5b0b35be-50ae-411d-a607-bf7464b73624
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 04bcf15b50a106833fd7cf79bdd7d3cc388d74f2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481936"
---
# <a name="x40x40servicename-transact-sql"></a>&#x40;&#x40;SERVICENAME (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve el nombre de la clave del Registro bajo la cual se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. @@SERVICENAME devuelve 'MSSQLSERVER' si la instancia actual es la predeterminada; esta función devuelve el nombre de la instancia si la instancia actual es una instancia con nombre.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
@@SERVICENAME  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **nvarchar**  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta como un servicio denominado MSSQLServer.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra el uso de `@@SERVICENAME`.  
  
```sql  
SELECT @@SERVICENAME AS 'Service Name';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Service Name                    
------------------------------  
MSSQLSERVER                     
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de configuración &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Administrar el servicio del motor de base de datos](../../database-engine/configure-windows/manage-the-database-engine-services.md)  
  
  
