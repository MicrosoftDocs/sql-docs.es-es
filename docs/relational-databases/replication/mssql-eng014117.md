---
description: MSSQL_ENG014117
title: MSSQL_ENG014117 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014117 error
ms.assetid: e5906a76-9511-4c47-8826-8c765b58a39d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 573e93f00f496b99bd685d28bf16ef985077a22f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198814"
---
# <a name="mssql_eng014117"></a>MSSQL_ENG014117
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|14117|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|%1!' no está configurado como base de datos de distribución.|  
  
## <a name="explanation"></a>Explicación  
 Este problema puede ocurrir si se cumple uno de los siguientes supuestos o ambos:  
  
-   Falta la entrada de la base de datos de distribución especificada en **msdb..MSdistributiondbs**.  
  
-   No hay una entrada para el servidor local en la base de datos **maestra** o la entrada que hay es incorrecta.  
  
     La replicación espera que todos los servidores de una topología se registren utilizando el nombre del equipo con un nombre de instancia opcional (en el caso de una instancia en clúster, el servidor virtual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el nombre de instancia opcional). Para que la replicación funcione correctamente, el valor que devuelve `SELECT @@SERVERNAME` por cada servidor de la topología debe coincidir con el nombre del equipo o con el nombre del servidor virtual con el nombre de la instancia opcional.  
  
     La replicación no se admite si ha registrado alguna de las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por dirección IP o por nombre de dominio completo (FQDN). Es posible que aparezca este error si registró alguna de las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante dirección IP o FQDN en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] cuando configuró la replicación.  
  
## <a name="user-action"></a>Acción del usuario  
 Compruebe que la instancia del distribuidor esté correctamente registrada. Si el nombre de red del equipo y el nombre de la instancia de SQL Server son diferentes, lleve a cabo una de estas acciones.  
  
-   Agregue el nombre de la instancia de SQL Server como nombre de red válido. Un método para establecer un nombre de red alternativo es agregarlo al archivo de hosts local. De manera predeterminada, el archivo de hosts local se encuentra en WINDOWS\system32\drivers\etc o WINNT\system32\drivers\etc. Para obtener más información, vea la documentación de Windows.  
  
     Por ejemplo, si el nombre de equipo es comp1 y el equipo tiene la dirección IP 10.193.17.129, y el nombre de la instancia es inst1/instname, agregue la siguiente entrada en el archivo de hosts:  
  
     10.193.17.129 inst1  
  
-   Deshabilite la distribución, registre la instancia y, a continuación, vuelva establecer la distribución. Si el valor de @@SERVERNAME no es correcto en una instancia no agrupada, siga estos pasos:  
  
    ```  
    sp_dropserver '<old_name>', 'droplogins'  
    go  
    sp_addserver '<new_name>', 'local'  
    go  
    ```  
  
     Después de ejecutar el procedimiento almacenado [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), debe reiniciar el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que el cambio en @@SERVERNAME surta efecto.  
  
     Si el valor de @@SERVERNAME no es correcto en una instancia en clúster, debe cambiarle el nombre usando el Administrador de clústeres. Para obtener más información, vea [Always On Failover Cluster Instances (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) (Instancias de clúster de conmutación por error de Always On [SQL Server]).  
  
 Después de comprobar que la instancia del distribuidor está correctamente registrada, asegúrese de que la base de datos de distribución aparece en **msdb..MSdistributiondbs**. Si no es así:  
  
1.  Genere un script para la configuración de la distribución. Para más información, consulte [Scripting Replication](../../relational-databases/replication/scripting-replication.md).  
  
2.  Deshabilite la distribución y, a continuación, vuelva a habilitarla. Para más información, consulte [Configure Distribution](../../relational-databases/replication/configure-distribution.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
