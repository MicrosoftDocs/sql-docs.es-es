---
description: MSSQLSERVER_3168
title: MSSQLSERVER_3168 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3168 (Database Engine error)
ms.assetid: 991111d9-1eb3-43e9-9333-a75a775c3200
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9e57fa84cf49932c0a550290c11f267d6a055921
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194203"
---
# <a name="mssqlserver_3168"></a>MSSQLSERVER_3168
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|3168|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LDDB_SYSTEMWRONGVER|  
|Texto del mensaje|No se puede restaurar la copia de seguridad de la base de datos del sistema en el dispositivo %!s! porque se creó con una versión de servidor (%!s!) distinta de la de este servidor (%!s!)|  
  
## <a name="explanation"></a>Explicación  
No puede restaurar una copia de seguridad de una base de datos del sistema (**master**, **model** o **msdb**) en una compilación de servidor distinta de la compilación en la que se realizó la copia de seguridad.  
  
> [!NOTE]  
> La instalación de un Service Pack o una compilación de revisión cambia el número de compilación del servidor y las compilaciones de servidor son siempre incrementales.  
  
### <a name="possible-causes"></a>Causas posibles  
El esquema de la base de datos para bases de datos del sistema pueda cambiar entre compilaciones de servidor. Para asegurarse de que un cambio de esquema no crea incoherencias, la instrucción RESTORE compara el número de compilación del servidor en el archivo de copia de seguridad con el del servidor en el que está intentando restaurar la copia de seguridad. Si las compilaciones son diferentes, la instrucción genera el mensaje de error 3168 y la operación de restauración finaliza de forma anómala.  
  
Entre los escenarios en los que se puede producir este problema se incluyen:  
  
-   Un usuario intenta restaurar una base de datos del sistema en el servidor A desde una copia de seguridad realizada en el servidor B. Los servidores A y B pertenecen a compilaciones de servidor diferentes. Por ejemplo, el servidor A se encuentra en una compilación de la versión original mientras que el servidor B se encuentra en una compilación del Service Pack 1 (SP1).  
  
-   Un usuario intenta restaurar una base de datos del sistema desde una copia de seguridad efectuada en el mismo servidor. Sin embargo, el servidor ejecutaba una compilación diferente cuando se realizó la copia de seguridad. Es decir, el servidor se actualizó después de realizar la copia de seguridad.  
  
## <a name="user-action"></a>Acción del usuario  
En estas situaciones, el proceso de restauración es muy complicado y solo se utiliza como último recurso. Para obtener más información, vea "[No puede restaurar copia de seguridad de base de datos de sistema a una generación distinta de SQL Server](https://support.microsoft.com/kb/264474)".  
  
## <a name="see-also"></a>Consulte también  
[Limitaciones sobre la restauración de las bases de datos del sistema &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md#limitations-on-restoring-system-databases)  
  
