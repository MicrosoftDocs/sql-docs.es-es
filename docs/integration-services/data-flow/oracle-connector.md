---
description: Microsoft Connector for Oracle
title: Microsoft Connector for Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 70acf129fa608c1058e808d2193cf4c901198d6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343218"
---
# <a name="microsoft-connector-for-oracle"></a>Microsoft Connector for Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Microsoft Connector for Oracle permite la capacidad de exportar datos desde un origen de datos de Oracle y cargar datos en él en un paquete SSIS.

## <a name="version-support"></a>Compatibilidad con versiones

Microsoft Connector for Oracle admite los siguientes productos de Microsoft SQL Server:

- A partir de SQL Server 2019 CU1
- SQL Server Data Tools (SSDT) 15.9.3 o posterior para Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) para Visual Studio 2019

Se admiten las siguientes versiones de la base de datos de Oracle del origen de datos:

- Oracle 10.x
- Oracle 11.x
- Oracle 12c
- Oracle 18c (sin compatibilidad con la autenticación de Windows)
- Oracle 19c (sin compatibilidad con la autenticación de Windows)

La base de datos de Oracle es compatible con todos los sistemas operativos y plataformas.
> [!NOTE]
>
> Microsoft Connector for Oracle Database no necesita el cliente de Oracle en SQL Server 2019.

## <a name="installation"></a>Instalación

Para instalar el conector para la base de datos de Oracle, descargue y ejecute el instalador a partir de la [última versión del conector de Microsoft para Oracle](https://www.microsoft.com/download/details.aspx?id=58228). A continuación, siga las instrucciones del asistente para la instalación.

Después de instalar el conector, debe reiniciar el Servicio de integración de SQL Server para asegurarse de que el origen y el destino de Oracle puedan funcionar correctamente.

Para ejecutar un paquete SSIS que tenga como destino SQL Server 2017 y versiones anteriores, además de **Microsoft Connector for Oracle**, tendrá que instalar el **cliente de Oracle** y **Microsoft Connector for Oracle de Attunity** con la versión correspondiente, disponibles en los vínculos siguientes:

- [SQL Server 2017: Microsoft Connector Version 5.0 for Oracle de Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016: Microsoft Connector Version 4.0 for Oracle de Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014: Microsoft Connector Version 3.0 for Oracle de Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012: Microsoft Connector Version 2.0 for Oracle de Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

- Las vistas no se muestran en el origen de Oracle *Nombre de la tabla o la vista*. Como solución alternativa, use el comando SQL y seleccione * en la vista, o bien establezca el nombre de la vista en la propiedad [Oracle Source].[TableName] en el Editor avanzado.

## <a name="uninstallation"></a>Desinstalación

Puede ejecutar el Asistente para desinstalación para quitar Microsoft Connector for Oracle Database de SQL Server.

## <a name="next-steps"></a>Pasos siguientes

- Configure el [Administrador de conexiones de Oracle](oracle-connection-manager.md).
- Configure el [origen de Oracle](oracle-source.md).
- Configure el [destino de Oracle](oracle-destination.md).
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA5u35j).
