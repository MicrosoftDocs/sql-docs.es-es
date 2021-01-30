---
description: Comando DROP TABLE
title: Comando DROP TABLE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- drop table command [ODBC]
ms.assetid: bc50459b-8861-4889-84a9-129ae9065aa8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1284b2bb446b21986ae2862e9f8138de910b7922
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198582"
---
# <a name="drop-table-command"></a>Comando DROP TABLE
Quita una tabla de la base de datos especificada con el origen de datos y la elimina del disco.  
  
 El controlador ODBC de Visual FoxPro admite la sintaxis nativa del lenguaje Visual FoxPro para este comando. Para obtener información específica del controlador, consulte la sección Comentarios.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DROP TABLE TableName | FileName | ?  
```  
  
## <a name="settings"></a>Configuración  
 *TableName*  
 Especifica la tabla que se va a quitar de la base de datos especificada con el origen de datos y que se va a eliminar del disco.  
  
 *FileName*  
 Especifica una tabla libre que se va a eliminar del disco.  
  
 ?  
 Muestra el cuadro de diálogo quitar en el que puede elegir una tabla para quitarla de la base de datos especificada con el origen de datos y eliminar del disco.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se emite DROP TABLE, también se quitan todos los índices principales, los valores predeterminados y las reglas de validación asociadas a la tabla. DROP TABLE también afecta a otras tablas de la base de datos especificada con el origen de datos si esas tablas tienen reglas o relaciones asociadas a la tabla que se va a quitar. Las reglas y las relaciones ya no son válidas cuando se quita la tabla de la base de datos.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Cuando la aplicación envía la instrucción DROP TABLE de ODBC SQL al origen de datos, el controlador ODBC de Visual FoxPro convierte el comando en el comando de la tabla FoxProDROP de visual mediante la sintaxis que se muestra en la tabla siguiente.  
  
|Sintaxis de ODBC|Origen de datos|Sintaxis de Visual FoxPro|  
|-----------------|-----------------|--------------------------|  
|DROP TABLE *base-TABLE-Name*|Base de datos (archivo. DBC)|QUITAR tabla *TableName* Delete|  
||Directorio de tablas libres (archivos. dbf)|BORRAR *dbfName*<br /><br /> BORRAR *cdxName*<br /><br /> BORRAR *fptName*|
