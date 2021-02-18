---
title: 'Protocolos de cliente: Propiedades de TCP/IP (pestaña Protocolo)'
description: Obtenga información sobre cómo especificar las opciones de TCP/IP en el Administrador de configuración de SQL Server, como el parámetro Mantener conexión y el número de puerto predeterminado.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- TCP/IP [SQL Server], client protocols
- client protocols [SQL Server]
ms.assetid: d04f1bce-069c-4a02-b561-c87c3282be36
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b173a2ac4e735f5a3924e957baab784ecc828efd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349835"
---
# <a name="client-protocols---tcp-ip-properties-protocol-tab"></a>Protocolos de cliente: Propiedades de TCP/IP (pestaña Protocolo)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  En el Administrador de configuración de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], use la pestaña **Protocolo** del cuadro de diálogo **Propiedades de TCP/IP** para ver o especificar las siguientes opciones. Para conectar a un puerto diferente, escriba el número de puerto en el cuadro **Puerto predeterminado** . Para obtener más información sobre cadenas de conexión, vea [Crear una cadena de conexión válida con TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md).  
  
## <a name="options"></a>Opciones  
 **Puerto predeterminado**  
 Especifica el puerto que la biblioteca de red de TCP/IP utilizará para intentar conectarse a la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El puerto predeterminado es 1433.  
  
 Al conectar a una instancia predeterminada del [!INCLUDE[ssDE](../../includes/ssde-md.md)], el cliente utiliza este valor. Si se ha configurado una instancia predeterminada para que escuche en un puerto diferente, cambie este valor a dicho número de puerto.  
  
 Al conectar a una instancia con nombre del [!INCLUDE[ssDE](../../includes/ssde-md.md)], el cliente intentará obtener el número de puerto desde el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en el servidor. Si el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se está ejecutando, se debe proporcionar el número de puerto mediante esta configuración o como parte de la cadena de conexión.  
  
 **Enabled**  
 Los valores posibles son **Yes** y **No**.  
  
 **Keep Alive**  
 Este parámetro (en milisegundos) controla la frecuencia con la que TCP intenta comprobar que una conexión inactiva sigue intacta mediante el envío de un paquete **KEEPALIVE** . El valor predeterminado es 30000 milisegundos.  
  
 **Intervalo entre mensajes de mantenimiento de conexión**  
 Este parámetro (en milisegundos) determina el intervalo que separa las retransmisiones **KEEPALIVE** hasta que se recibe una respuesta. El valor predeterminado es 1000 milisegundos.  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un protocolo de red](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [Nuevo alias &#40;pestaña Alias&#41;](../../tools/configuration-manager/new-alias-alias-tab.md)   
 [Propiedades de &#60;Alias&#62; &#40;pestaña Alias&#41;](../../tools/configuration-manager/alias-properties-alias-tab.md)  
  
