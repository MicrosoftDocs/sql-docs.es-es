---
title: Extensión dacpac de SQL Server
description: Aprenda a instalar e iniciar el Asistente para aplicaciones de capa de datos, que facilita la implementación y extracción de archivos dacpac y la importación y exportación de archivos bacpac.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: e0cfe057767488eb0a7ddb741934a10410898ffa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040265"
---
# <a name="sql-server-dacpac-extension"></a>Extensión dacpac de SQL Server

El **Asistente para aplicaciones de capa de datos** proporciona una experiencia de asistente fácil de usar al implementar y extraer archivos dacpac, así como importar y exportar archivos bacpac.

## <a name="features"></a>Características

* Implementación de un archivo dacpac en una instancia de SQL Server
* Extracción de una instancia de SQL Server en un archivo dacpac
* Creación de una base de datos a partir de un archivo bacpac
* Exportación del esquema y los datos a un archivo bacpac

![GIF de demostración de la extensión del archivo dacpac](media/sql-server-dacpac-extension/dacpac-extension-demo.gif)

## <a name="why-would-i-use-the-data-tier-application-wizard"></a>¿Por qué es tan útil Asistente para aplicaciones de capa de datos?

El asistente facilita la administración de archivos dacpac y bacpac, lo que simplifica el desarrollo y la implementación de los elementos de capa de datos que admiten la aplicación. Para obtener más información sobre el uso de las aplicaciones de capa de datos, [vea nuestra documentación.](../../relational-databases/data-tier-applications/data-tier-applications.md)

## <a name="install-the-extension"></a>Instalación de la extensión

1. Seleccione el icono de extensiones para ver las extensiones disponibles.

    ![Icono del Administrador de extensiones](media/add-extensions/extension-manager-icon.png)

2. Busque la extensión **SQL Server dacpac** y selecciónela para ver los detalles. Seleccione **Instalar** para agregar la extensión.

3. Una vez instalada, seleccione **Recargar** para habilitar la extensión en Azure Data Studio (solo es necesario cuando se instala una extensión por primera vez).

## <a name="launch-the-data-tier-application-wizard"></a>Inicio del asistente de aplicaciones de capa de datos

Para iniciar el asistente, haga clic con el botón secundario en la carpeta Databases o haga clic con el botón secundario en una base de datos específica en el Explorador de objetos. A continuación, seleccione **Asistente para aplicación de capa de datos**.

![menú de inicio de la extensión dacpac](media/sql-server-dacpac-extension/dacpac-extension-launch.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre dacpac, [vea nuestra documentación](../../relational-databases/data-tier-applications/data-tier-applications.md).
Puede notificar cualquier problema y solicitar nuevas características [aquí](https://github.com/microsoft/azuredatastudio/issues).