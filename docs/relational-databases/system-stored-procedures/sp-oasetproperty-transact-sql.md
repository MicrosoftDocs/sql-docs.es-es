---
description: sp_OASetProperty (Transact-SQL)
title: sp_OASetProperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OASetProperty
- sp_OASetProperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OASetProperty
ms.assetid: 0fe7d554-6b67-4d55-9d3e-4096802c47f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ff901b8f3aa7ad7c31680b8664fb753f9873cadc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195421"
---
# <a name="sp_oasetproperty-transact-sql"></a>sp_OASetProperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece un nuevo valor para una propiedad de un objeto OLE.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_OASetProperty objecttoken , propertyname , newvalue [ , index... ]  
```  
  
## <a name="arguments"></a>Argumentos  
 *objecttoken*  
 Es el token de objeto de un objeto OLE creado previamente por **sp_OACreate**.  
  
 *propertyname*  
 Es el nombre de la propiedad del objeto OLE que se va establecer en un nuevo valor.  
  
 *nuevovalor*  
 Es el nuevo valor de la propiedad y debe ser un valor del tipo de datos apropiado.  
  
 *índice*  
 Es un parámetro de índice. Si se especifica, *index* debe ser un valor del tipo de datos adecuado.  
  
 Algunas propiedades tienen parámetros. Estas propiedades se llaman propiedades indizadas y los parámetros se llaman parámetros de índice. Una propiedad puede tener varios parámetros de índice.  
  
> [!NOTE]  
>  Los parámetros para este procedimiento almacenado se especifican por posición, no por nombre.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o un valor distinto de cero (error) que es el valor entero del HRESULT devuelto por el objeto de OLE Automation.  
  
 Para obtener más información sobre los códigos de retorno de HRESULT, vea [códigos de retorno e información de error de automatización OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o al permiso Execute directamente en este procedimiento almacenado. `Ole Automation Procedures` la configuración debe estar **habilitada** para usar cualquier procedimiento del sistema relacionado con la automatización OLE.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se establece la `HostName` propiedad (del objeto **SQLServer** creado previamente) en un nuevo valor.  
  
```  
EXEC @hr = sp_OASetProperty @object, 'HostName', 'Gizmo';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END'  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de automatización OLE &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Ejemplo de script de automatización OLE](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
