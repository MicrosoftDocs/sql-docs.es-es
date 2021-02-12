---
title: Servicio Servidor de informes de Windows (MSSQLServer) 107 | Microsoft Docs
description: 'En esta página de referencia de error, obtenga información sobre el identificador de evento 107: El servicio Servidor de informes de Windows (SQL Server) no se puede conectar a la base de datos del servidor de informes.'
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- MSSQLServer 107 error
ms.assetid: 52b5704b-27f9-400a-a821-d8fa0786afe4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: aebf8df47e1cda77dcbbce89bd1e74a439082281
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045095"
---
# <a name="report-server-windows-service-mssqlserver-107"></a>Servicio Servidor de informes de Windows (MSSQLServer) 107
    
## <a name="details"></a>Detalles  
  
|Category|Value|  
|-|-|  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|107|  
|Origen de eventos|Servicio Servidor de informes de Windows|  
|Componente|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Texto del mensaje|El servicio Servidor de informes de Windows (MSSQLSERVER) no se puede conectar a la base de datos del servidor de informes.|  
  
## <a name="explanation"></a>Explicación  
 El servicio del Servidor de informes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede establecer conexión con la base de datos del servidor de informes. Este error se produce durante una reiniciación del servicio si no se puede establecer una conexión con la base de datos del servidor de informes. Entre las condiciones en que se produce este error se incluyen las siguientes:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] no se está ejecutando cuando se inicia el servicio del Servidor de informes.  
  
-   La conexión con el servicio de [!INCLUDE[ssDE](../../includes/ssde-md.md)] sufre un error porque no están habilitadas las conexiones remotas o el protocolo TCP/IP.  
  
-   La base de datos del servidor de informes no está configurada correctamente.  
  
-   La cuenta del servicio no está configurada correctamente o dicha cuenta ya no tiene permisos para la base de datos del servidor de informes. Esto puede producirse si no se utiliza la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para configurar la cuenta o la base de datos del servidor de informes.  
  
## <a name="user-action"></a>Acción del usuario  
 Inicie el servicio de [!INCLUDE[ssDE](../../includes/ssde-md.md)] si no se está ejecutando y compruebe que las conexiones remotas están habilitadas para el protocolo TCP/IP.  
  
 Use la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para configurar la cuenta del servicio y la base de datos del servidor de informes.  
  
## <a name="internal-only"></a>Solo para uso interno  
  
## <a name="see-also"></a>Consulte también  

 [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   

 [Inicio y detención del servicio del servidor de informes](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)  
  
  
