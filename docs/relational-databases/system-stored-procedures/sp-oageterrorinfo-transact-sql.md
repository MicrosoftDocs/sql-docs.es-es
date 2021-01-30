---
description: sp_OAGetErrorInfo (Transact-SQL)
title: sp_OAGetErrorInfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAGetErrorInfo_TSQL
- sp_OAGetErrorInfo
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetErrorInfo
ms.assetid: ceecea08-456f-4819-85d9-ecc9647d7187
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d55126c7feb362b910e83034a66e4d691f1be84c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191982"
---
# <a name="sp_oageterrorinfo-transact-sql"></a>sp_OAGetErrorInfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Obtiene información de errores de OLE Automation.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_OAGetErrorInfo [ objecttoken ]  
    [ , source OUTPUT ]   
    [ , description OUTPUT ]   
    [ , helpfile OUTPUT ]   
    [ , helpid OUTPUT ]   
```  
  
## <a name="arguments"></a>Argumentos  
 *objecttoken*  
 Es el token de objeto de un objeto OLE creado previamente mediante **sp_OACreate** o es NULL. Si se especifica *objecttoken* , se devuelve información de error para ese objeto. Si se especifica NULL, se devuelve la información de error de todo el lote.  
  
  **salida** de origen  
 Es el origen de la información de error. Si se especifica, debe ser una variable de tipo **Char**, **nchar**, **VARCHAR** o **nvarchar** local. El valor devuelto se trunca, si es necesario, para que se ajuste a la variable local.  
  
  **salida** de Descripción  
 Es la descripción del error. Si se especifica, debe ser una variable de tipo **Char**, **nchar**, **VARCHAR** o **nvarchar** local. El valor devuelto se trunca, si es necesario, para que se ajuste a la variable local.  
  
  **salida** de HelpFile  
 Es el archivo de ayuda del objeto OLE. Si se especifica, debe ser una variable de tipo **Char**, **nchar**, **VARCHAR** o **nvarchar** local. El valor devuelto se trunca, si es necesario, para que se ajuste a la variable local.  
  
  **salida** de helpID  
 Es el identificador de contexto del archivo de ayuda. Si se especifica, debe ser una variable local **int** .  
  
> [!NOTE]  
>  Los parámetros para este procedimiento almacenado se especifican por posición, no por nombre.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o un valor distinto de cero (error) que es el valor entero del HRESULT devuelto por el objeto de OLE Automation.  
  
 Para obtener más información sobre los códigos de retorno de HRESULT, vea [códigos de retorno e información de error de automatización OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Si no se especifican parámetros de salida, la información de error se devuelve al cliente como un conjunto de resultados.  
  
|Nombres de columna|Tipo de datos|Descripción|  
|------------------|---------------|-----------------|  
|**Error**|**binario (4)**|Representación binaria del número de error.|  
|**Origen**|**nvarchar (NN)**|Origen del error.|  
|**Descripción**|**nvarchar (NN)**|Descripción del error.|  
|**HelpFile**|**nvarchar (NN)**|Archivo de ayuda del origen.|  
|**HelpID**|**int**|Id. del contexto de Ayuda del archivo de origen correspondiente.|  
  
## <a name="remarks"></a>Observaciones  
 Cada llamada a un procedimiento almacenado de OLE Automation (excepto **sp_OAGetErrorInfo**) restablece la información de error; por tanto, **sp_OAGetErrorInfo** solo obtiene la información de error de la llamada más reciente a un procedimiento almacenado de OLE Automation. Tenga en cuenta que, dado que **sp_OAGetErrorInfo** no restablece la información de error, se puede llamar varias veces para obtener la misma información de error.  
  
 La tabla siguiente muestra los errores de OLE Automation y sus causas comunes.  
  
|Error y HRESULT|Causa común|  
|-----------------------|------------------|  
|**Tipo de variable incorrecto ?(0x80020008)**|El tipo de datos de un [!INCLUDE[tsql](../../includes/tsql-md.md)] valor pasado como parámetro de método no coincidía con el [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] tipo de datos del parámetro de método o se pasó un valor NULL como parámetro de método.|  
|**Nombre desconocido? (0x8002006)**|No se encontró el nombre de la propiedad o del método especificado para el objeto especificado.|  
|**Cadena de clase no válida (0x800401f3)**|El ProgID o CLSID especificado no está registrado como objeto OLE en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los servidores de automatización OLE personalizados deben registrarse para poder crear instancias de ellos mediante **sp_OACreate**. Esto puede hacerse mediante el uso de la utilidad de Regsvr32.exe para servidores en proceso (. dll) o el modificador de línea de comandos **/regserver** para servidores locales (. exe).|  
|**Error de ejecución del servidor (0x80080005)**|El objeto OLE especificado está registrado como servidor OLE local (archivo .exe), pero no se pudo encontrar o iniciar el archivo .exe.|  
|**No se pudo encontrar el módulo especificado (0x8007007e)**|El objeto OLE especificado está registrado como servidor OLE en proceso (archivo .dll), pero no se pudo encontrar o cargar el archivo .dll.|  
|**El tipo no coincide (0x80020005)**|El tipo de datos de una variable local de [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizada para almacenar un valor de propiedad o un valor de método devueltos no coincidió con el tipo de datos de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] para el valor devuelto de la propiedad o método. O bien, se solicitó el valor devuelto de una propiedad o método, pero no se devuelve un valor.|  
|**El tipo de valor o el valor del parámetro ' context ' de sp_OACreate no es válido. (0x8004275B)**|El valor del parámetro de contexto debe ser uno de los siguientes: 1, 4 o 5.|  
  
 Para obtener más información sobre el procesamiento de códigos de retorno HRESULT, vea [códigos de retorno e información de error de automatización OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o al permiso Execute directamente en este procedimiento almacenado. `Ole Automation Procedures` la configuración debe estar **habilitada** para usar cualquier procedimiento del sistema relacionado con la automatización OLE.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente muestra la información de errores de OLE Automation.  
  
```  
DECLARE @output varchar(255);  
DECLARE @hr int;  
DECLARE @source varchar(255);  
DECLARE @description varchar(255);  
PRINT 'OLE Automation Error Information';  
EXEC @hr = sp_OAGetErrorInfo @object, @source OUT, @description OUT;  
IF @hr = 0  
BEGIN  
    SELECT @output = '  Source: ' + @source  
    PRINT @output  
    SELECT @output = '  Description: ' + @description  
    PRINT @output  
END  
ELSE  
BEGIN  
    PRINT '  sp_OAGetErrorInfo failed.'  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de automatización OLE &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Ejemplo de script de automatización OLE](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
