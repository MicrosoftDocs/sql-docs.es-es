---
description: Pasos generales para solucionar problemas del Correo electrónico de base de datos
title: Solución de problemas del Correo electrónico de base de datos
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-dt-2019
ms.openlocfilehash: 2e9b7f5b5aec4f1b111983f893783e46ad14e021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352953"
---
# <a name="general-database-mail-troubleshooting-steps"></a>Pasos generales para solucionar problemas del Correo electrónico de base de datos 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

La solución de problemas del Correo electrónico de base de datos implica la comprobación de las siguientes áreas generales del sistema del Correo electrónico de base de datos. Estos procedimientos se presentan en un orden lógico, pero se pueden evaluar en cualquier orden.

## <a name="permissions"></a>Permisos

Debe ser miembro del rol fijo de servidor sysadmin para solucionar los problemas del Correo electrónico de base de datos. Los usuarios que no sean miembros del rol fijo de servidor sysadmin solo podrán obtener información relativa a los mensajes de correo electrónico que intenten enviar, pero no de los enviados por otros usuarios.

## <a name="is-database-mail-enabled"></a>¿Está habilitado el Correo electrónico de base de datos?

1. En SQL Server Management Studio, conéctese a una instancia de SQL Server mediante una ventana del editor de consultas y, a continuación, ejecute el código siguiente:

    ```sql
    sp_configure 'show advanced', 1; 
    GO
    RECONFIGURE;
    GO
    sp_configure;
    GO
    ```

   En el panel de resultados, confirme que run_value para [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) está establecido en 1.
   Si run_value no es 1, el Correo electrónico de base de datos no está habilitado. El Correo electrónico de base de datos no se habilita automáticamente para reducir el número de características disponibles en caso de ataque por parte de un usuario malintencionado. Para obtener más información, vea [Descripción de la configuración del área expuesta](../security/surface-area-configuration.md).

1. Si decide que es adecuado habilitar el Correo electrónico de base de datos, ejecute el código siguiente:

    ```sql
    sp_configure 'Database Mail XPs', 1; 
    GO
    RECONFIGURE;
    GO
    ```

1. Para restaurar el procedimiento sp_configure a su estado predeterminado, que no muestra las opciones avanzadas, ejecute el código siguiente:

    ```sql 
    sp_configure 'show advanced', 0; 
    GO
    RECONFIGURE;
    GO
    ```

## <a name="are-users-properly-configured-to-send-mail"></a>¿Están correctamente configurados los usuarios para enviar correo?

1. Para enviar Correo electrónico de base de datos, los usuarios deben ser miembros del rol de base de datos DatabaseMailUserRole en la base de datos msdb. Los miembros del rol fijo de servidor sysadmin y del rol db_owner de msdb son automáticamente miembros del rol DatabaseMailUserRole. Para obtener una lista de todos los demás miembros de DatabaseMailUserRole, ejecute la instrucción siguiente:

    ```sql
    EXEC msdb.sys.sp_helprolemember 'DatabaseMailUserRole';
    ```

1. Para agregar usuarios al rol DatabaseMailUserRole, utilice la instrucción siguiente:

    ```sql
    USE msdb;
    GO
    
    sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<database user>';
    ```

1. Para enviar mensajes del Correo electrónico de base de datos, los usuarios deben tener acceso al menos a un perfil del Correo electrónico de base de datos. Para obtener una lista de los usuarios (entidades de seguridad) y los perfiles a los que tienen acceso, ejecute la instrucción que se indica a continuación.

    ```sql
    EXEC msdb.dbo.sysmail_help_principalprofile_sp;
    ```

1. Utilice el Asistente para configuración de Correo electrónico de base de datos para crear perfiles y conceder acceso a los mismos a los usuarios.
 
## <a name="is-database-mail-started"></a>¿Se ha iniciado el Correo electrónico de base de datos?

1. El [Programa externo Correo electrónico de base de datos](database-mail-external-program.md) se activa cuando hay mensajes de correo electrónico que deben procesarse. El programa termina cuando ya no hay mensajes para enviar durante el período de tiempo de espera especificado. Para confirmar que se ha iniciado la activación del Correo electrónico de base de datos, ejecute la siguiente instrucción:

    ```sql
    EXEC msdb.dbo.sysmail_help_status_sp;
    ```
1. Si no se ha iniciado la activación del Correo electrónico de base de datos, ejecute la instrucción que se indica a continuación:

    ```sql
    EXEC msdb.dbo.sysmail_start_sp;
    ```

1. Si se ha iniciado el programa externo del Correo electrónico de base de datos, compruebe el estado de la cola de correo electrónico con la instrucción siguiente:

    ```sql
    EXEC msdb.dbo.sysmail_help_queue_sp @queue_type = 'mail';
    ```
  
   La cola debe tener el estado RECEIVES_OCCURRING. Su estado puede variar según el momento. Si el estado de la cola no es RECEIVES_OCCURRING, pruebe a reiniciar la cola. Detenga la cola con la siguiente instrucción:
   
```sql
EXEC msdb.dbo.sysmail_stop_sp;
```

Luego, iníciela con la siguiente instrucción:

```sql
EXEC msdb.dbo.sysmail_start_sp;
```

  > [!NOTE]
  >  Utilice la columna length del conjunto de resultados de sysmail_help_queue_sp para determinar el número de mensajes de correo electrónico que se encuentran en la cola de correo.

## <a name="do-problems-affect-some-or-all-accounts"></a>¿Hay problemas que afecten a algunas (o a todas) las cuentas?

1. Si ha determinado que solo algunos perfiles pueden enviar correo, puede que tenga problemas con las cuentas del Correo electrónico de base de datos utilizadas por los perfiles problemáticos. Para determinar qué cuentas envían correo correctamente, ejecute la instrucción siguiente:

    ```sql
    SELECT sent_account_id, sent_date FROM msdb.dbo.sysmail_sentitems;
    ```

1. Si un perfil que no funciona no utiliza ninguna de las cuentas indicadas, es posible que todas las cuentas disponibles para el perfil no funcionen correctamente. Para probar cuentas individuales, utilice el Asistente para configuración de Correo electrónico de base de datos para crear un nuevo perfil con una sola cuenta y, a continuación, utilice el cuadro de diálogo Enviar correo electrónico de prueba para enviar correo con la nueva cuenta. 
1. Para ver los mensajes de error devueltos por el Correo electrónico de base de datos, ejecute la instrucción siguiente:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log;
    ```

   > [!NOTE]
   > El Correo electrónico de base de datos considera que se ha enviado un mensaje cuando se ha entregado correctamente a un servidor de correo SMTP. La aparición de errores posteriores, como una dirección de correo electrónico de destinatario no válida, puede impedir que se entregue el mensaje, pero no constarán en el registro del Correo electrónico de base de datos.

## <a name="retry-mail-delivery"></a>Reintentar la entrega de correo

1. Si ha determinado que el Correo electrónico de base de datos genera un error porque no se puede alcanzar el servidor SMTP de forma confiable, quizás pueda aumentar la tasa de entrega correcta de correo aumentando el número de veces que el Correo electrónico de base de datos intenta enviar cada mensaje. Inicie el Asistente para configuración de Correo electrónico de base de datos y seleccione la opción Ver o cambiar parámetros del sistema. Como alternativa, puede asociar más cuentas al perfil de modo que, en caso de conmutación por error de la cuenta principal, el Correo electrónico de base de datos utilice la cuenta de conmutación por error para enviar mensajes de correo electrónico.
1. En la página Configurar parámetros del sistema, los valores predeterminados de cinco veces para Número de reintentos de cuenta y de 60 segundos para Intervalo entre reintentos de cuenta significan que la entrega de mensajes provocará un error si no se puede alcanzar el servidor SMTP en 5 minutos. Aumente los valores de estos parámetros para aumentar el tiempo tras el cual la entrega de mensajes provoca un error.

    > [!NOTE]
    > Cuando se envían grandes cantidades de mensajes, el hecho de aumentar los valores predeterminados puede aumentar la confiabilidad, pero aumentará sustancialmente la utilización de recursos, puesto que se intentará entregar un gran número de mensajes varias veces. Para tratar el problema de origen, resuelva el problema de red o de servidor SMTP que impide al Correo electrónico de base de datos ponerse en contacto con el servidor SMTP inmediatamente.



##  <a name="see-also"></a><a name="RelatedContent"></a> Vea también
  
-   [Objetos de configuración de Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-configuration-objects.md)  
  
-   [Objetos de mensajería de Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
-   [Programa externo Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-external-program.md)  
  
-   [Registro y auditorías del Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
-   [Configurar el Agente SQL Server para que use el Correo electrónico de base de datos](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
  
  
