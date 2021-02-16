---
title: Configuración del proyecto de base de datos
description: Obtenga información sobre la configuración de proyectos de base de datos. Vea cómo usar estos valores para controlar aspectos de las configuraciones de base de datos, depuración y compilación.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DebugProperties
- sql.data.tools.dacsettings.dialog
- sql.data.tools.dbsqlclrlanguagevb
- SQL.DATA.TOOLS.DBADVANCEDBUILDSETTINGSCS
- sql.data.tools.dbadvancedbuildsettingscs
- ql.data.tools.criticalerror.dialog
- sql.data.tools.dbassemblysigningchangekeypassword
- sql.data.tools.advanceddeploymentsystemdefined.dialog
- sql.data.tools.advanceddatabasesettings.dialog
- sql.data.tools.extendedpropertiesvalueeditor.dialog
- sql.data.tools.dbsqlclrlanguagecs
- ql.data.tools.dbassemblysigningpasswordneeded
- sql.data.tools.vbadvancedsettings.dialog
- sql.data.tools.dbsqlclr
- sql.data.tools.SqlCmdVariablesProperties
- sql.data.tools.dbassemblysigning
- sql.data.tools.advanceddeploymentsettings.dialog
- sql.data.tools.advancedpublishsettings.dialog
- sql.data.tools.BuildActionProperties
- sql.data.tools.serverselectionpolicy.dialog
- sql.data.tools.dbadvancedbuildsettingsvb
- sql.data.tools.dbprojectwizard.buildeventcommandline
- sql.data.tools.dbreferencepaths
- sql.data.tools.GeneralProperties
- sql.data.tools.BuildProperties
- sql.data.tools.DeployProperties
- sql.data.tools.csadvancedsettings.dialog
- sql.data.tools.dbassemblyinfo
- sql.data.tools.extendedpropertieseditor.dialog
ms.assetid: 34418730-1aaa-4948-aee2-8f1e62cda85c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 99552883fb06c01a862ddc973ff3747d11a89ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079556"
---
# <a name="database-project-settings"></a>Configuración del proyecto de base de datos

Utilice la configuración del proyecto de base de datos para controlar aspectos de las configuraciones de base de datos, depuración y compilación. Existen varias categorías de configuraciones.  
  
-   [Configuración del proyecto](#bkmk_proj_settings)  
  
-   [Comprobación extendida de Transact-SQL](#bkmk_evf)  
  
-   [SQLCLR](#bkmk_sqlclr)  
  
-   [SQLCLR y Compilación de SQLCLR](#bkmk_sqlclr_sqlclrbuild)  
  
-   [Compilar](#bkmk_build)  
  
-   [Variables SQLCMD](#bkmk_sqlcmd_variables)  
  
-   [Eventos de compilación](#bkmk_build_events)  
  
-   [Depuración](#bkmk_debug)  
  
-   [Rutas de acceso de referencia](#bkmk_ref_paths)  
  
-   [Análisis de código](#bkmk_code_analysis)  
  
### <a name="to-configure-properties-for-your-database-project"></a>Para configurar las propiedades del proyecto de base de datos  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto de base de datos para el que desea configurar las propiedades y seleccione **Propiedades**.  
  
    También puede hacer doble clic en el nodo **Propiedades** del proyecto en el **Explorador de soluciones**.  
  
2.  Aparecerá la hoja de propiedades para el proyecto de base de datos.  
  
3.  Haga clic en la ficha **Configuración del proyecto** . Ahora puede configurar las propiedades generales de las propiedades del proyecto de base de datos. Tenga en cuenta la disponibilidad de las diversas pestañas (que representan distintas categorías) en el panel de la izquierda.  
  
## <a name="project-settings"></a><a name="bkmk_proj_settings"></a>Configuración del proyecto  
Los valores de configuración de la siguiente tabla se aplican a todas las configuraciones de este proyecto de base de datos.  
  
|Campo|Valor predeterminado|Descripción|  
|---------|-----------------|---------------|  
|Plataforma de destino|Microsoft SQL Server 2012|Especifica la versión de SQL Server de destino para este proyecto de base de datos.|  
|Habilitar la comprobación Transact\-SQL extendida para objetos comunes.|No está habilitado cuando se crea un proyecto nuevo.<br /><br />Se habilita al crear un proyecto desde el Explorador de objetos de SQL Server conectado a SQL Azure, importar una base de datos de SQL Azure en el proyecto o cambiar la plataforma de destino de un proyecto a SQL Azure.|Cuando esta opción está habilitada, se notifican los errores encontrados en el proyecto que no pasó la comprobación del Compilador de SQL Server. Si cambia la plataforma de destino a SQL Azure, la comprobación extendida se habilita. La opción no se desactivará si cambia la plataforma de destino.<br /><br />Puede habilitar esta opción para otras versiones de SQL Server, pero la validación se limita a las bases de datos parcialmente independientes de Microsoft SQL Server 2012 y SQL Azure. No toda la sintaxis de Transact\-SQL es compatible con todas las extensiones de SQL Server.<br /><br />Para información más detallada, consulte [Comprobación extendida de Transact-SQL](#bkmk_evf) más adelante en este tema|  
|Tipos de salida|||  
|Aplicación de capa de datos (archivo .dacpac)|Habilitado y bloqueado. El resultado de la compilación de un proyecto de base de datos siempre produce un paquete .dacpac cuando se compila el proyecto.|Si va a usar la versión de SQL Server Data Tools (SSDT) que tiene la opción "Crear archivo .dacpac adicional de bajo nivel (v2.0)", actívela si desea que el paquete sea compatible con SQL Server Management Studio o con el Portal de administración de SQL Azure. Puede implementar un paquete .dacpac directamente desde (SSDT), pero solo se puede implementar un archivo de .dacpac versión 2.0 a través de SQL Server Management Studio en el momento en que SQL Server Data Tools se libera.|  
|Script de creación (archivo .sql)||Especifica si se generará un script CREATE .sql completo para todos los objetos del proyecto y se colocará en la carpeta bin\debug cuando se compile el proyecto. Puede crear un script de actualización incremental mediante el comando **Publicar proyecto** o mediante la utilidad de comparación de SQL.|  
|Genérico|||  
|Esquema predeterminado|dbo|Especifica el esquema predeterminado en el que se crean tanto los objetos SQLCLR como los objetos Transact\-SQL. Si desea invalidar este valor, especifique el esquema directamente en los objetos."|  
|Incluir nombre de esquema en nombre de archivo|no|Especifica si los nombres de archivo incluyen el esquema como prefijo (por ejemplo, dbo.Products.table.sql). Si se desactiva esta casilla, los nombres de archivo para los objetos adoptan el formato nombreDeObjeto.tipoDeObjeto.sql (por ejemplo, Products.table.sql).|  
|Validar el uso de mayúsculas y minúsculas en identificadores|sí|Especifica si se validará el uso de mayúsculas y minúsculas en identificadores en los objetos SQL del proyecto durante la compilación del proyecto. Esta opción se aplica a los proyectos de base de datos que especifican una intercalación de la base de datos que distingue mayúsculas de minúsculas.|  
|Database Settings|Configuración predeterminada basada en la configuración estándar para una base de datos|Entre las configuraciones que se pueden especificar se incluye el método de intercalación y el nivel de una base de datos de SQL Server.|  
  
## <a name="extended-transact-sql-verification"></a><a name="bkmk_evf"></a>Comprobación extendida de Transact-SQL  
  
> [!IMPORTANT]  
> La función Comprobación de Transact-SQL extendida se eliminará de la próxima versión de SQL Server Data Tools y la próxima versión principal de Visual Studio.  
  
La comprobación extendida de Transact-SQL es una característica dentro del sistema del proyecto de base de datos que permite a los desarrolladores enviar su proyecto de base de datos al Servicio Compilador de Transact-SQL en tiempo de compilación para validar su código con el analizador y el intérprete del Motor de SQL Server.  
  
### <a name="transact-sql-compiler-service"></a>Servicio Compilador de Transact-SQL  
Transact-SQL Compiler Service es un componente basado en el Motor de base de datos de Microsoft SQL Server 2012. Este servicio puede validar la sintaxis y la semántica de las instrucciones DDL con la misma fidelidad que un motor de base de datos de Microsoft SQL Server 2012. Esto, de modo inherente, significa que el Servicio compilador no admite la sintaxis ni las características que hayan dejado de usarse en Microsoft SQL Server 2012. Para obtener más información sobre las características en desuso, vea [Funcionalidad del motor de base de datos no incluida en SQL Server 2012](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).  
  
Para validar el proyecto de base de datos, el Servicio Compilador crea una base de datos parcialmente independiente y simula la ejecución de instrucciones DDL con dicha base de datos. Para obtener más información, vea [Bases de datos parcialmente independientes](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
El Servicio Compilador tiene dos categorías de limitaciones.  
  
Las características que se basan en la configuración de la instancia o la base de datos como son:  
  
-   Referencias a objetos de tres o cuatro partes  
  
-   FileTables  
  
-   Seguimiento de cambios  
  
-   Funciones Rowset - OPENROWSET, OPENQUERY, OPENDATASOURCE  
  
-   Búsqueda semántica de texto completo  
  
Las características que no se admiten para la validación en este momento, como son:  
  
-   Service Broker  
  
-   Los esquemas con particiones con FileGroups definidos por el usuario  
  
-   Intercalación de metadatos de SQL Azure (el Servicio Compilador usa la intercalación de metadatos Base de datos parcialmente independiente de SQL Server 2012: Latin1_General_100_CI_AS_KS_WS_SC)  
  
### <a name="enablingdisabling-extended-verification"></a>Habilitar o deshabilitar la comprobación extendida de Transact-SQL  
La comprobación extendida de Transact-SQL se habilita de forma predeterminada en un proyecto de base de datos que se crea directamente desde un proyecto o una base de datos de SQL Azure cuya plataforma de destino se establece en SQL Azure. Se recomienda que la comprobación extendida se use en el desarrollo con SQL Azure o de una base de datos de ámbito de aplicación destinada a SQL Server 2012. Para obtener más información acerca de las bases de datos de ámbito de aplicación, vea [Bases de datos parcialmente independientes](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
La característica de comprobación extendida puede usarse también al desarrollar una base de datos de ámbito de aplicación para SQL Server 2008/R2 a fin de lograr la compatibilidad con Microsoft SQL Server 2012 y SQL Azure.  
  
##### <a name="to-enable-or-disable-extended-verification-at-the-project-level"></a>Para habilitar o deshabilitar la comprobación extendida en el nivel de proyecto  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el archivo de proyecto y, a continuación, haga clic en **Propiedades**.  
  
2.  En **Configuración de proyecto**, en **Plataforma de destino**, active o desactive **Habilitar la comprobación extendida de Transact-SQL para objetos comunes**.  
  
##### <a name="to-disable-extended-verification-at-the-file-level"></a>Para deshabilitar la comprobación extendida en el nivel de archivo  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en un archivo .sql.  
  
    > [!NOTE]  
    > Para deshabilitar la característica Comprobación extendida de Transact\-SQL en el nivel de archivo, la propiedad **Acción de compilación** debe establecerse en **Compilar**.  
  
2.  En **Propiedades**, cambie la propiedad **Extended T-SQL Verification** (Comprobación extendida de T-SQL) a **False**.  
  
    ![Propiedades del archivo](../ssdt/media/ssdt-evf.gif "Propiedades del archivo")  
  
### <a name="special-considerations-for-collations"></a>Consideraciones especiales para las intercalaciones  
Para obtener más información acerca de las intercalaciones en las bases de datos parcialmente independientes, vea [Intercalaciones en bases de datos independientes](/previous-versions/sql/sql-server-2012/ff929080(v=sql.110)).  
  
## <a name="sqlclr"></a><a name="bkmk_sqlclr"></a>SQLCLR  
Para obtener información sobre las opciones de Ensamblado, vea [Cuadro de diálogo Información del ensamblado](/visualstudio/ide/reference/assembly-information-dialog-box?queryresult=true).  
  
Para obtener más información sobre la firma, vea la sección **Firma de ensamblados** del tema [Página Firma, Diseñador de proyectos](/visualstudio/ide/reference/signing-page-project-designer?queryresult=true) .  
  
## <a name="sqlclr-and-sqlclr-build"></a><a name="bkmk_sqlclr_sqlclrbuild"></a>SQLCLR y Compilación de SQLCLR  
Las páginas de propiedades **SQLCLR** y **Compilación de SQLCLR** contienen muchas configuraciones para usar objetos CLR de SQL en el proyecto. En concreto, la página de propiedades **SQLCLR** tiene una configuración de nivel de permisos para establecer permisos en el ensamblado SQLCLR. También tiene una configuración “Generar DDL” para controlar si se genera el archivo DDL para los objetos SQLCLR que se han agregado al proyecto. La página de propiedades **Compilación de SQLCLR** contiene todas las opciones de compilador que puede establecer para configurar la compilación de código SQLCLR en el proyecto.  
  
La página de propiedades **Compilación de SQLCLR** contiene configuraciones de compilación avanzadas para compilar objetos CLR de SQL. Se ofrecen distintas opciones según el lenguaje (VB o C#) que se vaya a utilizar para codificar los objetos CLR de SQL.  
  
1.  Si el objeto se ha escrito en C#, puede tener acceso a las opciones si hace clic en el botón **Avanzadas** en la página de propiedades **Compilación de SQLCLR**. Podrá encontrar descripciones de las opciones de C# en el [Cuadro de diálogo Configuración de compilación avanzada (C#)](/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp).  
  
2.  Si el objeto se ha escrito en VB, puede elegir VB en primer lugar en la lista desplegable **Lenguaje** y, a continuación, hacer clic en el botón **Avanzadas** . Podrá encontrar descripciones de las opciones de VB en [Configuración de compilador avanzada (Cuadro de diálogo, Visual Basic)](/visualstudio/ide/reference/advanced-compiler-settings-dialog-box-visual-basic)  
  

## <a name="build"></a><a name="bkmk_build"></a>Compilación  
Puede elegir una configuración de compilación para cada proyecto de base de datos de la solución. De forma predeterminada, existe una sola configuración, pero puede agregar configuraciones personalizadas. Puede optar por esta opción si desea, por ejemplo, una configuración personalizada en la que siempre elimina y vuelve a crear la base de datos. En soluciones que contienen distintos tipos de proyecto, puede crear una configuración de solución personalizada que contenga una configuración de compilación determinada para cada proyecto.  
  
#### <a name="to-specify-a-build-configuration-for-a-solution"></a>Para especificar una configuración de compilación para una solución  
  
1.  En el **Explorador de soluciones**, haga clic en el nodo de solución para el que desea especificar una configuración de compilación.  
  
2.  En el menú **Compilar**, haga clic en **Administrador de configuración**. Aparecerá el cuadro de diálogo **Administrador de configuración**.  
  
    Especifique los valores de configuración que vaya a utilizar para cada proyecto de la solución.  
  
#### <a name="to-specify-a-build-configuration-for-a-database-project"></a>Para especificar una configuración de compilación para un proyecto de base de datos  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto de base de datos para el que desea especificar una configuración de compilación y a continuación, seleccione **Propiedades**.  
  
2.  En la ficha **Compilar** , use la lista desplegable **Configuración** para especificar las opciones de configuración que desea usar para este proyecto.  
  
Los valores de configuración de la siguiente tabla se aplican a todas las configuraciones de compilación de este proyecto de base de datos.  
  
|Campo|Valor predeterminado|Descripción|  
|---------|-----------------|---------------|  
|Ruta de acceso de salida de la compilación|bin\Debug\|Especifica dónde se creará la salida de la compilación al compilar o implementar el proyecto de base de datos. Si especifica una ruta de acceso relativa, deberá ser relativa a la ruta de acceso al proyecto de base de datos. Si la ruta de acceso no existe, se creará.|  
|Nombre de archivo de salida de la compilación|*DatabaseProjectName*|Especifica el nombre que desea asignar a la salida generada al compilar el proyecto de base de datos.|  
|Tratar advertencias de Transact\-SQL como errores|No|Especifica si una advertencia de Transact\-SQL debe provocar la cancelación de los procesos de compilación e implementación. Si se desactiva esta casilla, aparecen mensajes de advertencia, pero el proceso de compilación e implementación continúa. Esta configuración es específica del proyecto, no del usuario, y se almacena en el archivo .sqlproj.|  
|Suprimir las advertencias de Transact\-SQL|En blanco|Especifica una lista de números de advertencia, separados por coma o punto y coma, que identifican las advertencias que se suprimen.<br /><br />Las advertencias suprimidas no aparecen en la ventana **Lista de errores** ni afectan a la correcta compilación, incluso cuando se activa la casilla **Tratar advertencias de Transact\-SQL como errores**.|  
  
## <a name="sqlcmd-variables"></a><a name="bkmk_sqlcmd_variables"></a>Variables SQLCMD  
En los proyectos de base de datos de SQL Server puede utilizar variables SQLCMD para proporcionar sustitución dinámica que se usará con fines de depuración o publicación. Tiene que especificar los nombres y los valores de las variables, y durante la compilación se sustituirán los valores. Si no hay ningún valor local, se usará el valor predeterminado. Si estas variables se especifican en las propiedades del proyecto, se ofrecerán automáticamente en la publicación y se almacenarán en los perfiles de publicación. Puede insertar los valores de proyecto de las variables en la publicación mediante el botón Cargar valores.  
  
Asegúrese de especificar las variables adecuadas en las propiedades del proyecto, ya que estas variables no se validan con ningún script del proyecto ni se rellenan automáticamente.  
  
Además, la publicación desde la línea de comandos le permite invalidar estos valores en la línea de comandos o mediante un perfil.  
  
## <a name="build-events"></a><a name="bkmk_build_events"></a>Eventos de compilación  
Puede usar estos valores para especificar una línea de comandos que se debe ejecutar antes de que se inicie la operación de compilación y otra línea de comandos que se debe ejecutar una vez finalizada dicha operación.  
  
|Campo|Valor predeterminado|Descripción|  
|---------|-----------------|---------------|  
|Línea de comandos del evento anterior a la compilación|None|Especifica la línea de comandos que se ejecutará antes de que se compile el proyecto. Haga clic en **Edición anterior a la compilación** para modificar la línea de comandos.|  
|Línea de comandos del evento posterior a la compilación|None|Especifica la línea de comandos que se ejecutará después de que se compile el proyecto. Haga clic en **Edición posterior a la compilación** para modificar la línea de comandos.|  
|Ejecutar el evento posterior a la compilación|Si la compilación es correcta|Especifica si la línea de comandos posterior a la generación se debería ejecutar siempre, sólo si la generación ha sido correcta o sólo si la generación ha actualizado el resultado del proyecto (el script de compilación).|  
  
## <a name="debug"></a><a name="bkmk_debug"></a>Depuración  
Puede utilizar esta configuración para controlar la depuración del proyecto de base de datos.  
  
|Campo|Valor predeterminado|Descripción|  
|---------|-----------------|---------------|  
|Acción de inicio|None|Especifica un script o un programa externo que se ejecutará al depurar el proyecto.|  
|Cadena de conexión de destino|Data Source=(localdb)\\*SolutionName*;Initial Catalog=*DatabaseProjectName*;Integrated Security=True;Pooling=False;Connect Timeout=30|Especifica la información de conexión del servidor de bases de datos que desea que sea el destino de la configuración de compilación especificada. La cadena de conexión predeterminada corresponde a una instancia de LocalDB y una base de datos de SQL Server creadas dinámicamente.|  
|Implementar propiedades de base de datos|Sí|Especifica si se implementa o se actualiza la configuración de DatabaseProperties.DatabaseProperties al implementar el proyecto de base de datos.|  
|Volver a crear siempre la base de datos|No|Especifica si se va a quitar y volver a crear la base de datos en lugar de realizar una actualización incremental. Seleccione esta casilla de verificación si desea ejecutar las pruebas unitarias de base de datos en una implementación limpia de la base de datos, por ejemplo. Si desactiva esta casilla, la base de datos existente se actualizará en lugar de eliminarse y volver a crearse.|  
|Bloquear implementación incremental si puede dar lugar a pérdida de datos|Sí|Especifica si la implementación debe detenerse si una actualización puede producir pérdida de datos. Si esta casilla está activada, los cambios que provocarían la pérdida de datos detendrían la implementación con un error, lo que impediría que se perdiesen los datos. Por ejemplo, la implementación se detendría si una columna `varchar(50)` se hubiera cambiado a `varchar(30)`.<br /><br />**NOTA:** La implementación solo se bloquea si las tablas en las que puede producirse pérdida de datos contienen datos. La implementación continúa si no se pierde ningún dato.|  
|Objetos DROP en destino pero no en proyecto|No|Especifica si los objetos que están en la base de datos de destino pero no en el proyecto de base de datos se deben eliminar del script de implementación. Esto permite excluir algunos archivos del proyecto y quitarlos temporalmente del script de compilación. Sin embargo, es posible que le interese mantener las versiones existentes de estos objetos en la base de datos de destino. Esta casilla de verificación no tiene ningún efecto si la casilla de verificación **Volver a crear siempre la base de datos** está seleccionada, porque se quitará la base de datos.|  
|No usar instrucciones ALTER ASSEMBLY para actualizar tipos CLR|No|Especifica si deben usarse instrucciones ALTER ASSEMBLY para actualizar tipos CLR (Common Language Runtime) o si el objeto que crea la instancia del tipo CLR se va a quitar y volver a generar al implementar los cambios.|  
|Avanzadas...|No|Botón de comando que permite especificar opciones que controlan eventos y el comportamiento de la implementación.|  
  
## <a name="reference-paths"></a><a name="bkmk_ref_paths"></a>Rutas de acceso de referencia  
Puede utilizar esta página para definir el servidor y las variables de la base de datos asociadas a referencias de bases de datos cruzadas. Además, puede especificar los valores de esas variables. Para obtener más información, consulte [Uso de referencias en proyectos de base de datos](/previous-versions/visualstudio/visual-studio-2010/bb386242(v=vs.100)).  
  
## <a name="code-analysis"></a><a name="bkmk_code_analysis"></a>Análisis de código  
Puede usar Análisis de código para detectar posibles problemas en los scripts, como problemas de diseño, nomenclatura y rendimiento. Las reglas para los proyectos de base de datos están organizadas en conjuntos de reglas predefinidos dirigidos a determinadas áreas y puede habilitar o deshabilitar cualquier regla en la pestaña **Análisis de código** de la página **Propiedades del proyecto** . En la misma pestaña, puede especificar que el análisis de código se ejecute automáticamente cada vez que se compile un proyecto o si las advertencias deben tratarse como errores.  
  
Para usar Análisis de código manualmente, haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Ejecutar análisis de código**. Las advertencias de análisis de código se mostrarán en la ventana **Lista de errores** . Puede hacer doble clic en una advertencia para navegar hasta el código fuente que contiene el problema, y puede ver información adicional y posibles correcciones de una advertencia usando el menú contextual **Ayuda para Mostrar mensaje**. Para obtener más información sobre Análisis de código, vea [Analizar el código de base de datos para mejorar la calidad del código](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
