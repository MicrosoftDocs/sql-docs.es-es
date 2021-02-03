---
description: MSSQLSERVER_3859
title: MSSQLSERVER_3859
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3859 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: c8356608b3596a444d5a8a5ebe8bfbd26e324457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201528"
---
# <a name="mssqlserver_3859"></a>MSSQLSERVER_3859
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|3859|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|DBCC_CHECKCAT_DIRECT_UPDATE|
|Texto del mensaje|Advertencia: el catálogo del sistema se actualizó directamente en la base de datos con id. \%d, la vez más reciente a las %S_DATE|
||

## <a name="explanation"></a>Explicación

Este error indica que un usuario ha iniciado cambios en las tablas del sistema. No se admite la actualización manual de las tablas del sistema. Las tablas del sistema solo deben actualizarse mediante el motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detecta cambios iniciados por un usuario en las tablas del sistema, se produce el error 3859 en los dos escenarios siguientes:

- Escenario 1

    Se registra un evento similar al siguiente en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en el registro de aplicaciones del Visor de eventos cuando se inicia una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que contiene una tabla del sistema que ha sido actualizada manualmente:

    > Nombre de registro: Application  
    Origen: MSSQLSERVER Id. de evento: 3859  
    Categoría de la tarea: Server  
    Nivel: Información  
    Descripción: Advertencia: el catálogo del sistema se actualizó directamente en la base de datos con id. \%d, la vez más reciente a las **date_time**  

- Escenario 2  

    Se devuelve el siguiente mensaje de advertencia cuando se ejecuta el comando `DBCC_CHECKDB` después de actualizar manualmente una tabla del sistema:

    > Resultados de DBCC para "**database_name**".  
    Mensaje 8992, nivel 16, estado 1, línea 1  
    Mensaje de comprobación del catálogo 3859, estado 1: Advertencia: el catálogo del sistema se actualizó directamente en la base de datos con id. \%d, la vez más reciente a las **date_time**.  
    CHECKDB detectó 0 errores de asignación y 0 errores de coherencia en la base de datos "**db_name**".  
    Ejecución de DBCC completada. Si DBCC imprime algún mensaje de error, póngase en contacto con el administrador del sistema.

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