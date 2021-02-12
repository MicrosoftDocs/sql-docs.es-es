---
title: Creación de pruebas unitarias de SQL Server para funciones, desencadenadores y procedimientos almacenados
description: Obtenga información sobre usar el Explorador de objetos de SQL Server para crear una prueba unitaria de SQL Server a partir de una función de base de datos, un desencadenador o un procedimiento almacenado.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: bda57c10-a1ab-4a1a-8a71-42085a3cb793
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2f99946dfb56e3fc1e3e67f42ca93ff5daa11cab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018275"
---
# <a name="how-to-create-sql-server-unit-tests-for-functions-triggers-and-stored-procedures"></a>Procedimientos: Creación de pruebas unitarias de SQL Server para funciones, desencadenadores y procedimientos almacenados

Puede escribir pruebas unitarias que evalúen los cambios en cualquier objeto de base de datos. Sin embargo, SQL Server Data Tools incluye compatibilidad adicional para crear pruebas para funciones de base de datos, desencadenadores y procedimientos almacenados de un nodo de proyecto de base de datos del Explorador de objetos de SQL Server. El código auxiliar de Transact\-SQL puede generarse automáticamente para que pueda personalizarlo.  
  
### <a name="to-create-a-sql-server-unit-test-from-a-function-trigger-or-stored-procedure"></a>Para crear una prueba unitaria de SQL Server a partir de una función, un desencadenador o un procedimiento almacenado  
  
1.  Consulte [Tutorial: Crear y ejecutar una prueba unitaria de SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md) para obtener un ejemplo de cómo agregar una prueba unitaria a un procedimiento almacenado en la sección "Para crear una prueba unitaria de SQL Server para los procedimientos almacenados".  
  
    La condición de prueba no concluyente es la condición predeterminada que se agrega a cada prueba. Esta condición de prueba se incluye para indicar que la comprobación de la prueba no se ha implementado. Elimine esta condición de la prueba después de agregar otras condiciones de prueba. Para más información, vea: [Cómo: Incorporar condiciones de prueba a pruebas unitarias de SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Cómo: Crear una prueba unitaria de SQL Server vacía](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
  
