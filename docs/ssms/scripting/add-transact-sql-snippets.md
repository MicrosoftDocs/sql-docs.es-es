---
title: Agregar fragmentos de código de Transact-SQL
description: Obtenga información sobre cómo agregar fragmentos de código de Transact-SQL propios al conjunto de fragmentos de código predefinidos que se incluyen en SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 901c7995-8eb5-4d12-8bb0-de0a922b48f8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 013551caad163e2172c725df7df5a30209d11979
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354825"
---
# <a name="add-transact-sql-snippets"></a>Agregar fragmentos de código de Transact-SQL

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Puede agregar sus propios fragmentos de código de Transact-SQL al conjunto de fragmentos de código predefinidos que se incluyen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="creating-a-transact-sql-snippet-file"></a>Crear un archivo de fragmento de código de Transact-SQL  
 La primera parte de la creación de un fragmento de código de [!INCLUDE[tsql](../../includes/tsql-md.md)] consiste en crear un archivo XML con el texto del fragmento de código. El archivo debe tener una extensión de archivo .snippet y cumplir los requisitos del tema sobre [referencia de esquemas de fragmento de código](/previous-versions/visualstudio/visual-studio-2015/ide/code-snippets-schema-reference). El lenguaje del fragmento de código se establece en SQL.  
  
 Puede utilizar los fragmentos de código predefinidos que se proporcionan con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como ejemplos. Para buscar los fragmentos de código predefinidos, abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], seleccione el menú **Herramientas** y haga clic en **Administrador de fragmentos de código**. Seleccione **SQL** en el cuadro de lista **Lenguaje** ; la ruta de acceso a los fragmentos de código de [!INCLUDE[tsql](../../includes/tsql-md.md)] se muestra en el cuadro **Ubicación** .  
  
## <a name="registering-the-code-snippet"></a>Registrar el fragmento de código  
 Después de crear el archivo de fragmento de código, utilice el Administrador de fragmentos de código para registrar el fragmento de código con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Puede agregar una carpeta que contiene varios fragmentos de código o importar fragmentos de código individuales a la carpeta **Mis fragmentos de código** .  
  
## <a name="procedures"></a>Procedimientos  
  
#### <a name="adding-a-snippet-folder"></a>Agregar una carpeta de fragmentos de código  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Seleccione el menú **Herramientas** y haga clic en **Administrador de fragmentos de código**.  
  
3.  Haga clic en el botón **Agregar**.  
  
4.  Navegue a la carpeta que contiene los fragmentos de código y haga clic en el botón **Seleccionar carpeta** .  
  
#### <a name="importing-a-snippet"></a>Importar un fragmento de código  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Seleccione el menú **Herramientas** y haga clic en **Administrador de fragmentos de código**.  
  
3.  Haga clic en el botón **Import** (Importar).  
  
4.  Navegue a la carpeta que contiene el fragmento de código, haga clic en el archivo .snippet y haga clic en el botón **Abrir** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea un fragmento de código rodear con **TRY-CATCH** y se importa a [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
1.  Pegue el código siguiente en el Bloc de notas y, a continuación, guárdelo como un archivo denominado TryCatch.snippet.  
  
    ```  
    <?xml version="1.0" encoding="utf-8" ?>  
    <CodeSnippets  xmlns="https://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">  
    <_locDefinition xmlns="urn:locstudio">  
        <_locDefault _loc="locNone" />  
        <_locTag _loc="locData">Title</_locTag>  
        <_locTag _loc="locData">Description</_locTag>  
        <_locTag _loc="locData">Author</_locTag>  
        <_locTag _loc="locData">ToolTip</_locTag>  
       <_locTag _loc="locData">Default</_locTag>  
    </_locDefinition>  
    <CodeSnippet Format="1.0.0">  
    <Header>  
    <Title>TryCatch</Title>  
                            <Shortcut></Shortcut>  
    <Description>Example Snippet for Try-Catch.</Description>  
    <Author>SQL Server Books Online Example</Author>  
    <SnippetTypes>  
                                    <SnippetType>SurroundsWith</SnippetType>  
    </SnippetTypes>  
    </Header>  
    <Snippet>  
    <Declarations>  
                                    <Literal>  
                                    <ID>CatchCode</ID>  
                                    <ToolTip>Code to handle the caught error</ToolTip>  
                                    <Default>CatchCode</Default>  
                                    </Literal>  
    </Declarations>  
    <Code Language="SQL"><![CDATA[  
    BEGIN TRY  
  
    $selected$ $end$  
  
    END TRY  
    BEGIN CATCH  
  
    $CatchCode$  
  
    END CATCH;  
    ]]>  
    </Code>  
    </Snippet>  
    </CodeSnippet>  
    </CodeSnippets>  
    ```  
  
2.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
3.  Seleccione el menú **Herramientas** y haga clic en **Administrador de fragmentos de código**.  
  
4.  Haga clic en el botón **Import** (Importar).  
  
5.  Navegue a la carpeta que contiene el archivo TryCatch.snippet, haga clic en el citado archivo y haga clic en el botón **Abrir** . Ahora debería tener un fragmento de código TryCatch en la carpeta **Mis fragmentos de código**.  
  
## <a name="see-also"></a>Consulte también  
 [Insertar fragmentos de código de Transact-SQL para rodear](./insert-surround-with-transact-sql-snippets.md)  
