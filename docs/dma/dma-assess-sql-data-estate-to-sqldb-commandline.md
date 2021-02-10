---
title: 'DMACMD: evaluar SQL Server preparación para migrar a Azure SQL'
titleSuffix: Data Migration Assistant
description: Aprenda a usar Data Migration Assistant herramienta de línea de comandos (DMACMD) para evaluar una SQL Server de datos para la migración a Azure SQL
ms.date: 10/02/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: ''
ms.openlocfilehash: 8688fa61a1ccaede3bd370e1d19ccd66531f5739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060940"
---
# <a name="dmacmd-assess-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql"></a>DMACMD: evaluar la preparación de una SQL Server la migración de datos a Azure SQL 

Con muchas organizaciones que intentan migrar a Azure, es fundamental evaluar las instancias de SQL Server locales existentes e identificar el Azure SQL Database de destino de Azure SQL, Azure SQL Instancia administrada o SQL Server en máquinas virtuales de Azure. 

[Data Migration Assistant (DMA)](dma-overview.md) ayuda a evaluar una instancia de SQL Server para un destino SQL de Azure específico y mide la preparación de SQL Server las bases de datos que se migran a Azure SQL. Cargue los resultados de la evaluación de DMA en Azure Migrate hub para obtener una vista de preparación centralizada de toda la información de los datos. 

En este artículo se explica cómo realizar evaluaciones a escala y cargar los resultados en Azure Migrate Hub mediante la interfaz de línea de comandos (DMACMD) de DMA. Como alternativa, puede usar la [GUI de DMA](dma-assess-sql-data-estate-to-sqldb.md) para realizar la evaluación en su lugar.

Para obtener más información, consulte el siguiente vídeo de Channel9:

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-to-Assess-Readiness-of-SQL-Server-Data-Estate-Migrating-to-Azure-SQL/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Prerrequisitos 

Para usar DMACMD para realizar una evaluación y cargar los resultados en Azure Migrate Hub, necesita lo siguiente: 

- La [versión más reciente de Data Migration Assistant (DMA)](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
- Un [proyecto de Azure Migrate](dma-assess-sql-data-estate-to-sqldb.md#create-a-project-and-add-a-tool). 
- Rol de colaborador: acceso al recurso de proyecto de Azure Migrate.

## <a name="use-dmacmd"></a>Usar DMACMD

Use un archivo XML como entrada para ejecutar evaluaciones a escala mediante la interfaz de línea de comandos (DMACMD.exe). 

Use el siguiente comando de ejemplo para pasar un archivo XML a DMACMD e iniciar la evaluación:

```console
C:\Program Files\Microsoft Data Migration Assistant\DmaCmd.exe /Action=Assess /AssessmentConfiguration= C:\Demo\ScaleAssessment\Assess-for-AzureSQLMI.xml
```

El contenido del ejemplo `Assess-for-AzureSQLMI.xml` define los elementos para evaluar SQL Server instancias para un destino de SQL instancia administrada: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<AssessmentConfiguration xmlns="http://microsoft.com/schemas/SqlServer/Advisor/AssessmentConfiguration">
   <AssessmentName>Scale-Assessment-for-AzureSQLManagedInstance</AssessmentName>
   <AssessmentSourcePlatform>SqlOnPrem</AssessmentSourcePlatform>
   <AssessmentTargetPlatform>ManagedSqlServer</AssessmentTargetPlatform>
   <AssessmentDatabases>
      <AssessmentDatabase>Server=ServerName\SQL2017;Integrated Security=true</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=AdventureWorks2016</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=TestDB</AssessmentDatabase>
   </AssessmentDatabases>
   <AssessmentResultDma>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.dma</AssessmentResultDma>
   <AssessmentResultJson>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.json</AssessmentResultJson>
   <AssessmentResultCsv>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.csv</AssessmentResultCsv>
   <AssessmentOverwriteResult>true</AssessmentOverwriteResult>
   <AssessmentEvaluateCompatibilityIssues>true</AssessmentEvaluateCompatibilityIssues>
   <AssessmentEvaluateFeatureParity>true</AssessmentEvaluateFeatureParity>
   <AzureCloudEnvironment>Azure</AzureCloudEnvironment>
   <SubscriptionId>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx</SubscriptionId>
   <AzureMigrateProjectName>Scale-Assessment-for-AzureSQLMI</AzureMigrateProjectName>
   <ResourceGroupName>Resource-Group-Name</ResourceGroupName>
   <AzureAuthenticationInteractiveAuthentication>true</AzureAuthenticationInteractiveAuthentication>
   <AzureAuthenticationTenantId>xxxxxxxx-xxxx-xxxxxxxx</AzureAuthenticationTenantId>
   <EnableAssessmentUploadToAzureMigrate>true</EnableAssessmentUploadToAzureMigrate>
</AssessmentConfiguration>
```



## <a name="xml-elements"></a>Elementos XML 

Los elementos XML que se pasan a DMACMD se definen en la tabla siguiente: 


|**Elemento XML** |**Definition**  |
|---------|---------|
|`AssessmentName`|El nombre de la evaluación.|
|`AssessmentSourcePlatform`|Plataforma de SQL Server de origen. El valor predeterminado es `SqlOnPrem`.|
|`AssessmentTargetPlatform`|Plataforma de SQL Server de destino.  </br> `AzureSqlDatabase` es para un destino de Azure SQL Database. </br> `ManagedSqlServer` es para un destino de Azure SQL Instancia administrada. </br></br>El ejemplo **Evaluate-for-AzureSQLMI** evalúa un destino de SQL instancia administrada.|
|`AssessmentDatabases`|Si necesita evaluar todas las bases de datos de una instancia de, especifique solo el nombre de la instancia, en la lista, las bases de datos específicas en cada línea. </br></br>El ejemplo **Evaluate-for-AzureSQLMI** evalúa todas las bases de datos de la instancia `Servername\SQL2017` y dos bases de datos específicas en la instancia `Servername\SQL2016` .  |
|`AssessmentResultDma` </br> `AssessmentResultJson` </br> `AssessmentResultCsv` | Especifica el formato del archivo de resultados. `.DMA`, `.JSON` y, `.CSV` respectivamente. Haga doble clic `.DMA` para abrir en la interfaz de usuario DMA. <br> `AssessmentResultDma` se requiere para cargar los resultados de la evaluación en Azure Migrate Hub.  |
|`AssessmentOverwriteResult`| Indica si se va a sobrescribir cualquier archivo de resultados de evaluación existente con la misma ruta de acceso que `AssessmentResultJson` , `AssessmentResultDma` o `AssessmentResultCsv` .|
|`AssessmentEvaluateCompatibilityIssues` </br> `AssessmentEvaluateFeatureParity` |Realice una evaluación para evaluar los problemas de compatibilidad y la paridad de características, respectivamente.|
|`AzureCloudEnvironment`|Entorno de nube de Azure para conectarse a, el valor predeterminado es nube pública de Azure. </br></br> Valores admitidos: </br>`Azure (default)`, `AzureChina`, `AzureGermany`, `AzureUSGovernment`.|
|`SubscriptionId`|Identificador de suscripción de Azure|
|`AzureMigrateProjectName`|Azure Migrate nombre del proyecto al que cargar los resultados de la evaluación.|
|`ResourceGroupName`|Azure Migrate nombre del grupo de recursos.|
|`AzureAuthenticationInteractiveAuthentication`|Establezca en `true` para que aparezca la ventana de autenticación.|
|`AzureAuthenticationTenantId`|Identificador del inquilino de Azure Active Directory. </br></br>Obtenga esto de la hoja de **información general** de Azure Active Directory en el [Azure portal](https://portal.azure.com). |
|`EnableAssessmentUploadToAzureMigrate`| Establezca en `true` para cargar y publicar los resultados de la evaluación en Azure Migrate Hub.|
|   |   |


## <a name="results"></a>Results 

DMACMD genera un estado cuando finaliza correctamente. 


A continuación se muestra un resultado de ejemplo: 

```console
Assessment finished for project: Scale-Assessment-for-AzureSQLManagedInstance
DATABASES:
Succeeded             : 4
Failed                : 0
SERVER INSTANCES:
Succeeded             : 2
Failed                : 0
CSV result file       : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.csv
JSON result file      : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.json
--------------------------------------------------------------------------------
```

Vea los resultados cargados en [Azure Migrate](dma-assess-sql-data-estate-to-sqldb.md#view-target-readiness-assessment-results) para obtener una vista centralizada de toda la información de los datos. . 

## <a name="best-practices"></a>Procedimientos recomendados 

Tenga en cuenta las siguientes prácticas recomendadas al usar DMACMD: 

- Agrupe lógicamente las instancias y bases de datos de SQL Server de destino en función de la aplicación, en lugar de evaluar todas las instancias SQL Servers en toda la información. 
- Cree un proyecto de Azure Migrate independiente para cada destino SQL de Azure para evitar sobrescribir los resultados. 
- El tiempo de ejecución de una evaluación depende del número de objetos de base de datos. Si es posible, evite ejecutar evaluaciones en el sistema de producción y descargarlas en un servidor de ensayo o máquina virtual en su lugar, especialmente en el caso de las bases de datos con un gran número de objetos. 


## <a name="see-also"></a>Consulte también

* [Data Migration Assistant (DMA)](../dma/dma-overview.md)
* [Data Migration Assistant: opciones de configuración](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedimientos recomendados](../dma/dma-bestpractices.md)

