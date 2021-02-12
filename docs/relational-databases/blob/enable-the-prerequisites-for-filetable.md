---
title: Habilitar los requisitos previos de FileTables | Microsoft Docs
description: Para usar FileTables, primero active FILESTREAM, especifique un directorio y establezca algunas opciones y niveles de acceso. Obtenga información sobre cómo cumplir todos los requisitos previos.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FileTables [SQL Server], prerequisites
ms.assetid: 6286468c-9dc9-4eda-9961-071d2a36ebd6
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: d5e02aa025d65fd7f3db6d1f5bd8f43e44566ac9
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552662"
---
# <a name="enable-the-prerequisites-for-filetable"></a>Habilitar los requisitos previos de FileTables
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Describe cómo habilitar los requisitos previos para crear y usar FileTables.  
  
##  <a name="enabling-the-prerequisites-for-filetable"></a><a name="EnablePrereq"></a> Habilitar los requisitos previos para FileTable  
 Para habilitar los requisitos previos para crear y usar FileTables, habilite los siguientes elementos:  
  
-   **En el nivel de instancia:**  
  
    -   [Habilitar FILESTREAM en el nivel de instancia](#BasicsFilestream)  
  
-   **En el nivel de base de datos:**  
  
    -   [Proporcionar un grupo de archivos de FILESTREAM en el nivel de base de datos](#filegroup)  
  
    -   [Habilitar el acceso no transaccional en el nivel de base de datos](#BasicsNTAccess)  
  
    -   [Especificar un directorio para FileTables en el nivel de base de datos](#BasicsDirectory)  
  
##  <a name="enabling-filestream-at-the-instance-level"></a><a name="BasicsFilestream"></a> Habilitar FILESTREAM en el nivel de instancia  
 Las FileTables amplían las capacidades de la característica FILESTREAM de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por lo tanto, debe habilitar FILESTREAM para el acceso de E/S de archivos en el nivel de Windows y en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de poder crear y usar FileTables.  
  
###  <a name="how-to-enable-filestream-at-the-instance-level"></a><a name="HowToFilestream"></a> Procedimientos para: habilitar FILESTREAM en el nivel de instancia  
 Para obtener información sobre cómo habilitar FILESTREAM, vea [Habilitar y configurar FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md).  
  
 Cuando se llame a **sp_configure** para habilitar FILESTREAM en el nivel de la instancia, tendrá que establecer la opción filestream_access_level en 2. Para obtener más información, vea [filestream access level (opción de configuración del servidor)](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).  
  
###  <a name="how-to-allow-filestream-through-the-firewall"></a><a name="firewall"></a> Procedimientos para: permitir FILESTREAM a través del firewall  
 Para obtener información acerca de cómo habilitar FILESTREAM a través del firewall, vea [Configure a Firewall for FILESTREAM Access](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md).  
  
##  <a name="providing-a-filestream-filegroup-at-the-database-level"></a><a name="filegroup"></a> Proporcionar un grupo de archivos de FILESTREAM en el nivel de base de datos  
 Para poder crear tablas FileTable en una base de datos, esta debe tener un grupo de archivos FILESTREAM. Para obtener más información sobre este requisito previo, vea [Crear una base de datos habilitada para FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md).  
  
##  <a name="enabling-non-transactional-access-at-the-database-level"></a><a name="BasicsNTAccess"></a> Habilitar el acceso no transaccional en el nivel de base de datos  
 Las FileTables permiten que las aplicaciones Windows obtengan un identificador de archivo de Windows en los datos FILESTREAM sin que sea necesaria ninguna transacción. Para permitir este acceso no transaccional a los archivos almacenados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe especificar el nivel deseado de acceso no transaccional en el nivel de base de datos para cada base de datos que contenga FileTables.  
  
###  <a name="how-to-check-whether-non-transactional-access-is-enabled-on-databases"></a><a name="HowToCheckAccess"></a> Procedimientos para: comprobar si el acceso no transaccional está habilitado en las bases de datos  
 Consulte la vista de catálogo [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md) y compruebe las columnas **non_transacted_access** y **non_transacted_access_desc**.  

```sql
SELECT DB_NAME(database_id), non_transacted_access, non_transacted_access_desc  
    FROM sys.database_filestream_options;  
GO  
```

###  <a name="how-to-enable-non-transactional-access-at-the-database-level"></a><a name="HowToNTAccess"></a> Procedimientos para: habilitar el acceso no transaccional en el nivel de base de datos  
 Los niveles disponibles de acceso no transaccional son FULL, READ_ONLY y OFF.  
  
 **Especificar el nivel de acceso no transaccional mediante Transact-SQL**  
 - Cuando **cree una base de datos nueva**, llame a la instrucción [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md) con la opción de FILESTREAM **NON_TRANSACTED_ACCESS**.

   ```sql
   CREATE DATABASE database_name  
     WITH FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' )  
   ```

- Cuando **modifique una base de datos existente**, llame a la instrucción [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) con la opción de FILESTREAM **NON_TRANSACTED_ACCESS**.

   ```sql
   ALTER DATABASE database_name  
     SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' )  
   ```

 **Especificar el nivel de acceso no transaccional mediante SQL Server Management Studio**  
 Puede especificar el nivel de acceso no transaccional en el campo **Acceso sin transacciones de FILESTREAM** de la página **Opciones** del cuadro de diálogo **Propiedades de la base de datos** . Para obtener más información sobre este cuadro de diálogo, vea [Propiedades de la base de datos &#40;página Opciones&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
##  <a name="specifying-a-directory-for-filetables-at-the-database-level"></a><a name="BasicsDirectory"></a> Especificar un directorio para FileTables en el nivel de base de datos  
 Cuando habilite el acceso no transaccional a archivos en el nivel de base de datos, puede indicar el nombre de un directorio opcionalmente al mismo tiempo mediante la opción **DIRECTORY_NAME** . Si no proporciona ningún directorio cuando habilite el acceso no transaccional, debe proporcionarlo posteriormente antes de que pueda crear FileTables en la base de datos.  
  
 En la jerarquía de carpetas de FileTable, este directorio de nivel de base de datos se convierte en el secundario del nombre del recurso compartido especificado para FILESTREAM en el nivel de instancia y en el primario de las FileTables creadas en la base de datos. Para más información, consulte [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
###  <a name="how-to-specify-a-directory-for-filetables-at-the-database-level"></a><a name="HowToDirectory"></a> Procedimientos para: especificar un directorio para FileTables en el nivel de base de datos  
 El nombre que especifique debe ser único en toda la instancia para los directorios de base de datos.  
  
**Especificar un directorio para FileTables mediante Transact-SQL**  
- Cuando **cree una base de datos nueva**, llame a la instrucción [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md) con la opción de FILESTREAM **DIRECTORY_NAME**.

   ```sql
   CREATE DATABASE database_name  
     WITH FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' );  
   GO  
   ```

-   Cuando **modifique una base de datos existente**, llame a la instrucción [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) con la opción de FILESTREAM **DIRECTORY_NAME**. Cuando use estas opciones para cambiar el nombre del directorio, la base de datos se debe bloquear de forma exclusiva y sin identificadores de archivo abiertos.  
  
    ```sql  
    ALTER DATABASE database_name  
        SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
-   Cuando **adjunte una base de datos**, llame a la instrucción [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md) con la opción **FOR ATTACH** y con la opción FILESTREAM de **DIRECTORY_NAME**.  
  
    ```sql  
    CREATE DATABASE database_name  
        FOR ATTACH WITH FILESTREAM ( DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
-   Cuando **restaure una base de datos**, llame a la instrucción [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) con la opción de FILESTREAM **DIRECTORY_NAME**.  
  
    ```sql  
    RESTORE DATABASE database_name  
        WITH FILESTREAM ( DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
 **Especificar un directorio para las FileTables mediante SQL Server Management Studio**  
 Puede especificar el nombre de un directorio en el campo **Nombre de directorio de FILESTREAM** de la página **Opciones** del cuadro de diálogo **Propiedades de la base de datos** . Para obtener más información sobre este cuadro de diálogo, vea [Propiedades de la base de datos &#40;página Opciones&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
###  <a name="how-to-view-existing-directory-names-for-the-instance"></a><a name="viewnames"></a> Procedimientos para: ver los nombres de directorio existentes para la instancia  
 Para ver la lista de nombres de directorio existentes de la instancia, consulte la vista de catálogo [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md) y compruebe la columna **filestream_database_directory_name**.  
  
```sql  
SELECT DB_NAME ( database_id ), directory_name  
    FROM sys.database_filestream_options;  
GO  
```  
  
###  <a name="requirements-and-restrictions-for-the-database-level-directory"></a><a name="ReqDirectory"></a> Requisitos y restricciones para el directorio de base de datos  
  
-   Establecer el valor de **DIRECTORY_NAME** es opcional cuando llame a **CREATE DATABASE** o a **ALTER DATABASE**. Si no especifica ningún valor para **DIRECTORY_NAME**, el nombre del directorio continúa siendo NULL y no podrá crear FileTables en la base de datos hasta que especifique un valor para **DIRECTORY_NAME** en el nivel de base de datos.  
  
-   El nombre de directorio que proporcione debe cumplir los requisitos del sistema de archivos de un nombre de directorio válido.  
  
-   Cuando la base de datos contenga FileTables, no puede volver a establecer el valor de **DIRECTORY_NAME** en NULL.  
  
-   Cuando adjunte o restaure una base de datos, se produce un error en la operación si la nueva base de datos tiene un valor para **DIRECTORY_NAME** que ya existe en la instancia de destino. Especifique un valor único para **DIRECTORY_NAME** cuando llame a **CREATE DATABASE FOR ATTACH** o a **RESTORE DATABASE**.  
  
-   Cuando actualice una base de datos existente, el valor de **DIRECTORY_NAME** será NULL.  
  
-   Cuando habilite o deshabilite el acceso no transaccional en el nivel de base de datos, la operación no comprueba si se ha especificado el nombre del directorio o si es único.  
  
-   Cuando quita una base de datos habilitada para FileTables, se quitan el directorio de nivel de base de datos y todas las estructuras de directorio de todas las FileTables contenidas en él.  
  
