---
title: Objetos que admite el asistente Generar scripts
description: Vea los tipos de objeto que el Asistente para generar y publicar scripts puede ayudarle a publicar.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 071eb2cb-f073-41ca-9f4d-11d3b8803495
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ba406c4418979f4cdc57363806c9561b3480959
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474276"
---
# <a name="objects-supported-by-the-generate-scripts-wizard"></a>Objetos que admite el asistente Generar scripts
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  El asistente Generar y publicar scripts admite un subconjunto de los objetos admitidos por [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
## <a name="supported-objects"></a>Objetos admitidos  
 La tabla siguiente enumera los objetos que se pueden publicar y que admite el Asistente para generar y publicar scripts.  
  
:::row:::
    :::column:::
        Rol de aplicación
    :::column-end:::
    :::column:::
        Rol de base de datos
    :::column-end:::
    :::column:::
        Schema
    :::column-end:::
    :::column:::
        Agregados definidos por el usuario
    :::column-end:::
    :::column:::
        Ver*
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Assembly
    :::column-end:::
    :::column:::
        Restricción DEFAULT
    :::column-end:::
    :::column:::
        Procedimiento almacenado*
    :::column-end:::
    :::column:::
        Tipos de datos definidos por el usuario
    :::column-end:::
    :::column:::
        Colección de esquemas XML
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Restricción CHECK
    :::column-end:::
    :::column:::
        Catálogo de texto completo
    :::column-end:::
    :::column:::
        Synonym (Sinónimo)
    :::column-end:::
    :::column:::
        Función definida por el usuario
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Procedimiento almacenado de CLR (Common Language Runtime)*
    :::column-end:::
    :::column:::
        Índice
    :::column-end:::
    :::column:::
        Tabla
    :::column-end:::
    :::column:::
        Tabla definida por el usuario
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Función CLR definida por el usuario
    :::column-end:::
    :::column:::
        Regla
    :::column-end:::
    :::column:::
        Usuario**
    :::column-end:::
    :::column:::
        Tipo definido por el usuario
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 *Publicado sin cifrado.  
  
 **Cualquier usuario no perteneciente al sistema que exista en la base de datos se publica como Funciones.  
  
  
