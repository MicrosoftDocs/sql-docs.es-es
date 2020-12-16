---
description: Solucionar problemas de indización de texto completo
title: Solucionar problemas de indización de texto completo | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- indexes [full-text search]
- troubleshooting [SQL Server], full-text search
- troubleshooting [full-text search]
ms.assetid: 964c43a8-5019-4179-82aa-63cd0ef592ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 20105d3261815da97369e7dfae5840ec5ff00f9a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479446"
---
# <a name="troubleshoot-full-text-indexing"></a>Solucionar problemas de indización de texto completo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
     
##  <a name="troubleshoot-full-text-indexing-failures"></a><a name="failure"></a> Solucionar errores de indización de texto completo  
 Al rellenar o mantener un índice de texto completo, es posible que el indizador de texto completo no pueda indizar una o varias filas por las razones que se explican a continuación. Estos errores de nivel de fila no impiden que se realice el llenado. El indizador omite estas filas, lo que significa que no se pueden realizar consultas del contenido de estas filas.  
  
 Pueden producirse errores de indización cuando:  
  
-   El indizador no puede encontrar o cargar un componente de un filtro o separador de palabras. Este error puede producirse si la fila de la tabla contiene un formato de documento o un contenido en un idioma que no se ha registrado para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Este error también puede darse si el componente del separador de palabras o del filtro no se firmó o si no superó la comprobación de firmas al cargarse.  
  
-   Se produce un error en un componente, como un separador de palabras o un filtro, y devuelve un error al indizador. Este error puede ocurrir si el documento que se está indizando está dañado y el filtro no puede extraer texto del documento. También puede suceder cuando un componente no puede administrar el contenido de una sola fila que supere cierto tamaño, debido a los límites de memoria del host de demonio de filtro de texto completo (fdhost.exe).  
  
 El registro de rastreo contiene información relativa al motivo del error para cada error de nivel de fila. Los recuentos de errores se encuentran resumidos al final de un llenado completo o incremental.  
  
 Existen otros errores que pueden afectar al proceso de indización en sí e impedir que se realice el llenado:  
  
-   El índice de texto completo supera el límite del número de filas que puede contener un catálogo de texto completo.  
  
-   Se modifica, se quita o vuelve a generarse un índice clúster o un índice de clave de texto completo de la tabla que se está indizando.  
  
-   El catálogo de texto completo queda dañado debido a un error de hardware o a que el disco está dañado.  
  
-   Un grupo de archivos que contiene la tabla cuyo texto completo se está indizando se queda sin conexión o pasa a ser de solo lectura.  
  
 Revise el registro de rastreo al final de cualquier operación de llenado de índice de texto completo importante o cuando el llenado no llega a completarse.  
  
### <a name="unsigned-components"></a>Componentes sin firmar  
 De forma predeterminada, el indizador de texto completo necesita los filtros y los separadores de palabras que carga para firmarse. Si no están firmados, lo cual puede ocurrir cuando se instalan componentes personalizados, debe configurar el indizador de texto completo para que omita la comprobación de firmas.  
  
> [!IMPORTANT]  
>  Si se omite la comprobación de firmas, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es menos segura. Se recomienda que firme todo componente que implemente o que se asegure de que todo componente que adquiera esté firmado. Para obtener más información sobre cómo firmar componentes, vea [sp_fulltext_service &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).  
  
  
##  <a name="full-text-index-in-inconsistent-state-after-transaction-log-restored"></a><a name="state"></a> Índice de texto completo en estado incoherente después de la restauración del registro de transacciones  
 Al restaurar el registro de transacciones de una base de datos, es posible que aparezca una advertencia que indica que el índice de texto completo no es coherente. El motivo es que se ha modificado el índice de texto completo en una tabla después de realizar la copia de seguridad de la base de datos. Si esto ocurre, deberá ejecutar un rellenado completo (rastreo) en la tabla para devolver la coherencia al índice de texto completo. Para obtener más información, vea [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md).  
  
  
## <a name="see-also"></a>Vea también  
 [ALTER FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
