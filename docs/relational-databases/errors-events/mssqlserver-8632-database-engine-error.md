---
description: MSSQLSERVER_8632
title: MSSQLSERVER_8632
ms.custom: ''
ms.date: 10/27/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8632 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6755ddfcf85b8e49e00697e94eb91e340d39093c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205738"
---
# <a name="mssqlserver_8632"></a>MSSQLSERVER_8632
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|8632|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|QUERY_EXPRESSION_TOO_COMPLEX|
|Texto del mensaje|Error interno: se ha alcanzado el límite de servicios de una expresión. Busque posibles expresiones complejas en la consulta e intente simplificarlas.|
||

## <a name="explanation"></a>Explicación

El error 8632 se produce cuando se ejecuta una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que contiene un gran número de identificadores y constantes en una sola expresión. El usuario recibe un mensaje de error similar al siguiente:

> Servidor:  Mensaje 8632, nivel 17, estado 2, línea 1  
Error interno: se ha alcanzado el límite de servicios de una expresión. Busque posibles expresiones complejas en la consulta e intente simplificarlas.

## <a name="cause"></a>Causa

Este problema se produce porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] limita el número de identificadores y constantes que se pueden incluir en una única expresión de una consulta. Este límite es 65 535 GB. Por ejemplo, la consulta siguiente solo tiene una expresión:

```sql
select a, b + c, d + e
```

Esta expresión recupera las cinco columnas, calcula los operadores de suma y envía tres resultados proyectados al cliente.

La prueba para el número de identificadores y constantes se realiza después de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] expanda todos los identificadores y constantes a los que se hace referencia. Por ejemplo, los elementos siguientes se pueden expandir:

- El asterisco (*) de la lista de selección
- Una vista
- Una definición de columna calculada

Si el número después de la expansión supera el límite, la consulta no se puede ejecutar.

## <a name="user-action"></a>Acción del usuario

Para solucionar este problema, vuelva a escribir la consulta. Haga referencia a menos identificadores y constantes en la expresión de mayor tamaño de la consulta. Tendrá que asegurarse de que el número de identificadores y constantes de cada expresión de la consulta no supere el límite. Para ello, es posible que tenga que dividir una consulta en más de una sola consulta. Después, cree un resultado intermedio temporal.
