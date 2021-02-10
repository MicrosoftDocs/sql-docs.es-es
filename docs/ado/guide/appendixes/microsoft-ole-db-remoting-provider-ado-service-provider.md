---
description: Proveedor de servicios remotos de Microsoft OLE DB (proveedor de servicios ADO)
title: Proveedor de servicios remotos de Microsoft OLE DB (proveedor de servicios ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB remoting provider [ADO]
- providers [ADO], OLE DB remoting provider
- remoting provider [ADO]
ms.assetid: a4360ed4-b70f-4734-9041-4025d033346b
author: rothja
ms.author: jroth
ms.openlocfilehash: c73122e5fe1b06fb44ad69219145972ef4009202
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029249"
---
# <a name="microsoft-ole-db-remoting-provider-overview"></a>Introducción al proveedor de comunicación remota de Microsoft OLE DB
El proveedor de servicios remotos de Microsoft OLE DB permite a un usuario local en un equipo cliente invocar proveedores de datos en un equipo remoto. Especifique los parámetros del proveedor de datos para la máquina remota como si fuera un usuario local en el equipo remoto. A continuación, especifique los parámetros que usa el proveedor de comunicación remota para tener acceso al equipo remoto. Después, puede tener acceso a la máquina remota como si fuera un usuario local.

> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al  [servicio de datos de WCF](/dotnet/framework/wcf/).

## <a name="provider-keyword"></a>Palabra clave Provider
 Para invocar el proveedor de comunicación remota de OLE DB, especifique la palabra clave y el valor siguientes en la cadena de conexión. (Tenga en cuenta el espacio en blanco en el nombre del proveedor).

```vb
"Provider=MS Remote"
```

## <a name="additional-keywords"></a>Palabras clave adicionales
 Cuando se invoca este proveedor de servicios, se aplican las siguientes palabras clave adicionales.

|Palabra clave|Descripción|
|-------------|-----------------|
|**Data Source** (Origen de datos)|Especifica el nombre del origen de datos remoto. Se pasa al proveedor de comunicación remota de OLE DB para su procesamiento.<br /><br /> Esta palabra clave es equivalente a [RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Propiedad [Connect](../../reference/rds-api/connect-property-rds.md) del objeto DataControl.|

## <a name="dynamic-properties"></a>Propiedades dinámicas
 Cuando se invoca este proveedor de servicios, se agregan las siguientes propiedades dinámicas a la colección de [propiedades](../../reference/ado-api/properties-collection-ado.md) del objeto de [conexión](../../reference/ado-api/connection-object-ado.md).

|Nombre de propiedad dinámica|Descripción|
|---------------------------|-----------------|
|**DFMode**|Indica el modo DataFactory. Cadena que especifica la versión deseada del objeto [DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) en el servidor. Establezca esta propiedad antes de abrir una conexión para solicitar una versión determinada de la **factoría** de datos. Si la versión solicitada no está disponible, se intentará usar la versión anterior. Si no hay ninguna versión anterior, se producirá un error. Si **DFMode** es menor que la versión disponible, se producirá un error. Esta propiedad es de solo lectura después de realizar una conexión.<br /><br /> Puede ser uno de los siguientes valores de cadena válidos:<br /><br /> -"25"-versión 2,5 (valor predeterminado)<br />-"21"-versión 2,1<br />-"20"-versión 2,0<br />-"15"-versión 1,5|
|**Propiedades del comando**|Indica los valores que se agregarán a la cadena de propiedades de comando (conjunto de filas) que el proveedor de MS Remote envía al servidor. El valor predeterminado de esta cadena es vt_empty.|
|**DFMode actual**|Indica el número de versión real de la **factoría** de datos en el servidor. Active esta propiedad para ver si se respeta la versión solicitada en la propiedad **DFMode** .<br /><br /> Puede ser uno de los siguientes valores enteros largos válidos:<br /><br /> -25-versión 2,5 (valor predeterminado)<br />-21-versión 2,1<br />-20-versión 2,0<br />-15-versión 1,5<br /><br /> Agregar "DFMode = 20;" a la cadena de conexión cuando se usa el proveedor de **MSRemote** puede mejorar el rendimiento del servidor al actualizar los datos. Con esta configuración, el objeto **RDSServer. DataFactory** en el servidor utiliza un modo con menos recursos. Sin embargo, las siguientes características no están disponibles en esta configuración:<br /><br /> -Uso de consultas con parámetros.<br />-Obtención de información de parámetros o columnas antes de llamar al método **Execute** .<br />-Establecer **Transact updates** en **true**.<br />-Obteniendo el estado de fila.<br />-Llamando al método **Resync** .<br />-Actualización (explícita o automáticamente) a través de la propiedad **resincronización** de la actualización.<br />-Estableciendo propiedades de **comando** o **conjunto de registros** .<br />-Usar **adCmdTableDirect**.|
|**Controlador**|Indica el nombre de un programa de personalización de servidor (o controlador) que extiende la funcionalidad de [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)y cualquier parámetro utilizado por el controlador, todo separado por comas (","). Un valor **String**.|
|**Tiempo de espera de Internet**|Indica el número máximo de milisegundos que se esperará a que una solicitud viaje hacia y desde el servidor. (El valor predeterminado es 5 minutos).|
|**Proveedor remoto**|Indica el nombre del proveedor de datos que se va a usar en el servidor remoto.|
|**Servidor remoto**|Indica el nombre del servidor y el protocolo de comunicación que va a usar esta conexión. Esta propiedad es equivalente a [RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Propiedad del [servidor](../../reference/rds-api/server-property-rds.md) de objetos de contro.|
|**Actualizaciones de Transact**|Cuando se establece en **true**, este valor indica que cuando se realiza [UpdateBatch](../../reference/ado-api/updatebatch-method.md) en el servidor, se realizará dentro de una transacción. El valor predeterminado de esta propiedad dinámica booleana es **false**.|

 También puede establecer propiedades dinámicas que se pueden escribir especificando sus nombres como palabras clave en la cadena de conexión. Por ejemplo, establezca la propiedad dinámica de **tiempo de espera de Internet** en cinco segundos especificando:

```vb
Dim cn as New ADODB.Connection
cn.Open "Provider=MS Remote;Internet Timeout=5000"
```

 También puede establecer o recuperar una propiedad dinámica especificando su nombre como índice de la propiedad **Properties** . En el ejemplo siguiente se muestra cómo obtener e imprimir el valor actual de la propiedad dinámica **tiempo de espera de Internet** y, a continuación, establecer un nuevo valor:

```vb
Debug.Print cn.Properties("Internet Timeout")
cn.Properties("Internet Timeout") = 5000
```

## <a name="remarks"></a>Observaciones
 En ADO 2,0, el proveedor de comunicación remota de OLE DB solo se puede especificar en el parámetro *ActiveConnection* del método **Open** del objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) . A partir de ADO 2,1, el proveedor también se puede especificar en el parámetro *ConnectionString* del método **Open** del objeto [Connection](../../reference/ado-api/connection-object-ado.md) .

 Equivalente del **objeto RDS.** La propiedad [SQL](../../reference/rds-api/sql-property.md) del objeto DataControl no está disponible. En su lugar, se usa el argumento de *origen* del método **Open** del objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) .

 **Nota:** Especificar "...; Proveedor remoto = MS Remote;... " crearía un escenario de cuatro niveles. Los escenarios de más de tres niveles no se han probado y no deben ser necesarios.

## <a name="example"></a>Ejemplo
 En este ejemplo se realiza una consulta en la tabla **authors** de la base de datos **pubs** en un servidor denominado *yourserver*. Los nombres del origen de datos remoto y el servidor remoto se proporcionan en el método [Open](../../reference/ado-api/open-method-ado-connection.md) del objeto[Connection](../../reference/ado-api/connection-object-ado.md) y la consulta SQL se especifica en el método[Open](../../reference/ado-api/open-method-ado-recordset.md) del objeto [Recordset](../../reference/ado-api/recordset-object-ado.md) . Un objeto de **conjunto de registros** se devuelve, se edita y se usa para actualizar el origen de datos.

```vb
Dim rs as New ADODB.Recordset
Dim cn as New ADODB.Connection
cn.Open  "Provider=MS Remote;Data Source=pubs;" & _
         "Remote Server=https://YourServer"
rs.Open "SELECT * FROM authors", cn
...                'Edit the recordset
rs.UpdateBatch     'Equivalent of RDS SubmitChanges
...
```

## <a name="see-also"></a>Consulte también
 [Información general del proveedor de comunicación remota de OLE DB](/previous-versions/windows/desktop/ms713673(v=vs.85))