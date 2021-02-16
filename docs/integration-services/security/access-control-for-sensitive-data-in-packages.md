---
description: Control del acceso a la información confidencial en paquetes
title: Control del acceso a la información confidencial en paquetes | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.packageprotectionlevel.f1
- sql13.ssis.bids.projectprotectionlevel.f1
- sql13.dts.designer.packagepassword.f1
- sql13.ssis.bids.projectpassword.f1
helpviewer_keywords:
- passwords [Integration Services]
- packages [Integration Services], security
- protection levels for packages [Integration Services]
- configuration files [Integration Services]
- encryption [Integration Services]
- cryptography [Integration Services]
- security [Integration Services], protection levels
ms.assetid: d4b073c4-4238-41fc-a258-4e114216e185
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b85dc4949e363a8e62ca3d992a04365f7ce414f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353010"
---
# <a name="access-control-for-sensitive-data-in-packages"></a>Control del acceso a la información confidencial en paquetes

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Para proteger los datos de un paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puede establecer un nivel de protección que ayude a proteger los datos confidenciales únicamente o todos los datos del paquete. Asimismo, puede cifrar estos datos con una contraseña o una clave de usuario, o confiar en que la base de datos cifre los datos. Además, el nivel de protección que utiliza para un paquete no es necesariamente estático, sino que cambia a lo largo de su ciclo de vida. A menudo se establece un nivel de protección durante el desarrollo y otro cuando el paquete se implementa.  
  
> [!NOTE]  
>  Además de los niveles de protección descritos en este tema, puede usar roles fijos de nivel de base de datos para proteger los paquetes que se guardan en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
## <a name="definition-of-sensitive-information"></a>Definición de la información confidencial  
 En un paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , la información siguiente se define como *confidencial*:  
  
-   La parte de contraseña de una cadena de conexión. Sin embargo, si elige una opción que cifra todo, la cadena de conexión completa se considerará confidencial.  
  
-   Los nodos XML generados por la tarea y etiquetados como confidenciales. El etiquetado de los nodos XML está controlado por [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y los usuarios no lo pueden cambiar.  
  
-   Todas las variables marcadas como confidenciales. El marcado de las variables está controlado por [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 El que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] considere una propiedad como confidencial dependerá de si el programador del componente de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , como un administrador de conexiones o una tarea, ha designado la propiedad como tal. Los usuarios no pueden agregar propiedades a la lista de propiedades que se consideran confidenciales, ni pueden quitarlas de la lista.  
  
## <a name="encryption"></a>Cifrado  
 El cifrado, como se usa en los niveles de protección de paquetes, se realiza con la API de protección de datos (DPAPI) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] , que forma parte de la API de criptografía (CryptoAPI).  
  
 Los niveles de protección de paquetes que cifran los paquetes con contraseñas requieren que el usuario también suministre una contraseña. Si cambia el nivel de protección de uno que no utilice contraseñas a otro que sí lo haga, se le pedirá una contraseña.  
  
 Además, para los niveles de protección que usan una contraseña, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa el algoritmo de cifrado Triple DES con una longitud de clave de 192 bits, disponible en la biblioteca de clases de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (FCL).  
  
## <a name="protection-levels"></a>Niveles de protección  
 En la tabla siguiente se describen los niveles de protección que proporciona [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Los valores entre paréntesis son valores de la enumeración <xref:Microsoft.SqlServer.Dts.Runtime.DTSProtectionLevel> . Estos valores aparecen en la ventana Propiedades que se utiliza para configurar las propiedades del paquete al trabajar con paquetes en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
|Nivel de protección|Descripción|  
|----------------------|-----------------|  
|No guardar la información confidencial (**DontSaveSensitive**)|Suprime los valores de las propiedades confidenciales del paquete al guardarlo. Este nivel de protección no cifra, sino que impide que se guarden con el paquete las propiedades marcadas como confidenciales y, en consecuencia, impide que los datos confidenciales estén disponibles para otros usuarios. Si un usuario distinto abre el paquete, aparecerán espacios en blanco en el lugar de la información confidencial y usuario deberá suministrar la información confidencial.<br /><br /> Cuando se usa con la utilidad **dtutil** (dtutil.exe), este nivel de protección se corresponde con el valor de 0.|  
|Cifrar todo con contraseña (**EncryptAllWithPassword**)|Usa una contraseña para cifrar todo el paquete. El paquete se cifra con una contraseña suministrada por el usuario al crear o exportar el paquete. Para abrir el paquete en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] o ejecutarlo con la utilidad del símbolo del sistema **dtexec** , el usuario debe proporcionar la contraseña del paquete. Sin la contraseña, el usuario no tiene acceso al paquete ni lo puede ejecutar.<br /><br /> Cuando se usa con la utilidad **dtutil** , este nivel de protección corresponde al valor 3.|  
|Cifrar todo con clave de usuario (**EncryptAllWithUserKey**)|Utiliza una clave que se basa en el perfil de usuario actual para cifrar el paquete entero. El usuario que creó o exportó el paquete es el único que puede abrirlo en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] o ejecutarlo con la utilidad del símbolo del sistema **dtexec** .<br /><br /> Cuando se usa con la utilidad **dtutil** , este nivel de protección corresponde al valor 4.<br /><br /> Nota: Para los niveles de protección que utilizan una clave de usuario, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utiliza los estándares DPAPI. Para obtener más información sobre DPAPI, vea MSDN Library en [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Cifrar información confidencial con contraseña (**EncryptSensitiveWithPassword**)|Utiliza una contraseña para cifrar solamente los valores de las propiedades confidenciales en el paquete. Para el cifrado, se utiliza DPAPI. Los datos confidenciales se guardan como parte del paquete, pero los datos se cifran utilizando una contraseña que el usuario actual suministra al crear o exportar el paquete. Para abrir el paquete en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , el usuario debe proporcionar la contraseña del paquete. Si no se proporciona la contraseña, el paquete se abre sin los datos confidenciales y el usuario actual debe proporcionar valores nuevos para estos datos. Si el usuario intenta ejecutar el paquete sin proporcionar la contraseña, el paquete no se ejecutará. Para obtener más información acerca de las contraseñas y la ejecución desde la línea de comandos, vea [dtexec Utility](../../integration-services/packages/dtexec-utility.md).<br /><br /> Cuando se usa con la utilidad **dtutil** , este nivel de protección corresponde al valor 2.|  
|Cifrar información confidencial con clave de usuario (**EncryptSensitiveWithUserKey**)|Utiliza una clave que se basa en el perfil de usuario actual para cifrar solamente los valores de las propiedades confidenciales en el paquete. Solo el mismo usuario que use el mismo perfil puede cargar el paquete. Si un usuario distinto abre el paquete, aparecerán espacios en blanco en el lugar de la información confidencial y usuario actual deberá suministrar valores nuevos para los datos confidenciales. Si el usuario intenta ejecutar el paquete, se generará un error. Para el cifrado, se utiliza DPAPI.<br /><br /> Cuando se usa con la utilidad **dtutil** , este nivel de protección corresponde al valor 1.<br /><br /> Nota: Para los niveles de protección que utilizan una clave de usuario, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utiliza los estándares DPAPI. Para obtener más información sobre DPAPI, vea MSDN Library en [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Basar el cifrado en el almacenamiento del servidor (**ServerStorage**)|Protege el paquete completo usando roles de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esta opción solo se admite cuando se guarda un paquete en la base de datos msdb de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Además, el catálogo de SSISDB emplea el nivel de protección **ServerStorage**<br /><br /> Esta opción no se admite cuando un paquete se guarda en el sistema de archivos desde [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|  
  
## <a name="protection-level-setting-and-the-ssisdb-catalog"></a>Configuración del nivel de protección y el catálogo de SSISDB  
 El catálogo de SSISDB emplea el nivel de protección **ServerStorage** . Al implementar un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , el catálogo cifra automáticamente los datos del paquete y los valores confidenciales. El catálogo también descifra automáticamente los datos cuando lo recupera.  
  
 Si exporta el proyecto (archivo .ispac) desde el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al sistema de archivos, el sistema cambiará automáticamente el nivel de protección a **EncryptSensitiveWithUserKey**. Si importa el proyecto mediante el **Asistente para importar proyectos de Integration Services** en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la propiedad **ProtectionLevel** de la ventana **Propiedades** muestra un valor **EncryptSensitiveWithUserKey**.  
  
## <a name="protection-level-setting-based-on-package-life-cycle"></a>Establecimiento del nivel de protección basado en el ciclo de vida de paquetes  
 El nivel de protección de un paquete de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se establece cuando se desarrolla por primera vez en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Posteriormente, al implementar, importar o exportar el paquete desde [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], o copiarlo desde [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el Almacén de paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] o el sistema de archivos puede actualizar su nivel de protección. Por ejemplo, si crea y guarda los paquetes en un equipo con una de las opciones de nivel de protección de claves de usuario, es probable que necesite cambiar el nivel de protección al entregar el paquete a otros usuarios; de lo contrario, no podrán abrirlo.  
  
 Normalmente, el nivel de protección se cambia como se muestra en los pasos siguientes:  
  
1.  Durante el desarrollo, deje el nivel de protección de los paquetes establecido en el valor predeterminado: **EncryptSensitiveWithUserKey**. Este valor ayuda a asegurarse de que solamente el programador vea los valores confidenciales del paquete. O bien, puede considerar usar **EncryptAllWithUserKey** o **DontSaveSensitive**.  
  
2.  Cuando llegue el momento de implementar los paquetes, tiene que cambiar el nivel de protección por uno que no dependa de la clave de usuario del programador. Por tanto, normalmente tiene que seleccionar **EncryptSensitiveWithPassword** o **EncryptAllWithPassword**. Cifre los paquetes asignando una contraseña segura temporal que también conozca el equipo de operaciones del entorno de producción.  
  
3.  Una vez implementados los paquetes en el entorno de producción, el equipo de operaciones puede volver a cifrar los paquetes implementados asignando una contraseña segura que solo ellos conozcan. O bien, pueden cifrar los paquetes implementados seleccionando **EncryptSensitiveWithUserKey** o **EncryptAllWithUserKey**, y utilizando las credenciales locales de la cuenta que ejecutará los paquetes.  

## <a name="set-or-change-the-protection-level-of-packages"></a><a name="set_protection"></a> Establecer o cambiar el nivel de protección de los paquetes
  Para controlar el acceso al contenido de los paquetes y a los valores confidenciales que contienen, como las contraseñas, establezca el valor de la propiedad **ProtectionLevel** . Los paquetes que se incluyen en un proyecto deben disponer del mismo nivel de protección que el proyecto con el fin de compilar el proyecto. Si cambia la configuración de las propiedades de **ProtectionLevel** en el proyecto, debe actualizar manualmente el valor de la propiedad de los paquetes.  
  
 Para obtener información general sobre las características de seguridad de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Información general sobre seguridad &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
 Los procedimientos de este tema describen cómo utilizar [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o la utilidad de símbolo del sistema dtutil para cambiar la propiedad **ProtectionLevel** .  
  
> [!NOTE]  
>  Además de los procedimientos de este tema, normalmente puede establecer o cambiar la propiedad **ProtectionLevel** de un paquete al importarlo o exportarlo. También puede cambiar la propiedad **ProtectionLevel** de un paquete al utilizar el Asistente para importación y exportación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para guardarlo.  
  
### <a name="to-set-or-change-the-protection-level-of-a-package-in-sql-server-data-tools"></a>Para establecer o cambiar el nivel de protección de un paquete en herramientas de datos de SQL Server  
  
1.  Revise los valores disponibles para la propiedad **ProtectionLevel** en la sección [Niveles de protección](#protection-levels) y determine el valor adecuado para su paquete.  
  
2.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete.  
  
3.  Abra el paquete en el diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
4.  Si la ventana Propiedades no muestra las propiedades del paquete, haga clic en la superficie de diseño.  
  
5.  En la ventana Propiedades, en el grupo **Seguridad** , seleccione el valor adecuado para la propiedad **ProtectionLevel** .  
  
     Si selecciona un nivel de protección que requiere una contraseña, escríbala como valor de la propiedad **PackagePassword** .  
  
6.  En el menú **Archivo** , seleccione **Guardar los elementos seleccionados** para guardar el paquete modificado.  
  
### <a name="to-set-or-change-the-protection-level-of-packages-at-the-command-prompt"></a>Para establecer o cambiar el nivel de protección de los paquetes en el símbolo del sistema  
  
1.  Revise los valores disponibles para la propiedad **ProtectionLevel** en la sección [Niveles de protección](#protection-levels) y determine el valor adecuado para su paquete.  
  
2.  Revise las asignaciones para la opción **Encrypt** en el tema [dtutil Utility](../../integration-services/dtutil-utility.md)y determine el número entero adecuado que usar como valor de la propiedad **ProtectionLevel** seleccionada.  
  
3.  Abra una ventana de símbolo del sistema.  
  
4.  En el símbolo del sistema, navegue a la carpeta que contiene el paquete o paquetes para los que desea establecer la propiedad **ProtectionLevel** .  
  
     Los ejemplos de sintaxis mostrados en el paso siguiente suponen que esta carpeta es la actual.  
  
5.  Establezca o cambie el nivel de protección del paquete o paquetes utilizando un comando similar al de los ejemplos siguientes:  
  
    -   El comando siguiente establece la propiedad **ProtectionLevel** de un paquete individual en el sistema de archivos en el nivel 2, "Cifrado confidencial con contraseña", con la contraseña "strongpassword":  
  
         `dtutil.exe /file "C:\Package.dtsx" /encrypt file;"C:\Package.dtsx";2;strongpassword`  
  
    -   El comando siguiente establece la propiedad **ProtectionLevel** de todos los paquetes en una carpeta concreta del sistema de archivos en el nivel 2, "Cifrado confidencial con contraseña", con la contraseña "strongpassword":  
  
         `for %f in (*.dtsx) do dtutil.exe /file %f /encrypt file;%f;2;strongpassword`  
  
         Si utiliza un comando similar en un archivo por lotes, escriba el marcador de posición del archivo, "% f", como "%% f" en el archivo por lotes.  

## <a name="package-project-protection-level-dialog-box"></a><a name="protection_dialog"></a> Cuadro de diálogo Package Project Protection Level (Nivel de protección del proyecto de paquetes)
  Utilice el cuadro de diálogo **Nivel de protección de paquetes** para actualizar el nivel de protección de un paquete. El nivel de protección determina el método de protección, la contraseña o la clave de usuario, así como el ámbito de protección de paquetes. La protección puede incluir todos los datos o solo los datos confidenciales.  
  
 Para entender los requisitos y las opciones de la seguridad de paquetes, puede resultar útil ver el tema [Security Overview &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md) (Información general sobre seguridad &#40;Integration Services&#41;).  
  
### <a name="options"></a>Opciones  
 **Nivel de protección de paquetes**  
 Seleccione un nivel de protección de la lista.  
  
 **Contraseña**  
 Si usa el nivel de protección **Cifrar la información confidencial con una contraseña** o **Cifrar todos los datos con una contraseña** , escriba una contraseña.  
  
 **Volver a escribir la contraseña**  
 Escriba la contraseña nuevamente.  

## <a name="package-password-dialog-box"></a><a name="password_dialog"></a>Cuadro de diálogo Contraseña del paquete
  Utilice el cuadro de diálogo **Contraseña del paquete** con el fin de proporcionar la contraseña para un paquete cifrado con una contraseña. Debe proporcionar una contraseña si el paquete utiliza el nivel de protección **Cifrar la información confidencial con una contraseña** o **Cifrar todos los datos con una contraseña** .  
  
### <a name="options"></a>Opciones  
 **Contraseña**  
 Escriba la contraseña.  
  
## <a name="see-also"></a>Consulte también  
 [Paquetes de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Información general sobre seguridad &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
 [dtutil (utilidad)](../../integration-services/dtutil-utility.md)  
