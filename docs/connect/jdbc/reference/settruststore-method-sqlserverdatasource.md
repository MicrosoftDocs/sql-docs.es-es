---
description: Método setTrustStore (SQLServerDataSource)
title: Método setTrustStore (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setTrustStore Method (SQLServerDataSource)
apilocation:
- setTrustStore Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: bab5485d-4547-426c-adbe-44e2b5702d1d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d537e17309db560b914ea4b9a71c726e9bfe2003
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172947"
---
# <a name="settruststore-method-sqlserverdatasource"></a>Método setTrustStore (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece la ruta de acceso (incluido el nombre de archivo) del archivo trustStore del certificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setTrustStore(java.lang.String trustStore)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *trustStore*  
  
 Un objeto **string** que contiene la ruta de acceso (incluido el nombre de archivo) del archivo trustStore del certificado.  
  
## <a name="remarks"></a>Observaciones  
 Si la propiedad trustStore no se especifica o se establece en NULL, el [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] se basará en las reglas de búsqueda del generador del administrador de confianza para determinar qué almacén de certificados se usará. El generador TrustManagerFactory SunX509 predeterminado intenta buscar material de confianza en el orden de búsqueda de ubicaciones siguiente:  
  
-   1. Un archivo especificado por la propiedad del sistema "javax.net.ssl.trustStore" de la máquina virtual Java (JVM).  
  
-   2. Archivo "\<java-home>/lib/security/jssecacerts".  
  
-   3. Archivo "\<java-home>/lib/security/cacerts".  
  
 Para obtener más información, consulte la documentación de la interfaz de SunX509 TrustManager en el sitio web de Sun Microsystems.  
  
 Si la propiedad trustStore está establecida en una cadena o en una cadena vacía "", el controlador usará ese valor para buscar el archivo trustStore con el fin de validar el certificado TLS/SSL del servidor.  
  
 La propiedad trustStorePassword se puede especificar junto con la propiedad trustStore y su valor se utiliza para abrir el archivo trustStore. Para obtener más información, vea [setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md).  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
