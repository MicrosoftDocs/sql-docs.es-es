---
title: Uso del Asistente para la optimización de motor de base de datos
description: Obtenga información sobre cómo la GUI del Asistente para la optimización de motor de base de datos de SQL Server optimiza las bases de datos, administra las sesiones de optimización y muestra recomendaciones de optimización.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 3317d4f8-ed9e-4f2e-b5f1-a6bf3a9d6c8d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 10d32bb3267e79f4f97ce7a4832c1a999751904c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353510"
---
# <a name="lesson-2-using-database-engine-tuning-advisor"></a>Lección 2: Uso del Asistente para la optimización de motor de base de datos

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El Asistente para la optimización de motor de base de datos le permite optimizar bases de datos, administrar sesiones de optimización y ver recomendaciones. Los usuarios con conocimientos avanzados de estructuras de diseño físico pueden utilizar esta herramienta para realizar un análisis de exploración de optimización de la base de datos. Los principiantes en optimizaciones de bases de datos también pueden utilizar esta herramienta para determinar cuál es la mejor configuración de las estructuras de diseño físico para las cargas de trabajo que optimicen. Esta lección proporciona una práctica básica para los administradores que no conocen la interfaz gráfica de usuario (GUI) del Asistente para la optimización de motor de base de datos y los administradores del sistema que no tengan amplios conocimientos acerca de las estructuras de diseño físico.  

## <a name="prerequisites"></a>Requisitos previos 

Para llevar a cabo este tutorial necesita tener SQL Server Management Studio, acceso a un servidor que ejecute SQL Server y una base de datos de AdventureWorks.

- Instale [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue la [base de datos de ejemplo de AdventureWorks2017](../../samples/adventureworks-install-configure.md).


Aquí encontrará instrucciones para restaurar bases de datos en SSMS: [Restaurar una base de datos.](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > Este tutorial está destinado a un usuario familiarizado con el uso de SQL Server Management Studio y las tareas básicas de administración de bases de datos. 
  
## <a name="tuning-a-workload"></a>Optimizar una carga de trabajo
El Asistente para la optimización de motor de base de datos puede utilizarse para determinar el mejor diseño físico de la base de datos para el rendimiento de las consultas en las bases de datos y las tablas que seleccione para optimizar.  

1.  Copie una instrucción [SELECT](../../t-sql/queries/select-examples-transact-sql.md) de ejemplo y péguela en el editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Guarde el archivo como **MyScript.sql** en un directorio donde pueda encontrarlo fácilmente. A continuación se proporciona un ejemplo que funciona en la base de datos AdventureWorks2017.  

 ```sql
 Use [Adventureworks2017]; -- may need to modify database name to match database
 GO
 SELECT DISTINCT pp.LastName, pp.FirstName 
 FROM Person.Person pp JOIN HumanResources.Employee e
 ON e.BusinessEntityID = pp.BusinessEntityID WHERE pp.BusinessEntityID IN 
 (SELECT SalesPersonID 
 FROM Sales.SalesOrderHeader
 WHERE SalesOrderID IN 
 (SELECT SalesOrderID 
 FROM Sales.SalesOrderDetail
 WHERE ProductID IN 
 (SELECT ProductID 
 FROM Production.Product p 
 WHERE ProductNumber = 'BK-M68B-42')));
 GO
 ```

  ![Guarde la consulta SQL](media/dta-tutorials/dta-save-query.png)
  
2.  Inicie el Asistente para la optimización de motor de base de datos. Seleccione **Asistente para la optimización de bases de datos** en el menú **Herramientas** de SQL Server Management Studio (SSMS).  Para más información, vea el [Asistente para la optimización de motor de base de datos](lesson-1-basic-navigation-in-database-engine-tuning-advisor.md#launch-database-tuning-advisor). Conéctese a SQL Server en el cuadro de diálogo **Conectar con el servidor**.  
  
3.  En la pestaña **General** del panel derecho de la GUI del Asistente para la optimización de motor de base de datos, escriba **MySession** en **Nombre de sesión**. 
  
4.  Seleccione **Archivo** para la **carga de trabajo** y, luego, seleccione el icono de prismáticos para que el usuario **Busque un archivo de carga de trabajo**. Busque el archivo **script. SQL** que guardó en el paso°1.  

   ![Busque el script que se guardó anteriormente](media/dta-tutorials/dta-script.png)
  
5.  Seleccione AdventureWorks2017 en la lista **Base de datos para análisis de carga de trabajo**, seleccione AdventureWorks2017 en la cuadrícula **Seleccionar bases de datos y tablas para optimizar** y seleccione la opción **Guardar registro de optimización**. **Base de datos para análisis de carga de trabajo** especifica la primera base de datos a la que se conecta el Asistente para la optimización de motor de base de datos al optimizar una carga de trabajo. Una vez iniciada la optimización, el Asistente para la optimización de motor de base de datos se conecta a las bases de datos especificadas en las instrucciones `USE DATABASE` que contiene la carga de trabajo.  

  ![Opciones de DTA para db](media/dta-tutorials/dta-select-db.png)
  
6.  Haga clic en la pestaña **Opciones de optimización** . En esta práctica, no configurará ninguna opción de optimización, pero tómese unos minutos para revisar las opciones predeterminadas. Presione F1 para ver la Ayuda para esta página con pestañas. Haga clic en **Opciones avanzadas** para ver opciones de optimización adicionales. Haga clic en **Ayuda** , en el cuadro de diálogo **Opciones avanzadas de optimización** , para obtener información sobre las opciones que aparecen. Haga clic en **Cancelar** para cerrar el cuadro de diálogo **Opciones avanzadas de optimización** , y deje seleccionadas las opciones predeterminadas.  

  ![Opciones de optimización DTA](media/dta-tutorials/dta-tuning-options.png)
  
7.  Haga clic en el botón **Iniciar análisis** de la barra de herramientas. Mientras el Asistente para la optimización de motor de base de datos analiza la carga de trabajo, puede supervisar el estado en la pestaña **Progreso** . Una vez se haya completado la optimización, aparecerá la pestaña **Recomendaciones** .  
  
    Si recibe un error acerca de la fecha y la hora de detención de la optimización, compruebe el valor de **Detener el** en la pestaña principal de **Opciones de optimización** . Asegúrese de que la fecha y la hora de **Detener el** son posteriores a la fecha y la hora actuales, y cámbielas si resulta necesario.  

  ![Inicio del análisis DTA](media/dta-tutorials/dta-start-analysis.png)

  
8.  Una vez completado el análisis, guarde su recomendación como un script de [!INCLUDE[tsql](../../includes/tsql-md.md)] haciendo clic en **Guardar recomendaciones** en el menú **Acciones** . En el cuadro de diálogo **Guardar como** , navegue hasta el directorio en el que quiere guardar el script de recomendaciones y escriba el nombre de archivo **MyRecommendations**.  

  ![Guarde las recomendaciones de DTA](media/dta-tutorials/dta-save-recommendations.png)

## <a name="view-tuning-recommendations"></a>Ver las recomendaciones de optimización
  
1.  En la pestaña **Recomendaciones** , utilice la barra de desplazamiento situada en la parte inferior de la página con pestañas para ver todas las columnas de **Recomendaciones de índices** . Cada fila representa un objeto de base de datos (índices o vistas indizadas) que el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] recomienda quitar o crear. Desplácese hasta la columna situada más a la derecha y haga clic en **Definición**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] muestra la ventana **Vista previa de script SQL** , donde se puede ver el script [!INCLUDE[tsql](../../includes/tsql-md.md)] que creará o quitará el objeto de base de datos de esa fila. Haga clic en **Cerrar** para cerrar la ventana de vista previa.  
  
    Si le resulta difícil encontrar una **Definición** que contenga un vínculo, haga clic para desactivar la casilla **Mostrar objetos existentes** al final de la página con pestañas. Esto hará que el número de filas mostradas disminuya. Cuando desactive esta casilla, el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] solamente mostrará los objetos para los que haya generado una recomendación. Active la casilla **Mostrar objetos existentes** para ver todos los objetos de base de datos que existen actualmente en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Utilice la barra de desplazamiento situada en la parte derecha de la página con pestañas para ver todos los objetos.

  ![Recomendación de índice de DTA](media/dta-tutorials/dta-recommendation.png)  
  
2.  Haga clic con el botón derecho en la cuadrícula del panel **Recomendaciones de índices** . Este menú contextual permite seleccionar y anular la selección de recomendaciones. También permite cambiar la fuente del texto de la cuadrícula.  
 
   ![Menú de selección para la recomendación de índices](media/dta-tutorials/dta-index-recommendation-options.png)
  
3.  En el menú **Acciones** , haga clic en **Guardar recomendaciones** para guardar todas las recomendaciones en un script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Asigne el nombre **MySessionRecommendations.sql** al script.  
  
    Abra el script MySessionRecommendations.sql en el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para verla. Podría aplicar las recomendaciones a la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] ejecutando el script en el Editor de consultas; pero no lo haga. Cierre el script en el Editor de consultas sin ejecutarla.  
  
    También podría aplicar las recomendaciones haciendo clic en **Aplicar recomendaciones** en el menú **Acciones** del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] ; pero no lo haga en esta práctica.  
  
4.  Si hay más de una recomendación en la pestaña **Recomendaciones** , borre algunas de las filas que enumeran objetos de base de datos en la cuadrícula **Recomendaciones de índices** .  
  
5.  En el menú **Acciones** , haga clic en **Evaluar recomendaciones**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea una nueva sesión de optimización en la que puede evaluar un subconjunto de las recomendaciones originales de MySession.  
  
6.  Escriba **EvaluateMySession** para el nuevo **Nombre de sesión** y haga clic en el botón **Iniciar análisis** de la barra de herramientas. Puede repetir los pasos 2 y 3 con esta nueva sesión de optimización para ver las recomendaciones.  
  
### <a name="summary"></a>Resumen  
Evaluar un subconjunto de recomendaciones de optimización puede ser necesario si tiene que cambiar las opciones de optimización después de ejecutar una sesión. Por ejemplo, si solicita al Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] que tenga en cuenta las vistas indizadas cuando especifique opciones de optimización para una sesión, pero, después de que la recomendación se genere, decide usar las vistas indizadas de nuevo, Puede usar la opción **Evaluar recomendaciones** del menú **Acciones** para que el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] vuelva a evaluar la sesión omitiendo las vistas indexadas. Cuando utilice la opción **Evaluar recomendaciones** , las recomendaciones generadas previamente se aplicarán hipotéticamente al diseño físico actual para lograr el diseño físico de la segunda sesión de optimización.  
  
La pestaña **Informes** , que se describe en la siguiente tarea de la lección, muestra más información sobre los resultados de optimización.  

## <a name="view-tuning-reports"></a>Ver informes de optimización
Aunque es útil ver los scripts que pueden utilizarse para implementar los resultados de optimización, el Asistente para la optimización de motor de base de datos también ofrece varios informes muy útiles que podrá ver. Estos informes proporcionan información acerca de las estructuras de diseño físico existentes en la base de datos que está optimizando y acerca de las estructuras recomendadas. Los informes de optimización pueden verse haciendo clic en la pestaña **Informes** , tal y como se describe en la práctica siguiente.


1. Seleccione la pestaña **Informes** en el Asistente para la optimización de bases de datos. 
2. En el panel **Resumen de la optimización** , puede ver información acerca de esta sesión de optimización. Utilice la barra de desplazamiento para ver todo el contenido del panel. Observe las opciones **Porcentaje de mejora esperada** y **Espacio usado por la recomendación**. Es posible limitar el espacio utilizado por la recomendación al establecer las opciones de optimización. En la pestaña **Opciones de optimización** , seleccione **Opciones avanzadas**. Active **Definir espacio máximo para recomendaciones** y especifique el espacio máximo, en megabytes, que una configuración de recomendación puede usar. Use el botón **Atrás** del explorador de la Ayuda para volver a este tutorial. 

    ![Resumen de la optimización de DTA](media/dta-tutorials/dta-tuning-summary.png)
  
3.  En el panel **Informes de optimización** , haga clic en **Informe de costo de instrucciones** en la lista **Seleccionar informe** . Si necesita más espacio para ver el informe, arrastre el borde del panel del **Monitor de sesión** hacia la izquierda. Cada instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecuta para una tabla en la base de datos tiene un costo de rendimiento asociado. Este costo de rendimiento puede reducirse creando índices efectivos en las columnas que se consultan frecuentemente en una tabla. Este informe muestra el porcentaje estimado de mejora entre el costo original que resulta de ejecutar una instrucción en la carga de trabajo y el costo que resultaría de implementar la recomendación de optimización. Observe que la cantidad de información que contiene el informe se basa en la longitud y complejidad de la carga de trabajo.  

    ![Informe de DTA: costo de instrucciones](media/dta-tutorials/dta-statement-cost.png)
  
4.  Haga clic con el botón derecho en el panel **Informe de costo de instrucciones** del área de la cuadrícula y haga clic en **Exportar a archivo**. Guarde el informe como **MyReport**. Automáticamente se anexará la extensión .xml al nombre del archivo. Puede abrir el archivo MyReport.xml en el editor XML que prefiera o en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ver el contenido del informe.  
  
5.  Vuelva a la pestaña **Informes** del Asistente para la optimización de motor de base de datos y vuelva a hacer clic con el botón derecho en **Informe de costo de instrucciones** . Revise las otras opciones disponibles. Tenga en cuenta que puede cambiar la fuente del informe que está viendo. Al cambiar la fuente aquí, también se cambiará en las demás páginas con pestañas.  
  
6.  Haga clic en otros informes de la lista **Seleccionar informe** para familiarizarse con ellos.  
  
### <a name="summary"></a>Resumen  
Ha explorado la pestaña **Informes** de la GUI del Asistente para la optimización de motor de base de datos para la sesión de optimización MySession. Puede utilizar estos mismos pasos para explorar los informes que se generaron para la sesión de optimización EvaluateMySession. Haga doble clic en **EvaluateMySession** en el panel **Monitor de sesión** para comenzar.  


 ## <a name="next-lesson"></a>Lección siguiente  
[Lección 3: Usa la utilidad del símbolo del sistema DTA](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
   
  
