---
title: Vincular aplicaciones de acceso a SQL Server Azure SQL Database | Microsoft Docs
description: Obtenga información sobre cómo vincular las tablas de Access a las tablas migradas para poder usar las aplicaciones de Access existentes con SQL Server o Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, linking to SQL Azure
- Access databases, linking to SQL Server
- auto-increment columns
- data types, unsupported
- hyperlink columns
- linking tables
- migrating databases, post-migration issues
- post-migration issues
- reference, post-migration issues
- refreshing linked tables
- slow performance
- unlinking tables
ms.assetid: 82374ad2-7737-4164-a489-13261ba393d4
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 53fbbd5a865594f46eacb75c9799b9c956efb2da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059052"
---
# <a name="linking-access-applications-to-sql-server---azure-sql-database-accesstosql"></a>Vinculación de aplicaciones de Access a SQL Server-Azure SQL Database (AccessToSQL)
Si desea usar las aplicaciones de Access existentes con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede vincular las tablas de Access originales a las tablas migradas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. La vinculación modifica la base de datos de Access de forma que las consultas, los formularios, los informes y las páginas de acceso a datos usan los datos de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database o en lugar de los datos de la base de datos de Access.  
  
> [!NOTE]  
> Las tablas de Access permanecen en el acceso, pero no se actualizan junto con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure actualizaciones. Después de vincular las tablas y comprobar la funcionalidad, es posible que desee eliminar las tablas de Access.  
  
## <a name="linking-access-and-sql-server-tables"></a>Vinculación de tablas de acceso y SQL Server  
Al vincular una tabla de Access a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla o SQL Azure, el motor de base de datos Jet almacena la información de conexión y los metadatos de la tabla, pero los datos se almacenan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Esta vinculación permite que las aplicaciones de Access operen en las tablas de Access, aunque las tablas y los datos reales están en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
> [!NOTE]  
> Si usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la autenticación de, la contraseña se almacena en texto no cifrado en las tablas de acceso vinculadas. Se recomienda usar la autenticación de Windows.  
  
**Para vincular tablas**  
  
1.  En el explorador de metadatos de Access, seleccione las tablas que desea vincular.  
  
2.  Haga clic con el botón derecho en **tablas** y seleccione **vínculo**.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para Access realiza una copia de seguridad de la tabla de acceso original y crea una tabla vinculada.  
  
Después de vincular las tablas, las tablas de SSMA aparecen con un icono de vínculo pequeño. En el acceso, las tablas aparecen con un icono "vinculado", que es un globo terráqueo con una flecha que apunta a ella.  
  
Al abrir una tabla en Access, los datos se recuperan mediante un cursor de conjunto de claves. Como resultado, en el caso de tablas grandes, no se recuperan todos los datos al mismo tiempo. Sin embargo, al examinar la tabla, Access recupera datos adicionales según sea necesario.  
  
> [!IMPORTANT]  
> Para vincular las tablas de Access con una base de datos de Azure, necesita SQL Server Native Client (SNAC) versión 10,5 o posterior.   
> Puede obtener la versión más reciente de SNAC del [Feature Pack de Microsoft® SQL Server® 2008 R2](https://www.microsoft.com/download/details.aspx?id=44272).  
  
## <a name="unlinking-access-tables"></a>Desvincular tablas de acceso  
Al desvincular una tabla de Access de una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla o SQL Azure, SSMA restaura la tabla de acceso original y sus datos.  
  
**Para desvincular tablas**  
  
1.  En el explorador de metadatos de Access, seleccione las tablas que desea desvincular.  
  
2.  Haga clic con el botón derecho en **tablas** y seleccione **desvincular**.  
  
## <a name="linking-tables-to-a-different-server"></a>Vinculación de tablas a un servidor diferente  
Si ha vinculado las tablas de Access a una instancia de SQL Server y posteriormente desea cambiar los vínculos a otra instancia, debe volver a vincular las tablas.  
  
**Para vincular tablas a un servidor diferente**  
  
1.  En el explorador de metadatos de Access, seleccione las tablas que desea desvincular.  
  
2.  Haga clic con el botón derecho en **tablas** y seleccione **desvincular**.  
  
3.  Haga clic en el botón **volver a conectar a SQL Server** .  
  
4.  Conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure a la que desea vincular las tablas de Access.  
  
5.  En el explorador de metadatos de Access, seleccione las tablas que desea vincular.  
  
6.  Haga clic con el botón derecho en **tablas** y seleccione **vínculo**.  
  
## <a name="updating-linked-tables"></a>Actualizar tablas vinculadas  
Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se modifican las definiciones de tabla o SQL Azure, puede desvincular y, a continuación, volver a vincular las tablas en SSMA mediante los procedimientos mostrados anteriormente en este tema. También puede actualizar las tablas mediante el acceso.  
  
**Para actualizar las tablas vinculadas mediante el acceso**  
  
1.  Abra la base de datos de Access.  
  
2.  En la lista **objetos** , haga clic en **tablas**.  
  
3.  Haga clic con el botón secundario en una tabla vinculada y seleccione **Administrador de tablas vinculadas**.  
  
4.  Active la casilla situada junto a cada tabla vinculada que desee actualizar y, a continuación, haga clic en **Aceptar**.  
  
## <a name="possible-post-migration-issues"></a>Posibles problemas posteriores a la migración  
En las secciones siguientes se enumeran los problemas que pueden producirse en las aplicaciones de Access existentes después de migrar las bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y, a continuación, vincular las tablas, junto con las causas y las resoluciones.  
  
### <a name="slow-performance-with-linked-tables"></a>Rendimiento lento con tablas vinculadas  
**Causa:** Algunas consultas pueden ser lentas después de realizar el ajuste de tamaño por los siguientes motivos:  
  
-   La aplicación depende de funciones que no existen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, lo que hace que jet Extraiga las tablas localmente para ejecutar una consulta Select.  
  
-   Jet envía las consultas que actualizan o eliminan muchas filas como una consulta con parámetros para cada fila.  
  
**Solución:** Convierta las consultas de ejecución lenta en consultas de paso a través, procedimientos almacenados o vistas. La conversión a consultas de paso a través tiene los siguientes problemas:  
  
-   Las consultas de paso a través no se pueden modificar. La modificación del resultado de la consulta o la adición de nuevos registros deben realizarse de una manera alternativa, como tener botones explícitos de **modificación** o **adición** en el formulario que esté enlazado a la consulta.  
  
-   Algunas consultas requieren la intervención del usuario, pero las consultas de paso a través no admiten la entrada del usuario. Los datos proporcionados por el usuario pueden obtenerse mediante código de Visual Basic para Aplicaciones (VBA) que solicita parámetros o un formulario que se usa como control de entrada. En ambos casos, el código VBA envía la consulta con la entrada del usuario al servidor.  
  
### <a name="auto-increment-columns-are-not-updated-until-the-record-is-updated"></a>Las columnas de incremento automático no se actualizan hasta que se actualiza el registro  
**Causa:** Después de llamar a RecordSet. AddNew en jet, la columna de incremento automático está disponible antes de que se actualice el registro. Esto no es cierto en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ni SQL Azure. El nuevo valor de la columna de identidad New Value solo está disponible después de guardar el nuevo registro.  
  
**Solución:** Ejecute el siguiente código de Visual Basic para Aplicaciones (VBA) antes de obtener acceso al campo de identidad:  
  
```  
Recordset.Update  
Recordset.Move 0,  
Recordset.LastModified  
```  
  
### <a name="new-records-are-not-available"></a>No hay nuevos registros disponibles  
**Causa:** Cuando se agrega un registro a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla o SQL Azure con VBA, si el campo de índice único de la tabla tiene un valor predeterminado y no se asigna un valor a ese campo, el nuevo registro no aparece hasta que se vuelve a abrir la tabla en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Si intenta obtener un valor del nuevo registro, recibirá el mensaje de error siguiente:  
  
`Run-time error '3167' Record is deleted.`  
  
**Solución:** Al abrir la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla o SQL Azure con código VBA, incluya la `dbSeeChanges` opción, como en el ejemplo siguiente:  
  
`Set rs = db.OpenRecordset("TestTable", dbOpenDynaset, dbSeeChanges)`  
  
### <a name="after-migration-some-queries-will-not-allow-the-user-to-add-a-new-record"></a>Después de la migración, algunas consultas no permitirán que el usuario agregue un nuevo registro  
**Causa:** Si una consulta no incluye todas las columnas incluidas en un índice único, no se pueden agregar nuevos valores mediante la consulta.  
  
**Solución:** Asegúrese de que todas las columnas incluidas en al menos un índice único forman parte de la consulta.  
  
### <a name="you-cannot-modify-a-linked-table-schema-with-access"></a>No se puede modificar un esquema de tabla vinculada con acceso  
**Causa:** Después de migrar datos y vincular tablas, el usuario no puede modificar el esquema de una tabla en Access.  
  
**Solución:** Modifique el esquema de tabla mediante y, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] a continuación, actualice el vínculo en Access.  
  
### <a name="hyperlink-functionality-is-lost-after-migrating-data"></a>La funcionalidad de hipervínculo se pierde después de migrar los datos  
**Causa:** Después de migrar los datos, los hipervínculos de las columnas pierden su funcionalidad y se convierten en columnas **nvarchar (Max)** simples.  
  
**Resolución:** Ninguno.  
  
### <a name="some-sql-server-data-types-are-not-supported-by-access"></a>Access no admite algunos tipos de datos SQL Server  
**Causa:** Si actualiza posteriormente las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas o SQL Azure para que contengan tipos de datos que no son compatibles con el acceso, no puede abrir la tabla en Access.  
  
**Solución:** Puede definir una consulta de acceso que solo devuelva las filas con tipos de datos admitidos.  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
