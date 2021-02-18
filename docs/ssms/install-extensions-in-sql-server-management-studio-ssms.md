---
description: Instalación de extensiones en SQL Server Management Studio (SSMS)
title: Instalación de extensiones en SQL Server Management Studio (SSMS)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- extensions
- vsix
- instalación de la extensión
- install vsix
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 07/29/2020
ms.openlocfilehash: d9fe47b283eb95ed624b68bc9c22abd9258bad15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344716"
---
# <a name="install-extensions-in-sql-server-management-studio-ssms"></a>Instalación de extensiones en SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Las extensiones de SQL Server Management Studio (SSMS) se crean con C# mediante la carga de trabajo "Desarrollo de extensiones de Visual Studio" en Visual Studio. SSMS 18.x se basa en el shell de Visual Studio 2017 y está sujeto a las limitaciones de ese entorno.

La instalación de extensiones en SSMS 18.x se puede lograr mediante la implementación de VSIX mediante Visual Studio o un instalador de paquetes administrado independiente.  A continuación, se describe la implementación de Visual Studio.

> [!NOTE]
> No se pueden instalar las extensiones de SQL Server Management Studio mediante VSIXInstaller en SSMS 18.x.
  
## <a name="visual-studio-deployment-of-an-extension-for-ssms-18x"></a>Implementación de Visual Studio de una extensión para SSMS 18.x

La instalación manual de la extensión se realiza copiando los archivos de extensión asociados (VSIX) en la carpeta predeterminada de extensiones de SSMS.  Al inicio, SSMS comprueba automáticamente si hay extensiones en esta carpeta.  Visual Studio puede completar la implementación de VSIX en el momento de compilarse el proyecto. 

  
1.  Busque la carpeta de instalación y de extensiones predeterminadas de SSMS.  Con la configuración predeterminada de instalación de SSMS, la ubicación de la carpeta es ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\```.  


2. Inicie Visual Studio como administrador.

3.  Visual Studio puede completar el proceso de copia de archivos en tiempo de compilación activando la casilla "Copiar el contenido de VSIX en la siguiente ubicación" en la pestaña VSIX de la ventana de propiedades del proyecto. En el cuadro de texto situado debajo de la casilla, especifique la ubicación de carpeta anterior con una carpeta para esta extensión anexada.  Por ejemplo: ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\SampleExtension```
  
![Configuración de VSIX de la ventana de propiedades del proyecto con tres casillas y un cuadro de texto](./media/install-extensions/vsix_ssms.png)

4. Compile el proyecto de extensión; una compilación correcta transferirá los archivos de extensión a la carpeta de extensiones de SSMS.

5.  Inicie SSMS y pruebe la funcionalidad de la extensión.
  
