---
description: Configuración del proyecto (conversión) (SybaseToSQL)
title: Configuración del proyecto (conversión) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eeb80fa5-f530-4f21-beee-25f5a4b8ace6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 66d1ce573f464f14e0b738fb4c461be031db64ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070090"
---
# <a name="project-settings-conversion-sybasetosql"></a>Configuración del proyecto (conversión) (SybaseToSQL)

La página **conversión** del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan el modo en que SSMA convierte la sintaxis de SAP Adaptive Server Enterprise (ASE) en o la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis SQL de Azure.

El panel **conversión** está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** :

- Si desea especificar la configuración de todos los proyectos de SSMA, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **conversión**.

- Para especificar la configuración del proyecto actual, en el menú **herramientas** , seleccione **configuración del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **conversión**.

## <a name="miscellaneous-section"></a>Sección varios

### <a name="error"></a>@@ERROR

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL y ASE usan códigos de error diferentes.

Use esta opción para especificar el tipo de mensaje (ADVERTENCIA o error) que SSMA muestra en el panel de salida o Lista de errores cuando encuentra una referencia a `@@ERROR` en el código de ase.

- Si selecciona **convertir y marcar con ADVERTENCIA**, SSMA convertirá las instrucciones y las marcará con comentarios de advertencia.
- Si selecciona **Mark with error**, SSMA omitirá la conversión y marcará las instrucciones con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Convertir y marcar con ADVERTENCIA|
|Optimistic|Convertir y marcar con ADVERTENCIA|
|Completo|Marcar con error|

### <a name="conversion-of-like-operator"></a>Conversión de operador LIKE

Especifica si se deben convertir los `LIKE` operandos para que coincidan con el comportamiento de SAP ase. El punto es que ASE recorta los espacios en blanco finales en un patrón like. La solución consiste en convertir una conversión de expresión derecha en un tipo de datos de longitud fija con una precisión máxima.
  
- Seleccione **conversión simple** para convertir las expresiones sin ninguna corrección.
- Para usar el comportamiento de ASE, seleccione **conversión a longitud fija.**
  
Al seleccionar un modo de conversión en el cuadro modo, SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Conversión simple|
|Optimistic|Conversión simple|
|Completo|Conversión a longitud fija|

### <a name="convert-or-cast-empty-strings-to-numeric-types"></a>CONVERTIR o convertir cadenas vacías en tipos numéricos

Especifica cómo administrar cadenas en blanco o vacías dentro de `CONVERT` `CAST` expresiones o con el tipo numérico como argumento DataType. Las siguientes opciones están disponibles para esta configuración:

- Seleccione **conversión simple** para convertir las expresiones sin ninguna corrección.
- Si se selecciona **cadena vacía como cero numérico** , el parámetro de cadena se `{s}` reemplazará por la `CASE ltrim(rtrim({s})) WHEN "" THEN 0 else {s} END` expresión.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Conversión simple|
|Optimistic|Conversión simple|
|Completo|Cadena vacía como cero numérico|

### <a name="concatenation-of-null"></a>Concatenación de NULL

Esta configuración especifica cómo convertir la concatenación de cadenas con `NULL` . Se pueden establecer las siguientes opciones para esta configuración determinada:

- Si la opción **Wrap with IsNull function** está seleccionada, todas las constantes `string_expression` de concatenación se ajustarán con `ISNULL(string_expression)` y `NULL` s se reemplazarán con una cadena vacía.
- **Mantener la sintaxis actual** mantendrá la sintaxis original.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Mantener la sintaxis actual|
|Optimistic|Mantener la sintaxis actual|
|Completo|Wrap con la función ISNULL|

### <a name="conversion-of-empty-strings"></a>Conversión de cadenas vacías

Esta configuración especifica cómo se convierten las cadenas vacías. Se pueden establecer las siguientes opciones para esta configuración determinada:

- **Reemplazar todas las expresiones de cadena por espacio**
- **Reemplazar constantes de cadena vacías por espacio**

Para usar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamiento de SQL/Azure, seleccione **mantener la sintaxis actual**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Mantener la sintaxis actual|
|Optimistic|Mantener la sintaxis actual|
|Completo|Reemplazar todas las expresiones de cadena por espacio|

### <a name="convert-and-cast-binary-string-conversion"></a>CONVERTIR y convertir la conversión de cadena binaria

La conversión de valores binarios en números puede devolver valores diferentes en distintas plataformas. Por ejemplo, en procesadores x86, `CONVERT(integer, 0x00000100)` devuelve `65536` en ase, pero `256` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . ASE también devuelve valores distintos según el orden de los bytes.

Utilice esta opción para controlar cómo SSMA convierte `CONVERT` y `CAST` expresiones que contienen valores binarios:

- Seleccione **conversión simple** para convertir las expresiones sin ninguna advertencia o corrección. Utilice esta opción si sabe que el servidor ASE tiene un orden de bytes que no requiere ningún cambio del valor binario.
- Seleccione **convertir y corregir** para que SSMA convierta y corrija las expresiones que se van a usar en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se revertirá el orden de bytes en las constantes literales. Todos los demás valores binarios (como las variables y columnas binarias) se marcarán con errores. Use este valor si sabe que el servidor ASE tiene un orden de bytes que requiere cambios en los valores binarios.

Seleccione **convertir y marcar con ADVERTENCIA** para que SSMA convierta y corrija las expresiones y marque todas las expresiones convertidas con comentarios de advertencia.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Convertir y marcar con ADVERTENCIA|
|Optimistic|Conversión simple|
|Completo|Convertir y corregir|

### <a name="dynamic-sql"></a>SQL dinámico

Use esta opción para especificar el tipo de mensaje (ADVERTENCIA o error) que SSMA muestra en el panel de **salida** o **lista de errores** cuando encuentra SQL dinámico en el código de ase.

- Si selecciona **convertir y marcar con ADVERTENCIA**, SSMA convertirá el SQL dinámico y marcará las instrucciones con comentarios de advertencia.
- Si selecciona **Mark with error**, SSMA omitirá la conversión y marcará las instrucciones con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Convertir y marcar con ADVERTENCIA|
|Optimistic|Convertir y marcar con ADVERTENCIA|
|Completo|Marcar con error|

### <a name="equality-check-conversion"></a>Conversión de comprobación de igualdad

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, si la `ANSI_NULLS` configuración está activada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL devuelve `UNKNOWN` cuando cualquier comparación de igualdad contiene un `NULL` valor. Si `ANSI_NULLS` es OFF, las comparaciones de igualdad que contienen `NULL` valores devuelven TRUE cuando la columna comparada y la expresión o dos expresiones son ambos `NULL` . De forma predeterminada (), las `ANSINULL OFF` comparaciones de igualdad de SAP ase se comportan como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL con `ANSI_NULLS OFF` .

- Si selecciona la **conversión simple**, SSMA convertirá el código de ase en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis SQL de/Azure sin comprobaciones adicionales para `NULL` los valores. Utilice esta opción si `ANSI_NULLS` está `OFF` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL o si desea revisar las comparaciones de igualdad en cada caso.
- Si selecciona **considerar valores NULL**, SSMA agregará las comprobaciones de `NULL` los valores mediante `IS NULL` las `IS NOT NULL` cláusulas y.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Conversión simple|
|Optimistic|Conversión simple|
|Completo|Considerar valores NULL|

### <a name="format-strings"></a>Cadenas de formato

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL ya no admite el `format_string` argumento en `PRINT` las `RAISERROR` instrucciones y. `format_string`Argumento que permite poner parámetros reemplazables directamente en la cadena y, a continuación, reemplazar los parámetros en tiempo de ejecución. En su lugar, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere la cadena completa mediante un literal de cadena o una cadena generada mediante una variable. Para obtener más información, vea el tema [Print ( [!INCLUDE[tsql](../../includes/tsql-md.md)] )](../../t-sql/language-elements/print-transact-sql.md) .

Cuando SSMA encuentra un `format_string` argumento, puede crear un literal de cadena mediante las variables o crear una nueva variable y compilar una cadena utilizando esa variable.

- Para usar un literal de cadena `PRINT` para `RAISERROR` las funciones y, seleccione **crear nueva cadena**.

    En este modo, si una instrucción PRINT o RAISERROR no usa marcadores de posición y variables locales, la instrucción no cambia. Porcentaje de caracteres dobles (%%) se cambian a un solo carácter de porcentaje en literales de cadena de impresión.

    Si una instrucción PRINT o RAISERROR usa marcadores de posición y una o varias variables locales, como en el ejemplo siguiente:

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA lo convertirá a la siguiente sintaxis:

    ```sql
    PRINT 'Total: '+ CAST(@percent AS varchar(max)) + '%'
    ```

    Si `format_string` es una variable, como en la siguiente instrucción:  

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA no puede realizar una conversión de cadena simple y debe crear una nueva variable:

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        CAST (@arg1 AS varchar(max)))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        CAST (@arg2 AS varchar(max)))
    PRINT @print_format_1
    ```

    Cuando se usa **crear nuevo** modo de cadena, SSMA supone que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] opción `CONCAT_NULL_YIELDS_NULL` es `OFF` . Por lo tanto, SSMA no comprueba los argumentos null.

- Para que SSMA cree una nueva variable para cada `PRINT` `RAISERROR` instrucción y y, a continuación, use esa variable para el valor de cadena, seleccione **crear nueva variable**.

    En este modo, si una `PRINT` `RAISERROR` instrucción o no usa marcadores de posición y variables locales, SSMA reemplaza todos los caracteres de porcentaje doble ( `%%` ) por un solo carácter de porcentaje para cumplir con la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis SQL de/Azure.

    Si una `PRINT` `RAISERROR` instrucción o usa marcadores de posición y una o varias variables locales, como en el ejemplo siguiente:

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA lo convertirá a la siguiente sintaxis:

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 = 'Total: %1!%'
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@percent AS VARCHAR(max)), ''))
    PRINT @print_format_1
    ```

    Si `format_string` es una variable, como en la siguiente instrucción:

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA crea una nueva variable como se indica a continuación, comprobando si hay valores NULL en cada argumento:

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@arg1 AS varchar(max)),''))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        ISNULL(CAST (@arg2 AS varchar(max)),''))
    PRINT @print_format_1
    ```

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Crear nueva cadena|
|Optimistic|Crear nueva cadena|
|Completo|Crear nueva variable|

### <a name="insert-an-explicit-value-into-a-timestamp-column"></a>Insertar un valor explícito en una columna de marca de tiempo

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL no admite la inserción de valores explícitos en una columna de marca de tiempo.

- Para excluir columnas de marca de tiempo de las `INSERT` instrucciones, seleccione **excluir columna**.
- Para imprimir un mensaje de error cada vez que una columna TIMESTAMP esté en una `INSERT` instrucción, seleccione **marcar con error**. En este modo, las `INSERT` instrucciones no se convertirán y se marcarán con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Excluir columna|
|Optimistic|Excluir columna|
|Completo|Marcar con error|

### <a name="store-temporary-objects-defined-in-procedures"></a>Almacenar objetos temporales definidos en procedimientos

Esta configuración especifica si las definiciones de los objetos temporales que aparecen en los procedimientos deben almacenarse en los metadatos de origen durante la conversión.

- Seleccione **sí** para almacenar en los metadatos.
- Seleccione **no** si no es necesario almacenar los objetos.

|Modo|Value|
|-|-|
|Valor predeterminado|Sí|
|Optimistic|Sí|
|Completo|No|

### <a name="proxy-table-conversion"></a>Conversión de tabla de proxy

Especifica si las tablas de proxy de ASE se convierten en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas SQL de/Azure, o no se convierten y el código está marcado con comentarios de error.

- Seleccione **convertir** para convertir las tablas de proxy en tablas normales.
- Seleccione **marcar con error** para marcar simplemente el código de la tabla de proxy con los comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Marcar con error|
|Optimistic|Marcar con error|
|Completo|Marcar con error|

### <a name="raiserror-base-message-number"></a>Número de mensaje base de RAISERROR

Los mensajes de usuario de ASE se almacenan en cada base de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los mensajes de usuario se almacenan de forma centralizada y se ponen a disposición a través de la `sys.messages` vista de catálogo. Además, los mensajes de usuario de ASE se inician en `20000` , pero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los mensajes de error se inician en `50001` .

Esta configuración especifica el número que se va a agregar al número de mensaje de usuario de ASE para convertirlo en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mensaje de usuario. Si su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene mensajes de usuario en la `sys.messages` vista de catálogo, es posible que tenga que cambiar este número a un valor superior. Esto es así para que los números de mensaje convertidos no entren en conflicto con los números de mensaje existentes.

Tenga en cuenta lo siguiente:
  
- Los mensajes ase en el intervalo `17000` - `19999` proceden de la `sysmessages` tabla del sistema y no se convierten.
- Si el número de mensaje al que se hace referencia en la `RAISERROR` instrucción es una constante, SSMA agregará el número de mensaje base a la constante para determinar el nuevo número de mensaje de usuario.
- Si el número de mensaje al que se hace referencia es una variable o una expresión, SSMA creará una variable local intermedia.
- En el **modo optimista**, SSMA supone que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] opción `CONCAT_NULL_YIELDS_NULL` es `OFF` y no realiza comprobaciones de los `NULL` argumentos.
- En el **modo completo**, SSMA comprueba los `NULL` argumentos.
- `RAISERROR` el `arg-list` argumento with no se convierte.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|30001|
|Optimistic|30001|
|Completo|30001|

### <a name="system-objects"></a>Objetos del sistema

Use esta opción para especificar el tipo de mensaje (ADVERTENCIA o error) que SSMA muestra en el panel de **salida** o **lista de errores** cuando encuentra el uso de objetos del sistema ase.

- Si selecciona **convertir y marcar con ADVERTENCIA**, SSMA convertirá las referencias a los objetos del sistema y marcará las instrucciones con comentarios de advertencia.
- Si selecciona **Mark with error**, SSMA no convertirá las referencias a objetos de sistema y marcará las instrucciones con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Convertir y marcar con ADVERTENCIA|
|Optimistic|Convertir y marcar con ADVERTENCIA|
|Completo|Marcar con error|

### <a name="unresolved-identifiers"></a>Identificadores sin resolver

Use esta opción para especificar el tipo de mensaje (ADVERTENCIA o error) que SSMA muestra en el panel de **salida** o **lista de errores** cuando no puede resolver un identificador.

- Si selecciona **convertir y marcar con ADVERTENCIA**, SSMA intentará convertir las referencias a identificadores sin resolver y marcará las instrucciones con comentarios de advertencia.
- Si selecciona **Mark with error**, SSMA no convertirá las referencias a identificadores sin resolver y marcará las instrucciones con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Convertir y marcar con ADVERTENCIA|
|Optimistic|Convertir y marcar con ADVERTENCIA|
|Completo|Marcar con error|

## <a name="system-functions-section"></a>Sección funciones del sistema

### <a name="charindex-function"></a>CHARINDEX, función

En ASE, `CHARINDEX` devuelve `NULL` solo si todas las expresiones de entrada son `NULL` . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL devolverá `NULL` si alguna expresión de entrada es `NULL` .

- Para usar el comportamiento de ASE, seleccione **reemplazar función**. Todas las llamadas a `CHARINDEX` la función se sustituyen por una llamada a o a una `CHARINDEX_VARCHAR`  `CHARINDEX_NVARCHAR` función definida por el usuario según el tipo de parámetros pasados (creados en la base de datos de usuario bajo el nombre del esquema `s2ss` ) para emular el comportamiento de SAP ase.
- Para usar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamiento de SQL/Azure, seleccione **mantener la sintaxis actual**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Mantener la sintaxis actual|
|Optimistic|Mantener la sintaxis actual|
|Completo|Replace (función)|
  
### <a name="datalength-function"></a>DATALENGTH, función

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL y ASE difieren en el valor devuelto por la `DATALENGTH` función cuando el valor es un espacio único. En este caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL devuelve `0` y ase devuelve `1` .

- Para usar el comportamiento de ASE, seleccione **reemplazar función**. Todas las llamadas a `DATALENGTH` function se sustituyen por `CASE` Expression para emular el comportamiento de SAP ase.
- Para usar el comportamiento predeterminado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, seleccione **mantener la sintaxis actual**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Mantener la sintaxis actual|
|Optimistic|Mantener la sintaxis actual|
|Completo|Replace (función)|

### <a name="index_col-function"></a>INDEX_COL, función

ASE es compatible con un `user_id` argumento opcional de la `INDEX_COL` función; sin embargo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL no admite este argumento. Si usa el `user_id` argumento, esta función no se puede convertir en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis SQL de/Azure.

- Para usar el comportamiento de ASE, seleccione **convertir función**. Si el código contiene el `user_id` argumento, SSMA mostrará un error.
- Para mostrar un mensaje de error cada vez que `INDEX_COL` se encuentre, seleccione **marcar con error**. SSMA no convertirá las referencias a la función y marcará la instrucción con comentarios de error.

|Modo|Value|
|-|-|
|Valor predeterminado|Marcar con error|
|Optimistic|Marcar con error|
|Completo|Marcar con error|

### <a name="index_colorder-function"></a>INDEX_COLORDER función)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL no tiene una `INDEX_COLORDER` función del sistema.

- Para usar el comportamiento de ASE, seleccione **convertir función**. Todas las llamadas a `INDEX_COLORDER` la función se sustituyen por una llamada a una función definida por el usuario con el mismo nombre `INDEX_COLORDER` (creada en la base de datos de usuario con el nombre de esquema `s2ss` ) que emula el comportamiento de SAP ase.
- Para imprimir un mensaje de error cada vez que `INDEX_COLORDER` se encuentre, seleccione **marcar con error**. SSMA no convertirá las referencias a la función y marcará la instrucción con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Marcar con error|
|Optimistic|Marcar con error|
|Completo|Marcar con error|

### <a name="left-and-right-functions"></a>Funciones izquierda y derecha

`LEFT``RIGHT`las funciones y de ase se comportan de manera diferente para el parámetro de longitud negativa.

- Para usar el comportamiento de ASE, seleccione **reemplazar función**. A continuación, el parámetro de longitud se reemplaza por una `CASE` expresión que devolvería `NULL` para un valor negativo.
- Para usar el comportamiento de SQL Server, seleccione **mantener la sintaxis actual**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Mantener la sintaxis actual|
|Optimistic|Mantener la sintaxis actual|
|Completo|Replace (función)|

> [!NOTE]
> Si el parámetro de longitud es un valor literal y no una expresión compleja, el valor de longitud siempre se reemplaza con `NULL` independientemente de la configuración del proyecto.

### <a name="next_identity-function"></a>NEXT_IDENTITY función)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL no tiene una `NEXT_IDENTITY` función del sistema.

- Para usar el comportamiento de ASE, seleccione **convertir función**. Todas las llamadas a `NEXT_IDENTITY` la función se sustituyen por una expresión `(IDENT_CURRENT(parameter Value) + IDENT_INCR(parameter Value)` que emula el comportamiento de SAP ase.
- Para imprimir un mensaje de error cada vez que `NEXT_IDENTITY` se encuentre, seleccione **marcar con error**. SSMA no convertirá las referencias a la función y marcará la instrucción con comentarios de error.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Marcar con error|
|Optimistic|Marcar con error|
|Completo|Marcar con error|

**Modo predeterminado/optimista/completo:** Marcar con error

### <a name="patindex-function"></a>PATINDEX, función

Especifica si se va a convertir la `PATINDEX` función para que coincida con el comportamiento de SAP ase. El punto es que ASE recorta los espacios en blanco finales en un patrón de búsqueda. La solución consiste en convertir una conversión de expresión de valor en un tipo de datos de longitud fija con una precisión máxima y aplicar la `rtrim` función al patrón de búsqueda.

- Para usar el comportamiento de ASE, seleccione **usar**.  
- Para usar el comportamiento predeterminado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, seleccione **no usar**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|No debe usarse|
|Optimistic|No debe usarse|
|Completo|Uso|

### <a name="replicate-function"></a>REPLICATE, función

La `REPLICATE` función repite una cadena el número especificado de veces. En ASE, si especifica que se repita la cadena cero veces, el resultado es `NULL` . En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, el resultado es una cadena vacía.

- Para usar el comportamiento de ASE, seleccione **reemplazar función**. Todas las llamadas a `REPLICATE` la función se sustituyen por una llamada a o a una `REPLICATE_VARCHAR` `REPLICATE_NVARCHAR` función definida por el usuario según el tipo de parámetros pasados (creados en la base de datos de usuario bajo el nombre del esquema `s2ss` ) para emular el comportamiento de SAP ase.
- Para usar el comportamiento predeterminado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, seleccione **reemplazar función**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Replace (función)|
|Optimistic|Replace (función)|
|Completo|Replace (función)|

### <a name="trim-ltrim-rtrim-function"></a>TRIM (LTRIM, RTRIM) (función)

Esta configuración especifica si se deben reemplazar las llamadas a `TRIM` `LTRIM` y `RTRIM` las funciones con las funciones de sintaxis equivalentes de SAP ase o para mantener la sintaxis actual. Las siguientes opciones están presentes para esta configuración determinada:

- **Replace (función)**
- **Mantener la sintaxis actual**

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Replace (función)|
|Optimistic|Replace (función)|
|Completo|Replace (función)|

### <a name="substring-function"></a>SUBSTRING, función

En ASE, la función `SUBSTRING(expression, start, length)` devuelve `NULL` si se especifica un valor de inicio mayor que el número de caracteres de la expresión, o si la longitud es igual a cero. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, la expresión equivalente devuelve una cadena vacía.

- Para usar el comportamiento de ASE, seleccione **reemplazar función**. Todas las llamadas a `SUBSTRING` la función se sustituyen por una llamada a o a una `SUBSTRING_VARCHAR` `SUBSTRING_NVARCHAR` `SUBSTRING_VARBINARY` función definida por el usuario según el tipo de parámetros pasados (creados en la base de datos de usuario bajo el nombre del esquema `s2ss` ) para EMULAR el comportamiento de SAP ase.
- Para usar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamiento de SQL/Azure, seleccione **mantener la sintaxis actual**.

Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:

|Modo|Value|
|-|-|
|Valor predeterminado|Mantener la sintaxis actual|
|Optimistic|Mantener la sintaxis actual|
|Completo|Replace (función)|

## <a name="tables-section"></a>Sección tablas

### <a name="add-primary-key"></a>Agregar clave principal

Crea una nueva clave principal en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla o de Azure SQL si una tabla de SAP ase no tiene una clave principal o un índice único.

|Modo|Value|
|-|-|
|Valor predeterminado|No|
|Optimistic|No|
|Completo|Sí|

> [!NOTE]
> Cuando se conecta a Azure SQL, es **sí** de forma predeterminada.

## <a name="see-also"></a>Consulte también

[Referencia de la interfaz de usuario (SybaseToSQL)](../../ssma/sybase/user-interface-reference-sybasetosql.md)
