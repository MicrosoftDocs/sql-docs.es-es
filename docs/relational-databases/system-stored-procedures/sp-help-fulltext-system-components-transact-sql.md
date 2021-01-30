---
description: sp_help_fulltext_system_components (Transact-SQL)
title: sp_help_fulltext_system_components (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_components_TSQL
- sp_help_fulltext_components
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_system_components
ms.assetid: ac1fc7a0-7f46-4a12-8c5c-8d378226a8ce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2adf7a7ec8b598accb876529eb274158c01391c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198243"
---
# <a name="sp_help_fulltext_system_components-transact-sql"></a>sp_help_fulltext_system_components (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Devuelve información de los separadores de palabras, filtros y controladores de protocolo registrados. **sp_help_fulltext_system_components** también devuelve una lista de identificadores de bases de datos y catálogos de texto completo que han utilizado el componente especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_fulltext_system_components   
         { 'all'| [ @component_type = ] 'component_type' }  
    , [ @param = ] 'param'  
```  
  
## <a name="arguments"></a>Argumentos  
 'all'  
 Devuelve información para todos los componentes de texto completo.  
  
`[ @component_type = ] component_type` Especifica el tipo de componente. *component_type* puede ser una de las siguientes:  
  
-   **separador**  
  
-   **filter**  
  
-   **controlador de protocolo**  
  
-   **FullPath**  
  
 Si se especifica una ruta de acceso completa, también se debe especificar *param* con la ruta de acceso completa del archivo DLL del componente; de lo contrario, se devuelve un mensaje de error.  
  
`[ @param = ] param` Dependiendo del tipo de componente, es uno de los siguientes: un identificador de configuración regional (LCID), la extensión de archivo con el prefijo ".", el nombre de componente completo del controlador de protocolo o la ruta de acceso completa al archivo DLL del componente.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Para los componentes del sistema, se devuelve el siguiente conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**componenttype**|**sysname**|Tipo de componente. Uno de los siguientes:<br /><br /> filter<br /><br /> protocol handler<br /><br /> wordbreaker|  
|**componentName**|**sysname**|Nombre del componente.|  
|**CLSID**|**uniqueidentifier**|Identificador de clase del componente.|  
|**FullPath**|**nvarchar(256)**|Ruta de acceso a la ubicación del componente.<br /><br /> NULL = el autor de la llamada no es miembro del rol fijo de servidor **ServerAdmin** .|  
|**version**|**nvarchar(30)**|Versión del componente.|  
|**le**|**sysname**|Nombre del fabricante del componente.|  
  
 El siguiente conjunto de resultados se devuelve solo si existe uno o más de un catálogo de texto completo que usa *component_type*.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DBID**|**int**|Identificador de la base de datos.|  
|**ftcatid**|**int**|Id. del catálogo de texto completo.|  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol **Public** ; sin embargo, los usuarios solo pueden ver información sobre los catálogos de texto completo para los que tienen permiso VIEW DEFINITION. Solo los miembros del rol fijo de servidor **serveradmin** pueden ver los valores de la columna **fullpath** .  
  
## <a name="remarks"></a>Observaciones  
 Este método es de particular importancia cuando se prepara una actualización. Ejecute el procedimiento almacenado en una base de datos determinada y utilice la salida para determinar si un catálogo concreto se verá afectado por la actualización.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-full-text-system-components"></a>A. Mostrar todos los componentes de texto completo del sistema  
 En el ejemplo siguiente se muestran todos los componentes de texto completo del sistema que se han registrado en la instancia del servidor.  
  
```  
EXEC sp_help_fulltext_system_components 'all';  
GO  
```  
  
### <a name="b-listing-word-breakers"></a>B. Mostrar separadores de palabras  
 En el ejemplo siguiente se muestran todos los separadores de palabras registrados en la instancia del servicio.  
  
```  
EXEC sp_help_fulltext_system_components 'wordbreaker';  
GO  
```  
  
### <a name="c-determining-whether-a-specific-word-breaker-is-registered"></a>C. Determinar si un separador de palabras concreto está registrado  
 El ejemplo siguiente mostrará el separador de palabras del idioma turco (LCID = 1055) si se ha instalado en el sistema y si está registrado en la instancia del servicio. Este ejemplo especifica los nombres de parámetro, **\@ component_type** y **\@ param**.  
  
```  
EXEC sp_help_fulltext_system_components @component_type = 'wordbreaker', @param = 1055;  
GO  
```  
  
 De forma predeterminada, este separador de palabras no está instalado, por lo que el conjunto de resultados está vacío.  
  
### <a name="d-determining-whether-a-specific-filter-has-been-registered"></a>D. Determinar si se ha registrado un filtro específico  
 En el ejemplo siguiente se muestra el filtro del componente .xdoc si se ha instalado manualmente en el sistema y si está registrado en la instancia del servidor.  
  
```  
EXEC sp_help_fulltext_system_components 'filter', '.xdoc';  
GO  
```  
  
 De forma predeterminada, este filtro no se instala, por lo que el conjunto de resultados está vacío.  
  
### <a name="e-listing-a-specific-dll-file"></a>E. Mostrar un archivo .dll específico  
 En el ejemplo siguiente se muestra un archivo .ddl específico, `nlhtml.dll`, que se instala de manera predeterminada.  
  
```  
EXEC sp_help_fulltext_system_components 'fullpath',   
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\nlhtml.dll';  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ver o cambiar los filtros y separadores de palabras registrados](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)   
 [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurar y administrar filtros para búsquedas](../../relational-databases/search/configure-and-manage-filters-for-search.md)   
 [Búsqueda de texto completo y procedimientos almacenados de búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
