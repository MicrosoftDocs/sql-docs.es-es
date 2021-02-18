---
title: " Página de opciones de SQL Server - Entorno - Configuración"
description: 'Opciones (entorno: página Configuración)'
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.date: 11/05/2018
ms.openlocfilehash: a409fc3687adfe00bdbdd091708dbb7b672506a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341075"
---
# <a name="options-environment---startup-page"></a>Opciones (entorno: página Configuración)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Use el cuadro de diálogo **Opciones** para configurar las acciones de inicio de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], opciones generales de administración de ventanas y otros valores de configuración generales. En el menú **Herramientas**, haga clic en **Opciones**, expanda la carpeta **Entorno** y haga clic en **Configuración**.

## <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario

**Al iniciar**

Seleccione la acción que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] debe llevar a cabo una vez que se ha iniciado. Las opciones son:

- **Abrir el Explorador de objetos** : solicita una conexión y abre el Explorador de objetos.

- **Abrir nueva ventana de consulta** : solicita una conexión y abre el Editor de consultas de SQL.

- **Abrir el Explorador de objetos y ventana de consulta**: solicita una conexión y abre el Explorador de objetos y el Editor de consultas de SQL con dicha conexión.

- **Abrir el Explorador de objetos y el Monitor de actividad**

- **Abrir entorno vacío** : abre [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sin ninguna ventana del Editor de consultas de SQL y sin conectar el Explorador de objetos a un servidor.

**Ocultar objetos del sistema en el Explorador de objetos**

Seleccione esta casilla para eliminar de la vista de árbol del Explorador de objetos las bases de datos del sistema, las tablas del sistema, las vistas del sistema y los procedimientos almacenados del sistema. Las funciones del sistema y los tipos de datos del sistema no se ocultan. Esta opción solo se aplica a instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no afecta a los servidores que ya están conectados en el Explorador de objetos.

## <a name="see-also"></a>Consulte también

- [Opciones (Ayuda F1 de cuadros de diálogos)](options-dialog-boxes-f1-help.md)
- [Opciones (entorno: página General)](options-environment-general-page.md)
