---
title: Ejecución de una consulta parcial
description: Obtenga ayuda para depurar una sección de una consulta compleja. Use el Editor de Transact-SQL para resaltar un segmento de script específico y ejecutarlo como una única consulta.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: af04ab37-6cbb-4185-9382-e5922fa5b1df
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 25ad063fc90539647a48bac2702e0cd2b80cd53b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018205"
---
# <a name="how-to-execute-a-partial-query"></a>Procedimientos: Ejecución de una consulta parcial

El Editor de Transact\-SQL le permite resaltar un segmento específico de script y ejecutarlo como una única consulta. Esto simplifica la depuración de secciones de consultas complejas.  
  
> [!WARNING]  
> El procedimiento siguiente usa entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
## <a name="to-partially-execute-a-query"></a>Para ejecutar parcialmente una consulta  
  
1. En el **Explorador de objetos de SQL Server**, haga doble clic en **PerishableFruits** en **Vistas** para abrirlo en el editor de Transact\-SQL.  
  
2. Resalte el segmento `SELECT p.Id, p.Name FROM dbo.Product p` del código, haga clic con el botón derecho y seleccione **Ejecutar consulta**.  
  
3. Observe que todas las filas con los campos especificados de la tabla `Products` se devuelven en el panel **Resultados**.