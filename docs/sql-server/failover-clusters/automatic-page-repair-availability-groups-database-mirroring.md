---
title: Reparación de página automática para grupos de disponibilidad y creación de reflejo de base de datos
description: Aprenda a reparar automáticamente determinados tipos de daños en las páginas cuando una base de datos participa en un grupo de disponibilidad AlwaysOn o en la creación de reflejo de la base de datos.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- automatic page repair
- Availability Groups [SQL Server], automatic page repair
- database mirroring [SQL Server], automatic page repair
- suspect pages [SQL Server]
ms.assetid: cf2e3650-5fac-4f34-b50e-d17765578a8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 111a5ad98210207fcdf55d3656953ddda6ac38c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346187"
---
# <a name="automatic-page-repair-availability-groups-database-mirroring"></a>Reparación de página automática (grupos de disponibilidad: creación de reflejo de base de datos)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La reparación de página automática es compatible con la creación de reflejo de la base de datos y con [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Cuando ciertos tipos de errores dañan una página, dejándola ilegible, un asociado de creación de reflejo de la base de datos (ya sea principal o reflejado) o una réplica de disponibilidad (principal o secundaria) intenta recuperar la página automáticamente. El asociado y la réplica que no pueden leer la página solicitan una nueva copia de la página a su asociado o a otra réplica. Si la solicitud se realiza correctamente, la copia legible sustituirá a la página ilegible y esto resuelve el error en la mayoría de los casos.  
  
 Hablando en general, la creación de reflejo de la base de datos y [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] controlan los errores de E/S de formas equivalentes. Las pocas diferencias se describen aquí.  
  
> [!NOTE]  
>  La reparación de página automática es diferente de la reparación de DBCC. Una reparación automática de la página conserva todos los datos. En cambio, es posible que sea necesario eliminar algunas páginas y, por lo tanto, algunos datos cuando se utiliza la opción REPAIR_ALLOW_DATA_LOSS de DBCC para la corrección de errores.  
  
-   [Tipos de errores que provocan un intento de reparación de página automática](#ErrorTypes)  
  
-   [Tipos de páginas que no se pueden reparar automáticamente](#UnrepairablePageTypes)  
  
-   [Control de errores de E/S en la base de datos principal](#PrimaryIOErrors)  
  
-   [Control de los errores de E/S en la base de datos reflejada o secundaria](#SecondaryIOErrors)  
  
-   [Prácticas recomendadas para el desarrollador](#DevBP)  
  
-   [Procedimiento: ver los intentos de reparación de página automática](#ViewAPRattempts)  
  
##  <a name="error-types-that-cause-an-automatic-page-repair-attempt"></a><a name="ErrorTypes"></a> Tipos de errores que provocan un intento de reparación de página automática  
 La reparación de página automática de la creación de reflejo de la base de datos intenta reparar únicamente aquellas páginas de un archivo de datos en las que una operación no se ha realizado correctamente debido a uno de los errores que se muestran en la siguiente tabla.  
  
|Número de error|Descripción|Instancias que provocan el intento de una reparación de página automática|  
|------------------|-----------------|---------------------------------------------------------|  
|823|Se toman medidas solo cuando el sistema operativo ha realizado una prueba cíclica de redundancia (CRC) en la que se ha producido un error en los datos.|ERROR_CRC. El valor del sistema operativo para este error es de 23.|  
|824|Errores lógicos.|Los errores de datos lógicos, como la escritura incompleta o la suma incorrecta de comprobación de página.|  
|829|Se ha marcado una página como pendiente de restauración.|Todos.|  
  
 Para ver los errores más recientes 823 y 824 de CRC, vea la tabla [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) en la base de datos [msdb](../../relational-databases/databases/msdb-database.md) .  

  
##  <a name="page-types-that-cannot-be-automatically-repaired"></a><a name="UnrepairablePageTypes"></a> Tipos de páginas que no se pueden reparar automáticamente  
 La reparación de página automática no puede reparar los siguientes tipos de páginas de control:  
  
-   Página de cabecera del archivo (Id. 0 de página).  
  
-   Página 9 (la página de arranque de la base de datos).  
  
-   Páginas de asignación: páginas del mapa de asignación global (GAM), páginas del mapa de asignación global compartido (SGAM) y páginas de espacio disponible en páginas (PFS).  
  
 
##  <a name="handling-io-errors-on-the-principalprimary-database"></a><a name="PrimaryIOErrors"></a> Handling I/O Errors on the Principal/Primary Database  
 En la base de datos principal, solo se intentará realizar la reparación de página automática cuando la base de datos esté en el estado SYNCHRONIZED y la base de datos principal todavía esté enviando entradas de registro de la base de datos al servidor reflejado o secundario. A continuación se muestra la secuencia básica de acciones en un intento de reparación de página automática:  
  
1.  Cuando se produce un error de lectura en una página de datos de la base de datos principal, esta inserta una fila en la tabla [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) con el estado de error adecuado. Para la creación de reflejo de la base de datos, el servidor principal solicita una copia de la página al reflejo. Para [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], el servidor principal difunde la solicitud a todos los secundarios y obtiene la página del primero que responda. La solicitud especifica el identificador de la página y el LSN que se encuentra actualmente al final del registro vaciado. La página se marca como *pendiente de restauración*. Esto hace que no se pueda tener acceso a ella durante el intento de reparación de página automática. Si se pretende tener acceso a esta página durante el intento de reparación, se producirá el error 829 (pendiente de restauración).  
  
2.  Después de recibir la solicitud de la página, el servidor reflejado o secundario espera hasta que haya vuelto a crear el registro del LSN especificado en la solicitud. A continuación, el servidor reflejado o secundario intenta tener acceso a la página en su copia de la base de datos. Si se puede tener acceso a la página, el servidor reflejado o secundario envía la copia de la página al servidor principal. De lo contrario, el servidor reflejado o secundario devuelve un error al servidor principal y se produce un error en el intento de reparación de página automática.  
  
3.  El servidor principal procesa la respuesta que contiene la copia nueva de la página.  
  
4.  Después de que el intento de reparación de página automática repare una página sospechosa, la página se marcará en la tabla **suspect_pages** como restaurada (**event_type** = 5).  
  
5.  Si el error de E/S de la página ha producido [transacciones diferidas](../../relational-databases/backup-restore/deferred-transactions-sql-server.md), después de reparar la página, el servidor principal intentará resolver esas transacciones.  
  
 
##  <a name="handling-io-errors-on-the-mirrorsecondary-database"></a><a name="SecondaryIOErrors"></a> Control de los errores de E/S en la base de datos reflejada o secundaria  
 La creación de reflejo de la base de datos y [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]administran normalmente de la misma forma los errores de E/S en las páginas de datos que se producen en la base de datos reflejada o secundaria.  
  
1.  Con la creación de reflejo de la base de datos, si el servidor reflejado encuentra uno o más errores de E/S de página cuando vuelve a generar una entrada del registro, la sesión de creación de reflejo establece el estado SUSPENDED. Con [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], si una réplica secundaria encuentra uno o varios errores de E/S de página cuando rehace una entrada de registro, la base de datos secundaria entra en el estado SUSPENDED. En ese momento, el servidor reflejado o secundario inserta una fila en la tabla **suspect_pages** con el estado de error adecuado. A continuación, el servidor reflejado o secundario solicita una copia de la página al servidor principal.  
  
2.  El servidor principal intenta tener acceso a la página en su copia de la base de datos. Si se puede acceder a la página, el servidor principal envía la copia de la página al servidor reflejado o secundario.  
  
3.  Si el servidor reflejado o secundario recibe copias de cada página que ha solicitado, el servidor secundario intentará reanudar la sesión de creación de reflejo. Si un intento de reparación de página automática repara una página sospechosa, la página se marcará en la tabla **suspect_pages** como restaurada (**event_type** = 4).  
  
     Si un servidor reflejado o secundario no recibe una página que solicitó al servidor principal, el intento de reparación de página automática no se realizará correctamente. Con la creación de reflejo de la base de datos, la sesión de creación de reflejo sigue suspendida. Con [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], la base de datos secundaria sigue suspendida. Si se reanuda manualmente la sesión de creación de reflejo o la base de datos secundaria, se tendrán en cuenta de nuevo las páginas dañadas durante la fase de sincronización.  
  
 
##  <a name="developer-best-practice"></a><a name="DevBP"></a> Developer Best Practice  
 La reparación de página automática es un proceso asincrónico que se ejecuta en segundo plano. Por consiguiente, una operación de base de datos que solicite una página ilegible producirá errores y devolverá el código de error acerca de las condiciones que han provocado el error. Al desarrollar una aplicación para una base de datos reflejada o una base de datos de disponibilidad, debería interceptar las excepciones relativas a las operaciones con errores. Si el código de error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es 823, 824 u 829, debería volver a intentar dicha operación más adelante.  
  

##  <a name="how-to-view-automatic-page-repair-attempts"></a><a name="ViewAPRattempts"></a> Procedimientos para: ver los intentos de reparación de página automática  
 Las siguientes vistas de administración dinámica devuelven filas para los últimos intentos de reparación de página automática en una base de datos de disponibilidad o base de datos reflejada determinada, con un máximo de 100 filas por base de datos.  
  
-   **Grupos de disponibilidad AlwaysOn:**  
  
     [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
     Devuelve una fila por cada intento de reparación de página automática en cualquier base de datos de disponibilidad en una réplica de disponibilidad hospedada para cualquier grupo de disponibilidad por la instancia de servidor.  
  
-   **Creación de reflejo de base de datos:**  
  
     [sys.dm_db_mirroring_auto_page_repair &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)  
  
     Devuelve una fila para cada intento de reparación de página automática en cualquier base de datos reflejada en la instancia del servidor.  
  
 
## <a name="see-also"></a>Consulte también  
 [Administrar la tabla suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  


