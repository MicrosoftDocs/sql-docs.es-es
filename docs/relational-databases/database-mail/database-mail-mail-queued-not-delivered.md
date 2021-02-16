---
description: 'Correo electrónico de base de datos: correo en cola, no entregado'
title: 'Correo electrónico de base de datos: Correo en cola, no entregado | Microsoft Docs'
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
ms.openlocfilehash: ebf8096c4b679cc6d5ac01b164bb9ae4bb014e6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352941"
---
# <a name="database-mail-mail-queued-not-delivered"></a>Correo electrónico de base de datos: correo en cola, no entregado 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

En este se tema describe cómo solucionar problemas en los que los mensajes de correo electrónico se colocan satisfactoriamente en la cola, pero no se entregan.

## <a name="diagnose-the-problem"></a>Diagnóstico del problema 

El programa externo de Correo electrónico de base de datos registra la actividad del correo electrónico en la base de datos **msdb**.

Primero, para confirmar que Correo electrónico de base de datos está habilitado, utilice la [opción Database Mail XPS](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) del procedimiento almacenado del sistema **sp_configure**, como en el siguiente ejemplo:

```sql 
EXEC sp_configure 'show advanced', 1;  
RECONFIGURE; 
EXEC sp_configure; 
GO
```

A continuación, ejecute la siguiente instrucción en la base de datos **msdb** para comprobar el estado de la cola de correo:

```sql
sysmail_help_queue_sp @queue_type = 'Mail' ;
```

Para obtener una explicación detallada de las columnas, vea [sysmail_help_queue_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-queue-sp-transact-sql.md#result-set).

Consulte la vista **sysmail_event_log** para conocer el nivel de actividad. La vista debe contener una entrada que indique que se ha iniciado el programa externo de Correo electrónico de base de datos. Si no hay ninguna entrada en la vista **sysmail_event_log**, vea el síntoma [Mensajes en cola, no hay entradas](database-mail-common-errors.md#database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log) en **sysmail_event_log**. Si hay errores en la vista **sysmail_event_log**, solucione este error concreto.

Si hay entradas en la vista **sysmail_event_log**, consulte la vista **sysmail_allitems** para conocer el estado de los mensajes.

## <a name="message-status-unsent"></a>Estado del mensaje: no enviado 

Un estado de **no enviado** indica que el [programa externo de Correo electrónico de base de datos](database-mail-external-program.md) todavía no ha procesado el mensaje de correo electrónico. Es posible que el programa externo de Correo electrónico de base de datos se haya retrasado en el procesamiento de los mensajes; la velocidad a la que el programa externo procesa los mensajes depende de las condiciones de la red, el tiempo de espera de reintento, el volumen de mensajes y la capacidad del servidor SMTP. Si el problema continúa, pruebe a utilizar más de un perfil para distribuir los mensajes entre más de un servidor SMTP.

Compruebe la fecha de notificación más reciente de los mensajes entregados satisfactoriamente. Si la última entrega correcta se ha producido hace algún tiempo, revise la vista sysmail_event_log para comprobar que el programa externo fue iniciado satisfactoriamente por Service Broker. Si el último intento no ha iniciado el programa externo, compruebe que el Programa externo de Correo electrónico de base de datos está ubicado en el directorio correcto, y que la cuenta de servicio de SQL Server dispone del permiso necesario para ejecutar el ejecutable.

   > [!NOTE]
   > Para eliminar los mensajes antiguos no enviados, espere hasta que los mensajes que no se pueden entregar sean los más antiguos de la cola y, a continuación, utilice [sysmail_delete_mailitems_sp](../system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) para eliminarlos.

## <a name="message-status-retrying"></a>Estado del mensaje: reintentando

El estado reintentando indica que el Correo electrónico de base de datos ha intentado entregar el mensaje al servidor SMTP pero no ha podido hacerlo. Normalmente esto se debe a una interrupción de la red, a un error del servidor SMTP o a que la cuenta de Correo electrónico de base de datos no se ha configurado correctamente. Finalmente, el mensaje debería entregarse o producir un error y publicar un mensaje en el registro de eventos.

## <a name="message-status-sent"></a>Estado del mensaje: enviado

Un estado de **enviado** indica que el programa externo de Correo electrónico de base de datos ha entregado satisfactoriamente el mensaje de correo electrónico al servidor SMTP. Si el mensaje no llegó a su destino, el servidor SMTP aceptó el mensaje de Correo electrónico de base de datos, pero no entregó el mensaje al destinatario final. Compruebe los registros del servidor SMTP o póngase en contacto con el administrador del servidor SMTP. También puede probar el servidor de correo SMTP mediante otro cliente, como Outlook Express.

## <a name="message-status-failed"></a>Estado del mensaje: error

Un estado de error indica que el programa externo de Correo electrónico de base de datos no ha entregado el mensaje al servidor SMTP. En este caso, la vista **sysmail_event_log** contiene la información detallada del programa externo. Para obtener un ejemplo de consulta que combina **sysmail_faileditems** y **sysmail_event_log** para recuperar mensajes de error detallados, vea [Comprobar el estado de los mensajes de correo electrónico enviados con Correo electrónico de base de datos](check-the-status-of-e-mail-messages-sent-with-database-mail.md). Las causas más habituales de los errores son las direcciones de destino incorrectas o problemas de red que impiden al programa externo alcanzar una o más cuentas de conmutación por error. Los problemas en el servidor SMTP también pueden ocasionar el rechazo de correo por parte del servidor. Mediante el Asistente para configuración de Correo electrónico de base de datos, cambie el **Nivel de registro** a **Detallado** y envíe un correo de prueba para investigar el punto de error.



##  <a name="see-also"></a><a name="RelatedContent"></a> Vea también
  
-  [Correo electrónico de base de datos](database-mail.md)

  
  
