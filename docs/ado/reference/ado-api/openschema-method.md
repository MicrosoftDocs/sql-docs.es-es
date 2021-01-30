---
description: Método OpenSchema
title: Método OpenSchema | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::OpenSchema
- Connection15::raw_OpenSchema
helpviewer_keywords:
- OpenSchema method [ADO]
ms.assetid: 850cf3ce-f18f-4e7c-8597-96c1dc504866
author: rothja
ms.author: jroth
ms.openlocfilehash: 9b5ca92e42a68639f6b9e3044894ec779109fff5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170674"
---
# <a name="openschema-method"></a>Método OpenSchema
Obtiene la información del esquema de la base de datos del proveedor.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set recordset = connection.OpenSchema(QueryType, Criteria, SchemaID)  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un objeto de [conjunto de registros](./recordset-object-ado.md) que contiene información de esquema. El **conjunto de registros** se abrirá como un cursor estático de solo lectura. El *QueryType* determina qué columnas aparecen en el **conjunto de registros**.  
  
#### <a name="parameters"></a>Parámetros  
 *QueryType*  
 Cualquier valor de [SchemaEnum](./schemaenum.md) que represente el tipo de consulta de esquema que se va a ejecutar.  
  
 *Criterios*  
 Opcional. Una matriz de restricciones de consulta para cada opción de *QueryType* , tal como se muestra en [SchemaEnum](./schemaenum.md).  
  
 *SchemaID*  
 GUID para una consulta de esquema de proveedor no definida por la especificación OLE DB. Este parámetro es necesario si *QueryType* está establecido en **adSchemaProviderSpecific**; de lo contrario, no se utiliza.  
  
## <a name="remarks"></a>Observaciones  
 El método **OpenSchema** devuelve información autodescriptiva sobre el origen de datos, como las tablas que se encuentran en el origen de datos, las columnas de las tablas y los tipos de datos admitidos.  
  
 El argumento *QueryType* es un GUID que indica las columnas (esquemas) devueltas. La especificación OLE DB tiene una lista completa de esquemas.  
  
 El argumento *criteria* limita los resultados de una consulta de esquema. *Criterios* especifica una matriz de valores que se debe producir en un subconjunto correspondiente de columnas, denominadas columnas de restricción, en el **conjunto de registros** resultante.  
  
 La constante **adSchemaProviderSpecific** se usa para el argumento *QueryType* si el proveedor define sus propias consultas de esquema no estándar fuera de las mencionadas anteriormente. Cuando se usa esta constante, se necesita el argumento *SchemaID* para pasar el GUID de la consulta de esquema que se va a ejecutar. Si *QueryType* se establece en **adSchemaProviderSpecific** , pero no se proporciona *SchemaID* , se producirá un error.  
  
 No es necesario que los proveedores admitan todas las consultas de esquema estándar de OLE DB. En concreto, la especificación OLE DB requiere solo **adSchemaTables**, **adSchemaColumns** y **adSchemaProviderTypes** . Sin embargo, no es necesario que el proveedor admita las restricciones de *criterios* enumeradas anteriormente para esas consultas de esquema.  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** El método **OpenSchema** no está disponible en un objeto de [conexión](./connection-object-ado.md) del lado cliente.  
  
> [!NOTE]
>  En Visual Basic, las columnas que tienen un entero sin signo de cuatro bytes (DBTYPE UI4) en el **conjunto de registros** devuelto desde el método **OpenSchema** en el objeto de **conexión** no se pueden comparar con otras variables. Para obtener más información sobre los tipos de datos de OLE DB, vea [tipos de datos en OLE DB (OLE DB)](/previous-versions/windows/desktop/ms714931(v=vs.85)) y [Apéndice A: tipos de datos](/previous-versions/windows/desktop/ms723969(v=vs.85)) en la referencia del programador de Microsoft OLE DB.  
  
> [!NOTE]
>  **Usuarios de Visual C/C++** Cuando no se utilizan cursores del lado cliente, la recuperación del "ORDINAL_POSITION" de un esquema de columna en ADO devuelve una variante de tipo VT_R8 en MDAC 2,7, MDAC 2,8 y Windows Data Access Components (Windows DAC) 6,0, mientras que el tipo utilizado en MDAC 2,6 se VT_I4. Programas escritos para MDAC 2,6 que solo buscan una variante devuelta de tipo VT_I4 obtendría un cero para cada ordinal si se ejecuta en MDAC 2,7, MDAC 2,8 y Windows DAC 6,0 sin modificarlo. Este cambio se realizó porque el tipo de datos que OLE DB devuelve es DBTYPE_UI4 y en el tipo de VT_I4 con signo no hay espacio suficiente para contener todos los valores posibles sin que se produzca un truncamiento y, por lo tanto, se produce una pérdida de datos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método OpenSchema (VB)](./openschema-method-example-vb.md)   
 [Ejemplo del método OpenSchema (VC + +)](./openschema-method-example-vc.md)   
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)   
 [Open (método) (record de ADO)](./open-method-ado-record.md)   
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)   
 [Open (método, secuencia de ADO)](./open-method-ado-stream.md)   
 [Apéndice A: Proveedores](../../guide/appendixes/appendix-a-providers.md)