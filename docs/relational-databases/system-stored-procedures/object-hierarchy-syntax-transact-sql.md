---
description: Sintaxis de jerarquía de objetos (Transact-SQL)
title: Sintaxis de jerarquía de objetos (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], hierarchy syntax
ms.assetid: 7ed8df86-9fd2-4e09-96bc-5381fec85f65
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6ebfc2afdcbaa3ca5eddd5d311262ba19780e332
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205253"
---
# <a name="object-hierarchy-syntax-transact-sql"></a>Sintaxis de jerarquía de objetos (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  El parámetro *PropertyName* de sp_OAGetProperty y sp_OASetProperty y el parámetro *MethodName* de sp_OAMethod admiten una sintaxis de jerarquía de objetos similar a la de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] . Cuando se utiliza esta sintaxis especial, estos parámetros tienen la forma general siguiente:  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
'TraversedObject.PropertyOrMethod'  
```  
  
## <a name="arguments"></a>Argumentos  
 *TraversedObject*  
 Es un objeto OLE de la jerarquía en el *objecttoken* especificado en el procedimiento almacenado. Utilice la sintaxis de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] para especificar un conjunto de colecciones, propiedades de objetos y métodos que devuelven objetos. Cada especificador de objeto del conjunto debe estar separado con un punto .  
  
 Un elemento del conjunto puede ser el nombre de una colección. Utilice esta sintaxis para especificar una colección:  
  
 Colección ("*Item*")  
  
 Las comillas dobles (") son necesarias. No se acepta la sintaxis de signo de admiración de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] (!) para las recopilaciones.  
  
 *PropertyOrMethod*  
 Es el nombre de una propiedad o método de *TraversedObject*.  
  
 Para especificar todos los índices o parámetros de métodos mediante los parámetros sp_OAGetProperty, sp_OASetProperty o sp_OAMethod  (incluida la compatibilidad con los parámetros de resultado de sp_OAMethod), utilice esta sintaxis:  
  
 *PropertyOrMethod*  
  
 Para especificar todos los índices o parámetros de métodos entre paréntesis (para que se pasen por alto todos los índices o parámetros de métodos de sp_OAGetProperty, sp_OASetProperty o sp_OAMethod) utilice esta sintaxis:  
  
 *PropertyOrMethod*([ *parameterName*: =] "*parámetro*" [,...])  
  
 Las comillas dobles (") son necesarias. Todos los parámetros con nombre deben especificarse después de especificar todos los parámetros de posición.  
  
## <a name="remarks"></a>Observaciones  
 Si no se especifica *TraversedObject* , se requiere *PropertyOrMethod* .  
  
 Si no se especifica *PropertyOrMethod* , *TraversedObject* se devuelve como un parámetro de salida de token de objeto del procedimiento almacenado de automatización OLE. Si se especifica *PropertyOrMethod* , se llama a la propiedad o método de *TraversedObject* y el valor de propiedad o el valor devuelto del método se devuelve como parámetro de salida del procedimiento almacenado de automatización OLE.  
  
 Si algún elemento de la lista *TraversedObject* no devuelve un objeto OLE, se produce un error.  
  
 Para obtener más información acerca de la sintaxis de objetos OLE de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], vea la documentación de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].  
  
 Para obtener más información sobre los códigos de retorno de HRESULT, vea [sp_OACreate &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 A continuación se muestran ejemplos de la sintaxis de jerarquía de objetos que usa un objeto de SQL-DMO SQLServer.  
  
```  
-- Get the AdventureWorks2012 Person.Address Table object.  
EXEC @hr = sp_OAGetProperty @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address")',  
   @table OUT  
  
-- Get the Rows property of the AdventureWorks2012 Person.Address table.  
EXEC @hr = sp_OAGetProperty @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address").Rows',  
   @rows OUT  
  
-- Call the CheckTable method to validate the   
-- AdventureWorks2012 Person.Address table.  
EXEC @hr = sp_OAMethod @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address").CheckTable',  
   @checkoutput OUT  
```  
  
## <a name="see-also"></a>Consulte también  
 [Script de ejemplo de automatización OLE](../../relational-databases/stored-procedures/ole-automation-sample-script.md)   
 [Procedimientos almacenados de automatización OLE &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
  
  
