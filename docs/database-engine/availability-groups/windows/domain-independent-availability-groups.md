---
title: Creación de un grupo de disponibilidad independiente del dominio
description: Pasos para crear un grupo de disponibilidad en el que se usa un clúster de grupo de trabajo. Esto permite que en SQL Server 2016 (y versiones posteriores) se implemente un grupo de disponibilidad Always On sobre un WSFC que no requiera Active Directory Domain Services y, por tanto, no requiera que cada servidor forme parte del mismo dominio.
ms.custom: seodec18
ms.date: 09/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], domain independent
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 060e9e635aa7be4dd3c00e11b0f3a36073cdaa54
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100349286"
---
# <a name="create-a-domain-independent-availability-group"></a>Creación de un grupo de disponibilidad independiente del dominio
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Los grupos de disponibilidad Always On requieren un clúster de conmutación por error de Windows Server (WSFC) subyacente. Implementar un WSFC a través de Windows Server 2012 R2 siempre ha requerido que los servidores que participan en ese WSFC (también conocidos como nodos) estén unidos al mismo dominio. Para más información sobre Active Directory Domain Services (AD DS), vaya [aquí](/previous-versions/windows/it-pro/windows-server-2003/cc759073(v=ws.10)).

La dependencia de AD DS y WSFC es más compleja de lo que se ha implementado previamente con una configuración de creación de reflejo de base de datos (DBM), dado que DBM se puede implementar a través de varios centros de datos con certificados sin necesidad de estas dependencias.  Un grupo de disponibilidad tradicional que abarca más de un centro de datos requiere que todos los servidores estén unidos al mismo dominio de Active Directory; si los dominios son diferentes, aun siendo de confianza, esto no funcionará. Todos los servidores deben ser nodos del mismo WSFC. En la siguiente imagen se muestra este proceso. SQL Server 2016 tiene, además, grupos de disponibilidad distribuidos que también pueden lograr este objetivo de otro modo.


![WSFC que abarca dos centros de datos conectados al mismo dominio][1]

Windows Server 2012 R2 incluyó un [clúster desasociado de Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970(v=ws.11)), una especie de clúster de conmutación por error de Windows Server que se puede usar con los grupos de disponibilidad. Este tipo de WSFC sigue requiriendo que los nodos estén unidos al mismo dominio de Active Directory, si bien en este caso el WSFC usa DNS, no dominios. Puesto que sigue habiendo un dominio presente, los clústeres desasociados de Active Directory siguen sin proporcionar una experiencia completamente libre de dominios.

Con Windows Server 2016 apareció un nuevo tipo de clúster de conmutación por error de Windows Server basado en el clúster desasociado de Active Directory: el clúster de grupo de trabajo. Un clúster de grupo de trabajo permite a SQL Server 2016 implementar un grupo de disponibilidad sobre un WSFC que no requiere AD DS. SQL Server requiere el uso de certificados para la seguridad de los puntos de conexión, de igual modo que el escenario de creación de reflejo de la base de datos requiere certificados.  Este tipo de grupo de disponibilidad se conoce como grupo de disponibilidad independiente del dominio. Implementar un grupo de disponibilidad con un clúster de grupo de trabajo subyacente tiene cabida para las siguientes combinaciones de los nodos que van a conformar el WSFC:
- No hay ningún nodo unido a un dominio.
- Todos los nodos están unidos a dominios distintos.
- Los nodos están mezclados, en una combinación de nodos unidos a dominio y nodos no unidos a dominio.

En la siguiente imagen se muestra un ejemplo de un grupo de disponibilidad independiente del dominio en el que los nodos del Centro de datos 1 están unidos a un dominio, mientras que los del Centro de datos 2 usan únicamente DNS. En este caso, establezca el sufijo DNS en todos los servidores que vayan a ser nodos del WSFC. Todas las aplicaciones y servidores que tienen acceso al grupo de disponibilidad deben ver la misma información de DNS.


![Clúster de grupo de trabajo con dos nodos que están unidos a un dominio][2]

Un grupo de disponibilidad independiente del dominio no es válido exclusivamente en escenarios de recuperación ante desastres o de varios sitios. Se puede implementar en un solo centro de datos e, incluso, usarse con un [grupo de disponibilidad básico](basic-availability-groups-always-on-availability-groups.md) (también conocido como grupo de disponibilidad de Standard Edition) para proporcionar una arquitectura similar a lo que solía lograrse con la creación de reflejo de base de datos con certificados, como se muestra aquí.


![Vista de alto nivel de un grupo de disponibilidad en Standard Edition][3]

La implementación de un grupo de disponibilidad independiente del dominio presenta algunas reservas conocidas:
- Los únicos tipos de testigo disponibles para su uso con cuórum son disco y [nube](/windows-server/failover-clustering/deploy-cloud-witness), que es nuevo en Windows Server 2016. Disco es problemático, ya que probablemente el grupo de disponibilidad no haga uso del disco compartido.
- La variante de clúster de grupo de trabajo subyacente de un WSFC solo se puede crear con PowerShell, si bien a partir de ahí se puede administrar con el Administrador de clústeres de conmutación por error.
- Si se requiere Kerberos, hay que implementar un WSFC estándar que esté adjuntado a un dominio de Active Directory, y seguramente un grupo de disponibilidad independiente del dominio no sea una opción en este caso.
- Aunque se puede configurar un agente de escucha, se debe registrar en DNS para que pueda usarse. Tal y como se ha mencionado anteriormente, no hay ninguna compatibilidad de Kerberos para el agente de escucha.
- Las aplicaciones que se conectan a SQL Server deben usar principalmente la autenticación de SQL Server, puesto que es posible que no haya dominios o que estos no estén configurados para funcionar conjuntamente. 
- Se usarán certificados en la configuración del grupo de disponibilidad.

## <a name="set-and-verify-the-dns-suffix-on-all-replica-servers"></a>Establecer y comprobar el sufijo DNS en todos los servidores de réplicas

Se necesita un sufijo DNS común para un clúster de grupo de trabajo de un grupo de disponibilidad independiente del dominio. Haga lo siguiente para establecer y comprobar el sufijo DNS en cada Windows Server que vaya a hospedar una réplica del grupo de disponibilidad:

1. Con la combinación tecla Windows+X, seleccione Sistema.
2. Si el nombre de equipo y el nombre completo de equipo son el mismo, quiere decir que el sufijo DNS no se ha establecido. Por ejemplo, si el nombre de equipo es ALLAN, el valor de nombre completo de equipo no debe ser simplemente ALLAN, sino algo parecido a ALLAN.SQLHA.LAB. SQLHA.LAB es el sufijo DNS. El valor de grupo de trabajo debe reflejar WORKGROUP. Si es necesario establecer el sufijo DNS, seleccione Cambiar configuración.
3. En la pestaña Nombre de equipo del cuadro de diálogo Propiedades del sistema, haga clic en Cambiar.
4. En el cuadro de diálogo Cambios en el dominio o el nombre del equipo, haga clic en Más.
5. En el cuadro de diálogo Sufijo DNS y nombre NetBIOS del equipo, escriba el sufijo DNS común como sufijo DNS principal. 
6. Haga clic en Aceptar para cerrar el cuadro de diálogo Sufijo DNS y nombre NetBIOS del equipo.
7. Haga clic en Aceptar para cerrar el cuadro de diálogo Cambios en el dominio o el nombre del equipo.
8. Se le pedirá que reinicie el servidor para que los cambios surtan efecto. Haga clic en Aceptar para cerrar el cuadro de diálogo Cambios en el dominio o el nombre del equipo.
9. Haga clic en Cerrar para cerrar el cuadro de diálogo Propiedades del sistema.
10. Se le pedirá que reinicie el equipo. Si no quiere reiniciar inmediatamente, haga clic en Reiniciar más tarde; si no, haga clic en Reiniciar ahora.
11. Tras reiniciar el servidor, vaya de nuevo a Sistema para confirmar que el sufijo DNS común está configurado.

![Configuración correcta del sufijo DNS][4]

  > [!NOTE]
  > Si está usando varias subredes y tiene un DNS estático, deberá tener un proceso establecido para actualizar el registro DNS asociado con el cliente de escucha antes de realizar una conmutación por error ya que, en caso contrario, el nombre de red no se conectará.

## <a name="create-a-domain-independent-availability-group"></a>Crear un grupo de disponibilidad independiente del dominio

Actualmente, la creación de un grupo de disponibilidad independiente del dominio no es posible tan solo con SQL Server Management Studio. Si bien crear un grupo de disponibilidad independiente del dominio consiste básicamente en lo mismo que crear un grupo de disponibilidad al uso, hay algunos aspectos (por ejemplo, la creación de certificados) que solo son posibles con Transact-SQL. En el siguiente ejemplo se da por hecho que existe una configuración de grupo de disponibilidad con dos réplicas: una principal y otra secundaria. 

1. [Con las instrucciones de este vínculo](https://techcommunity.microsoft.com/t5/Failover-Clustering/Workgroup-and-Multi-domain-clusters-in-Windows-Server-2016/ba-p/372059), implemente un clúster de grupo de trabajo formado por todos los servidores que van a participar en el grupo de disponibilidad. Antes de configurar el clúster de grupo de trabajo, asegúrese de que el sufijo DNS común ya está configurado.
2. [Habilite la característica Grupos de disponibilidad AlwaysOn](./enable-and-disable-always-on-availability-groups-sql-server.md) en cada instancia que vaya a participar en el grupo de disponibilidad. Para ello, será necesario reiniciar cada instancia de SQL Server.
3. Cada instancia que vaya a hospedar la réplica principal requiere una clave maestra de base de datos. Si no existe una clave maestra, ejecute el siguiente comando:

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Strong Password';
   GO
   ```

4. En la instancia que va a ser la réplica principal, cree el certificado que se usará en las conexiones entrantes de las réplicas secundarias y para proteger el punto de conexión de la réplica principal.

   ```sql
   CREATE CERTIFICATE InstanceA_Cert 
   WITH SUBJECT = 'InstanceA Certificate';
   GO
   ``` 

5. Realice una copia de seguridad del certificado. También la puede proteger con una clave privada si quiere. En este ejemplo no se usa una clave privada.

   ```sql
   BACKUP CERTIFICATE InstanceA_Cert 
   TO FILE = 'Backup_path\InstanceA_Cert.cer';
   GO
   ```

6. Repita los pasos 4 y 5 para crear certificados y hacer copias de seguridad de estos en cada réplica secundaria, usando para ello nombres de certificado apropiados, como InstanceB_Cert.
7. En la réplica principal, debe crear un inicio de sesión por cada réplica secundaria del grupo de disponibilidad. Este inicio de sesión tendrá permisos para conectarse al punto de conexión que use el grupo de disponibilidad independiente del dominio. Por ejemplo, si la réplica se llama InstanceB:

   ```sql
   CREATE LOGIN InstanceB_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

8. En cada réplica secundaria, cree un inicio de sesión para la réplica principal. Este inicio de sesión tendrá permisos para conectarse al punto de conexión. Por ejemplo, en una réplica llamada InstanceB:

   ```sql
   CREATE LOGIN InstanceA_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

9. En todas las instancias, cree un usuario por cada inicio de sesión que haya creado. Se usará al restaurar los certificados. Por ejemplo, para crear un usuario para la réplica principal:

   ```sql
   CREATE USER InstanceA_User FOR LOGIN InstanceA_Login;
   GO
   ```

10. Por cada réplica que vaya a ser una réplica principal, cree un inicio de sesión y un usuario en todas las réplicas secundarias correspondientes.
11. En cada instancia, restaure los certificados del resto de instancias que tenían un inicio de sesión y un usuario creados. En la réplica principal, restaure todos los certificados de réplica secundaria. En cada réplica secundaria, restaure el certificado de la réplica principal, así como en cualquier otra réplica que pueda ser principal. Por ejemplo:

   ```sql
   CREATE CERTIFICATE [InstanceB_Cert]
   AUTHORIZATION InstanceB_User
   FROM FILE = 'Restore_path\InstanceB_Cert.cer'
   ```

12. Cree el punto de conexión que usará el grupo de disponibilidad en cada instancia que va a ser una réplica. En los grupos de disponibilidad, el punto de conexión debe ser del tipo DATABASE_MIRRORING. El punto de conexión emplea el certificado creado en el paso 4 para esa instancia para la autenticación. La siguiente sintaxis de ejemplo muestra cómo crear un punto de conexión con un certificado. Recurra al método de cifrado pertinente y a otras opciones relevantes en su entorno. Para más información sobre las opciones disponibles, vea [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md).

   ```sql
   CREATE ENDPOINT DIAG_EP
   STATE = STARTED
   AS TCP (   
    LISTENER_PORT = 5022,
    LISTENER_IP = ALL
         )
   FOR DATABASE_MIRRORING (
    AUTHENTICATION = CERTIFICATE InstanceX_Cert,
    ROLE = ALL
         )
   ```

13. Asigne derechos a cada inicio de sesión creado en esa instancia en el paso 8 para poder conectarse al punto de conexión. 

   ```sql
   GRANT CONNECT ON ENDPOINT::DIAG_EP TO [InstanceX_Login];
   GO
   ```

14. Cuando haya configurado los certificados subyacentes y la seguridad del punto de conexión, cree el grupo de disponibilidad con el método de su elección. Se recomienda hacer manualmente una copia de seguridad y hacer una copia y restauración de la copia de seguridad usada para inicializar la base de datos secundaria, o bien usar [la propagación automática](automatically-initialize-always-on-availability-group.md). Si se usa el asistente para inicializar las réplicas secundarias, ello conllevará el uso de archivos de bloque de mensajes del servidor (SMB), que pueden no funcionar si se usa un clúster de grupo de trabajo no unido a un dominio.
15. Si crea un agente de escucha, asegúrese de que su nombre y dirección IP están registrados en DNS.

### <a name="next-steps"></a>Pasos siguientes 

- [Usar el Asistente para grupo de disponibilidad (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Usar el cuadro de diálogo Nuevo grupo de disponibilidad (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Crear un grupo de disponibilidad (Transact-SQL)](create-an-availability-group-transact-sql.md)

<!--Image references-->
[1]: ./media/diag-wsfc-two-data-centers-same-domain.png
[2]: ./media/diag-workgroup-cluster-two-nodes-joined.png
[3]: ./media/diag-high-level-view-ag-standard-edition.png
[4]: ./media/diag-successful-dns-suffix.png
