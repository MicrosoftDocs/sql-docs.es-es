---
title: Cambiar las zonas horarias y la configuración del reloj en un servidor de informes | Microsoft Docs
description: Cambie las zonas horarias y la configuración del reloj para un servidor de informes. No se puede establecer la zona horaria de un servidor de informes, por lo que se establece la del equipo o la configuración de la región de SharePoint
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- time zones [Reporting Services]
- clocks [Reporting Services]
- schedules [Reporting Services], clock settings
- schedules [Reporting Services], time zones
ms.assetid: 69a19468-baa1-40f6-b158-8afdab0f8968
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 02b64deba925bdf355fa72746e5b0236c2cf14b5
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987331"
---
# <a name="change-time-zones-and-clock-settings-on-a-report-server"></a>Cambiar las zonas horarias y la configuración del reloj en un servidor de informes
  Un servidor de informes siempre utiliza la hora local del equipo donde esté instalado. No puede configurarlo para que use una zona horaria diferente. Si una aplicación cliente señala a un servidor de informes en una zona horaria diferente, para ejecutar una operación programada se utilizará la zona horaria del servidor de informes. En el Administrador de informes y en las páginas de administración de SharePoint, la zona horaria se indica en cada página de programación para que sepa exactamente cuándo se realizará una operación programada. Por ejemplo, la página para crear programaciones personalizadas indicará "Las horas están expresadas en (UTC-08:00) Hora del Pacífico (EE. UU. y Canadá)".
El servidor de informes crea un trabajo del Agente SQL Server que se usa para desencadenar la programación. Cuando el servidor de informes y Agente SQL Server se encuentran en servidores independientes, la zona horaria debe ser la misma en todos los servidores.
  
## <a name="changing-the-time-zone-native-mode"></a>Cambiar la zona horaria (modo nativo)  
 Si cambia la zona horaria en el equipo que hospeda un servidor de informes, deberá reiniciar el servicio del servidor de informes para que el cambio de zona horaria surta efecto.  
  
 Los valores de marca de tiempo de instantáneas del historial de informes existente están sincronizados al nuevo ajuste de zona horaria. Si ha generado una instantánea del historial de informes a las 9:00 a.m. y después avanza una zona horaria, la marca de tiempo de la instantánea generada cambiará de 9:00 a.m. a 10:00 a.m. a 10:00 a.m.  
  
 Las programaciones conservan la configuración existente, excepto cuando se asignan a la nueva zona horaria. Por ejemplo, si una programación se ejecuta a las 2:00 a.m., a la hora estándar del Pacífico, y se cambia la zona horaria a la hora estándar del este de Australia, la programación se ejecuta a las 2:00 a.m., a la hora estándar del este de Australia.  
  
 Los valores de marca de tiempo de propiedades (por ejemplo, la hora a la que se creó una carpeta o un elemento de informe vinculado) no se sincronizan con la nueva configuración horaria. Si crea un elemento el 25 de junio a las 9:00 a.m. y después restablece la zona horaria o el reloj, la marca de tiempo sigue siendo 25 de junio a las 9:00 a.m.  
  
## <a name="changing-the-time-zone-sharepoint-mode"></a>Cambiar la zona horaria (modo de SharePoint)  
 La configuración de zona horaria para el modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se administra como parte de la configuración regional de SharePoint. Para más información, vea [Configuración regional (SharePoint Server 2010 (/previous-versions/office/sharepoint-server-2010/cc824907(v=office.14))](/previous-versions/office/sharepoint-server-2010/cc824907(v=office.14)).  
  
## <a name="changing-the-clock-settings"></a>Cambiar la configuración del reloj  
 El cambio del reloj del equipo no afecta a los valores de marca de tiempo existentes (por ejemplo, si adelanta el reloj una hora, las marcas de tiempo de las instantáneas de historial del informe no cambian). Es posible que se produzca una demora de 10 segundos antes de que el Procesador de entrega y programación utilice la nueva configuración. La demora real puede variar si ha modificado la configuración de intervalo de sondeo en los archivos de configuración.  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar y detener el servicio del servidor de informes](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
 [Programaciones](../../reporting-services/subscriptions/schedules.md)  
  
