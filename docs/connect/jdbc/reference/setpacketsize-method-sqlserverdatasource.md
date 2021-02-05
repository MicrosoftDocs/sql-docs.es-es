---
description: Método setPacketSize (SQLServerDataSource)
title: Método setPacketSize (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setPacketSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5d490edc-a223-4870-a838-784952497e5f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c178e8412de5bc59072fa9f6734fd79dad61ba52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178737"
---
# <a name="setpacketsize-method-sqlserverdatasource"></a>Método setPacketSize (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el tamaño del paquete de red actual que se usa para establecer la comunicación con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y se especifica en bytes.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setPacketSize(int packetSize)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *packetSize*  
  
 Valor **int** que contiene el tamaño del paquete de red.  
  
## <a name="remarks"></a>Observaciones  
 El rango de valores aceptables para esta propiedad es [-1 | 0 | 512..32767]. Si esta propiedad se establece en un valor fuera del rango aceptable, se produce una excepción.  
  
 Es posible que la aplicación quiera establecer la propiedad packetSize mientras se conecta con el cifrado de Seguridad de la capa de transporte (TLS), antes conocida como Capa de sockets seguros (SSL). El [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] negocia el tamaño del paquete con el servidor. Si la propiedad encrypt se establece en "**true**" y el tamaño del paquete negociado es mayor que el tamaño de registro TLS del proveedor de seguridad predeterminado de la máquina virtual Java (JVM), el controlador generará un error y finalizará la conexión.  
  
 Por otra parte, es posible que la aplicación quiera establecer la propiedad packetSize sin solicitar el cifrado TLS. En este caso, si el servidor requiere que el cliente admita el cifrado TLS, el controlador comprueba el tamaño de registro de TLS del proveedor de seguridad predeterminado de la JVM. Si la propiedad packetSize es más grande que el tamaño de registro de TLS del proveedor de seguridad predeterminado de la JVM, el controlador generará un error y finalizará la conexión.  
  
 Para obtener más información sobre cómo usar TLS, consulte [Uso de cifrado](../../../connect/jdbc/using-ssl-encryption.md).  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
