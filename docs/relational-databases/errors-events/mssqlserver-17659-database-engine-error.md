---
description: MSSQLSERVER_17659
title: MSSQLSERVER_17659
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17659 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 5e7baf4f821a7fd36ca4202bc5299ae4cc99ae65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196675"
---
# <a name="mssqlserver_17659"></a>MSSQLSERVER_17659
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|17659|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|DEMO_SYSCATUPDATE|
|Texto del mensaje|Se ha actualizado la tabla del sistema con id. \%d directamente en la base de datos con id. \%d y puede que no se haya mantenido la coherencia de la caché. <br/> Debe reiniciar SQL Server.|
||

## <a name="explanation"></a>Explicación

Este error indica que un objeto del sistema se ha actualizado directamente. No se admite la actualización manual de las tablas del sistema. Las tablas del sistema solo deben actualizarse mediante el motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detecta cambios iniciados por un usuario en las tablas del sistema, se produce el error 17659. En este escenario, se registra un evento similar al siguiente en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en el registro de aplicaciones del Visor de eventos.

> Nombre de registro: Application  
Origen: MSSQLServer  
Id. de evento: 17659  
Categoría de la tarea: Server  
Nivel: Información  
Descripción: Advertencia: se ha actualizado la tabla del sistema con id. \%d directamente en la base de datos con id. %d y puede que no se haya mantenido la coherencia de la caché. Debe reiniciar SQL Server.

## <a name="user-action"></a>Acción del usuario

Para solucionar este problema, use uno de los siguientes métodos:

- Método 1  
    Si tiene una copia de seguridad limpia de la base de datos, restaure la base de datos a partir de esta.  
    > [!NOTE]
    > Este método solo funciona si la copia de seguridad no tiene incoherencias en los metadatos.  

- Método 2  
    Si no puede restaurar la base de datos a partir de una copia de seguridad, exporte los datos y los objetos a una nueva base de datos. Después, transfiera el contenido de la base de datos actualizada manualmente a la nueva base de datos. Tenga en cuenta que no se pueden reparar las incoherencias en los catálogos del sistema mediante las opciones REPAIR de los comandos DBCC CHECKDB. Por lo tanto, dado que el comando no puede reparar los daños en los metadatos, no proporciona ningún nivel de reparación recomendado.

> [!NOTE]
> Puede ver los datos de las tablas del sistema mediante las vistas de catálogo del sistema.

## <a name="more-information"></a>Más información

Para obtener más información, consulte el artículo [Tablas base del sistema](../system-tables/system-base-tables.md).