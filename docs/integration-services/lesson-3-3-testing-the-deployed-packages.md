---
description: 'Lección 3-3: Probar los paquetes implementados'
title: 'Paso 3: Probar los paquetes implementados | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9159da3f-c9ca-4015-9e85-3bf4373a1349
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 96a06d721c1ccbb4ce254705ceb676c1c338b68a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354933"
---
# <a name="lesson-3-3---testing-the-deployed-packages"></a>Lección 3-3: Probar los paquetes implementados

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta tarea probará los paquetes que ha implementado en una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
En otros tutoriales de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , ejecutó paquetes en [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], el entorno de desarrollo de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], con la opción **Iniciar depuración** del menú **Depurar** . Esta vez ejecutará los paquetes de otra forma.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] proporciona varias herramientas que puede usar para ejecutar paquetes en el entorno de prueba y producción: la utilidad del símbolo del sistema **dtexec** y la utilidad de ejecución de paquetes. La Utilidad de ejecución de paquetes es una herramienta gráfica integrada en **dtexec**. Las dos herramientas ejecutan el paquete de forma inmediata. Además, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] proporciona un subsistema del Agente SQL Server que está diseñado especialmente para programar la ejecución de paquetes como un paso del trabajo del Agente SQL Server.  
  
Utilizará la Utilidad de ejecución de paquetes para ejecutar los paquetes implementados. Los paquetes se utilizarán tal como están; por tanto, no tiene que actualizar información en ninguna página del cuadro de diálogo. Ejecutará los paquetes desde la página General, que es la primera página de la Utilidad de ejecución de paquetes. Si lo desea, puede hacer clic en otras páginas para ver la información que contienen para cada paquete.  
  
> [!NOTE]  
> Para asegurarse de que los paquetes funcionan correctamente en el contexto de este tutorial, no debe modificar ninguna opción.  
  
Antes de ejecutar paquetes en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] con la Utilidad de ejecución de paquetes, asegúrese de que se está ejecutando el servicio Integration Services. El servicio Integration Services proporciona compatibilidad para la ejecución y el almacenamiento de paquetes. Si el servicio se detiene, no puede conectarse a [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] y [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] no enumera los paquetes que se van a ejecutar. También debe tener permisos para ejecutar el paquete en la instancia donde se haya implementado el paquete. Para obtener más información, vea [Roles de Integration Services &#40;servicio SSIS&#41;](../integration-services/security/integration-services-roles-ssis-service.md).  
  
Las carpetas de nivel superior dentro de la carpeta Paquetes almacenados son carpetas definidas por el usuario que supervisa el servicio Integration Services. En el archivo MsDtsSrvr.ini.xml puede especificar tantas carpetas como desee. En este tutorial se supone que va a utilizar el archivo MsDtsSrvr.ini.xml predeterminado y que los nombres de las carpetas de nivel superior dentro de Paquetes almacenados son Sistema de archivos y MSDB.  
  
### <a name="to-connect-to-integration-services-in-sql-server-management-studio"></a>Para conectar a Integration Services en SQL Server Management Studio  
  
1.  Haga clic en **Inicio**, elija **Todos los programas**, **Microsoft SQL Server** y, a continuación, haga clic en **SQL Server Management Studio**.  
  
2.  En el cuadro de diálogo **Conectar con el servidor** , seleccione **Integration Services** en la lista **Tipo de servidor** , proporcione un nombre de servidor en el cuadro **Nombre del servidor** y haga clic en **Conectar**.  
  
    > [!IMPORTANT]  
    > Si no puede conectarse a [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], es probable que no se esté ejecutando el servicio [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Para conocer el estado del servicio, haga clic en **Inicio**, elija sucesivamente **Todos los programas**, **Microsoft SQL Server**, **Herramientas de configuración**, y haga clic en **Administrador de configuración de SQL Server**. En el panel izquierdo, haga clic en **Servicios de SQL Server**. En el panel derecho, busque el servicio [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Inicie el servicio, si no se está ejecutando todavía.  
  
    [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] . De forma predeterminada, se abre la ventana Explorador de objetos y se coloca en la esquina superior derecha del estudio. Si el Explorador de objetos no está abierto, haga clic en **Explorador de objetos** en el menú **Ver** .  
  
### <a name="to-run-the-packages-using-the-execute-package-utility"></a>Para ejecutar los paquetes con la Utilidad de ejecución de paquetes  
  
1.  En el Explorador de objetos, expanda la carpeta Paquetes almacenados.  
  
2.  Expanda la carpeta MSDB. Puesto que ha implementado los paquetes en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], todos los paquetes implementados se almacenan en la base de datos msdb de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y todos ellos aparecen en la carpeta MSDB. La carpeta Sistema de archivos está vacía, a menos que haya implementado paquetes en el sistema de archivos fuera de Deployment Tutorial.  
  
3.  Empezando en la parte superior de la lista de paquetes, haga clic con el botón derecho en DataTransfer y haga clic en **Ejecutar paquete**.  
  
4.  En el cuadro de diálogo **Utilidad de ejecución de paquetes** , haga clic en **Ejecutar**.  
  
5.  En el cuadro de diálogo **Utilidad de ejecución de paquetes** , vea el progreso y los resultados de la ejecución del paquete. Cuando el botón **Detener** no esté disponible, lo que indica que el paquete ha finalizado, haga clic en **Cerrar**.  
  
    > [!IMPORTANT]  
    > Si hace clic en **Detener** mientras el paquete se está ejecutando, el paquete no finalizará.  
  
6.  En el cuadro de diálogo **Utilidad de ejecución de paquetes** , haga clic en **Cerrar**.  
  
7.  Repita los pasos 3 a 6 con el paquete LoadXML.  
  
8.  En el menú **Archivo** , haga clic en **Salir**.  
  
### <a name="to-verify-the-results-of-the-datatransfer-package"></a>Para comprobar los resultados del paquete DataTransfer  
  
1.  En la barra de herramientas de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], haga clic en **Nueva consulta**.  
  
2.  En el cuadro de diálogo **Conectar al servidor** , seleccione **Motor de base de datos** en la lista **Tipo de servidor** , proporcione el nombre de servidor en el que ha instalado los paquetes del tutorial o escriba (local) en el cuadro **Nombre del servidor** y seleccione un modo de autenticación. Si utiliza Autenticación de SQL Server, proporcione un nombre de usuario y una contraseña.  
  
3.  Haga clic en **Conectar**.  
  
4.  En la ventana de consultas, escriba o pegue la siguiente instrucción SQL:  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM HighIncomeCustomers`  
  
5.  Presione **F5** o haga clic en el icono Ejecutar en la barra de herramientas.  
  
    La consulta devuelve 31 filas de datos. El resultado devuelto contiene filas del archivo de texto, Customers.txt, que tienen valores mayores que 100000 en la columna YearlyIncome.  
  
6.  Busque la carpeta DeploymentTutorial, haga clic con el botón derecho en el archivo XML de registro, Deployment Tutorial Log y, después, haga clic en **Abrir**. Puede abrir el archivo con el Bloc de notas o el editor de texto o XML que prefiera.  
  
### <a name="to-verify-the-results-of-the-loadxmldata-package"></a>Para comprobar los resultados del paquete LoadXMLData  
  
1.  En la barra de herramientas de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], haga clic en **Nueva consulta**.  
  
2.  Si se le pide que se conecte de nuevo, en el cuadro de diálogo **Conectar al servidor** , seleccione **Motor de base de datos** en la lista **Tipo de servidor** , proporcione el nombre de servidor en el que ha instalado los paquetes del tutorial o escriba (local) en el cuadro **Nombre del servidor** y seleccione un modo de autenticación. Si utiliza Autenticación de SQL Server, proporcione un nombre de usuario y una contraseña.  
  
3.  Haga clic en **Conectar**.  
  
4.  En la ventana de consultas, escriba o pegue la siguiente instrucción SQL:  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM OrderDatesByCountryRegion`  
  
5.  Presione **F5** o haga clic en el icono Ejecutar en la barra de herramientas.  
  
    La consulta devuelve 21 filas de datos. El resultado devuelto consta de filas del archivo de datos XML, orders.xml. Cada fila es un resumen por país y región; la fila presenta el nombre de un país o región, el número de pedidos de cada país o región, y las fechas del pedido más reciente y más antiguo.  
  
## <a name="see-also"></a>Consulte también  
[dtexec (utilidad)](../integration-services/packages/dtexec-utility.md)  
  
  
  
