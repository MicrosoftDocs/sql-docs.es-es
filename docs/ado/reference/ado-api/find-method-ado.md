---
description: Find (método) (ADO)
title: Find (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Find
- Recordset15::Find
helpviewer_keywords:
- Find method [ADO]
ms.assetid: 55c9810a-d8ca-46c2-a9dc-80e7ee7aa188
author: rothja
ms.author: jroth
ms.openlocfilehash: 8f581b2c4b4861c852a9e5a24aec5a219ee098f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167319"
---
# <a name="find-method-ado"></a>Find (método) (ADO)
Busca en un [conjunto de registros](./recordset-object-ado.md) la fila que cumple los criterios especificados. Opcionalmente, se puede especificar la dirección de la búsqueda, la fila inicial y el desplazamiento de la fila inicial. Si se cumplen los criterios, la posición de la fila actual se establece en el registro encontrado; de lo contrario, la posición se establece en el final (o en el inicio) del **conjunto de registros**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Find (Criteria, SkipRows, SearchDirection, Start)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Criterios*  
 Valor de **cadena** que contiene una instrucción que especifica el nombre de columna, el operador de comparación y el valor que se van a usar en la búsqueda.  
  
 *SkipRows*  
 Opcional. Un valor **Long** , cuyo valor predeterminado es cero, que especifica el desplazamiento de fila desde la fila actual o el marcador de *Inicio* para comenzar la búsqueda. De forma predeterminada, la búsqueda se iniciará en la fila actual.  
  
 *SearchDirection*  
 Opcional. Valor [SearchDirectionEnum](./searchdirectionenum.md) que especifica si la búsqueda debe comenzar en la fila actual o en la siguiente fila disponible en la dirección de la búsqueda. Una búsqueda incorrecta se detiene al final del **conjunto de registros** si el valor es **adSearchForward**. Una búsqueda incorrecta se detiene al principio del **conjunto de registros** si el valor es **adSearchBackward**.  
  
 *Iniciar*  
 Opcional. Marcador de **variante** que funciona como la posición inicial de la búsqueda.  
  
## <a name="remarks"></a>Observaciones  
 En los *criterios* solo se puede especificar un nombre de una sola columna. Este método no admite búsquedas en varias columnas.  
  
 El operador de comparación de *criterios* puede ser " **>** " (mayor que), "* * \<**" (less than), "=" (equal), "> =" (mayor o igual que), "<=" (menor o igual que), "<>" (no igual) o "like" (coincidencia de patrones).  
  
 El valor de *criterios* puede ser una cadena, un número de punto flotante o una fecha. Los valores de cadena se delimitan entre comillas simples o "#" (signo de número) (por ejemplo, "State = ' WA '" o "State = #WA #"). Los valores de fecha se delimitan con marcas "#" (signo de número) (por ejemplo, "start_date > #7/22/97 #"). Estos valores pueden contener horas, minutos y segundos para indicar las marcas de tiempo, pero no deben contener milisegundos o se producirán errores.  
  
 Si el operador de comparación es "like", el valor de cadena puede contener un asterisco (*) para buscar una o más apariciones de cualquier carácter o subcadena. Por ejemplo, "State like \* '" coincide con Maine y Massachusetts. También puede usar asteriscos iniciales y finales para buscar una subcadena incluida dentro de los valores. Por ejemplo, "State like ' \* as \* '" coincide con Alaska, Arkansas y Massachusetts.  
  
 Los asteriscos solo se pueden usar al final de una cadena de criterios, o al principio y al final de una cadena de criterios, como se mostró anteriormente. No se puede usar el asterisco como carácter comodín inicial (' * str ') o como carácter comodín incrustado ( \* r '). Esto producirá un error.  
  
> [!NOTE]
>  Se producirá un error si no se establece una posición de fila actual antes de llamar a **Find**. Se debe llamar a cualquier método que establezca la posición de la fila, como [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), antes de llamar a **Find**.  
  
> [!NOTE]
>  Si llama al método **Find** en un conjunto de registros y la posición actual en el conjunto de registros está en el último registro o final de archivo (EOF), no encontrará nada. Debe llamar al método **MoveFirst** para establecer la posición o el cursor actual al principio del conjunto de registros.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método Find (VB)](./find-method-example-vb.md)   
 [Index (propiedad)](./index-property.md)   
 [Optimizar Property-Dynamic (ADO)](./optimize-property-dynamic-ado.md)   
 [El método de búsqueda](./seek-method.md)