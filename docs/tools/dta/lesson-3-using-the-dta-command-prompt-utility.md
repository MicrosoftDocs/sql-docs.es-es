---
title: Uso de la utilidad del símbolo del sistema dta
description: Obtenga información sobre la funcionalidad de la utilidad del símbolo del sistema dta, además de la que ofrece el Asistente para la optimización de motor de base de datos de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 30f27f4d-8852-4b12-ba62-57f63e496f1d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: aed8df33148a93d75fc5133f12be8e24b35bf599
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353498"
---
# <a name="lesson-3-using-the-dta-command-prompt-utility"></a>Lección 3: Uso de la utilidad del símbolo del sistema dta
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La utilidad del símbolo del sistema **dta** ofrece funcionalidad adicional a la del Asistente para la optimización de motor de base de datos.  
  
Puede utilizar las herramientas XML que desee para crear archivos de entrada para la utilidad mediante el esquema XML del Asistente para la optimización de motor de base de datos. Este esquema se instala al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y se encuentra en: C:\Archivos de programa (x86)\Microsoft SQL Server\110\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd.  
  
El esquema XML del Asistente para la optimización de motor de base de datos también se encuentra disponible en línea en el [sitio web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
Este esquema ofrece una mayor flexibilidad a la hora de configurar opciones de optimización. Por ejemplo, le permite realizar análisis de escenarios condicionales. El análisis de escenarios condicionales conlleva especificar un conjunto de estructuras de diseño físico hipotético ya existente para la base de datos que desea optimizar y, a continuación, analizarla con el Asistente para la optimización de motor de base de datos para determinar si este diseño físico hipotético mejorará el rendimiento del procesamiento de consultas. Este tipo de análisis tiene la ventaja de evaluar la nueva configuración sin incurrir en la sobrecarga que supone implementarla realmente. Si el diseño físico hipotético no proporciona las mejoras de rendimiento que desea, es fácil modificarlo y volver a analizarlo hasta que logre la configuración que se ajuste a sus necesidades.  
  
Además, al usar el esquema XML del Asistente para la optimización de motor de base de datos y la utilidad del símbolo del sistema **dta** , puede incorporar la funcionalidad del asistente a los scripts y usarla con otras herramientas de diseño de bases de datos.  
  
Esta lección no cubre el uso de la funcionalidad de entrada XML del Asistente para la optimización de motor de base de datos.  
  
 Esta tarea le guía por los pasos necesarios para iniciar la utilidad **dta**, ver su Ayuda y usarla para optimizar una carga de trabajo desde el símbolo del sistema. Se usará la carga de trabajo (MyScript.sql) que ha creado para la práctica sobre la interfaz gráfica de usuario (GUI) del Asistente para la optimización de motor de base de datos: [Optimizar una carga de trabajo](lesson-2-using-database-engine-tuning-advisor.md#tuning-a-workload).  
  
El tutorial usa la base de datos de ejemplo de AdventureWorksLT2017. Por motivos de seguridad, las bases de datos de ejemplo no se instalan de manera predeterminada. Para instalarlas, consulte [Instalar ejemplos de SQL Server y bases de datos de ejemplo](../../samples/adventureworks-install-configure.md).  
  
Las tareas siguientes le guían por los pasos necesarios para abrir un símbolo del sistema, iniciar la utilidad **dta** , ver la Ayuda de la sintaxis y optimizar una carga de trabajo sencilla (MyScript.sql) que ha creado en [Optimizar una carga de trabajo](./lesson-2-using-database-engine-tuning-advisor.md).  

## <a name="prerequisites"></a>Requisitos previos 

Para llevar a cabo este tutorial necesita tener SQL Server Management Studio, acceso a un servidor que ejecute SQL Server y una base de datos de AdventureWorks.

- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue la [base de datos de ejemplo de AdventureWorks2017](../../samples/adventureworks-install-configure.md).


Aquí encontrará instrucciones para restaurar bases de datos en SSMS: [Restaurar una base de datos.](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > Este tutorial está destinado a un usuario familiarizado con el uso de SQL Server Management Studio y las tareas básicas de administración de bases de datos. 

## <a name="access-dta-command-prompt-utility-help-menu"></a>Acceso al menú de ayuda de la utilidad del símbolo del sistema DTA
  
  
1.  En el menú **Inicio** , seleccione **Todos los programas**, **Accesorios** y, después, haga clic en **Símbolo del sistema**.  
  
2.  En el símbolo del sistema, escriba lo siguiente y presione ENTRAR:  
  
    ```  
    dta -? | more  
    ```  
  
    La parte `| more` de este comando es opcional. No obstante, si la utiliza, puede examinar la ayuda de la sintaxis para la utilidad. Presione ENTRAR para avanzar por el texto de ayuda línea a línea o la BARRA ESPACIADORA para avanzar página a página.  

  ![Uso de la ayuda con la utilidad cmd DTA](media/dta-tutorials/dta-cmd-help.png)

## <a name="tune-simple-workload-using-the-dta-command-prompt-utility"></a>Optimización de una carga de trabajo sencilla mediante la utilidad del símbolo del sistema DTA  


  
1.  En el símbolo del sistema, navegue al directorio donde haya almacenado el archivo MyScript.sql.  
  
2.  En el símbolo del sistema, escriba lo siguiente y presione ENTRAR para ejecutar el comando e iniciar la sesión de optimización (observe que la utilidad reconoce mayúsculas y minúsculas al analizar comandos):  
  
    ```  
    dta -S YourServerName\YourSQLServerInstanceName -E -D AdventureWorks2012 -if MyScript.sql -s MySession2 -of MySession2OutputScript.sql -ox MySession2Output.xml -fa IDX_IV -fp NONE -fk NONE  
    ```  
  
    donde `-S` especifica el nombre del servidor y la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] está instalada. El valor `-E` especifica que se desea utilizar una conexión de confianza para la instancia, lo cual es apropiado si piensa conectarse con una cuenta de dominio de Windows. El valor `-D` especifica la base de datos que desea optimizar, `-if` especifica el archivo de carga de trabajo, `-s` especifica el nombre de la sesión, `-of` especifica el archivo en el que desea que la herramienta escriba el script de recomendaciones [!INCLUDE[tsql](../../includes/tsql-md.md)] y `-ox` especifica el archivo en el que desea que la herramienta escriba las recomendaciones en formato XML. Los tres últimos modificadores especifican las opciones de optimización siguientes: `-fa IDX_IV` especifica que el Asistente para la optimización de motor de base de datos solo debe agregar índices (agrupados y no agrupados) y vistas indizadas; `-fp NONE` especifica que durante el análisis no se tendrá en cuenta ninguna estrategia de partición; y `-fk NONE` especifica que no se debe mantener ninguna estructura de diseño físico en la base de datos cuando el Asistente para la optimización de motor de base de datos haga sus recomendaciones.  

  ![uso de CMD con DTA](media/dta-tutorials/dta-cmd.png)
  
3.  Una vez que el Asistente para la optimización de motor de base de datos acabe de optimizar la carga de trabajo, mostrará un mensaje para indicar que la sesión de optimización finalizó correctamente. Puede ver los resultados de la optimización utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para abrir los archivos MySession2OutputScript.sql y MySession2Output.xml. También puede abrir la sesión de optimización MySession2 en la GUI del Asistente para la optimización de motor de base de datos y ver las recomendaciones e informes de la misma forma que ha hecho en [Ver recomendaciones de optimización](./lesson-2-using-database-engine-tuning-advisor.md) y [Ver informes de optimización](./lesson-2-using-database-engine-tuning-advisor.md).  
  
 
## <a name="after-you-finish-this-tutorial"></a>Al finalizar este tutorial  
Cuando haya finalizado las lecciones de este tutorial, consulte los temas siguientes para obtener más información acerca del Asistente para la optimización de motor de base de datos:  
  
-   [Asistente para la optimización de motor de base de datos](../../relational-databases/performance/database-engine-tuning-advisor.md) para obtener descripciones sobre cómo realizar tareas con esta herramienta. 
-   [dta (utilidad)](../../tools/dta/dta-utility.md) para obtener material de referencia sobre la utilidad de símbolo del sistema y el archivo XML opcional que puede usar para controlar el funcionamiento de la utilidad.  
  
Para volver al inicio del tutorial, consulte [Tutorial: Asistente para la optimización de motor de base de datos](../../tools/dta/tutorial-database-engine-tuning-advisor.md).  
  
## <a name="see-also"></a>Consulte también  
[Tutoriales del motor de base de datos](../../relational-databases/database-engine-tutorials.md)  
