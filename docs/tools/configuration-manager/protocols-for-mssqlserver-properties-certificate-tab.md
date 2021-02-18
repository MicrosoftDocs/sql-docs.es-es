---
title: Propiedades de Protocolos de MSSQLSERVER (pestaña Certificado)
description: Use la pestaña Certificado del cuadro de diálogo Propiedades de Protocolos de MSSQLSERVER para seleccionar un certificado para SQL Server o para ver las propiedades de un certificado.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.computermgr.cert.general.f1
helpviewer_keywords:
- MSSQLSERVER property protocols
ms.assetid: 776addd6-25f3-4875-9a71-064035787090
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: de4ad06ad4cca2dcc346b82c4e4fb6e66337106e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349705"
---
# <a name="protocols-for-mssqlserver-properties-certificate-tab"></a>Propiedades de Protocolos de MSSQLSERVER (pestaña Certificado)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilice la pestaña **Certificado** del cuadro de diálogo **Propiedades de Protocolos de MSSQLSERVER** para seleccionar un certificado para [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o ver las propiedades de un certificado. Todos los campos están en blanco hasta que se selecciona un certificado.  
  
 Los certificados se almacenan localmente para los usuarios del equipo. Para cargar un certificado que se va a utilizar en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe ejecutar el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la misma cuenta de usuario que el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="page-header"></a>Encabezado de página  
 **Vista**  
 Proporciona acceso a detalles adicionales acerca del certificado. No está disponible hasta que se selecciona un certificado en el cuadro **Certificado** . Para obtener información adicional acerca de los detalles del certificado, vea la documentación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Borrar**  
 Quita la selección del cuadro **Certificado** .  
  
 **Certificate**  
 Nombre del certificado determinado por el proveedor de seguridad. Seleccione un certificado para ver los detalles en la cuadrícula de propiedades.  
  
## <a name="options"></a>Opciones  
 Fecha de expiración  
 Fecha final del período de validez del certificado.  
  
 Nombre descriptivo  
 Nombre descriptivo o común para la persona o entidad de certificación a la que se emite el certificado.  
  
 Emitido por  
 Información relativa a la entidad de certificación que ha emitido el certificado.  
  
 Emitido a  
 Información relativa al destinatario del certificado.  
  
  
