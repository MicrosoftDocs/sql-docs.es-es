---
description: ExecuteOptionEnum
title: ExecuteOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ExecuteOptionEnum
helpviewer_keywords:
- ExecuteOptionEnum enumeration [ADO]
ms.assetid: 68bfa83a-5df4-4bef-8736-0f88ae8c29ea
author: rothja
ms.author: jroth
ms.openlocfilehash: f88d235c7ec38cd2f60ab5e07e1590f0f28e97ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167410"
---
# <a name="executeoptionenum"></a>ExecuteOptionEnum
Especifica cómo un proveedor debe ejecutar un comando.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adAsyncExecute**|0x10|Indica que el comando debe ejecutarse de forma asincrónica.<br /><br /> Este valor no se puede combinar con el valor **adCmdTableDirect** de [commandtypeenum](../../../ado/reference/ado-api/commandtypeenum.md) .|  
|**adAsyncFetch**|0x20|Indica que las filas restantes después de la cantidad inicial especificada en la propiedad [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) deben recuperarse de forma asincrónica.|  
|**adAsyncFetchNonBlocking**|0x40|Indica que el subproceso principal nunca se bloquea mientras se recupera. Si no se ha recuperado la fila solicitada, la fila actual se mueve automáticamente al final del archivo.<br /><br /> Si abre un [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) desde una [secuencia](../../../ado/reference/ado-api/stream-object-ado.md) que contiene un **conjunto de registros** almacenado de forma persistente, **adAsyncFetchNonBlocking** no tendrá ningún efecto; la operación será sincrónica y se bloqueará.<br /><br /> **adAsynchFetchNonBlocking** no tiene ningún efecto cuando se usa la opción [adCmdTableDirect](../../../ado/reference/ado-api/commandtypeenum.md) para abrir el **conjunto de registros**.|  
|**adExecuteNoRecords**|0x80|Indica que el texto del comando es un comando o un procedimiento almacenado que no devuelve filas (por ejemplo, un comando que solo inserta datos). Si se recuperan filas, se descartan y no se devuelven.<br /><br /> **adExecuteNoRecords** solo se puede pasar como parámetro opcional al método **Execute** de la conexión o **comando** .|  
|**adExecuteStream**|0x400|Indica que los resultados de la ejecución de un comando deben devolverse como una secuencia.<br /><br /> **adExecuteStream** solo se puede pasar como parámetro opcional al método **Execute de comando** .|  
|**adExecuteRecord**||Indica que **CommandText** es un comando o un procedimiento almacenado que devuelve una sola fila que debe devolverse como un objeto de **registro** .|  
|**adOptionUnspecified**|-1|Indica que el comando no está especificado.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.ExecuteOption. ASYNCEXECUTE|  
|AdoEnums.ExecuteOption. ASYNCFETCH|  
|AdoEnums.ExecuteOption. ASYNCFETCHNONBLOCKING|  
|AdoEnums.ExecuteOption. NORECORDS|  
|AdoEnums.ExecuteOption. no especificado|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Método Execute (Command ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)  
        [Execute (método) (conexión de ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)  
    :::column-end:::
    :::column:::
        [Open (método) (conjunto de registros ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)  
        [Requery (método)](../../../ado/reference/ado-api/requery-method.md)  
    :::column-end:::
:::row-end:::
