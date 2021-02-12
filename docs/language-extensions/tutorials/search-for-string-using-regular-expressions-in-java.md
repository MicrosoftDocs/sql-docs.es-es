---
title: 'Tutorial: Búsqueda de cadenas regex en Java'
description: En este tutorial se muestra cómo usar las extensiones de lenguaje de SQL Server y ejecutar código Java para realizar una búsqueda de una cadena mediante el uso de expresiones regulares (regex).
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 6502be49b2076e8f6d89bdd88695f24b65dc79ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075196"
---
# <a name="tutorial-search-for-a-string-using-regular-expressions-regex-in-java"></a>Tutorial: Búsqueda de una cadena mediante expresiones regulares (regex) en Java
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

En este tutorial se muestra cómo usar las [extensiones de lenguaje de SQL Server](../language-extensions-overview.md) para crear una clase Java que reciba dos columnas (identificador y texto) de SQL Server y una expresión regular (regex) como parámetro de entrada. La clase devuelve dos columnas a SQL Server (identificador y texto).

Para un texto determinado en la columna de texto que se envía a la clase Java, el código comprueba si se cumple la expresión regular especificada y devuelve el texto junto con el identificador original.

Este código de ejemplo utiliza una expresión regular que comprueba si un texto contiene la palabra "Java" o "java".

## <a name="prerequisites"></a>Prerrequisitos

+ Instancia de Motor de base de datos de SQL Server 2019 con el marco de extensibilidad y la extensión de programación de Java en [Windows](../install/windows-java.md) o en [Linux](../../linux/sql-server-linux-setup-language-extensions-java.md). Para obtener más información, consulte el artículo sobre las [extensiones de lenguaje de SQL Server 2019](../language-extensions-overview.md). Para obtener más información sobre los requisitos de codificación, consulte [Cómo llamar a Java en SQL Server](../how-to/call-java-from-sql.md).

+ SQL Server Management Studio o Azure Data Studio para ejecutar T-SQL.

+ Java SE Development Kit (JDK) 8 o JRE 8 en Windows o Linux.

+ El archivo **mssql-java-lang-extension.jar** del [SDK de extensibilidad de Java de Microsoft para Microsoft SQL Server](../how-to/extensibility-sdk-java-sql-server.md).

La compilación de línea de comandos con **javac** es suficiente para este tutorial.

## <a name="create-sample-data"></a>Creación de datos de ejemplo

En primer lugar, cree una base de datos y rellene una tabla **testdata** con columnas de **identificador**  y **texto**.

```sql
CREATE DATABASE javatest
GO

USE javatest
GO

CREATE TABLE testdata (
    id int NOT NULL,
    "text" nvarchar(100) NOT NULL
)
GO

-- Insert data into test table
INSERT INTO testdata(id, "text") VALUES (1, 'This sentence contains java')
INSERT INTO testdata(id, "text") VALUES (2, 'This sentence does not')
INSERT INTO testdata(id, "text") VALUES (3, 'I love Java!')
GO
```

## <a name="create-the-main-class"></a>Creación de la clase principal

En este paso, cree un archivo de clase denominado **RegexSample.java** y copie el siguiente código Java en ese archivo.

Esta clase principal importa el SDK, lo que significa que el archivo .jar descargado en el paso 1 debe ser reconocible desde esta clase.

```java
package pkg;

import com.microsoft.sqlserver.javalangextension.PrimitiveDataset;
import com.microsoft.sqlserver.javalangextension.AbstractSqlServerExtensionExecutor;
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.ListIterator;
import java.util.regex.*;

public class RegexSample extends AbstractSqlServerExtensionExecutor {
    private Pattern expr;

    public RegexSample() {
        // Setup the expected extension version, and class to use for input and output dataset
        executorExtensionVersion = SQLSERVER_JAVA_LANG_EXTENSION_V1;
        executorInputDatasetClassName = PrimitiveDataset.class.getName();
        executorOutputDatasetClassName = PrimitiveDataset.class.getName();
    }
    
    public PrimitiveDataset execute(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Validate the input parameters and input column schema
        validateInput(input, params);

        int[] inIds = input.getIntColumn(0);
        String[] inValues = input.getStringColumn(1);
        int rowCount = inValues.length;

        String regexExpr = (String)params.get("regexExpr");
        expr = Pattern.compile(regexExpr);

        System.out.println("regex expression: " + regexExpr);

        // Lists to store the output data
        LinkedList<Integer> outIds = new LinkedList<Integer>();
        LinkedList<String> outValues = new LinkedList<String>();

        // Evaluate each row
        for(int i = 0; i < rowCount; i++) {
            if (check(inValues[i])) {
                outIds.add(inIds[i]);
                outValues.add(inValues[i]);
            }
        }

        int outputRowCount = outValues.size();

        int[] idOutputCol = new int[outputRowCount];
        String[] valueOutputCol = new String[outputRowCount];

        // Convert the list of output columns to arrays
        outValues.toArray(valueOutputCol);

        ListIterator<Integer> it = outIds.listIterator(0);
        int rowId = 0;

        System.out.println("Output data:");
        while (it.hasNext()) {
            idOutputCol[rowId] = it.next().intValue();

            System.out.println("ID: " + idOutputCol[rowId] + " Value: " + valueOutputCol[rowId]);
            rowId++;
        }

        // Construct the output dataset
        PrimitiveDataset output = new PrimitiveDataset();

        output.addColumnMetadata(0, "ID", java.sql.Types.INTEGER, 0, 0);
        output.addColumnMetadata(1, "Text", java.sql.Types.NVARCHAR, 0, 0);

        output.addIntColumn(0, idOutputCol, null);
        output.addStringColumn(1, valueOutputCol);

        return output;
    }

    private void validateInput(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Check for the regex expression input parameter
        if (params.get("regexExpr") == null) {
            throw new IllegalArgumentException("Input parameter 'regexExpr' is not found");
        }

        // The expected input schema should be at least 2 columns, (INTEGER, STRING)
        if (input.getColumnCount() < 2) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }

        // Check that the input column types are expected
        if (input.getColumnType(0) != java.sql.Types.INTEGER &&
                (input.getColumnType(1) != java.sql.Types.VARCHAR && input.getColumnType(1) == java.sql.Types.NVARCHAR )) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }
    }

    private boolean check(String text) {
        Matcher m = expr.matcher(text);

        return m.find();
    }
}
```

## <a name="compile-and-create-a-jar-file"></a>Compilación y creación de un archivo .jar

Empaquete sus clases y dependencias en un archivo `.jar`. La mayoría de los IDE de Java (por ejemplo, Eclipse o IntelliJ) permiten generar archivos `.jar` al crear o compilar el proyecto. Asigne el nombre **regex.jar** al archivo `.jar`.

Si no usa un IDE de Java, puede crear manualmente un archivo `.jar`. Para obtener más información, consulte [Cómo crear un archivo .jar de Java a partir de archivos de clase](../how-to/create-a-java-jar-file-from-class-files.md).

> [!NOTE]
> En este tutorial se usan paquetes. La línea `package pkg;` en la parte superior de la clase garantiza que el código compilado se guarde en una subcarpeta llamada **pkg**. Si usa un IDE, el código compilado se guardará automáticamente en esta carpeta. Si usa **javac** para compilar manualmente las clases, debe colocar el código compilado en la carpeta **pkg**.

## <a name="create-external-language"></a>Creación de un lenguaje externo

Es necesario crear un lenguaje externo en la base de datos. El lenguaje externo es un objeto con ámbito de base de datos, lo que significa que es necesario crear lenguajes externos, como Java, para cada base de datos en la que quiera usarlo.

### <a name="create-external-language-on-windows"></a>Creación de un lenguaje externo en Windows

Si usa Windows, siga los pasos que se indican a continuación para crear un lenguaje externo para Java.

1. Cree un archivo. zip que contenga la extensión.

    Como parte de la configuración de SQL Server en Windows, el archivo **.zip** de la extensión de Java se instala en `[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip`. Este archivo .zip contiene el archivo **javaextension.dll**.

2. Cree un lenguaje Java externo a partir del archivo .zip:

    ```sql
    CREATE EXTERNAL LANGUAGE Java
    FROM
    (CONTENT = N'[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip', FILE_NAME = 'javaextension.dll',
    ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
    GO
    ```

### <a name="create-external-language-on-linux"></a>Creación de un lenguaje externo en Linux

Como parte de la instalación, el archivo **.tar.gz** de la extensión se guarda en la ruta de acceso `/opt/mssql-extensibility/lib/java-lang-extension.tar.gz`.

Para crear un lenguaje Java externo, ejecute la siguiente instrucción T-SQL en Linux:

```sql
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'/opt/mssql-extensibility/lib/java-lang-extension.tar.gz', file_name = 'javaextension.so',
ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
GO
```

### <a name="permissions-to-execute-external-language"></a>Permisos para ejecutar un lenguaje externo

Para ejecutar código Java, hay que conceder a un usuario permisos de ejecución de scripts externos en ese lenguaje específico.

Para obtener más información, vea [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="create-external-libraries"></a>Creación de bibliotecas externas

Utilice [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) para crear una biblioteca externa para sus archivos `.jar`. SQL Server tendrá acceso a los archivos `.jar`, sin que sea necesario establecer ningún permiso especial en la propiedad **classpath**.

En este ejemplo, creará dos bibliotecas externas: una para el SDK y otra para el código RegEx de Java.

1. El archivo .jar del SDK, **mssql-java-lang-extension.jar**, se instala como parte de SQL Server 2019 en Windows y Linux.

    + Ruta de instalación predeterminada en Windows: **[directorio principal de instalación de instancia]\MSSQL\Binn\mssql-java-lang-extension.jar**

    + Ruta de instalación predeterminada en Linux: **/opt/mssql/lib/mssql-java-lang-extension.jar**

    El código es abierto y puede encontrarse en el [repositorio de GitHub sobre extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions). Para obtener más información, consulte [SDK de extensibilidad de Java de Microsoft para Microsoft SQL Server](../how-to/extensibility-sdk-java-sql-server.md).

2. Cree una biblioteca externa para el SDK.

    ```sql
    CREATE EXTERNAL LIBRARY sdk
    FROM (CONTENT = '<OS specific path from above>/mssql-java-lang-extension.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

3. Cree una biblioteca externa para el código RegEx.

    ```sql
    CREATE EXTERNAL LIBRARY regex
    FROM (CONTENT = '<path>/regex.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

## <a name="set-permissions"></a>Establecimiento de permisos

> [!NOTE]
> Omita este paso si usa bibliotecas externas en el paso anterior. Lo recomendable es crear una biblioteca externa a partir del archivo `.jar`.

Si no quiere usar bibliotecas externas, tendrá que establecer los permisos necesarios. La ejecución del script solo se realiza correctamente si las identidades del proceso tienen acceso al código. Puede encontrar más información sobre cómo establecer permisos en la [guía de instalación](../install/windows-java.md).

### <a name="on-linux"></a>En Linux

Conceda permisos de lectura y ejecución en la propiedad classpath al usuario **mssql_satellite**.

### <a name="on-windows"></a>En Windows

Conceda permisos de lectura y ejecución a **SQLRUserGroup** y a los SID de **todos paquetes de aplicación** de la carpeta que contiene el código Java compilado.

Todo el árbol debe tener permisos, desde el elemento primario raíz hasta la última subcarpeta.

1. Haga clic con el botón derecho en la carpeta (por ejemplo, `C:\myJavaCode`) y elija **Propiedades** > **Seguridad**.
2. Haga clic en **Editar**.
3. Haga clic en **Agregar**.
4. En **Seleccionar usuarios, equipo, cuentas de servicio o grupos**:
   1. Haga clic en **Tipos de objeto** y asegúrese de que las opciones *Principios de seguridad integrados* y *Grupos* estén seleccionadas.
   2. Haga clic en **Ubicaciones** para seleccionar el nombre del equipo local en la parte superior de la lista.
5. Escriba **SQLRUserGroup**, compruebe el nombre y haga clic en Aceptar para agregar el grupo.
6. Escriba **TODOS LOS PAQUETES DE APLICACIONES**, compruebe el nombre y haga clic en Aceptar para agregar. 
    Si el nombre no se resuelve, vuelva al paso de las ubicaciones. El SID es local en su máquina.

Asegúrese de que las identidades de seguridad tengan permisos de **lectura y ejecución** en la carpeta y en la subcarpeta **pkg**.

<a name="call-method"></a>

## <a name="call-the-java-class"></a>Llamada a la clase Java

Cree un procedimiento almacenado que llame a `sp_execute_external_script` para llamar al código Java desde SQL Server. En el parámetro **script**, defina los `package.class` que quiera llamar. En el código siguiente, la clase pertenece a un paquete denominado **pkg** y un archivo de clase denominado **RegexSample.java**.

> [!NOTE]
> El código no define el método al que se va a llamar. De forma predeterminada, se llamará al método **execute**. Esto significa que, si quiere poder llamar a la clase desde SQL Server, ha de seguir la interfaz del SDK e implementar un método de ejecución en la clase Java.

El procedimiento almacenado toma una consulta de entrada (conjunto de datos de entrada) y una expresión regular, y devuelve las filas que cumplen la expresión regular especificada. Utiliza una expresión regular `[Jj]ava` que comprueba si un texto contiene la palabra **Java** o **java**.

```sql
CREATE OR ALTER PROCEDURE [dbo].[java_regex] @expr nvarchar(200), @query nvarchar(400)
AS
BEGIN
--Call the Java program by giving the package.className in @script
--The method invoked in the Java code is always the "execute" method
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'pkg.RegexSample'
, @input_data_1 = @query
, @params = N'@regexExpr nvarchar(200)'
, @regexExpr = @expr
with result sets ((ID int, text nvarchar(100)));
END
GO

--Now execute the above stored procedure and provide the regular expression and an input query
EXECUTE [dbo].[java_regex] N'[Jj]ava', N'SELECT id, text FROM testdata'
GO
```

### <a name="results"></a>Results

Después de ejecutar la llamada, debería obtener un conjunto de resultados con dos de las filas.

![Resultados del ejemplo de Java](../media/java/java-sample-results.png "Ejemplo de resultados")

### <a name="if-you-get-an-error"></a>Si obtiene un mensaje de error

+ Al compilar las clases, la subcarpeta **pkg** debe contener el código compilado para las tres clases.

+ Si no usa bibliotecas externas, compruebe los permisos en *cada* carpeta, desde la **raíz** hasta la subcarpeta **pkg**, para asegurarse de que las identidades de seguridad que ejecutan el proceso externo tengan permiso para leer y ejecutar el código.

## <a name="next-steps"></a>Pasos siguientes

+ [Cómo llamar a Java en SQL Server](../how-to/call-java-from-sql.md)
+ [Extensiones de Java en SQL Server](../language-extensions-overview.md)
+ [Tipos de datos de Java y SQL Server](../how-to/java-to-sql-data-types.md)