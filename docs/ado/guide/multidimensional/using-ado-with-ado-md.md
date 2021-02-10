---
description: Uso de ADO con ADO MD
title: Usar ADO con ADO MD | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, using with ADO MD
ms.assetid: cfae435e-2ac3-4312-8c1e-9ca4a74cd875
author: rothja
ms.author: jroth
ms.openlocfilehash: d6844590043e0f806381f35c3f74123c24a6dc30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032167"
---
# <a name="using-ado-with-ado-md"></a>Uso de ADO con ADO MD
ADO y ADO MD están relacionados pero modelos de objetos independientes. ADO proporciona objetos para conectarse a orígenes de datos, ejecutar comandos, recuperar datos tabulares y metadatos de esquema en un formato tabular, y ver la información de error del proveedor. ADO MD proporciona objetos para recuperar datos multidimensionales y ver metadatos de esquema multidimensionales.  
  
 Cuando trabaja con un MDP, puede elegir usar ADO, ADO MD o ambos con su aplicación. Al hacer referencia a ambas bibliotecas dentro del proyecto, tendrá acceso completo a la funcionalidad proporcionada por el MDP.  
  
 A menudo, resulta útil para los consumidores obtener una vista plana y tabular de un conjunto de información multidimensional. Puede hacerlo mediante el objeto de conjunto de [registros](../../reference/ado-api/recordset-object-ado.md) de ADO. Especifique el origen del conjunto de [celdas](../../reference/ado-md-api/cellset-object-ado-md.md) como el parámetro ***source** _ para el método [Open](../../reference/ado-api/open-method-ado-recordset.md) de un conjunto de registros * *, en lugar de como el origen de un conjunto de **celdas** ADO MD.  
  
 También puede ser útil ver los metadatos del esquema en una vista tabular en lugar de como una jerarquía de objetos. El método [OpenSchema](../../reference/ado-api/openschema-method.md) de ADO en el objeto de [conexión](../../reference/ado-api/connection-object-ado.md) permite al usuario abrir un **conjunto de registros** que contiene información del esquema. El **parámetro _QueryType_*_ del método _* OpenSchema** tiene varios valores [SchemaEnum](../../reference/ado-api/schemaenum.md) que se relacionan específicamente con MDPS. Estos valores son los siguientes:  
  
-   **adSchemaCubes**  
  
-   **adSchemaDimensions**  
  
-   **adSchemaHierarchies**  
  
-   **adSchemaLevels**  
  
-   **adSchemaMeasures**  
  
-   **adSchemaMembers**  
  
 Para usar valores de enumeración de ADO con ADO MD propiedades o métodos, el proyecto debe hacer referencia a las bibliotecas de ADO y ADO MD. Por ejemplo, puede usar los valores de enumeración de ADO **adState** con la propiedad ADO MD [State](../../reference/ado-md-api/state-property-ado-md.md) . Para obtener más información acerca de cómo establecer referencias a bibliotecas, consulte la documentación de la herramienta de desarrollo.  
  
 Para obtener más información sobre los objetos y métodos de ADO, vea la referencia de la [API de ADO](../../reference/ado-api/ado-api-reference.md).  
  
## <a name="see-also"></a>Consulte también  
 [Modelo de objetos de ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensional) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Información general de los esquemas y datos multidimensionales](./overview-of-multidimensional-schemas-and-data.md)   
 [Programar con ADO MD](./programming-with-ado-md.md)   
 [Trabajo con datos multidimensionales](./working-with-multidimensional-data.md)