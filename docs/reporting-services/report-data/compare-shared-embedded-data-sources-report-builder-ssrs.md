---
description: 'Comparación de orígenes de datos compartidos e incrustados: generador de informes y Reporting Services (SSRS)'
title: 'Comparación de orígenes de datos compartidos e incrustados: Generador de informes y Reporting Services | Microsoft Docs'
ms.date: 11/18/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5e2c72ad95392be7571c05e1dddcee1ed35c3d6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042225"
---
# <a name="compare-shared-and-embedded-data-sources---report-builder--reporting-services-ssrs"></a>Comparación de orígenes de datos compartidos e incrustados: generador de informes y Reporting Services (SSRS)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]
 
Puede conectarse a los datos con un origen de datos compartido o uno incrustado. Un *origen de datos compartido* se define independientemente de cualquier informe. Puede utilizarlo en varios informes en un servidor de informes o en un sitio de SharePoint. Un *origen de datos incrustado* se define en un informe. Solo se puede usar en ese informe. 

 Los orígenes de datos compartidos resultan útiles cuando se poseen orígenes de datos de uso frecuente. Se recomienda que cree y utilice los orígenes de datos compartidos tanto como sea posible. Facilitan la administración de los informes y del acceso a ellos, y ayudan a mantener una mayor seguridad en el acceso a los informes y los orígenes de datos. Si necesita un origen de datos compartido, es posible que tenga que solicitar a su administrador del sistema que le cree uno.  
  
 Un origen de datos incrustado, también conocido como *origen de datos específico del informe*, es una conexión de datos que se guarda en la definición de informe. La información de conexión a orígenes de datos insertados solo puede utilizarla el informe en el que se incrusta la información. Para definir y administrar los orígenes de datos insertados, utilice el cuadro de diálogo **Propiedades del origen de datos** .  
  
 La diferencia entre los orígenes de datos incrustados y compartidos es la manera en que se crean, almacenan y administran.  
  
-   En el Diseñador de informes, cree orígenes de datos incrustados o compartidos como parte de un proyecto de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Puede controlar si va a utilizarlos localmente para la obtención de una vista previa o implementarlos como parte del proyecto en un servidor de informes o un sitio de SharePoint. Puede utilizar las extensiones de datos personalizadas que se han instalado en el equipo y en el servidor de informes o un sitio de SharePoint donde se implementan los informes.  
  
     Los administradores del sistema pueden instalar y configurar extensiones de procesamiento de datos y proveedores de datos de .NET Framework adicionales. Para obtener más información, vea [Extensiones de procesamiento de datos y proveedores de datos de .NET Framework &#40;SSRS&#41;](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md).  
  
     Los desarrolladores pueden usar la API de <xref:Microsoft.ReportingServices.DataProcessing> para crear extensiones de procesamiento de datos compatibles con tipos de orígenes de datos adicionales.  
  
-   En [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], desplácese a un servidor de informes o un sitio de SharePoint y seleccione orígenes de datos compartidos o cree orígenes de datos incrustados en el informe. No se puede crear un origen de datos compartido en [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. No se pueden usar extensiones de datos personalizadas en [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  

## <a name="summary-of-differences"></a>Resumen de diferencias
  
 En la tabla siguiente se resumen las diferencias entre los orígenes de datos compartidos y los incrustados.  
  
|Descripción|Insertado<br /><br /> Origen de datos|Compartido<br /><br /> Origen de datos|  
|-----------------|------------------------------|----------------------------|  
|La conexión de datos se incrusta en la definición de informe.|![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")||  
|El puntero a la conexión de datos en el servidor de informes se incrusta en la definición de informe.||![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  
|Se administra en el servidor de informes|![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  
|Se requiere para los conjuntos de datos compartidos||![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  
|Se requiere para los componentes||![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  

## <a name="next-steps"></a>Pasos siguientes

[Crear y administrar orígenes de datos compartidos](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)   
[Creación y modificación de orígenes de datos incrustados](../../reporting-services/report-data/create-and-modify-embedded-data-sources.md)   
[Establecimiento de propiedades de implementación](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
[Especificación de información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
