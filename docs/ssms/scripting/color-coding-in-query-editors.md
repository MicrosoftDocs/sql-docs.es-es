---
title: Codificación de colores en el Editor de consultas
description: Obtenga información sobre cómo las categorías de texto están codificadas por colores para ayudarle a encontrar texto específico y cómo puede configurar una combinación de colores personalizada.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], color coding
- color coding [Query Editor]
ms.assetid: 802882dc-c997-4e3f-8a01-994bb43169ae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96fe150cd54cfd37074949a278ed40570dd18e83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354751"
---
# <a name="color-coding-in-query-editors"></a>Codificación de colores en el Editor de consultas

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El texto especificado en los editores de código se asigna a una categoría; cada categoría está identificada por un color. Los colores ayudan a encontrar texto rápidamente en el código. Por ejemplo, los comentarios se resaltan en verde oscuro. En la tabla siguiente se muestran los colores más comunes. Puede ver la lista completa de colores y sus categorías, así como configurar una combinación de colores personalizada, usando el menú **Herramientas**, **Opciones** . Para obtener más información sobre cómo cambiar los colores predeterminados, vea [Change Font Color, Size, and Style](./change-font-color-size-and-style.md).  
  
## <a name="default-code-colors"></a>Colores predeterminados para el código  
  
|Color|Category|  
|-----------|--------------|  
|Rojo|Cadena de SQL|  
|Verde oscuro|Comentario|  
|Negro sobre fondo plateado|Comando SQLCMD|  
|Fucsia|Función del sistema|  
|Verde|Tabla, vista o función con valores de tabla del sistema. También, los esquemas del sistema sys e INFORMATION_SCHEMA.|  
|Azul|Palabra clave|  
|Verde azulado|Números de línea o parámetro de plantilla|  
|Rojo oscuro|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procedimiento almacenado|  
|Gris oscuro|Operadores|  
  
## <a name="status-bar"></a>Barra de estado  
 Puede configurar que servidores registrados o servidores de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en el Explorador de objetos tengan distintos colores en la barra de estado del editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Esto ayuda a identificar a qué servidor está conectada cada ventana del editor cuando se tienen muchas ventanas abiertas al mismo tiempo. Para obtener más información sobre cómo establecer colores de la barra de estado, vea [Barra de estado &#40;Editor de consultas del motor de base de datos&#41;](./status-bar-database-engine-query-editor.md).  
  
 Algunos tipos de editores no muestran la barra de estado o no admiten varios colores.  
  
