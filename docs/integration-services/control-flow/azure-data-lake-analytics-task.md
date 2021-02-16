---
title: Tarea de Azure Data Lake Analytics | Microsoft Docs
description: Puede enviar trabajos de U-SQL al servicio Azure Data Lake Analytics con la tarea de Data Lake Analytics.
ms.custom: ''
ms.date: 06/27/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSTASK.F1
- SQL14.DTS.DESIGNER.AFPADLSTASK.F1
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: 2bc717a0dc59c0d880e7095e18a0c97432519e56
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346576"
---
# <a name="azure-data-lake-analytics-task"></a>Tarea de Azure Data Lake Analytics

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Puede enviar trabajos de U-SQL al servicio Azure Data Lake Analytics con la tarea de Data Lake Analytics. Esta tarea es un componente de [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

Para información general, consulte [Azure Data Lake Analytics](https://azure.microsoft.com/services/data-lake-analytics/).

## <a name="configure-the-task"></a>Configuración de la tarea

Para agregar una tarea de Data Lake Analytics a un paquete, arrástrela desde el cuadro de herramientas de SSIS al lienzo del diseñador. Luego haga doble clic en la tarea o haga clic con el botón derecho en ella y seleccione **Editar**. Se abre el cuadro de diálogo **Azure Data Lake Analytics Task Editor** (Editor de la tarea de Azure Data Lake Analytics). Puede establecer propiedades a través del Diseñador de SSIS o mediante programación.

## <a name="general-page-configuration"></a>Configuración de la página General

Use la página **General** para configurar la tarea y proporcione el script U-SQL que la tarea envía. Para más información sobre el lenguaje U-SQL, consulte el artículo sobre la [referencia del lenguaje U-SQL](/u-sql/).

### <a name="basic-configuration"></a>Configuración básica

Puede especificar el nombre y la descripción de la tarea.

### <a name="u-sql-configuration"></a>Configuración de U-SQL

La configuración de U-SQL tiene dos valores: **SourceType** y opciones dinámicas en función del valor de **SourceType**. 

**SourceType**: especifica el origen del script de U-SQL. El script se envía a una cuenta de Data Lake Analytics durante la ejecución del paquete SSIS. Las opciones de esta propiedad son las siguientes:

|Value|Descripción|  
|-----------|-----------------|  
|**DirectInput**|Especifica el script de U-SQL a través del editor insertado. Si selecciona este valor, se mostrará la opción dinámica **USQLStatement**.|  
|**FileConnection**|Especifica un archivo .usql local que contiene el script de U-SQL. Si selecciona esta opción, se mostrará la opción dinámica **FileConnection**.|  
|**Variable**|Especifica una variable SSIS que contiene el script de U-SQL. Si selecciona este valor, se mostrará la opción dinámica **SourceVariable**.|
| &nbsp; | &nbsp; |

**SourceType Dynamic Options** (Opciones dinámicas de SourceType) especifica el contenido del script de la consulta U-SQL. 

|SourceType|Opciones dinámicas|  
|-----------|-----------------|  
|**SourceType = DirectInput**|Escriba la consulta U-SQL que se enviará directamente en el cuadro de opción o seleccione el botón Examinar (...) para escribir la consulta U-SQL en el cuadro de diálogo **Enter U-SQL Query** (Escribir consulta U-SQL).|  
|**SourceType = FileConnection**|Seleccione un administrador de conexiones de archivos existente o seleccione <**Nueva conexión...** > para crear una nueva conexión de archivos. Para información relacionada, consulte [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md) y [Editor del administrador de conexiones de archivos](../connection-manager/file-connection-manager.md).|  
|**SourceType = Variable**|Seleccione una variable existente o seleccione \<**New variable...**> para crear una nueva. Para información relacionada, consulte [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) y [Agregar variable](../integration-services-ssis-variables.md).|
| &nbsp; | &nbsp; |


### <a name="job-configuration"></a>Configuración del trabajo
La configuración del trabajo especifica las propiedades del envío del trabajo de U-SQL.

- **AzureDataLakeAnalyticsConnection**: especifica la cuenta de Data Lake Analytics donde se envía el script U-SQL. Elija la conexión en la lista de administradores de conexión definidos. Para crear una conexión, seleccione <**Nueva conexión**>. Para información relacionada, consulte [Administrador de conexiones de Azure Data Lake Analytics](../../integration-services/connection-manager/azure-data-lake-analytics-connection-manager.md).

- **JobName**: especifica el nombre del trabajo de U-SQL. 
- **AnalyticsUnits**: especifica el número de unidades de análisis del trabajo de U-SQL.
- **Priority**: especifica la prioridad del trabajo de U-SQL. Este valor se puede establecer entre 0 y 1000. Cuanto menor sea el número, mayor será la prioridad.
- **RuntimeVersion**: especifica la versión en tiempo de ejecución de Data Lake Analytics del trabajo de U-SQL. De manera predeterminada, está establecido en "default". Por lo general, no es necesario modificar esta propiedad.
- **Synchronous**: un valor booleano especifica si la tarea espera que se complete la ejecución del trabajo o no. Si el valor se establece en true, la tarea se marca como **correcta** después de que se completa el trabajo. Si el valor se establece en false, la tarea se marca como **correcta** después de que el trabajo pasa la fase de preparación.

  |Value|Descripción|
  |-----------|-----------------|
  |True|El resultado de la tarea se basa en el resultado de la ejecución del trabajo de U-SQL. El trabajo se realiza correctamente > La tarea se realiza correctamente. El trabajo no se realiza > La tarea no se realiza. La tarea se realiza correctamente o no se realiza > La tarea se completa.|
  |False|El resultado de la tarea se basa en el resultado de la preparación y el envío del trabajo de U-SQL. El envío del trabajo se realiza correctamente y pasa la fase de preparación > La tarea se realiza correctamente. El envío del trabajo no se realiza o el trabajo no pasa la fase de preparación > La tarea no se realiza. La tarea se realiza correctamente o no se realiza > La tarea se completa.|
  | &nbsp; | &nbsp; |

- **TimeOut**: especifica un tiempo de espera en segundos para la ejecución del trabajo. Si se agota el tiempo de espera del trabajo, este se cancela y se marca como no realizado correctamente. Esta propiedad no está disponible si **Synchronous** está establecido en false.

## <a name="parameter-mapping-page-configuration"></a>Configuración de la página de asignación de parámetros

Use la página **Asignación de parámetros** del cuadro de diálogo **Azure Data Lake Analytics Task Editor** (Editor de la tarea de Azure Data Lake Analytics) para asignar variables a los parámetros (variables U-SQL) en el script U-SQL.

- **Nombre de variable**: una vez que agrega una asignación de parámetro mediante al seleccionar **Agregar**, seleccione en la lista una variable de sistema o una variable definida por el usuario. De manera alternativa, puede seleccionar <**Nueva variable...** > para agregar una variable nueva con el cuadro de diálogo **Agregar variable**. Para información relacionada, consulte [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  

- **Nombre de parámetro:** proporcione un nombre de variable o de parámetro en el script de U-SQL. Asegúrese de que el nombre del parámetro empieza con el signo \@, como \@Param1. 

Este es un ejemplo de cómo pasar parámetros al script de U-SQL.

**Script de U-SQL de ejemplo**
```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Tenga en cuenta que las rutas de acceso de entrada y salida están definidas en los parámetros **\@in** y **\@out**. Los valores de los parámetros **\@in** y **\@out** del script U-SQL se pasan de manera dinámica mediante la configuración de la asignación de parámetros.

|Nombre de la variable|Nombre de parámetro|
|-------------|--------------|
|Usuario: Variable1|\@in|
|Usuario: Variable2|\@out| 
| &nbsp; | &nbsp; |

## <a name="expression-page-configuration"></a>Configuración de la página de expresión

Puede asignar todas las propiedades de la configuración de la página General como una expresión de propiedad para habilitar la actualización dinámica de la propiedad en el runtime. Para información relacionada, consulte [Usar expresiones de propiedad en paquetes](../../integration-services/expressions/use-property-expressions-in-packages.md).

## <a name="see-also"></a>Consulte también
- [Administrador de conexiones de Azure Data Lake Analytics](../../integration-services/connection-manager/azure-data-lake-analytics-connection-manager.md)
- [Tarea Sistema de archivos de Azure Data Lake Store](../../integration-services/control-flow/azure-data-lake-store-file-system-task.md)
- [Administrador de conexiones de Azure Data Lake Store](../../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)