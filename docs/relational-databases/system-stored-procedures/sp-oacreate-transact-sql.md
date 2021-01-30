---
description: sp_OACreate (Transact-SQL)
title: sp_OACreate (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OACreate
- sp_OACreate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OACreate
ms.assetid: eb84c0f1-26dd-48f9-9368-13ee4a30a27c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6d44aedf1328234cf51366c3e81bcfb2d6dc94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191992"
---
# <a name="sp_oacreate-transact-sql"></a>sp_OACreate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una instancia de un objeto OLE.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_OACreate { progid | clsid } , objecttoken OUTPUT [ , context ]   
```  
  
## <a name="arguments"></a>Argumentos  
 *Programa*  
 Es el identificador de programa (ProgID) del objeto OLE que se va a crear. Esta cadena de caracteres describe la clase del objeto OLE y tiene el formato: **'**_OLEComponent_'**.** _Objeto_**'**  
  
 *OLEComponent* es el nombre del componente de servidor de OLE Automation y *Object* es el nombre del objeto OLE. El objeto OLE especificado debe ser válido y debe ser compatible con la interfaz **IDispatch** .  
  
 Por ejemplo, SQLDMO. SQLServer es el ProgID del objeto **SQLServer** de SQL-DMO. SQL-DMO tiene un nombre de componente de SQLDMO, el objeto **SQLServer** es válido y (como todos los objetos SQL-DMO) el objeto **SQLServer** admite **IDispatch**.  
  
 *CLSID*  
 Es el identificador de clase (CLSID) del objeto OLE que se va a crear. Esta cadena de caracteres describe la clase del objeto OLE y tiene el formato: **' {**_nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn_**} '**. El objeto OLE especificado debe ser válido y debe ser compatible con la interfaz **IDispatch** .  
  
 Por ejemplo, {00026BA1-0000-0000-C000-000000000046} es el CLSID del objeto **SQLServer** de SQL-DMO.  
  
  **salida** de objecttoken  
 Es el token del objeto devuelto y debe ser una variable local del tipo de datos **int**. Este token de objeto identifica el objeto OLE creado y se utiliza en llamadas a otros procedimientos almacenados de automatización OLE.  
  
 *contextoo*  
 Especifica el contexto de ejecución en que se ejecuta el objeto OLE recién creado. Si se especifica, este valor debe ser uno de los siguientes:  
  
 **1** = solo servidor OLE en proceso (. dll).  
  
 **4** = solo servidor OLE local (. exe).  
  
 **5** = se permite el servidor OLE local y el en proceso  
  
 Si no se especifica, el valor predeterminado es **5**. Este valor se pasa como el parámetro *dwClsContext* de la llamada a **CoCreateInstance**.  
  
 Si se permite un servidor OLE en proceso (mediante el uso de un valor de contexto de **1** o **5** o si no se especifica un valor de contexto), tiene acceso a la memoria y a otros recursos que pertenecen a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un servidor OLE de proceso interno puede dañar la memoria o los recursos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y causar resultados imprevisibles, como una infracción de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Cuando se especifica un valor de contexto de **4**, un servidor OLE local no tiene acceso a ningún [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recurso y no puede dañar la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] memoria o los recursos.  
  
> [!NOTE]  
>  Los parámetros para este procedimiento almacenado se especifican por la posición, no por el nombre.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o un valor distinto de cero (error) que es el valor entero del HRESULT devuelto por el objeto de OLE Automation.  
  
 Para obtener más información sobre los códigos de retorno de HRESULT, vea [códigos de retorno e información de error de automatización OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="remarks"></a>Observaciones  
 Si se habilitan los procedimientos de OLE Automation, una llamada a **sp_OACreate** iniciará el entorno de ejecución compartido de OLE Automation. Para obtener más información acerca de cómo habilitar la automatización OLE, vea [OLE Automation Procedures (opción de configuración del servidor](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)).  
  
 El objeto OLE creado se destruye automáticamente al final del lote de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o al permiso Execute directamente en este procedimiento almacenado. `Ole Automation Procedures` la configuración debe estar **habilitada** para usar cualquier procedimiento del sistema relacionado con la automatización OLE.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-progid"></a>A. Utilizar ProgID  
 En el ejemplo siguiente se crea un objeto **SQLServer** de SQL-DMO mediante su ProgID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate 'SQLDMO.SQLServer', @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
### <a name="b-using-clsid"></a>B. Utilizar CLSID  
 En el ejemplo siguiente se crea un objeto **SQLServer** de SQL-DMO mediante su CLSID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate '{00026BA1-0000-0000-C000-000000000046}',  
    @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de automatización OLE &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Ole Automation Procedures (opción de configuración del servidor)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)   
 [Ejemplo de script de automatización OLE](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
