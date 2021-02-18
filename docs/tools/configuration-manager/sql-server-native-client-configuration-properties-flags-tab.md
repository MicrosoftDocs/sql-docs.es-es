---
title: Propiedades de Configuración de SQL Server Native Client (pestaña Marcas)
description: Obtenga información sobre las opciones de la pestaña Marcas del cuadro de diálogo Propiedades de configuración de SQL Server Native Client.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 59af121d-c8b9-4faa-91a1-b664f2c9b441
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 582c05b7dad6d952aeec4a2f9464759069159b2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349598"
---
# <a name="sql-server-native-client-configuration-properties-flags-tab"></a>Propiedades de Configuración de SQL Server Native Client (pestaña Marcas)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Los clientes [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de este equipo se comunican con los servidores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante los protocolos suministrados en el archivo de biblioteca de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. En esta página se configura el equipo cliente para solicitar una conexión cifrada mediante la Seguridad de la capa de transporte (TLS), conocida anteriormente como Capa de sockets seguros (SSL). Si no es posible establecer una conexión cifrada, la conexión no se establecerá.  
  
 El proceso de inicio de sesión siempre está cifrado. Las opciones que se facilitan a continuación solo se aplican al cifrado de datos. Para obtener más información acerca de cómo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cifra la comunicación y las instrucciones sobre cómo configurar el cliente para que confíe en la entidad de certificación raíz del certificado del servidor, vea "Cifrar conexiones a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" y "Cómo: Habilitar las conexiones cifradas a [!INCLUDE[ssDE](../../includes/ssde-md.md)] ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager)" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Opciones  
 **ForceEncryption**  
 Se solicita una conexión mediante TLS.  
  
 **TrustServerCertificate**  
 Cuando se establece en **No**, el proceso de cliente intenta validar el certificado de servidor. El cliente y el servidor deben tener cada uno un certificado emitido por una entidad de certificación pública. Si el certificado no está presente en el equipo cliente o no se puede validar, la conexión finalizará.  
  
 Si se establece en **Yes**, el cliente no validará el certificado de servidor, por lo que se habilitará el uso de un certificado autofirmado.  
  
 **Confiar en certificado de servidor** solo está disponible si **Forzar cifrado de protocolos** se ha establecido en **Sí**.  
  
  
