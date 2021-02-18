---
title: Insertar fragmentos de código de Transact-SQL para rodear
description: Aprenda a insertar un fragmento de código para rodear que sirva como punto de partida para colocar instrucciones en bloques de código.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- snippets [Transact-SQL], surround with
- IntelliSense [SQL Server], surround with snippets
- Transact-SQL snippets, surround with
ms.assetid: 5b5a8c6c-968e-4361-a7f5-9e2ac186d927
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a62f995d8f75eef26c6e27afdfac06f365251796
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354633"
---
# <a name="insert-surround-with-transact-sql-snippets"></a>Insertar fragmentos de código de Transact-SQL para rodear
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Un fragmento de código rodea con es una plantilla que puede utilizar como punto inicial al agregar un conjunto de instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] en un bloque BEGIN, IF o WHILE.  
  
## <a name="inserting-surround-with-snippets"></a>Insertar fragmentos de código Rodea con  
 Los fragmentos de código Rodea con se pueden iniciar de tres modos distintos: a través de un método abreviado de teclado, a través del menú **Editar** y a través del menú contextual.  
  
 Después de insertar el fragmento de código, debe cambiar el texto de reemplazo para formar una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] válida. Para obtener más información, vea [Completar fragmentos de código de Transact-SQL](./complete-transact-sql-snippets.md).  
  
#### <a name="to-insert-a-surround-with-snippet"></a>Para insertar un fragmento de código rodea con  
  
1.  En la ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , seleccione el conjunto de instrucciones que se van a incluir en el bloque.  
  
2.  Utilice uno de estos tres métodos para mostrar la lista de fragmentos de código rodea con:  
  
    -   Escriba CTRL+K, CTRL+S.  
  
    -   En el menú **Editar** , señale a **IntelliSense** y, a continuación, seleccione el comando **Rodear con** .  
  
    -   Haga clic con el botón derecho en el texto seleccionado y, después, seleccione el comando **Rodear con** del menú contextual.  
  
3.  Seleccione el nombre del fragmento de código (BEGIN, IF o WHILE) en la lista utilizando el mouse o escribiendo el nombre del fragmento de código y presionando TAB o ENTRAR.  
  
## <a name="see-also"></a>Consulte también  
 [Insertar fragmentos de código de Transact-SQL](./insert-transact-sql-snippets.md)  
  
