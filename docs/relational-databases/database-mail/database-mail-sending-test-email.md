---
description: Envío de un correo electrónico de prueba con el Correo electrónico de base de datos
title: Envío de un correo electrónico de prueba con el Correo electrónico de base de datos | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 58b9594c7069347f7e458a5a375bdaf2e2d29103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272356"
---
# <a name="send-a-test-email-with-database-mail"></a>Envío de un correo electrónico de prueba con el Correo electrónico de base de datos  
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Utilice el cuadro de diálogo Enviar correo electrónico de prueba para probar la capacidad de enviar correo mediante un perfil específico.

## <a name="permissions"></a>Permisos

Debe ser miembro del rol fijo de servidor sysadmin para utilizar el cuadro de diálogo Enviar correo electrónico de prueba. Los usuarios que no sean miembros del rol fijo de servidor sysadmin podrán probar el Correo electrónico de base de datos mediante el procedimiento [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md).

## <a name="procedure"></a>Procedimiento

1. Conéctese mediante el Explorador de objetos a [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md), luego conéctese a una instancia del motor de base de datos de SQL Server en la que esté configurado el Correo electrónico de base de datos, expanda Administración, haga clic con el botón derecho en Correo electrónico de base de datos y, finalmente, haga clic en Enviar correo electrónico de prueba. Si no existe ningún perfil del Correo electrónico de base de datos, aparecerá un cuadro de diálogo para pedir al usuario que cree un perfil y se iniciará el Asistente para configuración de Correo electrónico de base de datos.
1. En el cuadro de diálogo **Enviar correo electrónico de prueba** desde <instance name> del perfil del Correo electrónico de base de datos, seleccione el perfil que quiera probar.
1. En el cuadro **Para**, escriba el nombre de correo electrónico del destinatario del mensaje de prueba.
1. En el cuadro **Asunto**, escriba la línea de asunto del mensaje de correo electrónico de prueba. Cambie el asunto predeterminado a fin de identificar más fácilmente el mensaje para solucionar el problema.
1. En el cuadro **Cuerpo**, escriba el cuerpo del mensaje de prueba. Cambie el asunto predeterminado a fin de identificar más fácilmente el mensaje para solucionar el problema.
1. Seleccione **Enviar correo electrónico de prueba** para enviar el mensaje de prueba a la cola del Correo electrónico de base de datos.
1. Al enviar el mensaje de prueba, se abre el cuadro de diálogo Correo electrónico de prueba de base de datos. Anote el número que aparece en el cuadro Correo electrónico enviado. Corresponde al valor mailitem_id del mensaje de prueba. Seleccione Aceptar.
1. En la barra de herramientas, seleccione Nueva consulta para abrir una ventana del Editor de consultas. Ejecute la siguiente instrucción T-SQL para determinar el estado del mensaje de correo electrónico de prueba:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_allitems 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```

    En la columna sent_status se indica si se ha enviado el mensaje de correo electrónico de prueba.

1. Si se produce un error, ejecute la instrucción siguiente para ver el mensaje correspondiente:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```


##  <a name="see-also"></a><a name="RelatedContent"></a> Vea también 
  
-   [Objetos de configuración de Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-configuration-objects.md)
-   [Objetos de mensajería de Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-messaging-objects.md)
-   [Programa externo Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-external-program.md)
-   [Registro y auditorías del Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-log-and-audits.md)
-   [Configurar el Agente SQL Server para que use el Correo electrónico de base de datos](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)
  
  
