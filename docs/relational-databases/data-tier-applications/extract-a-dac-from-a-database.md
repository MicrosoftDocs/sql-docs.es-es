---
description: Extraer una DAC de una base de datos
title: Extraer una DAC de una base de datos | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.extractdacwizard.validationandsummary.f1
- sql13.swb.extractdacwizard.introduction.f1
- sql13.swb.extractdacwizard.selectdatapage.f1
- sql13.swb.extractdacwizard.setdacproperties.f1
- sql13.swb.extractdacwizard.buildandsave.f1
helpviewer_keywords:
- extract DAC
- How to [DAC], extract
- data-tier application [SQL Server], extract
- wizard [DAC], extract
ms.assetid: ae52a723-91c4-43fd-bcc7-f8de1d1f90e5
author: stevestein
ms.author: sstein
ms.openlocfilehash: 5091c3437d0e74f3c108e13efb3789997cadb2d3
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489469"
---
# <a name="extract-a-dac-from-a-database"></a>Extraer una DAC de una base de datos
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Use el **Asistente para extraer aplicación de capa de datos** o un script de Windows PowerShell para extraer un paquete de aplicación de capa de datos (DAC) de una base de datos de SQL Server existente. El proceso de extracción crea un archivo de paquete DAC que contiene definiciones de los objetos de base de datos y sus elementos relacionados a nivel de instancia. Por ejemplo, un archivo de paquete DAC contiene las tablas de base de datos, procedimientos almacenados, vistas y usuarios, junto con los inicios de sesión que se asignan a los usuarios de la base de datos.  
  
 
## <a name="before-you-begin"></a>Antes de empezar  
 Puede extraer una DAC de las bases de datos que residen en instancias de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]o [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 4 o posterior. Si el proceso de extracción se ejecuta en una base de datos que se implementó a partir de una DAC, solo las definiciones de los objetos de la base de datos se extraen. El proceso no hace referencia a la DAC registrada en **msdb** (**maestra** en [!INCLUDE[ssSDS](../../includes/sssds-md.md)]). El proceso de extracción no registra la definición de DAC en la instancia actual del motor de base de datos. Para obtener más información sobre el registro de una DAC, vea [Register a Database As a DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Limitaciones y restricciones  
 Una DAC se puede extraer solo de una base de datos de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]o [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) o posterior. No puede extraer ninguna DAC si la base de datos tiene objetos que no se admiten en una DAC o usuarios contenidos. Para obtener más información acerca de los objetos admitidos por una DAC, vea [DAC Support For SQL Server Objects and Versions](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110)).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 La extracción de una DAC requiere al menos permisos ALTER ANY LOGIN y VIEW DEFINITION en el ámbito de la base de datos, así como permisos SELECT en **sys.sql_expression_dependencies**. La extracción de una DAC la pueden realizar los miembros del rol fijo de servidor securityadmin que sean también miembros del rol fijo de base de datos database_owner en la base de datos de la que se extrae la DAC. Los miembros del rol fijo de servidor sysadmin o de la cuenta de administrador del sistema de SQL Server integrada denominada **sa** también pueden extraer una DAC.  
  
##  <a name="using-the-extract-data-tier-application-wizard"></a><a name="UsingDACExtractWizard"></a> Usar el Asistente de Extraer aplicación de capa de datos  
 **Para extraer una DAC mediante un asistente**  
  
1.  En **Explorador de objetos**, expanda el nodo de la instancia que contiene la base de datos de la que debe extraerse la DAC.  
  
2.  Expanda el nodo **Bases de datos** .  
  
3.  Haga clic con el botón derecho en el nodo de la base de datos del que se va a extraer la DAC, haga clic en **Tareas** y, después, seleccione **Extraer aplicación de capa de datos...**.  
  
4.  Complete los cuadros de diálogo del asistente:  
  
    1.  [Página Introducción](#Introduction)  
  
    2.  [Seleccione Página de datos](#SelectData)  
  
    3.  [Página Definir propiedades](#SetProperties)  
  
    4.  [Página Validación y resumen](#ValidateSummary)  
  
    5.  [Página Compilar paquete](#BuildPackage)  
  
###  <a name="wizard-introduction-page"></a><a name="Introduction"></a> Página Introducción  
 Esta página describe los pasos para extraer una aplicación de capa de datos.  
  
 **No volver a mostrar esta página.** - Haga clic en la casilla para evitar que la página se muestre en el futuro.  
  
 **Siguiente >**: continúa hasta la página **Elegir método**.  
  
 **Cancelar:** termina el asistente sin extraer una aplicación de capa de datos de la base de datos.  
  
 [&#91;Asistente para extracción&#93;](#UsingDACExtractWizard)  
  
###  <a name="select-data-page"></a><a name="SelectData"></a> Select data page  
Seleccione los datos de referencia que quiera incluir en el archivo de paquete de la aplicación de capa de datos (DAC). La inclusión de datos en el paquete DAC es opcional. El paquete DAC ya incluirá el esquema de todos los objetos de base de datos compatibles y los objetos de instancia relacionados con la base de datos  
  
 Puede incluir hasta 10 MB de datos de referencia en el archivo del paquete DAC. Pero, en cuanto a las tablas que se van a incluir en el DAC, puede que no contengan tipos de datos de objetos binarios grandes (BLOB) como **image** o **varchar(max)**. Para extraer cantidades de datos más grandes con el fin de transferirlos a otra base de datos, use SQL Server Integration Services, la utilidad de copia masiva o una de las muchas otras técnicas de migración de datos.  
  
 **Tabla de base de datos:** active la casilla situada junto a las tablas de base de datos que contengan los datos que quiera incluir en el paquete DAC. Puede seleccionar hasta diez tablas con 10.000 filas o menos.  
  
 [&#91;Asistente para extracción&#93;](#UsingDACExtractWizard)  
  
###  <a name="set-properties-page"></a><a name="SetProperties"></a> Set properties page  
 Use esta página del asistente para describir la aplicación de capa de datos (DAC). Estas propiedades se usan para identificar la DAC y contribuyen a diferenciarla de otras.  
  
 **Nombre:** este nombre identifica la DAC. Puede ser distinto del nombre del archivo de paquete DAC y debe describir la aplicación. Por ejemplo, si la base de datos se usa para una aplicación de finanzas, puede llamar Finanza a la DAC.  
  
 **Versión (use xx.xx.xx.xx, donde x es un número):** valor numérico que identifica la versión de la DAC. La versión de DAC se usa en Visual Studio para identificar la versión de la DAC en la que están trabajando los desarrolladores. Al implementar una DAC, la versión se almacena en la base de datos **msdb** y se puede ver después en el nodo **Aplicaciones de capa de datos** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Descripción:** opcional. Describe la DAC. Al implementar una DAC, la descripción se almacena en la base de datos **msdb** y se puede ver después en el nodo **Aplicaciones de capa de datos** en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **Guardar en archivo de paquete DAC (incluye extensión .dacpac con nombre de archivo):** guarda la DAC en un archivo de paquete DAC, con una extensión .dacpac. Haga clic en el botón **Examinar** para especificar el nombre y la ubicación del archivo.  
  
 **Sobrescribir el archivo existente:** active esta casilla para reemplazar el archivo de paquete DAC si ya existe uno con el mismo nombre.  
  
###  <a name="validation-and-summary-page"></a><a name="ValidateSummary"></a> Validation and summary page  
 En esta página, el asistente valida que una aplicación de capa de datos (DAC) admite todos los objetos de base de datos. También comprueba las dependencias entre los objetos de base de datos para determinar el conjunto de objetos que se pueden incluir correctamente en la DAC. Tras ello, muestra el informe de validación y resume las opciones que ha seleccionado en este asistente. Para cambiar una opción, haga clic en **Anterior**. Para empezar a extraer una DAC, haga clic en **Siguiente**.  
  
> **NOTA:** Si una DAC no admite uno o varios objetos, el botón **Siguiente** está deshabilitado y el proceso de extracción no puede continuar. En estos casos, se recomienda quitar los objetos no admitidos y volver a ejecutar este asistente.  
  
 **Resumen:** un resumen de las opciones que ha seleccionado aparecen en **Propiedades de DAC**. Los resultados de la validación se enumeran en **Objetos de DAC**. La validación produce tres tipos de resultados:  
  
-   **Objetos admitidos en una DAC**: se admiten estos objetos y sus dependencias y se pueden incluir correctamente en la DAC.  
  
-   **Objetos admitidos en una DAC con advertencias**: se admiten estos objetos, pero dependen de otros objetos que no se admiten en una DAC.  
  
-   **Objetos no admitidos en una DAC**: estos objetos no se admiten y se deben quitar de la base de datos antes de extraer una DAC correctamente.  
  
 El proceso de validación comprueba varios niveles de dependencias. Por ejemplo, si un procedimiento almacenado depende de una tabla que usa un tipo de datos CLR no admitido, el procedimiento almacenado se enumerará en **Objetos incluidos en DAC con advertencias**.  
  
 Si una DAC no admite uno o varios objetos, el botón **Siguiente** está deshabilitado y el proceso de extracción no continuará. En estos casos, se recomienda quitar los objetos que no están admitidos y volver a ejecutar este asistente.  
  
 **Guardar informe:** permite guardar un archivo basado en HTML que enumera todos los objetos del nodo **Objetos DAC** en el resumen. Este informe puede ser útil cuando algunos de objetos de base de datos no se admiten en una DAC. Use el informe para cambiar o quitar objetos que no se admiten, antes de intentar extraer la DAC de nuevo.  
  
 ###  <a name="build-package-page"></a><a name="BuildPackage"></a> Build package page  
 Use esta página para supervisar el progreso del asistente cuando extrae la aplicación de capa de datos (DAC).  
  
 **Acción:** durante la acción **Crear y guardar archivo de paquete DAC** , el asistente extrae una DAC de la base de datos de SQL Server. A continuación, se crea un paquete DAC en memoria y se guarda en la ubicación especificada. Haga clic en los vínculos de la columna **Resultado** para ver el resultado del paso correspondiente.  
  
 **Guardar informe:** haga clic en esta opción para guardar los resultados del progreso del asistente en un archivo.  
  
 **Terminar** : haga clic en esta opción para cerrar el asistente después de que se haya completado el procesamiento o si se produce un error.  
   
##  <a name="extract-a-dac-using-powershell"></a><a name="ExtractDACPowerShell"></a> Extraer una DAC con PowerShell  
 **Para extraer una DAC de una base de datos con el método Extract() en un script de PowerShell**  
  
1.  Cree un objeto SMO Server y establézcalo en la instancia que contiene la base de datos desde la que se va a extraer una DAC.  
  
2.  Agregue una variable que especifique el nombre de la base de datos.  
  
3.  Especifique los metadatos de la DAC, como su nombre, versión y descripción.  
  
4.  Especifique la ruta de acceso y el nombre de archivo para el archivo de paquete DAC extraído.  
  
5.  Ejecutar el método Extract con la información especificada anteriormente.  
  
### <a name="example-powershell"></a>Ejemplo (PowerShell)  
 En el siguiente ejemplo se extrae una DAC denominada MyApplication de una base de datos llamada MyDB.  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Specify the database to extract to a DAC.  
$dbname = "MyDB"  
  
## Specify the DAC metadata.  
$applicationname = "MyApplication"  
$version = "1.0.0.0"  
$description = "This DAC defines the database used by my application."  
  
## Specify the location and name for the extracted DAC package.  
$dacpacPath = "C:\MyDACs\MyApplication.dacpac"  
  
## Extract the DAC.  
$extractionunit = New-Object Microsoft.SqlServer.Management.Dac.DacExtractionUnit($srv, $dbname, $applicationname, $version)  
$extractionunit.Description = $description  
$extractionunit.Extract($dacpacPath)  
```  
  
## <a name="see-also"></a>Vea también  
 - [Aplicaciones de capa de datos](../../relational-databases/data-tier-applications/data-tier-applications.md) 
 - [SqlPackage en canalizaciones de desarrollo](../../tools/sqlpackage/sqlpackage-pipelines.md)