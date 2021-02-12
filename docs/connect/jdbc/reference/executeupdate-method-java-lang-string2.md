---
description: Método executeUpdate (java.lang.String)
title: Método executeUpdate (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 02/07/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeUpdate (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 91ecb1cd-001d-4ac9-9ae8-5db05c3c2959
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 39ef10b030e96ca7d419977c56e9ca6388fb8e0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038565"
---
# <a name="executeupdate-method-javalangstring"></a>Método executeUpdate (java.lang.String)

Ejecuta la instrucción SQL determinada, que puede ser una instrucción INSERT, UPDATE, MERGE o DELETE; o una instrucción SQL que no devuelve nada, como una instrucción DDL de SQL.

## <a name="syntax"></a>Sintaxis

```
public final int executeUpdate(java.lang.String sql)
```

#### <a name="parameters"></a>Parámetros
*sql*

Objeto **String** que contiene la instrucción SQL.

## <a name="return-value"></a>Valor devuelto
Un valor **int** que indica el número de filas afectadas o 0 si se usa una instrucción DDL.

## <a name="exceptions"></a>Excepciones
[SQLServerException](./sqlserverexception-class.md)

## <a name="remarks"></a>Observaciones
El método executeUpdate especifica este método executeUpdate en la interfaz java.sql.PreparedStatement.

Si se llama a este método se producirá una excepción, ya que la instrucción SQL para el objeto SQLServerPreparedStatement se especificó cuando se creó el objeto.

## <a name="see-also"></a>Consulte también

[Método executeUpdate &#40;SQLServerPreparedStatement&#41;](./executeupdate-method-sqlserverpreparedstatement.md)

[Miembros de SQLServerPreparedStatement](./sqlserverpreparedstatement-members.md)

[Clase SQLServerPreparedStatement](./sqlserverpreparedstatement-class.md)
