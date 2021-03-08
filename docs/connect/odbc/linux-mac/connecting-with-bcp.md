---
title: Conexión con bcp
description: Obtenga información sobre cómo usar la utilidad bcp con Microsoft ODBC Driver for SQL Server en Linux y macOS.
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- bcp
ms.assetid: 3eca5717-e50f-40db-be16-a1cebbdfee70
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f8bff2d5d9892d709885d0888e36ca042aa72242
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837401"
---
# <a name="connecting-with-bcp"></a>Conexión con bcp
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

La utilidad [bcp](../../../tools/bcp-utility.md) está disponible en [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en Linux y macOS. En esta página se documentan las diferencias de la versión de `bcp` de Windows.
  
- El terminador de campo es una tabulación ("\t").  
  
- El terminador de línea es una nueva línea ("\n").  
  
- El modo de carácter es el formato preferido de los archivos de formato `bcp` y de datos que no contienen caracteres extendidos.  
  
> [!NOTE]  
> Las barras diagonales inversas "\\" de los argumentos de línea de comandos deben entrecomillarse o contener caracteres de escape. Por ejemplo, para especificar una nueva línea como terminador de fila personalizado debe utilizar uno de los siguientes mecanismos:  
>   
> -   -r\\\n  
> -   -r"\n"  
> -   -r'\n'  
  
A continuación, se muestra una invocación de ejemplo de un comando de `bcp` para copiar filas de la tabla en un archivo de texto:  
  
```  
bcp AdventureWorks2008R2.Person.Address out test.dat -Usa -Pxxxx -Sxxx.xxx.xxx.xxx  
```  
  
## <a name="available-options"></a>Opciones disponibles
En la versión actual, se encuentran disponibles las siguientes opciones y sintaxis:  

[_database_ **.** ]_schema_ **.** _table_ **in** _data\_file_ | **out** _data\_file_

- -a *packet_size*  
Especifica el número de bytes por paquete de red enviados y recibidos por el servidor.  
  
- -b *batch_size*  
Especifica el número de filas por lote de datos importados.  
  
- -c  
Utiliza un tipo de datos de caracteres.  
  
- -d *database_name*  
Especifica la base de datos a la que conectarse.  
  
- -d  
Hace que el valor transmitido a la opción `bcp` -S se interprete como un nombre de origen de datos (DSN). Para obtener más información, vea la sección "Compatibilidad con DSN en sqlcmd y bcp" de [Connecting with sqlcmd](connecting-with-sqlcmd.md).  
  
- -e *error_file* especifica la ruta completa de un archivo de error que se usa para almacenar las filas que la utilidad `bcp` no puede transferir del archivo a la base de datos.  
  
- -E  
Utiliza el valor o los valores de identidad del archivo de datos importado en la columna de identidad.  
  
- -f *format_file*  
Especifica la ruta de acceso completa de un archivo de formato.  
  
- -F *first_row*  
Especifica el número de la primera fila que se exportará desde una tabla o que se importará desde un archivo de datos.  

- -G  
El cliente usa este modificador al conectarse a Azure SQL Database o a Azure Synapse Analytics para especificar que el usuario se autentica mediante la autenticación de Azure Active Directory. El modificador -G requiere al menos sqlcmd versión 17.6. Para determinar su versión, ejecute bcp -v.

> [!IMPORTANT]
> La opción `-G` solo se aplica a Azure SQL Database y a Azure Synapse Analytics.
> La autenticación interactiva de AAD no se admite actualmente ni en Linux ni en macOS. La autenticación integrada de AAD requiere la versión 17.6.1 o superior de [Microsoft ODBC Driver 17 for SQL  Server](../download-odbc-driver-for-sql-server.md) y un [entorno de Kerberos configurado](using-integrated-authentication.md#configure-kerberos) correctamente.

- -k  
Especifica que las columnas vacías deben conservar un valor NULL durante la operación, en vez de tener valores predeterminados para las columnas insertadas.  
  
- -l  
Especifica un tiempo de espera de inicio de sesión. La opción -l especifica el número de segundos que tienen que transcurrir antes de que un inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] agote el tiempo de espera cuando se trate de conectar a un servidor. El tiempo de espera de inicio de sesión predeterminado es de 15 segundos. El período de tiempo de espera de inicio de sesión debe ser un número comprendido entre 0 y 65534. Si el valor proporcionado no es numérico o no está dentro de este intervalo, `bcp` genera un mensaje de error. Un valor de 0 especifica un tiempo de espera infinito.
  
- -L *last_row*  
Especifica el número de la última fila que se exportará desde una tabla o que se importará desde un archivo de datos.  
  
- -m *max_errors*  
Especifica el número máximo de errores de sintaxis que pueden producirse antes de que se cancele la operación `bcp`.  
  
- -n  
Utiliza los tipos de datos nativos (de la base de datos) de los datos para realizar la operación de copia masiva.  
  
- -P *password*  
Especifica la contraseña para el identificador de inicio de sesión.  
  
- -q  
Ejecuta la instrucción SET QUOTED_IDENTIFIERS ON en la conexión entre la utilidad `bcp` y una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
- -r *row_terminator*  
Especifica el terminador de la fila.  
  
- -R  
Especifica que se realice la copia masiva de datos de moneda, fecha y hora en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con el formato regional definido para la configuración regional del equipo cliente.  
  
- -S *server*  
Especifica el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a la que se va a conectar o, si se utiliza -D, un DSN.  
  
- -t *field_terminator*  
Especifica el terminador del campo.  
  
- -T  
Especifica que la utilidad `bcp` se conecte a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con una conexión de confianza (seguridad integrada).  
  
- -U *login_id*  
Especifica el identificador de inicio de sesión para conectar con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
- -v  
Facilita información acerca del número de versión y de los derechos de autor de la utilidad `bcp`.  
  
- -w  
Utiliza caracteres Unicode para realizar la operación de copia masiva.  
  
En esta versión, se admiten los caracteres Latin-1 y UTF-16.  
  
## <a name="unavailable-options"></a>Opciones no disponibles
En la versión actual, no se encuentran disponibles las siguientes opciones y sintaxis:  

- -c  
Especifica la página de códigos de los datos incluidos en el archivo de datos.  
  
- -h *hint*  
Especifica las sugerencias que deben usarse durante una importación de datos masiva en una tabla o vista.  
  
- -i *input_file*  
Especifica el nombre de un archivo de respuesta.  
  
- -n  
Utiliza los tipos de datos nativos (de la base de datos) para datos que no sean de caracteres, así como datos Unicode para los datos de caracteres.  
  
- -o *output_file*  
Especifica el nombre de un archivo que recibe la salida redirigida desde el símbolo del sistema.  
  
- -V (80 | 90 | 100)  
Utiliza los tipos de datos de una versión anterior de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
- -X  
Si se usa con las opciones format y -f format_file, genera un archivo de formato basado en XML en lugar del archivo de formato predeterminado que no es XML.  
  
## <a name="see-also"></a>Consulte también

[Conexión con **sqlcmd**](connecting-with-sqlcmd.md)  
[Notas de la versión](release-notes-tools.md)