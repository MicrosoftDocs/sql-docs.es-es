---
description: Propiedades dinámicas de ADO
title: Propiedades dinámicas de ADO | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO]
- properties [ADO], dynamic
ms.assetid: d7b06d72-f792-4328-93a2-5006b9e2c581
author: rothja
ms.author: jroth
ms.openlocfilehash: 438b14f2a3fc0ffde38983610a10b1a9f5c46a1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031307"
---
# <a name="ado-dynamic-properties"></a>Propiedades dinámicas de ADO
Se pueden agregar propiedades dinámicas a las colecciones de [propiedades](./properties-collection-ado.md) de los objetos [Connection](./connection-object-ado.md), [Command](./command-object-ado.md)o [Recordset](./recordset-object-ado.md) . El origen de estas propiedades es un proveedor de datos, como el [proveedor de OLE DB para SQL Server](../../guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), o un proveedor de servicios, como [Microsoft Cursor Service para OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md). Consulte la documentación correspondiente del proveedor de datos o del proveedor de servicios para obtener más información sobre una propiedad dinámica específica.  
  
 El [Índice de propiedades dinámicas de ADO](./ado-dynamic-property-index.md) proporciona una referencia cruzada entre los nombres ADO y OLE DB para cada propiedad dinámica de proveedor de OLE DB estándar.  
  
 Las siguientes propiedades dinámicas son especialmente interesantes y también se documentan en los orígenes que se mencionaron anteriormente. En los temas de ayuda de ADO de la lista siguiente se documenta la funcionalidad especial con ADO.  
  
|Propiedad dinámica|Descripción|  
|-|-|  
|[Optimize](./optimize-property-dynamic-ado.md) (Optimizar)|Especifica si se debe crear un índice en este campo.|  
|[Aviso](./prompt-property-dynamic-ado.md)|Especifica si el proveedor de OLE DB debe solicitar al usuario la información de inicialización.|  
|[Nombre de la reformación](./reshape-name-property-dynamic-ado.md)|Especifica un nombre para el objeto de **conjunto de registros** .|  
|[Comando Resync](./resync-command-property-dynamic-ado.md)|Especifica una cadena de comandos proporcionada por el usuario que el método **Resync** emite para actualizar los datos de la tabla denominada en la propiedad dinámica de **tabla única** .|  
|[Tabla única, esquema único, catálogo único](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|**Tabla única** Especifica el nombre de la tabla base en la que se permiten las actualizaciones, las inserciones y las eliminaciones.<br /><br /> **Esquema único** Especifica el esquema o el nombre del propietario de la tabla.<br /><br /> **Catálogo único** Especifica el catálogo o el nombre de la base de datos que contiene la tabla.|  
|[Actualizar resincronización](./update-resync-property-dynamic-ado.md)|Especifica si el método **UpdateBatch** va seguido de una operación de método **Resync** implícita y, en ese caso, el ámbito de esa operación.|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ADO](./ado-api-reference.md)   
 [Colecciones de ADO](./ado-collections.md)   
 [Constantes enumeradas de ADO](./ado-enumerated-constants.md)   
 [Apéndice B: errores de ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventos de ADO](./ado-events.md)   
 [Métodos de ADO](./ado-methods.md)   
 [Modelo de objetos ADO](./ado-object-model.md)   
 [Objetos e interfaces de ADO](./ado-objects-and-interfaces.md)   
 [Propiedades de ADO](./ado-properties.md)