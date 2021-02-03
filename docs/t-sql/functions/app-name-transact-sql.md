---
description: APP_NAME (Transact-SQL)
title: APP_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APP_NAME_TSQL
- APP_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- name checking for current session [SQL Server]
- sessions [SQL Server], application names
- applications [SQL Server], names
- current session application names
- APP_NAME function
ms.assetid: e491e192-9b30-4243-bc19-33c133fe08a8
author: cawrites
ms.author: chadam
ms.openlocfilehash: d41dd7e7e7c435b627be4c9b60a32a9869eadae1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211501"
---
# <a name="app_name-transact-sql"></a>APP_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función devuelve el nombre de aplicación de la sesión actual, si la aplicación establece ese valor de nombre.
  
> [!IMPORTANT]  
>  El cliente proporciona el nombre de la aplicación y `APP_NAME` no verifica de ninguna manera el valor del nombre de la aplicación. No utilice `APP_NAME` como parte de una comprobación de seguridad.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
APP_NAME  ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
**nvarchar(128)**
  
## <a name="remarks"></a>Comentarios  
Use `APP_NAME` para distinguir entre otras aplicaciones, como una manera de realizar otras acciones para esas aplicaciones. Por ejemplo, `APP_NAME` puede distinguir entre aplicaciones diferentes, lo que permite otro formato de fecha para cada aplicación. También puede permitir el envío de un mensaje informativo a ciertas aplicaciones.
  
Para establecer el nombre de una aplicación en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], haga clic en **Opciones** en el cuadro de diálogo **Conectar al motor de base de datos**. En la pestaña **Parámetros de conexión adicionales**, indique un atributo **app** con el formato `;app='application_name'`.
  
## <a name="example"></a>Ejemplo  
En este ejemplo se comprueba si la aplicación cliente que inició este proceso es una sesión de `SQL Server Management Studio`. Después, proporciona un valor de fecha en formato de EE. UU. o ANSI.
  
```sql
USE AdventureWorks2012;  
GO  
IF APP_NAME() = 'Microsoft SQL Server Management Studio - Query'  
PRINT 'This process was started by ' + APP_NAME() + '. The date is ' + CONVERT ( VARCHAR(100) , GETDATE(), 101) + '.';  
ELSE   
PRINT 'This process was started by ' + APP_NAME() + '. The date is ' + CONVERT ( VARCHAR(100) , GETDATE(), 102) + '.';  
GO  
```  
  
## <a name="see-also"></a>Vea también
[Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
[Funciones](../../t-sql/functions/functions.md)
  
  
