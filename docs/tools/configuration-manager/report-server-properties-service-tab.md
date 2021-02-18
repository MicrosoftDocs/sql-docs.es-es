---
title: Propiedades de Servidor de informes (pestaña Servicio)
description: Obtenga información sobre las opciones de la pestaña Servicio del cuadro de diálogo Propiedades del servidor de informes, como la ruta de acceso binaria, el nombre de host y el modo de inicio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2a2e1dbf-02b9-4893-aaf0-c0e4a2c9b4f9
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 93abf1f719ca4e9d541a4c2381e7177f526b6828
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349662"
---
# <a name="report-server-properties-service-tab"></a>Propiedades de Servidor de informes (pestaña Servicio)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  este es el servicio Servidor de informes de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los valores de propiedades en color gris claro no se pueden modificar con esta aplicación.  
  
## <a name="options"></a>Opciones  
 **Ruta de acceso binaria**  
 Muestra la ubicación de los archivos de programa utilizados por el servicio.  
  
 **Control de error**  
 1 indica "SERVICE_ERROR_NORMAL". Si el servicio no se inicia al iniciar el equipo, el programa de inicio registra el error y muestra un cuadro de mensaje emergente pero continúa con la operación de inicio. Este valor no puede modificarse.  
  
 **Código de salida**  
 Si se produce un error, el número de error aparece en este cuadro. Utilice este número para solucionar errores buscando el número en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base o informe del mismo al personal de soporte técnico.  
  
 **Host Name**  
 Muestra el nombre del equipo o clúster que ejecuta la búsqueda de texto completo.  
  
 **Nombre**  
 Indica el nombre para mostrar del servicio.  
  
 **Id. del proceso**  
 Muestra el Id. de proceso de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Tipo de servicio de SQL**  
 Tipo de servicio proporcionado a los procesos de llamada. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instala varios servicios.  
  
 **Modo de inicio**  
 Para este servicio se pueden configurar las siguientes opciones:  
  
-   Manual: este servicio no se inicia automáticamente cuando se inicia el equipo. Debe iniciarlo mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] u otra herramienta.  
  
-   Automático: este servicio intenta iniciarse cuando se inicia el equipo.  
  
-   Deshabilitado: no se puede iniciar el servicio.  
  
 **State**  
 Indica si el servicio está en ejecución, detenido o deshabilitado.  
  
## <a name="see-also"></a>Consulte también  
 [Servicios de SQL Server](../../tools/configuration-manager/sql-server-services.md)  
  
  
