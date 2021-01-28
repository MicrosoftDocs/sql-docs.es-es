---
title: Habilitar compresión de una tabla o un índice
description: Obtenga información sobre cómo habilitar la compresión en una tabla o índice en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 01/22/2021
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.compwiz.compressiontype.f1
- sql13.swb.compwiz.outputoptions.f1
- sql13.swb.compwiz.summary.f1
- sql13.swb.compwiz.scriptfileoption.f1
- sql13.swb.compwiz.progress.f1
- sql13.swb.compwiz.welcome.f1
- sql13.swb.compwiz.createjob.f1
- sql13.swb.compwiz.selectaction.f1
helpviewer_keywords:
- data compression wizard
- compression [SQL Server], enable
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 59d15d0f056a757fea1545025832990254a7e4ae
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766359"
---
# <a name="enable-compression-on-a-table-or-index"></a>Habilitar compresión de una tabla o un índice

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  En este artículo se describe cómo habilitar la [compresión de datos](../../relational-databases/data-compression/data-compression.md) en una tabla o un índice de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este artículo**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para habilitar la compresión a una tabla o un índice, utilice:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   En las tablas del sistema no se puede habilitar la compresión.  
  
-   Si la tabla es un montón, la operación de regeneración en modo ONLINE será de un solo subproceso. Utilice el modo OFFLINE para una operación de regeneración de montón de varios subprocesos. Las operaciones de recompilación están sin conexión (OFFLINE) a menos que se especifique la opción ONLINE. Para obtener información completa sobre cómo realizar una regeneración ONLINE, vea [Realizar operaciones de índice en línea](../indexes/perform-index-operations-online.md).
  
-   No se puede cambiar la configuración de compresión de una partición única si la tabla tiene índices no alineados.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o índice.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-enable-compression-on-a-table-or-index"></a>Para habilitar la compresión de una tabla o un índice  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla que desea comprimir y, a continuación, expanda la carpeta **Tablas** .  
  
2.  Para comprimir un índice, expanda la tabla que contiene el índice que desea comprimir y, a continuación, expanda la carpeta **Índices** .  
  
3.  Haga clic con el botón derecho en la tabla o el índice que quiera comprimir, seleccione **Almacenamiento** y, luego, **Administrar la compresión...** .  
  
4.  En la página de bienvenida del **Asistente para compresión de datos**, seleccione **Siguiente**.  
  
5.  En la página de **Seleccionar el tipo de compresión** , seleccione el tipo de compresión para aplicar a cada partición de la tabla o del índice que desea comprimir. Cuando termine, seleccione **Siguiente**.  
  
     Las siguientes opciones están disponibles en la página de **Seleccionar el tipo de compresión** :  
  
     Casilla **Usar el mismo tipo de compresión para todas las particiones**  
     Active esta casilla para configurar el mismo valor de compresión para todas las particiones. Esto habilita el cuadro de selección y deshabilita la columna **Tipo de compresión** en la cuadrícula. Cuando se activa, las opciones de la lista adyacente son **Ninguna**, **Fila** y **Página**.  
  
     **Número de partición**  
     Muestra una lista con cada partición en la tabla o el índice. Esta columna es de solo lectura.  
  
     **Tipo de compresión**  
     Seleccione la opción de compresión para cada partición. No estará disponible cuando la casilla **Usar el mismo tipo de compresión para todas las particiones** esté activada. Las opciones son **Ninguna**, **Fila** y **Página**.  
  
     **Límite**  
     Muestra el límite de la partición. Esta columna es de solo lectura.  
  
     **Recuento de filas**  
     Muestra el número de filas en esta partición. Esta columna es de solo lectura.  
  
     **Espacio actual**  
     Muestra el espacio actual que esta partición ocupa en megabytes (MB). Esta columna es de solo lectura.  
  
     **Espacio comprimido solicitado**  
     Después de seleccionar **Calcular**, en esta columna se muestra el tamaño estimado de cada partición después de llevar a cabo la compresión con la configuración especificada en la columna **Tipo de compresión**. Esta columna es de solo lectura.  
  
     **Calcular**  
     Seleccione para deducir el tamaño de cada partición después de realizar la compresión mediante la configuración que ha especificado en la columna **Tipo de compresión**.  
  
6.  En la página **Seleccionar la opción de salida** , especifique cómo desea completar la compresión. Seleccione **Crear script** para crear un script SQL basado en las páginas anteriores del asistente. Seleccione **Ejecutar inmediatamente** para crear la nueva tabla con particiones después de completar todas las páginas restantes del asistente. Seleccione **Programar** para crear la tabla con particiones en un momento predeterminado en el futuro.  
  
     Si selecciona **Crear script**, las opciones siguientes están disponibles debajo de **Opciones de script**:  
  
     **Generar script en archivo**  
     Genera el script como un archivo .sql. Escriba un nombre de archivo y una ubicación en el cuadro de diálogo **Nombre de archivo**, o bien seleccione **Examinar** para abrir **Ubicación del archivo de script**. En **Guardar como**, seleccione **Texto Unicode** o **Texto ANSI**.  
  
     **Generar script en Portapapeles**  
     Guarda el script en el Portapapeles.  
  
     **Generar script en la ventana Nueva consulta**  
     Genera el script en una nueva ventana del Editor de consultas. Esta es la selección predeterminada.  
  
     Si selecciona **Programar**, seleccione **Cambiar programación**.  
  
    1.  En el cuadro de diálogo **Nueva programación de trabajo**, en el cuadro **Nombre**, escriba el nombre de la programación de trabajo.  
  
    2.  En la lista **Tipo de programación** , seleccione el tipo de la programación:  
  
        -   **Iniciar automáticamente al iniciar el Agente SQL Server.**  
  
        -   **Iniciar al quedar inactivas las CPU**  
  
        -   **Periódica**. Seleccione esta opción si la nueva tabla con particiones se actualiza con la nueva información de forma periódica.  
  
        -   **Una vez**. Esta opción es la selección predeterminada.  
  
    3.  Active o desactive la casilla **Habilitado** para habilitar o deshabilitar la programación.  
  
    4.  Si selecciona **Periódica**:  
  
        1.  En **Frecuencia**, en la lista **Sucede** , especifique la frecuencia de repetición:  
  
            -   Si selecciona **Diario**, en el cuadro **Se repite cada** , escriba la frecuencia con que se repite la programación de trabajo en días.  
  
            -   Si selecciona **Semanal**, en el cuadro **Se repite cada** , escriba la frecuencia con que se repite la programación de trabajo en semanas. Seleccione el día o días de la semana en que se ejecuta la programación de trabajo.  
  
            -   Si selecciona **Mensual**, seleccione **Día** o **El**.  
  
                -   Si selecciona **Día**, especifique la fecha del mes que desea que se ejecute la programación de trabajo y con qué frecuencia debe repetirse la programación de trabajo en meses. Por ejemplo, si quiere que la programación de trabajo se ejecute el decimoquinto día de cada mes, seleccione **Día** y escriba "15" en el primer cuadro y "2" en el segundo. Tenga en cuenta que el mayor número permitido en el segundo cuadro es "99".  
  
                -   Si selecciona **El**, seleccione el día concreto de la semana del mes en que desea que se ejecute la programación de trabajo y con qué frecuencia debe repetirse la programación de trabajo en meses. Por ejemplo, si quiere que la programación de trabajo se ejecute el último día de la semana de cada mes, seleccione **Día**, seleccione **último** en la primera lista y **día de la semana** en la segunda y, después, escriba "2" en el último cuadro. En las primeras dos listas, también puede seleccionar **primero**, **segundo**, **tercero** o **cuarto**, así como días de la semana concretos (por ejemplo: domingo o miércoles). Tenga en cuenta que el mayor número permitido en el último cuadro es "99".  
  
        2.  Debajo de **Frecuencia diaria**, especifique la frecuencia con que se repite la programación de trabajo en el día en que se ejecuta:  
  
            -   Si selecciona **Sucede una vez a las**, escriba la hora concreta en que debe ejecutarse la programación de trabajo en el cuadro **Sucede una vez a las** . Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
            -   Si selecciona **Sucede cada**, especifique la frecuencia con que se ejecuta la programación de trabajo durante el día seleccionado en **Frecuencia**. Por ejemplo, si quiere que la programación de trabajo se repita cada dos horas al día cuando se ejecuta, seleccione **Sucede cada**, escriba "2" en el primer cuadro y, después, seleccione **horas** en la lista. En esta lista también puede seleccionar **minutos** y **segundos**. Tenga en cuenta que el mayor número permitido en el primer cuadro es "100".  
  
                 En el cuadro **A partir de** , especifique la hora en que la programación de trabajo debe iniciar su ejecución. En el cuadro **Finaliza** , especifique la hora en que la programación de trabajo debe dejar de repetirse. Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
        3.  Debajo de **Duración**, en **Fecha de inicio**, escriba la fecha en que desea que la programación de trabajo inicie su ejecución. Seleccione **Fecha de finalización** o **Sin fecha de finalización** para indicar cuándo se debe detener la ejecución de la programación de trabajo. Si selecciona **Fecha de finalización**, escriba la fecha en que desea que deje de ejecutarse la programación de trabajo.  
  
    5.  Si selecciona **Una vez**, debajo de **Única repetición**, en el cuadro **Fecha** , escriba la fecha en que se ejecutará la programación de trabajo. En el cuadro **Hora** , especifique la hora a la que se ejecutará la programación de trabajo. Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
    6.  Debajo de **Resumen**, en **Descripción**, compruebe que todos los valores de la programación de trabajo son correctos.  
  
    7.  Seleccione **Aceptar**.  
  
     Después de completar esta página, seleccione **Siguiente**.  
  
7.  En la página **Revisar resumen** , bajo **Revisar opciones seleccionadas**, expanda todas las opciones disponibles para comprobar que todos los valores de la compresión son correctos. Si todo está como se esperaba, seleccione **Finalizar**.  
  
8.  En la página **Progreso del asistente para compresión** , supervise la información de estado sobre las acciones del Asistente para la creación de particiones. Según las opciones que se seleccionen en el asistente, la página de progreso puede contener una o varias acciones. El cuadro superior muestra el estado general del asistente y el número de mensajes de estado, error y advertencia que ha recibido.  
  
     Las siguientes opciones están disponibles en la página **Progreso del asistente para compresión** :  
  
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
  
     Cuando haya finalizado, seleccione **Cerrar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  

### <a name="sql-server"></a>SQL Server

En SQL Server, ejecute `sp_estimate_data_compression_savings` y, después, habilite la compresión en la tabla o el índice. Vea las secciones siguientes. 

#### <a name="to-enable-compression-on-a-table"></a>Para habilitar la compresión en una tabla  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**. En el ejemplo primero se ejecuta el procedimiento almacenado `sp_estimate_data_compression_savings` para devolver el tamaño estimado del objeto en caso de que se use la configuración de compresión ROW. Después, se habilita la compresión de ROW en todas las particiones de la tabla especificada.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    EXEC sp_estimate_data_compression_savings 'Production', 'TransactionHistory', NULL, NULL, 'ROW' ;  
  
    ALTER TABLE Production.TransactionHistory REBUILD PARTITION = ALL  
    WITH (DATA_COMPRESSION = ROW);   
    GO  
    ```  
  
#### <a name="to-enable-compression-on-an-index"></a>Para habilitar la compresión en un índice  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**. En el ejemplo primero se consulta la vista de catálogo `sys.indexes` para devolver el nombre y el `index_id` para cada índice en la tabla `Production.TransactionHistory` . Después, se ejecuta el procedimiento almacenado `sp_estimate_data_compression_savings` para devolver el tamaño estimado del identificador del índice especificado en caso de que se use el valor de compresión PAGE. Finalmente, en el ejemplo se vuelve a generar el identificador de índice 2 (`IX_TransactionHistory_ProductID`), especificando la compresión PAGE.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, index_id  
    FROM sys.indexes  
    WHERE OBJECT_NAME (object_id) = N'TransactionHistory';  
  
    EXEC sp_estimate_data_compression_savings   
        @schema_name = 'Production',   
        @object_name = 'TransactionHistory',  
        @index_id = 2,   
        @partition_number = NULL,   
        @data_compression = 'PAGE' ;   
  
    ALTER INDEX IX_TransactionHistory_ProductID ON Production.TransactionHistory REBUILD PARTITION = ALL WITH (DATA_COMPRESSION = PAGE);  
    GO  
    ``` 
    
### <a name="on-azure-sql-database"></a>En Azure SQL Database

Azure SQL Database no admite el procedimiento almacenado `sp_estimate_data_compression_savings`. Los scripts siguientes habilitan la compresión sin estimar la cantidad de esta. 

#### <a name="to-enable-compression-on-a-table"></a>Para habilitar la compresión en una tabla  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**. En el ejemplo se habilita la compresión de ROW en todas las particiones de la tabla especificada.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  

    ALTER TABLE Production.TransactionHistory REBUILD PARTITION = ALL  
    WITH (DATA_COMPRESSION = ROW);   
    GO  
    ```  
  
#### <a name="to-enable-compression-on-an-index"></a>Para habilitar la compresión en un índice  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**. En el ejemplo primero se consulta la vista de catálogo `sys.indexes` para devolver el nombre y el `index_id` para cada índice en la tabla `Production.TransactionHistory` . Finalmente, en el ejemplo se vuelve a generar el identificador de índice 2 (`IX_TransactionHistory_ProductID`), especificando la compresión PAGE.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, index_id  
    FROM sys.indexes  
    WHERE OBJECT_NAME (object_id) = N'TransactionHistory';  
    
    ALTER INDEX IX_TransactionHistory_ProductID ON Production.TransactionHistory REBUILD PARTITION = ALL WITH (DATA_COMPRESSION = PAGE);  
    GO  
    ``` 
  
 Para obtener más información, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) y [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [Comprimir datos](../../relational-databases/data-compression/data-compression.md)   
 [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md)  
  
  
