---
description: MSSQLSERVER_30053
title: MSSQLSERVER_30053 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
ms.assetid: 8ad23889-e243-4bd7-bc3e-150403399d89
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ae4efe0d2f27a273714e21713f420709220ccffc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198176"
---
# <a name="mssqlserver_30053"></a>MSSQLSERVER_30053
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|30053|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|FTXT_QUERY_E_WORDBREAKINGTIMEOUT|  
|Texto del mensaje|Se agotó el tiempo de espera de separación de palabras para la cadena de consulta de texto completo. Esto puede ocurrir si el divisor de palabras tarda mucho tiempo en procesar la cadena de consulta de texto completo, o si se está ejecutando un gran número de consultas en el servidor. Intente ejecutar de nuevo la consulta con menos carga.|  
  
## <a name="explanation"></a>Explicación  
Se puede producir un error de tiempo de espera en la separación de palabras en las situaciones siguientes:  
  
-   El separador de palabras para el lenguaje de consultas está configurado incorrectamente; por ejemplo, la configuración del Registro es incorrecta.  
  
-   El separador de palabras no funciona correctamente para una cadena de consulta determinada.  
  
-   El separador de palabras devuelve demasiados datos para una cadena de consulta determinada. Un exceso de datos se trata como un ataque potencial por saturación del búfer y se cierra el proceso de demonio de filtro (fdhost.exe), que hospeda los servicios de separación de palabras.  
  
-   La configuración del proceso de demonio de filtro es incorrecta.  
  
    Los problemas de configuración más comunes son la expiración de las contraseñas o una política de dominio que impide que la cuenta de demonio de filtro inicie sesión.  
  
-   Una carga de trabajo de consulta muy pesada se ejecuta en la instancia del servidor; por ejemplo, el separador de palabras tardó mucho en procesar la cadena de consulta de texto completo o un número grande de consultas se está ejecutando en el servidor. Tenga en cuenta que esta es la causa menos probable.  
  
## <a name="user-action"></a>Acción del usuario  
Seleccione la acción del usuario que sea adecuada según la causa probable de tiempo de espera, de la forma siguiente:  
  
|Causa probable|Acción del usuario|  
|------------------|---------------|  
|El separador de palabras del lenguaje de consulta está configurado incorrectamente.|Si usa un separador de palabras de otro fabricante, podría estar registrado incorrectamente en el sistema operativo. En este caso, registre de nuevo el separador de palabras. Para más información, vea [Revertir los separadores de palabras usados por las búsquedas a la versión anterior](~/relational-databases/search/revert-the-word-breakers-used-by-search-to-the-previous-version.md) (Revertir los separadores de palabras usados por las búsquedas a la versión anterior).|  
|El separador de palabras no funciona correctamente para una cadena de consulta determinada.|Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el separador de palabras, póngase en contacto con el Servicio de atención al cliente y soporte técnico de Microsoft.|  
|El separador de palabras devuelve demasiados datos para una cadena de consulta determinada.|Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el separador de palabras, póngase en contacto con el Servicio de atención al cliente y soporte técnico de Microsoft.|  
|La configuración del proceso de demonio de filtro es incorrecta.|Asegúrese de que está utilizando la contraseña actual y que una directiva de dominio no está evitando que la cuenta de demonio de filtro se registre.|  
|Se está ejecutando un alto volumen de carga de trabajo de consultas en la instancia de servidor.|Intente ejecutar de nuevo la consulta con menos carga.|  
  
## <a name="see-also"></a>Consulte también  
[Establecer la cuenta del servicio para el selector del demonio de filtro de texto completo](~/relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md)  
[Búsqueda de texto completo](~/relational-databases/search/full-text-search.md)  
[sp_help_fulltext_system_components &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
[Configurar y administrar separadores de palabras y lematizadores para la búsqueda](~/relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)  
[Configurar y administrar filtros para búsquedas](~/relational-databases/search/configure-and-manage-filters-for-search.md)  
  
