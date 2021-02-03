---
description: Referencia de los métodos del tipo de datos hierarchyid
title: hierarchyid (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- hierarchyid
- hierarchyid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Hierarchy data type
- hierarchyid data type
ms.assetid: 69b756e0-a1df-45b3-8a24-6ded8658aefe
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b11771fe7c2e73854a2214e7218cff1bfda1250f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165147"
---
# <a name="hierarchyid-data-type-method-reference"></a>Referencia de los métodos del tipo de datos hierarchyid
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

El tipo de datos **hierarchyid** es un tipo de datos del sistema de longitud variable. Use **hierarchyid** para representar la posición en una jerarquía. Una columna de tipo **hierarchyid** no representa automáticamente un árbol. Dependerá de la aplicación generar y asignar los valores **hierarchyid** de tal forma que la relación deseada entre las filas se refleje en los valores.
  
Un valor del tipo de datos **hierarchyid** representa una posición en una jerarquía de árbol. Los valores de **hierarchyid** tienen las siguientes propiedades.
  
-   Muy compactos  
     El número medio de bits necesarios para representar un nodo en un árbol con *n* nodos depende del promedio de distribución ramificada secundarios (el promedio de elementos secundarios de un nodo). Para distribuciones ramificadas pequeñas (0-7), el tamaño es aproximadamente 6\*logA *n* bits, donde A es el promedio de distribución ramificada. Un nodo en una jerarquía organizativa de 100.000 personas con un promedio de nodos secundarios de 6 niveles supone aproximadamente 38 bits. Esto se redondea a 40 bits (o 5 bytes) para el almacenamiento.  
-   La comparación se realiza con prioridad a la profundidad  
     Dados dos valores **hierarchyid****a** y **b**, **a<b** significa que a viene antes que b en un corte transversal de prioridad a la profundidad del árbol. Los índices de los tipos de datos **hierarchyid** están en orden con prioridad a la profundidad y los nodos cercanos entre sí en un corte transversal de prioridad a la profundidad se almacenan casi uno junto a otro. Por ejemplo, los elementos secundarios de un registro se almacenan adyacentes a ese registro. Para más información, vea [Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md).  
-   Compatibilidad con inserciones y eliminaciones arbitrarias  
     Con el método [GetDescendant](../../t-sql/data-types/getdescendant-database-engine.md) siempre es posible generar un miembro del mismo nivel a la derecha de cualquier nodo determinado, a la izquierda de cualquier nodo determinado, o entre dos miembros cualesquiera del mismo nivel. Se mantiene la propiedad comparison cuando se inserta o elimina un número arbitrario de nodos de la jerarquía. La mayoría de las inserciones y eliminaciones conservan la propiedad compactness. Sin embargo, las inserciones entre dos nodos generarán valores hierarchyid con una representación ligeramente menos compacta.  
-   La codificación usada en el tipo **hierarchyid** está limitada a 892 bytes. Por consiguiente, el tipo **hierarchyid** no podrá representar los nodos con demasiados niveles en su representación como para caber en los 892 bytes.  
  
El tipo **hierarchyid** está disponible para clientes CLR como el tipo de datos **SqlHierarchyId**.
  
## <a name="remarks"></a>Observaciones  
El tipo **hierarchyid** codifica lógicamente la información de un nodo único en un árbol de jerarquía mediante la codificación de la ruta de acceso desde la raíz del árbol hasta el nodo. Este tipo de ruta de acceso se representa lógicamente como una secuencia de etiquetas de nodo de todos los elementos secundarios visitados después de la raíz. Una barra diagonal inicia la representación y una barra diagonal única representa una ruta de acceso que solo visita la raíz. Para los niveles por debajo de la raíz, cada etiqueta se codifica como una secuencia de enteros separados por puntos. La comparación entre los elementos secundarios se realiza comparando las secuencias de enteros separados por puntos en orden alfabético. Una barra diagonal termina cada nivel. Por consiguiente, una barra diagonal separa los elementos primarios de los secundarios. Por ejemplo, estas son rutas de acceso de **hierarchyid** válidas con longitudes 1, 2, 2, 3 y 3 niveles, respectivamente:
  
-   /  
  
-   /1/  
  
-   /0.3.-7/  
  
-   /1/3/  
  
-   /0.1/0.2/  
  
Pueden insertarse los nodos en cualquier ubicación. Los nodos insertados después de **/1/2/** pero antes de **/1/3/** pueden representarse como **/1/2.5/**. Los nodos insertados antes de 0 tienen la representación lógica de un número negativo. Por ejemplo, un nodo que viene antes de **/1/1/** puede representarse como **/1/-1/**. Los nodos no pueden tener ceros a la izquierda. Por ejemplo, **/1/1.1/** es válido, pero **/1/1.01/** no lo es. Para evitar errores, inserte los nodos mediante el método [GetDescendant](../../t-sql/data-types/getdescendant-database-engine.md).
  
## <a name="data-type-conversion"></a>Conversión de tipos de datos
El tipo de datos **hierarchyid** puede convertirse a otros tipos de datos de la siguiente manera:
-   Use el método [ToString()](../../t-sql/data-types/tostring-database-engine.md) para convertir el valor **hierarchyid** a la representación lógica como un tipo de datos **nvarchar(4000)**.  
-   Use [Read ()](../../t-sql/data-types/read-database-engine.md) y [Write ()](../../t-sql/data-types/write-database-engine.md) para convertir **hierarchyid** en **varbinary**.  
-   Para transmitir los parámetros de **hierarchyid** a través de SOAP, conviértalos previamente a cadenas.  
  
## <a name="upgrading-databases"></a>Actualizar bases de datos
Cuando se actualiza una base de datos a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], se instalan automáticamente el nuevo ensamblado y el tipo de datos **hierarchyid**. Las reglas de asesor de actualizaciones detectan tipos de usuario o ensamblados con nombres problemáticos. El asesor de actualizaciones aconsejará el cambio de nombre de los ensamblados problemáticos, y el cambio de nombre de los tipos problemáticos o la utilización de nombres de dos partes en el código para hacer referencia al tipo de usuario preexistente.
  
Si una actualización de la base de datos detecta un ensamblado del usuario con un nombre problemático, cambiará automáticamente el nombre del ensamblado y colocará la base de datos en modo sospechoso.
  
Si durante la actualización se encuentra un tipo de usuario con un nombre problemático, no se llevará a cabo ningún procedimiento especial. Después de la actualización, existirán tanto el tipo de usuario anterior como el nuevo tipo de sistema. El tipo de usuario solo estará disponible a través de nombres de dos partes.
  
## <a name="using-hierarchyid-columns-in-replicated-tables"></a>Usar columnas hierarchyid en tablas replicadas
Las columnas de tipo **hierarchyid** pueden usarse en cualquier tipo de tabla replicada. Los requisitos para su aplicación dependen de si la replicación es unidireccional o bidireccional, y de las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usadas.
  
### <a name="one-directional-replication"></a>Replicación unidireccional
La replicación unidireccional incluye la replicación de instantáneas, replicación transaccional y replicación de mezcla en las que las modificaciones no se realizan en el suscriptor. El funcionamiento de las columnas **hierarchyid** con la replicación unidireccional depende de la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que el suscriptor ejecute.
-   Un publicador de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] puede replicar las columnas **hierarchyid** en un suscriptor de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] sin ninguna consideración especial.  
-   Un publicador de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] debe convertir las columnas **hierarchyid** para replicarlas a un suscriptor que esté ejecutando [!INCLUDE[ssEW](../../includes/ssew-md.md)] o una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] y las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admiten columnas **hierarchyid**. Si está usando una de estas versiones, todavía puede replicar los datos a un suscriptor. Para ello, debe establecer una opción de esquema o el nivel de compatibilidad de la publicación (para la replicación de mezcla) de modo que la columna se pueda convertir en un tipo de datos compatible.  
  
Se admite el filtrado de columnas en ambos escenarios. Esto incluye el filtrado de las columnas **hierarchyid**. Se admite el filtrado de filas con tal de que el filtro no incluya una columna **hierarchyid**.
  
### <a name="bi-directional-replication"></a>Replicación bidireccional
La replicación bidireccional incluye la replicación transaccional con suscripciones de actualización, replicación transaccional del mismo nivel y replicación de mezcla en la que se realizan los cambios en el suscriptor. Con la replicación se puede configurar una tabla con columnas **hierarchyid** para replicación bidireccional. Tenga en cuenta los siguientes requisitos y consideraciones.
-   El publicador y todos los suscriptores deben ejecutar [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
-   La replicación replica los datos como bytes y no realiza ninguna validación para garantizar la integridad de la jerarquía.  
-   No se mantiene la jerarquía de las modificaciones realizadas en el origen (suscriptor o publicador) cuando se replican en el destino.  
-   Los valores hash de las columnas **hierarchyid** son específicos de la base de datos en la que se generan. Por consiguiente, se pudo generar el mismo valor en el publicador y suscriptor, pero se pudo aplicar a filas diferentes. La replicación no comprueba esta condición y, a diferencia de las columnas IDENTITY, no hay ningún medio integrado de particionar los valores de la columna **hierarchyid**. Las aplicaciones deben usar restricciones u otros mecanismos para evitar estos conflictos sin detectar.  
-   Es posible que las filas insertadas en el suscriptor sean huérfanas. Es posible que la fila primaria de la fila insertada se haya eliminado en el publicador. Esto producirá un conflicto sin detectar cuando la fila del suscriptor se inserte en el publicador.  
-   Los filtros de columnas no pueden filtrar las columnas **hierarchyid** que no admiten valores NULL: la operación de inserción desde el suscriptor generará un error porque no hay ningún valor predeterminado de la columna **hierarchyid** en el publicador.  
-   Se admite el filtrado de filas con tal de que el filtro no incluya una columna **hierarchyid**.  
  
## <a name="see-also"></a>Consulte también
[Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[Referencia de los métodos del tipo de datos hierarchyid]()
  
