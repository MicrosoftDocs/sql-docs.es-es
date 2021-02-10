---
description: Botones de comando de la libreta de direcciones
title: Botones de comando de la libreta de direcciones | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO], command buttons
- RDS scenarios [ADO], command buttons
ms.assetid: 80676831-6488-4dad-a558-c47c52256a22
author: rothja
ms.author: jroth
ms.openlocfilehash: dccdb22981bb1662cd50da1e28db8fa0a2ce47d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032061"
---
# <a name="address-book-command-buttons"></a>Botones de comando de la libreta de direcciones
La aplicación de libreta de direcciones incluye los siguientes botones de comando:  
  
-   Un botón **Buscar** para enviar una consulta a la base de datos.  
  
-   Un botón **Borrar** para borrar los cuadros de texto antes de iniciar una nueva búsqueda.  
  
-   Un botón **Actualizar perfil** para guardar los cambios en un registro de empleado.  
  
-   Botón **Cancelar cambios** para descartar los cambios.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="find-button"></a>Botón buscar  
 Al hacer clic en el botón **Buscar** , se activa el procedimiento Sub de VBScript Find_OnClick, que compila y envía la consulta SQL. Al hacer clic en este botón, se rellena la cuadrícula de datos.  
  
## <a name="building-the-sql-query"></a>Compilar la consulta SQL  
 La primera parte del procedimiento Sub Find_OnClick genera la consulta SQL, una frase cada vez, anexando cadenas de texto a una instrucción SELECT de SQL global. Comienza estableciendo la variable `myQuery` en una instrucción SELECT de SQL que solicita todas las filas de datos de la tabla de origen de datos. A continuación, el procedimiento Sub examina cada uno de los cuatro cuadros de entrada de la página.  
  
 Dado que el programa utiliza la palabra `like` para compilar las instrucciones SQL, las consultas son búsquedas de subcadenas en lugar de coincidencias exactas.  
  
 Por ejemplo, si el cuadro **Last Name** contenía la entrada "Berge" y el cuadro **title** contenía la entrada "Program Manager", la instrucción SQL (valor de `myQuery` ) leería:  
  
```sql
Select FirstName, LastName, Title, Email, Building, Room, Phone from Employee where lastname like 'Berge%' and title like 'Program Manager%'  
```  
  
 Si la consulta se realiza correctamente, todas las personas con un apellido que contenga el texto "Berge" (por ejemplo, Berge y Berger) y con un título que contenga las palabras "Administrador de programas" (por ejemplo, administrador de programas, tecnologías avanzadas) se mostrarán en la cuadrícula de datos HTML.  
  
## <a name="preparing-and-sending-the-query"></a>Preparar y enviar la consulta  
 La última parte del procedimiento Sub Find_OnClick consta de dos instrucciones. La primera instrucción asigna la propiedad [SQL](../../reference/rds-api/sql-property.md) de [RDS. Objeto DataControl](../../reference/rds-api/datacontrol-object-rds.md) igual a la consulta SQL generada dinámicamente. La segunda instrucción produce el **RDS. Objeto DataControl** ( `DC1` ) para consultar la base de datos y, a continuación, mostrar los nuevos resultados de la consulta en la cuadrícula.  
  
```vb
Sub Find_OnClick  
   '...  
   DC1.SQL = myQuery  
   DC1.Refresh  
End Sub  
```  
  
## <a name="update-profile-button"></a>Botón actualizar perfil  
 Al hacer clic en el botón **Actualizar perfil** , se activa el procedimiento Sub de VBScript Update_OnClick, que ejecuta [RDS.](../../reference/rds-api/datacontrol-object-rds.md) `DC1` Los métodos [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) y [Refresh](../../reference/rds-api/refresh-method-rds.md) del objeto DataControl ().  
  
```vb
Sub Update_OnClick  
   DC1.SubmitChanges  
   DC1.Refresh  
End Sub  
```  
  
 Cuando `DC1.SubmitChanges` se ejecuta, el servicio de datos remoto empaqueta toda la información de actualización y la envía al servidor a través de http. La actualización es All-or-Nothing; Si una parte de la actualización no se realiza correctamente, no se realiza ningún cambio y se devuelve un mensaje de estado. `DC1.Refresh` no es necesario después de **SubmitChanges** con el servicio de datos remoto, pero garantiza datos actualizados.  
  
## <a name="cancel-changes-button"></a>Botón Cancelar cambios  
 Al hacer clic en **Cancelar cambios** , se activa el procedimiento Sub de VBScript Cancel_OnClick, que ejecuta [RDS. Objeto DataControl](../../reference/rds-api/datacontrol-object-rds.md) ( `DC1)` método [CancelUpdate](../../reference/rds-api/cancelupdate-method-rds.md) .  
  
```vb
Sub Cancel_OnClick  
   DC1.CancelUpdate  
End Sub  
```  
  
 Cuando `DC1.CancelUpdate` se ejecuta, descarta cualquier modificación que haya realizado un usuario en un registro de empleado en la cuadrícula de datos desde la última consulta o actualización. Restaura los valores originales.  
  
## <a name="see-also"></a>Consulte también  
 [Botones de navegación de la libreta de direcciones](./address-book-navigation-buttons.md)   
 [Objeto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)