---
title: Navegación entre scripts
description: Obtenga información sobre cómo navegar entre scripts en el editor de Transact-SQL. Vea ejemplos en los que se muestra el uso de herramientas como Ir a la definición y Buscar todas las referencias.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.editor.howto.navigate
ms.assetid: 8664bde5-86ff-4e8b-b5a6-af003316f6ad
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: c62607e978fb32e332b518e0be6a436f318789e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018185"
---
# <a name="how-to-navigate-between-scripts"></a>Procedimientos: Navegación entre scripts

El Editor de Transact\-SQL para el desarrollo sin conexión proporciona dos herramientas de navegación útiles que resultan familiares a los usuarios de Visual Studio: Ir a la definición y Buscar todas las referencias. Por ejemplo, puede hacer clic con el botón secundario en el nombre de una tabla y usar “Buscar todas las referencias” para enumerar todas las referencias a la tabla en el proyecto. Puede hacer doble clic en un resultado de la búsqueda para ir a ese archivo de código determinado. En este archivo, puede volver a hacer clic con el botón secundario en el nombre de tabla y elegir “Ir a la definición” para volver a la definición de la tabla.  
  
> [!WARNING]  
> El procedimiento siguiente usa entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-navigate-between-scripts"></a>Para navegar entre scripts  
  
1.  Expanda la carpeta **Funciones** en el **Explorador de soluciones** y haga doble clic en **GetProductsBySupplier.sql**.  
  
2.  Haga clic con el botón derecho en `Products` en esta línea de código y seleccione **Ir a la definición**  
  
    ```  
    SELECT * from Products p  
    ```  
  
3.  Se abrirá automáticamente Products.sql, mostrando la ubicación donde se define el tipo `Products`.  
  
4.  Vuelva a GetProductsBySupplier.sql. Esta vez, seleccione **Buscar todas las referencias** en el menú contextual de `Products`. En el panel **Resultados de la búsqueda de símbolos**, verá una lista de ubicaciones donde se hace referencia a la tabla `Products`. Al hacer doble clic en cualquier resultado de la búsqueda irá a la ubicación de la referencia.  
  
