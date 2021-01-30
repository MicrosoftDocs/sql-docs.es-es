---
description: xp_msver (Transact-SQL)
title: xp_msver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_msver_TSQL
- xp_msver
dev_langs:
- TSQL
helpviewer_keywords:
- xp_msver
ms.assetid: 9264cf8c-92ba-45ad-b2d6-15d26d805a16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8a23619f5a1bf12ddcde718fc95db484f6d3cf6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99123846"
---
# <a name="xp_msver-transact-sql"></a>xp_msver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información de versión sobre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **xp_msver** también devuelve información sobre el número de compilación real del servidor y la información sobre el entorno del servidor. La información que devuelve **xp_msver** se puede usar en [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones, lotes, procedimientos almacenados, etc., para mejorar la lógica del código independiente de la plataforma.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
xp_msver [ optname ]  
```  
  
## <a name="arguments"></a>Argumentos  
 *optname*  
 Es el nombre de una opción y puede ser uno de los siguientes valores.  
  
|Opción/Nombre de columna|Descripción|  
|-------------------------|-----------------|  
|**ProductName**|Nombre del producto; por ejemplo, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**ProductVersion**|Versión del producto.|  
|**Idioma**|Versión del idioma de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Plataforma**|Nombre del sistema operativo, nombre del fabricante y nombre de la familia de circuitos integrados del equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Comentarios**|Información diversa acerca de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**CompanyName**|Nombre de la compañía que produce [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; por ejemplo, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corporation.|  
|**FileDescription**|Sistema operativo.|  
|**FileVersion**|Versión del ejecutable de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**InternalName**|Nombre interno de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; por ejemplo, SQLSERVR.|  
|**LegalCopyright**|Información legal acerca de los derechos de propiedad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; por ejemplo, Copyright© [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corp. 1988-2005.|  
|**LegalTrademarks**|Información legal de marca comercial requerida para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por ejemplo, [!INCLUDE[msCoName](../../includes/msconame-md.md)] es una marca registrada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corporation.|  
|**OriginalFilename**|Nombre del archivo que se ejecuta para iniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; por ejemplo, Sqlservr.exe.|  
|**PrivateBuild**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**SpecialBuild**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**WindowsVersion**|Versión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows instalada en el equipo en que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ProcessorCount**|Número de procesadores del equipo en que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ProcessorActiveMask**|Indica los procesadores instalados en el equipo donde se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows inicia y puede usar.|  
|**ProcessorType**|Tipo de procesador. Similar a la **plataforma**.|  
|**PhysicalMemory**|Cantidad de RAM en megabytes (MB) instalada en el equipo en que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Identificador de producto**|Número de Id. del producto (PID). Se especifica durante la instalación. Este número se encuentra en un adhesivo en la caja del CD original de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 1 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **xp_msver**, sin parámetros, devuelve un conjunto de resultados de cuatro columnas que enumera todos los valores de opción. **xp_msver**, para cualquier parámetro, devuelve el conjunto de resultados de cuatro columnas con los valores de esa opción.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Consulte también  
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados extendidos generales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [@@VERSION &#40;Transact-SQL&#41;](../../t-sql/functions/version-transact-sql-configuration-functions.md)  
  
  
