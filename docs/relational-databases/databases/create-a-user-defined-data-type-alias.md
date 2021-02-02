---
title: Creación de un alias de tipo de datos definido por el usuario | Microsoft Docs
description: Obtenga información sobre cómo crear un alias de tipo de datos definido por el usuario en SQL Server 2019 mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.userdefineddatatype.general.f1
- sql13.swb.new.datatype.properties.general.f1
helpviewer_keywords:
- alias data types [SQL Server], creating
ms.assetid: b1dd8413-0cd0-411b-a79b-1bb043ccc62d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bb3ec65ac712b64412896e011699cd57aa17c5be
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813577"
---
# <a name="create-a-user-defined-data-type-alias"></a>Crear un alias de tipo de datos definido por el usuario
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo crear un alias de tipo de datos definido por el usuario en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear un alias de tipo de datos definido por el usuario, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El nombre de un alias de tipo de datos definido por el usuario alias debe cumplir las reglas de los identificadores.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso CREATE TYPE en la base de datos actual y el permiso ALTER en *schema_name*. Si no se especifica *schema_name* , se aplican las reglas de resolución de nombres predeterminadas para determinar el esquema del usuario actual.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-user-defined-data-type"></a>Para crear un tipo de datos definido por el usuario  
  
1.  En el Explorador de objetos, expanda la opción **Bases de datos**, expanda una base de datos, expanda **Programación** y **Tipos**, haga clic con el botón derecho en **Tipos de datos definidos por el usuario** y haga clic en **Nuevo tipo de datos definido por el usuario**.  
  
     **Permitir valores NULL**  
     Especifique si el tipo de datos definido por el usuario puede aceptar valores NULL. La capacidad de admitir valores NULL de un tipo de datos existente definido por el usuario no puede modificarse.  
  
     **Tipo de datos**  
     Seleccione el tipo de datos base en el cuadro de lista. En el cuadro de lista se muestran todos los tipos de datos, excepto **geography**, **geometry**, **hierarchyid**, **sysname**, **timestamp** y **xml** . El tipo de un tipo de datos existente definido por el usuario no puede modificarse.  
  
     **Valor predeterminado**  
     Opcionalmente, seleccione un valor predeterminado para enlazarlo al alias de tipo de datos definido por el usuario.  
  
     **Longitud/Precisión**  
     Muestra la longitud o la precisión del tipo de datos, según proceda. **Longitud** se aplica a los tipos de datos definidos por el usuario basados en personajes; **Precisión** solo se aplica a los tipos de datos numéricos definidos por el usuario. La etiqueta cambia en función del tipo de datos seleccionado con anterioridad. Este cuadro no puede modificarse si el tipo de datos seleccionado tiene una longitud o precisión fija.  
  
     No se muestra la longitud de los tipos de datos **nvarchar(max)** , **varchar(max)** o **varbinary(max)** .  
  
     **Nombre**  
     Si va a crear un nuevo alias de tipo de datos definido por el usuario, escriba un nombre único que se usará en la base de datos para representar el tipo de datos definido por el usuario. El número máximo de caracteres debe coincidir con el del tipo de datos **sysname** del sistema. El nombre de un alias existente de tipo de datos definido por el usuario no puede modificarse.  
  
     **Regla**  
     Opcionalmente, seleccione una regla para enlazarla al alias de tipo de datos definido por el usuario.  
  
     **Escala**  
     Especifique el número máximo de dígitos decimales que se pueden almacenar a la derecha del separador decimal.  
  
     **Esquema**  
     Seleccione un esquema de la lista de esquemas disponibles para el usuario actual. La selección predeterminada es el esquema predeterminado del usuario actual.  
  
     **Storage**  
     Muestra el tamaño de almacenamiento máximo del alias de tipo de datos definido por el usuario. El tamaño de almacenamiento máximo varía en función de la precisión.  
  
    |Precision|Tamaño máximo de almacenamiento|  
    |-|-|  
    |1 - 9|5|  
    |10 - 19|9|  
    |20 - 28|13|  
    |29 - 38|17|  
  
     Para los tipos de datos **nchar** y **nvarchar** , el valor de almacenamiento siempre es el doble del valor de **Longitud**.  
  
     No se muestra el almacenamiento para los tipos de datos **nvarchar(max)** , **varchar(max)** o **varbinary(max)** .  
  
2.  En el cuadro de diálogo **Nuevo tipo de datos definido por el usuario** , en el cuadro **Esquema** , escriba el esquema al que pertenecerá el alias de tipo de datos o use el botón Examinar para seleccionar el esquema.  
  
3.  En el cuadro **Nombre** , escriba un nombre para el nuevo alias de tipo de datos.  
  
4.  En el cuadro **Tipo de datos** , seleccione el tipo de datos en el que se basará el nuevo alias.  
  
5.  Rellene los cuadros **Longitud**, **Precisión** y **Escala** si corresponde para el tipo de datos que esté creando.  
  
6.  Active la casilla **Permitir valores NULL** si el nuevo alias de tipo de datos puede permitir valores NULL.  
  
7.  En el área **Enlace** , rellene los cuadros **Predeterminado** o **Regla** si desea enlazar un valor predeterminado o una regla al nuevo alias de tipo de datos. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]no pueden crearse valores predeterminados ni reglas. Mediante [!INCLUDE[tsql](../../includes/tsql-md.md)]. El Explorador de plantillas incluye códigos de ejemplo para crear valores predeterminados y reglas.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-user-defined-data-type-alias"></a>Para crear un alias de tipo de datos definido por el usuario  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo siguiente se crea un alias de tipo de datos basado en el tipo de datos `varchar` suministrado por el sistema. El alias de tipo de datos `ssn` se usa para las columnas que almacenan números de la seguridad social de 11 cifras (999-99-9999). La columna no puede ser NULL.  
  
```sql  
CREATE TYPE ssn  
FROM varchar(11) NOT NULL ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Identificadores de base de datos](../../relational-databases/databases/database-identifiers.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)  
  
  
