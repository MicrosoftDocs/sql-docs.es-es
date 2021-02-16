---
title: Administración de servidores mediante la administración basada en directivas
description: Obtenga información sobre cómo utilizar la administración basada en directivas para administrar una o más instancias de SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- facet See facets
- Declarative Management Framework See Policy-Based Management
- surface area configuration [SQL Server], Policy-Based Management
- Policy-Based Management
- facets [Policy-Based Management]
- Policy-Based Management, administering
- conditions [Policy-Based Management]
- facets [Policy-Based Management], about facets
- PolicyAdministratorRole role
ms.assetid: ef2a7b3b-614b-405d-a04a-2464a019df40
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 721f2d692d95f8bdccbdf351193fb63c36ad146b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345518"
---
# <a name="administer-servers-by-using-policy-based-management"></a>Administrar servidores mediante administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   La administración basada en directivas es un sistema basado en directivas para administrar una o más instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Úsela para crear condiciones que contengan expresiones de condición. A continuación, cree directivas que apliquen las condiciones a los objetos de destino de la base de datos.  

Por ejemplo, como el administrador de la base de datos, puede que desee asegurarse de que ciertos servidores no tienen habilitada la opción Correo electrónico de base de datos, por lo que crea una condición y una directiva que establece esa opción de servidor. 
   
 > **IMPORTANTE:** Las directivas pueden afectar a la forma de funcionamiento de algunas características. Por ejemplo, la captura de datos modificados y la replicación transaccional usan ambas la tabla systranschemas, que carece de índice. Si habilita una directiva que indique que todas las tablas deben tener un índice, exigir el cumplimiento de la directiva hará que estas características generen un error.  
  
 Utilice SQL Server Management Studio a fin de crear y administrar directivas para:
  
1.  Seleccione una faceta de administración basada en directivas que contenga las propiedades que se van a configurar.  
  
2.  Defina una condición que especifique el estado de una faceta de administración.  
  
3.  Defina una directiva que contenga la condición, las condiciones adicionales que filtran los conjuntos de destinos y el modo de evaluación.  
  
4.  Compruebe si una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cumple la directiva.  
  
 En las directivas que no se cumplen, el Explorador de objetos indica una advertencia de estado crítico mediante un icono rojo junto al destino y los nodos superiores del árbol del Explorador de objetos.  
  
> **NOTA:** Cuando el sistema calcula el conjunto de objetos para una directiva, los objetos del sistema se excluyen de forma predeterminada.  Por ejemplo, si el conjunto de objetos de la directiva hace referencia a todas las tablas, la directiva no se aplicará a las tablas del sistema. Si los usuarios desean evaluar una directiva con los objetos del sistema, pueden agregar explícitamente objetos del sistema al conjunto de objetos. Sin embargo, aunque se admiten todas las directivas para el modo de evaluación **Comprobar en la programación** , por razones de rendimiento, no todas las directivas con conjuntos de objetos arbitrarios se admiten para el modo de evaluación **Comprobar en los cambios** . Para obtener más información, consulte: [https://docs.microsoft.com/archive/blogs/sqlpbm/policy-evaluation-modes](/archive/blogs/sqlpbm/policy-evaluation-modes)  
  
## <a name="three-policy-based-management-components"></a>Tres componentes de la administración basada en directivas  
 La administración basada en directivas tiene tres componentes:  
  
-   Administración de directivas. Los administradores de directivas crean las directivas.  
  
-   Administración explícita. Los administradores seleccionan uno o varios destinos administrados y comprueban explícitamente que los destinos cumplan una directiva concreta o hacen que la cumplan.  
  
-   Modos de evaluación. Hay cuatro modos de evaluación; tres se pueden automatizar:  
  
    -   **A petición**. Este modo evalúa la directiva cuando lo especifica el usuario directamente.  
  
    -   **Al cambiar: impedir**. Este modo automatizado utiliza desencadenadores DDL para evitar infracciones de las directivas.  
  
        > **IMPORTANTE:** Si la opción de configuración de servidor de desencadenadores anidados está deshabilitada, **On change: prevent** no funcionará correctamente. La administración basada en directivas se basa en los desencadenadores DDL para detectar y revertir las operaciones DDL que no obedecen a las directivas que utilizan este modo de evaluación. Al quitar los desencadenadores DDL de la administración basada en directivas o deshabilitar los desencadenadores anidados, este modo de evaluación provocará un error o se comportará de forma inesperada.  
  
    -   **Al cambiar: solo registrar**. Este modo automatizado utiliza la notificación de eventos para evaluar una directiva cuando se realiza un cambio relevante.  
  
    -   **Al programar**. Este modo automatizado utiliza un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para evaluar una directiva periódicamente.  
  
     Cuando las directivas automatizadas no estén habilitadas, la administración basada en directivas no afectará al rendimiento del sistema.  
  
## <a name="terms"></a>Términos  
 **Destino administrado por la administración basada en directivas** Entidades que se administran con la administración basada en directivas, como una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], una base de datos, una tabla o un índice. Todos los destinos de una instancia de servidor forman una jerarquía de destino. Un conjunto de destinos es el resultado de aplicar un conjunto de filtros de destino a la jerarquía de destino, por ejemplo, todas las tablas de la base de datos que es propiedad del esquema HumanResources.  
  
 **Faceta de administración basada en directivas** Conjunto de propiedades lógicas que modelan el comportamiento o las características de ciertos tipos de destinos administrados. El número y las características de las propiedades están integrados en la faceta y solo el fabricante de la faceta las puede agregar o quitar. Un tipo de destino puede implementar una o varias facetas de administración, y uno o varios tipos de destino pueden implementar una faceta de administración. Algunas propiedades de una faceta solo se pueden aplicar a una versión concreta.  
  
 **Condición de la administración basada en directivas**  
 Una expresión booleana que especifica un conjunto de estados permitidos de un destino administrado mediante la administración basada en directivas con respecto a una faceta de administración. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta observar las intercalaciones al evaluar una condición. Cuando las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no coinciden exactamente con las intercalaciones de Windows, compruebe su condición para determinar el modo en que el algoritmo resuelve los conflictos.  
  
 **Directiva de administración basada en directivas**  
 Una condición de administración basada en directivas y el comportamiento esperado, por ejemplo, el modo de evaluación, los filtros de destino y la programación. Una directiva solo puede contener una condición. Las directivas pueden estar habilitadas o deshabilitadas. Las directivas se almacenan en la base de datos msdb.  
  
 **Categoría de directiva de administración basada en directivas**  
 Categoría definida por el usuario para ayudar a administrar las directivas. Los usuarios pueden clasificar las directivas en categorías diferentes. Una directiva pertenece a una y solo una categoría. Las categorías de directiva se aplican a las bases de datos y servidores. En el nivel de base de datos, se aplican las condiciones siguientes:  
  
-   Los propietarios de base de datos pueden suscribir una base de datos a un conjunto de categorías de directiva.  
  
-   Solo las directivas de sus categorías subscritas pueden gobernar una base de datos.  
  
-   Todas las bases de datos se suscriben implícitamente a la categoría de directiva predeterminada.  
  
 En el nivel de servidor, las categorías de directiva se pueden aplicar a todas las bases de datos.  
  
 **Directiva efectiva**  
 Las directivas efectivas de un destino son las que lo gobiernan. Una directiva solo es efectiva con respecto a un destino si se satisfacen todas las condiciones siguientes:  
  
-   La directiva está habilitada.  
  
-   El destino pertenece al conjunto de destinos de la directiva.  
  
-   El destino o uno de los antecesores de los destinos se suscribe al grupo de directivas que contiene esta directiva.  
  
## <a name="links-to-specific-tasks"></a>Vínculos a tareas específicas 

 - [Almacenar directivas de administración basada en directivas.](policy-based-management-storage.md)
 - [Configurar alertas para notificar los errores de directiva a los administradores de directivas](../../relational-databases/policy-based-management/configure-alerts-to-notify-policy-administrators-of-policy-failures.md)
 - [Crear una nueva condición de administración basada en directivas](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md)
 - [Eliminar una condición de administración basada en directivas](../../relational-databases/policy-based-management/delete-a-policy-based-management-condition.md)
 - [Ver o modificar las propiedades de una condición de administración basada en directivas](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md)
 - [Exportar una directiva de administración basada en directivas](../../relational-databases/policy-based-management/export-a-policy-based-management-policy.md)
 - [Importar una directiva de administración basada en directivas](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md)
 - [Evaluar una directiva de administración basada en directivas desde un objeto](../../relational-databases/policy-based-management/evaluate-a-policy-based-management-policy-from-an-object.md)
 - [Trabajar con facetas de administración basada en directivas](../../relational-databases/policy-based-management/working-with-policy-based-management-facets.md)
 - [Supervisar y aplicar las prácticas recomendadas usando la administración basada en directivas](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)

## <a name="see-also"></a>Consulte también  
 
 - [Tutorial: Crear y aplicar una directiva Desactivado de forma predeterminada](lesson-1-create-and-apply-an-off-by-default-policy.md)
 - [Tutorial: Crear y aplicar una directiva de normas de denominación](lesson-2-create-and-apply-a-naming-standards-policy.md)
 - [Vistas de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)