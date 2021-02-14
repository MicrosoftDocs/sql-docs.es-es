---
title: Usar condiciones de prueba en pruebas unitarias de SQL Server
description: Obtenga información sobre las condiciones de prueba en las pruebas unitarias de SQL Server. Vea cómo usar condiciones predefinidas y pruebas negativas, y obtenga información sobre condiciones personalizadas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconditions
ms.assetid: e3d1c86c-1e58-4d2c-b625-d1b591b221aa
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 18021d85c9b490664411b7bb98f1b55972a13022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082416"
---
# <a name="using-test-conditions-in-sql-server-unit-tests"></a>Usar condiciones de prueba en pruebas unitarias de SQL Server

En una prueba unitaria de SQL Server, se ejecutan uno o varios scripts de prueba Transact\-SQL. Los resultados se pueden evaluar dentro del script Transact\-SQL y usar THROW o RAISERROR para devolver un error y que no se supere la prueba, o se pueden definir condiciones de prueba en la prueba para evaluar los resultados. La prueba devuelve una instancia de la clase [SqlExecutionResult](/previous-versions/sql/sql-server-data-tools/jj856590(v=vs.103)). La instancia de esta clase contiene uno o más DataSets, el tiempo de ejecución y las filas afectadas por el script. Toda esta información se recopila durante la ejecución del script. Estos resultados se pueden evaluar utilizando las condiciones de prueba. SQL Server Data Tools proporciona un conjunto de condiciones de prueba predefinidas. También puede crear y usar condiciones personalizadas, consulte [Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="predefined-test-conditions"></a>Condiciones de prueba predefinidas  
En la tabla siguiente se enumeran las condiciones de prueba predefinidas que puede agregar mediante el panel Condiciones de prueba del Diseñador de pruebas unitarias de SQL Server.  
  
|**Condición de prueba**|**Descripción de la condición de prueba**|  
|----------------------|----------------------------------|  
|Suma de comprobación de datos|Se produce un error si la suma de comprobación del conjunto de resultados devuelto del script Transact\-SQL no coincide con la suma de comprobación esperada. Para obtener más información, vea [Especificar una suma de comprobación de datos](#SpecifyDataChecksum).<br /><br />**NOTA:** Esta condición de prueba no se recomienda si va a devolver datos que varían entre las series de pruebas. Por ejemplo, si el conjunto de resultados contiene fechas u horas de salida, o contiene columnas de identidad, las pruebas producirán un error porque la suma de comprobación será diferente en cada ejecución.|  
|Conjunto de resultados vacío|Se produce un error si el conjunto de resultados devuelto script Transact\-SQL no está vacío.|  
|Tiempo de ejecución|Se produce un error si el script de prueba Transact\-SQL lleva más tiempo del esperado en ejecutarse. El tiempo de ejecución predeterminado es de 30 segundos.<br /><br />El tiempo de ejecución se aplica únicamente a la prueba de script de prueba, no al script anterior a la prueba ni al script posterior a la prueba.|  
|Esquema esperado|Se produce un error si las columnas y los tipos de datos del conjunto de resultados no coinciden con los especificados para la condición de prueba. Debe especificar un esquema mediante las propiedades de la condición de prueba. Para obtener más información, vea [Especificar un esquema esperado](#SpecifyExpectedSchema).|  
|No concluyente|Siempre genera una prueba con un resultado de No concluyente. Esta es la condición predeterminada agregada a cada prueba. Esta condición de prueba se incluye para indicar que la comprobación de la prueba no se ha implementado. Elimine esta condición de prueba de la prueba después de agregar otras condiciones de prueba.|  
|No se admite ResultSet vacío|Se produce un error si el conjunto de resultados está vacío. Puede usar esta condición de prueba o EmptyResultSet con la función @@RAISERROR de Transact\-SQL en el script de prueba para comprobar si una actualización funcionó correctamente. Por ejemplo, puede guardar los valores anteriores a la actualización, ejecutar la actualización, comparar los valores posteriores a la actualización y generar un error si no se obtienen los resultados esperados.|  
|Recuento de filas|Se produce un error si el conjunto de resultados no contiene el número de filas esperado.|  
|Valor escalar|Se produce un error si un valor determinado del conjunto de resultados no es igual al valor especificado. El **Valor esperado** predeterminado es null.|  
  
> [!NOTE]  
> La condición de prueba Tiempo de ejecución especifica un límite de tiempo en el que debe ejecutarse el script de prueba Transact\-SQL. Si se supera este límite de tiempo, la prueba genera un error. Los resultados de pruebas también incluyen una estadística de duración, que difiere de la condición de prueba Tiempo de ejecución. La estadística de duración no solo incluye el tiempo de ejecución, sino también el tiempo para conectarse a la base de datos dos veces; la hora de ejecutar cualquier otro script de prueba, como el script anterior a la prueba y el script posterior a la prueba, y la hora de ejecutar las condiciones de prueba. Por consiguiente, una prueba se puede superar aunque su duración sea mayor que el tiempo de ejecución.  
>   
> La duración notificada no incluye el tiempo usado para la generación de datos y la implementación del esquema, porque aparecen antes de que se ejecuten las pruebas. Para ver la duración de la prueba, seleccione una serie de pruebas en la ventana **Resultados de pruebas**, haga clic con el botón derecho y elija **Ver detalles de resultados de pruebas**.  
  
Puede agregar condiciones de prueba a pruebas unitarias de SQL Server mediante el panel Condiciones de prueba del Diseñador de pruebas unitarias de SQL Server. Para más información, vea: [Cómo: Incorporar condiciones de prueba a pruebas unitarias de SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
También puede modificar el código del método de prueba directamente para agregar más funcionalidad. Para más información, consulte [Cómo: Abrir una prueba unitaria de SQL Server para editarla](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md) y [Cómo: Escribir una prueba unitaria de SQL Server que se ejecuta en el ámbito de una única transacción](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md). Por ejemplo, puede agregar funcionalidad a un método de prueba si agrega instrucciones Assert. Para más información, consulte [Usar aserciones de Transact-SQL en pruebas unitarias de SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md).  
  
## <a name="expected-failures"></a>Errores esperados  
Puede crear pruebas unitarias de SQL Server para probar el comportamiento que no debe ejecutarse correctamente. Estos errores esperados se conocen a veces como pruebas negativas. A continuación, se exponen algunos ejemplos:  
  
-   Comprobar que un procedimiento almacenado que elimina los datos de un cliente produce un error si se especifica un identificador de cliente no válido.  
  
-   Comprobar que un procedimiento almacenado que rellena un pedido produce un error si el pedido no se realizó nunca o si el pedido ya se ha rellenado.  
  
-   Comprobar que un procedimiento almacenado que cancela un pedido no pueda cancelar los pedidos completados ni los pedidos que ya se han cancelado.  
  
Puede definir pruebas unitarias de SQL Server para los procedimientos almacenados que producen excepciones esperadas. Puede agregar un atributo al método de prueba unitaria para indicar qué excepción o excepciones se esperan. De esta forma, se evita que la prueba produzca un error cuando se genera la excepción.  
  
Para marcar un método de prueba unitaria de SQL Server con excepciones esperadas, agregue el atributo siguiente:  
  
```  
[ExpectedSqlException(MessageNumber = nnnnn, Severity = x, MatchFirstError = false, State = y)]  
```  
  
Donde:  
  
-   *nnnnn* es el número del mensaje esperado, por ejemplo 14025.  
  
-   *x* es la gravedad de la excepción esperada.  
  
-   *y* es el estado de la excepción esperada.  
  
Se omite cualquier parámetro no especificado. Estos parámetros se pasan a la instrucción **THROW** en el código de base de datos. Si especifica MatchFirstError = false, el atributo coincidirá con cualquier error de SQL en la excepción. El comportamiento predeterminado (MatchFirstError = true) consiste en hacer coincidir solamente el primer error que aparezca.  
  
Para obtener un ejemplo de cómo usar excepciones esperadas y una prueba unitaria negativa de SQL Server, consulte [Tutorial: Crear y ejecutar una prueba unitaria de SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="specifying-a-data-checksum"></a><a name="SpecifyDataChecksum"></a>Especificar una suma de comprobación de datos  
Para mostrar el Diseñador de pruebas unitarias de SQL Server, haga doble clic en el archivo de código fuente de la prueba unitaria en el **Explorador de soluciones**.  
  
Después de agregar una condición de prueba Suma de comprobación de datos a la prueba unitaria de base de datos, debe configurar la suma de comprobación esperada mediante el siguiente procedimiento:  
  
#### <a name="to-specify-an-expected-checksum"></a>Para especificar una suma de comprobación esperada  
  
1.  En la lista de condiciones de prueba, haga clic en la condición de prueba Suma de comprobación de datos para la que desee especificar una suma de comprobación.  
  
2.  Abra la ventana **Propiedades** presionando F4. También puede abrir el menú **Ver** y hacer clic en la ventana **Propiedades** .  
  
3.  (Opcional) Es posible que desee cambiar la propiedad **(Nombre)** de la condición de prueba para que sea más descriptiva.  
  
4.  En la propiedad **Configuración**, haga clic en el botón Examinar ( **…** ).  
  
    Aparece el cuadro de diálogo **Configuración de TestConditionName** .  
  
5.  Especifique una conexión a la base de datos que desee probar. Para más información, vea: [Cómo: Crear una conexión a una base de datos](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  De manera predeterminada, el cuerpo de Transact\-SQL de la prueba aparece en el panel de edición. Puede modificar el código en caso necesario, para generar los resultados esperados. Por ejemplo, si la prueba tiene código anterior a la prueba, quizás tenga que agregar ese código.  
  
    > [!IMPORTANT]  
    > Si modifica una condición de suma de comprobación para la que se había especificado anteriormente una suma de comprobación, los cambios realizados en el panel de edición no se guardan. Debe realizar los cambios de nuevo antes de hacer clic en **Recuperar**.  
  
7.  Haga clic en **Recuperar**.  
  
    Transact\-SQL se ejecuta en la conexión de base de datos especificada y los resultados aparecen en el cuadro de diálogo.  
  
8.  Si los resultados coinciden con los resultados esperados de la prueba, haga clic en **Aceptar**. De lo contrario, modifique el cuerpo de Transact\-SQL y repita los pasos 6, 7 y 8, hasta que los resultados son los esperados.  
  
    La columna **Valor** de la condición de prueba muestra el valor de la suma de comprobación esperada.  
  
## <a name="specifying-an-expected-schema"></a><a name="SpecifyExpectedSchema"></a>Especificar un esquema esperado  
Una vez agregada una condición de prueba Esquema esperado a la prueba unitaria de SQL Server, debe configurar el esquema esperado mediante el procedimiento siguiente:  
  
#### <a name="to-specify-an-expected-schema"></a>Para especificar un esquema esperado  
  
1.  En la lista de condiciones de prueba, haga clic en la condición de prueba Esquema esperado para la que desee especificar un esquema.  
  
2.  Abra la ventana **Propiedades** presionando F4. También puede abrir el menú **Ver** y hacer clic en la ventana **Propiedades** .  
  
3.  (Opcional) Es posible que desee cambiar la propiedad **(Nombre)** de la condición de prueba para que sea más descriptiva.  
  
4.  En la propiedad **Configuración**, haga clic en el botón Examinar ( **…** ).  
  
    Aparece el cuadro de diálogo **Configuración de TestConditionName** .  
  
5.  Especifique una conexión a la base de datos que desee probar. Para más información, vea: [Cómo: Crear una conexión a una base de datos](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  De manera predeterminada, el cuerpo de Transact\-SQL de la prueba aparece en el panel de edición. Puede modificar el código en caso necesario, para generar los resultados esperados. Por ejemplo, si la prueba tiene código anterior a la prueba, quizás tenga que agregar ese código.  
  
    > [!IMPORTANT]  
    > Si modifica una condición de esquema esperado para la que se había especificado anteriormente un esquema, los cambios realizados en el panel de edición no se guardan. Debe realizar los cambios de nuevo antes de hacer clic en **Recuperar**.  
  
7.  Haga clic en **Recuperar**.  
  
    Transact\-SQL se ejecuta en la conexión de base de datos especificada y los resultados aparecen en el cuadro de diálogo. Dado que está comprobando el esquema, o la forma, el conjunto de resultados y no los valores de los resultados, no tiene que ver los datos en los resultados devueltos, siempre y cuando las columnas aparezcan de la manera que se espera.  
  
8.  Si los resultados coinciden con los resultados esperados de la prueba, haga clic en **Aceptar**. De lo contrario, modifique el cuerpo de Transact\-SQL y repita los pasos 6, 7 y 8, hasta que los resultados son los esperados.  
  
    La columna **Valor** de la condición de prueba muestra información acerca del esquema esperado. Por ejemplo, podría indicar “Se esperaba: 2 tablas”.  
  
## <a name="extensible-test-conditions"></a>Condiciones de prueba extensibles  
Además de las seis condiciones de prueba predefinidas, puede escribir nuevas condiciones de prueba propias. Estas condiciones de prueba se mostrarán en el panel Condiciones de prueba del Diseñador de pruebas unitarias de SQL Server. Para más información, consulte [Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Usar aserciones de Transact-SQL en pruebas unitarias de SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)  
[Scripts de pruebas unitarias de SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
