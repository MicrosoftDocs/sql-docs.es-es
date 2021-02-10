---
description: Generación de informes (AccessToSQL)
title: Generación de informes (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: abb4264a-622e-4215-af5b-14e309b8a399
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1e80761c63aec6461af497af696a059ab7b86f60
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044543"
---
# <a name="generating-reports-accesstosql"></a>Generación de informes (AccessToSQL)
Los informes de ciertas actividades realizadas con comandos se generan en la consola de SSMA en el nivel de árbol de objetos.  
  
Use el procedimiento siguiente para generar informes:  
  
1.  Especifique el parámetro **Write-Summary-Report-to** . El informe relacionado se almacena como el nombre de archivo (si se especifica) o en la carpeta que especifique. El nombre de archivo está predefinido por el sistema, como se mencionó en la tabla siguiente, donde **&lt; n &gt;** es el número de archivo único que se incrementa con un dígito con cada ejecución del mismo comando.  
  
    Los comandos de los informes Vis-à-vis son los siguientes:  
  
    |SL. No.|Get-Help|Título del informe|  
    |-|-|-|  
    |1|generar informe de evaluación|AssessmentReport &lt; n &gt; . LENGUAJE|  
    |2|Convert-Schema|SchemaConversionReport &lt; n &gt; . LENGUAJE|  
    |3|migrar datos|DataMigrationReport &lt; n &gt; . LENGUAJE|  
    |4|sincronizar-destino|TargetSynchronizationReport &lt; n &gt; . LENGUAJE|  
    |5|actualizar desde la base de datos|SourceDBRefreshReport &lt; n &gt; . LENGUAJE|  
  
    > [!IMPORTANT]  
    > Un informe de salida es diferente del informe de evaluación. El primero es un informe sobre el rendimiento de un comando ejecutado, mientras que el segundo es un informe XML para el consumo mediante programación.  
  
    Para las opciones de comando para los informes de salida (desde SL. No. 2-4 arriba), consulte la sección [ejecución de la consola de SSMA &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md) .  
  
2.  Indique el grado de detalle que desea en el informe de salida mediante la configuración del nivel de detalle del informe:  
  
    |SL. No.|Comando y parámetro|Descripción de salida|  
    |-|-|-|  
    |1|verbose = "false"|Genera un informe resumido de la actividad.|  
    |2|verbose = "true"|Genera un informe de estado resumido y detallado para cada actividad.|  
  
    > [!NOTE]  
    > La configuración de nivel de detalle del informe especificada anteriormente es aplicable a los comandos Generate-Assessment-Report, Convert-Schema, Migrate-Data.  
  
3.  Indique el grado de detalle que desea en los informes de errores mediante la configuración de informes de errores:  
  
    |SL. No.|Comando y parámetro|Descripción de salida|  
    |-|-|-|  
    |1|Informe-errores = "false"|No hay detalles de mensajes de error, ADVERTENCIA o información.|  
    |2|Informe-errores = "true"|Mensajes de error, ADVERTENCIA o información detallados.|  
  
    > [!NOTE]  
    > La configuración de informes de errores especificada anteriormente es aplicable a los comandos Generate-Assessment-Report, Convert-Schema, Migrate-Data.  
  
**Ejemplo**:  
  
```xml  
<generate-assessment-report  
  
    object-name="testschema"  
  
    object-type="Schemas"  
  
    verbose="yes"  
  
    report-erors="yes"  
  
    write-summary-report-to="$AssessmentFolder$\Report1.xml"  
  
    assessment-report-folder="$AssessmentFolder$\assesment_report"  
  
    assessment-report-overwrite="true"  
  
/>  
```  
  
### <a name="synchronize-target"></a>sincronizar-destino:  
El comando **Synchronize-Target** tiene el parámetro **Report-Errors-to** , que especifica la ubicación del informe de errores para la operación de sincronización. A continuación, un archivo por el nombre **TargetSynchronizationReport &lt; n &gt; . XML** se crea en la ubicación especificada, donde **&lt; n &gt;** es el número de archivo único que se incrementa con un dígito con cada ejecución del mismo comando.  
  
**Nota:** Si se especifica la ruta de acceso de la carpeta, el parámetro "Report-Errors-to" se convierte en un atributo opcional para el comando "Synchronize-target".  
  
```xml  
<!-- Example: Synchronize target entire Database with all attributes-->  
  
<synchronize-target  
  
    object-name="$TargetDB$.dbo"  
  
    on-error="fail-script"  
  
    report-errors-to="$SynchronizationReports$"  
  
/>  
```  
**nombre del objeto:** Especifica los objetos que se tienen en cuenta para la sincronización (también puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
**error:** Especifica si se deben especificar errores de sincronización como advertencias o errores. Opciones disponibles para en caso de error:  
  
-   Informe-total como ADVERTENCIA  
  
-   Informe cada ADVERTENCIA  
  
-   error: script  
  
### <a name="refresh-from-database"></a>actualizar desde la base de datos:  
El comando **Refresh-from-Database** tiene el parámetro **Report-Errors-to** , que especifica la ubicación del informe de errores para la operación de actualización. A continuación, un archivo por el nombre **SourceDBRefreshReport &lt; n &gt; . XML** se crea en la ubicación especificada, donde **&lt; n &gt;** es el número de archivo único que se incrementa con un dígito con cada ejecución del mismo comando.  
  
**Nota:** Si se especifica la ruta de acceso de la carpeta, el parámetro "Report-Errors-to" se convierte en un atributo opcional para el comando "Synchronize-target".  
  
```xml  
<!-- Example: Refresh entire Schema (with all attributes)-->  
  
<refresh-from-database  
  
    object-name="$SourceDatabaseStandard$"  
  
    object-type ="Databases"  
  
    on-error="fail-script"  
  
    report-errors-to="$RefreshDBFolder$\RefreshReport.xml"  
  
/>  
```  
**nombre del objeto:** Especifica los objetos que se tienen en cuenta para la actualización (también puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
**error:** Especifica si se deben especificar errores de actualización como advertencias o errores. Opciones disponibles para en caso de error:  
  
-   Informe-total como ADVERTENCIA  
  
-   Informe cada ADVERTENCIA  
  
-   error: script  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar la consola SSMA (acceso)](./executing-the-ssma-console-accesstosql.md)  
