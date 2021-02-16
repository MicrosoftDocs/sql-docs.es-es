---
title: Usar el control RSClientPrint en aplicaciones personalizadas | Microsoft Docs
description: RSPrintClient proporciona impresión del lado cliente para los informes mostrados en el Visor HTML. El servidor de informes representa el informe en la extensión de representación Image (EMF).
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- RSPrintClient control
- print controls [Reporting Services]
- custom printing [Reporting Services]
- client-side printing
ms.assetid: 8c0bdd18-8905-4e22-9774-a240fc81a8a7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c977799cb20d3735b4cc77d2608c0412ce51769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022308"
---
# <a name="using-the-rsclientprint-control-in-custom-applications"></a>Usar el control RSClientPrint en aplicaciones personalizadas
  El control ActiveX de [!INCLUDE[msCoName](../../../includes/msconame-md.md)]**RSPrintClient** proporciona impresión del lado cliente para los informes mostrados en el Visor HTML. Proporciona un cuadro de diálogo **Imprimir** para que un usuario pueda iniciar un trabajo de impresión, obtener una vista previa de un informe, especificar las páginas que se van a imprimir y cambiar los márgenes. Durante una operación de impresión del lado cliente, el servidor de informes representa el informe en la extensión de representación en imágenes (EMF) y utiliza las capacidades de impresión del sistema operativo para crear el trabajo de impresión y enviarlo a una impresora.  
  
 La impresión del lado cliente ofrece un modo de controlar y mejorar la calidad de la copia impresa de un informe HTML anulando la configuración de impresión del explorador y utilizando, en su lugar, dimensiones de página, márgenes, texto de encabezados y pies de página del informe para crear la salida impresa. El control de impresión lee los valores de las propiedades del informe para establecer el tamaño de página y los márgenes.  
  
 Los desarrolladores que quieran habilitar la característica de impresión del lado cliente en barras de herramientas o visores de terceros pueden acceder al control ActiveX por medio del objeto COM **RSClientPrint**. El control se puede distribuir libremente. En la lista siguiente se ofrecen recomendaciones sobre el uso del control:  
  
-   Utilice el control para mejorar la impresión en informes basados en web. Puede especificar el objeto en cualquier lenguaje de programación compatible con [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] o en script. El control no está diseñado para aplicaciones de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows Forms.  
  
-   Copie el archivo .cab de los archivos de programa  [!INCLUDE[ssRSNoVersion](../../../includes/ssrsnoversion-md.md)] y agréguelo al código base de la aplicación personalizada.  
 
-   Use una etiqueta \<OBJECT> para especificar el control.  
  
-   Especifique una dirección URL relativa o completa al archivo .cab en el atributo OBJECT CODEBASE.  
  
-   Especifique la información de versión de su propia aplicación para el archivo .cab con el fin de realizar un seguimiento de la versión que se utiliza en su aplicación.  
    
  
## <a name="rsprintclient-overview"></a>Información general de RSPrintClient  
 El control muestra un cuadro de diálogo de impresión personalizado compatible con características comunes a otros cuadros de diálogo de impresión, incluida la vista previa de impresión, selecciones de páginas para especificar páginas e intervalos de páginas, márgenes y orientación. El control se empaqueta como un archivo CAB. El texto del cuadro de diálogo **Imprimir** aparece traducido a todos los idiomas admitidos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El control ActiveX **RSPrintClient** usa la extensión de representación en imágenes (EMF) para imprimir el informe. Se usa la siguiente información del dispositivo EMF: StartPage, EndPage, MarginBottom, MarginLeft, MarginTop, MarginRight, PageHeight y PageWidth. No se admite ninguna otra configuración de información de dispositivo para la representación en imágenes.  
  
### <a name="language-support"></a>Compatibilidad con idiomas  
 El control de impresión proporciona texto de la interfaz de usuario en distintos idiomas y acepta valores de entrada calibrados para diversos sistemas de medida. El idioma y el sistema de medida usados vienen determinados por las propiedades **Culture** y **UICulture**. Ambas propiedades aceptan valores LCID. Si especifica un LCID para un idioma que es una variación de un idioma compatible, obtendrá el idioma que más se aproxime. Si especifica un LCID no compatible y para el que no existe ningún otro LCID que se aproxime, obtendrá Inglés (Estados Unidos).  
  
## <a name="using-rsclientprint-in-code"></a>Usar RSClientPrint en el código  
 El objeto **RSClientPrint** se usa para obtener acceso mediante programación al control ActiveX y a sus métodos y propiedades. El control proporciona un cuadro de diálogo modal para la vista previa de impresión.  
  
### <a name="specifying-default-values"></a>Especificar los valores predeterminados  
 Puede inicializar el cuadro de diálogo **Imprimir** con los valores de margen y página del informe. De manera predeterminada, el cuadro de diálogo **Imprimir** se inicializa con valores de la definición de informe. Puede utilizar los valores predeterminados o especificar valores diferentes mediante el establecimiento de las propiedades en el objeto.  
  
 Todas las dimensiones están establecidas es milímetros. La conversión de medidas se produce en tiempo de ejecución si los elementos **Culture** y **UICulture** están establecidos en configuraciones regionales que no usan medidas métricas.  
  
 Para conocer qué valores se usan para las dimensiones y los márgenes de página, puede usar el método **GetProperties** para recuperar los valores predeterminados:  
  
-   **PageHeight** y **PageWidth** especifican el alto y el ancho de página predeterminados. Cuando se inicia el control de impresión, los valores de estas propiedades se utilizan para seleccionar el tamaño de papel más cercano disponible para la impresora actualmente seleccionada. Si el valor de **PageWidth** es mayor que el de **PageHeight**, la orientación se establece en Horizontal. De lo contrario, se establece en Vertical.  
  
-   De manera predeterminada, **LeftMargin**, **RightMargin**, **TopMargin** y **BottomMargin** están establecidos en 12,2 milímetros.  
  
 Estas propiedades están almacenadas en la colección de propiedades **Item** en el servidor de informes. Los valores se sobrescriben cada vez que se actualiza una definición de informe.  
  
### <a name="rsclientprint-properties"></a>Propiedades de RSClientPrint  
  
|Propiedad|Tipo|RW|Valor predeterminado|Descripción|  
|--------------|----------|--------|-------------|-----------------|  
|MarginLeft|Double|RW|Valor del informe|Obtiene o establece el margen izquierdo. El valor predeterminado, si no lo ha establecido el desarrollador de software o no está especificado en el informe, es 12,2 milímetros.|  
|MarginRight|Double|RW|Valor del informe|Obtiene o establece el margen derecho. El valor predeterminado, si no lo ha establecido el desarrollador de software o no está especificado en el informe, es 12,2 milímetros.|  
|MarginTop|Double|RW|Valor del informe|Obtiene o establece el margen superior. El valor predeterminado, si no lo ha establecido el desarrollador de software o no está especificado en el informe, es 12,2 milímetros.|  
|MarginBottom|Double|RW|Valor del informe|Obtiene o establece el margen inferior. El valor predeterminado, si no lo ha establecido el desarrollador de software o no está especificado en el informe, es 12,2 milímetros.|  
|PageWidth|Double|RW|Valor del informe|Obtiene o establece el ancho de página. El valor predeterminado, si no lo ha establecido el desarrollador o no está incluido en la definición de informe, es de 215,9 milímetros.|  
|PageHeight|Double|RW|Valor del informe|Obtiene o establece el alto de página. El valor predeterminado, si no lo ha establecido el desarrollador de software o no está incluido en la definición de informe, es 279,4 milímetros.|  
|Referencia cultural|Int32|RW|Configuración regional del explorador|Especifica el identificador de configuración regional (LCID). Este valor determina la unidad de medida para la entrada del usuario. Por ejemplo, si un usuario escribe **3**, el valor se mide en milímetros si el idioma es el francés, o en pulgadas, si el idioma es el inglés (Estados Unidos). Los valores válidos son: 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052 y 3082.|  
|UICulture|String|RW|Valor de cultura del cliente|Especifica la traducción de las cadenas del cuadro de diálogo. El texto del cuadro de diálogo Imprimir está traducido a los idiomas que se indican a continuación: alemán, chino simplificado, chino tradicional, coreano, español, francés, inglés, italiano y japonés. Los valores válidos son: 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052 y 3082.|  
|Authenticate|Boolean|RW|False|Especifica si el control envía un comando GET en el servidor de informes para iniciar una conexión para la impresión fuera de sesión.|  
  
### <a name="when-to-set-the-authenticate-property"></a>Cuándo establecer la propiedad Authenticate  
 Al imprimir desde una sesión del explorador, no es necesario establecer la propiedad **Authenticate**. En el contexto de una sesión activa, todas las solicitudes del control de impresión que se hacen al servidor de informes se controlan a través del propio explorador. El explorador establece las variables de sesión necesarias para la comunicación con el servidor de informes.  
  
 Si realiza una impresión fuera de sesión (por ejemplo, al enviar un informe directamente a una impresora sin abrirlo), el control de impresión debe emitir una solicitud HTTP **GET** para configurar la sesión en el servidor de informes. Para emitir la solicitud **GET**, establezca **Authenticate** en **True**.  
  
 Solo hay que emitir la solicitud **GET** si se está usando la seguridad integrada de Windows o la autenticación básica. Si se está usando la autenticación de formularios, la propiedad **Authenticate** no se tiene en cuenta. El código de la aplicación debe establecer la sesión y autenticar el usuario mediante la extensión de seguridad personalizada proporcionada. Si se utiliza la autenticación de formularios, es necesario establecer el valor de expiración de la cookie de autenticación en un valor que conserve las sesiones durante un periodo de tiempo razonable. Si el valor es demasiado bajo, se pedirá los usuarios que proporcionen las credenciales de inicio de sesión cada vez que expire la cookie.  
  
### <a name="clsids"></a>CLSID  
 Cuando ejecute el informe local, use los siguientes valores de CLSID.  
  
-   41861299-EAB2-4DCC-986C-802AE12AC499  
  
-   5554DCB0-700B-498D-9B58-4E40E5814405  
  
-   60677965-AB8B-464f-9B04-4BA871A2F17F  
  
 Cuando ejecute el informe en Azure SQL Reporting, use los siguientes valores de CLSID.  
  
-   3DD32426-554D-48C0-A200-65D3BF880E38  
  
-   05662494-ACF9-446A-BE4C-7D3F7EA7F62F  
  
### <a name="rsprintclient-support-for-the-print-method"></a>Compatibilidad de RSPrintClient con el método Print  
 El objeto **RSClientPrint** admite el método **Print** usado para iniciar el cuadro de diálogo Imprimir. El método **Print** dispone de los siguientes argumentos.  
  
|Argumento|E/S|Tipo|Descripción|  
|--------------|----------|----------|-----------------|  
|ServerPath|En|String|Especifica el directorio virtual del servidor de informes (por ejemplo, `https://adventure-works/reportserver`).|  
|ReportPathParameters|En|String|Especifica el nombre completo para obtener acceso al informe en el espacio de nombres de carpetas del servidor de informes, incluidos los parámetros. Los informes se recuperan mediante el acceso a una dirección URL. Por ejemplo: "/AdventureWorks Sample Reports/Employee Sales Summary&EmpID=1234"|  
|ReportName|En|String|Nombre corto del informe (en el ejemplo anterior, el nombre corto es Employee Sales Summary). Aparece en el cuadro de diálogo Imprimir y en la cola de impresión.|  
  
### <a name="example"></a>Ejemplo  
 El siguiente ejemplo HTML muestra cómo especificar el archivo .cab, el método **Print** y las propiedades en JavaScript:  
  
 `<BODY onload="Print()">`  
  
 `<OBJECT ID="RSClientPrint" CLASSID="CLSID: 5554DCB0-700B-498D-9B58-4E40E5814405D3" CODEBASE="<URL to the .CAB file>#Version=<your application version information>" VIEWASTEXT></OBJECT>`  
  
 `<script language="javascript">`  
  
 `function Print()`  
  
 `{`  
  
 `RSClientPrint.MarginLeft = 12.7;`  
  
 `RSClientPrint.MarginTop = 12.7;`  
  
 `RSClientPrint.MarginRight = 12.7;`  
  
 `RSClientPrint.MarginBottom = 12.7;`  
  
 `RSClientPrint.Culture = 1033;`  
  
 `RSClientPrint.UICulture = 9;`  
  
 `RSClientPrint.Print('https://localhost/rtm', '%2fEmployee_Sales_Summary&ReportMonth=6&ReportYear=2004&EmpID=20', 'Employee_Sales_Summary')`  
  
 `}`  
  
 `</script>`  
  
 `</BODY>`  
  
## <a name="see-also"></a>Consulte también  
 [Imprimir informes desde un explorador usando el control de impresión &#40;Generador de informes y SSRS&#41;](../../../reporting-services/report-builder/print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs.md)   
 [Imprimir informes &#40;Generador de informes y SSRS&#41;](../../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [Configuración de la información del dispositivo de imagen](../../../reporting-services/image-device-information-settings.md)  
  
  
