---
description: Configuración del proyecto (asignación de tipo) (AccessToSQL)
title: Configuración del proyecto (asignación de tipo) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access data types
- data types, default mappings
- default data type mappings
- Project Settings dialog box, Type Mapping
- SQL Server data types
- Type Mapping settings
ms.assetid: b87b9683-abed-4677-8c50-18bdba704655
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2fc9bbd8092c98d21263d3972ab04d90888fcbae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066415"
---
# <a name="project-settings-type-mapping-accesstosql"></a>Configuración del proyecto (asignación de tipo) (AccessToSQL)
La configuración del proyecto de asignación de tipos permite establecer asignaciones de tipos predeterminadas para el proyecto SSMA. También puede especificar asignaciones de tipos para objetos de base de datos individuales. Para obtener más información, vea [asignar tipos de datos de origen y de destino](mapping-source-and-target-data-types-accesstosql.md).  
  
La asignación de tipos está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** :  
  
-   Utilice el cuadro de diálogo **configuración del proyecto** para establecer las opciones de configuración del proyecto actual. Para obtener acceso a la configuración de asignación de tipos, en el menú **herramientas** , seleccione **configuración del proyecto** y, a continuación, haga clic en **tipo de asignación** en el panel izquierdo.  
  
-   Utilice el cuadro de diálogo **configuración predeterminada del proyecto** para establecer las opciones de configuración de todos los proyectos. Para obtener acceso a la configuración de asignación de tipos, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, seleccione tipo de proyecto de migración para el que se deben ver los valores de/Changed en la lista desplegable de la **versión de destino** de la migración y, a continuación, haga clic en **tipo de asignación** en el panel izquierdo.  
  
## <a name="options"></a>Opciones  
**Tipo de origen**  
Tipo de datos de acceso que se va a asignar.  
  
**Tipo de destino**  
El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino o SQL Azure para el tipo de datos de acceso especificado.  
  
En la tabla siguiente se muestra la asignación predeterminada entre los tipos de datos de origen y de destino.  
  
|Tipo de datos de Access|Tipo de datos de SQL Server|  
|--------------------|------------------------|  
|**binario [ \* .. \* ]**|**varbinary [ \* ]**|  
|**boolean**|**bit**|  
|**byte**|**tinyint**|  
|**monetaria**|**money**|  
|**date**|**datetime**|  
|**decimal**|**float**|  
|**double**|**float**|  
|**guid**|**uniqueidentifier**|  
|**integer**|**smallint**|  
|**long**|**int**|  
|**longbinary**|**varbinary(max)**|  
|**Memorando**|**nvarchar(max)**|  
|**memorando** -para Access 97|**ntext**|  
|**single**|**real**|  
|**texto [ \* ... \* ]**|**nvarchar [ \* ]**|  
|**texto [ \* .. \* ]** -para Access 97|**VARCHAR [ \* ]**|  
  
**Add (Agregar)**  
Haga clic para agregar un tipo de datos a la lista de asignaciones.  
  
**Editar**  
Haga clic para editar un tipo de datos en la lista asignación.  
  
**Quitar**  
Haga clic en esta opción para quitar la asignación de tipos de datos seleccionada de la lista de asignaciones.  
  
**Valores predeterminados**  
Haga clic para restablecer todas las asignaciones de tipos de datos a los valores predeterminados de SSMA.  
  
## <a name="see-also"></a>Consulte también  
[Asignación de tipos de datos de origen y de destino](mapping-source-and-target-data-types-accesstosql.md)  
[Referencia de la interfaz de usuario (acceso)](./user-interface-reference-accesstosql.md)  
