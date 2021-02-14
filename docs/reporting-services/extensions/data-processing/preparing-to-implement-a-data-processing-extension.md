---
title: Preparación de la implementación de una extensión de procesamiento de datos | Microsoft Docs
description: Descubra cómo implementar una extensión de procesamiento de datos en Reporting Services. Obtenga información sobre las interfaces disponibles y la funcionalidad necesaria y opcional.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- interfaces [Reporting Services]
- data processing extensions [Reporting Services], implementing
ms.assetid: 698817e4-33da-4eb5-9407-4103e1c35247
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f057daa09caf7ba3d690fb0dc0586a5e28afc81e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065422"
---
# <a name="preparing-to-implement-a-data-processing-extension"></a>Prepararse para implementar una extensión de procesamiento de datos
  Antes de implementar la extensión de procesamiento de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], debería definir las interfaces que se van a implementar. Puede que quiera proporcionar implementaciones específicas de la extensión del conjunto completo de interfaces o simplemente centrar la implementación en un subconjunto, como las interfaces <xref:Microsoft.ReportingServices.DataProcessing.IDataReader> y <xref:Microsoft.ReportingServices.DataProcessing.IDbCommand> en las que los clientes interactuarían principalmente con un conjunto de resultados como un objeto **DataReader** y usarían la extensión de procesamiento de datos de [!INCLUDE[ssRS](../../../includes/ssrs.md)] como un puente entre el conjunto de resultados y el origen de datos.  
  
 Puede implementar las extensiones de procesamiento de datos de una de dos maneras:  
  
-   Las clases de extensión de procesamiento de datos pueden implementar las interfaces del proveedor de datos de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] y, opcionalmente, las interfaces de extensión de procesamiento de datos extendidas que proporciona [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
-   Las clases de extensión de procesamiento de datos pueden implementar las interfaces de extensión de procesamiento de datos que proporciona [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y, opcionalmente, las interfaces de extensión de procesamiento de datos extendidas.  
  
 Si la extensión de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] no va a admitir ninguna propiedad ni método concretos, implemente la propiedad o método como una no-operación. Si un cliente espera un comportamiento determinado, inicie una excepción **NotSupportedException**.  
  
> [!NOTE]  
>  Una implementación no-operación de una propiedad o método solo se aplica a las propiedades y métodos de las interfaces que decida implementar. Las interfaces opcionales que decida no implementar se deberían omitir del ensamblado de extensión de procesamiento de datos. Para obtener más información sobre si se requiere una interfaz o es opcional, vea la tabla posteriormente en esta sección.  
  
## <a name="required-extension-functionality"></a>Funcionalidad de extensión necesaria  
 Cada extensión de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] debe proporcionar la funcionalidad siguiente:  
  
-   Abrir una conexión con un origen de datos.  
  
-   Analizar una consulta y devolver una lista de nombres del conjunto de resultados.  
  
-   Ejecutar una consulta en el origen de datos y devolver un conjunto de filas.  
  
-   Pasar los parámetros de un solo valor a la consulta.  
  
-   Recorrer las filas del conjunto de filas y recuperar los datos.  
  
 Cada extensión de procesamiento de datos se puede extender para incluir la funcionalidad siguiente:  
  
-   Analizar una consulta y devolver una lista de los nombres de parámetro utilizados en la consulta.  
  
-   Analizar una consulta y devolver la lista de campos por los que la consulta se agrupa.  
  
-   Analizar una consulta y devolver la lista de campos por los que la consulta se ordena.  
  
-   Proporcionar un nombre de usuario y una contraseña para conectarse al origen de datos que es independiente de la cadena de conexión.  
  
-   Recorrer en iteración las filas del conjunto de filas y recuperar los metadatos auxiliares sobre los valores de datos.  
  
-   Agregar datos al servidor.  
  
## <a name="available-extension-interfaces"></a>Interfaces de extensión disponibles  
 En la tabla siguiente se describen las interfaces disponibles y si se requiere la implementación o es opcional.  
  
|Interfaz|Descripción|Implementación|  
|---------------|-----------------|--------------------|  
|IDbConnection|Representa una sesión única con un origen de datos. En el caso de un sistema de bases de datos cliente/servidor, la sesión puede ser equivalente a una conexión de red al servidor.|Obligatorio|  
|IDbConnectionExtension|Representa propiedades de conexión adicionales que pueden ser implementadas por extensiones de procesamiento de datos de [!INCLUDE[ssRS](../../../includes/ssrs.md)] con respecto a la seguridad y autenticación.|Opcional|  
|IDbTransaction|Representa una transacción local.|Obligatorio|  
|IDbTransactionExtension|Representa las propiedades de transacción adicionales que pueden ser implementadas por extensiones de procesamiento de datos de [!INCLUDE[ssRS](../../../includes/ssrs.md)].|Opcional|  
|IDbCommand|Representa una consulta o comando que se utiliza al conectarse a un origen de datos.|Obligatorio|  
|IDbCommandAnalysis|Representa información del comando adicional para analizar una consulta y devolver una lista de los nombres de parámetros que se usan en la consulta.|Opcional|  
|IDataParameter|Representa un parámetro o un par de nombre y valor que se pasa a un comando o consulta.|Obligatorio|  
|IDataParameterCollection|Representa una colección de todos los parámetros pertinentes para un comando o consulta.|Obligatorio|  
|IDataReader|Proporciona un método para leer un flujo de solo lectura y solo avance de datos del origen de datos.|Obligatorio|  
|IDataReaderExtension|Proporciona un método para leer uno o varios flujos de solo avance de conjuntos de resultados, obtenidos al ejecutar un comando en un origen de datos. Esta interfaz proporciona compatibilidad adicional para los agregados de campo.|Opcional|  
|IExtension|Proporciona la clase base para la extensión de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. También permite a un implementador incluir un nombre traducido para la extensión y pasar la configuración del archivo de configuración a la extensión.|Obligatorio|  
  
 Las interfaces de extensión de procesamiento de datos son idénticas a un subconjunto de las interfaces, métodos y propiedades del proveedor de datos de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], siempre que sea posible. Para obtener más información sobre cómo implementar un proveedor de datos de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] completo, vea el tema sobre la implementación de un proveedor de datos de .NET Framework en la documentación del Kit de desarrollo de software (SDK) de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementar una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
