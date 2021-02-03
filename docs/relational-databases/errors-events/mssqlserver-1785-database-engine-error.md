---
description: MSSQLSERVER_1785
title: MSSQLSERVER_1785
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1785 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: f083af775defa1b95eab75e1635e67ad79abbf93
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196623"
---
# <a name="mssqlserver_1785"></a>MSSQLSERVER_1785
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|1785|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|CRTFKINVTOPO|
|Texto del mensaje|Si especifica la restricción FOREIGN KEY "%.ls" en la tabla "%. ls", podrían producirse ciclos o múltiples rutas en cascada. Especifique ON DELETE NO ACTION o UPDATE NO ACTION, o bien modifique otras restricciones FOREIGN KEY.|
||

## <a name="explanation"></a>Explicación

Recibe este mensaje de error porque, en SQL Server, una tabla no puede aparecer más de una vez en una lista de todas las acciones referenciales en cascada iniciadas por una instrucción `DELETE` o `UPDATE`. El árbol de acciones referenciales en cascada solo debe tener una ruta de acceso a una tabla determinada en el árbol de acciones referenciales en cascada.

El usuario recibe un mensaje de error similar al siguiente:

> Servidor:  Mensaje 1785, nivel 16, estado 1, línea 1 Si especifica la restricción FOREIGN KEY 'fk_two' en la tabla 'table2', podrían producirse ciclos o múltiples rutas en cascada. Especifique ON DELETE NO ACTION o UPDATE NO ACTION, o bien modifique otras restricciones FOREIGN KEY. Servidor:  Mensaje 1750, nivel 16, estado 1, línea 1 No se pudo crear la restricción. Consulte los errores anteriores

## <a name="user-action"></a>Acción del usuario

Para resolver este problema, cree una clave externa que cree una ruta de acceso única a una tabla en una lista de acciones referenciales en cascada.

Puede aplicar la integridad referencial de varias maneras. La integridad referencial declarativa (DRI) es la forma más básica, pero también la menos flexible. Si necesita más flexibilidad, pero aún desea un alto grado de integridad, puede usar desencadenadores en su lugar.

## <a name="more-information"></a>Más información

El código de ejemplo siguiente es un ejemplo de un intento de creación de FOREIGN KEY que genera el mensaje de error:

```sql
USE tempdb
GO

CREATE TABLE table1 (user_ID INTEGER NOT NULL PRIMARY KEY, user_name
CHAR(50) NOT NULL)
GO

CREATE TABLE table2 (author_ID INTEGER NOT NULL PRIMARY KEY, author_name
CHAR(50) NOT NULL, lastModifiedBy INTEGER NOT NULL, addedby INTEGER NOT NULL)
GO

ALTER TABLE table2 ADD CONSTRAINT fk_one FOREIGN KEY (lastModifiedby)
REFERENCES table1 (user_ID) ON DELETE CASCADE ON UPDATE cascade
GO

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1(user_ID) ON DELETE NO ACTION ON UPDATE cascade
GO
--this fails with the error because it provides a second cascading path to table2.

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1 (user_ID) ON DELETE NO ACTION ON UPDATE NO ACTION
GO
-- this works.
```

### <a name="see-also"></a>Consulte también

[Integridad referencial en cascada](../tables/primary-and-foreign-key-constraints.md#referential-integrity)