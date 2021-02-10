---
description: 'Apéndice A: proveedores de datos y servicios'
title: 'Apéndice A: proveedores | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data providers [ADO]
- providers [ADO]
- ADO, providers
- service providers [ADO]
- service components [ADO]
ms.assetid: e2581b47-b11e-4e1e-b96c-d39c77c5b48a
author: rothja
ms.author: jroth
ms.openlocfilehash: a49518c1fcc1618884751d503def46a29dd9c048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029489"
---
# <a name="appendix-a-data-and-service-providers"></a>Apéndice A: proveedores de datos y servicios
En esta sección se tratan tres tipos de proveedores: proveedores de datos, proveedores de servicios y componentes de servicio. Los proveedores se dividen en dos categorías: aquellas que proporcionan datos y que proporcionan servicios. Un *proveedor de datos* posee sus propios datos y los expone en formato tabular a la aplicación. Un *proveedor de servicios* encapsula un servicio mediante la generación y el consumo de datos, lo que aumenta las características de las aplicaciones de ADO. También se puede definir un proveedor de servicios como *componente de servicio*, que debe funcionar junto con otros proveedores de servicios o componentes.

## <a name="data-providers"></a>Proveedores de datos
 ADO es eficaz y flexible porque puede conectarse a cualquiera de los distintos proveedores de datos y seguir exponiendo el mismo modelo de programación, independientemente de las características específicas de cualquier proveedor determinado.

 Sin embargo, dado que cada proveedor de datos es único, el modo en que la aplicación interactúa con ADO variará ligeramente en función del proveedor de datos. Las diferencias suelen corresponder a una de estas tres categorías:

-   Parámetros de conexión en la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) .

-   Uso del objeto de [comando](../../reference/ado-api/command-object-ado.md) .

-   Comportamiento del [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) específico del proveedor.

 Los detalles de cada uno de los proveedores de datos disponibles actualmente en Microsoft se muestran como se indica a continuación.

|Área|Tema|
|----------|-----------|
|Bases de datos ODBC|[Proveedor Microsoft OLE DB para ODBC](./microsoft-ole-db-provider-for-odbc.md)|
|Servicio de Index Server de Microsoft|[Proveedor Microsoft OLE DB para el Servicio de Microsoft Index Server](./microsoft-ole-db-provider-for-microsoft-indexing-service.md)|
|Active Directory Service|[Proveedor de Microsoft OLE DB para el servicio Microsoft Active Directory](./microsoft-ole-db-provider-for-microsoft-active-directory-service.md)|
|Bases de datos de Microsoft Jet|[Proveedor de OLE DB para Microsoft Jet](./microsoft-ole-db-provider-for-microsoft-jet.md)|
|Microsoft SQL Server|[Proveedor OLE DB de Microsoft para SQL Server](./microsoft-ole-db-provider-for-sql-server.md)|
|bases de datos Oracle|[proveedor Microsoft OLE DB para Oracle](./microsoft-ole-db-provider-for-oracle.md)|
|Publicación en Internet|[Proveedor de Microsoft OLE DB para la publicación en Internet](./microsoft-ole-db-provider-for-internet-publishing.md)|
|Orígenes de datos simples|[Proveedor simple de Microsoft OLE DB](./microsoft-ole-db-simple-provider.md)|

## <a name="provider-specific-dynamic-properties"></a>Provider-Specific propiedades dinámicas
 Las colecciones de [propiedades](../../reference/ado-api/properties-collection-ado.md) de los objetos [Connection](../../reference/ado-api/connection-object-ado.md), [Command](../../reference/ado-api/command-object-ado.md)y [Recordset](../../reference/ado-api/recordset-object-ado.md) incluyen propiedades dinámicas específicas del proveedor. Estas propiedades proporcionan información sobre la funcionalidad específica del proveedor más allá de las propiedades integradas que admite ADO.

 Después de establecer la conexión y crear estos objetos, utilice el método [Refresh](../../reference/ado-api/refresh-method-ado.md) en la colección **Properties** del objeto para obtener las propiedades específicas del proveedor. Consulte la documentación del proveedor y la [Guía del programador de OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)) para obtener información detallada acerca de estas propiedades dinámicas.

## <a name="service-providers"></a>Proveedores de servicios
 Para usar un proveedor de servicios, debe proporcionar una palabra clave. También debe tener en cuenta las propiedades dinámicas específicas del proveedor asociadas a cada proveedor de servicios. Los detalles específicos del proveedor se enumeran para cada proveedor de servicios que está disponible actualmente en Microsoft:

-   [Datos de Microsoft para dar forma al servicio para información general acerca OLE DB](./microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)

-   [Proveedor de persistencia de Microsoft OLE DB](./microsoft-ole-db-persistence-provider-ado-service-provider.md)

-   [Proveedor de servicios remotos de Microsoft OLE DB](./microsoft-ole-db-remoting-provider-ado-service-provider.md)

## <a name="service-components"></a>Componentes del servicio
 El [servicio de cursor para OLE DB](./microsoft-cursor-service-for-ole-db-ado-service-component.md) componente de servicio complementa las funciones de compatibilidad de cursores de los proveedores de datos. También requiere una palabra clave y tiene propiedades dinámicas.

 Para obtener más información acerca de los proveedores de OLE DB, vea [OLE DB de Microsoft](/previous-versions/windows/desktop/ms722784(v=vs.85)).

## <a name="provider-commands"></a>Comandos de proveedor
 Para cada proveedor que se muestra aquí, si las aplicaciones permiten a los usuarios especificar instrucciones SQL como comandos de proveedor, siempre debe validar los datos proporcionados por el usuario y estar atento a posibles ataques de hacker mediante instrucciones SQL potencialmente peligrosas, como `DROP TABLE t1` , como parte de la entrada del usuario.

## <a name="see-also"></a>Consulte también
 [Objeto de conexión (ADO](../../reference/ado-api/command-object-ado.md) ) [objeto de conexión (ADO)](../../reference/ado-api/connection-object-ado.md) [proveedor de Microsoft OLE DB para la publicación en Internet](./microsoft-ole-db-provider-for-internet-publishing.md) proveedor de [microsoft OLE DB para Microsoft Active Directory Service](./microsoft-ole-db-provider-for-microsoft-active-directory-service.md) [Microsoft OLE DB Provider para el servicio de microsoft Index Server](./microsoft-ole-db-provider-for-microsoft-indexing-service.md) [Microsoft OLE DB provider para ODBC](./microsoft-ole-db-provider-for-odbc.md) [proveedor OLE DB de Microsoft para Oracle](./microsoft-ole-db-provider-for-oracle.md) [proveedor de OLE DB de Microsoft para SQL Server](./microsoft-ole-db-provider-for-sql-server.md) proveedor de Microsoft OLE DB para la colección de propiedades [de Microsoft Jet](./microsoft-ole-db-provider-for-microsoft-jet.md) [(](../../reference/ado-api/properties-collection-ado.md) [ADO)](../../reference/ado-api/recordset-object-ado.md) [método Refresh (RDS)](../../reference/rds-api/refresh-method-rds.md)