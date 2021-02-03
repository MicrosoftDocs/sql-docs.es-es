---
title: Requisitos para usar las tablas con optimización para memoria | Microsoft Docs
description: Obtenga información sobre los requisitos para usar OLTP en memoria, como la versión de SQL Database, consideraciones sobre memoria y almacenamiento, y la instalación.
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47d9a7e8-c597-4b95-a58a-dcf66df8e572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5a7c64a949ad4eabfbd02ad78229b04a919ab032
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237190"
---
# <a name="requirements-for-using-memory-optimized-tables"></a>Requisitos para utilizar las tablas con optimización para memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Para usar OLTP en memoria en la base de datos de Azure, vea [Introducción a In-Memory (vista previa) en Base de datos SQL](/azure/azure-sql/in-memory-oltp-overview).  
  
 Además de los [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), estos son los requisitos para usar OLTP en memoria:  
  
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 (o posterior), cualquier edición. Para [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM (anterior a SP1) necesita las ediciones Enterprise, Developer o Evaluation.
    
    > [!NOTE]
    > Nota: OLTP en memoria necesita la versión de 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necesita suficiente memoria para almacenar los datos en tablas optimizadas para memoria e índices, así como memoria adicional para admitir la carga de trabajo en línea. Vea [Estimar los requisitos de memoria para las tablas con optimización para memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) para obtener más información.  

-   Cuando ejecute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una máquina virtual (VM), asegúrese de que haya suficiente memoria asignada a la VM para admitir la memoria necesaria para índices y tablas optimizadas para memoria. Dependiendo de la aplicación host de VM, la opción de configuración para garantizar la asignación de memoria para la VM podría denominarse Reserva de memoria o, si se usa la memoria dinámica, RAM mínima. Asegúrese de que esta configuración sea suficiente para las necesidades de las bases de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
-   Espacio en disco libre equivalente al doble del tamaño de las tablas durables optimizadas para memoria.  
  
-   El procesador debe admitir la instrucción **cmpxchg16b** para usar OLTP en memoria. Todos los procesadores de 64 bits modernos admiten la instrucción **cmpxchg16b**.  
  
     Si usa una máquina virtual y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muestra un error provocado por un procesador más antiguo, compruebe si la aplicación host de VM tiene una opción de configuración para permitir **cmpxchg16b**. Si no, puede usar Hyper-V, que admite **cmpxchg16b** sin necesidad de modificar ninguna opción de configuración.  
  
-   OLTP en memoria se instala como parte de **Servicios de Motor de base de datos**.  
  
     Para instalar la generación de informes ([Determinar si una tabla o un procedimiento almacenado se debe pasar a OLTP en memoria](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)) y [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (para administrar OLTP en memoria mediante el Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]), [Descarga de SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).   
  
## <a name="important-notes-on-using-hek_2"></a>Información importante sobre el uso de [!INCLUDE[hek_2](../../includes/hek-2-md.md)]  
  
-   A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] no hay límite para el tamaño de las tablas optimizadas para memoria más que la memoria disponible. 

-   En [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], el tamaño total en memoria de todas las tablas durables de una base de datos no debe superar los 250 GB. Para obtener más información, vea [Estimar los requisitos de memoria para las tablas con optimización para memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md).  

> [!NOTE]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, las ediciones Standard y Express admiten OLTP en memoria, pero imponen cuotas con respecto a la cantidad de memoria que se puede usar para las tablas optimizadas para memoria en una base de datos determinada. En la edición Standard es de 32 GB por base de datos; en la edición Express es de 352 MB por base de datos. 
  
-   Si crea una o más bases de datos con tablas optimizadas para memoria, debe habilitar la inicialización instantánea de archivos (IFI) mediante la concesión del derecho de usuario *SE_MANAGE_VOLUME_NAME* a la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin IFI, los archivos de almacenamiento optimizados para memoria (datos y archivos delta) se inicializarán en el momento de la creación, lo cual puede tener un impacto negativo en el rendimiento de la carga de trabajo. Para obtener más información sobre IFI, incluido cómo habilitarla, consulte [Inicialización instantánea de archivos de la base de datos](../../relational-databases/databases/database-instant-file-initialization.md).
  
## <a name="see-also"></a>Consulte también  
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
 [Inicialización instantánea de archivos de la base de datos](../../relational-databases/databases/database-instant-file-initialization.md)  
 [Guía de arquitectura de la memoria](../../relational-databases/memory-management-architecture-guide.md)
