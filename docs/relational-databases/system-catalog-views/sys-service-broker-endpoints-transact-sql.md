---
description: sys.service_broker_endpoints (Transact-SQL)
title: sys.service_broker_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.service_broker_endpoints_TSQL
- service_broker_endpoints
- service_broker_endpoints_TSQL
- sys.service_broker_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_broker_endpoints catalog view
ms.assetid: 6979ec9b-0043-411e-aafb-0226fa26c5ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a3859eea7237eebb83b994fef037e377b433940
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212105"
---
# <a name="sysservice_broker_endpoints-transact-sql"></a>sys.service_broker_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esta vista de catálogo contiene una fila por cada extremo de Service Broker. Para cada fila de esta vista, hay una fila correspondiente con el mismo **endpoint_id** en la vista de **Sys.tcp_endpoints** que contiene los metadatos de configuración de TCP. TCP es el único protocolo permitido para Service Broker.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Hereda columnas de [Sys. endpoints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_message_forwarding_enabled**|**bit**|El extremo admite el reenvío de mensajes. Inicialmente se establece en **0** (deshabilitado). No acepta valores NULL.|  
|**message_forwarding_size**|**int**|Número máximo de megabytes de espacio de **tempdb** que se pueden usar para los mensajes que se reenvían. Inicialmente se establece en **10**. No acepta valores NULL.|  
|**connection_auth**|**tinyint**|Tipo de autenticación de la conexión necesario para las conexiones con este extremo; uno de los siguientes:<br /><br /> **1** : NTLM<br /><br /> **2** -Kerberos<br /><br /> **3** -negociación<br /><br /> **4** : certificado<br /><br /> **5** : NTLM, certificado<br /><br /> **6** -Kerberos, certificado<br /><br /> **7** -Negotiate, certificado<br /><br /> **8** -certificado, NTLM<br /><br /> **9** -certificado, Kerberos<br /><br /> **10** -certificado, Negotiate<br /><br /> No acepta valores NULL.|  
|**connection_auth_desc**|**nvarchar(60)**|Descripción del tipo de autenticación de conexión requerido para conexiones a este extremo. Es una de las opciones siguientes:<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICADO<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE<br /><br /> Acepta valores NULL.|  
|**certificate_id**|**int**|Identificador del certificado utilizado para la autenticación, si lo hay.<br /><br /> 0 = Se está utilizando la Autenticación de Windows.|  
|**encryption_algorithm**|**tinyint**|Algoritmo de cifrado. A continuación se muestran los posibles valores con sus descripciones y sus correspondientes opciones de DDL.<br /><br /> **0** : ninguno. Opción de DDL correspondiente: deshabilitada.<br /><br /> **1** : RC4. Opción de DDL correspondiente: {required &#124; obligatorio Algorithm RC4}.<br /><br /> **2** : AES. Opción de DDL correspondiente: algoritmo obligatorio AES.<br /><br /> **3** : ninguno, RC4. Opción de DDL correspondiente: {compatible &#124; algoritmo RC4}.<br /><br /> **4** : ninguno, AES. Opción de DDL correspondiente: algoritmo admitido AES.<br /><br /> **5** : RC4, AES. Opción de DDL correspondiente: algoritmo obligatorio RC4 AES.<br /><br /> **6** : AES, RC4. Opción DDL correspondiente: algoritmo obligatorio AES RC4.<br /><br /> **7** : ninguno, RC4, AES. Opción de DDL correspondiente: algoritmo RC4 compatible AES.<br /><br /> **8** : ninguno, AES, RC4. Opción de DDL correspondiente: algoritmo admitido AES RC4.<br /><br /> No acepta valores NULL.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Descripción del algoritmo de cifrado. A continuación se enumeran los valores posibles y sus correspondientes opciones de DDL:<br /><br /> NINGUNO: deshabilitado<br /><br /> RC4: {obligatorio &#124; algoritmo RC4}<br /><br /> AES: algoritmo obligatorio AES<br /><br /> NINGUNO, RC4: {compatible &#124; algoritmo RC4}<br /><br /> NINGUNO, AES: algoritmo compatible AES<br /><br /> RC4, AES: se requiere el algoritmo RC4 AES<br /><br /> AES, RC4: se requiere el algoritmo AES RC4<br /><br /> NINGUNO, RC4, AES: se admite el algoritmo RC4 AES<br /><br /> NINGUNO, AES, RC4: algoritmo compatible AES RC4<br /><br /> Acepta valores NULL.|  
  
## <a name="remarks"></a>Observaciones  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)  
  
  
