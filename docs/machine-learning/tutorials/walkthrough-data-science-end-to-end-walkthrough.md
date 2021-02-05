---
title: 'Tutorial de R: Desarrollo del modelo en SQL'
description: Aprenda a crear una solución completa para el modelado de predicción basado en la compatibilidad con características de R de SQL Server 2016 o SQL Server 2017.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/11/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0537e602d5c6773c4978c7fff0f5bf0f927985c6
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536346"
---
# <a name="tutorial-sql-development-for-r-data-scientists"></a>Tutorial: Desarrollo de SQL para científicos de datos de R
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

En este tutorial para científicos de datos, aprenderá a crear una solución completa para el modelado de predicción basado en la compatibilidad con características de R de SQL Server 2016 o SQL Server 2017. En este tutorial se usa una base de datos [NYCTaxi_sample](demo-data-nyctaxi-in-sql.md) en SQL Server. 

Usará una combinación de código R, datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y funciones SQL personalizadas para generar un modelo de clasificación que indique la probabilidad de que el conductor reciba una propina en un recorrido de taxi determinado. También implementará el modelo R en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y usará datos del servidor para generar puntuaciones según el modelo.

Este ejemplo se puede ampliar a todos los tipos de problemas de la vida real, como predecir las respuestas de los clientes en las campañas de ventas o predecir los gastos o la asistencia a eventos. Dado que el modelo se puede invocar desde un procedimiento almacenado, puede insertarlo con facilidad en una aplicación.

Como el tutorial está diseñado para presentar [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] a los desarrolladores de R, se usará R siempre que sea posible, si bien esto no significa que R sea necesariamente la mejor herramienta para cada tarea. En muchos casos, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría proporcionar un mejor rendimiento, especialmente en tareas como la agregación de datos e ingeniería de características.  Esas tareas pueden beneficiarse de las nuevas características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como los índices de almacén de columnas con optimización para memoria. En el camino le indicaremos posibles optimizaciones.

## <a name="prerequisites"></a>Prerrequisitos

+ [SQL Server Machine Learning Services con integración de R](../install/sql-machine-learning-services-windows-install.md#verify-installation) o [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

+ [Permisos de base de datos](../security/user-permission.md) y un inicio de sesión de usuario de base de datos de SQL Server

+ [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)

+ [Base de datos de demo NYC Taxi](demo-data-nyctaxi-in-sql.md)

+ Un IDE de R como RStudio o la herramienta RGUI integrada incluida con R

Se recomienda realizar este tutorial en una estación de trabajo de cliente. Debe poder conectarse en la misma red a un equipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con SQL Server y el lenguaje R habilitados. Para obtener instrucciones sobre la configuración de la estación de trabajo, vea [Configuración de un cliente de ciencia de datos para el desarrollo en R](../r/set-up-a-data-science-client.md).

Como alternativa, puede ejecutar el tutorial en un equipo que tenga tanto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como un entorno de desarrollo de R, pero esta configuración es desaconsejable en un entorno de producción. Si necesita colocar el cliente y el servidor en el mismo equipo, asegúrese de instalar un segundo conjunto de bibliotecas de Microsoft R para enviar scripts de R desde un cliente "remoto". No use las bibliotecas de R que están instaladas en los archivos de programa de la instancia de SQL Server. Concretamente, si usa un equipo, necesita tener la biblioteca RevoScaleR en ambas ubicaciones para admitir las operaciones de cliente y de servidor.

+ C:\Archivos de programa\Microsoft\R Client\R_SERVER\library\RevoScaleR 
+ C:\Archivos de programa\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

> [!NOTE]
> Si, en lugar de R Client, usa [Machine Learning Server](/machine-learning-server/) o [Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/), la ruta de acceso a RevoScaleR será C:\Archivos de programa\Microsoft\ML Server\R_SERVER\library\RevoScaleR.

<a name="add-packages"></a>

## <a name="additional-r-packages"></a>Otros paquetes de R

Este tutorial requiere varias bibliotecas de R que no se instalan de forma predeterminada como parte de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Debe instalar los paquetes tanto en el cliente donde desarrolle la solución como en el equipo con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde la implemente.

### <a name="on-a-client-workstation"></a>En una estación de trabajo de cliente

En el entorno de R, copie las siguientes líneas y ejecute el código en una ventana de la consola (RGui o un IDE). Algunos paquetes también instalan paquetes necesarios. En general, se instalarán unos 32 paquetes. Deberá disponer de conexión a Internet para poder realizar este paso.
    
  ```R
  # Install required R libraries, if they are not already installed.
  if (!('ggmap' %in% rownames(installed.packages()))){install.packages('ggmap')}
  if (!('mapproj' %in% rownames(installed.packages()))){install.packages('mapproj')}
  if (!('ROCR' %in% rownames(installed.packages()))){install.packages('ROCR')}
  if (!('RODBC' %in% rownames(installed.packages()))){install.packages('RODBC')}
  ```

### <a name="on-the-server"></a>En el servidor

Existen varias opciones para instalar paquetes en SQL Server. Por ejemplo, SQL Server proporciona una característica de [administración de paquetes de R](../package-management/install-additional-r-packages-on-sql-server.md) que permite a los administradores de bases de datos crear un repositorio de paquetes y asignar a los usuarios derechos para instalar sus propios paquetes. Con todo, si es un administrador en el equipo, puede instalar nuevos paquetes mediante R, siempre que instale en la biblioteca correcta.

> [!NOTE]
> En el servidor, **no** instale en una biblioteca de usuario, aunque se le pida. Si lo hace, la instancia de SQL Server no podrá encontrar ni ejecutar los paquetes. Para más información, vea [Instalación de paquetes de R nuevos en SQL Server](../package-management/install-additional-r-packages-on-sql-server.md).

1. En el equipo con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], abra RGui.exe **como administrador**.  Si ha instalado SQL Server R Services usando los valores predeterminados, Rgui.exe estará en C:\Archivos de programa\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES\bin\x64).

2. En un símbolo del sistema de R, ejecute los siguientes comandos de R:
  
  ```R
  install.packages("ggmap", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("mapproj", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("ROCR", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("RODBC", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  ```
  En este ejemplo se usa la función grep de R para buscar en el vector de rutas de acceso disponibles y encontrar la ruta de acceso que incluya "Archivos de programa". Para más información, vea [https://www.rdocumentation.org/packages/base/functions/grep](https://www.rdocumentation.org/packages/base/functions/grep).

  Si cree que los paquetes ya están instalados, ejecute `installed.packages()` para revisar la lista de paquetes instalados.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración y resumen de los datos](walkthrough-view-and-summarize-data-using-r.md)