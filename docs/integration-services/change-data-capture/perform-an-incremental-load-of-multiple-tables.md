---
description: Realizar una carga incremental de varias tablas
title: Realizar una carga incremental de varias tablas | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],multiple tables
ms.assetid: 39252dd5-09c3-46f9-a17b-15208cfd336d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 229809b060a9a0d3103acdb9a562a3dc639b91e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338487"
---
# <a name="perform-an-incremental-load-of-multiple-tables"></a>Realizar una carga incremental de varias tablas

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En el tema [Mejorar las cargas incrementales con la captura de datos modificados](../../integration-services/change-data-capture/change-data-capture-ssis.md), el diagrama muestra un paquete básico que realiza una carga incremental en una sola tabla. Sin embargo, cargar una tabla no es una acción tan común como realizar una carga incremental de varias tablas.  
  
 Cuando se realiza una carga incremental de varias tablas, algunos pasos se tienen que realizar una vez para todas las tablas y otros se tienen que repetir para cada tabla de origen. Tiene varias opciones para implementar estos pasos en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]:  
  
-   Utilizar un paquete primario y varios paquetes secundarios.  
  
-   Utilizar varias tareas Flujo de datos en un único paquete.  
  
## <a name="loading-multiple-tables-by-using-a-parent-package-and-multiple-child-packages"></a>Cargar varias tablas mediante un paquete primario y varios paquetes secundarios  
 Puede usar un paquete primario para realizar aquellos pasos que solo deben seguirse una vez. Los paquetes secundarios realizarán los pasos que se deben efectuar para cada tabla de origen.  
  
#### <a name="to-create-a-parent-package-that-performs-those-steps-that-only-have-to-be-done-once"></a>Para crear un paquete primario que realice los pasos que solo se deben efectuar una vez  
  
1.  Cree un paquete primario.  
  
2.  En el flujo de control, utilice una tarea Ejecutar SQL o expresiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para calcular los extremos.  
  
     Para obtener un ejemplo de cómo calcular los puntos de conexión, vea [Especificar un intervalo de datos modificados](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md).  
  
3.  Si es necesario, utilice un contenedor de bucles For para retrasar la ejecución hasta que los datos modificados para el período seleccionado estén listos.  
  
     Para obtener un ejemplo de un contenedor de bucles For, vea [Determinar si los datos modificados están preparados](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md).  
  
4.  Utilice varias tareas Ejecutar paquete para ejecutar los paquetes secundarios para cada tabla que se va a cargar. Pase los extremos calculados en el paquete primario a cada paquete secundario mediante configuraciones de variable de paquete primario.  
  
     Para más información, vea [Tarea Ejecutar paquete](../../integration-services/control-flow/execute-package-task.md) y [Usar los valores de variables y parámetros en un paquete secundario](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
#### <a name="to-create-child-packages-to-perform-those-steps-that-have-to-be-done-for-each-source-table"></a>Para crear paquetes secundarios que realicen los pasos que se deben efectuar para cada tabla de origen  
  
1.  Cree un paquete secundario para cada tabla de origen.  
  
2.  En el flujo de control, utilice una tarea Script o una tarea Ejecutar SQL para ensamblar la instrucción SQL que se usará para consultar los cambios.  
  
     Para obtener un ejemplo de cómo ensamblar la consulta, vea [Preparar para consultar datos modificados](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md).  
  
3.  Utilice una única tarea Flujo de datos en cada paquete secundario para cargar los datos modificados y aplicarlos al destino. Configure el flujo de datos como se describe en los pasos siguientes:  
  
    1.  En el flujo de datos, utilice un componente de origen para consultar en las tablas de cambios los cambios comprendidos entre los extremos seleccionados.  
  
         Para obtener un ejemplo de cómo consultar las tablas de cambios, vea [Recuperar y describir datos modificados](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
    2.  Use la transformación División condicional para dirigir las inserciones, las actualizaciones y las eliminaciones a salidas diferentes para procesarlas apropiadamente.  
  
         Para obtener un ejemplo de cómo configurar esta transformación para dirigir la salida, vea [Procesar inserciones, actualizaciones y eliminaciones](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md).  
  
    3.  Use un componente de destino para aplicar las inserciones al destino. Use transformaciones Comando de OLE DB con instrucciones UPDATE y DELETE con parámetros para aplicar las actualizaciones y las eliminaciones al destino.  
  
         Para obtener un ejemplo de cómo usar esta transformación para aplicar las actualizaciones y eliminaciones, vea [Aplicar los cambios al destino](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
## <a name="loading-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>Cargar varias tablas mediante varias tareas Flujo de datos en un único paquete  
 También puede utilizar un único paquete que contenga una tarea Flujo de datos independiente para cada tabla de origen que se va a cargar.  
  
#### <a name="to-load-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>Para cargar varias tablas mediante varias tareas Flujo de datos en un único paquete  
  
1.  Cree un único paquete.  
  
2.  En el flujo de control, utilice una tarea Ejecutar SQL o expresiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para calcular los extremos.  
  
     Para obtener un ejemplo de cómo calcular los puntos de conexión, vea [Especificar un intervalo de datos modificados](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md).  
  
3.  Si es necesario, utilice un contenedor de bucles For para retrasar la ejecución hasta que estén listos los datos modificados para el intervalo seleccionado.  
  
     Para obtener un ejemplo de un contenedor de bucles For, vea [Determinar si los datos modificados están preparados](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md).  
  
4.  Use una tarea Script o Ejecutar SQL para ensamblar la instrucción SQL que se usará para consultar si hay cambios.  
  
     Para obtener un ejemplo de cómo ensamblar la consulta, vea [Preparar para consultar datos modificados](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md).  
  
5.  Utilice varias tareas Flujo de datos para cargar los datos modificados de cada tabla de origen y aplicarlos al destino. Configure cada tarea Flujo de datos tal y como se describe en los pasos siguientes.  
  
    1.  En cada flujo de datos, utilice un componente de origen para consultar en las tablas de cambios los cambios comprendidos entre los extremos seleccionados.  
  
         Para obtener un ejemplo de cómo consultar las tablas de cambios, vea [Recuperar y describir datos modificados](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
    2.  Use la transformación División condicional para dirigir las inserciones, las actualizaciones y las eliminaciones a salidas diferentes para procesarlas apropiadamente.  
  
         Para obtener un ejemplo de cómo configurar esta transformación para dirigir la salida, vea [Procesar inserciones, actualizaciones y eliminaciones](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md).  
  
    3.  Use un componente de destino para aplicar las inserciones al destino. Use transformaciones Comando de OLE DB con instrucciones UPDATE y DELETE con parámetros para aplicar las actualizaciones y las eliminaciones al destino.  
  
         Para obtener un ejemplo de cómo usar esta transformación para aplicar las actualizaciones y eliminaciones, vea [Aplicar los cambios al destino](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
  
