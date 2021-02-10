---
description: Conexión a Azure SQL Database (SybaseToSQL)
title: Conexión a Azure SQL Database (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 96538007-1099-40c8-9902-edd07c5620ee
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 93d1901cd5701cbdea537b947aa93de64fe59661
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056720"
---
# <a name="connect-to-azure-sql-database--sybasetosql"></a>Conexión a Azure SQL Database (SybaseToSQL)
Utilice el cuadro de diálogo conectar con Azure SQL Database para conectarse a la base de datos de Azure SQL Database que desea migrar.  
  
Para obtener acceso a este cuadro de diálogo, en el menú **archivo** , seleccione **conectarse a Azure SQL Database**. Si ya se ha conectado, el comando se **vuelve a conectar a Azure SQL Database.**  
  
## <a name="options"></a>Opciones  
**Nombre de servidor**  
  
Seleccione o escriba el nombre del servidor para conectarse a Azure SQL Database.  
  
**Base de datos**  
  
Seleccione, escriba o **examine** el nombre de la base de datos.  
  
> [!IMPORTANT]  
> SSMA para Sybase no admite la conexión a la base de datos maestra en Azure SQL Database.  
  
**Nombre de usuario**  
  
Escriba el nombre de usuario que SSMA usará para conectarse a la base de datos de Azure SQL Database  
  
**Contraseña**  
  
Escriba la contraseña del nombre de usuario.  
  
**Encrypt**  
  
SSMA recomienda una conexión cifrada para Azure SQL Database.  
  
## <a name="create-azure-database"></a>Creación de una base de datos de Azure  
Si no hay ninguna base de datos en la cuenta de Azure SQL Database, puede crear la primera base de datos.  
  
Para crear una nueva base de datos por primera vez, siga los pasos siguientes:  
  
1.  Haga clic en el botón examinar que se encuentra en el cuadro de diálogo conectar con Azure SQL Database  
  
2.  Si no hay ninguna base de datos, aparecen los dos elementos de menú siguientes.  
  
    1.  **(no se encontró ninguna base de datos)** que está deshabilitada y atenuada todo el tiempo  
  
    2.  **Cree una nueva base de datos** que solo esté habilitada cuando no haya bases de datos en Azure SQL Database cuenta. Al hacer clic en este elemento de menú, el cuadro de diálogo crear base de datos de Azure aparece con el nombre y el tamaño de la base de datos.  
  
3.  En el momento de la creación de la base de datos, los dos parámetros siguientes se proporcionan como entrada:  
  
    1.  **Nombre de la base de datos:** Escriba el nombre de la base de datos.  
  
    2.  **Tamaño de la base de datos:** Seleccione el tamaño de la base de datos que necesita crear en Azure SQL Database cuenta.  
  
