---
description: Conexión a Azure SQL Database (MySQLToSQL)
title: Conexión a Azure SQL Database (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 81623d27-25af-444f-9779-1edb8c6fb470
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 44f97ee6686bbe61a90e5fccf2cfb165e7e97cf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069056"
---
# <a name="connect-to-azure-sql-database-mysqltosql"></a>Conexión a Azure SQL Database (MySQLToSQL)
Utilice el cuadro de diálogo conectar con SQL Azure para conectarse a la base de datos de Azure SQL Database que desea migrar.  
  
Para obtener acceso a este cuadro de diálogo, en el menú **archivo** , seleccione **conectarse a SQL Azure**. Si ya se ha conectado, el comando se **vuelve a conectar a SQL Azure.**  
  
## <a name="options"></a>Opciones  
**Nombre de servidor**  
  
Seleccione o escriba el nombre del servidor para conectarse a SQL Azure.  
  
**Base de datos**  
  
Seleccione, escriba o **examine** el nombre de la base de datos.  
  
> [!IMPORTANT]  
> SSMA para MySQL no admite la conexión a la base de datos maestra en SQL Azure.  
  
**Nombre de usuario**  
  
Escriba el nombre de usuario que SSMA usará para conectarse a Azure SQL Database  
  
**Contraseña**  
  
Escriba la contraseña del nombre de usuario.  
  
**Encrypt**  
  
SSMA recomienda una conexión cifrada para SQL Azure.  
  
## <a name="create-azure-database"></a>Creación de una base de datos de Azure  
Si no hay ninguna base de datos en la cuenta de SQL Azure, puede crear la primera base de datos.  
  
Para crear una nueva base de datos por primera vez, siga los pasos siguientes:  
  
1.  Haga clic en el botón examinar que se encuentra en el cuadro de diálogo conectar con SQL Azure  
  
2.  Si no hay ninguna base de datos, aparecen los dos elementos de menú siguientes.  
  
    1.  **(no se encontró ninguna base de datos)** que está deshabilitada y atenuada todo el tiempo  
  
    2.  **Cree una nueva base de datos** que solo esté habilitada cuando no haya bases de datos en SQL Azure cuenta. Al hacer clic en este elemento de menú, el cuadro de diálogo crear base de datos de Azure aparece con el nombre y el tamaño de la base de datos.  
  
3.  En el momento de la creación de la base de datos, los dos parámetros siguientes se proporcionan como entrada:  
  
    1.  **Nombre de la base de datos:** Escriba el nombre de la base de datos.  
  
    2.  **Tamaño de la base de datos:** Seleccione el tamaño de la base de datos que necesita crear en SQL Azure cuenta.  
  
