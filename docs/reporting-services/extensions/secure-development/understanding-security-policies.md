---
title: Descripción de las directivas de seguridad | Microsoft Docs
description: Obtenga información sobre las directivas de seguridad de Reporting Services, que especifican permisos para ensamblados, extensiones y expresiones de informe basadas en grupos de código.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- code access security [Reporting Services], security policies
- Execution permission set
- custom assemblies [Reporting Services], security
- permission sets [Reporting Services]
- expressions [Reporting Services], security
- evidence [Reporting Services]
- FullTrust permission set
- security policies [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: a9bf043a-139a-4929-9a58-244815323df0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 072a2f88ce87c5330614961dc1aed842aae2e058
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065280"
---
# <a name="understanding-security-policies"></a>Descripción de las directivas de seguridad
  Cualquier código ejecutado por un servidor de informes debe formar parte de una directiva de seguridad de acceso del código concreta. Estas directivas de seguridad constan de grupos de código que asignan evidencias a un conjunto de conjuntos de permisos con nombre. Con frecuencia, los grupos de código están asociados a un conjunto de permisos con nombre que especifica los permisos que puede tener el código de ese grupo. El motor en tiempo de ejecución usa las evidencias proporcionadas por un host de confianza o por el cargador para determinar a qué grupos de código pertenece el código y, por tanto, qué permisos se deben conceder al código. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] cumple esta arquitectura de la directiva de seguridad como lo define Common Language Runtime (CLR) de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. En las secciones siguientes se describen los distintos tipos de código de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y las reglas de directivas asociadas a los mismos.  
  
## <a name="report-server-assemblies"></a>Ensamblados del servidor de informes  
 Los ensamblados del servidor de informes son aquellos que contienen código que forma parte del producto [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] se escribe mediante ensamblados de código administrado; todos estos ensamblados tienen nombres seguros (es decir, están firmados digitalmente). Los grupos de código para estos ensamblados se definen mediante **StrongNameMembershipCondition**, que proporciona evidencias basadas en información de clave pública para el nombre seguro del ensamblado. Al grupo de código se le permite el conjunto de permisos **FullTrust**.  
  
## <a name="report-server-extensions-rendering-data-delivery-and-security"></a>Extensiones del servidor de informes (Representación, Datos, Entrega y Seguridad)  
 Las extensiones del servidor de informes son extensiones de seguridad, representación, entrega y datos personalizados que crean los usuarios para extender la funcionalidad de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Debe conceder el permiso **FullTrust** a estas extensiones o código de ensamblado en los archivos de configuración de directivas asociados al componente [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que está extendiendo. Las extensiones incluidas como parte de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] se firman con la clave pública del servidor de informes y reciben el conjunto de permisos **FullTrust**.  
  
> [!IMPORTANT]  
>  Debe modificar los archivos de configuración de directivas de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] para permitir **FullTrust** a cualquier extensión de otro fabricante. Si no agrega un grupo de código con **FullTrust** para sus extensiones personalizadas, el servidor de informes no podrá usarlas.  
  
 Para más información sobre los archivos de configuración de directivas de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vea [Using Reporting Services Security Policy Files](../../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md) (Usar los archivos de directivas de seguridad de Reporting Services).  
  
## <a name="expressions-used-in-reports"></a>Expresiones usadas en informes  
 Las expresiones de informes son expresiones de código insertado o métodos definidos por el usuario incluidos dentro del elemento **Code** de un archivo de lenguaje RDL (Report Definition Language). Hay un grupo de código ya configurado en los archivos de directivas que concede a estas expresiones el conjunto de permisos **Ejecución** de forma predeterminada. El grupo de código tiene un aspecto similar al siguiente:  
  
```  
<CodeGroup  
   class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="Execution"  
   Name="Report_Expressions_Default_Permissions"  
   Description="This code group grants default permissions for code in report expressions and Code element. ">  
    <IMembershipCondition  
       class="StrongNameMembershipCondition"  
       version="1"  
       PublicKeyBlob="002400..."  
    />  
</CodeGroup>  
```  
  
 El permiso **Ejecución** permite la ejecución del código, pero no el uso de recursos protegidos. Todas las expresiones incluidas en un informe se compilan en un ensamblado (denominado "ensamblado de expresiones") que se almacena como parte del informe compilado. Cuando se ejecuta el informe, el servidor de informes carga el ensamblado de expresiones y realiza llamadas a ese ensamblado para ejecutar expresiones. Los ensamblados de expresiones se firman con una clave concreta que se usa para definir el grupo de código para todos los ensamblados de expresiones.  
  
 Las expresiones de informe hacen referencia a las colecciones de modelos de objetos (campos, parámetros, etc.) y realizan tareas simples como operaciones de cadenas y aritméticas. El código que realiza estas operaciones simples solo requiere el permiso **Ejecución**. De forma predeterminada, a los métodos definidos por el usuario en el elemento **Code** y cualquier ensamblado personalizado se les concede el permiso **Ejecución** en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Así, para la mayoría de las expresiones, la configuración actual no requiere la modificación de ningún archivo de directiva de seguridad. Para conceder permisos adicionales a los ensamblados de expresiones, un administrador tiene que modificar los archivos de configuración de directivas del servidor de informes y el Diseñador de informes, y cambiar el grupo de código de expresiones de informe. Dado que es un valor global, el cambio de los permisos predeterminados para los ensamblados de expresiones afecta a todos los informes. Por esta razón, resulta muy recomendable que coloque todo el código que requiere seguridad adicional en un ensamblado personalizado. Solo a este ensamblado se le concederán los permisos que necesita.  
  
> [!IMPORTANT]  
>  El código que llama a ensamblados externos o a recursos protegidos debe incorporarse a un ensamblado personalizado para usarlo en informes. Al hacerlo, obtendrá más control sobre los permisos solicitados y validados por su código. No debería realizar llamadas para proteger métodos dentro del elemento **Code**. Si lo hace, se le exigirá que conceda el permiso **FullTrust** al ensamblado de expresiones de informe y se concederá todo el acceso total del código personalizado al CLR.  
  
> [!CAUTION]  
>  No conceda el permiso **FullTrust** al grupo de código de un ensamblado de expresiones de informe. Si lo hace, permitirá que todas las expresiones de informe realicen llamadas al sistema protegidas.  
  
## <a name="custom-assemblies-referenced-in-reports"></a>Ensamblados personalizados a los que se hace referencia en informes  
 Algunas expresiones de informes pueden llamar a ensamblados de terceros, también conocidos en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] como ensamblados personalizados. El servidor de informes espera que estos ensamblados tengan al menos el permiso **Ejecución** en los archivos de configuración de directivas. De forma predeterminada, los archivos de directivas que se incluyen con [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] conceden el permiso **Ejecución** a todos los ensamblados que se inician desde la zona 'Mi PC'. Puede conceder permisos adicionales a ensamblados personalizados según sea necesario.  
  
 En algunos casos, puede que necesite realizar una operación que requiera permisos de código concretos en una expresión de informe. Normalmente, esto significa que una expresión de informe tiene que realizar una llamada a un método de biblioteca CLR protegido (como uno que tiene acceso a archivos o al Registro del sistema). La documentación de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] describe los permisos de código que se exigen para realizar esta llamada segura; para ejecutar la llamada, al código de llamada se le deben conceder estos permisos concretos y seguros. Si realiza la llamada desde una expresión de informe o el elemento **Code**, al ensamblado de expresiones se le deben conceder los permisos adecuados. Sin embargo, una vez que concede los permisos al ensamblado de expresiones, a todo el código que se ejecuta en cualquier expresión de cualquier informe se le concede dicho permiso concreto. Es mucho más seguro realizar la llamada desde un ensamblado personalizado y conceder los permisos concretos a dicho ensamblado personalizado.  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de acceso del código en Reporting Services](../../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)   
 [Desarrollo seguro &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
  
