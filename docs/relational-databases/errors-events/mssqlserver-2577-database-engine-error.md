---
description: MSSQLSERVER_2577
title: MSSQLSERVER_2577 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2577 (Database Engine error)
ms.assetid: f53256a2-2fb0-47fd-9ed9-c45389104145
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8c5656bbe837c18177206a3c97055a84f33d7c39
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209834"
---
# <a name="mssqlserver_2577"></a>MSSQLSERVER_2577
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|2577|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBCC_IAM_CHAIN_SEQUENCE_OUT_OF_ORDER|  
|Texto del mensaje|Los números de secuencia de cadena no están ordenados en la cadena IAM (Mapa de asignación de índices) para el Id. de objeto O_ID, Id. de índice I_ID, Id. de partición PN_ID, Id. de unidad de asignación A_ID (tipo TYPE). La página P_ID1 con el número de secuencia SEQUENCE1 apunta a la página P_ID2 con el número de secuencia SEQUENCE2.|  
  
## <a name="explanation"></a>Explicación  
Cada página IAM (Mapa de asignación de índices) tiene un número de secuencia. Este número es la posición de la página IAM en la cadena IAM. La regla es que los números de secuencia aumenten de uno en uno para cada página IAM. La página IAM, *P_ID2*, tiene un número de secuencia que no sigue esta regla.  
  
## <a name="user-action"></a>Acción del usuario  
  
### <a name="look-for-hardware-failure"></a>Busque un error de hardware  
Ejecute un diagnóstico de hardware y corrija cualquier problema. Examine también los registros del sistema y de aplicación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows así como el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ver si el error se produjo como resultado de un error de hardware. Arregle cualquier problema relacionado con el hardware que encuentre en estos registros.  
  
Si sigue teniendo problemas de datos dañados, intente intercambiar diferentes componentes de hardware para aislar el problema. Asegúrese de que el sistema no tiene habilitada la memoria caché de escritura en el controlador de disco. Si cree que el problema se debe a la caché de escritura, póngase en contacto con su proveedor de hardware.  
  
Finalmente, puede resultarle útil cambiar a un nuevo sistema de hardware. Este cambio puede incluir volver a formatear las unidades de disco y volver a instalar el sistema operativo.  
  
### <a name="restore-from-backup"></a>Restaure mediante la copia de seguridad  
Si el problema no está relacionado con el hardware y tiene una copia de seguridad limpia disponible, úsela para restaurar la base de datos.  
  
### <a name="run-dbcc-checkdb"></a>Ejecute DBCC CHECKDB  
Si no tiene disponible una copia de seguridad limpia, ejecute DBCC CHECKDB sin una cláusula REPAIR para determinar el alcance de los daños. DBCC CHECKDB recomendará el uso de una cláusula REPAIR. A continuación, ejecute DBCC CHECKDB con la cláusula REPAIR apropiada para reparar el problema.  
  
> [!CAUTION]  
> Si no está seguro del efecto que tendrá DBCC CHECKDB con una cláusula REPAIR en los datos, póngase en contacto con su proveedor principal de soporte antes de ejecutar esta instrucción.  
  
Si ejecuta DBCC CHECKDB con una de las cláusulas REPAIR y no se soluciona el problema, póngase en contacto con su proveedor principal de soporte.  
  
### <a name="results-of-running-repair-options"></a>Resultados de ejecutar opciones REPAIR  
Al ejecutar REPAIR se volverá a generar la cadena IAM. REPAIR divide primero la cadena IAM existente en dos mitades. La primera mitad de la cadena finalizará con la página IAM, *P_ID1*. El puntero de página siguiente de la página *P_ID1* se establecerá en (0:0). La segunda mitad de la cadena comenzará con la página IAM, *P_ID2*. El puntero de página anterior de la página *P_ID2* se establecerá en (0:0).  
  
Después, REPAIR conectará las dos mitades de la cadena y volverá a generar los números de secuencia de la cadena IAM. Se cancelará la asignación de las páginas IAM que no se puedan reparar.  
  
> [!CAUTION]  
> Esta reparación puede causar la pérdida de datos.  
  
