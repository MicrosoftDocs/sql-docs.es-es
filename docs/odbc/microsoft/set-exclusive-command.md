---
description: Comando exclusivo de Set
title: ESTABLECER comando exclusivo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET EXCLUSIVE command [ODBC]
ms.assetid: d4fe12c5-7e8b-4d20-9ea4-2bcaffb271f2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43cd58e5e9f714de0defe36cea020ccb96ac3efa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208625"
---
# <a name="set-exclusive-command"></a>Comando exclusivo de Set
Especifica si los archivos de tabla se abren para uso exclusivo o compartido en una red.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET EXCLUSIVE ON | OFF  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTIVAR  
 Limita la accesibilidad de una tabla abierta en una red al usuario que la abrió. La tabla no es accesible para otros usuarios de la red. SET EXCLUSIVe ON también evita que todos los demás usuarios tengan acceso de solo lectura.  
  
 Apagado  
 (Valor predeterminado para el controlador; los valores predeterminados de Visual FoxPro están ACTIVAdos para la sesión de datos global y desactivados para una sesión de datos privada). Permite que cualquier usuario de la red comparta y modifique una tabla abierta en una red.  
  
## <a name="remarks"></a>Observaciones  
 Al cambiar la configuración de SET EXCLUSIVe no se cambia el estado de las tablas abiertas previamente. Por ejemplo, si se abre una tabla con SET EXCLUSIVe SET ON y SET EXCLUSIVe se cambia posteriormente a OFF, la tabla conserva su estado de uso exclusivo.  
  
## <a name="see-also"></a>Consulte también  
 [Cuadro de diálogo de configuración de Visual FoxPro ODBC](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)
