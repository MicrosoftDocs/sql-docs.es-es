---
description: Configuración del proyecto (conversión) (DB2ToSQL)
title: Configuración del proyecto (conversión) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 538c93cf-c5bb-43d5-b758-186d9fb00c19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5d01a893a2284a85fd4540d532500b7915a97326
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072349"
---
# <a name="project-settings-conversion-db2tosql"></a>Configuración del proyecto (conversión) (DB2ToSQL)
La página conversión del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan cómo SSMA convierte la sintaxis de DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis.  
  
El panel conversión está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** :  
  
-   Para especificar la configuración de todos los proyectos de SSMA, en el menú **herramientas** , haga clic en **configuración de proyecto predeterminada**, seleccione tipo de proyecto de migración para el que se deben ver o cambiar valores de configuración en la lista desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **conversión**.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , haga clic en **configuración del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **conversión**.  
  
## <a name="conversion-messages"></a>Mensajes de conversión  
  
### <a name="generate-messages-about-issues-applied"></a>Generar mensajes sobre problemas aplicados  
Especifica si SSMA genera mensajes informativos durante la conversión, los muestra en el panel de salida y los agrega al código convertido.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** No  
  
**Modo completo:** No  
  
## <a name="miscellaneous-options"></a>Otras opciones  
  
### <a name="cast-rownum-expressions-as-integers"></a>Conversión de expresiones de ROWNUM como enteros  
Cuando SSMA convierte expresiones ROWNUM, convierte la expresión en una cláusula TOP, seguida de la expresión. En el ejemplo siguiente se muestra ROWNUM en una instrucción DELETE de DB2:  
  
`DELETE FROM Table1`  
  
`WHERE ROWNUM < expression and Field1 >= 2`  
  
En el ejemplo siguiente se muestra el resultado [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
`DELETE TOP (expression-1)`  
  
`FROM Table1`  
  
`WHERE Field1>=2`  
  
La parte superior requiere que la expresión de las cláusulas TOP se evalúe como un entero. Si el entero es negativo, la instrucción generará un error.  
  
-   Si selecciona **sí**, SSMA convierte la expresión en un entero.  
  
-   Si selecciona **no**, SSMA marcará todas las expresiones no enteras como un error en el código convertido.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/completo:** No  
  
**Modo optimista:** ?  
  
### <a name="default-schema-mapping"></a>Asignación de esquemas predeterminada  
Esta configuración especifica cómo se asignan los esquemas de DB2 a los esquemas de SQL Server. En esta configuración están disponibles dos opciones:  
  
1.  **Esquema a base de datos:** En este modo, el esquema DB2 ' sch1 ' se asignará de forma predeterminada a ' DBO ' SQL Server esquema en SQL Server base de datos ' sch1 '.  
  
2.  **Esquema a esquema:** En este modo, el esquema DB2 ' sch1 ' se asignará de forma predeterminada a ' sch1 ' SQL Server esquema en la base de datos de SQL Server predeterminada que se proporciona en el cuadro de diálogo de conexión.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** Esquema a base de datos  
  
### <a name="conversion-ways-of-merge-statement"></a>Formas de conversión de la instrucción MERGE  
  
-   Si selecciona **usar la instrucción INSERT, Update, Delete**, SSMA convierte la instrucción de fusión en instrucciones INSERT, Update y DELETE.  
  
-   Si selecciona **usar instrucción Merge**, SSMA convierte la instrucción de fusión en instrucción Merge en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!WARNING]  
> Esta opción de configuración del proyecto solo está disponible en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** Usar la instrucción MERGE  
  
### <a name="convert-calls-to-subprograms-that-use-default-arguments"></a>Convertir llamadas a subprogramas que usan argumentos predeterminados  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las funciones no admiten la omisión de parámetros en la llamada de función. Además, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las funciones y los procedimientos no admiten expresiones como valores de parámetros predeterminados.  
  
-   Si selecciona **sí** y una llamada de función omite los parámetros, SSMA insertará la palabra clave **default** en la función y llamará a en la posición correcta. A continuación, marcará la llamada con una advertencia.  
  
-   Si selecciona **no**, SSMA marcará las llamadas de función como errores.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-count-function-to-count_big"></a>Convertir la función COUNT a COUNT_BIG  
Si es probable que las funciones de recuento devuelvan valores mayores que 2.147.483.647, que es 2<sup>31</sup>-1, debe convertir las funciones en COUNT_BIG.  
  
-   Si selecciona **sí**, SSMA convertirá todos los usos de COUNT a COUNT_BIG.  
  
-   Si selecciona **no**, las funciones permanecerán como recuento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá un error si la función devuelve un valor mayor que 2<sup>31</sup>-1.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/completo:** ?  
  
**Modo optimista:** No  
  
### <a name="convert-forall-statement-to-while-statement"></a>Convertir la instrucción FORALl a WHILE (instrucción)  
Define cómo SSMA tratará los bucles de FORALl en los elementos de la colección de PL/SQL.  
  
-   Si selecciona **sí**, SSMA crea un bucle while donde los elementos de la colección se recuperan uno a uno.  
  
-   Si selecciona **no**, SSMA genera un conjunto de filas a partir de la colección mediante el método Nodes () y lo usa como una sola tabla. Esto es más eficaz, pero hace que el código de salida sea menos legible.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** No  
  
**Modo completo:** ?  
  
### <a name="convert-foreign-keys-with-set-null-referential-action-on-column-that-is-not-null"></a>Convertir claves externas con la acción referencial SET NULL en una columna que no sea NULL  
DB2 permite la creación de restricciones Foreign Key, donde no se puede realizar una acción SET NULL porque no se permiten valores NULL en la columna a la que se hace referencia. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no permite esta configuración de clave externa.  
  
-   Si selecciona **sí**, SSMA generará acciones referenciales como en DB2, pero tendrá que realizar cambios manuales antes de cargar la restricción en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Por ejemplo, puede elegir ninguna acción en lugar de establecer NULL.  
  
-   Si selecciona **no**, la restricción se marcará como un error.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** No  
  
### <a name="convert-function-calls-to-procedure-calls"></a>Convertir llamadas de función a llamadas a procedimientos  
Algunas funciones de DB2 se definen como transacciones autónomas o contienen instrucciones que no serían válidas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En estos casos, SSMA crea un procedimiento y una función que es un contenedor para el procedimiento. La función convertida llama al procedimiento de implementación.  
  
SSMA puede convertir las llamadas a la función contenedora en llamadas al procedimiento. Esto crea código más legible y puede mejorar el rendimiento. Sin embargo, el contexto no siempre lo permite; por ejemplo, no puede reemplazar una llamada de función en la lista de selección con una llamada a procedimiento. SSMA tiene algunas opciones para cubrir los casos comunes:  
  
-   Si selecciona **Always**, SSMA intenta convertir las llamadas de función de contenedor en llamadas a procedimiento. Si el contexto actual no permite esta conversión, se genera un mensaje de error. De este modo, no se deja ninguna llamada de función en el código generado.  
  
-   Si selecciona **siempre que sea posible**, SSMA realiza una transición a procedimiento solo si la función tiene parámetros de salida. Cuando no es posible el movimiento, se quita el atributo de salida del parámetro. En todos los demás casos, SSMA abandona las llamadas de función.  
  
-   Si selecciona **nunca**, SSMA dejará todas las llamadas de función como llamadas a funciones. A veces, esta opción puede ser inaceptable debido a las razones de rendimiento.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** Cuando sea posible  
  
### <a name="convert-lock-table-statements"></a>Convertir instrucciones de bloqueo de tabla  
SSMA puede convertir muchas instrucciones de tabla LOCK en sugerencias de tabla. SSMA no puede convertir ninguna instrucción LOCK TABLE que contenga las cláusulas PARTITION, subpartition, @dblink y nowait, y marcará dichas instrucciones con mensajes de error de conversión.  
  
-   Si selecciona **sí**, SSMA convertirá las instrucciones de tabla de bloqueo admitidas en sugerencias de tabla.  
  
-   Si selecciona **no**, SSMA marcará todas las instrucciones de bloqueo de tabla con mensajes de error de conversión.  
  
En la tabla siguiente se muestra cómo SSMA convierte los modos de bloqueo de DB2:  
  
|Modo de bloqueo de DB2|SQL Server sugerencia de tabla|  
|-|-|  
|RECURSO COMPARTIDO DE FILAS|ROWLOCK, HOLDLOCK|  
|FILA EXCLUSIVA|ROWLOCK, XLOCK, HOLDLOCK|  
|COMPARTIR ACTUALIZACIÓN = RECURSO COMPARTIDO DE FILAS|ROWLOCK, HOLDLOCK|  
|COMPARTIR|TABLOCK, HOLDLOCK|  
|COMPARTIR FILA EXCLUSIVA|TABLOCK, XLOCK, HOLDLOCK|  
|ÚNICO|TABLOCKX, HOLDLOCK|  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-open-for-statements-for-ref-cursor-out-parameters"></a>Convertir instrucciones OPEN-FOR para parámetros REF CURSOR OUT  
En DB2, la instrucción OPEN-FOR se puede usar para devolver un conjunto de resultados al parámetro OUT de un subprograma de tipo REF CURSOR. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los procedimientos almacenados devuelven directamente los resultados de las instrucciones SELECT.  
  
SSMA puede convertir muchas instrucciones OPEN-FOR en instrucciones SELECT.  
  
-   Si selecciona **sí**, SSMA convierte la instrucción Open-for en una instrucción SELECT, que devuelve el conjunto de resultados al cliente.  
  
-   Si selecciona **no**, SSMA generará un mensaje de error en el código convertido y en el panel de salida.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-record-as-a-list-of-separates-variables"></a>Convertir el registro en una lista de variables independientes  
SSMA puede convertir registros DB2 en variables independientes y en variables XML con una estructura específica.  
  
-   Si selecciona **sí**, SSMA convierte el registro en una lista de variables independientes siempre que sea posible.  
  
-   Si selecciona **no**, SSMA convierte el registro en variables XML con una estructura específica.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-substr-function-calls-to-substring-function-calls"></a>Convertir las llamadas de función SUBSTR a llamadas a funciones subcadenas  
SSMA puede convertir las llamadas a funciones SUBSTR [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de DB2 en llamadas a funciones **subcadenas** , en función del número de parámetros. Si SSMA no puede convertir una llamada a la función SUBSTR o no se admite el número de parámetros, SSMA convertirá la llamada a la función SUBSTR en una llamada a la función personalizada SSMA.  
  
-   Si selecciona **sí**, SSMA convertirá las llamadas a funciones substr que usan tres parámetros en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **SUBSTRING**. Otras funciones SUBSTR se convertirán para llamar a la función de SSMA personalizada.  
  
-   Si selecciona **no**, SSMA convertirá la llamada de función substr en una llamada a la función personalizada SSMA.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** ?  
  
**Modo completo:** No  
  
### <a name="convert-subtypes"></a>Convertir subtipos  
SSMA puede convertir subtipos de PL/SQL de dos maneras:  
  
-   Si selecciona **sí**, SSMA creará [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un tipo definido por el usuario a partir de un subtipo y lo utilizará para cada variable de este subtipo.  
  
-   Si selecciona **no**, SSMA sustituye todas las declaraciones de origen del subtipo por el tipo subyacente y convierte el resultado como de costumbre. En este caso, no se crea ningún tipo adicional en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** No  
  
### <a name="convert-synonyms"></a>Convertir sinónimos  
Los sinónimos para los siguientes objetos de DB2 se pueden migrar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Tablas y tablas de objetos  
  
-   Vistas y vistas de objetos  
  
-   Procedimientos almacenados y funciones  
  
-   Vistas materializadas  
  
Los sinónimos para los siguientes objetos de DB2 se pueden reemplazar por referencias directas a los objetos:  
  
-   Secuencias  
  
-   Paquetes  
  
-   Objetos de esquema de clase Java  
  
-   Tipos de objetos definidos por el usuario  
  
No se pueden migrar otros sinónimos. SSMA generará mensajes de error para el sinónimo y todas las referencias que utilicen el sinónimo.  
  
-   Si selecciona **sí**, SSMA creará [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sinónimos y referencias a objetos directos según las listas anteriores.  
  
-   Si selecciona **no**, SSMA creará referencias a objetos directos para todos los sinónimos que se enumeran aquí.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-to_chardate-format"></a>Convertir TO_CHAR (Date, Format)  
SSMA puede convertir DB2 TO_CHAR (Date, Format) en procedimientos de la base de datos sysdb.  
  
-   Si selecciona **usar TO_CHAR_DATE función**, SSMA convierte el TO_CHAR (Date, Format) en TO_CHAR_DATE función utilizando el idioma inglés para la conversión.  
  
-   Si selecciona la **función de TO_CHAR_DATE_LS (la ayuda de NLS)**, SSMA convierte el TO_CHAR (Date, Format) en TO_CHAR_DATE_LS función utilizando el idioma de la sesión para la conversión.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** Usar TO_CHAR_DATE función)  
  
**Modo completo:** Usar TO_CHAR_DATE_LS función (precaución de NLS)  
  
### <a name="convert-transaction-processing-statements"></a>Convertir instrucciones de procesamiento de transacciones  
SSMA puede convertir instrucciones de procesamiento de transacciones DB2:  
  
-   Si selecciona **sí**, SSMA convierte las instrucciones de procesamiento de transacciones de DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instrucciones.  
  
-   Si selecciona **no**, SSMA marca las instrucciones de procesamiento de transacciones como errores de conversión.  
  
> [!NOTE]  
> DB2 abre las transacciones implícitamente. Para emular este comportamiento en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe agregar instrucciones BEGIN TRANSACTION manualmente donde desee que se inicien las transacciones. Como alternativa, puede ejecutar el comando SET IMPLICIT_TRANSACTIONS ON al principio de la sesión. SSMA agrega SET IMPLICIT_TRANSACTIONS ON automáticamente al convertir subrutinas con transacciones autónomas.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="emulate-db2-null-behavior-in-order-by-clauses"></a>Emular el comportamiento nulo de DB2 en cláusulas ORDER BY  
Los valores NULL se ordenan de manera diferente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y DB2:  
  
-   En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los valores NULL son los valores más bajos de una lista ordenada. En una lista ascendente, los valores NULL aparecerán en primer lugar.  
  
-   En DB2, los valores NULL son los valores más altos de una lista ordenada. De forma predeterminada, los valores NULL aparecen en último lugar en una lista de orden ascendente.  
  
-   DB2 tiene valores NULL en la cláusula FIRST y NULLs LAST, lo que permite cambiar el modo en que DB2 ordena los valores NULL.  
  
SSMA puede emular el comportamiento de orden de DB2 mediante la comprobación de valores NULL. A continuación, primero ordena por valores NULL en el orden especificado y, a continuación, ordena por otros valores.  
  
-   Si selecciona **sí**, SSMA convertirá la instrucción DB2 de forma que Emule el comportamiento de orden de DB2 por.  
  
-   Si selecciona **no**, SSMA omitirá las reglas de DB2 y generará un mensaje de error cuando encuentre las cláusulas First y Nulls Last.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** No  
  
**Modo completo:** ?  
  
### <a name="emulate-row-count-exceptions-in-select"></a>Emular excepciones de recuento de filas en SELECT  
Si una instrucción SELECT con una cláusula INTO no devuelve ninguna fila, DB2 genera una excepción NO_DATA_FOUND. Si la instrucción devuelve dos o más filas, se produce la excepción TOO_MANY_ROWS. La instrucción convertida en no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera ninguna excepción si el recuento de filas es diferente de uno.  
  
-   Si selecciona **sí**, SSMA agrega una llamada al procedimiento sysdb db_error_exact_one_row_check después de cada instrucción SELECT. Este procedimiento emula las excepciones de NO_DATA_FOUND y TOO_MANY_ROWS. Este es el valor predeterminado y permite reproducir el comportamiento de DB2 lo más cerca posible. Siempre debe elegir **sí** si el código fuente tiene controladores de excepciones que procesan estos errores. Tenga en cuenta que si la instrucción SELECT aparece dentro de una función definida por el usuario, este módulo se convertirá en un procedimiento almacenado, ya que la ejecución de procedimientos almacenados y la generación de excepciones no es compatible con el contexto de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] función.  
  
-   Si selecciona **no**, no se generará ninguna excepción. Esto puede ser útil cuando SSMA convierte una función definida por el usuario y desea que permanezca en una función. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="generate-error-for-dbms_sqlparse"></a>Generar error para DBMS_SQL. ANALIZA  
  
-   Si selecciona **error**, SSMA genera un error en la DBMS_SQL de conversión. Analiza.  
  
-   Si selecciona **ADVERTENCIA**, SSMA generará una advertencia en la DBMS_SQL de conversión. Analiza.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** Error  
  
### <a name="generate-rowid-column"></a>Generar columna ROWID  
Cuando SSMA crea tablas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede crear una columna ROWID. Cuando se migran los datos, cada fila obtiene un nuevo valor UNIQUEIDENTIFIER generado por la función NEWID ().  
  
-   Si selecciona **sí**, la columna ROWID se crea en todas las tablas y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera GUID a medida que se insertan valores. Elija siempre **sí** si tiene previsto usar el evaluador de SSMA.  
  
-   Si selecciona **no**, las columnas ROWID no se agregan a las tablas.  
  
-   **Agregar columna ROWID para tablas con desencadenadores** agregue ROWID para las tablas que contienen desencadenadores.  
  
> [!WARNING]  
> La configuración predeterminada en el caso de SQL Server 2005, SQL Server 2008 y SQL Server 2012 y 2014 es **Agregar columna ROWID para tablas con desencadenadores**.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** Agregar columna ROWID para tablas con desencadenadores  
  
**Modo completo:** ?  
  
### <a name="generate-unique-index-on-rowid-column"></a>Generar índice único en la columna ROWID  
Especifica si SSMA genera una columna de índice UNIQUE en la columna generada por ROWID o no. Si la opción se establece en "sí", se genera un índice único y, si se establece en "NO", no se genera un índice único en la columna ROWID.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="local-modules-conversion"></a>Conversión de módulos locales  
Define el tipo de subprograma anidado DB2 (declarado en el procedimiento almacenado o la función independiente).  
  
-   Si selecciona **insertado**, las llamadas de subprograma anidadas se reemplazarán por su cuerpo.  
  
-   Si selecciona **procedimientos almacenados**, el subprograma anidado se convertirá en un SQL Server procedimiento almacenado y sus llamadas se reemplazarán en esta llamada al procedimiento.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** En línea  
  
### <a name="use-isnull-in-string-concatenation"></a>Usar ISNULL en la concatenación de cadenas  
DB2 y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelven resultados diferentes cuando las concatenaciones de cadenas incluyen valores NULL. DB2 trata el valor NULL como un juego de caracteres vacío. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve NULL.  
  
-   Si selecciona **sí**, SSMA reemplaza el carácter de concatenación de DB2 (| |) por el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] carácter de concatenación (+). SSMA también comprueba las expresiones en ambos lados de la concatenación para los valores NULL.  
  
-   Si selecciona no, SSMA reemplaza los caracteres de concatenación, pero no comprueba si **hay** valores NULL.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
### <a name="use-isnull-in-replace-function-calls"></a>Usar ISNULL en las llamadas de función Replace  
La instrucción ISNULL se utiliza en las llamadas de función Replace para emular el comportamiento de DB2. Las siguientes opciones están presentes para esta configuración:  
  
-   SÍ  
  
-   No  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** No  
  
**Modo completo:** ?  
  
### <a name="use-isnull-in-concat-function-calls"></a>Usar ISNULL en las llamadas a funciones CONCAt  
La instrucción ISNULL se utiliza en las llamadas de función CONCAt para emular el comportamiento de DB2. Las siguientes opciones están presentes para esta configuración:  
  
-   SÍ  
  
-   No  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** No  
  
**Modo completo:** ?  
  
### <a name="use-native-convert-function-when-possible"></a>Usar la función de conversión nativa cuando sea posible  
  
-   Si selecciona **sí**, SSMA convierte el TO_CHAR (Date, Format) en la función de conversión nativa cuando sea posible.  
  
-   Si selecciona **no**, SSMA convierte el TO_CHAR (Date, Format) en TO_CHAR_DATE o TO_CHAR_DATE_LS (se define mediante las opciones "convertir TO_CHAR (Date, Format)").  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista:** ?  
  
**Modo completo:** No  
  
### <a name="use-selectfor-xml-when-converting-selectinto-for-record-variable"></a>Usar SELECT... FOR XML al convertir SELECT... INTO para variable de registro  
Especifica si se debe generar un conjunto de resultados XML cuando se selecciona en una variable de registro.  
  
-   Si selecciona **sí**, la instrucción SELECT devuelve XML.  
  
-   Si selecciona **no**, la instrucción SELECT devuelve un conjunto de resultados.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** No  
  
## <a name="returning-clause-conversion"></a>Conversión de la cláusula de devolución  
  
### <a name="convert-returning-clause-in-delete-statement-to-output"></a>Convertir la cláusula devuelta en la instrucción DELETE en la salida  
DB2 proporciona una cláusula RETURNING como una manera de obtener inmediatamente los valores eliminados. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona esa funcionalidad con la cláusula OUTPUT.  
  
-   Si selecciona **sí**, SSMA convertirá las cláusulas devueltas en las instrucciones delete en cláusulas output. Dado que los desencadenadores de una tabla pueden cambiar valores, el valor devuelto podría ser diferente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que en DB2.  
  
-   Si selecciona **no**, SSMA generará una instrucción SELECT antes de las instrucciones delete para recuperar los valores devueltos.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-returning-clause-in-insert-statement-to-output"></a>Convertir la cláusula devuelta en la instrucción INSERT en la salida  
DB2 proporciona una cláusula RETURNING como una manera de obtener inmediatamente los valores insertados. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona esa funcionalidad con la cláusula OUTPUT.  
  
-   Si selecciona **sí**, SSMA convertirá una cláusula devuelta en una instrucción INSERT en Output. Dado que los desencadenadores de una tabla pueden cambiar valores, el valor devuelto podría ser diferente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que en DB2.  
  
-   Si selecciona **no**, SSMA emula la funcionalidad de DB2 insertando y seleccionando los valores de una tabla de referencia.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
### <a name="convert-returning-clause-in-update-statement-to-output"></a>Convertir la cláusula devuelta en la instrucción UPDATE en la salida  
DB2 proporciona una cláusula RETURNING como una manera de obtener inmediatamente los valores actualizados. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona esa funcionalidad con la cláusula OUTPUT.  
  
-   Si selecciona **sí**, SSMA convertirá las cláusulas devueltas en las instrucciones Update en las cláusulas output. Dado que los desencadenadores de una tabla pueden cambiar valores, el valor devuelto podría ser diferente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que en DB2.  
  
-   Si selecciona **no**, SSMA generará instrucciones SELECT después de las instrucciones Update para recuperar los valores devueltos.  
  
Al seleccionar un modo de conversión en el cuadro **modo** , SSMA aplica la siguiente configuración:  
  
**Modo predeterminado/optimista/completo:** ?  
  
## <a name="sequence-conversion"></a>Conversión de secuencias  
  
### <a name="convert-sequence-generator"></a>Generador de secuencias de conversión  
En DB2, puede usar una secuencia para generar identificadores únicos.  
  
SSMA puede convertir secuencias en lo siguiente.  
  
-   Usar el generador de secuencias de SQL Server (esta opción solo está disponible cuando se convierte en SQL Server 2012 y SQL Server 2014).  
  
-   Usar el generador de secuencias de SSMA.  
  
-   Usar la identidad de columna.  
  
La opción predeterminada al convertir a SQL Server 2012 o SQL Server 2014 es usar el generador de secuencias de SQL Server. Sin embargo, SQL Server 2012 y SQL Server 2014 no admiten la obtención del valor de secuencia actual (por ejemplo, el del método currval de secuencia DB2). Consulte el sitio del blog del equipo de SSMA para obtener instrucciones sobre cómo migrar el método currval de secuencia de DB2.  
  
SSMA también proporciona una opción para convertir la secuencia de DB2 en el emulador de secuencia de SSMA. Esta es la opción predeterminada cuando se convierte en SQL Server antes de 2012  
  
Por último, también puede convertir la secuencia asignada a una columna de la tabla para SQL Server valores de identidad. Debe especificar la asignación entre las secuencias en una columna de identidad en la pestaña de la **tabla** DB2  
  
### <a name="convert-currval-outside-triggers"></a>Convertir los desencadenadores de CURRVAL fuera de  
Solo es visible cuando el generador de secuencias de conversión se establece en **utilizando la identidad de columna**. Dado que las secuencias DB2 son objetos independientes de las tablas, muchas de las tablas que usan secuencias utilizan un desencadenador para generar e insertar un nuevo valor de secuencia. SSMA comenta estas instrucciones o las marca como errores cuando el comentario genera errores.  
  
-   Si selecciona **sí**, SSMA marcará todas las referencias a los desencadenadores externos en la secuencia convertida CURRVAL con una advertencia.  
  
-   Si selecciona **no**, SSMA marcará todas las referencias a los desencadenadores externos en la secuencia convertida CURRVAL con un error.  
  
## <a name="see-also"></a>Consulte también  
[Referencia de la interfaz de usuario &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
