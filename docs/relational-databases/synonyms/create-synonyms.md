---
description: Crear sinónimos
title: Creación de sinónimos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- sql13.swb.synonym.general.f1
helpviewer_keywords:
- creating synonyms
- synonyms [SQL Server], creating
ms.assetid: fedfa7a5-d0b6-4e2b-90f4-a08122958e33
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 47da9230787a74198314c335e0227a54f38c1561
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250196"
---
# <a name="create-synonyms"></a>Crear sinónimos
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo crear un sinónimo en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para crear un sinónimo, mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Para crear un sinónimo en un esquema determinado, el usuario debe tener el permiso CREATE SYNONYM y ser propietario del esquema o tener el permiso ALTER SCHEMA. El permiso CREATE SYNONYM se puede conceder.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-synonym"></a>Para crear un sinónimo  
  
1.  En el **Explorador de objetos**, expanda la base de datos donde desea crear la nueva vista.  
  
2.  Haga clic con el botón derecho en la carpeta **Sinónimos** y después haga clic en **Nuevo sinónimo...**.  
  
3.  En el cuadro de diálogo **Agregar sinónimo** , escriba la siguiente información.  

     **Nombre de sinónimo**  
     Escriba el nombre que desea utilizar para este objeto.  
  
     **Esquema de sinónimos**  
     Escriba el esquema del nuevo nombre que desea utilizar para este objeto.  
  
     **Nombre del servidor**  
     Escriba la instancia de servidor a la que va a conectarse.  
  
     **Nombre de la base de datos**  
     Escriba o seleccione la base de datos que contiene el objeto.  
  
     **Esquema**  
     Escriba o seleccione el esquema al que pertenece el objeto.  
  
     **Tipo de objeto**  
     Seleccione el tipo de objeto.  
  
     **Nombre de objeto**  
     Escriba el nombre del objeto al que hace referencia el sinónimo.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-synonym"></a>Para crear un sinónimo  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue los ejemplos siguientes en la ventana de consulta y haga clic en **Ejecutar**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En el siguiente ejemplo se crea un sinónimo para una tabla existente en la base de datos de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . El sinónimo se utiliza en los ejemplos siguientes.  
  
```  
USE tempdb;  
GO  
CREATE SYNONYM MyAddressType  
FOR AdventureWorks2012.Person.AddressType;  
GO  
```  
  
 En el siguiente ejemplo se inserta una fila en la tabla base a la que hace referencia el sinónimo `MyAddressType` .  
  
```  
USE tempdb;  
GO  
INSERT INTO MyAddressType (Name)  
VALUES ('Test');  
GO  
```  
  
 En el siguiente ejemplo se muestra cómo se puede hacer referencia a un sinónimo en SQL dinámica.  
  
```  
USE tempdb;  
GO  
EXECUTE ('SELECT Name FROM MyAddressType');  
GO  
```  
  
  
