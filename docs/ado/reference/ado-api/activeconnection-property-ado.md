---
description: ActiveConnection (propiedad, ADO)
title: ActiveConnection (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::ActiveConnection
- Recordset15::get_ActiveConnection
- _Record::ActiveConnection
helpviewer_keywords:
- ActiveConnection property [ADO]
ms.assetid: 52d0a96c-14fb-4ad9-b004-4d821bc0a6db
author: rothja
ms.author: jroth
ms.openlocfilehash: efcb376c9ad27dff5a0c85d73f70ae4000385e3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159233"
---
# <a name="activeconnection-property-ado"></a>ActiveConnection (propiedad, ADO)
Indica a qué objeto de [conexión](./connection-object-ado.md) pertenece actualmente el objeto de [comando](./command-object-ado.md), [conjunto de registros](./recordset-object-ado.md)o [registro](./record-object-ado.md) especificado.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** que contiene una definición para una conexión si la conexión está cerrada o una **variante** que contiene el objeto de **conexión** actual si la conexión está abierta. El valor predeterminado es una referencia de objeto null. Vea la propiedad [ConnectionString](./connectionstring-property-ado.md) .  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **ActiveConnection** para determinar el objeto de **conexión** en el que se ejecutará el objeto de **comando** especificado o se abrirá el **conjunto de registros** especificado.  
  
## <a name="command"></a>Comando  
 En el caso de los objetos de **comando** , la propiedad **ActiveConnection** es de lectura y escritura.  
  
 Si intenta llamar al método [Execute](./execute-method-ado-command.md) en un objeto **Command** antes de establecer esta propiedad en un objeto de **conexión** abierto o en una cadena de conexión válida, se produce un error.  
  
 Si se asigna un objeto de **conexión** a la propiedad **ActiveConnection** , se debe abrir el objeto. Al asignar un objeto de conexión cerrado, se produce un error.  
  
### <a name="note"></a>Nota  
 **Microsoft Visual Basic** Si se establece la propiedad **ActiveConnection** en *Nothing* , se Desasocia el objeto de **comando** de la **conexión** actual y hace que el proveedor libere los recursos asociados en el origen de datos. A continuación, puede asociar el objeto de **comando** con el mismo u otro objeto de **conexión** . Algunos proveedores permiten cambiar la configuración de la propiedad de una **conexión** a otra, sin tener que establecer primero la propiedad en *nada*.  
  
 Si la colección de [parámetros](./parameters-collection-ado.md) del objeto de **comando** contiene parámetros proporcionados por el proveedor, la colección se borra si se establece la propiedad **ActiveConnection** en *Nothing* o en otro objeto de **conexión** . Si crea manualmente objetos de [parámetro](./parameter-object.md) y los usa para rellenar la colección de **parámetros** del objeto de **comando** , al establecer la propiedad **ActiveConnection** en *Nothing* o en otro objeto de **conexión** , la colección de **parámetros** queda intacta.  
  
 Al cerrar el objeto de **conexión** con el que está asociado un objeto de **comando** , se establece la propiedad **ActiveConnection** en *Nothing*. Al establecer esta propiedad en un objeto de **conexión** cerrado, se genera un error.  
  
## <a name="recordset"></a>DataRecordsets  
 En el caso de los objetos Open **Recordset** o de los objetos **Recordset** cuya propiedad [source](./source-property-ado-recordset.md) esté establecida en un objeto **Command** válido, la propiedad **ActiveConnection** es de solo lectura. De lo contrario, es de lectura y escritura.  
  
 Puede establecer esta propiedad en un objeto de **conexión** válido o en una cadena de conexión válida. En este caso, el proveedor crea un nuevo objeto de **conexión** con esta definición y abre la conexión. Además, el proveedor puede establecer esta propiedad en el nuevo objeto de **conexión** para proporcionar una manera de tener acceso al objeto de **conexión** para obtener información de error extendida o para ejecutar otros comandos.  
  
 Si usa el argumento *ActiveConnection* del método [Open](./open-method-ado-recordset.md) para abrir un objeto de **conjunto de registros** , la propiedad **ActiveConnection** heredará el valor del argumento.  
  
 Si establece la propiedad **source** del objeto de **conjunto de registros** en una variable de objeto de **comando** válida, la propiedad **ActiveConnection** del **conjunto de registros** hereda la configuración de la propiedad **ActiveConnection** del objeto **Command** .  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** Cuando se usa en un objeto de **conjunto de registros** del cliente, esta propiedad solo se puede establecer en una cadena de conexión o (en Microsoft Visual Basic o Visual Basic, Scripting Edition) en *Nothing*.  
  
## <a name="record"></a>Grabar  
 Esta propiedad es de lectura y escritura cuando el objeto de **registro** está cerrado y puede contener una cadena de conexión o una referencia a un objeto de **conexión** abierto. Esta propiedad es de solo lectura cuando el objeto de **registro** está abierto y contiene una referencia a un objeto de **conexión** abierto.  
  
 Un objeto de **conexión** se crea implícitamente cuando el objeto de **registro** se abre desde una dirección URL. Abra el **registro** con un objeto de **conexión** abierto existente asignando el objeto de **conexión** a esta propiedad o usando el objeto de **conexión** como parámetro en la llamada al método [Open](./open-method-ado-record.md) . Si el **registro** se abre desde un **registro** o un [conjunto de registros](./recordset-object-ado.md)existente, se asocia automáticamente a ese **registro** o al objeto de **conexión** del objeto de conjunto de **registros** .  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Command (ADO)](./command-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Connection (objeto) (ADO)](./connection-object-ado.md)   
 [Propiedad ConnectionString (ADO)](./connectionstring-property-ado.md)