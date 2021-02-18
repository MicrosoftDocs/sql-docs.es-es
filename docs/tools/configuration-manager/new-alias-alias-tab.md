---
title: Nuevo alias (pestaña Alias)
description: Aprenda a crear un alias, un nombre alternativo para una instancia de SQL Server que se usa al conectarse a esa instancia. Vea ejemplos de cuándo usar un alias.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 785eb6fb-f67e-449d-b1c8-c38dfbb95ef6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d2a34ab643359f7afca304b64328638675f8a742
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349777"
---
# <a name="new-alias-alias-tab"></a>Nuevo alias (pestaña Alias)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Un alias es un nombre alternativo que se puede utilizar para establecer una conexión. El alias encapsula los elementos necesarios de una cadena de conexión y los expone con un nombre elegido por el usuario. Use la página **Alias** del cuadro de diálogo **Alias - Nuevo** para especificar los elementos de la cadena de conexión de un alias. Para cambiar la cadena de conexión de un alias existente, vea [Propiedades de &#60;Alias&#62; &#40;pestaña Alias&#41;](../../tools/configuration-manager/alias-properties-alias-tab.md).  
  
 No es necesario completar todos los valores de la cuadrícula **Propiedades** . Las combinaciones válidas varían en función del protocolo seleccionado. Vea los temas enumerados abajo para obtener ejemplos de combinaciones válidas.  
  
 **Nombre de alias**  
 Nombre (alias) que desee usar para hacer referencia a esta conexión.  
  
 **Nombre de canalización** / **Número de puerto**  
 Elementos adicionales de la cadena de conexión. El nombre de este cuadro varía según el protocolo seleccionado.  
  
 **Protocolo**  
 Protocolo utilizado para la conexión.  
  
 **Server**  
 Nombre de la instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que se va a conectar.  
  
## <a name="when-to-use-an-alias"></a>Cuándo utilizar un alias  
 De forma predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conecta a una instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el protocolo **Memoria compartida** y a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en otro equipo mediante **TCP/IP** o **Canalizaciones con nombre**. Cree un alias cuando use TCP/IP o canalizaciones con nombre y desee proporcionar una cadena de conexión personalizada; o bien, cuando desee usar un nombre distinto del nombre de servidor para la conexión.  
  
### <a name="examples"></a>Ejemplos  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no escucha en el puerto TCP/IP predeterminado 1433, por lo que querrá proporcionar una cadena de conexión con otro número de puerto.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no escucha en la canalización con nombre predeterminada, por lo que desea proporcionar una cadena de conexión con una canalización con nombre diferente.  
  
-   Una aplicación espera conectarse a una base de datos en el servidor llamado `ACCT`, pero la base de datos se ha consolidado como una instancia con el nombre `ACCT` en un servidor llamado `CENTRAL`. La aplicación no se puede modificar de forma sencilla. Cree un alias llamado `ACCT`, con una cadena de conexión que apunte a `CENTRAL\ACCT`.  
  
## <a name="creating-a-valid-connection-string"></a>Crear una cadena de conexión válida  
 Vea los siguientes temas para obtener una descripción y ejemplos de combinaciones válidas de propiedades de alias:  
  
-   [Crear una cadena de conexión válida con el protocolo de memoria compartida](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
-   [Crear una cadena de conexión válida con TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
-   [Crear una cadena de conexión válida con canalizaciones con nombre](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
