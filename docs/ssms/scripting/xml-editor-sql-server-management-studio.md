---
title: Editores XML (SQL Server Management Studio)
description: Aprenda a usar las herramientas visuales del Editor XML de SQL Server Management Studio para trabajar con esquemas XML (XSD), conjuntos de datos ADO.NET y documentos XML.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editor.xml.f1
- sql13.swb.editorxml.f1
- vs.xmleditor
- sql13.swb.xmleditor.f1
helpviewer_keywords:
- XML Designer [SQL Server Management Studio]
ms.assetid: 0824a5ce-e67b-4b53-98d9-d371faf2d23c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24a4fa3e7957ca7e2d47188a7f3f982f3b8cff59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346097"
---
# <a name="xml-editor-sql-server-management-studio"></a>Editores XML (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Proporciona un conjunto de herramientas visuales para trabajar con esquemas XML, conjuntos de datos ADO.NET y documentos XML. El Diseñador XML admite el lenguaje de definición de esquemas XML (XSD) definido por el World Wide Web Consortium (WC3). El diseñador no admite definiciones de tipo de documento (DTD) ni otros lenguajes de esquemas XML, como el esquema XML simplificado (XDR).  

Para mostrar el diseñador, agregue un conjunto de datos, un esquema XML o un archivo XML al proyecto o abra cualquiera de los tipos de archivos que se enumeran en la siguiente tabla.  

> [!CAUTION]
> No hay ningún comando **Deshacer** cuando se trabaja en la vista Esquema. Planifique su trabajo meticulosamente y guarde los archivos con frecuencia.  
  
 El diseñador proporciona las tres siguientes vistas (o modos) para trabajar en archivos XML, esquemas XML y conjuntos de datos:  
  
|Ver|Descripción|Tipos de archivos compatibles|  
|----------|-----------------|--------------------------|  
|**Esquema**|Para crear y modificar visualmente esquemas XML y conjuntos de datos ADO.NET.|.xsd|  
|**Data**|Para modificar visualmente archivos de datos XML en una cuadrícula de datos estructurada.|.xml|  
|**XML**|Para editar XML; el editor de origen proporciona codificación de colores e IntelliSense, e incluye las características Palabra completa y Lista de miembros.|.xml .xsd .xslt .wsdl.web.resx.tdl.wsf.hta.disco.vsdisco.config|  
|**ShowPlan**|Muestra los planes de consulta xml creados mediante la opción SET SHOWPLAN_XML ON.|.showplan|  
  
## <a name="schema-view"></a>Vista Esquema  
 La vista Esquema proporciona una representación visual de los elementos, atributos, tipos, etc., que componen los esquemas XML y los conjuntos de datos ADO.NET.  
  
 En la vista Esquema se pueden crear esquemas y conjuntos de datos colocando elementos en la superficie de diseño desde la pestaña Esquema XML del Cuadro de herramientas o desde el Explorador de servidores. Además, se pueden agregar elementos al diseñador haciendo clic con el botón secundario en la superficie de diseño y seleccionando Agregar desde el menú contextual.  
  
 En la vista Esquema se pueden realizar las siguientes acciones:  
  
-   Crear o modificar esquemas XML y conjuntos de datos ADO.NET existentes  
  
-   Crear y editar relaciones entre tablas  
  
-   Crear y editar claves  
  
-   Generar conjuntos de datos ADO.NET a partir de esquemas XML  
  
> [!NOTE]  
>  El diseño de los elementos de la vista Esquema se almacena en el archivo .xsx; para poder ver este archivo, es necesario hacer clic en **Mostrar todos los archivos** en la barra de herramientas del Explorador de soluciones y, a continuación, expandir el archivo .xsd. Si no se muestra ningún archivo .xsx, significa que el archivo .xsd no se ha abierto nunca en el Diseñador XML.  
  
### <a name="customizing-schema-view"></a>Personalizar la vista Esquema  
 Las siguientes características modifican el aspecto visual de los elementos de la vista Esquema:  
  
-   Acercar o alejar  
  
-   Expandir o contraer elementos anidados  
  
-   Organizar automáticamente el diseño de los elementos  
  
-   Restablecer el estado predeterminado de los elementos contraídos  
  
##### <a name="to-expand-hidden-nested-elements"></a>Para expandir elementos anidados ocultos  
  
-   Haga clic en el icono de signo más de la parte inferior del elemento.  
  
##### <a name="to-collapse-nested-elements"></a>Para contraer elementos anidados  
  
-   Haga clic en el icono de signo menos del elemento inferior que desea que aparezca en el diseñador.  
  
## <a name="data-view"></a>Vista de datos  
 La vista de datos proporciona una cuadrícula de datos que puede utilizarse para modificar archivos .xml. En la vista de datos, solo se puede editar el contenido (pero no las etiquetas ni la estructura) de un archivo XML.  
  
 Hay dos áreas independientes en la vista de datos: **Tablas de datos** y **Datos**. El área **Tablas de datos** es una lista de relaciones definidas en el archivo XML, colocadas en el orden de su anidamiento (de exterior a interior). El área **Datos** es una cuadrícula de datos que muestra los datos en función de la selección realizada en el área Tablas de datos.  
  
> [!NOTE]  
>  Los archivos XML recién creados no contienen ningún dato y, por lo tanto, no pueden mostrarse en la vista de datos. Existen también algunas instancias de documentos XML donde no se puede invocar la vista de datos. Aunque se considere que el documento XML tiene un formato correcto, si no está formado por datos estructurados que están intentando cambiar a la vista de datos, se generará el siguiente mensaje: "Aunque este documento XML está bien formado, contiene una estructura que la Vista de datos no puede mostrar".  
  
 En la vista de datos se pueden realizar las siguientes acciones:  
  
-   Rellenar tablas de datos manualmente  
  
-   Editar tablas de datos existentes  
  
-   Generar un esquema XML a partir de un documento XML  
  
## <a name="xml-view"></a>Vista XML  
 La vista XML proporciona un editor para editar XML sin formato y proporciona también IntelliSense y codificación de colores. La finalización automática de instrucciones estará disponible cuando se trabaje con archivos .xsd y .xml que dispongan de un esquema asociado. Escriba < para iniciar una etiqueta y se le mostrará una lista de elementos válidos para dicha ubicación. Después de escribir el nombre del elemento y de presionar la BARRA ESPACIADORA, se le mostrará una lista de atributos compatibles con dicho elemento.  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] IntelliSense no están disponibles en la barra de herramientas. Para obtener acceso a las opciones desde el Editor XML, haga clic en **IntelliSense** , en el menú **Edición**.  
  
## <a name="showplan-view"></a>Vista SHOWPLAN  
 Los planes de consulta pueden guardarse en formato XML cuando se crean mediante la opción SET SHOWPLAN_XML ON. Haga doble clic en un archivo con la extensión .showplan para abrir el plan de consulta.  
  
## <a name="see-also"></a>Consulte también  
 [Guardar un plan de ejecución en formato XML](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
