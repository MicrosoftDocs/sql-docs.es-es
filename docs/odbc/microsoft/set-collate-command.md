---
description: Comando COLLATE Set
title: ESTABLECER comando COLLAte | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set collate command [ODBC]
ms.assetid: 00efbcd4-fea8-4061-86a5-82de413cb753
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6158f79c589e446c2b3c106a1d14fd58715714f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208649"
---
# <a name="set-collate-command"></a>Comando COLLATE Set
Especifica una secuencia de intercalación para los campos de caracteres en operaciones posteriores de indización y ordenación.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET COLLATE TO cSequenceName  
```  
  
## <a name="arguments"></a>Argumentos  
 *cSequenceName*  
 Especifica una secuencia de intercalación. En la tabla siguiente se describen las opciones de la secuencia de intercalación disponibles.  
  
|Opciones|Idioma|  
|-------------|--------------|  
|Holandés|Neerlandés|  
|GENERAL|Inglés, Francés, alemán, español moderno, Portugués y otros idiomas de Europa occidental|  
|Alemán|Orden de la libreta de teléfonos alemán (DIN)|  
|Islandia|Islandés|  
|SISTEMA|Equipo (la secuencia de intercalación predeterminada para versiones anteriores de FoxPro)|  
|NORDAN|Noruego, danés|  
|Español|Español tradicional|  
|SWEFIN|Sueco, finés|  
|UNIQWT|Peso único|  
  
> [!NOTE]  
>  Cuando se especifica la opción SPANISH, *CH* es una sola letra que se ordena entre *c* y *d*, y *ll* se ordena entre *l* y *m*.  
  
 Si especifica una opción de secuencia de intercalación como una cadena de caracteres literales, asegúrese de incluir la opción entre comillas:  
  
```  
SET COLLATE TO "SWEFIN"  
```  
  
 El equipo es la opción de secuencia de intercalación predeterminada y es la secuencia con la que los usuarios están familiarizados. Los caracteres se ordenan tal y como aparecen en la página de códigos actual.  
  
 La GENERAL puede ser preferible para los usuarios de los Estados Unidos y europeo occidental. Los caracteres se ordenan tal y como aparecen en la página de códigos actual. En las versiones de FoxPro anteriores a 2,5, es posible que se hayan creado índices con las funciones **Upper**() o **Lower**() para convertir los campos de caracteres en un caso coherente. En las versiones de FoxPro posteriores a la 2,5, en su lugar, puede especificar la opción de secuencia de intercalación GENERAL y omitir la conversión **superior**().  
  
 Si se especifica una opción de secuencia de intercalación distinta de MACHINE y se crea un archivo. idx, siempre se crea un Compact. idx.  
  
 Utilice SET ("COLLAte") para devolver la secuencia de intercalación actual.  
  
 Puede especificar una secuencia de intercalación para un origen de datos mediante el [cuadro de diálogo Configuración de ODBC Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md) o mediante la palabra clave COLLATE en la cadena de conexión con [SQLDriverConnect](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md). Esto es idéntico a emitir el comando siguiente:  
  
```  
SET COLLATE TO cSequenceName  
```  
  
## <a name="remarks"></a>Observaciones  
 SET COLLAte le permite ordenar las tablas que contienen caracteres acentuados para cualquiera de los idiomas admitidos. Cambiar la configuración de SET COLLAte no afecta a la secuencia de intercalación de los índices abiertos previamente. Visual FoxPro mantiene automáticamente los índices existentes, lo que proporciona la flexibilidad de crear muchos tipos diferentes de índices, incluso para el mismo campo.  
  
 Por ejemplo, si se crea un índice con SET COLLAte establecido en GENERAL y la opción establecer intercalación se cambia posteriormente a español, el índice conserva la secuencia de intercalación GENERAL.  
  
## <a name="see-also"></a>Consulte también  
 [Cuadro de diálogo de configuración de Visual FoxPro ODBC](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)
