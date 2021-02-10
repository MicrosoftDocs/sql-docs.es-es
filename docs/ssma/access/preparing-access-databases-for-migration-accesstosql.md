---
title: Preparar las bases de datos de Access para la migración (AccessToSQL) | Microsoft Docs
description: Obtenga información acerca de cómo determinar qué bases de datos de Access se van a migrar a SQL Server o Azure SQL Database y asegúrese de que dichas bases de datos están listas para la migración.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, versions
- Access databases, when to migrate
- Access databases, workgroup security
- backing up databases
- documenting databases
- files, preparing
- migrating databases, versions
- migrating databases, when to migrate
- versions of Access
- workgroup security
ms.assetid: 9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7c52b13a2a71afc005f19d1733e8dff435d2a582
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066590"
---
# <a name="preparing-access-databases-for-migration-accesstosql"></a>Preparar las bases de datos de Access para la migración (AccessToSQL)
Antes de migrar las bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe determinar qué bases de datos se van a migrar y asegurarse de que dichas bases de datos están listas para la migración.  
  
## <a name="determining-when-to-migrate-to-sql-server"></a>Determinar cuándo migrar a SQL Server  
El motor de base de datos Jet, que se utiliza como motor de base de datos para el acceso, es una solución flexible y fácil de usar para la administración de datos. Sin embargo, a medida que las bases de datos se hacen más grandes y más críticas, muchos usuarios descubren que necesitan mayor rendimiento, seguridad o disponibilidad. En el caso de las aplicaciones que requieren una plataforma de datos más sólida, considere la posibilidad de mover las bases de datos subyacentes para esas aplicaciones a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información acerca de cómo decidir cuándo migrar, consulte la [Página de información de migración](https://go.microsoft.com/fwlink/?LinkId=68571) en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sitio Web.  
  
Después de migrar las bases de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede seguir usando el acceso mediante el uso de tablas vinculadas, o puede migrar manualmente las aplicaciones a [!INCLUDE[msCoName](../../includes/msconame_md.md)] código basado en .NET Framework que interactúe directamente con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="determining-which-databases-to-migrate"></a>Determinar las bases de datos que se van a migrar  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para Access puede ubicar las bases de datos de Access. Después, puede exportar los metadatos de esas bases de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información sobre cómo exportar y consultar metadatos, consulte [exportación de un inventario de acceso](exporting-an-access-inventory-accesstosql.md).  

   > [!NOTE]
   > No se admiten todas las características y configuraciones de acceso de, o se pueden convertir fácilmente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Antes de empezar a migrar las bases de datos, consulte [características de acceso incompatibles](incompatible-access-features-accesstosql.md).
  
## <a name="preparing-for-migration"></a>Preparación de la migración  
Utilice las instrucciones siguientes como ayuda para preparar las bases de datos de Access para la migración a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="upgrading-older-access-databases"></a>Actualización de bases de datos de Access anteriores  
SSMA para Access admite el acceso 97 y versiones posteriores. Si tiene bases de datos de versiones anteriores de Access, abra y guarde las bases de datos en Access 97 o una versión posterior.  
  
### <a name="removing-workgroup-protection"></a>Quitar la protección de grupo de trabajo  
SSMA no puede migrar las bases de datos que usan la protección de grupo de trabajo. Para quitar la protección de grupo de trabajo de una base de datos de Access, realice los pasos siguientes:  
  
1.  Copie el archivo de base de datos de Access en otra ubicación.  
  
2.  Abra la base de datos copiada.  
  
3.  En el menú **herramientas** , seleccione **seguridad** y, a continuación, seleccione **permisos de usuario y de grupo**.  
  
4.  Seleccione la opción **usuarios** , seleccione el usuario **Administrador** y asegúrese de que está seleccionado el permiso **administrar** .  
  
5.  Seleccione la opción **grupos** , seleccione el grupo **usuarios** y asegúrese de que está seleccionado el permiso **administrar** .  
  
6.  Haga clic en **Aceptar** y, a continuación, en el menú **archivo** , haga clic en **salir**.  
  
Ahora puede usar SSMA para migrar la base de datos copiada. Después de cargar el esquema en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede proteger manualmente la base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="backing-up-databases"></a>Copias de seguridad de bases de datos  
Antes de migrar las bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe realizar una copia de seguridad de las bases de datos de Access que va a migrar, así como de las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de datos en las que va a migrar los datos y objetos de Access.  
  
Para realizar una copia de seguridad de una base de datos de Access, en el menú **herramientas** , seleccione **utilidades de base de datos** y, a continuación, **copia de seguridad de base de datos**.  
  
Para obtener información acerca de cómo realizar copias de seguridad de bases de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea "realizar copias de seguridad y restaurar bases de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] " en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
### <a name="documenting-databases"></a>Documentar bases de datos  
También es posible que desee documentar las propiedades, como las listas de objetos de base de datos, los tamaños de archivo y los permisos, de las bases de datos de Access. Para generar esta documentación en Access, en el menú **herramientas** , seleccione **analizar** y, a continuación, haga clic en **documentado**.  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Vincular aplicaciones de Access a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)
