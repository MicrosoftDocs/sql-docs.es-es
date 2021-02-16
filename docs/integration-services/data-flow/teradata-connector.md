---
description: Conector de Microsoft para Teradata
title: Microsoft Connector para Teradata| Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5f82833ec5498c107155a5284d7a419ba6289239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346540"
---
# <a name="microsoft-connector-for-teradata"></a>Conector de Microsoft para Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Microsoft Connector para Teradata permite exportar datos desde bases de datos de Teradata y cargar datos en ellas en un paquete SSIS.

Este nuevo conector es compatible con las bases de datos con tablas habilitadas para 1 MB.

## <a name="version-support"></a>Compatibilidad con versiones

Microsoft Connector para Teradata admite los siguientes productos de Microsoft SQL Server:

- Microsoft SQL Server 2019
- Microsoft SQL Server Data Tools (SSDT) 15.8.1 o posterior para Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) para Visual Studio 2019

Microsoft Connector para Teradata usa la interfaz del lenguaje de programación de aplicaciones Teradata Parallel Transporter para cargar datos en la base de datos de Teradata y exportarlos desde allí. Las versiones compatibles son las siguientes:

- Teradata Parallel Transporter (Teradata PT) 16.10
- Teradata Parallel Transporter (Teradata PT) 16.20

Se admiten las siguientes versiones de la base de datos de Teradata del origen de datos:

- Base de datos de Teradata 16.20
- Base de datos de Teradata 16.10
- Base de datos de Teradata 15.10
- Base de datos de Teradata 15.00

Consulte la [documentación de Teradata](https://docs.teradata.com/) para obtener más información sobre la guía para programadores de la interfaz de programación de Teradata Parallel Transporter.

## <a name="installation"></a>Instalación

En equipos de 32 bits, instale los siguientes controladores desde [Herramientas y utilidades de Teradata: paquete de instalación de Windows](https://downloads.teradata.com/download/tools/teradata-tools-and-utilities-windows-installation-package):

- Controlador ODBC de Teradata (32 bits)
- API PT de Teradata (32 bits)

En equipos de 64 bits, instale los siguientes controladores:

- Controlador ODBC de Teradata (64 bits)
- API PT de Teradata (64 bits)

Para instalar el conector para la base de datos de Teradata, descargue y ejecute el instalador desde [la versión más reciente de Microsoft Connector para Teradata](https://www.microsoft.com/download/details.aspx?id=100599). A continuación, siga las instrucciones del asistente para la instalación.

Después de instalar el conector, debe reiniciar el Servicio de integración de SQL Server para asegurarse de que el origen y el destino de Teradata funcionen correctamente.

## <a name="design-and-execute-ssis-packages"></a>Diseño y ejecución de paquetes de SSIS

Microsoft Connector para Teradata proporciona una experiencia de usuario similar a la del conector para Teradata de Attunity. El usuario puede diseñar nuevos paquetes basados en la experiencia anterior, utilizando SSDT para VS 2017 o VS 2019, *que tengan como destino SQL Server 2019*.

El origen y el destino de Teradata están en la categoría común.

![El componente Teradata](media/teradata-component.png)

El administrador de conexiones de Teradata se muestra como "TERADATA".

![El tipo de administrador de conexiones de Teradata](media/teradata-connection-manager-type.png)

Los paquetes SSIS existentes que se han diseñado con el conector de Teradata de Attunity se actualizarán automáticamente para usar el conector de Microsoft para Teradata. También se cambiarán los iconos.

Para ejecutar un paquete SSIS *que tenga como destino SQL Server 2017 y versiones anteriores*, sí debe instalar **Microsoft Connector para Teradata de Attunity** con la versión correspondiente, que encontrará en el vínculo siguiente:

- [SQL Server 2017: Microsoft Connector Versión 5.0 para Teradata de Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016: Microsoft Connector Versión 4.0 para Teradata de Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014: Microsoft Connector Versión 3.0 para Teradata de Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012: Microsoft Connector Versión 2.0 para Teradata de Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

Para diseñar un paquete SSIS en SSDT *que tenga como destino SQL Server 2017 y versiones anteriores*, deberá tener **Microsoft Connector para Teradata** e instalar **Microsoft Connector para Teradata de Attunity** con la versión correspondiente.

## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

- En el editor de origen/destino de Teradata, la propiedad **Base de datos predeterminada** no surte efecto.  Como solución alternativa, escriba el nombre de la base de datos en el cuadro desplegable para filtrar la tabla o vista.

- En el editor de origen/destino de Teradata, el paso de asignación no funciona al escribir \<database>.<table/view>. Como solución alternativa, escriba \<database>.<table/view> y, después, haga clic en el botón desplegable.

- En el editor de origen de Teradata, no se puede mostrar la vista cuando el modo de acceso a datos es "Table Name – TPT Export". Como solución alternativa, use el editor avanzado de origen de Teradata.

- En el destino de Teradata, el atributo 'PackMaximum' no se puede establecer en 'True'.  De lo contrario, se producirá un error.

## <a name="uninstallation"></a>Desinstalación

Puede ejecutar el Asistente para desinstalación para quitar **Microsoft Connector para Teradata**.

## <a name="next-steps"></a>Pasos siguientes

- Configurar el [administrador de conexiones de Teradata](teradata-connection-manager.md)
- Configurar el [origen de Teradata](teradata-source.md)
- Configurar el [destino de Teradata](teradata-destination.md)
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA6iwdw).
