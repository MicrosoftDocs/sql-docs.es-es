---
description: Método Execute21 (RDS)
title: Método Execute21 (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Execute21 method [RDS]
ms.assetid: 9f131c8d-1497-416d-8209-abb481c38f7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 8bd48d561f9b21997df0f7a77a993f95900e8cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053226"
---
# <a name="execute21-method-rds"></a>Método Execute21 (RDS)
Ejecuta la solicitud y crea un conjunto de registros ADO para su uso en ADO 2,1.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.Execute21(ConnectionString As String, HandlerString As String, QueryString As String, lMarshalOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *ConnectionString*  
 Cadena que se usa para conectar con el proveedor de OLE DB donde se enviará la solicitud para su ejecución. Si un controlador se especifica mediante *HandlerString*, puede modificar o reemplazar la cadena de conexión.  
  
 *HandlerString*  
 La cadena identifica el controlador que se va a utilizar con esta ejecución. La cadena contiene dos partes. La primera parte contiene el nombre (ProgID) del controlador que se va a usar. La segunda parte de la cadena contiene los argumentos que se van a pasar al controlador. La forma en que se interpreta la cadena arguments es específica del controlador. Las dos partes se separan mediante la primera instancia de una coma de la cadena (aunque la cadena arguments puede contener comas adicionales). Los argumentos son opcionales.  
  
 *Cadenas*  
 Comando del lenguaje de comandos compatible con el proveedor de OLE DB identificado en la cadena de conexión. En el caso de los proveedores basados en SQL, puede que contenga una [!INCLUDE[tsql](../../../includes/tsql-md.md)] instrucción de comando, pero para los proveedores que no son de SQL (por ejemplo, MSDataShape), es posible que no sea una [!INCLUDE[tsql](../../../includes/tsql-md.md)] instrucción de consulta.  
  
 Además, si se usa un controlador (y se recomienda encarecidamente que se use un controlador), el controlador puede modificar o reemplazar el valor especificado aquí. Por ejemplo, el controlador suele reemplazar *QueryString* por una cadena de consulta de su archivo. ini. De forma predeterminada, se usa el archivo de Msdfmap.ini.  
  
 *lMarshalOptions*  
 Se usa para establecer las opciones de serialización en el conjunto de filas o el conjunto de registros que se va a devolver.  
  
 *TableID*  
 Una variante de tipo VT_EMPTY o VT_BSTR. Si este valor es de tipo VT_EMPTY, se omite. Si es de tipo VT_BSTR, el conjunto de registros se crea mediante **adCmdTableDirect** usando el valor especificado aquí y se omite el parámetro *QueryString* .  
  
 *lExecuteOptions*  
 Una máscara de la opción de ejecución:  
  
 1 =*solo lectura* el conjunto de registros se abrirá mediante **adLockReadOnly**.  
  
 2 =*nobatch* el conjunto de registros se abrirá mediante **adLockOptimistic**.  
  
 4 =*AllParamInfoSupplied* el autor de la llamada garantiza que la información de parámetros de todos los parámetros se proporciona en *pParameters*.  
  
 8 = la información del parámetro *GetInfo* de la consulta se obtendrá del proveedor de OLE DB y se devolverá en el parámetro *pParameters* . No se ejecuta la consulta y no se devuelve ningún conjunto de registros.  
  
 16 = GetHiddenColumns el conjunto de registros se abrirá con **adLockBatchOptimistic** y las columnas ocultas se incluirán en el conjunto de registros.  
  
 Aunque *ReadOnly*, *nobatch* y *GetHiddenColumns* son opciones mutuamente excluyentes, no es un error establecer más de uno. Si se establecen varias opciones, *GetHiddenColumns* tiene prioridad sobre las demás opciones, seguida de *ReadOnly*. Si no se especifica ninguna opción, de forma predeterminada, el conjunto de registros se abre utilizando **adLockBatchOptimistic** pero las columnas ocultas no se incluyen en el conjunto de registros.  
  
 *pParameters*  
 Variante que contiene una matriz segura de definiciones de parámetros. Si se ha especificado la opción *GetInfo* en *lExecuteOptions*, este parámetro se utiliza para devolver las definiciones de parámetros obtenidas del proveedor de OLE DB. De lo contrario, este parámetro puede estar vacío.  
  
## <a name="remarks"></a>Observaciones  
 El parámetro *HandlerString* puede ser null. Lo que sucede en este caso depende de cómo esté configurado el servidor RDS. Una cadena de controlador de "MSDFMAP. handler" indica que se debe usar el controlador proporcionado por Microsoft (Msdfmap.dll). Una cadena de controlador de "MASDFMAP. handler, sample.ini" indica que se debe usar el controlador de Msdfmap.dll y que el argumento "sample.ini" se debe pasar al controlador. MSDFMAP.dll interpretará el argumento como una dirección para usar el sample.ini para comprobar las cadenas de conexión y de consulta.  
  
> [!NOTE]
>  El método **Execute21** es una versión del [método execute (RDS)](./execute-method-rds.md). En el caso de que necesite usar el método **Execute** para comunicarse con ADO 2,1, se puede llamar al método **Execute21** en su lugar. Las capacidades del método **Execute** en ADO 2,5 y versiones posteriores son un superconjunto de las funciones proporcionadas para el mismo método en ADO 2,1.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)