---
description: sp_cursorexecute (Transact-SQL)
title: sp_cursorexecute (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorexecute
- sp_cursorexecute_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_execute
ms.assetid: 6a204229-0a53-4617-a57e-93d4afbb71ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aab1670ae9d517ec60d3af572d15eaf12432f7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205161"
---
# <a name="sp_cursorexecute-transact-sql"></a>sp_cursorexecute (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea y rellena un cursor basado en el plan de ejecución creado por sp_cursorprepare. Este procedimiento, junto con sp_cursorprepare, tiene la misma función que sp_cursoropen, pero se divide en dos fases. sp_cursorexecute se invoca especificando el identificador 4 en un paquete de flujo TDS.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cursorexecute prepared_handle, cursor  
    [ , scrollopt[ OUTPUT ]  
    [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,bound param][,...n]]]]]  
```  
  
## <a name="arguments"></a>Argumentos  
 *prepared_handle*  
 Es el valor de *identificador* de la instrucción preparada devuelto por sp_cursorprepare. *prepared_handle* es un parámetro necesario que requiere un valor de entrada **int** .  
  
 *cursor*  
 Es el identificador del cursor generado por SQL Server. *cursor* es un parámetro necesario que se debe proporcionar en todos los procedimientos subsiguientes que actúan sobre el cursor, como sp_cursorfetch  
  
 *scrollopt*  
 Opción de desplazamiento. *scrollopt* es un parámetro opcional que requiere un valor de entrada **int** . El parámetro sp_cursorexecute *scrollopt* tiene las mismas opciones de valor que para Sp_cursoropen.  
  
> [!NOTE]  
>  No se admite el valor PARAMETERIZED_STMT.  
  
> [!IMPORTANT]  
>  Si no se especifica un valor de *scrollopt* , el valor predeterminado es Keyset independientemente del valor de *scrollopt* especificado en sp_cursorprepare.  
  
 *ccopt*  
 Opción de control de divisa. *ccopt* es un parámetro opcional que requiere un valor de entrada **int** . El parámetro sp_cursorexecute *ccopt* tiene las mismas opciones de valor que para Sp_cursoropen.  
  
> [!IMPORTANT]  
>  Si no se especifica un valor de *ccopt* , el valor predeterminado es optimista, independientemente del valor de *ccopt* especificado en sp_cursorprepare.  
  
 *empleado*  
 Es un parámetro opcional que indica el número de filas del búfer de captura que se van a usar con AUTO_FETCH. El valor predeterminado es 20 filas. *RowCount* se comporta de forma diferente cuando se asigna como valor de entrada frente a un valor devuelto.  
  
|Como valor de entrada|Como valor devuelto|  
|--------------------|---------------------|  
|Cuando se especifica AUTO_FETCH con FAST_FORWARD cursor *RowCount* representa el número de filas que se van a colocar en el búfer de captura.|Representa el número de filas en el conjunto de resultados. Cuando se especifica el valor *scrollopt* AUTO_FETCH, *RowCount* devuelve el número de filas que se capturaron en el búfer de captura.|  
  
 *bound_param*  
 Indica el uso opcional de parámetros adicionales.  
  
> [!NOTE]  
>  Cualquier parámetro después del quinto se pasa como parámetro de entrada al plan de instrucción.  
  
## <a name="code-return-value"></a>Valor del código de retorno  
 *RowCount* puede devolver los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|-1|Número de filas desconocido.|  
|-n|Un rellenado asincrónico está en vigor.|  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="scrollopt-and-ccopt-parameters"></a>Parámetros ccopt y scrollopt  
 *scrollopt* y *ccopt* son útiles cuando se retienen los planes almacenados en caché para la caché del servidor, lo que significa que se debe volver a compilar el identificador preparado que identifica la instrucción. Los valores de los parámetros *scrollopt* y *ccopt* deben coincidir con los valores enviados en la solicitud original a sp_cursorprepare.  
  
> [!NOTE]  
>  PARAMETERIZED_STMT no deben asignarse a *scrollopt*.  
  
 Si no se pueden proporcionar los valores correspondientes, se provocará la recompilación de los planes, impidiéndose las operaciones de preparación y ejecución.  
  
## <a name="rpc-and-tds-considerations"></a>Consideraciones sobre RPC y TDS  
 La marca de entrada RPC RETURN_METADATA puede establecerse en 1 para solicitar que se devuelvan los metadatos de la lista de selección del cursor en el flujo de TDS.  
  
## <a name="see-also"></a>Consulte también  
 [sp_cursoropen &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
