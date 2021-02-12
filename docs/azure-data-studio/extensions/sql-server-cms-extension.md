---
title: Extensión Servidores de administración central de SQL Server
description: Obtenga información sobre cómo instalar y usar la extensión de Servidores de administración central de SQL Server. Una extensión para agrupar servidores y aplicar acciones al grupo.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 06/06/2019
ms.openlocfilehash: 0d93943619d0825e5ba01b51a9c413950fffde37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048397"
---
# <a name="sql-server-central-management-servers-extension-preview"></a>Extensión de Servidores de administración central de SQL Server (versión preliminar)

La extensión Servidores de administración central permite a los usuarios almacenar una lista de instancias de SQL Server organizadas en uno o más grupos. Las acciones que se llevan a cabo mediante un servidor de administración central (CMS) actúan en todos los servidores del grupo.

Esta experiencia está actualmente en su versión preliminar inicial. Puede notificar cualquier problema y solicitar nuevas características [aquí](https://github.com/microsoft/azuredatastudio/issues).

![Extensión CMS](media/sql-server-cms-extension/cms-list.png)

## <a name="install-the-sql-server-central-management-servers-extension"></a>Instalación de la extensión Servidores de administración central de SQL Server

1. Para abrir el administrador de extensiones y acceder a las extensiones disponibles, seleccione el icono de extensiones, o bien seleccione **Extensiones** en el menú **Ver**.
2. Seleccione una extensión disponible para ver sus detalles.
3. Seleccione la extensión que desee (Servidores de administración central de SQL Server) y seleccione **Instalar**.

### <a name="how-do-i-start-central-management-servers"></a>¿Cómo se inicia Servidores de administración central?

 Para ver Servidores de administración central, haga clic en el icono Conexiones (Ctrl/CMD + G). La primera vez que se descarga la extensión, la vista de CMS aparece minimizada; para abrirla, haga clic en **Servidores de administración central**.

## <a name="next-steps"></a>Pasos siguientes

[Aquí puede leer más información](../../ssms/register-servers/create-a-central-management-server-and-server-group.md) sobre los conceptos de Servidores de administración central.