---
title: API SQL Server Assessment
description: Obtenga información sobre la API SQL Assessment, que ofrece un mecanismo para evaluar la configuración de la instancia de SQL Server a efectos de los procedimientos recomendados.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 3/3/2021
ms.openlocfilehash: 7e55abe0d02a9f9deffffdaa7639d911faad1fa3
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102185752"
---
# <a name="sql-assessment-api"></a>API de SQL Assessment

La API SQL Server Assessment ofrece un mecanismo para evaluar la configuración de la instancia de SQL Server a efectos de los procedimientos recomendados. La API se entrega con un conjunto de reglas que contiene las reglas de procedimientos recomendados sugeridas por el equipo de SQL Server. Este conjunto de reglas se ha mejorado con el lanzamiento de nuevas versiones, pero cabe destacar que, al mismo tiempo, la API se ha diseñado con el objetivo de proporcionar una solución altamente personalizable y extensible. Por lo tanto, los usuarios pueden ajustar las reglas predeterminadas y crear las suyas propias.

La API SQL Server Assessment resulta útil si quiere asegurarse de que la configuración de SQL Server esté en consonancia con los procedimientos recomendados. Después de una valoración inicial, se puede realizar un seguimiento de la estabilidad de la configuración mediante evaluaciones programadas periódicamente.

La API se puede usar para evaluar lo siguiente:
 
* SQL Server en Azure Virtual Machines

* Instancia administrada de Azure SQL Database

* SQL Server 2012 y versiones posteriores

* SQL en sistemas basados en Linux

La extensión SQL Assessment de SQL Server también usa la API para Azure Data Studio (ADS).

## <a name="rules"></a>Reglas

Las reglas, a las que a veces se hace referencia como "comprobaciones", se definen en archivos con formato JSON. El formato del conjunto de reglas requiere que se especifiquen su nombre y versión. Al usar conjuntos de reglas personalizados, podrá saber fácilmente qué recomendaciones proceden de los distintos conjunto de reglas.

El conjunto de reglas de Microsoft está disponible en GitHub. Puede visitar el [repositorio de ejemplos](https://aka.ms/sql-assessment-api) para obtener más detalles.

## <a name="sql-assessment-cmdlets-and-associated-extensions"></a>Cmdlets de SQL Assessment y extensiones asociadas

La API de SQL Assessment forma parte de:

* [Azure Data Studio (ADS)](../../azure-data-studio/what-is-azure-data-studio.md)

    Versión de junio de 2020 y posteriores.

* [Objetos de administración de SQL Server (SMO)](../../relational-databases/server-management-objects-smo/installing-smo.md)

    Versión de julio de 2019 y posteriores.

* [Módulo de SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md)

    Versión de julio de 2019 y posteriores.

Antes de empezar a usar la API de SQL Assessment, asegúrese de lo siguiente:

* [Instalación de ADS](https://techcommunity.microsoft.com/t5/sql-server/released-sql-server-assessment-extension-for-azure-data-studio/ba-p/1470603)

* [Instalación de SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

* [Instalación de un módulo de SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md)

El módulo SqlServer incluye dos nuevos cmdlets para trabajar con la API de la extensión SQL Assessment de SQL Server:

* **Get-SqlAssessmentItem**: ofrece una lista de las comprobaciones de valoración disponibles para un objeto SQL Server.

* **Invoke-SqlAssessment**: facilita los resultados de una valoración.

El marco de SMO se complementa con la extensión de la API SQL Server Assessment que proporciona los métodos siguientes:

* **GetAssessmentItems**: devuelve las comprobaciones disponibles para un objeto de SQL Server determinado (IEnumerable<…>).

* **GetAssessmentResults**: evalúa sincrónicamente la valoración, y devuelve los resultados y errores, si los hay (IEnumerable<…>).

* **GetAssessmentResultsList**: evalúa asincrónicamente la valoración, y devuelve los resultados y errores, si los hay (Task<…>).

## <a name="get-started-using-sql-assessment-cmdlets"></a>Introducción al uso de cmdlets de SQL Server Assessment

Se realiza una valoración en un objeto de SQL Server determinado. En el conjunto de reglas predeterminado, solo hay comprobaciones para dos tipos de objetos: Server y Database (además de ellos, la API admite dos tipos más, Filegroup y AvailabilityGroup). Si quiere evaluar una instancia de SQL y todas sus bases de datos, debe ejecutar los cmdlets de SQL Server Assessment para cada objeto por separado. También puede pasar objetos para su valoración a los cmdlets de SQL Server Assessment en una variable o la canalización.

Los objetos SqlServer y RegisteredServer son intercambiables, por lo que puede pasar cualquiera a los cmdlets de SQL Server Assessment.

Repase estos ejemplos para empezar.

1. Obtenga una lista de las comprobaciones disponibles para una instancia predeterminada local a fin de familiarizarse con las comprobaciones. En este ejemplo, se canaliza la salida del cmdlet Get-SqlInstance al cmdlet Get-SqlAssessmentItem para pasarle el objeto de la instancia.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

2. Obtenga una lista de las comprobaciones disponibles para todas las bases de datos de la instancia. Aquí se va a usar el cmdlet Get-Item y una ruta de acceso implementada con el proveedor de SQL Server de Windows PowerShell para obtener una lista de las bases de datos y, luego, canalizarla con el cmdlet Get-SqlDatabase.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    Además, puede usar el cmdlet Get-SqlDatabase para hacer lo mismo.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

3. Obtenga una lista de las comprobaciones disponibles para todas las bases de datos de la instancia. Aquí se va a usar el cmdlet Get-Item y una ruta de acceso implementada con el proveedor de SQL Server de Windows PowerShell para obtener una lista de las bases de datos y, luego, canalizarla con el cmdlet Get-SqlDatabase.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    Además, puede usar el cmdlet Get-SqlDatabase para hacer lo mismo.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

4. Invoque la valoración de la instancia y guarde los resultados en una tabla SQL. En este ejemplo, se canaliza la salida del cmdlet Get-SqlInstance al cmdlet Invoke-SqlAssessment, cuyos resultados se canalizan al cmdlet Write-SqlTableData. En este ejemplo, el cmdlet Invoke-Assessment se ejecuta con el parámetro `-FlattenOutput`. Este parámetro hace que la salida sea adecuada para el cmdlet Write-SqlTableData. En el último caso, se produce un error si se omite el parámetro.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

    Ahora vamos a invocar una valoración para todas las bases de datos de la instancia y agregar los resultados a la misma tabla.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

5. Siga las descripciones y los vínculos de la tabla para comprender mejor las recomendaciones.

6. Personalice las reglas en función de su entorno y los requisitos de la organización; puede consultarlos a continuación.

7. Programe una tarea o un trabajo para ejecutar la valoración periódicamente o a petición para medir el progreso.

## <a name="customizing-rules"></a>Personalización de las reglas

Las reglas están diseñadas para ser personalizables y extensibles. El conjunto de reglas de Microsoft está diseñado para funcionar en la mayoría de los entornos. Sin embargo, es imposible tener un conjunto de reglas que funcione con todos los entornos. Los usuarios pueden escribir sus propios archivos JSON y personalizar las reglas existentes o agregar otras nuevas. En el [repositorio de ejemplos](https://aka.ms/sql-assessment-api) hay disponibles ejemplos de personalización y un conjunto de reglas completo publicado por Microsoft. Para obtener más información sobre cómo ejecutar los cmdlets de SQL Server Assessment con archivos JSON personalizados, use el cmdlet Get-Help.

### <a name="options-available-with-rule-customization-feature"></a>Opciones disponibles con la característica de personalización de reglas

#### <a name="enablingdisabling-certain-rules-or-groups-of-rules-using-tags"></a>Habilitación y deshabilitación de ciertas reglas o grupos de reglas (mediante etiquetas)

Puede silenciar reglas específicas cuando no sean aplicables a su entorno o hasta que se realicen trabajos programados para rectificar el problema.

#### <a name="changing-threshold-parameters"></a>Cambio de parámetros de umbral

Las reglas específicas tienen umbrales que se comparan con el valor actual de una métrica para averiguar un problema. Si los umbrales predeterminados no se ajustan a sus necesidades, puede cambiarlos.

#### <a name="adding-more-rules-written-by-you-or-third-parties"></a>Incorporación de otras reglas escritas por usted o por terceros

Puede encadenar conjuntos de reglas agregando uno o más archivos JSON como parámetros a la llamada API SQL Server Assessment. Su organización podría escribir esos archivos u obtenerlos de un tercero. Por ejemplo, puede tener el archivo JSON que deshabilite reglas específicas del conjunto de reglas de Microsoft y otro archivo JSON de un experto del sector que incluya reglas que le resulten útiles para su entorno, seguido de otro archivo JSON que cambie algunos valores de umbral en ese archivo JSON.

> [!IMPORTANT]  
> Le recomendamos que no utilice conjuntos de reglas que provengan de fuentes que no sean de confianza hasta que los haya revisado detenidamente para asegurarse de que sean seguros.

## <a name="next-steps"></a>Pasos siguientes

* [Objetos de administración de SQL Server (SMO)](../../relational-databases/server-management-objects-smo/overview-smo.md)
* [PowerShell](../../powershell/download-sql-server-ps-module.md).
