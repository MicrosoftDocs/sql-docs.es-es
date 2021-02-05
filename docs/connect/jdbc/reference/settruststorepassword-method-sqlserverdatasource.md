---
description: Método setTrustStorePassword (SQLServerDataSource)
title: Método setTrustStorePassword (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setTrustStorePassword Method (SQLServerDataSource)
apilocation:
- setTrustStorePassword Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: fa87cbde-71cc-4f21-bc07-f8ba2b6a0a3f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b83cabd8bba6cbebbf3649e6a85d89a7c80b3d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178469"
---
# <a name="settruststorepassword-method-sqlserverdatasource"></a>Método setTrustStorePassword (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece la contraseña que se usa para comprobar la integridad de los datos trustStore.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setTrustStorePassword(java.lang.String trustStorePassword)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *trustStorePassword*  
  
 Un objeto **String** que contiene la contraseña que se usa para comprobar la integridad de los datos trustStore.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad trustStorePassword se puede especificar junto con la propiedad trustStore y su valor se utiliza para comprobar la integridad del archivo trustStore.  
  
 Si se establece la propiedad trustStore pero no se establece la propiedad trustStorePassword, la integridad de trustStore no se comprueba.  
  
 Cuando las propiedades trustStore y trustStorePassword no se especifican, el controlador utilizará las propiedades del sistema de la máquina virtual Java (JVM), "javax.net.ssl.trustStore" y "javax.net.ssl.trustStorePassword". Si no se especifica la propiedad del sistema "javax.net.ssl.trustStorePassword", no se comprueba la integridad del trustStore.  
  
 Si no se establece la propiedad trustStore pero se establece la propiedad trustStorePassword, el controlador JDBC utilizará el archivo especificado por "javax.net.ssl.trustStore" como almacén de confianza y la integridad de éste se comprueba con la trustStorePassword especificada. Esto podría ser necesario cuando la aplicación cliente no desea almacenar la contraseña en la propiedad del sistema de JVM.  
  
 Para obtener más información, vea [Establecer las propiedades de conexión](../../../connect/jdbc/setting-the-connection-properties.md).  
  
 Desde el controlador JDBC 3.0, si establece SQLServerDataSource.setTrustStorePassword antes de enlazar las propiedades de origen de datos, deberá llamar a SQLServerDataSource.setTrustStorePassword antes de obtener la conexión. Para más información, vea [SQLServerDataSource.getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md).  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
