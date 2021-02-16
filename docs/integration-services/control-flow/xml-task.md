---
description: Tarea XML
title: Tarea XML | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.xmltask.f1
- sql13.dts.designer.xmltask.general.f1
helpviewer_keywords:
- XML [Integration Services]
- XML task [Integration Services]
ms.assetid: 9f761846-390e-46d5-9db7-858943d40849
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f3b2b309d07e14347aa2d89038c897b3db804c9b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354398"
---
# <a name="xml-task"></a>Tarea XML

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea XML se usa para trabajar con datos XML. Mediante esta tarea, un paquete puede recuperar documentos XML, aplicar operaciones a los documentos mediante las hojas de estilos Extensible Stylesheet Language Transformations (XSLT) y expresiones XPath, combinar varios documentos, o bien validar, comparar y guardar los documentos actualizados en archivos y variables.  
  
 Esta tarea habilita un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para modificar de forma dinámica los documentos XML en tiempo de ejecución. Puede usar la tarea XML para los siguientes objetivos:  
  
-   Cambiar el formato de un documento XML. Por ejemplo, la tarea puede obtener acceso a un informe que reside en un archivo XML y aplicar dinámicamente una hoja de estilos XSLT para personalizar la presentación del documento.  
  
-   Seleccionar secciones de un documento XML. Por ejemplo, la tarea puede obtener acceso a un informe que reside en un archivo XML y aplicar dinámicamente una expresión XPath para seleccionar una sección del documento. La operación también puede obtener y procesar valores en el documento.  
  
-   Combinar documentos de varios orígenes. Por ejemplo, la tarea puede descargar informes de varios orígenes y combinarlos dinámicamente en un solo documento XML completo.  
  
-   Validar un documento XML y, de forma opcional, obtener una salida de error detallada. Para obtener más información, vea [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 Puede incluir datos XML en un flujo de datos mediante un origen XML para extraer valores de un documento XML. Para más información, consulte [XML Source](../../integration-services/data-flow/xml-source.md).  
  
## <a name="xml-operations"></a>Operaciones XML  
 La primera acción que realiza la tarea XML es recuperar un documento XML específico. Esta acción se genera en la tarea XML y se produce automáticamente. El documento XML recuperado se usa como origen de los datos para la operación que realiza la tarea XML.  
  
 Las operaciones de XML de comparación (Diff), combinación (Merge) y revisión (Patch) requieren dos operandos. El primer operando especifica el documento XML de origen. El segundo operando también especifica un documento XML, cuyo contenido depende de los requisitos de la operación. Por ejemplo, la operación de comparación compara dos documentos: por lo tanto, el segundo operando especifica otro documento XML similar con el que se compara el documento XML de origen.  
  
 La tarea XML puede usar una variable o un administrador de conexiones de archivo como origen, o incluir los datos XML en una propiedad de tarea.  
  
 Si el origen es una variable, la variable especificada contiene la ruta del documento XML.  
  
 Si el origen es un administrador de conexiones de archivos, el administrador de conexiones de archivos especificado proporciona la información de origen. El administrador de conexiones de archivos se configura de forma independiente de la tarea XML y se hace referencia a él en la tarea XML. La cadena de conexión para el administrador de conexiones de archivos especifica la ruta del archivo XML. Para obtener más información, consulte [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 La tarea XML se puede configurar para guardar el resultado de la operación en una variable o en un archivo. Si se guarda en un archivo, la tarea XML usa un administrador de conexiones de archivos para tener acceso al archivo. También puede guardar los resultados del DiffGram generado por la operación de comparación en archivos y variables.  
  
## <a name="predefined-xml-operations"></a>Operaciones XML predefinidas  
 La tarea XML incluye un conjunto predefinido de operaciones para trabajar con documentos XML. Estas operaciones se describen en la siguiente tabla.  
  
|Operación|Descripción|  
|---------------|-----------------|  
|Diferencias|Compara dos documentos XML. Usando el documento XML de origen como documento base, la operación de comparación lo compara con otro documento XML, detecta sus diferencias y escribe las diferencias en un documento DiffGram XML. Esta operación incluye propiedades para personalizar la comparación.|  
|Merge|Combina dos documentos XML. Usando el documento XML de origen como documento base, la operación de combinación agrega el contenido de un segundo documento en el documento base. La operación puede especificar una ubicación de combinación dentro del documento base.|  
|Revisión|Aplica el resultado de la operación de comparación, denominada documento DiffGram, en un documento XML para crear un nuevo documento principal que incluye contenido del documento DiffGram.|  
|Validación|Valida el documento XML según una definición de tipo de documento (DTD) o un esquema de definición de esquema XML (XSD).|  
|XPath|Realiza consultas y evaluaciones XPath.|  
|XSLT|Realiza transformaciones XSL en documentos XML.|  
  
### <a name="diff-operation"></a>Operación de comparación  
 La operación de comparación se puede configurar para usar un algoritmo de comparación diferente dependiendo de si la comparación debe ser rápida o precisa. La operación también se puede configurar para seleccionar automáticamente una comparación rápida o precisa según el tamaño de los documentos que se comparan.  
  
 La operación de comparación incluye un conjunto de opciones que personalizan la comparación XML. Las opciones se describen en la siguiente tabla.  
  
|Opción|Descripción|  
|------------|-----------------|  
|**IgnoreComments**|Valor que especifica si se comparan los nodos de comentarios.|  
|**IgnoreNameSpaces**|Valor que especifica si se comparan el identificador uniforme de recursos (URI) de espacio de nombres de un elemento y sus nombres de atributos. Si esta opción se establece en **true**, dos elementos que tienen el mismo nombre local pero un espacio de nombres diferente se consideran idénticos.|  
|**IgnorePrefixes**|Valor que especifica si se comparan los prefijos de nombres de elementos y atributos. Si esta opción se establece en **true,** , dos elementos que tienen el mismo nombre local pero un URI de espacio de nombres y prefijos diferentes se consideran idénticos.|  
|**IgnoreXMLDeclaration**|Un valor que especifica si se comparan las declaraciones XML.|  
|**IgnoreOrderOfChildElements**|Valor que especifica si se compara el orden de los elementos secundarios. Si esta opción se establece en **true**, los elementos secundarios que se diferencian solamente por su posición en una lista de elementos del mismo nivel se consideran idénticos.|  
|**IgnoreWhiteSpaces**|Valor que especifica si se comparan los espacios en blanco.|  
|**IgnoreProcessingInstructions**|Valor que especifica si se comparan las instrucciones de procesamiento.|  
|**IgnoreDTD**|Valor que especifica si se pasa por alto el DTD.|  
  
### <a name="merge-operation"></a>Operación de combinación  
 Al utilizar una instrucción XPath para identificar la ubicación de la combinación en el documento de origen, se espera que esta instrucción devuelva un único nodo. Si la instrucción devuelve varios nodos, solo se utiliza el primero. El contenido del segundo documento se combina bajo el primer nodo que devuelve la consulta de XPath.  
  
### <a name="xpath-operation"></a>Operación XPath  
 La operación XPath se puede configurar para usar diferentes tipos de funcionalidad de XPath.  
  
-   Seleccione la opción **Evaluación** para implementar funciones XPath tales como sum().  
  
-   Seleccione la opción **Lista de nodos** para devolver los nodos seleccionados como un fragmento XML.  
  
-   Seleccione la opción **Valores** para devolver el valor de texto interno de todos los nodos seleccionados, concatenados en una cadena.  
  
### <a name="validation-operation"></a>Operación de validación  
 La operación de validación se puede configurar para usar una Definición de tipo de documento (DTD) o un esquema de definición del esquema XML (XSD).  
  
 Habilite **ValidationDetails** para obtener una salida de error detallada. Para obtener más información, vea [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
## <a name="xml-document-encoding"></a>Codificación de documentos XML  
 La tarea XML únicamente admite la mezcla de documentos Unicode. Esto significa que la tarea puede aplicar la operación de combinación únicamente a documentos con una codificación Unicode. El uso de otras codificaciones hará que la tarea XML genere un error.  
  
> [!NOTE]  
>  Las operaciones de comparación y revisión incluyen una opción para omitir la declaración XML en los datos XML del segundo operando, lo que permite usar documentos que tienen otras codificaciones en estas operaciones.  
  
 Para comprobar que se puede usar el documento XML, revise la declaración XML. La declaración debe especificar explícitamente UTF-8, que indica una codificación Unicode de 8 bits.  
  
 La etiqueta siguiente muestra la codificación Unicode de 8 bits.  
  
 `<?xml version="1.0" encoding="UTF-8"?>`  
  
## <a name="custom-logging-messages-available-on-the-xml-task"></a>Mensajes de registro personalizados disponibles en la tarea XML  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea XML. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**XMLOperation**|Proporciona información sobre la operación que la tarea realiza.|  
  
## <a name="configuration-of-the-xml-task"></a>Configuración de la tarea XML  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener más información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en uno de los temas siguientes:  
  
-   [Validar XML con la tarea XML](../../integration-services/control-flow/validate-xml-with-the-xml-task.md)  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener más información sobre cómo establecer propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-xml-task"></a>Configuración mediante programación de la tarea XML  
 Para obtener más información sobre cómo establecer estas propiedades mediante programación, haga clic en el tema siguiente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.XMLTask.XMLTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="xml-task-editor-general-page"></a>Editor de la tarea XML (página General)
  Use el **Nodo general** del cuadro de diálogo **Editor de la tarea XML** para especificar el tipo de operación y configurar la operación.  
  
 Para obtener información acerca de esta tarea, vea [Validar XML con la tarea XML](../../integration-services/control-flow/validate-xml-with-the-xml-task.md). Para obtener información acerca de cómo trabajar con datos y documentos XML, vea el artículo sobre el[uso de XML en .NET Framework](/previous-versions/aa720019(v=vs.71))en MSDN Library.  
  
### <a name="static-options"></a>Opciones estáticas  
 **OperationType**  
 Seleccione un tipo de operación de la lista. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Validación**|Valida el documento XML según una definición de tipo de documento (DTD) o un esquema de definición de esquema XML (XSD). Seleccione esta opción para mostrar las opciones dinámicas de la sección **Validar**.|  
|**XSLT**|Realiza transformaciones XSL en documentos XML. Seleccione esta opción para mostrar las opciones dinámicas de la sección **XSLT**.|  
|**XPATH**|Realiza consultas y evaluaciones XPath. Seleccione esta opción para mostrar las opciones dinámicas de la sección **XPATH**.|  
|**Combinar**|Combina dos documentos XML. Seleccione esta opción para mostrar las opciones dinámicas de la sección **Mezclar**.|  
|**Diferencias**|Compara dos documentos XML. Seleccione esta opción para mostrar las opciones dinámicas de la sección **Comparación**.|  
|**Revisión**|Aplica la salida de la operación de comparación para crear un nuevo documento. Seleccione esta opción para mostrar las opciones dinámicas de la sección **Revisión**.|  
  
 **Tipo de origen**  
 Seleccione el tipo de origen del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **Origen**  
 Si **Source** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** para escribir el código XML mediante el cuadro de diálogo **Editor de origen del documento**.  
  
 Si **Source** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **Source** está establecido en **Variable**, seleccione una variable existente o haga clic en **\<New variable...>** para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
### <a name="operationtype-dynamic-options"></a>Opciones dinámicas de OperationType  
  
#### <a name="operationtype--validate"></a>OperationType = Validar  
 Especifique las opciones de la operación Validar.  
  
 **SaveOperationResult**  
 Especifique si desea que la tarea XML guarde el resultado de la operación Validar.  
  
 **OverwriteDestination**  
 Especifique si desea sobrescribir el archivo o variable de destino.  
  
 **Destino**  
 Seleccione un administrador de conexiones de archivos existente o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 **DestinationType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **ValidationType**  
 Seleccione el tipo de validación. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**DTD**|Use una definición de tipo de documento (DTD).|  
|**XSD**|Use un esquema de definición de esquema XML (XSD). Seleccione esta opción para mostrar las opciones dinámicas de la sección **ValidationType**.|  
  
 **FailOnValidationFail**  
 Especifique si desea que se produzca un error en la operación si se produce un error al validar el documento.  
  
 **ValidationDetails**  
 Proporciona una salida de error enriquecida cuando el valor de esta propiedad es true. Para obtener más información, vea [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
### <a name="validationtype-dynamic-options"></a>Opciones dinámicas de ValidationType  
  
#### <a name="validationtype--xsd"></a>ValidationType = XSD  
 **SecondOperandType**  
 Seleccione el tipo de origen del segundo documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen**.  
  
 Si **SecondOperandType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xslt"></a>OperationType = XSLT  
 Especifique las opciones de la operación XSLT.  
  
 **SaveOperationResult**  
 Especifique si desea que la tarea XML guarde el resultado de la operación XSLT.  
  
 **OverwriteDestination**  
 Especifique si desea sobrescribir el archivo o variable de destino.  
  
 **Destino**  
 Si **DestinationType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperandType**  
 Seleccione el tipo de origen del segundo documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen**.  
  
 Si **SecondOperandType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xpath"></a>OperationType = XPATH  
 Especifique las opciones de la operación XPath.  
  
 **SaveOperationResult**  
 Especifique si desea que la tarea XML guarde el resultado de la operación XPath.  
  
 **OverwriteDestination**  
 Especifique si desea sobrescribir el archivo o variable de destino.  
  
 **Destino**  
 Si **DestinationType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperandType**  
 Seleccione el tipo de origen del segundo documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen**.  
  
 Si **SecondOperandType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
 **PutResultInOneNode**  
 Especifique si desea escribir el resultado en un único nodo.  
  
 **XPathOperation**  
 Seleccione el tipo de resultado XPath. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Evaluación**|Devuelve los resultados de una función XPath.|  
|**Lista de nodos**|Devuelve los nodos seleccionados como un fragmento de XML.|  
|**Valores**|Devuelve el valor de texto interno de todos los nodos seleccionados concatenado en una cadena.|  
  
#### <a name="operationtype--merge"></a>OperationType = Mezclar  
 Especifique las opciones de la operación Mezclar.  
  
 **XPathStringSourceType**  
 Seleccione el tipo de origen del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **XPathStringSource**  
 Si **XPathStringSourceType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen del documento**.  
  
 Si **XPathStringSourceType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
 Al utilizar una instrucción XPath para identificar la ubicación de la combinación en el documento de origen, se espera que esta instrucción devuelva un único nodo. Si la instrucción devuelve varios nodos, solo se utiliza el primero. El contenido del segundo documento se combina bajo el primer nodo que devuelve la consulta de XPath.  
  
 **SaveOperationResult**  
 Especifique si desea que la tarea XML guarde el resultado de la operación Mezclar.  
  
 **OverwriteDestination**  
 Especifique si desea sobrescribir el archivo o variable de destino.  
  
 **Destino**  
 Si **DestinationType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperandType**  
 Seleccione el tipo de destino del segundo documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen del documento**.  
  
 Si **SecondOperandType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **SecondOperandType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--diff"></a>OperationType = Comparación  
 Especifique las opciones de la operación de comparación.  
  
 **DiffAlgorithm**  
 Seleccione el algoritmo de comparación que desea utilizar para comparar los documentos. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Automático**|Permita que la tarea XML determine si es necesario utilizar el algoritmo rápido o preciso.|  
|**Rápido**|Utilice un algoritmo de comparación rápido, aunque menos preciso.|  
|**Preciso**|Utilice un algoritmo de comparación preciso.|  
  
 **Opciones de comparación**  
 Establezca las opciones de comparación de la operación de comparación. Las opciones se muestran en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**IgnoreXMLDeclaration**|Especifique si desea comparar la declaración XML.|  
|**IgnoreDTD**|Especifique si desea ignorar la definición de tipo de documento (DTD).|  
|**IgnoreWhiteSpaces**|Especifique si se omitirán las diferencias en la cantidad de espacios en blanco al comparar documentos.|  
|**IgnoreNameSpaces**|Especifique si desea comparar el identificador uniforme de recursos (URI) de espacio de nombres de un elemento y los nombres de atributo.<br /><br /> Nota: Si esta opción se establece en **True**, se considerarán idénticos dos elementos con el mismo nombre local, pero con distintos espacios de nombres.|  
|**IgnoreProcessingInstructions**|Especifique si desea comparar las instrucciones de procesamiento.|  
|**IgnoreOrderOf ChildElements**|Especifique si desea comparar el orden de los elementos secundarios.<br /><br /> Nota: Si esta opción se establece en **True**, los elementos secundarios que se diferencian solamente por su posición en una lista de elementos del mismo nivel se consideran idénticos.|  
|**IgnoreComments**|Especifique si desea comparar los nodos de comentario.|  
|**IgnorePrefixes**|Especifique si desea comparar los prefijos de los nombres de elemento y atributo.<br /><br /> Nota: Si esta opción se establece en **True**, dos elementos con el mismo nombre local, pero con prefijos y URI de espacios de nombres diferentes se consideran idénticos.|  
  
 **FailOnDifference**  
 Especifique si desea que se produzca un error en la tarea si se produce un error en la operación de comparación.  
  
 **SaveDiffGram**  
 Especifique si desea guardar el resultado de comparación en un documento DiffGram.  
  
 **SaveOperationResult**  
 Especifique si desea que la tarea XML guarde el resultado de la operación de comparación.  
  
 **OverwriteDestination**  
 Especifique si desea sobrescribir el archivo o variable de destino.  
  
 **Destino**  
 Si **DestinationType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperandType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen del documento**.  
  
 Si **SecondOperandType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **SecondOperandType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--patch"></a>OperationType = Revisión  
 Especifique las opciones de la operación Revisión.  
  
 **SaveOperationResult**  
 Especifique si desea que la tarea XML guarde el resultado de la operación Revisión.  
  
 **OverwriteDestination**  
 Especifique si desea sobrescribir el archivo o variable de destino.  
  
 **Destino**  
 Si **DestinationType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperandType**  
 Seleccione el tipo de destino del documento XML. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para un documento XML.|  
|**Conexión de archivos**|Seleccione el archivo que contiene el documento XML.|  
|**Variable**|Establezca el origen para la variable que contiene el documento XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** se establece en **Entrada directa**, escriba el código XML o haga clic en el botón de puntos suspensivos **(…)** y escriba el código XML mediante el cuadro de diálogo **Editor de origen del documento**.  
  
 Si **SecondOperandType** se ha establecido en **Conexión de archivos**, seleccione un administrador de conexiones de archivos o haga clic en \<**New connection...**> para crear uno nuevo.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
 Si **SecondOperandType** está establecido en **Variable**, seleccione una variable existente o haga clic en \<**New variable...**> para crear una nueva.  
  
 **Temas relacionados**: [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
## <a name="related-content"></a>Contenido relacionado  

-   Ejemplo en CodePlex, [ejemplo de proceso de paquete de datos XML](https://msftisprodsamples.codeplex.com/wikipage?title=SS2008!Process%20XML%20Data%20Package%20Sample&version=10&ProjectName=msftisprodsamples), en www.codeplex.com  
  
