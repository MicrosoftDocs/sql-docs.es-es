---
description: Modificar relaciones de claves externas.
title: Modificación de relaciones de claves externas | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65538
- vdt.ppg.relationships
helpviewer_keywords:
- foreign keys [SQL Server], modifying
- modifying foreign keys
ms.assetid: 0c9ca80d-d79b-44c4-a21e-0fce39c398ec
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a3e95908a2c816b070f02adf207b5e485f7777e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195746"
---
# <a name="modify-foreign-key-relationships"></a>Modificar relaciones de claves externas.

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Puede modificar el lado de clave externa de una relación en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Modificar los cambios de clave externa de una tabla cuyas columnas están relacionadas con las columnas de la tabla de clave principal.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para modificar una clave externa con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 La nueva columna de clave externa debe tener el mismo tipo de datos y el mismo tamaño que la columna de clave principal con la que está relacionada, con las siguientes excepciones:  
  
-   Se puede asociar una columna de tipo **char** o **sysname** a una columna de tipo **varchar** .  
  
-   Se puede asociar una columna de tipo **binary** a una columna de tipo **varbinary** .  
  
-   Se puede asociar un tipo de datos de alias a su tipo básico.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-modify-a-foreign-key"></a>Para modificar una clave externa  
  
1.  En el **Explorador de objetos**, expanda la tabla con la clave externa y luego expanda **Claves**.  
  
2.  Haga clic con el botón derecho en la clave externa que se va a modificar y seleccione **Modificar**.  
  
3.  En el cuadro de diálogo **Relaciones de clave externa** , puede realizar las siguientes modificaciones.  
  
     **Relación seleccionada**  
     Muestra las relaciones existentes. Seleccione una relación para mostrar sus propiedades en la cuadrícula situada a la derecha. Si la lista está vacía, no se han definido relaciones para la tabla.  
  
     **Add (Agregar)**  
     Crea una nueva relación. Debe definir **Especificación de tablas y columnas** para que la relación sea válida.  
  
     **Eliminar**  
     Elimina la relación seleccionada en la lista **Relaciones seleccionadas** . Para cancelar la adición de una relación, utilice este botón para eliminar la relación.  
  
     **Categoría General**  
     Se expande para mostrar **Comprobar datos existentes al crear o al habilitar de nuevo** y **Especificación de tablas y columnas**.  
  
     **Comprobar datos existentes al crear o al habilitar de nuevo**  
     Comprueba con la restricción todos los datos que había en la tabla antes de crear o habilitar de nuevo la restricción.  
  
     **Especificación de tablas y columnas (Categoría)**  
     Se expande para mostrar qué columnas actúan como clave externa y principal (o única) en la relación y a qué tablas pertenecen. Para editar o definir estos valores, haga clic en el botón de puntos suspensivos ( **...** ) situado a la derecha del campo de propiedad.  
  
     **Tabla base de clave externa**  
     Muestra la tabla que contiene la columna que actúa como clave externa en la relación seleccionada.  
  
     **Columnas de clave externa**  
     Muestra la columna que actúa como clave externa en la relación seleccionada.  
  
     **Tabla base de claves Primary/Unique**  
     Muestra la tabla que contiene la columna que actúa como clave principal (o única) en la relación seleccionada.  
  
     **Columnas de clave Primary/Unique**  
     Muestra la columna que actúa como clave principal (o única) en la relación seleccionada.  
  
     **Categoría Identidad**  
     Se expande para mostrar los campos de propiedades de **Nombre** y **Descripción**.  
  
     **Nombre**  
     Muestra el nombre de relación. Cuando se crea una nueva relación, se le da un nombre predeterminado que se basa en la tabla de la ventana activa del **Diseñador de tablas**. Puede cambiar el nombre en cualquier momento.  
  
     **Descripción**  
     Describe la relación. Para escribir una descripción más detallada, haga clic en **Descripción** y luego en los puntos suspensivos **(...)** que aparecen a la derecha del campo de propiedad. De este modo, obtendrá un área más grande en la que escribir el texto.  
  
     **Categoría Diseñador de tablas**  
     Se expande para mostrar la información de **Comprobar datos existentes al crear o al habilitar de nuevo** y **Exigir para replicación**.  
  
     **Exigir para replicación**  
     Indica si se exigirá la restricción cuando un agente de replicación realice una inserción, actualización o eliminación en esta tabla.  
  
     **Exigir restricción de clave externa**  
     Especifica si se pueden modificar los datos de las columnas de la relación y si estos cambios pueden invalidar la integridad de la relación de clave externa. Elija **Sí** si no desea permitir esos cambios y **No** si desea permitirlos.  
  
     **Especificación de INSERT y UPDATE (Categoría)**  
     Se expande para mostrar la información de **Regla de eliminación** y **Regla de actualización** de la relación.  
  
     **Regla de eliminación**  
     Especifica lo que sucede si un usuario intenta eliminar una fila con datos que están implicados en una relación de clave externa:  
  
    -   **Sin acción** Un mensaje de error indica al usuario que no se permite la eliminación y, a continuación, se revierte la eliminación.  
  
    -   **Cascada** Elimina todas las filas que contengan datos implicados en la relación de clave externa. No especifique CASCADE si la tabla se va a incluir en una publicación de combinación que utiliza registros lógicos.  
  
    -   **Establecer en Null** Establece el valor en NULL si todas las columnas de clave externa de la tabla aceptan valores NULL.  
  
    -   **Establecer predeterminado** Establece el valor predeterminado definido para la columna cuando todas las columnas de clave externa de la tabla tienen definidos valores predeterminados.  
  
     **Regla de actualización**  
     Especifica lo que sucede si un usuario intenta actualizar una fila con datos que están implicados en una relación de clave externa:  
  
    -   **Ninguna acción** Un mensaje de error indica al usuario que no se permite la actualización y, a continuación, se revierte la actualización.  
  
    -   **Cascada** Actualiza todas las filas que contengan datos implicados en la relación de clave externa. No especifique CASCADE si la tabla se va a incluir en una publicación de combinación que utiliza registros lógicos.  
  
    -   **Establecer en Null** Establece el valor en NULL si todas las columnas de clave externa de la tabla aceptan valores NULL.  
  
    -   **Establecer predeterminado** Establece el valor predeterminado definido para la columna si todas las columnas de clave externa de la tabla tienen valores predeterminados definidos.  
  
4.  En el menú **Archivo**, haga clic en ***Guardar**_nombre de tabla_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para modificar una clave externa**  
  
 Para modificar una restricción FOREIGN KEY mediante Transact-SQL, primero debe eliminar la restricción FOREIGN KEY existente y, a continuación, vuelva a crearla con la nueva definición. Para obtener más información, consulte [Delete Foreign Key Relationships](../../relational-databases/tables/delete-foreign-key-relationships.md) y [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md).  
  
###  <a name="TsqlExample"></a>  
