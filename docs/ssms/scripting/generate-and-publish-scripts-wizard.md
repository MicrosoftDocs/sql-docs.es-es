---
title: Asistente generar y publicar scripts
description: Aprenda a usar el Asistente para generar y publicar scripts para crear scripts con el fin de transferir una base de datos entre instancias de base de datos. Las instancias pueden ser instancias del Motor de base de datos de SQL Server o Azure SQL Database.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- sql9.swb.generatescriptswizard.chooseviews.f1
- sql13.swb.generatescriptswizard.manageproviders.f1
- sql9.swb.generatescriptswizard.scriptwizarddescription.f1
- sql9.swb.generatescriptswizard.choosedefaults.f1
- sql13.swb.generatescriptswizard.summarypage.f1
- sql13.swb.generatescriptswizard.providerconfiguration.f1
- sql9.swb.generatescriptswizard.chooseuddt.f1
- sql9.swb.generatescriptswizard.chooserules.f1
- sql13.swb.generatescriptswizard.introduction.f1
- sql13.swb.generatescriptswizard.setscriptingoptions.f1
- sql13.swb.generatescriptswizard.saveorpublishscripts.f1
- sql9.swb.generatescriptswizard.progress.f1
- sql9.swb.generatescriptswizard.chooseobjects.f1
- sql9.swb.generatescriptswizard.welcome.f1
- sql9.swb.generatescriptswizard.scriptfileoption.f1
- sql13.swb.generatescriptswizard.chooseobjects.f1
- sql13.swb.generatescriptswizard.advancedpublishingoptions.f1
- sql9.swb.generatescriptswizard.selectdatabase.f1
- sql9.swb.generatescriptswizard.choosetables.f1
- sql9.swb.generatescriptswizard.choosestoredprocedures.f1
- sql9.swb.generatescriptswizard.chooseobjecttypes.f1
- sql13.swb.generatescriptswizard.advancedscriptingoptions.f1
- sql9.swb.generatescriptswizard.choosescriptoptions.f1
- sql9.swb.generatescriptswizard.chooseudf.f1
helpviewer_keywords:
- databases [SQL Server], publishing
- publishing databases
- scripts [SQL Server], generating
- scripts [SQL Server], publishing
- databases [SQL Server], generating scripts
- Publish Database Wizard
ms.assetid: 5ee520ba-ec7e-4199-a441-189e9e264b37
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 04/07/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7a24570538161a8b0196e9c7d8ee20af9e32587
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271634"
---
# <a name="generate-and-publish-scripts-wizard"></a>Asistente generar y publicar scripts

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Puede usar el **Asistente Generar y publicar scripts** para crear scripts con el fin de transferir una base de datos entre instancias de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] o [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Puede generar scripts para una base de datos en una instancia del motor de base de datos en la red local o a partir de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Los scripts generados se pueden ejecutar en otra instancia del motor de base de datos o [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. También puede usar el asistente para publicar el contenido de una base de datos directamente en un servicio web creado usando Database Publishing Services. Es posible crear scripts para una base de datos completa o limitarlos a objetos específicos.

Para un tutorial más detallado sobre cómo usar el Asistente para generar y publicar scripts, consulte [Tutorial: Asistente para generar scripts](../tutorials/scripting-ssms.md#script-databases).

## <a name="before-you-begin"></a>Antes de empezar

Las bases de datos de origen y de destino pueden estar en [!INCLUDE[ssSDS](../../includes/sssds-md.md)]o en una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] que ejecuta [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versión posterior.

### <a name="publishing-to-a-hosted-service"></a><a name="PubHostSvc"></a> Publicar en un servicio hospedado

Además de crear scripts, el **Asistente Generar y publicar scripts** se puede usar para publicar una base de datos en un tipo específico de servicio web hospedado de SQL Server. El SQL Server Hosting Toolkit proporciona Database Publishing Services como un proyecto de origen compartido en CodePlex. Los proveedores del hospedaje web pueden usar el proyecto Database Publishing Services para generar un conjunto de servicios web que faciliten a sus clientes la implementación de bases de datos en el servicio web. Para obtener más información sobre cómo descargar el SQL Server Hosting Toolkit, vea [SQL Server Database Publishing Services](https://go.microsoft.com/fwlink/?LinkId=142025).

Para publicar una base de datos a un servicio de hospedaje web, seleccione la opción **Publicar en servicio web** en la página **Establecer opciones de scripting** del asistente.

### <a name="permissions"></a><a name="Permissions"></a> Permisos

El permiso mínimo para publicar una base de datos es la pertenencia al rol fijo de base de datos db_ddladmin en la base de datos de origen. El permiso mínimo para publicar un script de base de datos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el proveedor de hospedaje es la pertenencia al rol fijo de base de datos db_ddladmin en la base de datos de destino.

Para publicar con el asistente, el usuario también debe proporcionar un nombre de usuario y una contraseña para tener acceso a su cuenta en el proveedor de hospedaje. La base de datos destino se debe crear en el proveedor del hospedaje antes de que la base de datos de origen se publique. Al publicar, se sobrescriben los objetos presentes en la base de datos.

## <a name="using-the-generate-and-publish-scripts-wizard"></a><a name="GenPubScriptWiz"></a> Usar el Asistente Generar y publicar scripts

**Para generar y publicar un script**

1. En **Explorador de objetos**, expanda el nodo de la instancia que contiene la base de datos que se va a incluir en el script.

2. Seleccione **Tareas** y, a continuación, seleccione **Generar scripts**.

    ![Asistente para generar scripts](media/generate-and-publish-scripts-wizard/generate-scripts.png)

3. Complete los cuadros de diálogo del asistente:

    - [Página Introducción](#Introduction)
    - [Página Elegir objetos](#ChooseObjects)
    - [Página Establecer opciones de scripting](#SetScriptOpt)
    - [Página Opciones de scripting avanzadas](#AdvScriptOpt)
    - [Página Resumen](#Summary)
    - [Página Guardar o publicar scripts](#SavePubScripts)

###  <a name="introduction-page"></a><a name="Introduction"></a> Página Introducción

Esta página describe los pasos para generar o publicar un script.

**No volver a mostrar esta página** : omite esta página la próxima vez que inicie el **asistente Generar y publicar scripts**.

  ![Página Introducción](media/generate-and-publish-scripts-wizard/intro.png)

### <a name="choose-objects-page"></a><a name="ChooseObjects"></a> Página Elegir objetos

Use esta página para elegir los objetos que desea incluir en los scripts generados por el asistente. En la siguiente página del asistente, puede guardar estos scripts en la ubicación que elija o usarlos para publicar objetos de base de datos en un proveedor de hospedaje web remoto que tenga instalado [SQL Server Database Publishing Services](https://go.microsoft.com/fwlink/?LinkId=142025).

**Opción de incluir en el script toda la base de datos**: seleccione esta opción para generar scripts para todos los objetos de la base de datos e incluir un script para la propia base de datos.

   ![Crear scripts de todas las bases de datos](media/generate-and-publish-scripts-wizard/script-all.png)

**Seleccionar objetos de base de datos específicos**: seleccione esta opción para limitar el asistente con el fin de que genere scripts solo para los objetos concretos de la base de datos que elija:

- **Objetos de base de datos** : seleccione al menos un objeto para incluirlo en el script.

- **Seleccionar todo** : activa todas las casillas disponibles.

- **Anular la selección** : desactiva todas las casillas. Para poder continuar, deberá seleccionar al menos un objeto de base de datos.

   ![Específico de la creación de scripts](media/generate-and-publish-scripts-wizard/script-specific-objects.png)

### <a name="set-scripting-options-page"></a><a name="SetScriptOpt"></a> Página Establecer opciones de scripting

Use esta página para especificar si desea que el asistente guarde los scripts en la ubicación que elija o usarlos para publicar objetos de base de datos en un proveedor de hospedaje web remoto. Para publicar, debe tener acceso a un servicio web que se haya instalado mediante el servicio web Database Publishing Services Web.

**Opciones** : si quiere que el asistente guarde los scripts en la ubicación que elija, seleccione **Guardar scripts en una ubicación específica**. Posteriormente, podrá ejecutar los scripts con respecto a una instancia del motor de base de datos o [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Si desea que el asistente publique los objetos de base de datos en un proveedor de hospedaje web remoto, seleccione **Publicar en servicio web**.

**Guardar scripts en una ubicación específica**: guarda uno o varios archivos de script Transact-SQL en una ubicación que especifique.

![Guardar como cuaderno](media/generate-and-publish-scripts-wizard/save.png)

- **[Guardar como cuaderno](../../azure-data-studio/notebooks/notebooks-guidance.md)** : guarde el script en uno o varios archivos .sql. Seleccione el botón Examinar ( **…** ) para especificar el nombre y la ubicación del archivo.

- **Guardar como archivo de script**: guarde el script en uno o varios archivos .sql. Seleccione el botón Examinar ( **…** ) para especificar el nombre y la ubicación del archivo. Active la casilla **Sobrescribir el archivo existente** para reemplazar el archivo si ya existe uno con el mismo nombre. Seleccione la opción de un **archivo de script único** o un **archivo de script por objeto** para especificar cómo se deben generar los scripts. Seleccione **Texto Unicode** o **Texto ANSI** para especificar el tipo de texto que se debe usar en el script.

- **Guardar en el Portapapeles** : guarda el script Transact-SQL en el Portapapeles.

- **Abrir en nueva ventana de consulta**: genera el script en una ventana del editor de consultas del motor de base de datos. Si no hay ninguna ventana de editor abierta, se abre una nueva ventana como destino del script.

- **Opciones avanzadas** : se muestra el cuadro de diálogo **Opciones de publicación avanzadas** , donde puede seleccionar las opciones avanzadas para publicar scripts.

- **Proveedor** : seleccione el proveedor que especifique la información de conexión para el servicio de hospedaje web para la base de datos donde quiere publicar los objetos que ha seleccionado. Debe haber al menos un proveedor en el cuadro de diálogo **Proveedores administrados** para seleccionar un proveedor.

- **Base de datos de destino** : seleccione la base de datos de destino donde quiere publicar los objetos que ha seleccionado. Debe seleccionar un proveedor antes de seleccionar una base de datos de destino.

### <a name="advanced-scripting-options-page"></a><a name="AdvScriptOpt"></a> Página Opciones de scripting avanzadas

Use esta página para especificar cómo desea que este asistente genere los scripts. Hay disponibles numerosas opciones. Las opciones se atenúan si no se admiten en la versión de SQL Server o [!INCLUDE[ssSDS](../../includes/sssds-md.md)] especificada en **Tipo de motor de base de datos**.

![Opciones avanzadas](media/generate-and-publish-scripts-wizard/advanced.png)

**Opciones** : para especificar las opciones avanzadas, seleccione un valor de la lista de opciones de configuración disponibles, situada a la derecha de cada opción.

**General**: las opciones siguientes se aplican a todo el script.

- **Relleno ANSI** : incluye **ANSI PADDING ON** en el script. El valor predeterminado es **True**.

- **Anexar a archivo** : si es **True**, este script se agrega al final de un script existente, especificado en la página **Establecer opciones de scripting** . Si es **False**, el nuevo script sobrescribe un script anterior. El valor predeterminado es **False**.

- **Comprobar la existencia de objetos**: si es **true**, agrega la comprobación de existencia antes de generar la instrucción CREATE de los objetos SQL (por ejemplo, tablas, vistas, funciones o procedimientos almacenados). La instrucción CREATE se encapsula en una instrucción IF. Si sabe que el destino está limpio, el script será mucho más limpio. Si NO espera que existan objetos en el destino, obtendrá un error. El valor predeterminado es **False**.

- **Continuar scripting en caso de error**: si es **false**, el script se detendrá si se produce un error. Si es **true**, el scripting continuará. El valor predeterminado es **False**.

- **Convertir UDDT en tipos base** : si es **True**, los tipos de datos definidos por el usuario (UDDT) se convierten en los tipos de datos base subyacentes que se usaron para crearlos. Use **True** cuando el UDDT no exista en la base de datos en la que se ejecuta el script. Si es **False**, se usan los UDDT. El valor predeterminado es **False**.

- **Generar script para objetos dependientes** : genera un script para cualquier objeto que deba estar presente cuando se ejecute el script para el objeto seleccionado. El valor predeterminado es **True**.

- **Incluir encabezados descriptivos** : si es **True**, se agregarán comentarios descriptivos al script, que lo separarán en secciones para cada objeto. El valor predeterminado es **False**.

- **Incluir IF NOT EXISTS** : si es **True**, el script incluirá una instrucción para comprobar si el objeto ya existe en la base de datos y no intentará crear un nuevo objeto si este ya existe. El valor predeterminado es **False**.

- **Incluir nombres de restricción del sistema**: si es **False**, el valor predeterminado de las restricciones que se denominaron automáticamente en la base de datos de origen se vuelven a denominar automáticamente en la base de datos de destino. Si es **True**, las restricciones tienen el mismo nombre en las bases de datos de origen y de destino.

- **Incluir instrucciones no compatibles** : si es **False**, el script no contiene las instrucciones para los objetos que no se admiten en la versión de servidor o tipo de motor seleccionados. Si es **True**, el script contiene los objetos no compatibles. Cada instrucción para un objeto no compatible tiene un comentario que indica que se debe editar la instrucción antes de que el script pueda ejecutarse con respecto a la versión del SQL Server o tipo de motor seleccionados. El valor predeterminado es **False**.

- **Nombres de objeto de certificación de esquema** : incluye el nombre de esquema en el nombre de los objetos que se crean. El valor predeterminado es **True**.

- **Incluir enlaces** : genera un script para enlazar los objetos predeterminados y de regla. El valor predeterminado es **False**. Para obtener más información, vea [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md) y [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md).

- **Incluir intercalación** : incluye información de intercalación en el script. El valor predeterminado es **False**. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).

- **Generar script de valores predeterminados** : incluye los objetos predeterminados que se usan para establecer los valores en las columnas de tabla. El valor predeterminado es **True**. Para obtener más información, vea [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md).

- **Incluir DROP y CREATE en el script** : Si es **Incluir CREATE en el script**, se incluyen las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para crear objetos. Si es **Incluir DROP en el script**, se incluyen las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para quitar objetos. Si es **Incluir DROP y CREATE en el script**, se incluye la instrucción DROP de [!INCLUDE[tsql](../../includes/tsql-md.md)] en el script, seguida de la instrucción CREATE, por cada objeto del script. El valor predeterminado es **Incluir CREATE en el script**.

- **Generar script de propiedades extendidas** : incluye propiedades extendidas en el script si el objeto tiene propiedades extendidas. El valor predeterminado es **True**.

- **Script para tipo de motor** : crea un script que se puede ejecutar en el tipo seleccionado de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o en una instancia del motor de base de datos de SQL Server. Los objetos no admitidos en el tipo especificado no se incluyen en el script. El valor predeterminado es el tipo del servidor de origen.

- **Script para versión de servidor** : crea un script que se puede ejecutar en la versión seleccionada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las características nuevas de una versión no se pueden incluir en scripts para versiones anteriores. El valor predeterminado es la versión del servidor de origen.

- **Incluir inicios de sesión en el script** : cuando el objeto que se debe incluir en un script es un usuario de base de datos, esta opción crea los inicios de sesión de los que depende el usuario. El valor predeterminado es **False**.

- **Incluir permisos de objeto en el script** : incluye scripts para establecer permisos en los objetos de la base de datos. El valor predeterminado es **False**.

- **Incluir estadísticas en el script** : si se establece el valor **Incluir estadísticas en el script**, esta opción incluye la instrucción **CREATE STATISTICS** para volver a crear estadísticas del objeto. La opción **Incluir estadísticas e histogramas en el script** también crea información de histogramas. El valor predeterminado es **No incluir estadísticas en el script**. Para obtener más información, vea [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).

- **Incluir USE DATABASE en el script** : agrega la instrucción **USE DATABASE** al script. Para asegurarse de que se creen objetos de base de datos en la base de datos correcta, incluya la instrucción **USE DATABASE** . Cuando necesite utilizar el script en otra base de datos, seleccione **False** para omitir la instrucción **USE DATABASE** . El valor predeterminado es **True**. Para obtener más información, vea [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).

- **Tipos de datos para generar por script**: selecciona lo que se debe generar por script: **Solo datos**, **Solo esquema** o ambos. El valor predeterminado **Solo esquema**.

**Opciones de tabla o vista** : las siguientes opciones solo se aplican a scripts para tablas o vistas.

- **Generar script de seguimiento de cambios** : incluye en el script el seguimiento de cambios si se ha habilitado en la base de datos de origen o en las tablas de la base de datos de origen. El valor predeterminado es **False**. Para obtener más información, vea [Acerca del seguimiento de cambios &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md).

- **Incluir restricciones CHECK en el script**: agrega restricciones **CHECK** al script. El valor predeterminado es **True**. Las restricciones **CHECK** requieren datos que se escriban en una tabla para cumplir con una condición especificada. Para más información, consulte [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).

- **Incluir opciones de compresión de datos en el script** : incluye las opciones de compresión de datos en el script, si se han configurado en la base de datos de origen o en las tablas de la base de datos de origen. Para obtener más información, consulte [Data Compression](../../relational-databases/data-compression/data-compression.md). El valor predeterminado es **False**.

- **Generar script de claves externas** : agrega claves externas al script. El valor predeterminado es **True**. Las claves externas indican y exigen relaciones entre tablas.

- **Generar script de índices de texto completo** : incluye en el script la creación de índices de texto completo. El valor predeterminado es **False**.

- **Generar script de índices** : incluye en el script la creación de índices. El valor predeterminado es **True**. Los índices ayudan a encontrar rápidamente los datos.

- **Generar script de claves principales** : incluye en el script la creación de claves principales en las tablas. El valor predeterminado es **True**. Las claves principales identifican de forma exclusiva cada fila de una tabla.

- **Generar script de desencadenadores** : incluye en el script la creación de desencadenadores DML en las tablas. El valor predeterminado es **False**. Un desencadenador DML es una acción programada para ejecutarse cuando se produce un evento DML (lenguaje de manipulación de datos) en el servidor de base de datos. Para más información, consulte [DML Triggers](../../relational-databases/triggers/dml-triggers.md).

- **Generar script de claves únicas** : incluye en el script la creación de claves únicas en las tablas. Las claves únicas evitan que se especifiquen datos duplicados. El valor predeterminado es **True**. Para más información, consulte [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).

### <a name="summary-page"></a><a name="Summary"></a> Página Resumen

![Resumen de GS](media/generate-and-publish-scripts-wizard/summary.png)

En esta página se resumen las opciones que ha seleccionado en este asistente. Para cambiar una opción, seleccione **Anterior**. Para empezar a generar scripts guardados o publicados, seleccione **Siguiente**.

**Revisar opciones seleccionadas** : muestra las selecciones que ha realizado en cada página del asistente. Expanda un nodo para ver las opciones seleccionadas de la página correspondiente.

### <a name="save-or-publish-scripts-page"></a><a name="SavePubScripts"></a> Página Guardar o publicar scripts  

Use esta página para supervisar el progreso del asistente a medida que se produce.

**Detalles** : vea en la columna **Acción** el progreso del asistente. Después de generar los scripts, el asistente los guarda en un archivo o los usa para publicar en un servicio web, según las selecciones. Cuando cada uno de estos pasos se haya completado, seleccione el valor de la columna **Resultado** para ver el resultado del paso correspondiente.

**Guardar informe:** seleccione esta opción para guardar los resultados del progreso del asistente en un archivo.

**Cancelar**: seleccione esta opción para cerrar el asistente antes de que se haya completado el procesamiento, o si se produce un error.

**Terminar**: seleccione esta opción para cerrar el asistente después de que se haya completado el procesamiento o si se produce un error.

### <a name="save-scripts"></a>Guardar scripts

![Finalizar](media/generate-and-publish-scripts-wizard/save-scripts-finish.png)

Si todos los valores de configuración son correctos, la configuración finaliza correctamente.

## <a name="generating-scripts-on-azure-synapse-analytics"></a>Generación de scripts en Azure Synapse Analytics

Si la sintaxis generada al usar "Script como..." no se parece a la sintaxis de [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] o si recibe un mensaje de error, es posible que deba configurar las opciones de scripting de SQL Server Management Studio en [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

### <a name="how-to-set-default-scripting-options-to-sql-data-warehouse"></a>Cómo establecer opciones de scripting predeterminadas en el Almacenamiento de datos SQL

Para generar un script de objetos con la sintaxis de [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] , establezca la opción de scripting predeterminada en [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] , tal y como se indica a continuación:

1. Seleccione **Herramientas** y, luego, **Opciones**.
2. En **Opciones generales de scripting** , establezca lo siguiente:
    1. Script para el tipo de motor de base de datos: **Microsoft Azure SQL Database**.
    2. Script para la edición del motor de base de datos: **Microsoft Azure SQL Data Warehouse Edition**.
3. Seleccione **Aceptar**.

### <a name="how-to-generate-scripts-for-sql-data-warehouse-when-it-is-not-the-default-scripting-option"></a>Cómo generar scripts para el Almacenamiento de datos SQL cuando no es la opción de scripting predeterminada

Si establece el [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] como la opción de scripting predeterminada, como se indica anteriormente, puede ignorar estas instrucciones, aunque si decide usar unas opciones de scripting predeterminadas diferentes, es posible que se produzca un error. Para evitar errores, siga estos pasos para generar y publicar scripts para el [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]:

1. Haga clic con el botón derecho en la base de datos de SQL Data Warehouse.
2. Seleccione **Generar scripts**.
3. Elija los objetos de los que quiera generar un script.
4. En **Opciones de scripting**, seleccione **Opciones avanzadas**. En **General** , establezca lo siguiente:
    1. Script para el tipo de motor de base de datos: **Microsoft Azure SQL Database**.
    2. Script para la edición del motor de base de datos: **Microsoft Azure SQL Data Warehouse Edition**.
5. Seleccione **Guardar o publicar scripts** y, luego, **Finalizar**.

No se conservan las opciones establecidas en el paso 4. Si prefiere que se conserven estas opciones, siga las instrucciones de **Cómo establecer opciones de scripting predeterminadas en el Almacenamiento de datos SQL**.

## <a name="see-also"></a>Consulte también

- [Instalar SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

- [Copiar bases de datos en otros servidores](../../relational-databases/databases/copy-databases-to-other-servers.md)