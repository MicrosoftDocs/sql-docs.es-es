---
title: Propiedades de Analysis Server (pestaña Iniciar sesión)
description: Obtenga información sobre la pestaña Iniciar sesión del cuadro de diálogo Propiedades de Analysis Server. Vea cómo se usa para especificar la cuenta de servicio de SSAS e iniciar o detener el servicio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: a82e0c98-efaa-4b0b-9582-3c879ee42444
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6429ac0bee9725ed3a404847e7f1080f785c4919
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463916"
---
# <a name="analysis-server-properties-log-on-tab"></a>Propiedades de Analysis Server (pestaña Iniciar sesión)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilice la pestaña **Iniciar sesión** del cuadro de diálogo **Propiedades de Analysis Server** para especificar la cuenta que utiliza el servicio [!INCLUDE[ssAS](../../includes/ssas-md.md)] así como para iniciar y detener el servicio.  
  
> [!NOTE]  
>  Cuando cambie el **Nombre de cuenta** utilizado por un servicio en una instancia en clúster, la nueva cuenta debe formar parte del grupo de dominios que haya especificado durante la instalación del servicio modificado, o bien debe tener permiso para agregar miembros a ese grupo. Si no tiene permisos para modificar la pertenencia al grupo, póngase en contacto con el administrador del dominio.  
  
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
  
  
