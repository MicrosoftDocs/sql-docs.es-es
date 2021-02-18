---
title: Scripting del motor de base de datos
description: Obtenga información sobre cómo puede usar el entorno de scripting de Microsoft PowerShell para administrar instancias del Motor de base de datos de SQL Server y cómo puede crear y ejecutar consultas del motor de base de datos que contienen Transact-SQL y XQuery.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], PowerShell
- scripts [SQL Server]
- scripting [SQL Server Database Engine]
- scripting [SQL Server Database Engine], PowerShell
ms.assetid: 9978a884-59a2-4e7f-a82a-335149f3a261
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d02d4fabf61d54390ce3eb1d411fed41209e378
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354701"
---
# <a name="database-engine-scripting"></a>Scripting del motor de base de datos
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  El [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] admite el entorno de scripting de [!INCLUDE[msCoName](../../includes/msconame-md.md)] PowerShell para administrar las instancias del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y los objetos en las instancias. También puede generar y ejecutar consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] que contengan [!INCLUDE[tsql](../../includes/tsql-md.md)] y XQuery en entornos muy similares a los de scripts.  
  
## <a name="sql-server-powershell"></a>SQL Server PowerShell  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye dos complementos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell que implementan:  
  
-   Un proveedor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell que expone las jerarquías del modelo de objetos de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como rutas de acceso de PowerShell que son similares a las rutas de acceso al sistema de archivos. Puede utilizar las clases del modelo de objetos de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para administrar los objetos representados en cada nodo de la ruta de acceso.  
  
-   Un conjunto de cmdlets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que implementan los comandos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Uno de los cmdlets es **Invoke-Sqlcmd**. Se utiliza para ejecutar scripts de consultas del [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se van a ejecutar con la utilidad **sqlcmd** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona estas características para ejecutar PowerShell:  
  
-   El módulo **sqlps** de PowerShell, que se puede importar en una sesión de PowerShell y que carga los complementos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede ejecutar interactivamente los comandos de PowerShell ad hoc. Puede ejecutar archivos de script utilizando un comando como .\MyFolder\MyScript.ps1.  
  
-   Los archivos de script de PowerShell se pueden utilizar como entrada de los pasos de trabajo de PowerShell del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecutan los scripts a intervalos programados o como respuesta a los eventos del sistema.  
  
-   La utilidad **sqlps** que inicia PowerShell e importa el módulo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Después puede realizar todas las acciones que admite el módulo. Puede iniciar la utilidad **sqlps** en un símbolo del sistema o haciendo clic con el botón derecho en los nodos del árbol del Explorador de objetos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio y seleccionando **Iniciar PowerShell**.  
  
## <a name="database-engine-queries"></a>Consultas del motor de base de datos  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] contienen tres tipos de elementos:  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   Instrucciones del lenguaje XQuery  
  
-   Comandos y variables de la utilidad **sqlcmd** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona tres entornos para generar y ejecutar consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] :  
  
-   Puede ejecutar y depurar interactivamente consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Puede codificar y depurar varias instrucciones en una sesión; a continuación, puede guardar todas las instrucciones en un único archivo de script.  
  
-   La utilidad de símbolo del sistema **sqlcmd** permite ejecutar interactivamente consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y también permite ejecutar archivos de script de consulta de [!INCLUDE[ssDE](../../includes/ssde-md.md)] existentes.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] se suelen codificar interactivamente en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usando el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . El archivo se puede abrir después en uno de estos entornos:  
  
-   Use el menú [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Archivo**/**Abrir** para abrir el archivo en una nueva ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Use el parámetro **-i**_archivo_entrada_ para ejecutar el archivo con la utilidad **sqlcmd** .  
  
-   Use el parámetro **-QueryFromFile** para ejecutar el archivo con el cmdlet **Invoke-Sqlcmd** en los scripts de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell.  
  
-   Utilice los pasos de trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del Agente [!INCLUDE[tsql](../../includes/tsql-md.md)] para ejecutar los scripts a intervalos programados o en respuesta a los eventos del sistema.  
  
 Además, puede utilizar el Asistente para scripts de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el fin de generar scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Puede hacer clic con el botón derecho en los objetos del Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, después, seleccionar el elemento de menú **Generar script** . **Generar script** inicia el asistente, que le guía a través del proceso de creación de un script.  
  
## <a name="database-engine-scripting-tasks"></a>Scripting del motor de base de datos  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Describe cómo usar el código y los editores de texto en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para desarrollar, para depurar, y ejecutar interactivamente los scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] .|[Editores de consultas y texto &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)|  
|Describe cómo usar la utilidad de **sqlcmd** para ejecutar scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] del símbolo del sistema, incluida la capacidad de desarrollar de forma interactiva los scripts.|[Temas de procedimientos sobre sqlcmd](./sqlcmd-start-the-utility.md)|  
|Describe cómo integrar los componentes de SQL Server en un entorno de Windows PowerShell y, a continuación, compilar scripts de PowerShell y administrar instancias y objetos de SQL Server.|[SQL Server PowerShell](../../powershell/sql-server-powershell.md)|  
|Describe cómo usar el asistente de **Generar y publicar scripts** para crear scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] que vuelven a crear los objetos de una base de datos.|[Generar scripts &#40;SQL Server Management Studio&#41;](./generate-scripts-sql-server-management-studio.md)|  
  
## <a name="see-also"></a>Consulte también  
 [sqlcmd (utilidad)](../../tools/sqlcmd-utility.md)   
 [Tutorial: Escribir instrucciones Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
