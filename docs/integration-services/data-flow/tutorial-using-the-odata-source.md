---
description: 'Tutorial: Usar el origen OData'
title: 'Tutorial: Usar el origen OData | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2c64cf8b-5edb-48df-8ffe-697096258f71
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b0bb777c05b30e4b794fc889dc6322e24c4dd5ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339728"
---
# <a name="tutorial-using-the-odata-source"></a>Tutorial: Usar el origen OData

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En este tutorial se describe el proceso para extraer la colección **Employees** del servicio OData del ejemplo **Northwind** (https://services.odata.org/V3/Northwind/Northwind.svc/)) y, después, cargarlo en un archivo plano.  
  
## <a name="1-create-an-integration-services-project"></a>1. Crear un proyecto de Integration Services  
  
1.  Inicie **SQL Server Data Tools** o [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
2.  Haga clic en **Archivo**, seleccione **Nuevo** y, luego, haga clic en **Proyecto**.  
  
3.  En el cuadro de diálogo **Nuevo proyecto** , expanda **Instalado**, expanda **Plantillas**, expanda **Business Intelligence** y haga clic en **Integration Services**.  
  
4.  Seleccione **Proyecto de Integration Services** como tipo de proyecto.  
  
5.  Escriba un **nombre** y seleccione una **ubicación** para el proyecto; a continuación, haga clic en **Aceptar**.  
  
## <a name="2-add-and-configure-an-odata-source"></a>2. Agregar y configurar un origen de OData 
  
1.  Arrastre y coloque una **Tarea Flujo de datos** desde el **Cuadro de herramientas de SSIS** hasta la superficie de diseño del flujo de control del paquete SSIS.  
  
2.  Haga clic en la pestaña **Flujo de datos** o haga doble clic en la **Tarea Flujo de datos** para abrir la superficie de diseño Flujo de datos.  
  
3.  Arrastre y coloque **Origen OData** desde el grupo **Común** del **Cuadro de herramientas de SSIS**.
  
4.  Haga doble clic en el componente **Origen OData** para iniciar el cuadro de diálogo **Editor de origen OData**.  
  
5.  Haga clic en **Nuevo…** para agregar un nuevo Administrador de conexiones OData.  
  
6.  Escriba la dirección URL del servicio OData en **Ubicación de documento de servicio**. Esta URL puede ser la dirección URL del documento de servicio, o de una fuente o entidad específica. Para este tutorial, escriba la dirección URL en el documento de servicio: [https://services.odata.org/V3/Northwind/Northwind.svc/](https://services.odata.org/V3/Northwind/Northwind.svc/).  
  
7.  Confirme que se ha seleccionado **Autenticación de Windows** como la **autenticación** que se usará para tener acceso al servicio OData. **Autenticación de Windows** está seleccionada de forma predeterminada.  
  
8.  Haga clic en **Probar conexión** para probar la conexión y, después, haga clic en **Aceptar** para terminar de crear una instancia del Administrador de conexiones OData.  
  
9. En el cuadro de diálogo **Editor de origen OData** , confirme que se ha seleccionado **Colección** para la opción **Usar colección en la ruta de acceso a recursos** .  
  
10. En la lista desplegable **Colección**, seleccione **Empleados**.  
  
11. Especifique cualquier otra opción o filtro de consulta adicional de OData en **Opciones de consulta**. Por ejemplo, `$orderby=CompanyName&$top=100`. Para este tutorial, escriba `$top=5`.  
  
12. Haga clic en **Vista previa** para obtener una vista previa de los datos.  
  
13. Haga clic en **Columnas** en el panel de navegación de la izquierda para cambiar a la página **Columnas** .  
  
14. Seleccione **EmployeeID**, **FirstName** y **LastName** en **Columnas externas disponibles** activando las casillas correspondientes.  
  
15. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Editor de origen OData** .  
  
## <a name="3-add-and-configure-a-flat-file-destination"></a>3. Agregar y configurar un destino de archivo plano
  
1.  Después, arrastre y coloque un **Destino de archivo plano** desde el **Cuadro de herramientas de SSIS** hasta la superficie de diseño Flujo de datos, debajo el componente **Origen OData** .  
  
2.  Conecte el componente **Origen OData** con el componente **Destino de archivo plano** usando la flecha azul.  
  
3.  Haga doble clic en **Destino de archivo plano**. Debe ver el cuadro de diálogo **Editor de destino de archivos planos** .  
  
4.  En el cuadro de diálogo **Editor de destino de archivos planos** , haga clic en **Nuevo** para crear un nuevo administrador de conexiones de archivos planos.  
  
5.  En el cuadro de diálogo **Formato del archivo plano** , seleccione **Delimitado**. A continuación, verá el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**.  
  
6.  En el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**, en **Nombre de archivo**, escriba `c:\Employees.txt`.  
  
7.  En el panel de navegación de la izquierda, haga clic en **Columnas**. Puede obtener una vista previa de los datos de esta página.  
  
8.  Haga clic en Aceptar para cerrar el cuadro de diálogo **Editor del administrador de conexiones de archivos planos** .  
  
9. En el cuadro de diálogo **Editor de destino de archivos planos** , haga clic en **Asignaciones** en el panel de navegación de la izquierda. Revise las asignaciones.  
  
10. Haga clic en Aceptar para cerrar el cuadro de diálogo **Editor de destino de archivos planos** .  

## <a name="4-run-the-package"></a>4. Ejecutar el paquete
Ejecute el paquete SSIS. Compruebe que el archivo de salida creado contiene el identificador, el nombre y los apellidos de cinco empleados de la fuente OData.
  
  
