---
description: Propiedades de Agente SQL Server (página Sistema de alerta)
title: Propiedades de Agente SQL Server (página Sistema de alerta)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.agent.alert.f1
ms.assetid: 3e6d3bfd-20ee-4593-86cc-f65b1c08c69d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 19ee180bdc54ea36804f18b9ab064f459db0d23b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464366"
---
# <a name="sql-server-agent-properties-alert-system-page"></a>Propiedades de Agente SQL Server (página Sistema de alerta)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Use esta página para ver y modificar la configuración de los mensajes enviados por alertas del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Opciones  
**Sesión de correo**  
Las opciones de esta sección configuran el correo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Habilitar perfil de correo**  
Habilita el correo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . De forma predeterminada, el correo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no está habilitado.  
  
**Sistema de correo**  
Establece el sistema de correo que debe utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Correo electrónico de base de datos  
  
> [!NOTE]  
> Después de cambiar el sistema de correo electrónico, debe reiniciar el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que el cambio entre en vigor.  
  
**Perfil de correo**  
Establece el perfil que debe utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . También puede seleccionar **\<new Database Mail profile...>** para crear un nuevo perfil.  
  
**Correo electrónico de buscapersonas**  
Las opciones de esta sección le permiten configurar los mensajes de correo electrónico enviados a direcciones de buscapersonas para que funcionen con su sistema de buscapersonas.  
  
**Formato de dirección para correo electrónico de buscapersonas**  
Esta sección le permite especificar el formato de las direcciones y la línea de asunto que se incluyen en los mensajes de correo electrónico de buscapersonas.  
  
**Línea Para**  
Especifica las opciones de la línea **Para** del mensaje  
  
**Prefijo**  
Escriba cualquier tipo de texto fijo que su sistema exija al principio de la línea **Para** de los mensajes que se envían a un buscapersonas.  
  
**Buscapersonas**  
Incluye la dirección de correo electrónico del mensaje entre el prefijo y el sufijo.  
  
**Sufijo**  
Escriba cualquier tipo de texto fijo que su sistema de localización exija al final de la línea **Para** de los mensajes que se envían a un buscapersonas.  
  
**Línea CC**  
Especifica las opciones de la línea **CC** del mensaje.  
  
**Prefijo**  
Escriba cualquier tipo de texto fijo que su sistema exija al principio de la línea **CC** de los mensajes que se envían a un buscapersonas.  
  
**Buscapersonas**  
Incluye la dirección de correo electrónico del mensaje entre el prefijo y el sufijo.  
  
**Sufijo**  
Escriba cualquier tipo de texto fijo que su sistema de localización exija al final de la línea **CC** de los mensajes que se envían a un buscapersonas.  
  
**Subject**  
Especifica las opciones del asunto del mensaje  
  
**Prefijo**  
Escriba cualquier tipo de texto fijo que su sistema de localización exija al principio de la línea **Asunto** de los mensajes que se envían a un buscapersonas.  
  
**Sufijo**  
Escriba cualquier tipo de texto fijo que su sistema de localización exija al final de la línea **Asunto** de los mensajes que se envían a un buscapersonas.  
  
**Incluir cuerpo del mensaje en la notificación**  
Incluye el cuerpo de un mensaje de correo electrónico en el mensaje que se envía al buscapersonas.  
  
**Operador para notificaciones de error**  
Esta sección le permite especificar las opciones del operador para notificaciones de error.  
  
**Habilitar operador para notificaciones de error**  
Especifica un operador para notificaciones de error.  
  
**Operador**  
Establece el nombre del operador para recibir notificaciones de error.  
  
**Notificar mediante**  
Establece el método que se va a utilizar para las notificaciones al operador para notificaciones de error.  
  
**Reemplazo de tokens**  
Esta sección le permite habilitar tokens de paso de trabajo que pueden utilizarse en trabajos ejecutados por alertas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para más información sobre los tokens de paso de trabajo, consulte [Usar tokens en pasos de trabajo](../../ssms/agent/use-tokens-in-job-steps.md).  
  
> [!IMPORTANT]  
> Todos los usuarios de Windows con permisos de escritura en el Registro de eventos de Windows pueden tener acceso a pasos de trabajo activados por alertas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para evitar este riesgo de seguridad, se deshabilitan de manera predeterminada los tokens del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que pueden utilizarse en trabajos activados por alertas. Los tokens son: **$(A-DBN)** , **$(A-SVR)** , **$(A-ERR)** , **$(A-SEV)** y **$(A-MSG)** .  
>   
> Si necesita utilizar estos tokens, asegúrese de que solo los miembros de grupos de seguridad confiables de Windows, tales como el grupo de administradores, tengan permisos de escritura en el Registro de eventos del equipo en el que reside [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , antes de habilitarlos.  
  
**Reemplazar tokens para todas las respuestas de trabajos a alertas**  
Seleccione esta casilla para habilitar el reemplazo de tokens en trabajos activados por alertas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Consulte también  
[Operadores](../../ssms/agent/operators.md)  
[Configurar el Agente SQL Server para que use el Correo electrónico de base de datos](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
[Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)  
