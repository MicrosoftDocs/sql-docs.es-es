---
description: Asignación de SQLInstallTranslator
title: Asignación de SQLInstallTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLInstallTranslator function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLInstallTranslator
ms.assetid: bcd9ba4f-7834-4bc4-876e-c7478998e524
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b063768fed57adffd4a6e5051e5383a2ad32a943
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202707"
---
# <a name="sqlinstalltranslator-mapping"></a>Asignación de SQLInstallTranslator
Cuando una aplicación ODBC *2. x* llama a **SQLInstallTranslator** a través de un controlador ODBC *3. x* , el administrador de controladores asigna la llamada a **SQLInstallTranslatorEx**. Una aplicación no debe llamar a **SQLInstallTranslator** en el administrador de controladores ODBC *3. x* con el argumento *lpszInfFile* establecido en un valor distinto de NULL. El ODBC. El archivo INF que se usa en ODBC *2. x* ya no se admite en ODBC *3. x*, incluso para mantener la compatibilidad con versiones anteriores.
