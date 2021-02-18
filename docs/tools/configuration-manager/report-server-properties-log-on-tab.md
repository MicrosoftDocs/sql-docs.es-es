---
title: Propiedades de Servidor de informes (pestaña Iniciar sesión)
description: Obtenga información sobre la pestaña Iniciar sesión del cuadro de diálogo Propiedades del servidor de informes en SQL Server. Vea cómo se usa para especificar una cuenta e iniciar o detener el servicio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: f54be594-f290-4db2-bf18-fd2521728a4a
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 04ce5ac7bb7dcb820cf32fac89aa207a3722bae7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349721"
---
# <a name="report-server-properties-log-on-tab"></a>Propiedades de Servidor de informes (pestaña Iniciar sesión)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilice la pestaña **Iniciar sesión** del cuadro de diálogo **Propiedades de Servidor de informes** para especificar la cuenta que utiliza el servicio Servidor de informes así como para iniciar y detener el servicio.  
  
## <a name="options"></a>Opciones  
 **Cuenta de sistema local**  
 Especifique una cuenta de sistema local, que no requiere una contraseña. No obstante, la cuenta del sistema local puede restringir la interacción del servicio con otros servidores, en función de los privilegios que se le hayan concedido.  
  
 **Esta cuenta**  
 Especifique una cuenta de usuario local o de dominio que utilice la autenticación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda utilizar una cuenta de usuario de dominio que tenga derechos mínimos para los servicios. Para obtener información acerca de cómo seleccionar una cuenta, busque el tema sobre cómo configurar cuentas de servicios de Windows en los Libros en pantalla.  
  
 **Nombre de cuenta**  
 Escriba el nombre de la cuenta de usuario local o de dominio.  
  
 **Contraseña**  
 Escriba la contraseña de la cuenta.  
  
 **Confirmar contraseña**  
 Escriba de nuevo la contraseña de la cuenta.  
  
 **Iniciar**  
 Inicie el servicio.  
  
 **Detención**  
 Detenga el servicio.  
  
 **Pausar**  
 Pause el servicio.  
  
 **Reanudar**  
 Reanude un servicio en pausa.  
  
  
