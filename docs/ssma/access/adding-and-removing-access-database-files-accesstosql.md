---
title: Agregar y quitar archivos de base de datos de Access (AccessToSQL) | Microsoft Docs
description: Obtenga información sobre cómo agregar o quitar bases de datos de Access en o desde el proyecto de SSMA para migrar datos de Access a SQL Server o Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- adding Access files
- adding and removing Access databases
- browsing Access metadata
- browsing metadata, Access databases
- connecting, Access databases
- databases
- files, adding and removing
- finding Access databases
- finding database files
- locating database files
- metadata
- metadata, browsing
- metadata, refreshing
- refreshing metadata
- removing Access files
- scanning for database files
- searching for database files
ms.assetid: e944c740-4c8a-4bc1-b0ed-be57bc06dced
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d8443eaceb150bd0cf2c936e44cb29fc2292f89c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042055"
---
# <a name="adding-and-removing-access-database-files-accesstosql"></a>Agregar y quitar archivos de base de datos de Access (AccessToSQL)
Para migrar datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, debe agregar una o varias bases de datos de Access al proyecto SSMA. Estas bases de datos deben ser Access 97 o versiones posteriores. Si tiene bases de datos de una versión anterior de Access, debe convertir las bases de datos en una versión más reciente. Para ello, abra y guarde las bases de datos en Access 97 o una versión posterior antes de agregarlas a SSMA.  
  
## <a name="what-happens-when-you-add-access-database-files"></a>¿Qué ocurre cuando se agregan archivos de base de datos de Access?  
Al agregar una base de datos de Access a un proyecto de SSMA, SSMA Lee los metadatos de la base de datos y, a continuación, agrega estos metadatos al archivo de proyecto. Estos metadatos describen la base de datos y sus objetos. SSMA usa los metadatos cuando convierte objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis o SQL Azure, y cuando migra datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede examinar estos metadatos en el explorador de metadatos de acceso y revisar las propiedades de cada uno de los objetos de base de datos.  
  
> [!NOTE]  
> Una base de datos de Access se puede dividir en varios archivos: una base de datos back-end que contiene tablas y bases de datos front-end que contienen consultas, formularios, informes, macros, módulos y accesos directos. Si desea migrar una base de datos dividida a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, agregue la base de datos de front-end a SSMA.  
  
## <a name="permissions-that-are-required-by-ssma"></a>Permisos requeridos por SSMA  
Para migrar una base de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, el grupo usuarios y el usuario administrador deben tener permisos para administrar. Para obtener información sobre cómo migrar bases de datos con protección de grupo de trabajo, vea [preparar las bases de datos de Access para la migración](preparing-access-databases-for-migration-accesstosql.md).  
  
## <a name="selecting-databases-to-add"></a>Seleccionar las bases de datos que se van a agregar  
Si desea agregar una o varias bases de datos a un proyecto de SSMA y todos los archivos están en una ubicación conocida, puede Agregar los archivos mediante el procedimiento siguiente.  
  
**Para agregar archivos de base de datos individuales**  
  
1.  En el menú **archivo** , haga clic en **Agregar bases de datos**.  
  
2.  En el cuadro de diálogo **abrir** , busque la carpeta que contiene el archivo o los archivos de la base de datos.  
  
3.  Seleccione los archivos que desea agregar y, a continuación, haga clic en **abrir**.  
  
## <a name="finding-databases-to-add"></a>Buscar bases de datos que agregar  
Si desea agregar varias bases de datos de Access de distintas carpetas a un proyecto de SSMA o desea agregar un solo archivo, pero tiene que buscar el archivo, puede seguir estos pasos para buscar uno de más archivos y agregarlos al proyecto.  
  
**Para buscar y agregar bases de datos**  
  
1.  En el menú **archivo** , haga clic en **Buscar bases de datos**.  
  
2.  En el asistente buscar bases de datos, escriba el nombre de la unidad, la ruta de acceso del archivo o la ruta de acceso UNC en la que desea realizar la búsqueda. O bien, haga clic en **examinar** para buscar la unidad o carpeta de red.  
  
3.  Haga clic en **Agregar** para agregar la ubicación a la lista.  
  
    Repita los dos pasos anteriores para agregar más ubicaciones de búsqueda.  
  
4.  Opcionalmente, agregue criterios de búsqueda para refinar la lista de bases de datos que se devuelven.  
  
    > [!IMPORTANT]  
    > El cuadro **de texto todo o parte del nombre de archivo** no admite caracteres comodín.  
  
5.  Haz clic en **Examen**.  
  
    Aparece la página digitalizar. Esto muestra las bases de datos que se han encontrado y el progreso de la búsqueda. Para detener la búsqueda, haga clic en **detener**.  
  
6.  En la página seleccionar archivos, seleccione las bases de datos que desea agregar al proyecto.  
  
    Puede usar los botones **seleccionar todo** y **Borrar todo** de la parte superior de la lista para seleccionar o borrar todas las bases de datos. Puede mantener presionada la tecla CTRL para seleccionar varias bases de datos o mantener presionada la tecla Mayús para seleccionar un intervalo de bases de datos.  
  
7.  Haga clic en **Siguiente**.  
  
8.  En la página comprobar, haga clic en **Finalizar**.  
  
## <a name="browsing-access-metadata"></a>Examinar metadatos de acceso  
Después de agregar una base de datos de Access a un proyecto, los metadatos del proyecto aparecen en el explorador de metadatos de Access. Puede examinar la jerarquía de bases de datos y objetos de base de datos en el explorador.  
  
**Para examinar los metadatos**  
  
1.  En el explorador de metadatos de Access, expanda **Access-metabase** y, a continuación, expanda **bases** de datos.  
  
2.  Expanda la base de datos que desea revisar y, a continuación, expanda **consultas**.  
  
    Observe la lista de consultas. Si selecciona una consulta, aparece una pestaña **SQL** y una pestaña **propiedades** en el panel derecho.  
  
3.  Expanda **tablas** y, a continuación, seleccione una tabla.  
  
    Observe que aparecen cuatro pestañas: **tabla**, **asignación de tipos**, **propiedades** y **datos**.  
  
4.  Expanda una tabla, expanda **claves** y, a continuación, seleccione una clave.  
  
    Las propiedades clave aparecen en el panel derecho.  
  
5.  Expanda **índices** y, a continuación, seleccione un índice.  
  
    Las propiedades del índice aparecen en el panel derecho.  
  
## <a name="refreshing-databases"></a>Actualización de bases de datos  
Si una base de datos de Access cambia después de agregar su archivo, puede actualizar los metadatos de la base de datos de Access.  
  
**Para actualizar los metadatos de Access**  
  
-   En el explorador de metadatos de Access, haga clic con el botón derecho en la base de datos y seleccione **actualizar desde base de** datos.  
  
## <a name="removing-databases"></a>Quitar bases de datos  
Puede quitar una base de datos de Access de un proyecto siguiendo estos pasos.  
  
**Para quitar una base de datos de un proyecto**  
  
1.  En el explorador de metadatos de Access, expanda **Access-metabase** y, a continuación, expanda **bases** de datos.  
  
2.  Haga clic con el botón secundario en la base de datos y seleccione **quitar base de datos**.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [conectarse a SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Creación y administración de proyectos](creating-and-managing-projects-accesstosql.md)  
