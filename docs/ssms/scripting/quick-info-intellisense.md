---
title: Información rápida (IntelliSense)
description: Obtenga información sobre cómo usar la opción Información rápida de IntelliSense para mostrar la declaración completa de cualquier identificador del código. En SQL Server Management Studio, la opción está disponible en el Editor del motor de base de datos y en el Editor de consultas XML.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Quick Info option [IntelliSense]
- declarations [IntelliSense]
- IntelliSense [SQL Server], Quick Info
- identifier declarations [IntelliSense]
ms.assetid: 3c8b59f4-1922-4bde-844f-5f2306514d96
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff2473620822f915deab34f67339cec6e4d0ea49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270833"
---
# <a name="quick-info-intellisense"></a>Información rápida (IntelliSense)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La opción [!INCLUDE[msCoName](../../includes/msconame-md.md)] Información rápida **de** IntelliSense muestra la declaración completa de cualquier identificador del código. Al mover el puntero del mouse sobre un identificador, su declaración se muestra en una ventana emergente de color amarillo. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], **Quick Info** está disponible en los Editores de Consulta XML y Motor de base de datos.  
  
## <a name="transact-sql-quick-info"></a>Quick Info de Transact-SQL  
 **Quick Info** muestra dos tipos de información en el Editor de consultas del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Cuando no está en modo de depuración, **Quick Info** muestra la declaración de la expresión. Cuando está en modo de depuración, **Quick Info** muestra en cambio el nombre de la expresión y su valor actual.  
  
 En el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , la opción **Información rápida** solo está disponible para las partes de la sintaxis de [!INCLUDE[tsql](../../includes/tsql-md.md)] que IntelliSense admite. Por ejemplo, si mueve el puntero del mouse sobre el identificador de un objeto que tiene un tipo de datos que IntelliSense no admite, la ventana emergente **Información rápida** muestra un mensaje que indica que no se admite el tipo de datos.  
  
## <a name="see-also"></a>Consulte también  
 [Sintaxis de Transact-SQL compatible con IntelliSense](./transact-sql-syntax-supported-by-intellisense.md)  
  
