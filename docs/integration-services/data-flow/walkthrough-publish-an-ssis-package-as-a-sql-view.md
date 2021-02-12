---
description: 'Tutorial: Publicación de un paquete SSIS como una vista SQL'
title: 'Tutorial: Publicar un paquete SSIS como una vista SQL | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.packagepublishwizard.f1
ms.assetid: d32d9761-93fb-4020-bf82-231439c6f3ac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0a9c522bf292147852203fb9ac740e7103195b35
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99836000"
---
# <a name="walkthrough-publish-an-ssis-package-as-a-sql-view"></a>Tutorial: Publicar un paquete SSIS como una vista SQL

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En este tutorial se explica detalladamente cómo publicar un paquete SSIS como una vista SQL en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="prerequisites"></a>Prerequisites  
 Para realizar este tutorial, debe tener instalado el siguiente software en el equipo:  
  
1.  [!INCLUDE[ssNoVersion ](../../includes/ssnoversion-md.md)] con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
2.  [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).  
  
## <a name="step-1-build-and-deploy-ssis-project-to-the-ssis-catalog"></a>Paso 1: Compilar e implementar un proyecto de SSIS en el catálogo de SSIS  
 En este paso, creará un paquete SSIS que extrae datos de un origen de datos compatible con SSIS (en este ejemplo, usaremos una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) y genera datos de salida con un componente de Destino de streaming de datos. Luego, compilará e implementará el proyecto de SSIS en el catálogo de SSIS.  
  
1.  Inicie **SQL Server Data Tools**. En el menú **Inicio** , elija **Todos los programas**, **Microsoft SQL Server** y, a continuación, haga clic en **SQL Server Data Tools**.  
  
2.  Cree un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
    1.  En la barra de menús, haga clic en **Archivo** , elija **Nuevo** y, después, haga clic en **Proyecto**.  
  
    2.  Expanda **Business Intelligence** en el panel de la izquierda y haga clic en **Integration Services** en la vista de árbol.  
  
    3.  Seleccione **Proyecto de Integration Services** si aún no está seleccionado.  
  
    4.  Escriba **SSISPackagePublishing** como **nombre del proyecto**.  
  
    5.  Indique una ubicación donde guardar el proyecto.  
  
    6.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Nuevo proyecto** .  
  
3.  Arrastre el componente **Flujo de datos** desde el **cuadro de herramientas de SSIS** a la superficie de diseño de la pestaña **Flujo de control** .  
  
4.  Haga doble clic en el componente **Flujo de datos** en **Flujo de control** para abrir el **diseñador de flujos de datos**.  
  
5.  Arrastre un **componente de origen** desde el cuadro de herramientas al **diseñador de flujos de datos** y configúrelo para que extraiga datos de un origen de datos.  
  
    1.  En este tutorial crearemos una base de datos de prueba, **TestDB** , con una tabla, **Employee**. Cree la tabla con tres columnas: **ID**, **FirstName** y **LastName**.  
  
    2.  Establezca **ID** como clave principal.  
  
    3.  Inserte dos registros con los siguientes datos.  
  
        |id|FIRSTNAME|LASTNAME|  
        |--------|---------------|--------------|  
        |1|John|Doe|  
        |2|Julia|Doe|  
  
    4.  Arrastre el componente **Origen de OLE DB** desde el **cuadro de herramientas de SSIS** hasta el **diseñador de flujos de datos**.  
  
    5.  Configure el componente para que extraiga datos de la tabla **Employee** de la base de datos **TestDB** . Seleccione **(local).TestDB** en **Administrador de conexiones OLE DB**, **Tabla o vista** en **Modo de acceso a datos** y **[dbo].[Employee]** en **Nombre de la tabla o la vista**.  
  
         ![Destino de streaming de datos: Conexión OLE DB](../../integration-services/data-flow/media/dsd-oledbconnectionmanager.jpg "Destino de streaming de datos: Conexión OLE DB")  
  
6.  Ahora, arrastre el **Destino de streaming de datos** desde el cuadro de herramientas al flujo de datos. Este componente debería estar en la sección Común del cuadro de herramientas.  
  
7.  Conecte el componente **Origen de OLE DB** del flujo de datos al componente **Destino de streaming de datos** .  
  
8.  Compile e implemente el proyecto de SSIS en el catálogo de SSIS.  
  
    1.  Haga clic en **Proyecto** en la barra de menús y, después, haga clic en **Implementar**.  
  
    2.  Siga las instrucciones del asistente para implementar el proyecto en el catálogo de SSIS en el servidor de base de datos local. En el siguiente ejemplo, se usa **Power BI** como nombre de carpeta y **SSISPackagePublishing** como nombre del proyecto en el catálogo de SSIS.  
  
## <a name="step-2-use-the-ssis-data-feed-publishing-wizard-to-publish-ssis-package-as-a-sql-view"></a>Paso 2: Usar el asistente para la publicación de fuentes de distribución de datos de SSIS para publicar el paquete SSIS como una vista SQL  
 En este paso, usará el asistente para la publicación de fuentes de distribución de datos de SQL Server Integration Services (SSIS ) para publicar el paquete SSIS como una vista en la base de datos de SQL Server. Los datos de salida del paquete se podrán usar realizando una consulta en esta vista.  
  
 El asistente para la publicación de fuentes de distribución de datos de SSIS crea un servidor vinculado mediante el proveedor OLE DB para SSIS (SSISOLEDB) y, tras ello, crea una vista SQL que consta de una consulta en el servidor vinculado. Esta consulta incluye el nombre de la carpeta, el nombre del proyecto y el nombre del paquete en el catálogo de SSIS.  
  
 En el tiempo de ejecución, la vista envía la consulta al proveedor OLE DB para SSIS a través del servidor vinculado creado. El proveedor OLE DB para SSIS ejecuta el paquete especificado en la consulta y devuelve el conjunto de resultados tabulares a la consulta.  
  
1.  Para iniciar el **Asistente para la publicación de fuentes de distribución de datos de SSIS** , ejecute ISDataFeedPublishingWizard.exe desde C:\Archivos de programa\Microsoft SQL Server\130\DTS\Binn o haga clic en Microsoft SQL Server 2016\SQL Server 2016 Data Feed Publishing Wizard en Inicio\Todos los programas.  
  
2.  Haga clic en **Siguiente** en la página **Introducción** .  
  
     ![Asistente para publicar fuentes de distribución de datos: página Introducción](../../integration-services/data-flow/media/dsd-feedpublishingwizard-introductionpage.jpg "Asistente para publicar fuentes de distribución de datos: página Introducción")  
  
3.  En la página **Configuración del paquete** , haga lo siguiente:  
  
    1.  Escriba el **nombre** de la instancia de SQL Server que contiene el catálogo de SSIS o haga clic en **Examinar** para seleccionar el servidor.  
  
         ![Asistente para publicar fuentes de distribución de datos: página Configuración del paquete](../../integration-services/data-flow/media/dsd-feedpublishingwizard-packagesettingspage.jpg "Asistente para publicar fuentes de distribución de datos: página Configuración del paquete")  
  
    2.  Haga clic en **Examinar** en el campo Ruta de acceso, vaya al catálogo de SSIS, seleccione el paquete SSIS que quiera publicar (por ejemplo, **SSISDB**->**SSISPackagePublishing**->**Package.dtsx**) y haga clic en **Aceptar**.  
  
         ![Asistente para publicar fuentes de distribución de datos: Buscar paquete](../../integration-services/data-flow/media/dsd-feedpublishingwizard-browseforpackage.jpg "Asistente para publicar fuentes de distribución de datos: Buscar paquete")  
  
    3.  En las pestañas Parámetros de paquete, Parámetros de proyecto y Administradores de conexiones al final de la página, escriba los valores de configuración de los parámetros de paquete, los parámetros de proyecto o los administradores de conexiones relativos al paquete. También puede indicar que se use un entorno de referencia para ejecutar el paquete y enlazar los parámetros de paquete y de proyecto a variables de entorno.  
  
         Se aconseja enlazar los parámetros confidenciales a variables de entorno, ya que esto evita que el valor de un parámetro confidencial se almacene como texto sin formato en la vista SQL creada por el asistente.  
  
    4.  Haga clic en **Siguiente** para pasar a la página **Configuración de publicación** .  
  
4.  En la página **Configuración de publicación** , haga lo siguiente:  
  
    1.  Seleccione la **base de datos** relativa a la vista que se va a crear.  
  
         ![Asistente para publicar fuentes de distribución de datos: página Configuración de publicación](../../integration-services/data-flow/media/dsd-feedpublishingwizard-publishsettingspage.jpg "Asistente para publicar fuentes de distribución de datos: página Configuración de publicación")  
  
    2.  Escriba un **nombre** para la **vista**. También puede seleccionar una vista existente de la lista desplegable.  
  
    3.  En la lista **Configuración** , especifique el **nombre** del **servidor vinculado** que se va a asociar a la vista. Si el servidor vinculado aún no existe, el asistente lo creará antes de crear la vista. Aquí también puede establecer los valores de **User32BitRuntime** y **Timeout** .  
  
    4.  Haga clic en el botón **Avanzadas** . Debería abrirse el cuadro de diálogo **Configuración avanzada** .  
  
    5.  Haga lo siguiente en el cuadro de diálogo **Configuración avanzada**:  
  
        1.  Especifique el esquema de base de datos en el que quiere crear la vista (campo Esquema).  
  
        2.  Especifique si los datos se van a cifrar antes de enviarlos a través de la red (campo Cifrar). Consulte el tema [Usar el cifrado sin validación](../../relational-databases/native-client/features/using-encryption-without-validation.md) para ver más detalles sobre esta configuración y la configuración de TrustServerCertificate.  
  
        3.  Especifique si se puede usar un certificado de servidor autofirmado cuando la opción de cifrado esté habilitada (campo **TrustServerCertificate** ).  
  
        4.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Configuración avanzada** .  
  
    6.  Haga clic en **Siguiente** para pasar a la página **Validación** .  
  
5.  En la página **Validación** , revise los resultados correspondientes a la validación de los valores de todas las configuraciones. En el siguiente ejemplo, se abre una **advertencia** sobre la existencia de un servidor vinculado, ya que no hay un servidor vinculado en la instancia de SQL Server seleccionada. Si ve **Error** en **Resultado**, mantenga el puntero sobre **Error** para ver más detalles al respecto. Por ejemplo, si no se habilitó la opción Permitir InProcess del proveedor SSISOLEDB, obtendrá un error en la acción de configuración del servidor vinculado.  
  
     ![Asistente para publicar fuentes de distribución de datos: página Validación](../../integration-services/data-flow/media/dsd-feedpublishingwizard-validationpage.jpg "Asistente para publicar fuentes de distribución de datos: página Validación")  
  
6.  Haga clic en Guardar informe para guardar el informe como un archivo XML.  
  
7.  Haga clic en **Siguiente** en la página **Validación** para pasar a la página **Resumen** .  
  
8.  Revise la selección en la página **Resumen** y haga clic en **Publicar** para iniciar el proceso de publicación, en el que se creará el servidor vinculado (si aún no existe en el servidor) con el que, luego, se creará la vista.  
  
     ![Asistente para publicar fuentes de distribución de datos: página Resumen](../../integration-services/data-flow/media/dsd-feedpublishingwizard-summarypage.jpg "Asistente para publicar fuentes de distribución de datos: página Resumen")  
  
     Ahora los datos de salida del paquete se pueden consultar ejecutando la siguiente instrucción SQL en la base de datos TestDB: SELECT * FROM [SSISPackageView].  
  
9. Haga clic en **Guardar informe** para guardar el informe como un archivo XML.  
  
10. Revise los resultados del proceso de publicación y haga clic en **Finalizar** para cerrar el asistente.  
  
    > [!NOTE]  
    >  No se admiten los siguientes tipos de datos: text, ntext, image, nvarchar(max), varchar(max) ni varbinary(max).  
  
## <a name="step-3-test-the-sql-view"></a>Paso 3: Probar la vista SQL  
 Aquí ejecutará la vista SQL creada en el asistente para la publicación anterior.  
  
1.  Inicie SQL Server Management Studio.  
  
2.  Expanda \<**machine name**>, **Bases de datos**, \<**database you selected in the wizard**> y **Vistas**.  
  
3.  Haga clic con el botón derecho en el elemento \<**view created by the wizard**> creado por el asistente y haga clic en **Seleccionar las primeras 1000 filas**.  
  
4.  Confirme que ve los resultados del paquete SSIS.  
  
## <a name="step-4-verify-the-ssis-package-execution"></a>Paso 4: Comprobar la ejecución del paquete SSIS  
 En este paso comprobará que el paquete SSIS se ha ejecutado.  
  
1.  En SQL Server Management Studio, expanda **Catálogos de Integration Services**, **SSISDB** y la **carpeta** en la que está el proyecto de SSIS. Después, expanda **Proyectos**, luego el nodo del proyecto y, por último, **Paquetes**.  
  
2.  Haga clic con el botón derecho en el paquete SSIS, seleccione **Informes** e **Informes estándar** y, después, haga clic en **Todas las ejecuciones**.  
  
3.  La ejecución del paquete SSIS debería aparecer reflejada en el informe.  
  
    > [!NOTE]  
    >  En un equipo con Windows Vista Service Pack 2, es posible que el informe contenga dos ejecuciones de paquete SSIS, una correcta y otra con errores. Omita esta última, ya que se debe a un problema conocido de esta versión.  
  
## <a name="more-info"></a>Más información  
 El asistente para la publicación de fuentes de distribución de datos realiza los siguientes pasos importantes:  
  
1.  Crea un servidor vinculado y lo configura para que use el proveedor OLE DB para SSIS.  
  
2.  Crea una vista SQL en la base de datos especificada, que consulta el servidor vinculado con la información del catálogo relativa al paquete seleccionado.  
  
 En esta sección se describen otros procedimientos para crear un servidor vinculado y una vista SQL sin recurrir al asistente para la publicación de fuentes de distribución de datos. En ella también encontrará más información sobre cómo usar la función OPENQUERY con el proveedor OLE DB para SSIS.  
  
### <a name="create-a-linked-server-using-the-ole-db-provider-for-ssis"></a>Crear un servidor vinculado con el proveedor OLE DB para SSIS  
 Cree un servidor vinculado con el proveedor OLE DB para SSIS (SSISOLEDB); para ello, ejecute la siguiente consulta en SQL Server Management Studio.  
  
```sql 
  
USE [master]  
GO  
  
EXEC sp_addlinkedserver  
@server = N'SSISFeedServer',  
@srvproduct = N'Microsoft',  
@provider = N'SSISOLEDB',  
@datasrc = N'.'  
GO  
  
```  
  
### <a name="create-a-view-using-linked-server-and-ssis-catalog-information"></a>Crear una vista con un servidor vinculado y la información del catálogo de SSIS  
 En este paso, creará una vista SQL que ejecuta una consulta en el servidor vinculado que creó en la sección anterior. Esta consulta incluye el nombre de la carpeta, el nombre del proyecto y el nombre del paquete en el catálogo de SSIS.  
  
 En el tiempo de ejecución, cuando la vista se ejecuta, la consulta de servidor vinculado definida en la vista inicia el paquete SSIS especificado en dicha consulta y recibe el resultado del paquete como un conjunto de resultados tabulares.  
  
1.  Antes de crear la vista, escriba y ejecute la siguiente consulta en la ventana de nueva consulta. OPENQUERY es una función de conjunto de filas compatible con SQL Server. Ejecuta la consulta de paso a través especificada en el servidor vinculado por medio del proveedor OLE DB asociado al servidor vinculado. Se puede hacer referencia a OPENQUERY en la cláusula FROM de una consulta como si fuera un nombre de tabla. Para obtener más información, consulte la [documentación de OPENQUERY en MSDN Library](../../t-sql/functions/openquery-transact-sql.md) .  
  
    ```sql
    SELECT * FROM OPENQUERY(SSISFeedServer,N'Folder=Eldorado;Project=SSISPackagePublishing;Package=Package.dtsx')   
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  Actualice el nombre de carpeta, el nombre del proyecto y el nombre del paquete si procede. Si la función OPENQUERY genera un error, en **SQL Server Management Studio**, expanda **Objetos de servidor**, **Servidores vinculados** y **Proveedores** y, después, haga doble clic en el proveedor **SSISOLEDB** y asegúrese de que la opción **Permitir InProcess** está activada.  
  
2.  Ejecute la siguiente consulta para crear una vista en la base de datos **TestDB** para este tutorial.  
  
    ```sql
  
    USE [TestDB]   
    GO   
  
    CREATE VIEW SSISPackageView AS   
    SELECT * FROM OPENQUERY(SSISFeedServer, 'Folder=Eldorado;Project=SSISPackagePublishing;Package=Package.dtsx')   
    GO  
  
    ```  
  
3.  Ejecute la siguiente consulta para comprobar la vista.  
  
    ```sql
    SELECT * FROM SSISPackageView  
    ```  
  
### <a name="openquery-function"></a>Función OPENQUERY  
 La sintaxis de la función OPENQUERY es:  
  
```sql 
SELECT * FROM OPENQUERY(<LinkedServer Name>, N'Folder=<Folder Name from SSIS Catalog>; Project=<SSIS Project Name>; Package=<SSIS Package Name>; Use32BitRuntime=[True | False];Parameters="<parameter_name_1>=<value1>; parameter_name_2=<value2>";Timeout=<Number of Seconds>;')  
```  
  
 Los parámetros de carpeta, proyecto y paquete son obligatorios, mientras que los de Use32BitRuntime y Timeout son opcionales.  
  
 El valor de Use32BitRuntime puede ser 0, 1, true o false. Indica si el paquete se debe ejecutar con un tiempo de ejecución de 32 bits (1 o true) cuando la plataforma de SQL Server sea de 64 bits.  
  
 El valor de Timeout indica el número de segundos que el proveedor OLE DB para SSIS puede esperar antes de que lleguen datos nuevos procedentes del paquete SSIS. El valor predeterminado es 60 segundos. Se puede especificar un valor entero de tiempo de espera de entre 20 y 32 000.  
  
 Los parámetros contienen el valor tanto de los parámetros de paquete como de los parámetros del proyecto. Las reglas de los parámetros son las mismas que las de los parámetros de [DTExec](../packages/dtexec-utility.md).  
  
 En la siguiente lista se especifican los caracteres especiales que se pueden usar en la cláusula de consulta:  
  
-   Comilla simple ('): se puede usar en el estándar OPENQUERY. En caso de que quiera usar comillas simples en la cláusula de consulta, use dos comillas simples (").  
  
-   Comillas dobles ("): los parámetros que forman parte de la consulta se insertan entre comillas dobles. Si el valor de parámetro en sí ya contiene comillas dobles, use el carácter de escape. Por ejemplo: \".  
  
-   Corchetes de apertura y cierre ([ y ]): estos caracteres sirven para indicar espacios iniciales o finales. Por ejemplo, “[ algunos espacios ]” representa la cadena “ algunos espacios ” con un espacio inicial y un espacio final. Si estos caracteres se usan en la cláusula de consulta, deben ir acompañados de un carácter de escape. Por ejemplo, \\[ y \\].  
  
-   Barra diagonal (\\): cada \ usada en la cláusula de consulta tiene que ir acompañada de un carácter de escape. Por ejemplo, \\\ se evalúa como \ en la cláusula de consulta.  
  
## <a name="see-also"></a>Consulte también  
 [Destino de streaming de datos](../../integration-services/data-flow/data-streaming-destination.md)   
 [Configuración del destino de streaming de datos](./data-streaming-destination.md)  
  
