---
description: Opciones (Diseñadores - Página Diseñadores de tablas y bases de datos)
title: Opciones (Diseñadores - Página Diseñadores de tablas y bases de datos)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Designers.Table_Designer
ms.assetid: b43f4b97-17b9-4004-a824-f77b9e145741
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 36e62c2d30e4678c8b41223003db35b13e8d15ac
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679243"
---
# <a name="options-designers---table-and-database-designers-page"></a>Opciones (Diseñadores - Página Diseñadores de tablas y bases de datos)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Utilice esta página para determinar el comportamiento predeterminado del diseñador. Para tener acceso a esta configuración, en el menú **Herramientas** , haga clic en **Opciones** , expanda la carpeta **Diseñadores** y haga clic en **Diseñador de tablas**.  
  
## <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario  
**Sobrescribir el valor de tiempo de espera de la cadena de conexión para actualizaciones del diseñador de tabla**  
Permite establecer un nuevo valor de tiempo de espera para las acciones del diseñador de tablas. Esto puede resultar útil si el diseñador de tablas afecta a una tabla grande y requiere más tiempo para completar la modificación.  
  
**Tiempo de espera de transacción después de**  
Establece un valor de tiempo de espera para el diseñador de tablas.  
  
**Generar automáticamente Scripts de cambio**  
Crea automáticamente un script para implementar los cambios definidos en el diseñador de tablas.  
  
**Advertir si existen claves principales nulas**  
Presenta un cuadro de diálogo de advertencia cuando un campo seleccionado para una clave principal puede contener valores NULL.  
  
**Advertir sobre detección de diferencias**  
Si activa esta casilla, al guardar los cambios, un cuadro de mensaje mostrará cualquier conflicto que se haya producido entre sus cambios y los realizados por otro usuario.  
  
**Advertir sobre las tablas afectadas**  
Presenta un cuando de diálogo de advertencia que muestra las tablas a las que va a afectar la acción y solicita confirmación.  
  
**Impedir guardar cambios que requieran volver a crear tablas**  
Evita que un usuario realice modificaciones que requieran volver a crear la tabla. Las acciones siguientes pueden requerir que se vuelva a crear una tabla:  
  
-   Agregar una nueva columna en la mitad de la tabla  
  
-   Quitar una columna  
  
-   Cambiar la nulabilidad de columnas  
  
-   Cambiar el orden de las columnas  
  
-   Cambiar el tipo de datos de una columna  
  
**Vista de tabla predeterminada**  
Seleccione la forma en que desea ver las tablas en los diseñadores:  
  
-   **Estándar**  
  
    Muestra el encabezado de tablas, todos los nombres de columna, tipos de datos y la configuración Permitir valores NULL.  
  
-   **Nombres de columna**  
  
    Muestra los nombres de columna.  
  
-   **Clave**  
  
    Muestra el encabezado de tabla y las columnas de clave principal.  
  
-   **Solo nombre**  
  
    Solo muestra el encabezado de tabla con su nombre.  
  
-   **Personalizada**  
  
    Permite elegir las columnas que se muestran.  
  
**Iniciar cuadro de diálogo Agregar tabla en un nuevo diagrama**  
Abre automáticamente el cuadro de diálogo **Agregar tabla** cuando se abren los diseñadores.  
  
