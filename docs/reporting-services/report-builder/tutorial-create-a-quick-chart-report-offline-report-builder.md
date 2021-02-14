---
title: 'Tutorial: Creación de un informe de gráfico rápido sin conexión (Generador de informes) | Microsoft Docs'
description: En este tutorial, creará un gráfico circular en un informe paginado de Reporting Services en Generador de informes. Después, agregará porcentajes y modificará el gráfico circular.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- reports, creating
- tutorials, getting started
- creating reports
ms.assetid: 6b1db67a-cf75-494c-b70c-09f1e6a8d414
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2f7fb9781583a56a2765f066462e62717c1d32d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070555"
---
# <a name="tutorial-create-a-quick-chart-report-offline-report-builder"></a>Tutorial: Creación de un informe de gráfico rápido sin conexión (Generador de informes)

  En este tutorial, se usará un asistente para crear un gráfico circular en un informe paginado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. A continuación, agregue porcentajes y modifique el gráfico circular un poco. 
  
Puede hacerlo de dos maneras diferentes: Ambos métodos tienen como resultado un gráfico circular semejante al de esta ilustración:  
  
 ![Gráfico circular rápido de Report Builder](../../reporting-services/report-builder/media/report-builder-quick-pie-chart.png "Gráfico circular rápido de Report Builder")  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Si usa datos XML o una consulta [!INCLUDE[tsql](../../includes/tsql-md.md)], necesita tener acceso al Generador de informes. Puede iniciar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde un servidor de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en modo nativo o en modo integrado de SharePoint, o bien puede descargar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde Centro de descarga de Microsoft. Para obtener más información, consulte [Install Report Builder](../../reporting-services/install-windows/install-report-builder.md).  
  
##  <a name="two-ways-to-do-this-tutorial"></a><a name="TwoWays"></a> Dos maneras de hacer este tutorial  
  
-   [Crear el gráfico circular con datos XML](#CreatePieChartXML)  
  
-   [Crear el gráfico circular con una consulta Transact-SQL que contenga datos](#CreatePieQueryData)  
  
### <a name="using-xml-data-for-this-tutorial"></a>Usar datos XML para este tutorial  
 Puede utilizar los datos XML que copia de este tema y pegarlos en el asistente. No necesita estar conectado a un servidor de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en modo nativo o en modo integrado de SharePoint, y no necesita acceso a una instancia de SQL Server.  
  
 [Crear el gráfico circular con datos XML](#CreatePieChartXML)  
  
### <a name="using-a-tsql-query-that-contains-data-for-this-tutorial"></a>Usar una consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] que contenga los datos de este tutorial  
 Puede copiar desde este tema una consulta con datos incluidos en él y pegarlos en el asistente. Necesitará el nombre de una instancia de SQL Server y credenciales suficiente para el acceso de solo lectura a cualquier base de datos. La consulta del conjunto de datos en el tutorial usa datos literales, pero la consulta debe ser procesada por una instancia de SQL Server para que devuelva los metadatos requeridos por un conjunto de datos de informe.  
  
 La ventaja de usar la consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] es que todos los demás tutoriales del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] usan el mismo método, de modo que cuando realice los otros tutoriales ya sabrá lo que debe hacer.  
  
 La consulta [!INCLUDE[tsql](../../includes/tsql-md.md)] TSQL requiere otros requisitos previos. Para obtener más información, consulte [Requisitos previos para los tutoriales&#40;Generador de informes&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
 [Crear el gráfico circular con una consulta Transact-SQL que contenga datos](#CreatePieQueryData)  
  
##  <a name="creating-the-pie-chart-with-xml-data"></a><a name="CreatePieChartXML"></a> Crear el gráfico circular con datos XML  
  
1.  [Inicie el Generador de informes](../../reporting-services/report-builder/start-report-builder.md) desde el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o el servidor de informes en modo integrado de SharePoint o bien desde su equipo.  
  
     Aparecerá el cuadro de diálogo **Introducción** .  
  
     ![Introducción a Report Builder](../../reporting-services/media/rb-getstarted.png "Introducción a Report Builder")  
  
     Si no aparece el cuadro de diálogo **Introducción** , haga clic en **Archivo** >**Nuevo**. El cuadro de diálogo **Nuevo informe o conjunto de datos** tiene prácticamente el mismo contenido que el cuadro de diálogo **Introducción** .  
  
2.  En el panel de la izquierda, compruebe que está seleccionada la opción **Nuevo informe** .  
  
3.  En el panel derecho, haga clic en **Asistente para gráfico** y, a continuación, haga clic en **Crear**.  
  
4.  En la página **Elegir un conjunto de datos** , haga clic en **Crear un conjunto de datos** y, a continuación, haga clic en **Siguiente**.  
  
5.  En la página **Elegir una conexión a un origen de datos** , haga clic en **Nueva**.  
  
     Se abre el cuadro de diálogo **Propiedades del origen de datos** .  
  
6.  Puede dar el nombre que desee a un origen de datos. En el cuadro **Nombre** , escriba: **MiGráficoCircular**.  
  
7.  En el cuadro **Seleccionar tipo de conexión** , haga clic en **XML**.  
  
8.  Haga clic en la pestaña **Credenciales** y seleccione **Usar usuario de Windows actual. Puede que se requiera la delegación Kerberos** y haga clic en **Aceptar**.  
  
9. En la página **Elegir una conexión a un origen de datos** , haga clic en **miGráficoCircular** y, a continuación, haga clic en **Siguiente**.  
  
10. Copie el texto siguiente y péguelo en el cuadro grande en la parte superior de la página **Diseñar una consulta** .  
  
    ```  
    <Query>  
    <ElementPath>Root /S  {@Sales (Integer)} /C {@FullName} </ElementPath>  
    <XmlData>  
    <Root>  
    <S Sales="150">  
      <C FullName="Jae Pak" />  
    </S>  
    <S Sales="350">  
      <C FullName="Jillian  Carson" />  
    </S>  
    <S Sales="250">  
      <C FullName="Linda C Mitchell" />  
    </S>  
    <S Sales="500">  
      <C FullName="Michael Blythe" />  
    </S>  
    <S Sales="450">  
      <C FullName="Ranjit Varkey" />  
    </S>  
    </Root>  
    </XmlData>  
    </Query>  
    ```  
  
11. (Opcional) Haga clic en el botón **Ejecutar** ( **!** ) para ver los datos en los que se basará su gráfico.  
  
     ![Consulta de diseño de Report Builder](../../reporting-services/report-builder/media/rb-designquery.png "Consulta de diseño de Report Builder")  
  
12. Haga clic en **Next**.  
  
13. En la página **Elegir un tipo de gráfico** , haga clic en **Circular** y, a continuación, haga clic en **Siguiente**.  
  
14. En la página **Organizar campos del gráfico**, haga doble clic en el campo **Ventas** en el cuadro **Campos disponibles**.  
  
     Observe que se desplaza automáticamente al cuadro de diálogo **Valores** , porque es un valor numérico.  
  
     ![Organizar campos en el asistente de Report Builder](../../reporting-services/report-builder/media/rb-wizarrangefields.png "Organizar campos en el asistente de Report Builder")  
  
15. Arrastre el campo **FullName** desde el cuadro **Campos disponibles** hasta **Categorías** (o haga doble clic en él; se desplazará al cuadro **Categorías**) y, después, haga clic en **Siguiente**.  
  
     La página de vista previa muestra el nuevo gráfico circular con datos representacionales. La leyenda dice Full Name 1, Full Name 2, etc., en lugar de los nombres de los vendedores, y el tamaño de los sectores del gráfico circular no es preciso. Únicamente es para que se haga una idea de cómo será el informe.  
  
     ![Vista previa de nuevo gráfico de Report Builder](../../reporting-services/report-builder/media/rb-newchartpreview.png "Vista previa de nuevo gráfico de Report Builder")  
  
16. Haga clic en **Finalizar**  
  
     Ahora verá el nuevo informe de gráfico circular en la vista de diseño, todavía con datos representacionales.  
  
     ![Nuevo gráfico circular en la vista de diseño de Report Builder](../../reporting-services/report-builder/media/rb-newpiedesign.png "Nuevo gráfico circular en la vista de diseño de Report Builder")  
  
17. Para ver el gráfico circular real, haga clic en **Ejecutar** en la pestaña **Inicio** de la cinta de opciones.  
  
     ![Ejecución de nuevo gráfico de Report Builder](../../reporting-services/report-builder/media/rb-newchartrun.png "Ejecución de nuevo gráfico de Report Builder")  
  
18. Para seguir modificando el gráfico circular, vaya a la sección [Después de ejecutar el asistente](#AfterWizard) de este artículo.  
  
##  <a name="creating-the-pie-chart-with-a-tsql-query"></a><a name="CreatePieQueryData"></a> Crear el gráfico circular con una consulta [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
1.  [Inicie el Generador de informes](../../reporting-services/report-builder/start-report-builder.md) desde el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o el servidor de informes en modo integrado de SharePoint o bien desde su equipo.  
  
     Aparecerá el cuadro de diálogo **Introducción** .  
  
    > [!NOTE]  
    >  Si no aparece el cuadro de diálogo **Introducción** , haga clic en **Archivo** >**Nuevo**. El cuadro de diálogo **Nuevo informe o conjunto de datos** tiene prácticamente el mismo contenido que el cuadro de diálogo **Introducción** .  
  
2.  En el panel de la izquierda, compruebe que está seleccionada la opción **Nuevo informe** .  
  
3.  En el panel derecho, haga clic en **Asistente para gráfico** y, a continuación, haga clic en **Crear**.  
  
4.  En la página **Elegir un conjunto de datos** , haga clic en **Crear un conjunto de datos** y, a continuación, haga clic en **Siguiente**.  
  
5.  En la página **Elegir una conexión a un origen de datos** , seleccione un origen de datos existente o vaya al servidor de informes y seleccione un origen de datos y, a continuación, haga clic en **Siguiente**. Puede que necesite escribir un nombre de usuario y contraseña.  
  
    > [!NOTE]  
    >  El origen de datos que elija no importa, con tal de que tenga los permisos adecuados. No está recibiendo datos del origen de datos. Para obtener más información, consulte [Requisitos previos para los tutoriales&#40;Generador de informes&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
6.  En la página **Diseñar una consulta** , haga clic en **Editar como texto**.  
  
7.  Pegue la siguiente consulta en el panel de consulta:  
  
    ```  
    SELECT 150 AS Sales, 'Jae Pak' AS FullName   
    UNION SELECT 350 AS Sales, 'Jillian Carson' AS FullName   
    UNION SELECT 250 AS Sales, 'Linda C Mitchell' AS FullName   
    UNION SELECT 500 AS Sales, 'Michael Blythe' AS FullName   
    UNION SELECT 450 AS Sales, 'Ranjit Varkey' AS FullName   
    ```  
  
8.  (Opcional) Haga clic en el botón Ejecutar ( **!** ) para ver los datos en los que se basará su gráfico.  
  
9. Haga clic en **Next**.  
  
10. En la página **Elegir un tipo de gráfico** , haga clic en **Circular** y, a continuación, haga clic en **Siguiente**.  
  
11. En la página **Organizar campos del gráfico**, haga doble clic en el campo **Ventas** en el cuadro **Campos disponibles**.  
  
     Observe que se desplaza automáticamente al cuadro **Valores** , porque es un valor numérico.  
  
12. Arrastre el campo **FullName** desde el cuadro **Campos disponibles** hasta **Categorías** (o haga doble clic en él; se desplazará al cuadro **Categorías**) y, después, haga clic en **Siguiente**.  
  
13. Haga clic en **Finalizar**  
  
     Se ve ahora el nuevo informe de gráfico circular en la superficie de diseño. Lo que ve es figurativo. La leyenda dice Full Name 1, Full Name 2, etc., en lugar de los nombres de los vendedores, y el tamaño de los sectores del gráfico circular no es preciso. Únicamente es para que se haga una idea de cómo será el informe.  
  
15. Para ver el gráfico circular real, haga clic en **Ejecutar** en la pestaña **Inicio** de la cinta de opciones.  
 
##  <a name="after-you-run-the-wizard"></a><a name="AfterWizard"></a> Después de ejecutar el asistente  
 Ahora que tiene el informe del gráfico circular, puede trabajar con él. En la pestaña **Ejecutar** de la cinta de opciones, haga clic en **Diseño**, de modo que pueda seguir modificándolo.  
  
## <a name="make-the-chart-bigger"></a>Agrandar el gráfico  
 Quizá quiera que el gráfico circular sea más grande. 
 
*  Haga clic en el gráfico, pero no en ningún elemento del gráfico, para seleccionarlo y arrastre la esquina inferior derecha para cambiar el tamaño.  

Observe que la superficie de diseño aumenta a medida que realiza la acción de arrastrar.
  
## <a name="add-a-report-title"></a>Agregar un título de informe  
1. Seleccione las palabras **Título de gráfico** en la parte superior del gráfico y, a continuación, escriba un título, como **Gráfico circular de ventas**.  
2. Con el título seleccionado, en el panel Propiedades, cambie **Color** a **Negro** y **FontSize** a **12 pt**.
  
## <a name="add-percentages"></a>Agregar porcentajes  
 
1.  Haga clic con el botón derecho en el gráfico circular y seleccione **Mostrar etiquetas de datos**. Las etiquetas de datos aparecen dentro de cada sector del gráfico circular.  
  
2.  Haga clic con el botón derecho en las etiquetas y seleccione **Propiedades de la etiqueta de la serie**. Aparece el cuadro de diálogo **Propiedades de la etiqueta de la serie** .  
  
3.  Escriba **#PERCENT{P0}** en el cuadro **Datos de etiqueta**.  
  
     **{P0}** le da el porcentaje sin decimales. Si escribe **#PERCENT**, los números tendrán dos posiciones decimales. **#PERCENT** es una palabra clave que realiza un cálculo o función automáticamente; hay muchas otras.  
     
4. Haga clic en **Sí** para confirmar que desea establecer **UseValueAsLabel** en **False**.

5. En la pestaña **Fuente** , seleccione **Negrita** y cambie **Color** a **Blanco**.

6. Haga clic en **OK**.     
  
 Para obtener más información acerca de cómo personalizar las etiquetas del gráfico y las leyendas, consulte [Mostrar valores de porcentaje en un gráfico circular &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md) y [Cambiar el texto de un elemento de leyenda&#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md).  
  
##  <a name="whats-next"></a><a name="WhatsNext"></a> ¿Qué debe hacer a continuación?  
 Ahora que ha creado su primer informe en el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], está listo para leer los demás tutoriales y empezar a crear informes a partir de sus propios datos. Para ejecutar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], necesitará permiso de acceso a los orígenes de datos, como las bases de datos, con una *cadena de conexión*, que le conecta al origen de datos. El administrador del sistema tiene esta información y puede facilitársela.  
  
 Para trabajar con otros tutoriales, necesita el nombre de una instancia de SQL Server y credenciales suficientes para el acceso de solo lectura a cualquier base de datos. El administrador del sistema puede facilitárselo también.  
  
 Finalmente, para guardar los informes en un servidor de informes o en un sitio de SharePoint integrado con un servidor de informes, necesitará la dirección URL y los permisos necesarios. Puede ejecutar cualquier informe que cree directamente desde su equipo, pero los informes tienen más funcionalidad cuando se ejecutan desde el servidor de informes o desde el sitio de SharePoint. Necesita permisos para ejecutar sus informes u otros desde el servidor de informes o desde el sitio de SharePoint donde estén publicados. Hable con el administrador del sistema para obtener acceso.  
  
 Podría resultarle de ayuda leer sobre algunos conceptos y términos antes de empezar. Vea [Conceptos de Reporting Services (SSRS)](../reporting-services-concepts-ssrs.md). Además, dedique algún tiempo al planeamiento antes de crear su primer informe. Será tiempo bien invertido. Consulte [Planear un informe &#40;Generador de informes&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md).  

## <a name="next-steps"></a>Pasos siguientes

[Tutoriales del Generador de informes](../../reporting-services/report-builder-tutorials.md)   
[Generador de informes en SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
