---
description: Vistas
title: Vistas | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], about views
ms.assetid: ada83c28-e8b7-45d9-b53c-b3d67c8820c8
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5a3bcbd7b22d7d51cc9d1824b192e0311d898b7c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484387"
---
# <a name="views"></a>Vistas
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Una vista es una tabla virtual cuyo contenido está definido por una consulta. Al igual que una tabla, una vista consta de un conjunto de columnas y filas de datos con un nombre. Sin embargo, a menos que esté indizada, una vista no existe como conjunto de valores de datos almacenados en una base de datos. Las filas y las columnas de datos proceden de tablas a las que se hace referencia en la consulta que define la vista y se producen de forma dinámica cuando se hace referencia a la vista.  
  
 Una vista actúa como filtro de las tablas subyacentes a las que se hace referencia en ella. La consulta que define la vista puede provenir de una o de varias tablas, o bien de otras vistas de la base de datos actual u otras bases de datos. Asimismo, es posible utilizar las consultas distribuidas para definir vistas que utilicen datos de orígenes heterogéneos. Esto puede resultar de utilidad, por ejemplo, si desea combinar datos de estructura similar que proceden de distintos servidores, cada uno de los cuales almacena los datos para una región distinta de la organización.  
  
 Las vistas suelen usarse para centrar, simplificar y personalizar la percepción de la base de datos para cada usuario. Las vistas pueden emplearse como mecanismos de seguridad, que permiten a los usuarios obtener acceso a los datos por medio de la vista, pero no les conceden el permiso de obtener acceso directo a las tablas base subyacentes de la vista. Las vistas pueden utilizarse para proporcionar una interfaz compatible con versiones anteriores con el fin de emular una tabla que existía pero cuyo esquema ha cambiado. También pueden usarse para copiar datos entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a fin de mejorar el rendimiento y crear particiones de los datos.  
  
## <a name="types-of-views"></a>Tipos de vistas  
 Además del rol estándar de las vistas básicas definidas por el usuario, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona los siguientes tipos de vistas que permiten llevar a cabo objetivos especiales en una base de datos.  
  
 Vistas indizadas  
 Una vista indizada es una vista que se ha materializado. Esto significa que se ha calculado la definición de la vista y que los datos resultantes se han almacenado como una tabla. Se puede indizar una vista creando un índice clúster único en ella. Las vistas indizadas pueden mejorar de forma considerable el rendimiento de algunos tipos de consultas. Las vistas indizadas funcionan mejor para consultas que agregan muchas filas. No son adecuadas para conjuntos de datos subyacentes que se actualizan frecuentemente.  
  
 Vistas con particiones  
 Una vista con particiones combina datos horizontales con particiones de un conjunto de tablas miembro en uno o más servidores. Esto hace que los datos aparezcan como si fueran de una tabla. Una vista que combina tablas miembro en la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es una vista con particiones local.  
  
 Vistas del sistema  
 Las vistas de sistema exponen metadatos de catálogo. Puede usar las vistas del sistema para devolver información acerca de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] u objetos definidos en la instancia. Por ejemplo, puede consultar la vista de catálogo sys.databases para devolver información sobre las bases de datos definidas por el usuario disponibles en la instancia. Para obtener más información, vea [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md).  
  
## <a name="common-view-tasks"></a>Tareas de vista comunes  
 En la tabla siguiente se proporcionan vínculos a las tareas comunes asociadas con la creación o modificación de una vista.  
  
|Tareas de vista|Tema|  
|----------------|-----------|  
|Describe cómo crear una vista.|[Crear vistas](../../relational-databases/views/create-views.md)|  
|Describe cómo crear una vista indizada.|[Crear vistas indizadas](../../relational-databases/views/create-indexed-views.md)|  
|Describe cómo modificar la definición de la vista.|[Modificar vistas](../../relational-databases/views/modify-views.md)|  
|Describe cómo modificar datos mediante una vista.|[Modificar datos mediante una vista](../../relational-databases/views/modify-data-through-a-view.md)|  
|Describe cómo eliminar una vista.|[Eliminar vistas](../../relational-databases/views/delete-views.md)|  
|Describe cómo devolver información acerca de una vista, como la definición de la vista.|[Obtener información acerca de una vista](../../relational-databases/views/get-information-about-a-view.md)|  
|Describe cómo cambiar el nombre de una vista.|[Cambiar el nombre de las vistas](../../relational-databases/views/rename-views.md)|  
  
## <a name="see-also"></a>Vea también  
 [Crear vistas sobre columnas XML](../../relational-databases/xml/create-views-over-xml-columns.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)  
  
