---
title: 'Ejecución de trabajos de Spark: Kit de herramientas de Azure para IntelliJ'
titleSuffix: SQL Server Big Data Clusters
description: Obtenga información sobre cómo enviar trabajos de Spark en clústeres de macrodatos de SQL Server en Azure Toolkit for IntelliJ mediante el envío de un archivo Jar o Py local.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.topic: conceptual
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 36718ef7ea9def99a2b2629119cbd7dfee63dac3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043645"
---
# <a name="submit-spark-jobs-on-big-data-clusters-2019-in-intellij"></a>Envío de trabajos de Spark en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en IntelliJ

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Uno de los escenarios clave para [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] es la capacidad de enviar trabajos de Spark. La característica de envío de trabajos de Spark permite enviar archivos Jar o Py locales con referencias a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]. También permite ejecutar archivos Jar o Py, que ya se encuentran en el sistema de archivos HDFS. 

## <a name="prerequisites"></a>Prerrequisitos

- Clúster de macrodatos de SQL Server.
- Java Development Kit para Oracle. Puede instalarlo desde el [sitio web de Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. Puede instalarlo desde el [sitio web de JetBrains](https://www.jetbrains.com/idea/download/).
- Extensión Azure Toolkit for IntelliJ. Para obtener instrucciones de instalación, vea [Instalación del kit de herramientas de Azure para IntelliJ](/azure/azure-toolkit-for-intellij-installation).

## <a name="link-sql-server-big-data-cluster"></a>Vincular el clúster de macrodatos de SQL Server
1. Abra la herramienta IntelliJ IDEA.

2. Si usa un certificado autofirmado, deshabilite la validación de certificados TLS/SSL en el menú **Herramientas**, seleccione **Azure**, **Validate Spark Cluster SSL Certificate** (Validar certificado SSL de clúster de Spark) y, luego, **Deshabilitar**.

    ![Vinculación de clúster de macrodatos de SQL Server: deshabilitación de TLS/SSL](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-disableSSL.png)

3. Abra Azure Explorer desde el menú **Ver**, seleccione **Ventanas de herramientas** y, luego, **Azure Explorer**.
4. Haga clic con el botón derecho en **SQL Server big data cluster** (Clúster de macrodatos de SQL Server) y seleccione **Link SQL Server big data cluster** (Vincular clúster de macrodatos de SQL Server). Escriba el **Servidor**, el **Nombre de usuario** y la **Contraseña** y haga clic en **Aceptar**.

    ![Vinculación de clúster de macrodatos: cuadro de diálogo](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-dialog.png)

5. Cuando aparezca el cuadro de diálogo que indica que el certificado del servidor no es de confianza, haga clic en **Aceptar**. Puede administrar el certificado más adelante, vea [Certificados de servidor](https://www.jetbrains.com/help/idea/settings-tools-server-certificates.html).

6. El clúster vinculado aparece en **SQL Server big data cluster** (Clúster de macrodatos de SQL Server). Puede supervisar el trabajo de Spark si abre la interfaz de usuario del historial de Spark y la interfaz de usuario de Yarn; también puede desvincular si hace clic con el botón derecho en el clúster.

    ![Vinculación de clúster de macrodatos: menú contextual](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-contextmenu.png)

## <a name="create-a-spark-scala-application-from-spark-template"></a>Crear una aplicación de Spark Scala a partir de una plantilla de Spark

1. Inicie IntelliJ IDEA y, luego, cree un proyecto. En el cuadro de diálogo **Nuevo proyecto**, siga estos pasos: 

   a. Seleccione **Azure Spark/HDInsight** > **Spark Project with Samples (Scala)** (Proyecto de Spark con ejemplos (Scala)).

   b. En la lista **Herramienta de compilación**, seleccione una de las siguientes, según sus necesidades:

      * **Maven**, para la compatibilidad con el asistente para la creación de proyectos de Scala
      * **SBT**, para administrar las dependencias y compilar para el proyecto de Scala

    ![Cuadro de diálogo Nuevo proyecto](./media/spark-submit-job-intellij-tool-plugin/create-hdi-scala-app.png)

2. Seleccione **Next** (Siguiente).

3. El asistente para la creación de proyectos de Scala detecta automáticamente si ha instalado el complemento Scala. Seleccione **Instalar**.

   ![Comprobación del complemento Scala](./media/spark-submit-job-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. Para descargar el complemento Scala, seleccione **Aceptar**. Siga las instrucciones para reiniciar IntelliJ. 

   ![Cuadro de diálogo de instalación del complemento Scala](./media/spark-submit-job-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. En la ventana **Nuevo proyecto**, siga estos pasos:  

    ![Selección del SDK de Spark](./media/spark-submit-job-intellij-tool-plugin/hdi-new-project.png)

   a. Escriba un nombre de proyecto y una ubicación.

   b. En la lista desplegable **Project SDK** (SDK del proyecto), seleccione **Java 1.8** para el clúster de Spark 2.x o **Java 1.7** para el clúster de Spark 1.x.

   c. En la lista desplegable **Versión de Spark**, el asistente para la creación de proyectos de Scala integra la versión correcta del SDK de Spark y el SDK de Scala. Si la versión del clúster de Spark es anterior a 2.0, seleccione **Spark 1.x**. De lo contrario, seleccione **Spark2.x**. En este ejemplo se usa **Spark 2.0.2 (Scala 2.11.8)** .

6. Seleccione **Finalizar**.

7. El proyecto de Spark crea automáticamente un artefacto. Para ver el artefacto, siga estos pasos:

   a. En el menú **Archivo**, seleccione **Estructura del proyecto**.

   b. En el cuadro de diálogo **Estructura del proyecto**, seleccione **Artefactos** para ver el artefacto predeterminado que se ha creado. También puede crear su propio artefacto si selecciona el signo más ( **+** ).

      ![Información del artefacto en el cuadro de diálogo](./media/spark-submit-job-intellij-tool-plugin/default-artifact.png)
      

## <a name="submit-application-to-sql-server-big-data-cluster"></a>Enviar aplicación a clúster de macrodatos de SQL Server
Después de vincular un clúster de macrodatos de SQL Server, puede enviarle una aplicación.

1. Establezca la configuración en la ventana **Run/Debug Configurations** (Ejecutar o depurar configuraciones), haga clic en +->**Apache Spark en SQL Server**, seleccione la pestaña **Remotely Run in Cluster** (Ejecutar en clúster de forma remota), establezca los parámetros como sigue y haga clic en Aceptar.

    ![Entrada de adición de configuración en consola interactiva](./media/spark-submit-job-intellij-tool-plugin/interactive-console-add-config-entry.png)

    ![Vinculación de clúster de macrodatos: configuración](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-config.png)

    * En **Spark clusters (Linux only)** (Clústeres de Spark (solo Linux)), seleccione el clúster en el que quiere ejecutar la aplicación.

    * Seleccione un artefacto en el proyecto de IntelliJ o en el disco duro.

    * Campo **Main class name** (Nombre de clase principal): el valor predeterminado es la clase principal del archivo seleccionado. Puede cambiar la clase si selecciona los puntos suspensivos ( **...** ) y elige otra clase.   

    * Campo **Job Configurations** (Configuraciones de trabajo):  los valores predeterminados se establecen como la imagen mostrada arriba. Puede cambiar el valor o agregar un nuevo valor o clave para el envío del trabajo. Para obtener más información: [API de REST de Apache Livy](http://livy.incubator.apache.org./docs/latest/rest-api.html)

      ![Significado de la configuración del trabajo del cuadro de diálogo de envío de Spark](./media/spark-submit-job-intellij-tool-plugin/submit-job-configurations.png)

    * Campo **Argumentos de la línea de comandos**: puede especificar los valores de los argumentos divididos mediante espacios para la clase principal en caso necesario.

    * Campos **Referenced Jars** (Jar a los que se hace referencia) y **Referenced Files** (Archivos a los que se hace referencia): puede escribir las rutas de acceso de los archivos y los Jar a los que se hace referencia, si los hubiera. Para obtener más información: [Configuración de Apache Spark](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) 

      ![Significado de los archivos Jar del cuadro de diálogo de envío de Spark](./media/spark-submit-job-intellij-tool-plugin/jar-files-meaning.png)

       > [!NOTE]  
       > Para cargar los archivos jar y los archivos a los que se hace referencia, vea: [Cómo cargar recursos en un clúster](/azure/storage/blobs/storage-quickstart-blobs-storage-explorer)
                         
    * **Ruta de acceso de carga**: puede indicar la ubicación de almacenamiento para el envío de recursos de proyecto Jar o Scala. Se admiten varios tipos de almacenamiento: **Use Spark interactive session to upload** (Usar sesión interactiva de Spark para cargar) y **Use WebHDFS to upload** (Usar WebHDFS para cargar)
    
2. Haga clic en **SparkJobRun** para enviar el proyecto al clúster seleccionado. La pestaña **Remote Spark Job in Cluster** (Trabajo de Spark remoto en clúster) muestra el progreso de la ejecución del trabajo en la parte inferior. Puede detener la aplicación si hace clic en el botón rojo.  

    ![Vinculación de clúster de macrodatos: ejecución](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-run.png)

## <a name="spark-console"></a>Consola de Spark
Puede ejecutar la consola local de Spark (Scala) o la consola de sesión interactiva de Spark Livy (Scala).

### <a name="spark-local-consolescala"></a>Consola local de Spark (Scala)
Asegúrese de que cumple el requisito previo de WINUTILS.EXE.

1. En la barra de menús, vaya a **Ejecutar** > **Editar configuraciones...**

2. En la ventana **Run/Debug Configurations** (Ejecutar o depurar configuraciones), en el panel izquierdo, vaya a **Apache Spark on SQL Server big data cluster** >  **[Spark on SQL] myApp** (Apache Spark en clúster de macrodatos de SQL Server - [Spark en SQL] myApp).

3. En la ventana principal, seleccione la pestaña **Ejecutar de forma local**.

4. Proporcione los valores siguientes y seleccione **Aceptar**:

    |Propiedad |Value |
    |----|----|
    |Clase principal del trabajo|el valor predeterminado es la clase principal del archivo seleccionado. Puede cambiar la clase si selecciona los puntos suspensivos ( **...** ) y elige otra clase.|
    |Variables de entorno|Asegúrese de que el valor de HADOOP_HOME sea correcto.|
    |Ubicación de WINUTILS.exe|Asegúrese de que la ruta de acceso sea correcta.|

    ![Configuración de conjunto de consola local](./media/spark-submit-job-intellij-tool-plugin/console-set-configuration.png)

5. En el proyecto, vaya a **myApp** > **src** > **main** > **scala** > **myApp**.  

6. En la barra de menús, vaya a **Herramientas** > **Consola de Spark** > **Run Spark Local Console(Scala)** (Ejecutar consola local de Spark (Scala)).

7. Pueden aparecer dos cuadros de diálogo para preguntarle si quiere corregir automáticamente las dependencias. Si es así, seleccione **Autocorrección**.

    ![Autocorrección 1 de Spark](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix1.png)

    ![Autocorrección 2 de Spark](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix2.png)

8. El aspecto de la consola debería ser similar al de la siguiente imagen. En la ventana de la consola, escriba `sc.appName` y presione CTRL + Entrar.  Se muestra el resultado. Puede cerrar la consola local si hace clic en el botón rojo.

    ![Resultado de la consola local](./media/spark-submit-job-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Consola de sesión interactiva de Spark Livy (Scala)
La consola de sesión interactiva de Spark Livy (Scala) solo se admite en IntelliJ 2018.2 y 2018.3.

1. En la barra de menús, vaya a **Ejecutar** > **Editar configuraciones...**

2. En la ventana **Run/Debug Configurations** (Ejecutar o depurar configuraciones), en el panel izquierdo, vaya a **Apache Spark on SQL Server big data cluster** >  **[Spark on SQL] myApp** (Apache Spark en clúster de macrodatos de SQL Server - [Spark en SQL] myApp).

3. En la ventana principal, seleccione la pestaña **Remotely Run in Cluster** (Ejecutar en clúster de forma remota).

4. Proporcione los valores siguientes y seleccione **Aceptar**:

    |Propiedad |Value |
    |----|----|
    |Clústeres de Spark (solo Linux)|Seleccione el clúster de macrodatos de SQL Server en el que quiere ejecutar la aplicación.|
    |Nombre de clase principal|el valor predeterminado es la clase principal del archivo seleccionado. Puede cambiar la clase si selecciona los puntos suspensivos ( **...** ) y elige otra clase.|

    ![Configuración de conjunto de consola interactiva](./media/spark-submit-job-intellij-tool-plugin/interactive-console-configuration.png)

5. En el proyecto, vaya a **myApp** > **src** > **main** > **scala** > **myApp**.  

6. En la barra de menús, vaya a **Herramientas** > **Consola de Spark** > **Run Spark Livy Interactive Session Console(Scala)** (Ejecutar consola interactiva de Spark Livy (Scala)).

7. El aspecto de la consola debería ser similar al de la siguiente imagen. En la ventana de la consola, escriba `sc.appName` y presione CTRL + Entrar.  Se muestra el resultado. Puede cerrar la consola local si hace clic en el botón rojo.

    ![Resultado de consola interactiva](./media/spark-submit-job-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Enviar selección a consola de Spark

Por comodidad, puede ver el resultado del script si envía algún código a la consola local o la consola de sesión interactiva de Livy (Scala). Puede resaltar parte del código en el archivo de Scala y hacer clic con el botón derecho en **Send Selection To Spark Console** (Enviar selección a consola de Spark). El código seleccionado se envía a la consola y se ejecuta. El resultado se muestra después del código en la consola. La consola comprueba si hay errores.  

   ![Enviar selección a consola de Spark](./media/spark-submit-job-intellij-tool-plugin/send-selection-to-console.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre los clústeres de macrodatos de SQL Server y los escenarios relacionados, vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)?