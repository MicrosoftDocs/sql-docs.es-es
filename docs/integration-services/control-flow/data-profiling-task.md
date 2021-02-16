---
description: Tarea de generación de perfiles de datos
title: Tarea de generación de perfiles de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.f1
helpviewer_keywords:
- Data Profiling task [Integration Services], about Data Profiling task
- data profiling
- profiling data
ms.assetid: 248ce233-4342-42c5-bf26-f4387ea152cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 99c1b0dbc98cd0c99b335c753a1b4b086b209921
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352092"
---
# <a name="data-profiling-task"></a>Tarea de generación de perfiles de datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea de generación de perfiles de datos calcula diversos perfiles que le ayudan a familiarizarse con un origen de datos y a identificar en los datos problemas que deban corregirse.  
  
 Puede utilizar la tarea de generación de perfiles de datos dentro de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para generar perfiles de datos que están almacenados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e identificar posibles problemas de calidad de los datos.  
  
> [!NOTE]  
>  En este tema únicamente se describen las características y requisitos de la tarea de generación de perfiles de datos. Para obtener un tutorial sobre cómo usar la tarea de generación de perfiles de datos, vea la sección [Visor y tarea de generación de perfiles de datos](../../integration-services/control-flow/data-profiling-task-and-viewer.md).  
  
## <a name="requirements-and-limitations"></a>Requisitos y limitaciones  
 La tarea de generación de perfiles de datos solo funciona con datos que estén almacenados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta tarea no funciona con orígenes de datos de otros fabricantes o basados en archivos.  
  
 Además, para ejecutar un paquete que contiene la tarea de generación de perfiles de datos, debe utilizar una cuenta que tenga permisos de lectura/escritura, incluidos los permisos CREATE TABLE, en la base de datos tempdb.  
  
## <a name="data-profiler-viewer"></a>Visor del generador de perfiles de datos  
 Después de utilizar la tarea para calcular los perfiles de datos y guardarlos en un archivo, puede utilizar el Visor de perfil de datos independiente para revisar el perfil generado. El Visor de perfil de datos también admite la obtención de detalles para ayudarle a entender los problemas de calidad de los datos que se identifican en el perfil generado. Para más información, vea [Visor de perfil de datos](../../integration-services/control-flow/data-profile-viewer.md).  
  
> [!IMPORTANT]  
>  El archivo de salida podría contener datos confidenciales acerca de la base de datos y de los datos que contiene. Para conocer sugerencias sobre cómo hacer que este archivo sea más seguro, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
>   
>  La capacidad de detalle, que está disponible en el Visor de perfil de datos, envía las consultas actuales al origen de datos original.  
  
## <a name="available-profiles"></a>Perfiles disponibles  
 La tarea de generación de perfiles de datos puede calcular ocho perfiles de datos diferentes. Cinco de estos perfiles analizan columnas individuales y los otros tres analizan varias columnas o relaciones entre las columnas y las tablas.  
  
 Los cinco perfiles siguientes analizan columnas individuales.  
  
|Perfiles que analizan columnas individuales|Descripción|  
|----------------------------------------------|-----------------|  
|Perfil de distribución de longitud de columnas|Notifica las diferentes longitudes de valores de cadena existentes en la columna seleccionada y el porcentaje de filas de la tabla que representa cada longitud.<br /><br /> Este perfil le ayuda a identificar problemas en los datos, como los valores no válidos. Por ejemplo, genera un perfil de una columna de códigos de estados de Estados Unidos que deberían ser de dos caracteres y detecta valores con más de dos caracteres.|  
|Perfil de proporción de columnas nulas|Notifica el porcentaje de valores nulos en la columna seleccionada.<br /><br /> Este perfil permite identificar problemas con los datos, como una proporción inesperadamente alta de valores nulos en una columna. Por ejemplo, genera un perfil de una columna de códigos postales y detecta un porcentaje inaceptablemente alto de códigos que faltan.|  
|Perfil de patrón de columnas|Notifica un conjunto de expresiones regulares que cubren el porcentaje de valores especificado en una columna de cadenas.<br /><br /> Este perfil le ayuda a identificar problemas con los datos, como las cadenas no válidas. Este perfil también puede sugerir expresiones regulares que se pueden usar en el futuro para validar los valores nuevos. Por ejemplo, un perfil del patrón de una columna de códigos postales de Estados Unidos podría generar las expresiones regulares: \d{5}-\d{4}, \d{5} y \d{9}. Si ve otras expresiones regulares, es posible que los datos contengan valores no válidos o tengan un formato incorrecto.|  
|Perfil de estadísticas de columnas|Notifica estadísticas, como los valores mínimo, máximo, medio y la desviación estándar para las columnas numéricas, y los valores mínimo y máximo para las columnas **datetime** .<br /><br /> Este perfil le ayuda a identificar problemas existentes en los datos, como las fechas no válidas. Por ejemplo, genera un perfil de una columna de fechas históricas y detecta una fecha máxima futura.|  
|Perfil de distribución de valores de columna|Notifica todos los valores distintos existentes en la columna seleccionada y el porcentaje de filas de la tabla que representa cada valor. También puede notificar los valores existentes en un número de filas de la tabla que supera cierto porcentaje.<br /><br /> Este perfil le ayuda a identificar problemas con los datos, como un número incorrecto de valores distintos en una columna. Por ejemplo, si al generar un perfil de una columna que se supone que contiene los estados de Estados Unidos detecta más de 50 valores distintos.|  
  
 Los tres perfiles siguientes analizan varias columnas o relaciones entre columnas y tablas.  
  
|Perfiles que analizan varias columnas|Descripción|  
|--------------------------------------------|-----------------|  
|Perfil de claves candidatas|Notifica si una columna o un conjunto de columnas es una clave, o una clave aproximada, para la tabla seleccionada.<br /><br /> Este perfil le ayuda a identificar problemas con los datos, como por ejemplo, valores duplicados en una posible columna de clave.|  
|Perfil de dependencia funcional|Notifica hasta qué punto los valores de una columna (la columna dependiente) dependen de los valores de otra columna o de un conjunto de columnas (la columna determinante).<br /><br /> Este perfil le ayuda a identificar problemas con los datos, como valores no válidos. Por ejemplo, al generar un perfil de la dependencia entre una columna que contiene códigos postales de Estados Unidos y una columna que contiene estados de Estados Unidos. El mismo código postal debería tener siempre el mismo estado, pero el perfil detecta infracciones de esta dependencia.|  
|Perfil de inclusión de valores|Calcula la superposición existente entre los valores de dos columnas o conjuntos de columnas. Este perfil puede determinar si una columna o un conjunto de columnas resulta adecuado para actuar como una clave externa entre las tablas seleccionadas.<br /><br /> Este perfil le ayuda a identificar problemas con los datos, como valores no válidos. Por ejemplo, puede generar el perfil de una columna de identificadores de producto de una tabla de ventas y detectar que dicha columna contiene valores que no se encuentran en la columna de identificadores de producto de la tabla de productos.|  
  
## <a name="prerequisites-for-a-valid-profile"></a>Requisitos previos para un perfil válido  
 Un perfil no es válido si no selecciona tablas y columnas que no están vacías, y las columnas contienen tipos de datos que son válidos para el perfil.  
  
### <a name="valid-data-types"></a>Tipos de datos válidos  
 Algunos de los perfiles disponibles solo tienen sentido para ciertos tipos de datos. Por ejemplo, no tiene sentido calcular un perfil de patrón de columnas para una columna que contiene valores numéricos o **datetime** . Por consiguiente, este tipo de perfil no es válido.  
  
|Perfil|Tipos de datos válidos*|  
|-------------|------------------------|  
|ColumnStatisticsProfile|Columnas de tipo numérico o tipo **datetime** (ni **mean** ni **stddev** para la columna **datetime** )|  
|ColumnNullRatioProfile|Todas las columnas**|  
|ColumnValueDistributionProfile|Columnas de tipo **integer** , **char** y **datetime**|  
|ColumnLengthDistributionProfile|Columnas de tipo **char**|  
|ColumnPatternProfile|Columnas de tipo **char**|  
|CandidateKeyProfile|Columnas de tipo **integer** , **char** y **datetime**|  
|FunctionalDependencyProfile|Columnas de tipo **integer** , **char** y **datetime**|  
|InclusionProfile|Columnas de tipo **integer** , **char** y **datetime**|  
  
 \* En la tabla anterior de tipos de datos válidos, los tipos **integer**, **char**, **datetime** y **numeric** incluyen los tipos de datos específicos siguientes:  
  
 Los tipos enteros incluyen **bit**, **tinyint**, **smallint**, **int** y **bigint**.  
  
 Los tipos de caracteres incluyen **char**, **nchar**, **varchar** y **nvarchar** , pero no incluyen **varchar (max)** ni **nvarchar (max)**.  
  
 Los tipos de fecha y hora incluyen **datetime**, **smalldatetime** y **timestamp**.  
  
 Los tipos numéricos incluyen los tipos **integer** (excepto **bit**), **money**, **smallmoney**, **decimal**, **float**, **real** y **numeric**.  
  
 \*\* **image**, **text**, **XML**, **udt** y **variant** solo se admiten para el perfil de proporción de columnas nulas.  
  
### <a name="valid-tables-and-columns"></a>Tablas y columnas válidas  
 Si la tabla o la columna está vacía, la tarea de generación de perfiles de datos realiza las acciones siguientes:  
  
-   Cuando la tabla o la vista seleccionadas estén vacías, la tarea de generación de perfiles de datos no calculará ningún perfil.  
  
-   Cuando todos los valores de la columna seleccionada sean NULL, la tarea de generación de perfiles de datos solo calculará el perfil de proporción de columnas nulas. La tarea no calculará el perfil de distribución de longitud de columnas, el perfil de patrón de columnas, el perfil de estadísticas de columnas ni el perfil de distribución de valores de columna.  
  
## <a name="features-of-the-data-profiling-task"></a>Características de la tarea de generación de perfiles de datos  
 La tarea de generación de perfiles de datos tiene estas prácticas opciones de configuración:  
  
-   **Columnas comodín**: mientras se configura una solicitud de perfil, la tarea acepta el comodín **(\*)** en lugar de un nombre de columna. Esto simplifica la configuración y permite descubrir con facilidad las características de los datos poco familiares. Cuando se ejecuta la tarea, ésta genera perfiles para cada columna con un tipo de datos adecuado.  
  
-   **Perfil rápido** You can select Perfil rápido to configure the task quickly. Un perfil rápido genera perfiles para una tabla o una vista mediante todos los perfiles y valores de configuración predeterminados.  
  
## <a name="custom-logging-messages-available-on-the-data-profililng-task"></a>Mensajes de registro personalizados disponibles en la tarea de generación de perfiles de datos  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea de generación de perfiles de datos. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**DataProfilingTaskTrace**|Proporciona información descriptiva sobre el estado de la tarea. Los mensajes incluyen la información siguiente:<br /><br /> Inicio de las solicitudes de procesamiento<br /><br /> Inicio de la consulta<br /><br /> Query End<br /><br /> Finalización de la solicitud de cálculo|  
  
## <a name="output-and-its-schema"></a>Salida y su esquema  
 La tarea de generación de perfiles de datos genera los perfiles seleccionados en XML y se estructura según el esquema DataProfile.xsd. Puede especificar si este XML generado se guarda en un archivo o en una variable de paquete. Puede ver este esquema en línea en [https://schemas.microsoft.com/sqlserver/2008/DataDebugger/](https://schemas.microsoft.com/sqlserver/2008/DataDebugger/). Desde la página web puede guardar una copia local del esquema. A continuación, puede ver la copia local del esquema en Microsoft [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] u otro editor de esquemas, en un editor XML o en un editor de texto, como el Bloc de notas.  
  
 Este esquema de información sobre la calidad de los datos podría ser útil para:  
  
-   Intercambiar información sobre la calidad de los datos dentro de las organizaciones y entre ellas.  
  
-   Generar herramientas personalizadas para trabajar con información sobre la calidad de los datos.  
  
 El espacio de nombres de destino se identifica en el esquema como [https://schemas.microsoft.com/sqlserver/2008/DataDebugger/](https://schemas.microsoft.com/sqlserver/2008/DataDebugger/).  
  
## <a name="output-in-the-conditional-workflow-of-a-package"></a>Salida en el flujo de trabajo condicional de un paquete  
 Los componentes que generan perfiles de datos no incluyen funcionalidad integrada para implementar la lógica condicional en el flujo de trabajo del paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] basándose en el resultado de la tarea de generación de perfiles de datos. Sin embargo, puede agregar fácilmente esta lógica en una tarea Script con una cantidad de programación mínima. Este código realizaría una consulta XPath en el XML generado y, a continuación, guardaría el resultado en una variable de paquete. Las restricciones de precedencia que conectan la tarea Script con las tareas subsiguientes pueden utilizar una expresión para determinar el flujo de trabajo. Por ejemplo, la tarea Script detecta que el porcentaje de valores NULL de una columna supera un cierto umbral. Cuando esta condición sea True, quizá desee interrumpir el paquete y resolver el problema antes de continuar.  
  
## <a name="configuration-of-the-data-profiling-task"></a>Configuración de la tarea de generación de perfiles de datos  
 La tarea de generación de perfiles de datos se configura mediante el **Editor de tareas de generación de perfiles de datos**. El editor tiene dos páginas:  
  
 [Página General](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)  
 En la página **General** , se especifica el archivo o la variable de resultados. También es posible seleccionar **Perfil rápido** para configurar rápidamente la tarea con objeto de calcular los perfiles utilizando la configuración predeterminada. Para más información, vea [Formulario de perfil rápido de tabla única &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md).  
  
 [Página Solicitudes de perfil](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)  
 En la página **Solicitudes de perfil** , especifique el origen de datos y seleccione y configure los perfiles de los datos que quiere calcular. Para obtener más información sobre los diversos perfiles que puede configurar, vea los temas siguientes:  
  
-   [Opciones de Solicitud de perfil de claves candidatas &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/candidate-key-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de distribución de longitud de columna &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/column-length-distribution-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de proporción de columnas nulas &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/column-null-ratio-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de patrón de columnas &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/column-pattern-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de estadísticas de columnas &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/column-statistics-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de distribución de valores de columna &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/column-value-distribution-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de dependencia funcional &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/functional-dependency-profile-request-options-data-profiling-task.md)  
  
-   [Opciones de Solicitud de perfil de inclusión de valores &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/value-inclusion-profile-request-options-data-profiling-task.md)  
  
  
