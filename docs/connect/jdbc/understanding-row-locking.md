---
description: Descripción del bloqueo de fila
title: Descripción del bloque de fila | Microsoft Docs
ms.custom: ''
ms.date: 12/08/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 63c76a2f-f2b9-461f-8904-acbda0169ac3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5305f3feaa80d0a83dd1e7bfd97088492608ae5
ms.sourcegitcommit: 7f76975c29d948a9a3b51abce564b9c73d05dcf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96901065"
---
# <a name="understanding-row-locking"></a>Descripción del bloqueo de fila

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] usa bloqueos de fila de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estos implementan controles de la simultaneidad entre varios usuarios que realizan modificaciones en una base de datos al mismo tiempo. De forma predeterminada, las transacciones y bloqueos se administran para cada conexión. Por ejemplo, si una aplicación abre dos conexiones JDBC, los bloqueos que una conexión adquiere no se pueden compartir con la otra. Ninguna conexión puede adquirir bloqueos que estarían en conflicto con los bloqueos contenidos en la otra conexión.

> [!NOTE]  
> Si se utiliza el bloqueo de filas, se bloquean todas las filas en el búfer de captura, por lo que si se establece un valor muy grande como tamaño de la lectura, se puede afectar a la simultaneidad.

El bloqueo se utiliza para asegurar la integridad transaccional y la coherencia de las bases de datos. El bloqueo evita que los usuarios lean datos que otros usuarios estén cambiado y que varios usuarios cambien al mismo tiempo los mismos datos. Si no se utilizan bloqueos, los datos dentro de la base de datos podrían llegar a ser lógicamente incorrectos y las consultas de esos datos podrían generar resultados inesperados.

> [!NOTE]  
> Para obtener más información sobre el bloqueo de fila en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Bloqueo del [!INCLUDE[ssDE](../../includes/ssde_md.md)]](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine) en Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="see-also"></a>Consulte también

[Administración de conjuntos de resultados con el controlador JDBC](../../connect/jdbc/managing-result-sets-with-the-jdbc-driver.md)
