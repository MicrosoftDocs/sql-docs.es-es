---
title: Selector del demonio de filtro de texto completo de SQL (pestaña Iniciar sesión)
description: Obtenga información sobre el Selector de demonio de filtro de texto completo de SQL, que se usa en la búsqueda de texto completo de SQL Server. Obtenga información sobre la pestaña Iniciar sesión del cuadro de diálogo Propiedades.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 13e260f9-a75f-430b-88a3-959ddcead8fe
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 08224bf7ed6ec8a6ba2c5b4ccd8cc88bea4bd22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346038"
---
# <a name="sql-full-text-filter-daemon-launcher-log-on-tab"></a>Selector del demonio de filtro de texto completo de SQL (pestaña Iniciar sesión)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], la búsqueda de texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza el servicio Selector de demonio de filtro de texto completo de SQL (iniciador FDHOST). Este servicio debe estar ejecutándose si se utiliza la búsqueda de texto completo. Para obtener información sobre los procesos de host de demonio de filtro, vea "Arquitectura de la búsqueda de texto completo" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Utilice la pestaña **Iniciar sesión** del cuadro de diálogo **Propiedades de Selector del demonio de filtro de texto completo de SQL** para especificar la cuenta que utiliza el servicio de búsqueda en texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cambiar la contraseña de una cuenta e iniciar y detener el servicio. El cambio de la contraseña de una cuenta surte efecto después de reiniciar el servicio.  
  
> [!NOTE]  
>  Cuando cambie el nombre de cuenta utilizado por un servicio en una instancia en clúster, la nueva cuenta debe formar parte del grupo de dominios que haya especificado durante la instalación del servicio, o bien debe tener permiso para agregar miembros a ese grupo. Si no tiene permisos para modificar la pertenencia al grupo, póngase en contacto con el administrador del dominio.  
>   
>  Para obtener información acerca de cómo seleccionar una cuenta para ejecutar el servicio, vea el tema sobre la configuración de cuentas de servicios de Windows en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Opciones  
 **Cuenta integrada**  
 **Sistema local**  
 Especifique la cuenta de sistema local. Esta cuenta no requiere ninguna contraseña. No obstante, la cuenta del sistema local puede impedir la interacción del servicio con otros servidores, en función de los privilegios que se hayan concedido a la cuenta.  
  
 **Servicio local**  
 Especifique la cuenta de servicio local. Esta cuenta no requiere ninguna contraseña. No obstante, la cuenta de servicio local puede impedir la interacción del servicio con otros servidores, en función de los privilegios que se hayan concedido a la cuenta.  
  
 **Servicio de red**  
 Se recomienda no utilizar la cuenta de servicio de red para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Las cuentas de usuario local o de usuario de dominio son más adecuadas para estos servicios.  
  
 **Esta cuenta**  
 Especifique una cuenta de usuario local o de dominio que utilice la autenticación de Windows. Se recomienda utilizar una cuenta de usuario de dominio que tenga derechos mínimos para los servicios.  
  
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
 Pause el servicio. No disponible para este servicio.  
  
 **Reanudar**  
 Reanude un servicio en pausa.  
  
  
