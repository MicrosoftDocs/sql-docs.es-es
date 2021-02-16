---
description: 'CREATE EXTERNAL LANGUAGE (Transact-SQL): SQL Server'
title: CREATE EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 04/03/2020
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a299c47b9d0d2fccde5c44c26d9f78e8bc1cca2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070693"
---
# <a name="create-external-language-transact-sql"></a>CREATE EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Registra las extensiones de lenguaje externo en la base de datos desde la secuencia de bytes o la ruta de acceso de archivo especificada. Esta instrucción actúa como un mecanismo genérico para que el administrador de base de datos pueda registrar las extensiones del lenguaje externo nuevo en cualquier plataforma de sistema operativo compatible con SQL Server. Para obtener más información, consulte las [extensiones de lenguaje](../../language-extensions/language-extensions-overview.md).

> [!NOTE]
> **R** y **Python** son nombres reservados y no se puede crear ningún lenguaje externo con esos nombres. Para obtener más información sobre cómo usar **R** y **Python**, consulte [SQL Server Machine Learning Services](../../machine-learning/index.yml).

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = { <external_lang_specifier> | <content_bits> },
    FILE_NAME = <external_lang_file_name>
    [ , PLATFORM = <platform> ]
    [ , PARAMETERS = <external_lang_parameters> ]
    [ , ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
    | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'


<platform> :: =
{
    WINDOWS
  | LINUX
}

<external_lang_parameters> :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>Argumentos

**language_name**

Los lenguajes son objetos del ámbito de la base de datos. Los nombres del lenguaje deben ser únicos en la base de datos.

**owner_name**

Especifica el nombre del usuario o rol que es propietario del lenguaje externo. Si no se especifica, la propiedad se otorga al usuario actual. Dependiendo de los permisos, puede ser que haya que conceder permisos explícitos a otros usuarios para ejecutar scripts mediante un lenguaje específico.

**file_spec**

Especifica el contenido de la extensión del lenguaje. Solo se permite un argumento filespec para un lenguaje específico por plataforma.

**external_lang_specifier**

La ruta de acceso completa al archivo .zip o tar.gz que contiene el código de las extensiones. Este contenido puede ser una ruta de acceso a un archivo .zip (en Windows) o tar.gz (en Linux).

**content_bits**

Especifica el contenido del lenguaje como un literal hexadecimal, similar a los ensamblados.

Esta opción es útil si necesita crear un lenguaje o modificar uno ya existente (y tiene los permisos necesarios para hacerlo), pero el sistema de archivos en el servidor está restringido y no puede copiar los archivos de la biblioteca en una ubicación a la que pueda tener acceso el servidor.

**external_lang_file_name**

Nombre del archivo con extensión .dll o .so. Es necesario para identificar el archivo correcto en casos donde hay varios archivos .dll o .so en el <external_lang_specifier> .zip o tar.gz.

**external_lang_parameters**

Proporciona la posibilidad de conceder un conjunto de parámetros al tiempo de ejecución del lenguaje externo. Los valores de los parámetros se proporcionan para el tiempo de ejecución externo una vez iniciado el proceso externo. Sin embargo, la extensión del lenguaje puede acceder a las variables de entorno antes del inicio del proceso externo.

**external_lang_env_variables**

Proporciona la posibilidad de conceder un conjunto de variables de entorno al tiempo de ejecución del lenguaje externo antes del inicio del proceso externo. Un ejemplo de una variable de entorno sería, por ejemplo, el directorio principal del mismo tiempo de ejecución. Por ejemplo: JRE_HOME.

**platform**

Este parámetro es necesario para escenarios de sistemas operativos híbridos. En una arquitectura híbrida, el lenguaje debe registrarse una vez por cada plataforma. El nombre de la plataforma y del lenguaje será la clave única por el lenguaje externo. Si no se especifica ninguna plataforma, se usará el sistema operativo actual.

## <a name="permissions"></a>Permisos

Requiere el permiso `CREATE EXTERNAL LANGUAGE`. De forma predeterminada, cualquier usuario que tenga **dbo** y sea miembro del rol **db_owner** tiene permisos para crear un lenguaje externo. Para todos los demás usuarios, debe concederles explícitamente permiso con una instrucción [GRANT](./grant-database-permissions-transact-sql.md) y especificando CREATE EXTERNAL LANGUAGE como privilegio.

Para modificar una biblioteca, se necesita el permiso `ALTER ANY EXTERNAL LANGUAGE`.

### <a name="execute-external-script-permission"></a>Permiso EXECUTE EXTERNAL SCRIPT

Puede usar permisos EXECUTE EXTERNAL SCRIPT para que se pueda conceder la ejecución de scripts externos en determinados lenguajes. No sucede lo mismo que con el permiso de base de datos EXECUTE ANY EXTERNAL SCRIPT, que no permite conceder el permiso de ejecución en un lenguaje específico.

Esto significa que es necesario conceder permiso a los usuarios que no son de **dbo** para ejecutar un lenguaje específico:

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::language_name 
TO database_principal_name;
```

### <a name="reference-permissions-to-external-libraries"></a>Permisos de referencia para bibliotecas externas

De forma similar a los ensamblados, los lenguajes externos requieren permisos de referencia para que se establezca un vínculo entre bibliotecas externas y lenguajes externos. Por ejemplo, si se va a quitar un lenguaje externo, en primer lugar el usuario debe asegurarse de que se quitarán todas las bibliotecas externas que hacen referencia a dicho idioma. Puede ver el lenguaje externo en una jerarquía, como un objeto de nivel más alto que las bibliotecas externas.

## <a name="examples"></a>Ejemplos

### <a name="a-create-an-external-language-in-a-database"></a>A. Crear un lenguaje externo en una base de datos  

En el ejemplo siguiente se agrega un lenguaje externo denominada Java a una base de datos de SQL Server en Windows.

```sql
CREATE EXTERNAL LANGUAGE Java 
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

### <a name="b-create-an-external-language-for-both-windows-and-linux"></a>B. Crear un lenguaje externo para Windows y Linux

Puede especificar hasta dos `<file_spec>`, uno para Windows y otro para Linux.

```sql
CREATE EXTERNAL LANGUAGE Java
FROM
(CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll', PLATFORM = WINDOWS),
(CONTENT = N'<path-to-tar.gz>', FILE_NAME = 'javaextension.so', PLATFORM = LINUX);
GO
```
### <a name="c-grant-permissions-to-execute-external-script"></a>C. Conceder permisos para ejecutar un script externo

En el ejemplo siguiente se concede acceso a la entidad **mylogin** para que ejecute scripts mediante el lenguaje externo de **Java**.

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::Java 
TO mylogin;
```


## <a name="see-also"></a>Consulte también

[ALTER EXTERNAL LANGUAGE (Transact-SQL)](alter-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)