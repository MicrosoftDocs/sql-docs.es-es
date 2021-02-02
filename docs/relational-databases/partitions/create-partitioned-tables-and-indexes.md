---
description: Crear tablas e índices con particiones
title: Creación de tablas e índices con particiones | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.createpartition.progress.f1
- sql13.swb.createpartition.partitioncolumn.f1
- sql13.swb.createpartition.createjob.f1
- sql13.swb.createpartition.finish.f1
- sql13.swb.createpartition.selectoutput.f1
- sql13.swb.createpartition.partitionfunction.f1
- sql13.swb.createpartition.partitionscheme.f1
- sql13.swb.createpartition.getstart.f1
- sql13.swb.createpartition.mappartition.f1
- sql13.swb.createpartition.summary.f1
helpviewer_keywords:
- partitioned indexes [SQL Server], creating
- partition schemes [SQL Server], creating
- partition functions [SQL Server], creating
- partitioned tables [SQL Server], creating
- partition functions [SQL Server]
- partition schemes [SQL Server]
ms.assetid: 7641df10-1921-42a7-ba6e-4cb03b3ba9c8
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 273e6f625b0a86be179da529e8b52e0d7ce991cc
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049168"
---
# <a name="create-partitioned-tables-and-indexes"></a>Crear tablas e índices con particiones
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Puede crear una tabla o índice con particiones en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Los datos en tablas e índices con particiones se dividen horizontalmente en unidades que pueden propagarse por más de un grupo de archivos de la base de datos. Las particiones pueden hacer que las tablas y los índices grandes sean más escalables y fáciles de administrar.  
  
 La creación de una tabla o índice con particiones tiene lugar normalmente en cuatro partes:  
  
1.  Crear un grupo o grupos de archivos y los archivos correspondientes que contendrán las particiones especificadas por el esquema de partición.  
  
2.  Crear una función de partición que asigna las filas de una tabla o un índice a particiones según los valores de una columna especificada.  
  
3.  Crear un esquema de partición que asigna las particiones de una tabla o índice con particiones a los nuevos grupos de archivos.  
  
4.  Crear o modificar una tabla o un índice y especificar el esquema de partición como ubicación de almacenamiento.  
 
> [!NOTE]
> En Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] solamente se admiten grupos de archivos principales.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear una tabla o un índice con particiones, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El ámbito de una función y un esquema de partición se limita a la base de datos en la que se han creado. En la base de datos, las funciones de partición residen en un espacio de nombres independiente de las demás funciones.  
  
-   Si algunas filas de una función de partición tienen columnas de partición con valores NULL, estas filas se asignan a la partición situada más a la izquierda. Sin embargo, si se especifica NULL como valor de límite y se indica RIGHT, la partición situada más a la izquierda permanece vacía y los valores NULL se colocan en la segunda partición.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 La creación de una tabla con particiones requiere el permiso CREATE TABLE en la base de datos y el permiso ALTER en el esquema en el que se crea la tabla. Crear un índice con particiones requiere el permiso ALTER en la tabla o vista donde se crea el índice. Crear una tabla o índice con particiones requiere alguno de los permisos adicionales siguientes:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se está creando la función de partición y el esquema de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se está creando la función de partición y el esquema de partición.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Siga los pasos de este procedimiento para crear uno o más grupos de archivos, los archivos correspondientes y una tabla. Hará referencia a estos objetos en el siguiente procedimiento al crear la tabla con particiones.  
  
#### <a name="to-create-new-filegroups-for-a-partitioned-table"></a>Para crear nuevos grupos de archivos en una tabla con particiones  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en la base de datos en la que quiere crear una tabla con particiones y seleccione **Propiedades**.  
  
2.  En el cuadro de diálogo **Propiedades de la base de datos -** *nombre de base de datos*, en **Seleccionar una página**, seleccione **Grupos de archivos**.  
  
3.  En **Filas**, haga clic en **Agregar**. En la nueva fila, escriba el nombre del grupo de archivos.  
  
    > [!WARNING]  
    >  Al especificar varios grupos de archivos, siempre debe haber uno adicional, además de los que especifique para los valores de límite en el momento de crear las particiones.  
  
4.  Continúe agregando filas hasta que haya creado todos los grupos de archivos que la tabla con particiones.  
  
5.  Haga clic en **Aceptar**.  
  
6.  En **Seleccionar una página**, seleccione **Archivos**.  
  
7.  En **Filas**, haga clic en **Agregar**. En la nueva fila, escriba un nombre de archivo y seleccione un grupo de archivos.  
  
8.  Continúe agregando filas hasta que haya creado al menos un archivo para cada grupo de archivos.  
  
9. Expanda la carpeta de **Tablas** y cree una tabla como haría normalmente. Para obtener más información, vea [Crear tablas &#40;motor de base de datos&#41;](../../relational-databases/tables/create-tables-database-engine.md). O bien, puede especificar una tabla existente en el procedimiento siguiente.  
  
#### <a name="to-create-a-partitioned-table"></a>Para crear una tabla con particiones  
  
1.  Haga clic con el botón derecho en la tabla donde quiere crear las particiones, seleccione **Almacenamiento** y, después, haga clic en **Crear partición...**.  
  
2.  En el **Asistente para la creación de particiones**, en la página **Asistente para la creación de particiones** , haga clic en **Siguiente**.  
  
3.  En la página **Seleccionar una columna de particionamiento** , en la cuadrícula de **Columnas de particionamiento disponibles** , seleccione la columna en la que desea crear particiones de la tabla. En la cuadrícula **Columnas de particionamiento disponibles** solo se mostrarán las columnas con tipos de datos que se pueden utilizar para crear particiones de los datos. Si selecciona una columna calculada como columna de partición, la columna se debe designar como una columna PERSISTED.  
  
     Las opciones de que dispone para la columna de partición y el intervalo de valores vienen determinados, principalmente, por el grado en que los datos se pueden agrupar de un modo lógico. Por ejemplo, puede decidir dividir los datos en agrupaciones lógicas por meses o trimestres del año. Las consultas que planea realizar en los datos determinarán si esta agrupación lógica es adecuada para administrar las particiones de la tabla. Todos los tipos de datos son válidos para su uso como columnas de partición, excepto **text**, **ntext**, **image**, **xml**, **timestamp**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, tipos de datos de alias o tipos de datos definidos por el usuario CLR.  
  
     En esta página están disponibles las opciones siguientes:  
  
     **Colocar esta tabla en la tabla con partición seleccionada**  
     Permite seleccionar una tabla con particiones que contenga los datos relacionados para combinar con esta tabla en la columna de partición. Las tablas con particiones combinadas en las columnas de partición suelen dar lugar a consultas más eficientes.  
  
     **Alinear almacenamiento de índices no únicos e índices únicos con una columna de partición indizada**  
     Alinea todos los índices de la tabla que tiene particiones con el mismo esquema de partición. Cuando una tabla y sus índices están alineados, puede mover las particiones dentro y fuera de las tablas con particiones de forma más eficaz, porque se usa el mismo algoritmo para crear las particiones de los datos.  
  
     Después de seleccionar la columna de partición y otras opciones, haga clic en **Siguiente**.  
  
4.  En la página **Seleccionar una función de partición** , en **Seleccionar una función de partición**, haga clic en **Nueva función de partición** o en **Función de partición existente**. Si elige **Nueva función de partición**, escriba el nombre de la función. Si elige **Función de partición existente**, seleccione el nombre de la función que quiera usar en la lista. La opción **Función de partición existente** no estará disponible si no hay otras funciones de partición en la base de datos.  
  
     Después de completar esta página, haga clic en **Siguiente**.  
  
5.  En la página **Seleccionar un esquema de partición** , en **Seleccionar un esquema de partición**, haga clic en **Nuevo esquema de partición** o **Esquema de partición existente**. Si elige **Nuevo esquema de partición**, escriba el nombre del esquema. Si elige **Esquema de partición existente**, seleccione el nombre del esquema que quiera usar en la lista. La opción **Esquema de partición existente** no estará disponible si no hay otros esquemas de partición en la base de datos.  
  
     Después de completar esta página, haga clic en **Siguiente**.  
  
6.  En la página **Asignar particiones** , debajo de **Intervalo**, seleccione **Límite izquierdo** o **Límite derecho** para especificar si se debe incluir el valor de límite superior o inferior de cada grupo de archivos que se crea. Al especificar varios grupos de archivos, siempre debe indicar uno adicional, además de los que especifique para los valores de límite en el momento de crear las particiones.  
  
     En la cuadrícula **Seleccione grupos de archivos y especifique valores límite** , debajo de **Grupo de archivos**, seleccione el grupo de archivos en el que desea crear particiones de los datos. Debajo de **Límite**, escriba el valor límite para cada grupo de archivos. Si el valor límite está vacío, la función de partición asigna la tabla o el índice completos a una sola partición y usa el nombre de la función de partición.  
  
     En esta página están disponibles las opciones siguientes:  
  
     **Establecer límites...**  
     Abre el cuadro de diálogo **Establecer valores límite** para seleccionar los valores límite y los rangos de fechas que desea para las particiones. Esta opción solo está disponible cuando ha seleccionado una columna de partición que contiene uno de los tipos de datos siguientes: **date**, **datetime**, **smalldatetime**, **datetime2** o **datetimeoffset**.  
  
     **Estimar almacenamiento**  
     Calcula el número de filas, el espacio necesario y el espacio disponible para el almacenamiento de cada grupo de archivos especificado para las particiones. Estos valores se muestran en la cuadrícula como valores de solo lectura.  
  
     El cuadro de diálogo **Establecer valores límite** admite las opciones adicionales siguientes:  
  
     **Fecha de inicio**  
     Selecciona la fecha de inicio para los valores de rango de las particiones.  
  
     **Fecha de finalización**  
     Selecciona la fecha de finalización para los valores de rango de las particiones. Si ha seleccionado la opción **Límite izquierdo** en la página **Asignar particiones** , esta fecha será el último valor para cada grupo de archivos o partición. Si ha seleccionado la opción **Límite derecho** en la página **Asignar particiones** , esta fecha será el primer valor en el grupo de archivos siguiente al último.  
  
     **Intervalo de fechas**  
     Selecciona la granularidad de fechas o el incremento de los valores de rango que desea para cada partición.  
  
     Después de completar esta página, haga clic en **Siguiente**.  
  
7.  En la página **Seleccionar la opción de salida** , especifique cómo desea completar la tabla con particiones. Seleccione **Crear script** para crear un script SQL basado en las páginas anteriores del asistente. Seleccione **Ejecutar inmediatamente** para crear la nueva tabla con particiones después de completar todas las páginas restantes del asistente. Seleccione **Programar** para crear la tabla con particiones en un momento predeterminado en el futuro.  
  
     Si selecciona **Crear script**, las opciones siguientes están disponibles debajo de **Opciones de script**:  
  
     **Generar script en archivo**  
     Genera el script como un archivo .sql. Escriba un nombre de archivo y ubicación en el cuadro de diálogo **Nombre de archivo** o haga clic en **Examinar** para abrir **Ubicación del archivo script** . En **Guardar como**, seleccione **Texto Unicode** o **Texto ANSI**.  
  
     **Generar script en Portapapeles**  
     Guarda el script en el Portapapeles.  
  
     **Generar script en la ventana Nueva consulta**  
     Genera el script en una nueva ventana del Editor de consultas. Esta es la selección predeterminada.  
  
     Si selecciona **Programar**, haga clic en **Cambiar programación**.  
  
    1.  En el cuadro de diálogo **Nueva programación de trabajo**, en el cuadro **Nombre**, escriba el nombre de la programación de trabajo.  
  
    2.  En la lista **Tipo de programación** , seleccione el tipo de la programación:  
  
        -   **Iniciar automáticamente al iniciar el Agente SQL Server.**  
  
        -   **Iniciar al quedar inactivas las CPU**  
  
        -   **Periódica**. Seleccione esta opción si la nueva tabla con particiones se actualiza con la nueva información de forma regular.  
  
        -   **Una vez**. Esta es la selección predeterminada.  
  
    3.  Active o desactive la casilla **Habilitado** para habilitar o deshabilitar la programación.  
  
    4.  Si selecciona **Periódica**:  
  
        1.  En **Frecuencia**, en la lista **Sucede** , especifique la frecuencia de repetición:  
  
            -   Si selecciona **Diario**, en el cuadro **Se repite cada** , escriba la frecuencia con que se repite la programación de trabajo en días.  
  
            -   Si selecciona **Semanal**, en el cuadro **Se repite cada** , escriba la frecuencia con que se repite la programación de trabajo en semanas. Seleccione el día o días de la semana en que se ejecuta la programación de trabajo.  
  
            -   Si selecciona **Mensual**, seleccione **Día** o **El**.  
  
                -   Si selecciona **Día**, especifique la fecha del mes que desea que se ejecute la programación de trabajo y con qué frecuencia debe repetirse la programación de trabajo en meses. Por ejemplo, si quiere que la programación de trabajo se ejecute el decimoquinto día de cada mes, seleccione **Día** y escriba "15" en el primer cuadro y "2" en el segundo. Tenga en cuenta que el mayor número permitido en el segundo cuadro es "99".  
  
                -   Si selecciona **El**, seleccione el día concreto de la semana del mes en que desea que se ejecute la programación de trabajo y con qué frecuencia debe repetirse la programación de trabajo en meses. Por ejemplo, si quiere que la programación de trabajo se ejecute el último día de la semana de cada mes, seleccione **Día**, seleccione **último** en la primera lista y **día de la semana** en la segunda y, después, escriba "2" en el último cuadro. En las primeras dos listas, también puede seleccionar **primero**, **segundo**, **tercero** o **cuarto**, así como días de la semana concretos (por ejemplo: domingo o miércoles). Tenga en cuenta que el mayor número permitido en el último cuadro es "99".  
  
        2.  Debajo de **Frecuencia diaria**, especifique la frecuencia con que se repite la programación de trabajo en el día en que se ejecuta:  
  
            -   Si selecciona **Sucede una vez a las**, escriba la hora concreta en que debe ejecutarse la programación de trabajo en el cuadro **Sucede una vez a las** . Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
            -   Si selecciona **Sucede cada**, especifique la frecuencia con que se ejecuta la programación de trabajo durante el día seleccionado en **Frecuencia**. Por ejemplo, si quiere que la programación de trabajo se repita cada dos horas al día cuando se ejecuta, seleccione **Sucede cada**, escriba "2" en el primer cuadro y, después, seleccione **horas** en la lista. En esta lista también puede seleccionar **minutos** y **segundos**. Tenga en cuenta que el mayor número permitido en el primer cuadro es "100".  
  
                 En el cuadro **A partir de** , especifique la hora en que la programación de trabajo debe iniciar su ejecución. En el cuadro **Finaliza** , especifique la hora en que la programación de trabajo debe dejar de repetirse. Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
        3.  Debajo de **Duración**, en **Fecha de inicio**, escriba la fecha en que desea que la programación de trabajo inicie su ejecución. Seleccione **Fecha de finalización** o **Sin fecha de finalización** para indicar cuándo se debe detener la ejecución de la programación de trabajo. Si selecciona **Fecha de finalización**, escriba la fecha en que desea que deje de ejecutarse la programación de trabajo.  
  
    5.  Si selecciona **Una vez**, debajo de **Única repetición**, en el cuadro **Fecha** , escriba la fecha en que se ejecutará la programación de trabajo. En el cuadro **Hora** , especifique la hora a la que se ejecutará la programación de trabajo. Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
    6.  Debajo de **Resumen**, en **Descripción**, compruebe que todos los valores de la programación de trabajo son correctos.  
  
    7.  Haga clic en **OK**.  
  
     Después de completar esta página, haga clic en **Siguiente**.  
  
8.  En la página **Revisar resumen** , debajo de **Revisar opciones seleccionadas**, expanda todas las opciones disponibles para comprobar que todos los valores de la partición son correctos. Si todo está como se esperaba, haga clic en **Finalizar**.  
  
9. En la página **Progreso del Asistente para la creación de particiones** , supervise la información de estado sobre las acciones del Asistente para la creación de particiones. Según las opciones que se seleccionen en el asistente, la página de progreso puede contener una o varias acciones. El cuadro superior muestra el estado general del asistente y el número de mensajes de estado, error y advertencia que ha recibido.  
  
     Las siguientes opciones están disponibles en la página **Progreso del Asistente para la creación de particiones** :  
  
     **Detalles**  
     Proporciona la acción, el estado y los mensajes devueltos por la acción llevada a cabo por el asistente.  
  
     **Acción**  
     Especifica el tipo y el nombre de cada acción.  
  
     **Estado**  
     Indica si la acción del asistente como conjunto ha devuelto el valor **Correcto** o **Error**.  
  
     **Mensaje**  
     Proporciona los mensajes de error o de advertencia devueltos por el proceso.  
  
     **Report**  
     Crea un informe que contiene los resultados del Asistente para la creación de particiones. Las opciones son **Ver informe**, **Guardar informe en archivo**, **Copiar informe al Portapapeles** y **Enviar informe como correo electrónico**.  
  
     **Ver informe**  
     Abre el cuadro de diálogo **Ver informe** , que contiene un informe de texto del progreso del Asistente para la creación de particiones.  
  
     **Guardar informe en archivo**  
     Abre el cuadro de diálogo **Guardar informe como** .  
  
     **Copiar informe al Portapapeles**  
     Copia los resultados del informe de progreso del asistente al Portapapeles.  
  
     **Enviar informe como correo electrónico**  
     Copia los resultados del informe de progreso del asistente en un mensaje de correo electrónico.  
  
     Cuando termine, haga clic en **Cerrar**.  
  
 El Asistente para la creación de particiones crea la función y el esquema de partición y aplica las particiones a la tabla especificada. Para comprobar las particiones de tabla, en el Explorador de objetos, haga clic con el botón derecho en la tabla y seleccione **Propiedades**. Haga clic en la página **Almacenamiento**. La página muestra información como el nombre de la función y el esquema de partición y el número de particiones.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-partitioned-table"></a>Para crear una tabla con particiones  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En el ejemplo se crean nuevos grupos de archivos, una función de partición y un esquema de partición. Se crea una nueva tabla con el esquema de partición especificado como ubicación de almacenamiento.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Adds four new filegroups to the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test1fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test4fg;   
  
    -- Adds one file for each filegroup.  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test1dat1,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat1.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test1fg;  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test2dat2,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t2dat2.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test3dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t3dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test4dat4,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t4dat4.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test4fg;  
    GO  
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the four filegroups created above  
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        TO (test1fg, test2fg, test3fg, test4fg) ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO  
    ```  


#### <a name="to-create-a-partitioned-table-in-azure-sqldbesa"></a>Procedimiento para crear una tabla con particiones en Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

En Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], no se permite agregar archivos ni grupos de archivos, pero las tablas con particiones sí que se admiten mediante la creación de particiones únicamente en el grupo de archivos con el valor PRIMARY.
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
1.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
1.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo, se crean una función de partición y un esquema de partición. Se crea una nueva tabla con el esquema de partición especificado como ubicación de almacenamiento. 

    ```
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the PRIMARY filegroup 
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        ALL TO ('PRIMARY') ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO
    ```  

  
#### <a name="to-determine-if-a-table-is-partitioned"></a>Para determinar si se crean particiones de una tabla  
  
1.  La consulta siguiente devuelve una o más filas si la tabla tiene particiones `PartitionTable` . Si la tabla no tiene particiones, no se devuelve ninguna fila.  
  
    ```  
    SELECT *   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] IN (0,1)   
    JOIN sys.partition_schemes ps   
        ON i.data_space_id = ps.data_space_id   
    WHERE t.name = 'PartitionTable';   
    GO  
    ```  
  
#### <a name="to-determine-the-boundary-values-for-a-partitioned-table"></a>Para determinar los valores de límite para una tabla con particiones  
  
1.  La consulta siguiente devuelve los valores de límite para cada partición de la tabla `PartitionTable` .  
  
    ```  
    SELECT t.name AS TableName, i.name AS IndexName, p.partition_number, p.partition_id, i.data_space_id, f.function_id, f.type_desc, r.boundary_id, r.value AS BoundaryValue   
    FROM sys.tables AS t  
    JOIN sys.indexes AS i  
        ON t.object_id = i.object_id  
    JOIN sys.partitions AS p  
        ON i.object_id = p.object_id AND i.index_id = p.index_id   
    JOIN  sys.partition_schemes AS s   
        ON i.data_space_id = s.data_space_id  
    JOIN sys.partition_functions AS f   
        ON s.function_id = f.function_id  
    LEFT JOIN sys.partition_range_values AS r   
        ON f.function_id = r.function_id and r.boundary_id = p.partition_number  
    WHERE t.name = 'PartitionTable' AND i.type <= 1  
    ORDER BY p.partition_number;  
    ```  
  
#### <a name="to-determine-the-partition-column-for-a-partitioned-table"></a>Para determinar la columna de partición de una tabla con particiones  
  
1.  La consulta siguiente devuelve el nombre de la columna de partición de la tabla. `PartitionTable`.  
  
    ```  
    SELECT   
        t.[object_id] AS ObjectID   
        , t.name AS TableName   
        , ic.column_id AS PartitioningColumnID   
        , c.name AS PartitioningColumnName   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] <= 1 -- clustered index or a heap   
    JOIN sys.partition_schemes AS ps   
        ON ps.data_space_id = i.data_space_id   
    JOIN sys.index_columns AS ic   
        ON ic.[object_id] = i.[object_id]   
        AND ic.index_id = i.index_id   
        AND ic.partition_ordinal >= 1 -- because 0 = non-partitioning column   
    JOIN sys.columns AS c   
        ON t.[object_id] = c.[object_id]   
        AND ic.column_id = c.column_id   
    WHERE t.name = 'PartitionTable' ;   
    GO  
    ```  
  
 Para obtener más información, consulte:  
  
-   [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
  
-   [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)  
  
-   [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)  
  
-   [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
  
