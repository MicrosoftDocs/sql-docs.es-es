---
description: MSSQLSERVER_17112
title: MSSQLSERVER_17112
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17112 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 29f6dd1b508a5833c465ef5af013e69de8d44077
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196801"
---
# <a name="mssqlserver_17112"></a>MSSQLSERVER_17112
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|17112|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|INIT_INVCOMMAND|
|Texto del mensaje|Se especificó una opción de inicio no válida, a, desde el Registro o el símbolo del sistema. Corrija la opción o quítela.|
||

## <a name="explanation"></a>Explicación

Este error indica que se ha especificado una [opción de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md) no válida. Cuando no se especifica correctamente una opción de inicio, SQL Server no se inicia o no se puede ejecutar según lo esperado. También se genera el error 17112.

En algunos casos, la instancia podría iniciarse, pero, al revisar el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los parámetros de inicio no parecen correctos:

> \<Datetime> Parámetros de inicio del Registro del servidor:  
\<Datetime> Servidor -d D:\Archivos de programa\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\master.mdf  
\<Datetime> Servidor -e D:\Archivos de programa\Microsoft SQL Server\MSSQL.1\MSSQL\LOG\ERRORLOG  
\<Datetime> Servidor -l D:\Archivos de programa\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\mastlog.ldf  
\<Datetime> Servidor -T1118 -g512

Observe que los dos últimos parámetros de inicio están en la misma línea.

En algunos casos, también es posible que vea que la adición de los parámetros de inicio necesarios no tiene el efecto previsto en el comportamiento del servidor.

## <a name="possible-causes"></a>Causas posibles

Estos problemas se producen por las siguientes causas:

- Uso de parámetros de inicio que no aparecen en la lista de parámetros de inicio válidos
- Especificación de parámetros de inicio sin los delimitadores adecuados (;)
- Copia y pegado de los parámetros de inicio desde editores de texto que introdujeron algunos caracteres especiales invisibles, (por ejemplo, un espacio antes de -T)
- Uso incorrecto de mayúsculas y minúsculas en el parámetro de inicio

## <a name="user-action"></a>Acción del usuario

Use la herramienta Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para proporcionar y validar los parámetros de inicio especificados para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Asegúrese de que cada uno de los parámetros de inicio esté delimitado correctamente y de que no haya ningún carácter especial.

## <a name="more-information"></a>Más información

Consulte los siguientes artículos para obtener más información acerca de este tema:

- [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md)
- [Servicios SCM - Configurar opciones de inicio del servidor](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)