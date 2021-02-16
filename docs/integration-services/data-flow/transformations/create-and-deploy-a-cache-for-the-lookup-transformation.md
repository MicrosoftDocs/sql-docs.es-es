---
description: Crear e implementar una memoria caché para la transformación Búsqueda
title: Crear e implementar una memoria caché para la transformación Búsqueda | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- creating cache files for Lookup transformation
- deploying cache files for Lookup transformation
- Lookup transformation cache files
ms.assetid: cedf5cad-2fac-42d0-ad91-9461e117d330
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 426ec69e927a61acfc083f9f4c0462553ea11d97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348288"
---
# <a name="create-and-deploy-a-cache-for-the-lookup-transformation"></a>Crear e implementar una memoria caché para la transformación Búsqueda

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede crear e implementar un archivo caché (.caw) para la transformación Búsqueda. El conjunto de datos de referencia está almacenado en el archivo caché.  
  
 La transformación Búsqueda realiza búsquedas mediante la combinación de datos de las columnas de entrada procedentes de un origen de datos conectado con columnas de un conjunto de datos de referencia.  
  
 Se crea un archivo caché mediante un administrador de conexiones de caché y una transformación de caché. Para obtener más información, consulte [Cache Connection Manager](../../connection-manager/cache-connection-manager.md) y [Cache Transform](../../../integration-services/data-flow/transformations/cache-transform.md).  
  
 Para obtener más información acerca de la transformación Búsqueda y de los archivos caché, vea [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
### <a name="to-create-a-cache-file"></a>Para crear un archivo caché  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] que contiene el paquete con el que desea trabajar y ábralo.  
  
2.  En la pestaña **Flujo de control** , agregue una tarea Flujo de datos.  
  
3.  En la pestaña **Flujo de datos** , agregue una transformación de caché al flujo de datos y, a continuación, conéctela a un origen de datos.  
  
     Configure el origen de datos según sea necesario.  
  
4.  Haga doble clic en la transformación de caché y, después, en el **Editor de transformación Caché**, en la página **Administrador de conexiones** , haga clic en **Nuevo** para crear un administrador de conexiones de caché.  
  
5.  Para guardar la caché, en el **Editor del administrador de conexiones de caché**, en la pestaña **General** , configure el administrador de conexiones de caché mediante las opciones siguientes:  
  
    1.  Seleccione **Utilizar la caché del archivo**.  
  
    2.  Para **Nombre de archivo**, escriba la ruta de acceso del archivo.  
  
     El sistema crea el archivo al ejecutar el paquete.  
  
    > [!NOTE]  
    >  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Haga clic en la pestaña **Columnas** e indique qué columnas son las columnas de índice mediante la opción **Posición de índice** .  
  
     Para las columnas de no índice, la posición de índice es 0. Para las columnas de índice, la posición de índice es un número secuencial positivo.  
  
    > [!NOTE]  
    >  Cuando la transformación Búsqueda se configura para utilizar un administrador de conexiones de caché, a las columnas de entrada solo se les puede asignar las columnas de índice del conjunto de datos de referencia. Asimismo, todas las columnas de índice deben estar asignadas.  
  
     Para obtener más información, vea [Cache Connection Manager Editor](../../connection-manager/cache-connection-manager.md).  
  
7.  Configure la transformación de caché según sea necesario.  
  
     Para obtener más información, vea [Editor de transformación Caché &#40;página Administrador de conexiones&#41;](./cache-transform.md) y [Editor de transformación Caché &#40;página Asignaciones&#41;](./cache-transform.md).  
  
8.  Ejecute el paquete.  
  
### <a name="to-deploy-a-cache-file"></a>Para implementar un archivo caché  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] que contiene el paquete con el que desea trabajar y ábralo.  
  
2.  Opcionalmente, puede crear una configuración de paquetes. Para obtener más información, vea [Crear configuraciones de paquetes](../../packages/legacy-package-deployment-ssis.md).  
  
3.  Haga lo siguiente para agregar el archivo caché al proyecto:  
  
    1.  En el Explorador de soluciones, seleccione el proyecto que abrió en el paso 1.  
  
    2.  En el menú **Proyecto** , haga clic en **Agregar elemento existente**.  
  
    3.  Seleccione el archivo caché y, a continuación, haga clic en **Agregar**.  
  
     El archivo aparece en la carpeta **Varios** del Explorador de soluciones.  
  
4.  Configure el proyecto para crear una utilidad de implementación y, a continuación, genere el proyecto. Para más información, consulte [Create a Deployment Utility](../../packages/legacy-package-deployment-ssis.md).  
  
     Se crea un archivo de manifiesto, \<*project name*>.SSISDeploymentManifest.xml, que enumera los archivos varios del proyecto, los paquetes y las configuraciones del paquete.  
  
5.  Implemente el paquete en el sistema de archivos. Para más información, consulte [Deploy Packages by Using the Deployment Utility](../../packages/legacy-package-deployment-ssis.md).  
  
## <a name="see-also"></a>Consulte también  
 [Crear una utilidad de implementación](../../packages/legacy-package-deployment-ssis.md)  
  
