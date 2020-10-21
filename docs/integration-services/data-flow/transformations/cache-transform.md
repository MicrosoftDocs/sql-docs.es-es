---
description: Transformación de caché
title: Transformación de caché | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.cachetrans.f1
- sql13.dts.designer.cachetranscon.f1
- sql13.dts.designer.cachetransmap.f1
helpviewer_keywords:
- Cache transform
ms.assetid: a5683fc8-9c32-4634-819e-e9815627e4f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21f4d8059ee7e0e2f9d466289f4b40f9f51ff950
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194687"
---
# <a name="cache-transform"></a>Transformación de caché

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  La transformación de caché genera un conjunto de datos de referencia para la transformación de búsqueda escribiendo datos de un origen de datos conectado del flujo de datos en un Administrador de conexiones de caché. La transformación Búsqueda realiza búsquedas mediante la combinación de datos de las columnas de entrada procedentes de un origen de datos conectado con columnas de la base de datos de referencia.  
  
 Puede usar el Administrador de conexiones de caché si desea configurar la transformación Búsqueda para que se ejecute en modo de caché completa. En este modo, el conjunto de datos de referencia se carga en la memoria caché antes de que se ejecute la transformación Búsqueda.  
  
 Para obtener instrucciones sobre cómo configurar la transformación Búsqueda en modo de caché completa con el Administrador de conexiones de caché y la transformación de caché, vea [Implementar una transformación de búsqueda en el modo de caché completa mediante el Administrador de conexiones de caché](../../connection-manager/lookup-transformation-full-cache-mode-cache-connection-manager.md).  
  
 Para obtener más información acerca del almacenamiento en memoria caché del conjunto de datos de referencia, vea [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
> [!NOTE]  
>  La transformación de caché solo escribe filas únicas en el administrador de conexiones de caché.  
  
 En un paquete único, solo una transformación de caché puede escribir datos en el mismo administrador de conexiones de caché. Si el paquete contiene varias transformaciones de caché, la primera transformación de caché a la que se llama cuando se ejecuta el paquete escribe los datos en el administrador de conexiones. Las operaciones de escritura de las transformaciones de caché posteriores producirán errores.  
  
 Para obtener más información, vea [Administrador de conexiones de caché](../../connection-manager/cache-connection-manager.md).  
  
## <a name="configuration-of-the-cache-transform"></a>Configuración de la transformación de caché  
 Puede configurar el administrador de conexiones de caché para guardar los datos en un archivo caché (.caw).  
  
 Puede configurar la transformación de caché de las siguientes maneras:  
  
-   Especificar el administrador de conexiones de caché.  
  
-   Asignar columnas de entrada de la transformación de caché a columnas de destino del administrador de conexiones de caché.  
  
    > [!NOTE]  
    >  Cada columna de entrada debe estar asignada a una columna de destino y los tipos de datos de columna deben ser iguales. De lo contrario, el Diseñador [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] muestra un mensaje de error.  
  
     Puede utilizar el **Editor del administrador de conexiones de caché** para modificar tipos de datos y nombres de columna, y otras propiedades de columna.  
  
 Puede establecer propiedades a través del Diseñador [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] . Para obtener más información acerca de las propiedades que se pueden establecer en el cuadro de diálogo **Editor avanzado** , vea [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Para más información sobre cómo establecer propiedades, vea [Establecer las propiedades de un componente de flujo de datos](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="cache-transformation-editor-connection-manager-page"></a>Editor de transformación Caché (página Administrador de conexiones)
  Use la pestaña **Administrador de conexiones** del cuadro de diálogo **Editor de transformación Caché** para seleccionar un administrador de conexiones de caché existente o crear uno nuevo.  
  
 Para obtener más información acerca del administrador de conexiones de caché, vea [Cache Connection Manager](../../connection-manager/cache-connection-manager.md).  
  
### <a name="options"></a>Opciones  
 **Cache connection manager**  
 Seleccione un administrador de conexiones de caché existente usando la lista o cree una conexión mediante el botón **Nueva** .  
  
 **Nuevo**  
 Cree una nueva conexión mediante el cuadro de diálogo Administrador de conexiones de caché.  
  
 **Edición**  
 Modifique una conexión existente.  
  
## <a name="see-also"></a>Consulte también  
 [Transformaciones de Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Flujo de datos](../../../integration-services/data-flow/data-flow.md)  
  
