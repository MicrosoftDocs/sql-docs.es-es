---
title: Generación de archivos de volcado para la ejecución de paquetes SSIS
description: Obtenga información sobre cómo solucionar problemas de SQL Server Integration Services mediante las opciones de "Volcado de errores". Estas opciones generan un archivo de volcado de depuración .mdmp y un archivo de volcado de depuración .tmp de texto. Obtenga información sobre los formatos de archivo de volcado de depuración.
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 61ef1731-cb3a-4afb-b4a4-059b04aeade0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d213c8849c23ec1cb57e2628403542a31655a495
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193777"
---
# <a name="generating-dump-files-for-package-execution"></a>Generar archivos de volcado para la ejecución de paquetes

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

En [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], puede crear archivos de volcado de depuración que proporcionen información sobre la ejecución de un paquete. La información de estos archivos puede ayudarle a solucionar los problemas de ejecución del paquete.  
  
> [!NOTE]
> Los archivos de volcado de depuración podrían contener información confidencial. Para proteger la información confidencial, puede utilizar una lista de control de acceso (ACL) con objeto de restringir el acceso a los archivos o copiarlos en una carpeta con acceso restringido. Por ejemplo, antes de enviar los archivos de depuración a los servicios de soporte al cliente de [!INCLUDE[msCoName](../../includes/msconame-md.md)] , se recomienda que quite cualquier información sensible o confidencial.  
  
 Al implementar un proyecto en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puede crear archivos de volcado que proporcionen información sobre la ejecución de paquetes incluidos en el proyecto. Cuando finaliza el proceso de ISServerExec.exe, se crean los archivos de volcado. Puede especificar que un archivo de volcado se cree cuando aparezcan errores durante la ejecución del paquete; para ello, seleccione la opción **Volcado de errores** en el cuadro de diálogo **Ejecutar paquete** . También puede utilizar los siguientes procedimientos almacenados:  
  
-   [catalog.set_execution_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) (catalog.set_execution_parameter_value [base de datos de SSISDB];)  
  
     Llame a este procedimiento almacenado para configurar un archivo de volcado que se creará cuando se produzca algún error o evento y cuando se produzcan eventos específicos durante la ejecución de un paquete.  
  
-   [catalog.create_execution_dump](../../integration-services/system-stored-procedures/catalog-create-execution-dump.md)  
  
     Llame a este procedimiento almacenado para hacer que se detenga un paquete en ejecución y crear un archivo de volcado.  
  
 Si usa el modelo de implementación de paquetes, puede crear archivos de volcado de depuración mediante la utilidad **dtexec** o la utilidad **dtutil** para especificar una opción de volcado de depuración en la línea de comandos. Para obtener más información, consulte [dtexec Utility](../../integration-services/packages/dtexec-utility.md) y [dtutil Utility](../../integration-services/dtutil-utility.md). Para más información sobre el modelo de implementación de paquetes, consulte [Implementación de proyectos y paquetes de Integration Services (SSIS)](../packages/deploy-integration-services-ssis-projects-and-packages.md) y [Implementación de paquetes heredada &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).   
  
## <a name="debug-dump-file-format"></a>Formato de los archivos de volcado de depuración  
 Cuando se especifica una opción de volcado de depuración, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea los archivos de volcado de depuración siguientes:  
  
-   Un archivo de volcado de depuración .mdmp. Es un archivo binario.  
  
-   El archivo de volcado de depuración .tmp. Es un archivo de texto con formato.  
  
 De forma predeterminada, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] almacena estos archivos en la carpeta *\<drive>:* \Archivos de programa\Microsoft SQL Server\110\Shared\ErrorDumps.  
  
 En la tabla siguiente solo se describen determinadas secciones del archivo .tmp. El archivo .tmp incluye datos adicionales que no se incluyen en la tabla.  
  
|Tipo de información|Descripción|Ejemplo|  
|-------------------------|-----------------|-------------|  
|Entorno|Versión del sistema operativo, datos de uso de la memoria, identificador de proceso y nombre de imagen de proceso. La información del entorno se encuentra al principio del archivo .tmp.|# SSIS Textual Dump taken at 9/13/2007 1:50:34 PM<br /><br /> #PID 4120<br /><br /> #Image Name [C:\Program Files\Microsoft SQL Server\110\DTS\Binn\DTExec.exe]<br /><br /> # OS major=6 minor=0 build=6000<br /><br /> # Running on 2 amd64 processors under WOW64<br /><br /> # Memory: 58% in use. Physical: 845M/2044M  Paging: 2404M/4095M (avail/total)|  
|Ruta de acceso y versión de la biblioteca de vínculos dinámicos (DLL)|Ruta de acceso y número de versión de cada DLL que el sistema carga durante el procesamiento de un paquete.|# Loaded Module: c:\bb\Sql\DTS\src\bin\debug\i386\DTExec.exe (10.0.1069.5)<br /><br /> # Loaded Module: C:\Windows\SysWOW64\ntdll.dll (6.0.6000.16386)<br /><br /> # Loaded Module: C:\Windows\syswow64\kernel32.dll (6.0.6000.16386)|  
|Mensajes recientes|Mensajes recientes emitidos por el sistema. Incluye la fecha y hora, el tipo, la descripción y el identificador de subproceso de cada mensaje.|[M:1]   Ring buffer entry:              (*pRecord)<br /><br /> [D:2]      <<\<CRingBufferLogging::RingBufferLoggingRecord>>> ( \@ 0282F1A8 )<br /><br /> [E:3]         Time Stamp: 2007-09-13 13:50:32.786      (szTimeStamp)<br /><br /> [E:3]         Thread ID: 2368           (ThreadID)<br /><br /> [E:3]         Event Name: OnError                        (EventName)<br /><br /> [E:3]         Source Name:                (SourceName)<br /><br /> [E:3]         Source ID:                        (SourceID)<br /><br /> [E:3]         Execution ID:                 (ExecutionGUID)<br /><br /> [E:3]         Data Code: -1073446879              (DataCode)<br /><br /> [E:3]         Description: Falta el componente, no está registrado, no puede actualizarse o faltan interfaces necesarias. La información de contacto para este componente es "".|  
  
## <a name="related-information"></a>Información relacionada  
[Ejecutar paquete (cuadro de diálogo)](../../integration-services/packages/run-integration-services-ssis-packages.md#execute_package_dialog)