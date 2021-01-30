---
description: Compatibilidad con versiones anteriores de los tipos de datos de C
title: Compatibilidad con versiones anteriores de los tipos de datos de C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], C data types
- compatibility [ODBC], C data types
- data types [ODBC], backward compatibility
- C data types [ODBC], backward compatibility
ms.assetid: b1453a65-ae03-4061-b0cf-a8434d8bc40b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 75ea0c53091faaece23f9d7316c9a6f242677a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212503"
---
# <a name="backward-compatibility-of-c-data-types"></a>Compatibilidad con versiones anteriores de los tipos de datos de C
SQL_C_SHORT, SQL_C_LONG y SQL_C_TINYINT se han reemplazado en ODBC por tipos con signo y sin signo: SQL_C_SSHORT y SQL_C_USHORT, SQL_C_SLONG y SQL_C_ULONG y SQL_C_STINYINT y SQL_C_UTINYINT. Un controlador ODBC *3. x* que debe funcionar con aplicaciones ODBC *2. x* debe admitir SQL_C_SHORT, SQL_C_LONG y SQL_C_TINYINT, porque cuando se les llama, el administrador de controladores los pasa al controlador.
