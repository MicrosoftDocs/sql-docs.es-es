---
description: Generar, implementar y depurar objetos personalizados
title: Generar, implementar y depurar objetos personalizados | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom objects [Integration Services]
ms.assetid: b03685bc-5398-4c3f-901a-1219c1098fbe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb634702b48de70d505851c53ce3f1f6562547a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348949"
---
# <a name="building-deploying-and-debugging-custom-objects"></a>Generar, implementar y depurar objetos personalizados

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Después de haber escrito el código para un objeto personalizado de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], debe generar, implementar e integrar el ensamblado en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] para que esté disponible para su uso en paquetes, y probarlo y depurarlo.  
  
##  <a name="steps-in-building-deploying-and-debugging-a-custom-object-for-integration-services"></a><a name="top"></a> Pasos para generar, implementar y depurar un objeto personalizado para Integration Services  
 Ya ha escrito la funcionalidad personalizada para el objeto. Ahora tiene que probarlo y hacer que esté disponible para los usuarios. Los pasos son muy similares para todos los tipos de objetos personalizados que puede crear para [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Estos son los pasos para generarlo, implementarlo y probarlo.  
  
1.  [Firme](#signing) el ensamblado que se va a generar con un nombre seguro.  
  
2.  [Genere](#building) el ensamblado.  
  
3.  [Implemente](#deploying) el ensamblado; para ello, muévalo o cópielo en la carpeta de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] adecuada.  
  
4.  [Instale](#installing) el ensamblado en la caché global de ensamblados (GAC).  
  
     El objeto se agregará automáticamente al cuadro de herramientas.  
  
5.  [Solucione los problemas](#troubleshooting) de la implementación, si es necesario.  
  
6.  [Pruebe](#testing) y depure el código.  
  
 Ahora puede usar el Diseñador SSIS en SQL Server Data Tools (SSDT) para crear, mantener y ejecutar paquetes que se destinen a distintas versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre el impacto de esta mejora en sus extensiones personalizadas, vea [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/) (Conseguir que la compatibilidad con varias versiones de SSDT 2015 para SQL Server 2016 admita extensiones personalizadas de SSIS).  
  
##  <a name="signing-the-assembly"></a><a name="signing"></a> Firmar el ensamblado  
 Cuando se tiene pensado compartir un ensamblado, se debe instalar en la memoria caché de ensamblados global. Una vez agregado el ensamblado a la memoria caché de ensamblados global, aplicaciones como [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pueden utilizarlo. Un requisito de la memoria caché de ensamblados global es que el ensamblado se debe firmar con un nombre seguro, que garantiza que el ensamblado es único de forma global. Un ensamblado con nombre seguro especifica un nombre completo que incluye el nombre del ensamblado, la referencia cultural, la clave pública y el número de versión. El motor en tiempo de ejecución utiliza esta información para localizar el ensamblado y distinguirlo entre otros ensamblados con el mismo nombre.  
  
 Para firmar un ensamblado con un nombre seguro, es necesario crear o tener antes un par de claves publica y privada. Este par de claves criptográficas pública y privada se usa al realizar la compilación para crear un ensamblado con nombre seguro.  
  
 Para obtener más información sobre los nombres seguros y los pasos que debe seguir para firmar un ensamblado, consulte los temas siguientes en la documentación del SDK de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]:  
  
-   Ensamblados con nombre seguro  
  
-   Crear un par de claves  
  
-   Firmar un ensamblado con un nombre seguro  
  
 Puede firmar con facilidad el ensamblado con un nombre seguro en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] durante la generación. En el cuadro de diálogo **Propiedades del proyecto**, seleccione la pestaña **Firma**. Seleccione la opción para **Firmar el ensamblado** y, a continuación, proporcione la ruta de acceso del archivo de claves (.snk).  
  
##  <a name="building-the-assembly"></a><a name="building"></a> Generar el ensamblado  
 Después de firmar el proyecto, debe generar o volver a generar el proyecto o la solución utilizando los comandos disponibles en el menú **Generar** de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. La solución puede contener un proyecto independiente para una interfaz de usuario personalizada, que también se debe firmar con un nombre seguro y se puede generar al mismo tiempo.  
  
 El método más sencillo para realizar los dos pasos siguientes (implementar el ensamblado e instalarlo en la memoria caché de ensamblados global) es crear un script con estos pasos como un evento posterior a la compilación en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Los eventos de compilación están disponibles en la página **Compilar** de Propiedades del proyecto para un proyecto de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] y en la página **Eventos de compilación** para un proyecto de C#. Es necesaria la ruta de acceso completa para las utilidades de símbolo del sistema como **gacutil.exe**. Son necesarias comillas alrededor de las rutas de acceso que contienen espacios en blanco y alrededor de macros como $(TargetPath) que se expanden a rutas de acceso que contienen espacios en blanco.  
  
 A continuación figura un ejemplo de una línea de comandos del evento posterior a la generación para un proveedor de registro personalizado:  
  
```cmd
"C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe" -u $(TargetName)  
"C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe" -i $(TargetFileName)  
copy $(TargetFileName) "C:\Program Files\Microsoft SQL Server\130\DTS\LogProviders "  
```  
  
##  <a name="deploying-the-assembly"></a><a name="deploying"></a> Implementar el ensamblado  
 El Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] busca los objetos personalizados disponibles para su uso en paquetes enumerando los archivos buscados en una serie de carpetas que se crean cuando se instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Cuando se utilizan los valores de instalación predeterminados de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este conjunto de carpetas se encuentra en **C:\Archivos de programa\Microsoft SQL Server\130\DTS**. Sin embargo, si crea un programa de instalación para el objeto personalizado, debe comprobar el valor de la clave del Registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\Setup\DtsPath** para comprobar la ubicación de esta carpeta.  
  
> [!NOTE]  
>  Para obtener información sobre cómo implementar componentes personalizados para que funcionen bien con la compatibilidad con varias versiones de SQL Server Data Tools, consulte [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/) (Conseguir que la compatibilidad con varias versiones de SSDT 2015 para SQL Server 2016 admita extensiones personalizadas de SSIS).  
  
 Puede colocar el ensamblado en la carpeta de dos maneras:  
  
-   Copiar o mover el ensamblado compilado a la carpeta adecuada después de generarlo. (Para su comodidad, puede incluir el comando de copia en un evento posterior a la generación).  
  
-   Generar el ensamblado directamente en la carpeta adecuada.  
  
 Las carpetas de implementación de **C:\Archivos de programa\Microsoft SQL Server\130\DTS** se usan para los distintos tipos de objetos personalizados:  
  
|Objeto personalizado|Carpeta de implementación|  
|-------------------|-----------------------|  
|Tarea|Tareas|  
|Administrador de conexiones|Conexiones|  
|Proveedor de registro|LogProviders|  
|Componente de flujo de datos|PipelineComponents|  
  
> [!NOTE]  
>  Los ensamblados se copian en estas carpetas para admitir la enumeración de tareas disponibles, administradores de conexión, etc. Por consiguiente no tiene que implementar los ensamblados que contienen solo la interfaz de usuario personalizada para los objetos personalizados en estas carpetas.  
  
##  <a name="installing-the-assembly-in-the-global-assembly-cache"></a><a name="installing"></a> Instalar el ensamblado en la memoria caché de ensamblados global  
 Para instalar el ensamblado de tarea en la caché global de ensamblados (GAC), use la herramienta de línea de comandos **gacutil.exe** o arrastre los ensamblados al directorio `%system%\assembly`. Para su comodidad, puede incluir también la llamada a **gacutil.exe** en un evento posterior a la compilación.  
  
 El comando siguiente instala un componente denominado *MyTask.dll* en la GAC mediante **gacutil.exe**.  
  
 `gacutil /iF MyTask.dll`  
  
 Se debe cerrar y volver a abrir el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] después de instalar una nueva versión del objeto personalizado. Si ha instalado versiones anteriores del objeto personalizado en la caché global de ensamblados, debe quitarlos antes de instalar la nueva versión. Para desinstalar un ensamblado, ejecute **gacutil.exe** y especifique el nombre del ensamblado con la opción `/u`.  
  
 Para obtener más información sobre la caché global de ensamblados, vea Herramienta Caché global de ensamblados (Gactutil.exe) en [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Tools.  
  
##  <a name="troubleshooting-the-deployment"></a><a name="troubleshooting"></a> Solucionar problemas relacionados con la implementación  
 Si el objeto personalizado aparece en el **cuadro de herramientas** o en la lista de objetos disponibles, pero no puede agregarlo a un paquete, pruebe lo siguiente:  
  
1.  Busque en la memoria caché de ensamblados global varias versiones del componente. Si hay varias versiones del componente en la memoria caché de ensamblados global, el diseñador quizá no pueda cargar el componente. Elimine todas las instancias del ensamblado de la memoria caché de ensamblados global y vuelva a agregar el ensamblado.  
  
2.  Asegúrese de que solo existe una instancia única del ensamblado en la carpeta de implementación.  
  
3.  Actualice el cuadro de herramientas.  
  
4.  Adjunte [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] a **devenv.exe** y establezca un punto de interrupción para reproducir paso a paso el código de inicialización con el fin de asegurarse de que no se produce ninguna excepción.  
  
##  <a name="testing-and-debugging-your-code"></a><a name="testing"></a> Probar y depurar el código  
 El enfoque más sencillo para depurar los métodos en tiempo de ejecución de un objeto personalizado consiste en iniciar **dtexec.exe** desde [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] después de compilar el objeto personalizado y ejecutar un paquete que use el componente.  
  
 Si desea depurar los métodos en tiempo de diseño del componente, como el método **Validate**, abra un paquete que use el componente en una segunda instancia de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] y adjúntelo a su proceso **devenv.exe**.  
  
 Si también quiere depurar los métodos en tiempo de ejecución del componente cuando un paquete está abierto y ejecutándose en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)], debe forzar una pausa en la ejecución del paquete para poder adjuntarlo también al proceso **DtsDebugHost.exe**.  
  
#### <a name="to-debug-an-objects-run-time-methods-by-attaching-to-dtexecexe"></a>Para depurar los métodos en tiempo de ejecución de un objeto adjuntándolos a dtexec.exe  
  
1.  Firme e integre el proyecto en la configuración de depuración, impleméntelo e instálelo en la memoria caché de ensamblados global tal y como se describe en este tema.  
  
2.  En la pestaña **Depurar** de **Propiedades del proyecto**, seleccione **Programa externo de inicio** como la **Acción de inicio** y busque **dtexec.exe**, que se instala de forma predeterminada en C:\Archivos de programa\Microsoft SQL Server\130\DTS\Binn.  
  
3.  En el cuadro de texto **Opciones de la línea de comandos**, en **Opciones de inicio**, escriba los argumentos de la línea de comandos necesarios para ejecutar un paquete que usa el componente. A menudo el argumento de la línea de comandos estará compuesto del modificador /F[ILE] seguido de la ruta de acceso y nombre de archivo del archivo .dtsx. Para obtener más información, consulte [utilidad dtexec](../../integration-services/packages/dtexec-utility.md).  
  
4.  Establezca los puntos de interrupción en el código fuente donde sea adecuado en los métodos en tiempo de ejecución del componente.  
  
5.  Ejecute el proyecto.  
  
#### <a name="to-debug-a-custom-objects-design-time-methods-by-attaching-to-sql-server-data-tools"></a>Para depurar los métodos en tiempo de diseño de un objeto personalizado adjuntándolos a las herramientas de datos de SQL Server  
  
1.  Firme e integre el proyecto en la configuración de depuración, impleméntelo e instálelo en la memoria caché de ensamblados global tal y como se describe en este tema.  
  
2.  Establezca puntos de interrupción en el código fuente donde se adecuado en los métodos en tiempo de diseño del objeto personalizado.  
  
3.  Abra una segunda instancia de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] y cargue un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene un paquete que utiliza el objeto personalizado.  
  
4.  En la primera instancia de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], adjúntelo a la segunda instancia de **devenv.exe** en la que se carga el paquete seleccionando **Adjuntar al proceso** en el menú **Depurar** de la primera instancia.  
  
5.  Ejecute el paquete desde la segunda instancia de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
#### <a name="to-debug-a-custom-objects-run-time-methods-by-attaching-to-sql-server-data-tools"></a>Para depurar los métodos en tiempo de ejecución de un objeto personalizado adjuntándolos a las herramientas de datos de SQL Server  
  
1.  Una vez completados los pasos enumerados en el procedimiento anterior, fuerce una pausa en la ejecución del paquete para que se pueda adjuntar a **DtsDebugHost.exe**. Para forzar esta pausa, agregue un punto de interrupción al evento **OnPreExecute** o agregue una tarea Script al proyecto y escriba el script que muestra un cuadro de mensaje modal.  
  
2.  Ejecute el paquete. Cuando se produzca la pausa, cambie a la instancia de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] en la que está abierto el código del proyecto y seleccione **Adjuntar al proceso** en el menú **Depurar**. Asegúrese de adjuntarlo a la instancia de **DtsDebugHost.exe** que se muestra como **Administrado, x86** en la columna **Tipo**, no a la instancia mostrada solo como **x86**.  
  
3.  Vuelva al paquete en pausa y continúe más allá del punto de interrupción o haga clic en **Aceptar** para descartar el cuadro de mensaje generado por la tarea Script, y continuar con la ejecución y depuración del paquete.  
  
## <a name="see-also"></a>Consulte también  
 [Desarrollar objetos personalizados para Integration Services](../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)   
 [Conservar objetos personalizados](../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)   
 [Herramientas para solucionar problemas con el desarrollo de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
  
  
