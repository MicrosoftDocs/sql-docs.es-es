---
title: Selector del demonio de filtro de texto completo de SQL (pestaña Servicio)
description: Obtenga información sobre el Selector de demonio de filtro de texto completo de SQL, que se usa en la búsqueda de texto completo de SQL Server. Obtenga información sobre la pestaña Servicio del cuadro de diálogo Propiedades.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 6aad7ebe-c4be-4d37-8536-61502f51faa2
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f357bfeed8f5ce9a9cf7583f973fe506df04b3fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340595"
---
# <a name="sql-full-text-filter-daemon-launcher-service-tab"></a>Selector del demonio de filtro de texto completo de SQL (pestaña Servicio)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  A partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], el Selector de demonio de filtro de texto completo de SQL (selector FDHOST) se usa en el texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este servicio debe estar ejecutándose si se utiliza la búsqueda de texto completo. Para obtener información sobre los procesos de host de demonio de filtro, vea "Arquitectura de la búsqueda de texto completo" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Use la pestaña **Servicio** del cuadro de diálogo **Propiedades de Selector de demonio de filtro de texto completo de SQL** para ver o especificar las opciones siguientes.  
  
## <a name="options"></a>Opciones  
 **Ruta de acceso binaria**  
 Muestra la ubicación de los archivos de programa utilizados por el servicio.  
  
 **Control de error**  
 1 indica `SERVICE_ERROR_NORMAL`. Si el servicio no se inicia al iniciar el equipo, el programa de inicio registra el error y muestra un cuadro de mensaje emergente pero continúa con la operación de inicio. Este valor no puede modificarse.  
  
 **Código de salida**  
 Si se produce un error, el número de error aparece en este cuadro. Utilice este número para solucionar errores buscando el número en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base o informe del mismo al personal de soporte técnico.  
  
 **Host Name**  
 Muestra el nombre del equipo o clúster que ejecuta el servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Nombre**  
 Indica el nombre para mostrar del servicio.  
  
 **Id. del proceso**  
 Muestra el identificador de proceso de Windows.  
  
 **Tipo de servicio de SQL**  
 Muestra el tipo de servicio proporcionado a los procesos de llamada. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instala varios servicios.  
  
 **Modo de inicio**  
 Para este servicio se pueden configurar las siguientes opciones:  
  
-   Manual: este servicio no se inicia automáticamente cuando se inicia el equipo. Debe iniciarlo mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] u otra herramienta.  
  
-   Automático: este servicio intenta iniciarse cuando se inicia el equipo.  
  
-   Deshabilitado: no se puede iniciar el servicio.  
  
 **State**  
 Indica si el servicio está en ejecución, detenido o deshabilitado. " **…** " indica que hay un cambio de estado pendiente.  
  
  
