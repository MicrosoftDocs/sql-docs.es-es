---
title: Notas de la versión de mssql-tools en Linux y macOS
description: Conozca las novedades y los cambios en las versiones publicadas de Microsoft SQL Server Tools.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: v-zhangw
ms.author: v-zhangw
manager: kenvh
ms.openlocfilehash: 75f1144e84792b3c9361dcab74dcdbcb7b072268
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075927"
---
# <a name="release-notes-for-the-microsoft-sql-server-tools-on-linux-and-macos"></a>Notas de la versión de Microsoft SQL Server Tools en Linux y macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

En este artículo se enumeran y describen las novedades en los lanzamientos de versiones de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] SQL Server Tools en Linux y macOS.

## <a name="17711-january-2021"></a>17.7.1.1, enero de 2021

| Característica agregada | Detalles |
| :------------ | :------ |
| Corrección de errores de sqlcmd | Se ha corregido el error de redireccionamiento de entrada y las líneas vacías que causaban una ejecución repetida. |
| Corrección de errores de sqlcmd | Se han corregido los informes de errores erróneos para las opciones -r, -p, -X y -k con ciertos formatos. |
| Opción -z/-Z "contraseña" de sqlcmd | Ahora se admite. |
| &nbsp; | &nbsp; |

## <a name="17611-july-2020"></a>17.6.1.1 (julio de 2020)

| Característica agregada | Detalles |
| :------------ | :------ |
| Analizador de línea de comandos sqlcmd actualizado | Se han corregido errores en los que se producía un comportamiento inesperado al usar determinadas opciones en pedidos diferentes. |
| Mensajes de error de sqlcmd actualizados | Se han corregido varias incoherencias en el modo en que se devolvieron los errores de sqlcmd. |
| Opción Y de sqlcmd corregida | Se ha corregido un problema en el que la opción Y no funcionaba correctamente. |
| Truncado del nombre de columna de sqlcmd corregido | Se ha corregido un problema por el que los nombres de columna se truncaban incorrectamente. |
| Códigos de salida de Linux de sqlcmd | Se ha corregido un problema en el que faltaba el código de salida del proceso en Linux. |
| &nbsp; | &nbsp; |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo conectarse con [BCP](connecting-with-bcp.md) y [SQLCMD](connecting-with-sqlcmd.md).
