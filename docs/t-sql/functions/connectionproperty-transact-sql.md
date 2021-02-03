---
description: CONNECTIONPROPERTY (Transact-SQL)
title: CONNECTIONPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONNECTIONPROPERTY_TSQL
- CONNECTIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- CONNECTIONPROPERTY statement
ms.assetid: 6bd9ccae-af77-4a05-b97f-f8ab41cfde42
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7168556e8b4352839eb7d23ea6ae73e811f61ce8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184240"
---
# <a name="connectionproperty-transact-sql"></a>CONNECTIONPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Para una solicitud que llega al servidor, esta función devuelve información sobre las propiedades de conexión de la conexión única que admite esa solicitud.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CONNECTIONPROPERTY ( property )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*property*  
La propiedad de la conexión. *property* puede ser uno de estos valores:
  
|Value|Tipo de datos|Descripción|  
|---|---|---|
|net_transport|**nvarchar(40)**|Devuelve el protocolo de transporte físico utilizado por esta conexión. Este valor no admite valores NULL. Posibles valores devueltos:<br /><br /> **HTTP**<br /> **Canalización con nombre**<br /> **De sesión**<br /> **Memoria compartida**<br /> **SSL**<br /> **TCP**<br /><br /> y<br /><br /> **VIA**<br /><br /> Nota: Siempre se devuelve **Session** cuando una conexión tiene habilitado tanto el conjunto de resultados activo múltiple (MARS) como la agrupación de conexiones.|  
|protocol_type|**nvarchar(40)**|Devuelve el tipo de protocolo de la carga. Actualmente, distingue entre TDS (TSQL) y SOAP. Acepta valores NULL.|  
|auth_scheme|**nvarchar(40)**|Devuelve el esquema de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la conexión. El esquema de autenticación puede utilizar la autenticación de Windows (NTLM, KERBEROS, DIGEST, BASIC, NEGOTIATE) o la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No admite valores NULL.|  
|local_net_address|**varchar(48)**|Devuelve la dirección IP del servidor que es el destino de esta conexión específico. Solo está disponible para las conexiones que utilicen el proveedor de transporte TCP. Acepta valores NULL.|  
|local_tcp_port|**int**|Devuelve el puerto TCP del servidor de destino de esta conexión, si se trata de una conexión que utiliza el transporte TCP. Acepta valores NULL.|  
|client_net_address|**varchar(48)**|Solicita la dirección del cliente que se está conectando a este servidor. Acepta valores NULL.|  
|physical_net_transport|**nvarchar(40)**|Devuelve el protocolo de transporte físico utilizado por esta conexión. Preciso cuando una conexión tiene habilitado Multiple Active Result Sets (MARS).|  
|\<Any other string>||Devuelve NULL en una entrada no válida.|  
  
## <a name="remarks"></a>Observaciones  
**local_net_address** y **local_tcp_port** devuelven NULL en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
Los valores devueltos coinciden con las opciones mostradas en las columnas correspondientes de la vista de administración dinámica [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md). Por ejemplo:
  
```sql
SELECT   
ConnectionProperty('net_transport') AS 'Net transport',   
ConnectionProperty('protocol_type') AS 'Protocol type';  
```  
  
## <a name="see-also"></a>Consulte también
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
  
  
