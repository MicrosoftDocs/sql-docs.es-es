---
description: Características de ADO para cada versión
title: Historial de ADO | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, what's new
ms.assetid: 667673f2-3151-432b-894a-3fc60b704ea4
author: rothja
ms.author: jroth
ms.openlocfilehash: 7247523daf68116c949d4ec8ad3eec90fe4c8fe2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030623"
---
# <a name="ado-features-for-each-release"></a>Características de ADO para cada versión

En este tema se enumeran las nuevas características introducidas por cada versión de ADO, ADO MD y ADOX.

## <a name="ado-60"></a>ADO 6,0

ADO 6,0 se incluye en Windows Vista, como parte de Windows Data Access Components (Windows DAC) 6,0. ADO 6,0 es funcionalmente equivalente a ADO 2,8.

## <a name="ado-28"></a>ADO 2,8

ADO 2,8 se incluyó en Windows XP y Windows Server 2003, como parte de Microsoft Data Access Components (MDAC) 2,8. También está disponible una versión redistribuible de MDAC 2,8; Tenga en cuenta que esta versión redistribuible solo debe instalarse en Windows 2000. ADO 2,8 aborda varios problemas relacionados con la seguridad:

*No se permite el acceso a la unidad de disco duro fuera de una zona de confianza.*
En el scripting entre dominios que implica sitios que no son de confianza, se deshabilitan las siguientes operaciones: **Stream. SaveToFile**, **Stream. LoadFromFile**, **Recordset. Save** y **Recordset. Open**, que se usan junto con la marca **adCmdFile** o con el proveedor de persistencia de Microsoft OLE DB (MSPersist).

**Recordset. Open** _,_  **Recordset. Save** _,_  **Stream. SaveToFile** _y_  **Stream. LoadFromFile**  _solo funcionan en archivos físicos._
Estos métodos comprueban ahora que los identificadores de archivo apuntan únicamente a archivos físicos.

**Recordset. ActiveCommand**  _devuelve un error cuando se invoca desde una página HTML/ASP._
Esto evita que el objeto de **comando** se use inutilizable.

_El número de_**conjuntos de registros**_devueltos por un comando de forma anidada__tiene un límite superior._        
Ahora, un comando de forma anidada devuelve un máximo de 512 **conjuntos de registros**. Esto significa que un comando de **forma** ya no puede anidarse a cualquier profundidad. En su lugar, la profundidad de nivel máximo es 512, si cada comando da como resultado un **conjunto de registros** único (secundario). Si, en cualquier nivel, un comando **Shape** devuelve varios **conjuntos de registros**, el nivel máximo de profundidad será inferior a 512.

## <a name="ado-27"></a>ADO 2,7

*compatibilidad con la plataforma de 64 bits* ADO 2,7 introduce compatibilidad con procesadores de 64 bits.

## <a name="ado-26"></a>ADO 2,6

_Método_ **CubDef. GetSchemaObject** a partir de ADO 2,6, ADO MD objetos se pueden recuperar mediante nombres únicos, tal y como se especifica en la [propiedad UniqueName (ADO MD)](../reference/ado-md-api/uniquename-property-ado-md.md).   No es necesario conocer los nombres de los objetos primarios y no es necesario rellenar las colecciones primarias para recuperar un objeto de esquema. Consulte [método GetSchemaObject (ADO MD)](../reference/ado-md-api/getschemaobject-method-ado-md.md).

*Secuencias de comandos* El objeto de **comando** admite comandos en formato de secuencia como alternativa al uso de la propiedad **CommandText** . La [propiedad CommandStream (ADO)](../reference/ado-api/commandstream-property-ado.md) se puede usar para especificar plantillas XML o diagramas como entrada de **comando** con el proveedor de Microsoft OLE DB para SQL Server.

El dialecto de la propiedad de **dialecto** 
 [](../reference/ado-api/dialect-property.md) es una nueva propiedad que define la sintaxis y las reglas generales que el proveedor utiliza para analizar la cadena o el flujo.  

**Command.Exe**  _método_ de la función de [ejecución el método Execute](../reference/ado-api/execute-method-ado-command.md) del objeto **Command** de ADO se ha mejorado para usar secuencias para la entrada y la salida.

*Campo statusvalues* Si el usuario encuentra un error DB_E_ERRORSOCCURRED al modificar un **campo** de un **conjunto de registros**, ADO rellenará ahora la propiedad **Field. status** con la información de estado adecuada para que el usuario tenga más información sobre lo que ha ido mal. Vea [propiedad Status (campo ADO)](../reference/ado-api/status-property-ado-field.md).

La propiedad **NamedParameters** 
 [NamedParameters](../reference/ado-api/namedparameters-property-ado.md) es una nueva propiedad del objeto **Command** que indica que el proveedor debe usar parámetros con nombre.  

*Conjuntos de ResultSet en secuencias* ADO puede devolver conjuntos de resultados de un origen de datos en una **secuencia**, en lugar de un objeto de **conjunto de registros** . Con la versión más reciente del proveedor de Microsoft OLE DB para SQL Server, puede obtener resultados XML del proveedor ejecutando una consulta "for XML". Una **secuencia** que recibe el conjunto de resultados se puede abrir con un comando "for XML" como origen. Consulte [recuperación de conjuntos de resultados en transmisiones](./data/retrieving-resultsets-into-streams.md).

*Conjunto de resultados de fila única* Ahora se puede abrir el objeto de **registro** de ADO en una cadena de comandos o un objeto de **comando** que devuelve una fila de datos del proveedor. Esto mejora el rendimiento con los proveedores de MDAC 2,6. Vea [método Open (registro de ADO)](../reference/ado-api/open-method-ado-record.md).

## <a name="ado-25"></a>ADO 2,5

 _Objeto_ de registro ADO 2,5 introduce el objeto de **registro** para representar y administrar una fila de un **conjunto de registros** o un proveedor de datos, o un objeto que encapsula los datos semiestructurados, como un archivo o un directorio.

 _Objeto_ de secuencia ADO 2,5 también presenta el objeto *de secuencia y** * para representar una secuencia de datos binarios o de texto.

*Enlace de URL* ADO 2,5 introduce el uso de una dirección URL, como alternativa a una cadena de conexión y un texto de comando, para asignar nombres a los objetos de almacén de datos. Se puede usar una dirección URL con los objetos **Connection** y **Recordset** existentes, así como con los nuevos objetos **Record** y **Stream** .

*Proveedores de datos que admiten enlaces de URL* ADO 2,5 admite proveedores OLE DB que reconozcan los esquemas de dirección URL. Esto incluye OLE DB proveedor para la publicación en Internet, que tiene acceso al sistema de archivos de Windows 2000 y reconoce el esquema HTTP existente.
