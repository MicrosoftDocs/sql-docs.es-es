---
description: sys.database_mirroring_endpoints (Transact-SQL)
title: sys.database_mirroring_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring_endpoints_TSQL
- database_mirroring_endpoints
- sys.database_mirroring_endpoints
- database_mirroring_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HADR [SQL Server], endpoint
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_endpoints catalog view
ms.assetid: f2285199-97ad-473c-a52d-270044dd862b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d9570a43fe817eac49dfa953745272021af0b6f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209509"
---
# <a name="sysdatabase_mirroring_endpoints-transact-sql"></a>sys.database_mirroring_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila para el extremo de creación de reflejo de la base de datos de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  El extremo de creación de reflejo de la base de datos admite ambas sesiones entre los asociados de creación de reflejo de la base de datos y con testigos y sesiones entre la réplica principal de una Always On grupo de disponibilidad y sus réplicas secundarias.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|-|Hereda columnas de **Sys. endpoints** (para obtener más información, vea [Sys. endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)).|  
|**role**|**tinyint**|Rol de creación de reflejo, uno de los siguientes:<br /><br /> **0** = ninguno<br /><br /> **1** = asociado<br /><br /> **2** = testigo<br /><br /> **3** = todo<br /><br /> Nota: este valor solo es relevante para la creación de reflejo de la base de datos.|  
|**role_desc**|**nvarchar(60)**|Descripción del rol de creación de reflejo, uno de los siguientes:<br /><br /> **NONE**<br /><br /> **ASOCIADO**<br /><br /> **TESTIGO**<br /><br /> **ALL**<br /><br /> Nota: este valor solo es relevante para la creación de reflejo de la base de datos.|  
|**is_encryption_enabled**|**bit**|**1** significa que el cifrado está habilitado.<br /><br /> **0** significa que el cifrado está deshabilitado.|  
|**connection_auth**|**tinyint**|Tipo de autenticación de la conexión necesario para las conexiones con este extremo; uno de los siguientes:<br /><br /> **1** : NTLM<br /><br /> **2** -Kerberos<br /><br /> **3** -negociación<br /><br /> **4** : certificado<br /><br /> **5** : NTLM, certificado<br /><br /> **6** -Kerberos, certificado<br /><br /> **7** -Negotiate, certificado<br /><br /> **8** -certificado, NTLM<br /><br /> **9** -certificado, Kerberos<br /><br /> **10** -certificado, Negotiate|  
|**connection_auth_desc**|**Nvarchar (60)**|Descripción del tipo de autenticación necesario para las conexiones con este extremo; uno de los siguientes:<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICADO<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE|  
|**certificate_id**|**int**|Identificador del certificado utilizado para la autenticación, si lo hay.<br /><br /> 0 = Se está utilizando la Autenticación de Windows.|  
|**encryption_algorithm**|**tinyint**|Algoritmo de cifrado, uno de los siguientes:<br /><br /> **0** : ninguno<br /><br /> **1** -RC4<br /><br /> **2** -AES<br /><br /> **3** -ninguno, RC4<br /><br /> **4** : ninguno, AES<br /><br /> **5** -RC4, AES<br /><br /> **6** : AES, RC4<br /><br /> **7** : ninguno, RC4, AES<br /><br /> **8** : ninguno, AES, RC4|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Descripción del algoritmo de cifrado, uno de los siguientes:<br /><br /> Ninguno<br /><br /> RC4<br /><br /> AES<br /><br /> NONE, RC4<br /><br /> NONE, AES<br /><br /> RC4, AES<br /><br /> AES, RC4<br /><br /> NONE, RC4, AES<br /><br /> NONE, AES, RC4|  
  
## <a name="remarks"></a>Observaciones  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Especifique la dirección URL del extremo al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.database_mirroring &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
