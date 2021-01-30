---
description: sp_get_query_template (Transact-SQL)
title: sp_get_query_template (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_get_query_template
- sp_get_query_template_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_query_template
ms.assetid: 85e9bef7-2417-41a8-befa-fe75507d9bf2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89e30950c072cbfe5737a65934155276bbf6a27d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204780"
---
# <a name="sp_get_query_template-transact-sql"></a>sp_get_query_template (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el formato con parámetros de una consulta. Los resultados devueltos simulan el formato con parámetros de un consulta resultante cuando se utiliza la parametrización forzada. sp_get_query_template se utiliza principalmente al crear guías de plan de plantilla.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_get_query_template  
   [ @querytext = ] N'query_text'  
   , @templatetext OUTPUT   
   , @parameters OUTPUT   
```  
  
## <a name="arguments"></a>Argumentos  
 '*query_text*'  
 Es la consulta para la que se va a generar la versión con parámetros. '*query_text*' debe ir entre comillas simples e ir precedida del especificador Unicode N. N '*query_text*' es el valor asignado al @querytext parámetro. Es de tipo **nvarchar (Max)**.  
  
 @templatetext  
 Es un parámetro de salida de tipo **nvarchar (Max)**, proporcionado como se indica, para recibir la forma parametrizada de *query_text* como un literal de cadena.  
  
 @parameters  
 Es un parámetro de salida de tipo **nvarchar (Max)**, proporcionado como se indica, para recibir un literal de cadena de los nombres de parámetro y los tipos de datos con parámetros en @templatetext .  
  
## <a name="remarks"></a>Observaciones  
 sp_get_query_template devuelve un error cuando se produce lo siguiente:  
  
-   No parametriza ningún valor literal constante en *query_text*.  
  
-   *query_text* es null, no una cadena Unicode, no es válido sintácticamente o no se puede compilar.  
  
 Si sp_get_query_template devuelve un error, no modifica los valores de los parámetros de @templatetext @parameters salida y.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol de base de datos public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve el formato con parámetros de una consulta que contiene dos valores literales constantes.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @my_templatetext nvarchar(max)  
DECLARE @my_parameters nvarchar(max)  
EXEC sp_get_query_template   
    N'SELECT pi.ProductID, SUM(pi.Quantity) AS Total  
        FROM Production.ProductModel pm   
        INNER JOIN Production.ProductInventory pi  
        ON pm.ProductModelID = pi.ProductID  
        WHERE pi.ProductID = 2  
        GROUP BY pi.ProductID, pi.Quantity  
        HAVING SUM(pi.Quantity) > 400',  
@my_templatetext OUTPUT,  
@my_parameters OUTPUT;  
SELECT @my_templatetext;  
SELECT @my_parameters;  
```  
  
 Estos son los resultados con parámetros del parámetro `@my_templatetext``OUTPUT`:  
  
 `select pi . ProductID , SUM ( pi . Quantity ) as Total`  
  
 `from Production . ProductModel pm`  
  
 `inner join Production . ProductInventory pi`  
  
 `on pm . ProductModelID = pi . ProductID`  
  
 `where pi . ProductID = @0`  
  
 `group by pi . ProductID , pi . Quantity`  
  
 `having SUM ( pi . Quantity ) > 400`  
  
 Observe que el primer literal constante, `2`, se convierte en un parámetro. El segundo literal, `400`, no se convierte porque está en una cláusula `HAVING`. Los resultados devueltos por sp_get_query_template simulan la forma con parámetros de una consulta cuando la opción PARAMETERIZATION de ALTER DATABASE está establecida en FORCED.  
  
 Estos son los resultados con parámetros del parámetro `@my_parameters OUTPUT`:  
  
```  
@0 int  
```  
  
> [!NOTE]  
>  El orden y el nombre de los parámetros en la salida de sp_get_query_template puede variar en la ingeniería de corrección rápida, el Service Pack y las actualizaciones de versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las actualizaciones también pueden hacer que se parametrice un conjunto distinto de literales constantes para la misma consulta y que se aplique un espaciado diferente a los resultados de ambos parámetros de salida.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Especificar el comportamiento de parametrización de consultas por medio de guías de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)  
  
  
