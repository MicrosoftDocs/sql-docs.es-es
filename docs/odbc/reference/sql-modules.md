---
description: Módulos SQL
title: Módulos de SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- SQL modules [ODBC]
- sending SQL statements to DBMS [ODBC]
- SQL [ODBC], modules
- modules [ODBC]
- SQL statements [ODBC], modules
ms.assetid: 07551472-87ee-4765-951f-1364ed32f0c0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 39739ed5469b791cd0faf3df3946bebeb5d21761
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88499648"
---
# <a name="sql-modules"></a>Módulos SQL
La segunda técnica para enviar instrucciones SQL al DBMS se realiza a través de módulos. En Resumen, un módulo consta de un grupo de procedimientos, al que se llama desde el lenguaje de programación del host. Cada procedimiento contiene una única instrucción SQL y los datos se pasan a y desde el procedimiento a través de los parámetros.  
  
 Un módulo se puede considerar como una biblioteca de objetos que se vincula al código de la aplicación. Sin embargo, el modo en que se vinculan los procedimientos y el resto de la aplicación depende de la implementación. Por ejemplo, los procedimientos se pueden compilar en código objeto y vincularse directamente al código de la aplicación, se pueden compilar y almacenar en el DBMS y llamar a los identificadores del plan de acceso colocados en el código de la aplicación, o bien se pueden interpretar en tiempo de ejecución.  
  
 La principal ventaja de los módulos es que separan claramente las instrucciones SQL del lenguaje de programación. En teoría, debe ser posible cambiar uno sin cambiar el otro y simplemente volver a vincularlos.
