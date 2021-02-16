---
description: MSSQLSERVER_6602
title: MSSQLSERVER_6602
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 6602 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 594898082fb21f712cfb99abfb535aea46b5164d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348723"
---
# <a name="mssqlserver_6602"></a>MSSQLSERVER_6602
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|6602|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|XMLERR_PARSEERR2|
|Texto del mensaje|La descripción del error es '%.*ls'.|
||

## <a name="explanation"></a>Explicación

Este error se produce cuando intenta ejecutar un procedimiento almacenado `sp_xml_preparedocument` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que el contenido del parámetro `xmltext` es un documento XML complejo. Un mensaje de error similar al siguiente se notifica al usuario

> Error de análisis de XML 0x80004005 en la línea número 1, junto al texto XML "\<XML document sample>"  
Mensaje 6602, nivel 16, estado 2, procedimiento sp_xml_preparedocument, línea 1  
La descripción del error es "Error no especificado".

## <a name="cause"></a>Causa

Este problema se produce debido a una limitación de diseño del analizador MSXML (Msxmlsql.dll) que usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

El problema no está estrictamente relacionado con el tamaño del documento XML, sino con su estructura compleja. Una combinación de la profundidad de la estructura del elemento XML, el número y el tamaño de los atributos, y el número de entidades dentro de los atributos puede producir este problema. Sin embargo, el nivel de complejidad necesario para alcanzar este límite se encuentra en documentos XML con varios megabytes.

## <a name="user-action"></a>Acción del usuario

Para solucionar este problema, intente reducir la complejidad del documento XML.

> [!NOTE]
> Tenga cuidado con los atributos de cadena única de gran tamaño que contienen muchas entidades/XML.
