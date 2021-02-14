---
title: Scripts de pruebas unitarias de SQL Server
description: Familiarícese con los scripts de prueba unitaria de SQL Server. Obtenga información sobre los scripts de prueba, los anteriores y posteriores a la prueba, y los scripts comunes TestInitialize y TestCleanup.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 80c5cf62-a9c9-4e9d-8c6f-8eed50a595a7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: eb725a491eb87a0116413a786c514a0ede98361f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066950"
---
# <a name="scripts-in-sql-server-unit-tests"></a>Scripts de pruebas unitarias de SQL Server

Cada prueba unitaria de SQL Server contiene una única acción anterior a la prueba, una acción de prueba y una acción posterior a la prueba. Cada una de estas acciones contiene, a su vez, lo siguiente:  
  
-   Un script Transact\-SQL que se ejecuta en una base de datos.  
  
-   Cero o más condiciones de prueba que evalúan los resultados devueltos de la ejecución del script.  
  
El script de prueba Transact\-SQL de la acción de prueba es el único componente que debe incluir en cada prueba unitaria de SQL Server. Además del propio script de prueba, probablemente desee especificar también condiciones de prueba para comprobar si el script de prueba devolvió el valor o el conjunto de valores que esperaba. La acción de prueba ejecuta o modifica un objeto determinado de esa base de datos y evalúa después ese cambio.  
  
En cada acción de prueba, puede incluir una acción anterior a la prueba y una acción posterior a la prueba. De forma similar a la acción de prueba, cada acción anterior a la prueba y cada acción posterior a la prueba contiene un script Transact\-SQL y cero o más condiciones de prueba. Puede usar una acción anterior a la prueba para asegurarse de que la base de datos está en un estado que permite que la acción de prueba se ejecute y devuelva resultados significativos. Por ejemplo, puede usar una acción previa a la prueba para comprobar que una tabla contiene datos antes de que el script de prueba realice una operación sobre esos datos. Cuando la acción anterior a la prueba haya preparado la base de datos y la acción de prueba haya devuelto resultados significativos, se puede usar la acción posterior a la prueba para devolver la base de datos al estado en que estaba antes de ejecutar la acción anterior a la prueba. O bien, en algunos casos, puede usar la acción posterior a la prueba para validar los resultados de la acción de prueba. Esto se debe a que la acción posterior a la prueba puede tener mayores privilegios de base de datos que la acción de prueba. Para obtener más información, consulte [Información general acerca de las cadenas de conexión y los permisos](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
Además de estas tres acciones, también hay dos scripts de prueba (denominados scripts comunes), que se ejecutan antes y después de cada prueba unitaria de SQL Server. Como resultado, se pueden ejecutar hasta cinco scripts Transact\-SQL durante la ejecución de una única prueba unitaria de SQL Server. Solo se requiere el script Transact\-SQL que se encuentra en la acción de prueba; los scripts comunes y los scripts de acción anterior y posterior a la prueba son opcionales.  
  
En la tabla siguiente se proporciona una lista completa de scripts asociados a cualquier prueba unitaria de SQL Server.  
  
|**Acción**|**Tipo de script**|**Descripción**|  
|--------------|-------------------|-------------------|  
|TestInitialize|Script común (inicialización)|(Opcional) Este script precede a todas las acciones de prueba y a todas las acciones anteriores a la prueba en la prueba unitaria. El script TestInitialize se ejecuta antes de cada prueba unitaria en una clase de prueba especificada. Este script se ejecuta con el contexto privilegiado.|  
|Anterior a la prueba|Script de prueba|(Opcional) Este script forma parte de la prueba unitaria. El script anterior a la prueba se ejecuta antes de la acción de prueba en una prueba unitaria. Este script se ejecuta con el contexto privilegiado.|  
|Prueba|Script de prueba|(Requerido) Este script forma parte de la prueba unitaria. Este script puede, por ejemplo, ejecutar un procedimiento almacenado que obtiene, inserta o actualiza valores de tabla. Este script se ejecuta con el contexto de ejecución.|  
|Posterior a la prueba|Script de prueba|(Opcional) Este script forma parte de la prueba unitaria. El script posterior a la prueba se ejecuta después de una prueba unitaria individual. Este script se ejecuta con el contexto privilegiado.|  
|TestCleanup|Script común (limpieza)|(Opcional) Este script sigue a la prueba unitaria. El script TestCleanup se ejecuta después de todas las pruebas unitarias en una clase de prueba especificada. Este script se ejecuta con el contexto privilegiado.|  
  
Para más información sobre los diferentes contextos de seguridad en los que se ejecuta cada uno de estos scripts, consulte [Información general acerca de las cadenas de conexión y los permisos](../ssdt/overview-of-connection-strings-and-permissions.md) y la sección sobre los permisos de pruebas unitarias de SQL Server en [Permisos necesarios para SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md).  
  
## <a name="order-in-which-scripts-are-run"></a>Orden en el que se ejecutan los scripts  
Es importante comprender el orden en el que se ejecuta cada script. Aunque no puede cambiar este orden, puede decidir qué scripts desea ejecutar. En la ilustración siguiente se incluye la selección de scripts que puede usar en una ejecución de pruebas que contiene dos pruebas unitarias de SQL Server y se muestra el orden en que se ejecutan:  
  
![Dos pruebas unitarias de bases de datos](../ssdt/media/twodatabaseunittests.png "Dos pruebas unitarias de bases de datos")  
  
> [!NOTE]  
> Si se ha configurado la implementación del proyecto de base de datos de SQL Server, se realiza al principio de la ejecución de pruebas, bajo la cadena de conexión del contexto privilegiado. Para más información, vea: [Cómo: Configurar una ejecución de prueba unitaria de SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
## <a name="initialization-and-cleanup-scripts"></a>Scripts de inicialización y de limpieza  
En el Diseñador de pruebas unitarias de SQL Server, los scripts TestInitialize y TestCleanup se denominan scripts comunes. En el ejemplo anterior se asume que las dos pruebas unitarias forman parte de la misma clase de prueba. Como resultado, comparten los mismos scripts TestInitialize y TestCleanup. Este es siempre el caso para todas las pruebas unitarias de una única clase de prueba. Sin embargo, si la serie de pruebas contiene pruebas unitarias de distintas clases de prueba, los scripts comunes para la clase de prueba asociada se ejecutarán antes y después de la serie de pruebas unitarias.  
  
Si solo escribe pruebas unitarias mediante el Diseñador de pruebas unitarias de SQL Server, quizás no esté familiarizado con el concepto de clase de prueba. Cada vez que se crea una prueba unitaria al abrir el menú **Prueba** y hacer clic en **Nueva prueba**, SQL Server Data Tools genera una clase de prueba. Las clases de prueba aparecen en el **Explorador de soluciones** con el nombre de prueba especificado, seguido de una extensión .cs o .vb. En cada clase de prueba, las pruebas unitarias individuales se almacenan como métodos de prueba. Sin embargo, independientemente del número de métodos de prueba (es decir, pruebas unitarias), cada clase de prueba puede tener un script TestInitialize y TestCleanup, o ninguno.  
  
Puede usar el script TestInitialize para preparar la base de datos de prueba y el script TestCleanup para devolver la base de datos de prueba a un estado conocido. Por ejemplo, puede usar TestInitialize para crear un procedimiento almacenado por el asistente que se ejecutará después, en el script de prueba, para probar otro procedimiento almacenado.  
  
## <a name="pre-test-and-post-test-scripts"></a>Scripts anteriores y posteriores a la prueba  
Los scripts asociados a las acciones anteriores y posteriores a la prueba varían probablemente de una prueba unitaria a la siguiente. Puede usar estos scripts para establecer cambios incrementales en la base de datos y, a continuación, limpiar los cambios.  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Usar condiciones de prueba en pruebas unitarias de SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
  
