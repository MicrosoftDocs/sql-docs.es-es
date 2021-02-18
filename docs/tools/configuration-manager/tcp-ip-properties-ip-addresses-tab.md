---
title: Propiedades de TCP/IP (pestaña Direcciones IP)
description: Aprenda a usar la pestaña Direcciones IP del cuadro de diálogo Propiedades de TCP/IP para configurar las opciones del protocolo TCP/IP para una dirección IP específica en SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], listening on
- listening [SQL Server], on ports
ms.assetid: 4c17ed45-9da7-4bec-bce6-970109fe7365
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 56055b7d3537413398980579342884ab6a58c142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345085"
---
# <a name="tcpip-properties-ip-addresses-tab"></a>Propiedades de TCP/IP (pestaña Direcciones IP)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Use el cuadro de diálogo **Propiedades de TCP/IP (pestaña Direcciones IP)** para configurar las opciones del protocolo TCP/IP para una dirección IP específica. Solo las opciones **Puertos dinámicos TCP** y **Puerto TCP** pueden configurarse para todas las direcciones al mismo tiempo seleccionando **IPAll**.  
  
 Los cambios surten efecto cuando se reinicia SQL Server SQL Server. Para información sobre cómo iniciar y detener el servicio SQL Server Browser, consulte [Iniciar y detener el servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
## <a name="static-vs-dynamic-ports"></a>Puertos estáticos frente a Puertos dinámicos  
 La instancia predeterminada de SQL Server realiza escuchas para las conexiones entrantes en el puerto 1433. Se puede cambiar el puerto por razones de seguridad o debido a un requisito de la aplicación cliente. De forma predeterminada, las instancias con nombre (incluido SQL Server Express) se configuran para escuchar en los puertos dinámicos. Para configurar un puerto estático, deje en blanco la casilla **Puertos dinámicos TCP** y proporcione un número de puerto disponible en la casilla **Puerto TCP** . Para obtener más información acerca de cómo abrir los puertos del firewall, vea Configurar Firewall de Windows para permitir el acceso a SQL Server en los Libros en pantalla.  
  
## <a name="dynamic-ports"></a>Puertos dinámicos  
 Al iniciar, cuando una instancia de SQL Server está configurada para escuchar en puertos dinámicos, comprueba la existencia en el sistema operativo de un puerto disponible y abre un extremo para dicho puerto. Las conexiones entrantes deben especificar dicho número de puerto para conectarse. Puesto que el número de puerto puede cambiar cada vez que se inicia SQL Server, SQL Server proporciona el servicio SQL Server Browser para supervisar los puertos y dirigir las conexiones entrantes al puerto actual para dicha instancia. El uso de puertos dinámicos dificulta la conexión de SQL Server mediante un firewall porque el número de puerto puede cambiar cuando SQL Server se reinicia, lo que implica tener que hacer cambios en la configuración del firewall. Para evitar problemas de conexión a través de un firewall, configure SQL Server para que use un puerto estático.  
  
## <a name="options"></a>Opciones  
 **Activo**  
 Indica que la dirección IP está activa en el equipo. No está disponible para **IPAll**.  
  
 **Enabled**  
 Si la propiedad **Escuchar todo** del cuadro de diálogo **Propiedades de TCP/IP (pestaña Protocolo)** se ha establecido en **No**, esta propiedad indicará si SQL Server está escuchando en la dirección IP. Si la propiedad **Escuchar todo** del cuadro de diálogo **Propiedades de TCP/IP (pestaña Protocolo)** se ha establecido en **Sí**, se omitirá esta propiedad. No está disponible para **IPAll**.  
  
 **Dirección IP**  
 Presenta o cambia la dirección IP empleada por esta conexión. Muestra la dirección IP empleada por el equipo y la dirección IP de bucle invertido, 127.0.0.1. No está disponible para **IPAll**. La dirección IP puede tener el formato IPv4 o IPv6.  
  
 **Puertos dinámicos TCP**  
 En blanco, si los puertos dinámicos no están habilitados. Para utilizar puertos dinámicos, establezca esta opción en 0.  
  
 Para **IPAll**, muestra el número del puerto dinámico utilizado.  
  
 **Puerto TCP**  
 Vea o cambie el puerto donde escucha SQL Server. De manera predeterminada, la instancia predeterminada de SQL Server escucha en el puerto 1433.  
  
 El motor de base de datos puede escuchar en varios puertos con la misma dirección IP y enumerar los puertos, separados por comas, con el formato 1433,1500,1501. Este campo tiene un límite de 2.047 caracteres.  
  
 Para configurar que una misma dirección IP escuche en varios puertos, el parámetro **Escuchar todo** también tiene que establecerse en **No**(en la **pestaña Protocolos** del cuadro de diálogo **Propiedades de TCP/IP** ). Para obtener más información, consulte: "Procedimientos: Configuración del motor de base de datos para escuchar en varios puertos TCP" en los Libros en pantalla de SQL Server.  
  
## <a name="adding-or-removing-ip-addresses"></a>Agregar o quitar direcciones IP  
 Administrador de configuración de SQL Server muestra las direcciones IP disponibles en el momento en que se instaló SQL Server. Las direcciones IP disponibles pueden cambiar cuando se agregan o se quitan tarjetas de red, cuando expira una dirección IP asignada dinámicamente, cuando se vuelve a configurar la estructura de red o cuando cambia la ubicación física del equipo, por ejemplo un equipo portátil que se conecta a la red en un edificio diferente. Para cambiar una dirección IP, edite el cuadro **Dirección IP** y, luego, reinicie SQL Server.  
  
## <a name="additional-topics-in-books-online"></a>Temas adicionales en los libros en pantalla  
 En MSDN puede encontrar temas como **Configurar un servidor para que escuche en un puerto TCP específico (Administrador de configuración de SQL Server)** y **Configurar el motor de base de datos para escuchar en varios puertos TCP**.  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un protocolo de red](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [Crear una cadena de conexión válida con TCP/IP](creating-a-valid-connection-string-using-tcp-ip.md)   
 [Servicio SQL Server Browser](sql-server-browser-service.md)  
  
