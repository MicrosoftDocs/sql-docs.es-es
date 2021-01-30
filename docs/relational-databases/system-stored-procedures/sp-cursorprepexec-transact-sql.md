---
description: sp_cursorprepexec (Transact-SQL)
title: sp_cursorprepexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorprepexec_TSQL
- sp_cursorprepexec
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorprepexec
ms.assetid: 8094fa90-35b5-4cf4-8012-0570cb2ba1e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b59597048ba7edc1102c3151d720e9bc6ff36942
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209697"
---
# <a name="sp_cursorprepexec-transact-sql"></a>sp_cursorprepexec (Transact-SQL)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Compila un plan para la instrucción de cursor enviada o lote. Después, crea y rellena el cursor. sp_cursorprepexec combina las funciones de sp_cursorprepare y sp_cursorexecute. Este procedimiento se invoca especificando el identificador 5 en un paquete de flujo TDS.  
  
 ![icono de vínculo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cursorprepexec prepared handle OUTPUT, cursor OUTPUT, params , statement , options  
    [ , scrollopt [ , ccopt [ , rowcount ] ] ]  
    [, '@parameter_name[,...n ]']
```  
  
## <a name="arguments"></a>Argumentos  
 *controlador preparado*  
 Es un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificador de *controlador* preparado generado. el *controlador preparado* es obligatorio y devuelve **int**.  
  
 *cursor*  
 Es el identificador de cursor generado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *cursor* es un parámetro necesario que se debe proporcionar en todos los procedimientos subsiguientes que actúan sobre este cursor, por ejemplo, sp_cursorfetch.  
  
 *params*  
 Identifica instrucciones con parámetros. La definición de *params* de variables se sustituye por los marcadores de parámetros de la instrucción. *params* es un parámetro necesario que requiere un valor de entrada **ntext**, **nchar** o **nvarchar** .  
  
> [!NOTE]  
>  Use una cadena **ntext** como valor de entrada cuando *stmt* tiene parámetros y el valor de PARAMETERIZED_STMT *scrollopt* es on.  
  
 *instrucción*  
 Define el conjunto de resultados del cursor. El parámetro de *instrucción* es obligatorio y llama a para un valor de entrada **ntext**, **nchar** o **nvarchar** .  
  
> [!NOTE]  
>  Las reglas para especificar el valor de stmt son las mismas que para sp_cursoropen, con la excepción de que el tipo de datos de la cadena *stmt* debe ser **ntext**.  
  
 *options*  
 Parámetro opcional que devuelve una descripción de las columnas del conjunto de resultados del cursor. * las opciones requieren el siguiente valor de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *scrollopt*  
 Opción de desplazamiento. *scrollopt* es un parámetro opcional que requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 Debido a la posibilidad de que la opción solicitada no sea adecuada para el cursor definido por *\<stmt>* , este parámetro actúa como entrada y salida. En casos como este, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna un tipo adecuado y modifica este valor.  
  
 *ccopt*  
 Opción de control de simultaneidad. *ccopt* es un parámetro opcional que requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (conocido anteriormente como LOCKCC)|  
|0x0004|**Optimista** (conocido anteriormente como OPTCC)|  
|0x0008|OPTIMISTIC (conocido anteriormente como OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Al igual que con *scrollpt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede asignar un valor diferente al que se solicitó.  
  
 *empleado*  
 Es un parámetro opcional que indica el número de filas del búfer de captura que se van a usar con AUTO_FETCH. El valor predeterminado es 20 filas. *RowCount* se comporta de forma diferente cuando se asigna como valor de entrada frente a un valor devuelto.  
  
|Como valor de entrada|Como valor devuelto|  
|--------------------|---------------------|  
|Cuando se especifica AUTO_FETCH con FAST_FORWARD cursor *RowCount* representa el número de filas que se van a colocar en el búfer de captura.|Representa el número de filas en el conjunto de resultados. Cuando se especifica el valor *scrollopt* AUTO_FETCH, *RowCount* devuelve el número de filas que se capturaron en el búfer de captura.|  

*parameter_name* Designe uno o varios nombres de parámetro tal y como se define en el argumento params.  Debe haber un parámetro proporcionado para cada parámetro incluido en params. Este argumento no es necesario cuando la instrucción de Transact-SQL o el lote en params no tienen parámetros definidos.
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Si params devuelve un valor NULL, la instrucción no tiene parámetros.  
  
## <a name="see-also"></a>Consulte también  
 [sp_cursoropen &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorexecute &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursorprepare &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorprepare-transact-sql.md)   
 [sp_cursorfetch &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
