---
title: Clasificación y detección de datos de SQL | Microsoft Docs
description: Clasificación y detección de datos de SQL
documentationcenter: ''
ms.reviewer: vanto
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.prod_service: sql-database,sql
ms.custom: security
ms.topic: conceptual
ms.date: 06/10/2020
ms.author: datrigan
author: DavidTrigano
ms.openlocfilehash: 57ddcd78bd05cda262c9e4d041562dc5c2dfc4b7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236545"
---
# <a name="sql-data-discovery-and-classification"></a>Clasificación y detección de datos de SQL
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La clasificación y detección de datos cuenta con una nueva herramienta integrada en [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) para **detectar**, **clasificar**, **etiquetar** y **notificar** los datos confidenciales de las bases de datos.
La detección y clasificación de la información más confidencial (empresarial, financiera, sanitaria, etc.) pueden desempeñar un papel fundamental en el estado de protección de la información de la organización. Puede servir como infraestructura para lo siguiente:
* Ayudar a cumplir los estándares de privacidad de datos.
* Supervisar el acceso a bases de datos o columnas que contengan datos altamente confidenciales.

> [!NOTE]
> La clasificación y detección de datos se **admite para SQL Server 2012 y versiones posteriores, y se puede usar con [SSMS 17.5](../../ssms/download-sql-server-management-studio-ssms.md) o versiones posteriores**. Para Azure SQL Database, vea [Clasificación y detección de datos de Azure SQL Database](/azure/sql-database/sql-database-data-discovery-and-classification/).

## <a name="overview"></a><a id="subheading-1"></a>Información general
La característica Clasificación y detección de datos presenta un conjunto de servicios que forman un nuevo paradigma de SQL Information Protection pensado para proteger los datos, no solo la base de datos:

* **Detección y recomendaciones**: el motor de clasificación examina la base de datos e identifica las columnas que contienen datos potencialmente confidenciales. Después, proporciona una manera sencilla de revisar y aplicar las recomendaciones de clasificación apropiadas, así como de clasificar las columnas de forma manual.
* **Etiquetado**: las etiquetas de clasificación de confidencialidad se pueden etiquetar de forma persistente en columnas.
* **Visibilidad**: el estado de clasificación de la base de datos puede consultarse en un informe detallado que se puede imprimir o exportar para su uso con fines de cumplimiento y auditoría, así como para otras necesidades.

## <a name="discovering-classifying--labeling-sensitive-columns"></a><a id="subheading-2"></a>Detectar, clasificar y etiquetar columnas confidenciales
En la sección siguiente se describen los pasos necesarios para detectar, clasificar y etiquetar las columnas que contienen datos confidenciales en la base de datos, así como para ver el estado de clasificación actual de la base de datos y exportar informes.

La clasificación incluye dos atributos de metadatos:
* Etiquetas: son los atributos principales de clasificación, que se usan para definir el nivel de confidencialidad de los datos almacenados en la columna.  
* Tipos de información: proporcionan una granularidad adicional para el tipo de datos almacenados en la columna.

**Para clasificar la base de datos de SQL Server:**

1. En SQL Server Management Studio (SSMS), conéctese a SQL Server.

2. En el Explorador de objetos de SSMS, haga clic con el botón derecho en la base de datos que quiere clasificar y seleccione **Tareas** > **Detección y clasificación de datos** > **Clasificación de datos...** .

   ![Captura de pantalla en el que se muestra el Explorador de objetos de SSMS con la opción Tareas > Detección y clasificación de datos > Clasificar datos… seleccionada.][0]

3. El motor de clasificación examina la base de datos en busca de columnas que contengan datos potencialmente confidenciales y proporciona una lista de **clasificaciones de columna recomendadas**:

    * Para ver la lista de clasificaciones de columna recomendadas, haga clic en el cuadro de notificación de recomendaciones en la parte superior o en el panel de recomendaciones en la parte inferior de la ventana:

        ![Captura de pantalla en la que se muestra una notificación en la que se indica lo siguiente: Hemos encontrado 39 columnas con recomendaciones de clasificación. Haga clic aquí para verlas.][2]

        ![Captura de pantalla en la que se muestra una notificación en la que se indica lo siguiente: 39 columnas con recomendaciones de clasificación. Haga clic para verlas.][3]

    * Revise la lista de recomendaciones:
        * Para aceptar una recomendación para una columna específica, active la casilla en la columna izquierda de la fila correspondiente. También puede marcar *todas las recomendaciones* como aceptadas. Para ello, active la casilla del encabezado de la tabla de recomendaciones.

        * También puede cambiar el tipo de información y la etiqueta de confidencialidad recomendados mediante los cuadros desplegables.        

        ![Captura de pantalla en la que se muestra la lista de recomendaciones.][4]

    * Para aplicar las recomendaciones seleccionadas, haga clic en el botón azul **Accept selected recommendations** (Aceptar recomendaciones seleccionadas).

        ![Captura de pantalla del botón Aceptar las recomendaciones seleccionadas.][5]

4. También puede **clasificar manualmente** las columnas como alternativa a la clasificación basada en recomendaciones, o además de ella:

    * Haga clic en **Agregar clasificación** en el menú superior de la ventana.

        ![Captura de pantalla en la que se muestra el menú superior con la opción Agregar clasificación resaltada.][6]

    * En la ventana contextual que se abre, seleccione el esquema > tabla > columna que quiera clasificar, así como el tipo de información y la etiqueta de confidencialidad. Después, haga clic en el botón azul **Agregar clasificación** situado en la parte inferior de la ventana contextual.

        ![Captura de pantalla en la que se muestra la ventana contextual Agregar clasificación.][7]

5. Para completar la clasificación y etiquetar de forma persistente las columnas de la base de datos con los nuevos metadatos de clasificación, haga clic en **Guardar** en el menú superior de la ventana.

    ![Captura de pantalla en la que se muestra el menú superior con la opción Guardar resaltada.][8]


6. Para generar un informe con un resumen completo del estado de clasificación de la base de datos, haga clic en **Ver informe** en el menú superior de la ventana. (También puede generar un informe mediante SSMS. Haga clic con el botón derecho en la base de datos en la que quiera generar el informe y elija **Tareas** > **Detección y clasificación de datos** > **Generar informe...** ).

    ![Captura de pantalla en la que se muestra el menú superior con la opción Ver informe resaltada.][9]

    ![Captura de pantalla en la que se muestra el informe de clasificación de datos de SQL.][10]

## <a name="manage-information-protection-policy-with-ssms"></a><a id="subheading-3"></a>Administración de la directiva de Information Protection con SSMS

Puede administrar la directiva de Information Protection mediante [SSMS 18.4](../../ssms/download-sql-server-management-studio-ssms.md) o posterior:

1. En SQL Server Management Studio (SSMS), conéctese a SQL Server.

2. En el Explorador de objetos de SSMS, haga clic con el botón derecho en una de las bases de datos y elija **Tareas** > **Detección y clasificación de datos**.

   Las siguientes opciones de menú permiten administrar la directiva de Information Protection:

* **Establecer archivo de directiva de Information Protection**: usa la directiva de Information Protection tal y como se define en el archivo JSON seleccionado.

* **Exportar directiva de Information Protection**: exporta la directiva de Information Protection a un archivo JSON.

* **Restablecer directiva de Information Protection**: restablece la directiva de Information Protection predeterminada.

> [!IMPORTANT]
> El archivo de directiva de Information Protection no se almacena en SQL Server.
> SSMS usa una directiva de Information Protection predeterminada. Si se produce un error en una directiva de Information Protection personalizada, SSMS no puede usar la directiva predeterminada. No se puede realizar la clasificación de datos. Para resolverlo, haga clic en **Restablecer directiva de Information Protection** para usar la directiva predeterminada y vuelva a habilitar la clasificación de datos.

## <a name="accessing-the-classification-metadata"></a><a id="subheading-4"></a>Acceso a los metadatos de clasificación

SQL Server 2019 presenta la vista de catálogo del sistema [`sys.sensitivity_classifications`](../system-catalog-views/sys-sensitivity-classifications-transact-sql.md). Esta vista devuelve los tipos de información y las etiquetas de confidencialidad. 

> [!NOTE]
> Esta vista requiere el permiso **VIEW ANY SENSITIVITY CLASSIFICATION**. Para obtener más información, consulte [Metadata Visibility Configuration](./metadata-visibility-configuration.md).

En las instancias de SQL Server 2019, consulte `sys.sensitivity_classifications` para revisar todas las columnas clasificadas con sus clasificaciones correspondientes. Por ejemplo: 

```sql
SELECT 
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    label,
    rank,
    rank_desc
FROM sys.sensitivity_classifications sc
    JOIN sys.objects O
    ON  sc.major_id = O.object_id
    JOIN sys.columns C 
    ON  sc.major_id = C.object_id  AND sc.minor_id = C.column_id
```

Antes de SQL Server 2019, los metadatos de clasificación para tipos de información y etiquetas de confidencialidad se almacenaban en las propiedades extendidas siguientes: 

* `sys_information_type_name`
* `sys_sensitivity_label_name`

Se puede acceder a los metadatos mediante la vista de catálogo de propiedades extendidas [`sys.extended_properties`](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md).

Para las instancias de SQL Server 2017 y versiones anteriores, en el ejemplo de código siguiente se devuelven todas las columnas clasificadas con sus clasificaciones correspondientes:

```sql
SELECT
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    sensitivity_label 
FROM
    (
        SELECT
            IT.major_id,
            IT.minor_id,
            IT.information_type,
            L.sensitivity_label 
        FROM
        (
            SELECT
                major_id,
                minor_id,
                value AS information_type 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_information_type_name'
        ) IT 
        FULL OUTER JOIN
        (
            SELECT
                major_id,
                minor_id,
                value AS sensitivity_label 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_sensitivity_label_name'
        ) L 
        ON IT.major_id = L.major_id AND IT.minor_id = L.minor_id
    ) EP
    JOIN sys.objects O
    ON  EP.major_id = O.object_id 
    JOIN sys.columns C 
    ON  EP.major_id = C.object_id AND EP.minor_id = C.column_id
```

## <a name="manage-classifications"></a><a id="subheading-5"></a>Administración de clasificaciones

# <a name="t-sql"></a>[T-SQL](#tab/t-sql)
Puede utilizar T-SQL para agregar o quitar las clasificaciones de columna, así como recuperar todas las clasificaciones para toda la base de datos.

- Agregue o actualice la clasificación de una o varias columnas: [ADD SENSITIVITY CLASSIFICATION](../../t-sql/statements/add-sensitivity-classification-transact-sql.md) (Agregar clasificación de confidencialidad)
- Quite la clasificación de una o varias columnas: [DROP SENSITIVITY CLASSIFICATION](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md) (Eliminar clasificación de la confidencialidad)

# <a name="powershell-cmdlet"></a>[Cmdlet de PowerShell](#tab/sql-powelshell)
Puede usar cmdlet de PowerShell para agregar o quitar las clasificaciones de columna, así como recuperar todas las clasificaciones y obtener recomendaciones para toda la base de datos.

- [Get-SqlSensitivityClassification](/powershell/module/sqlserver/Get-SqlSensitivityClassification)
- [Get-SqlSensitivityRecommendations](/powershell/module/sqlserver/Get-SqlSensitivityRecommendations)
- [Set-SqlSensitivityClassification](/powershell/module/sqlserver/Set-SqlSensitivityClassification)
- [Remove-SqlSensitivityClassification](/powershell/module/sqlserver/Remove-SqlSensitivityClassification)

## <a name="next-steps"></a><a id="subheading-6"></a>Pasos siguientes

Para Azure SQL Database, vea [Clasificación y detección de datos de Azure SQL Database](/azure/azure-sql/database/data-discovery-and-classification-overview).

Considere la posibilidad de proteger sus columnas confidenciales mediante la aplicación de mecanismos de seguridad en el nivel de columna:

* [Enmascaramiento dinámico de datos](./dynamic-data-masking.md) para ofuscar columnas confidenciales en uso.
* [Always Encrypted](./encryption/always-encrypted-database-engine.md) para cifrar columnas confidenciales en reposo.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Manage information protection policy with SSMS]: #subheading-3
[Accessing the classification metadata]: #subheading-4
[Manage classifications]: #subheading-5
[Next Steps]: #subheading-6

<!--Image references-->

[0]: ./media/sql-data-discovery-and-classification/0-data-classification-explorer.png
[2]: ./media/sql-data-discovery-and-classification/2-recommendations-notification-box.png
[3]: ./media/sql-data-discovery-and-classification/3-recommendations-panel.png
[4]: ./media/sql-data-discovery-and-classification/4-recommendations.png
[5]: ./media/sql-data-discovery-and-classification/5-accept-recommendations-button.png
[6]: ./media/sql-data-discovery-and-classification/6-add-classification-button.png
[7]: ./media/sql-data-discovery-and-classification/7-manual-classification.png
[8]: ./media/sql-data-discovery-and-classification/8-save.png
[9]: ./media/sql-data-discovery-and-classification/9-view-report.png
[10]: ./media/sql-data-discovery-and-classification/10-report.png
