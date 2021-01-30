---
description: Execute (método) (RDS)
title: Método execute (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 2d9c30e9-ab5b-4920-91b8-48454c2fb5d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e5ad157ffccc07ba0b64064122f7f0e55ccd422
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163806"
---
# <a name="execute-method-rds"></a>Execute (método) (RDS)
Ejecuta la solicitud y crea un conjunto de registros ADO para su uso en ADO 2,5 y versiones posteriores.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.Execute(ConnectionString As String, HandlerString As String, QueryString As String, lFetchOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters, [lcid As Long], [pInformation])  
```  
  
#### <a name="parameters"></a>Parámetros  
 *ConnectionString*  
 Cadena que se usa para conectar con el proveedor de OLE DB donde se enviará la solicitud para su ejecución. Si se especifica un controlador mediante *HandlerString* , puede modificar o reemplazar la cadena de conexión.  
  
 *HandlerString*  
 Cadena de dos partes que identifica el controlador que se va a utilizar con esta ejecución. La cadena contiene dos partes. La primera parte contiene el nombre (ProgID) del controlador que se va a usar. La segunda parte contiene los argumentos que se van a pasar al controlador. Los detalles de cómo se interpreta la cadena arguments son específicos de cada controlador. Las dos partes se separan mediante la primera instancia de una coma de la cadena. La cadena arguments puede contener comas adicionales. Los argumentos son opcionales.  
  
 *Cadenas*  
 Comando del lenguaje de comandos compatible con el proveedor de OLE DB identificado en la cadena de conexión. En el caso de los proveedores basados en SQL, *QueryString* puede contener una instrucción de comandos de TRANSACT-SQL, pero para los proveedores que no son de SQL (por ejemplo, MSDataShape) puede que no sea una [!INCLUDE[tsql](../../../includes/tsql-md.md)] instrucción de consulta.  
  
 Si se usa un controlador, el controlador puede modificar o reemplazar el valor especificado aquí. Por ejemplo, el controlador suele reemplazar *QueryString* por una cadena de consulta de su archivo. ini. De forma predeterminada, se usa el archivo de Msdfmap.ini.  
  
 *lFetchOptions*  
 Indica el tipo de captura asincrónica.  
  
 Para obtener más información, consulte [propiedad FetchOptions (RDS)](./fetchoptions-property-rds.md).  
  
 *TableID*  
 Una **variante** de tipo VT_EMPTY o VT_BSTR. Si este valor es de tipo VT_EMPTY, se omite. Si es de tipo VT_BSTR, el conjunto de registros se crea mediante **adCmdTableDirect** y el valor especificado aquí y se omite el parámetro *QueryString* .  
  
 *lExecuteOptions*  
 Máscara de bits de las opciones de ejecución:  
  
 1 =*solo lectura* el conjunto de registros se abrirá mediante **adLockReadOnly**.  
  
 2 =*nobatch* el conjunto de registros se abrirá mediante **adLockOptimistic**.  
  
 4 =*AllParamInfoSupplied* el autor de la llamada garantiza que la información de parámetros de todos los parámetros se proporciona en *pParameters*.  
  
 8 = la información del parámetro *GetInfo* de la consulta se obtendrá del proveedor de OLE DB y se devolverá en el parámetro *pParameters* . No se ejecuta la consulta y no se devuelve ningún conjunto de registros.  
  
 16 =*GetHiddenColumns* el conjunto de registros se abrirá con **adLockBatchOptimistic** y las columnas ocultas se incluirán en el conjunto de registros.  
  
 *ReadOnly*, *nobatch* y *GetHiddenColumns* son opciones mutuamente excluyentes; sin embargo, no genera un error para establecer más de uno. Si se establecen varias opciones, *GetHiddenColumns* tiene prioridad sobre las demás, seguido de *ReadOnly*. Si no se especifica ninguna opción, de forma predeterminada, el conjunto de registros se abre mediante **adLockBatchOptimistic** y las columnas ocultas no se incluyen en el conjunto de registros.  
  
 *pParameters*  
 **Variante** que contiene una matriz segura de definiciones de parámetros. Si se ha especificado la opción *GetInfo* en *lExecuteOptions*, este parámetro se utiliza para devolver las definiciones de parámetros obtenidas del proveedor de OLE DB. De lo contrario, este parámetro puede estar vacío.  
  
 *lcid*  
 LCID que se usa para compilar los errores que se devuelven en *pInformation*.  
  
 *pInformation*  
 Un puntero a un error de información devuelto por Execute. Si es NULL, no se devuelve información de error.  
  
## <a name="remarks"></a>Observaciones  
 El parámetro *HandlerString* puede ser null. Lo que sucede en este caso depende de cómo esté configurado el servidor RDS. Una cadena de controlador de "MSDFMAP. handler" indica que se debe usar el controlador proporcionado por Microsoft (Msdfmap.dll). Una cadena de controlador de "MASDFMAP. handler, sample.ini" indica que se debe usar el controlador de Msdfmap.dll y que el argumento "sample.ini" se debe pasar al controlador. MSDFMAP.dll interpretará el argumento como una dirección para usar el sample.ini para comprobar las cadenas de conexión y de consulta.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)