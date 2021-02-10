---
description: Objeto de conexión (ADO)
title: Objeto de conexión (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection
helpviewer_keywords:
- Connection object [ADO]
ms.assetid: ef6b1824-5b12-43db-89d7-8f3d13896d4d
author: rothja
ms.author: jroth
ms.openlocfilehash: b09ba2f8f026b6c08d64acab6701a0d3da4013f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034775"
---
# <a name="connection-object-ado"></a>Objeto de conexión (ADO)
Representa una conexión abierta a un origen de datos.  
  
## <a name="remarks"></a>Observaciones  
 Un objeto de **conexión** representa una sesión única con un origen de datos. En un sistema de base de datos cliente/servidor, puede ser equivalente a una conexión de red real al servidor. Dependiendo de la funcionalidad admitida por el proveedor, es posible que algunas colecciones, métodos o propiedades de un objeto de **conexión** no estén disponibles.  
  
 Con las colecciones, los métodos y las propiedades de un objeto de **conexión** , puede hacer lo siguiente:  
  
-   Configure la conexión antes de abrirla con las propiedades [ConnectionString](./connectionstring-property-ado.md), [ConnectionTimeout](./connectiontimeout-property-ado.md)y [mode](./mode-property-ado.md) . **ConnectionString** es la propiedad predeterminada del objeto de **conexión** .  
  
-   Establezca la propiedad [CursorLocation](./cursorlocation-property-ado.md) en cliente para invocar el [servicio de cursores de Microsoft para OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md), que admite las actualizaciones por lotes.  
  
-   Establezca la base de datos predeterminada para la conexión con la propiedad [DefaultDatabase](./defaultdatabase-property.md) .  
  
-   Establezca el nivel de aislamiento para las transacciones abiertas en la conexión con la propiedad [IsolationLevel](./isolationlevel-property.md) .  
  
-   Especifique un proveedor de OLE DB con la propiedad [Provider](./provider-property-ado.md) .  
  
-   Establezca y, posteriormente, interrumpa la conexión física al origen de datos con los métodos de [apertura](./open-method-ado-connection.md) y [cierre](./close-method-ado.md) .  
  
-   Ejecute un comando en la conexión con el método [Execute](./execute-method-ado-connection.md) y configure la ejecución con la propiedad [CommandTimeout](./commandtimeout-property-ado.md) .  
  
    > [!NOTE]
    >  Para ejecutar una consulta sin usar un objeto Command, pase una cadena de consulta al método **Execute** de un objeto **Connection** . Sin embargo, se requiere un objeto de [comando](./command-object-ado.md) si desea conservar el texto del comando y volver a ejecutarlo, o usar parámetros de consulta.  
  
-   Administrar transacciones en la conexión abierta, incluidas las transacciones anidadas si el proveedor las admite, con los métodos [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md), [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md)y [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) y la propiedad [attributes](./attributes-property-ado.md) .  
  
-   Examine los errores devueltos del origen de datos con la colección de [errores](./errors-collection-ado.md) .  
  
-   Lea la versión de la implementación de ADO utilizada con la propiedad [version](./version-property-ado.md) .  
  
-   Obtenga información de esquema sobre la base de datos con el método [OpenSchema](./openschema-method.md) .  
  
 Puede crear objetos de **conexión** independientemente de cualquier otro objeto definido previamente.  
  
 Puede ejecutar comandos con nombre o procedimientos almacenados como si fueran métodos nativos en un objeto de **conexión** , como se muestra en la sección siguiente. Cuando un comando con nombre tiene el mismo nombre que el de un procedimiento almacenado, invocar la "llamada al método nativo" en un objeto de **conexión** siempre ejecuta el comando con nombre en lugar del procedimiento de almacenamiento.  
  
> [!NOTE]
>  No utilice esta característica (llamando a un comando o procedimiento almacenado con nombre como si fuera un método nativo en el objeto de **conexión** ) en una aplicación de Microsoft® .NET Framework, porque la implementación subyacente de la característica entra en conflicto con la forma en que el .NET Framework INTEROPERA con com.  
  
## <a name="execute-a-command-as-a-native-method-of-a-connection-object"></a>Ejecutar un comando como método nativo de un objeto de conexión  
 Para ejecutar un comando, asigne un nombre al comando mediante la propiedad **Command** Object [Name](./name-property-ado.md) . Establezca la propiedad **ActiveConnection** del objeto de **comando** en la conexión. A continuación, emita una instrucción en la que el nombre de comando se usa como si fuera un método del objeto de **conexión** , seguido de cualquier parámetro y un objeto de **conjunto de registros** si se devuelven filas. Establezca las propiedades del conjunto de **registros** para personalizar el **conjunto de registros** resultante. Por ejemplo:  
  
```  
Dim cnn As New ADODB.Connection  
Dim cmd As New ADODB.Command  
Dim rst As New ADODB.Recordset  
...  
cnn.Open "..."  
cmd.Name = "yourCommandName"  
cmd.ActiveConnection = cnn  
...  
'Your command name, any parameters, and an optional Recordset.  
cnn. "parameter", rst  
```  
  
## <a name="execute-a-stored-procedure-as-a-native-method-of-a-connection-object"></a>Ejecutar un procedimiento almacenado como un método nativo de un objeto de conexión  
 Para ejecutar un procedimiento almacenado, emita una instrucción en la que se use el nombre del procedimiento almacenado como si fuera un método en el objeto de **conexión** , seguido de cualquier parámetro. ADO realizará una "mejor estimación" de los tipos de parámetros. Por ejemplo:  
  
```  
Dim cnn As New ADODB.Connection  
...  
'Your stored procedure name and any parameters.  
cnn. "parameter"  
```  
  
 El objeto de **conexión** es seguro para el scripting.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Connection](./connection-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Command (objeto) (ADO)](./command-object-ado.md)   
 [Colección de errores (ADO)](./errors-collection-ado.md)   
 [Colección Properties (ADO)](./properties-collection-ado.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)   
 [Apéndice A: Proveedores](../../guide/appendixes/appendix-a-providers.md)