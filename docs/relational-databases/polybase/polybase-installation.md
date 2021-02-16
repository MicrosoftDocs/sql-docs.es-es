---
title: Instalación de PolyBase en Windows | Microsoft Docs
description: Aprenda a instalar PolyBase como un solo nodo o grupo de escalado horizontal de PolyBase. Puede usar un asistente para la instalación o un símbolo del sistema. Por último, habilite PolyBase.
ms.date: 09/24/2018
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
helpviewer_keywords:
- PolyBase, installation
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 00cb6468e269b057fb5d5d539a3f0efdd8be36ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351816"
---
# <a name="install-polybase-on-windows"></a>Instalación de PolyBase en Windows

[!INCLUDE [SQL Server Windows Only ](../../includes/applies-to-version/sql-windows-only.md)]

Para instalar una versión de evaluación de SQL Server, vaya a [SQL Server Evaluaciones](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016). 
   
## <a name="prerequisites"></a>Requisitos previos  
   
- Edición SQL Server Evaluation (64 bits).  
   
- Microsoft .NET Framework 4.5.  

- Memoria mínima: 4 GB. 
   
- Espacio mínimo disponible en disco duro: 2 GB.
  
- Se recomienda: Un mínimo de 16 GB de RAM.
   
- TCP/IP debe estar habilitado para que PolyBase funcione correctamente. TCP/IP está habilitado de manera predeterminada en todas las ediciones de SQL Server, excepto en las ediciones Developer y Express de SQL Server. Para que PolyBase funcione correctamente en las ediciones Developer y Express, debe habilitar la conectividad TCP/IP. Vea [Habilitar o deshabilitar un protocolo de red de servidor](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


>[!NOTE] 
> PolyBase solo se puede instalar en una instancia de SQL Server por máquina.


>[!NOTE]
>Para poder usar PolyBase debe tener permisos a nivel de CONTROL SERVER o sysadmin en la base de datos.

>[!IMPORTANT]
>Para usar la funcionalidad de aplicación de cálculos en Hadoop, el clúster de Hadoop de destino debe tener los componentes principales de HDFS, YARN y MapReduce con el servidor de historial de trabajos habilitado. PolyBase envía la consulta de aplicación a través de MapReduce y extrae el estado del servidor de historial de trabajos. Si falta algún componente, se produce un error en la consulta.
  
## <a name="single-node-or-polybase-scale-out-group"></a>Nodo único o grupo de escalado horizontal de PolyBase

Antes de instalar PolyBase en las instancias de SQL Server, decida si quiere una instalación de nodo único o un [grupo de escalado horizontal de PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).

En el caso de un grupo de escalado horizontal de PolyBase, asegúrese de que:

- Todos los equipos estén en el mismo dominio.
- Use la misma cuenta y contraseña de servicio durante la instalación de PolyBase.
- Las instancias de SQL Server puedan comunicarse entre sí a través de la red.
- Las instancias de SQL Server tengan todas la misma versión de SQL Server.

Después de instalar PolyBase de forma independiente o en un grupo de escalado horizontal, esto no se puede cambiar. Para cambiar este valor, tiene que desinstalar y volver a instalar la característica.

## <a name="use-the-installation-wizard"></a>Usar el Asistente para instalación
   
1. Ejecute el archivo setup.exe de SQL Server.   
   
2. Seleccione **Instalación** y luego **New standalone SQL Server installation or add features** (Nueva instalación independiente de SQL Server o adición de características).  
   
3. En la página Selección de características, elija **PolyBase Query Service for External Data** (Servicio de consultas PolyBase para datos externos).  

   ![Servicios de PolyBase](../../relational-databases/polybase/media/install-wizard.png "Servicios de PolyBase")  
   
   >[!NOTE]
   >SQL Server 2019 PolyBase ahora incluye una opción adicional, el **conector de Java para orígenes de datos HDFS**. Consulte las [características en vista previa (GB) de SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/04/24/sql-server-2019-community-technology-preview-2-5-is-now-available/) para más información sobre esta característica.
   
4. En la página Configuración del servidor, configure el **servicio Motor de SQL Server PolyBase** y el **servicio Movimiento de datos de SQL Server PolyBase** para que se ejecuten en la misma cuenta de dominio.  

   >[!IMPORTANT]
   >En un grupo de escalado horizontal de PolyBase, los servicios Movimiento de datos de PolyBase y Motor de PolyBase de todos los nodos deben ejecutarse en la misma cuenta de dominio. Vea [Grupos de escalado horizontal de PolyBase](#enable).

5. En la página Configuración de PolyBase, seleccione una de las dos opciones. Para obtener más información, vea [Grupos de escalado horizontal de PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
   
   - Use la instancia de SQL Server como una instancia independiente habilitada para PolyBase.  
   
     Elija esta opción para usar la instancia de SQL Server como nodo principal independiente.  
   
   - Utilice la instancia de SQL Server como parte de un grupo de escalado horizontal de PolyBase. Esta opción abre el firewall para permitir las conexiones entrantes. Se permiten las conexiones al Motor de base de datos de SQL Server, al Motor de SQL Server PolyBase, al servicio Movimiento de datos de SQL Server PolyBase y a SQL Browser. El firewall además permite las conexiones entrantes desde otros nodos de un grupo de escalado horizontal de PolyBase.  
   
     Esta opción también habilita las conexiones de firewall de Microsoft DTC (Coordinador de transacciones distribuidas) y modifica la configuración del registro de Microsoft DTC.  
   
6. En la página Configuración de PolyBase, especifique un intervalo de puertos con al menos seis puertos. El programa de instalación de SQL Server asigna los seis primeros puertos disponibles del intervalo.  

   >[!IMPORTANT]
   > Después de la instalación, debe [habilitar la característica PolyBase](#enable).


##  <a name="use-a-command-prompt"></a><a name="installing"></a> Usar un símbolo del sistema

Use los valores de esta tabla para crear scripts de instalación. Los servicios Motor de SQL Server PolyBase y Movimiento de datos de SQL Server PolyBase deben ejecutarse en la misma cuenta. En un grupo de escalado horizontal de PolyBase, se deben ejecutar con la misma cuenta de dominio los servicios de PolyBase en todos los nodos.  
   
<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017"

|Componente de SQL Server|Parámetro y valores|Descripción|  
|--------------------------|--------------------------|-----------------|  
|Control del programa de instalación de SQL Server|**Obligatorio**<br /><br /> /FEATURES=PolyBase|Selecciona la característica PolyBase.|  
|Motor de SQL Server PolyBase|**Opcional**<br /><br /> /PBENGSVCACCOUNT|Especifica la cuenta del servicio de motor. El valor predeterminado es **NT Authority\NETWORK SERVICE**.|  
|Motor de SQL Server PolyBase|**Opcional**<br /><br /> /PBENGSVCPASSWORD|Especifica la contraseña de la cuenta del servicio de motor.|  
|Motor de SQL Server PolyBase|**Opcional**<br /><br /> /PBENGSVCSTARTUPTYPE|Especifica el modo de inicio para el motor de PolyBase: Automático (predeterminado), Deshabilitado y Manual.|  
|Movimiento de datos de SQL Server PolyBase |**Opcional**<br /><br /> /PBDMSSVCACCOUNT|Especifica la cuenta del servicio Movimiento de datos. El valor predeterminado es **NT Authority\NETWORK SERVICE**.|  
|Movimiento de datos de SQL Server PolyBase |**Opcional**<br /><br /> /PBDMSSVCPASSWORD|Especifica la contraseña de la cuenta de movimiento de datos.|  
|Movimiento de datos de SQL Server PolyBase |**Opcional**<br /><br /> /PBDMSSVCSTARTUPTYPE|Especifica el modo de inicio para el servicio de movimiento de datos: Automático (predeterminado), Deshabilitado y Manual.|  
|PolyBase|**Opcional**<br /><br /> /PBSCALEOUT|Especifica si la instancia de SQL Server se usa como parte de un grupo de cálculo de escalabilidad horizontal de PolyBase. <br />Valores admitidos: True, False.|  
|PolyBase|**Opcional**<br /><br /> /PBPORTRANGE|Especifica un intervalo de puertos con un mínimo de seis puertos para los servicios de PolyBase. Ejemplo:<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

|Componente de SQL Server|Parámetro y valores|Descripción|  
|--------------------------|--------------------------|-----------------|  
|Control del programa de instalación de SQL Server|**Obligatorio**<br /><br /> /FEATURES=PolyBaseCore, PolyBaseJava, PolyBase | PolyBaseCore habilita la compatibilidad con todas las características de PolyBase, excepto la conectividad de Hadoop. PolyBaseJava habilita la conectividad de Hadoop. PolyBase habilita ambas. |  
|Motor de SQL Server PolyBase|**Opcional**<br /><br /> /PBENGSVCACCOUNT|Especifica la cuenta del servicio de motor. El valor predeterminado es **NT Authority\NETWORK SERVICE**.|  
|Motor de SQL Server PolyBase|**Opcional**<br /><br /> /PBENGSVCPASSWORD|Especifica la contraseña de la cuenta del servicio de motor.|  
|Motor de SQL Server PolyBase|**Opcional**<br /><br /> /PBENGSVCSTARTUPTYPE|Especifica el modo de inicio para el motor de PolyBase: Automático (predeterminado), Deshabilitado y Manual.|  
|Movimiento de datos de SQL Server PolyBase |**Opcional**<br /><br /> /PBDMSSVCACCOUNT|Especifica la cuenta del servicio de movimiento de datos. El valor predeterminado es **NT Authority\NETWORK SERVICE**.|  
|Movimiento de datos de SQL Server PolyBase |**Opcional**<br /><br /> /PBDMSSVCPASSWORD|Especifica la contraseña de la cuenta de movimiento de datos.|  
|Movimiento de datos de SQL Server PolyBase |**Opcional**<br /><br /> /PBDMSSVCSTARTUPTYPE|Especifica el modo de inicio para el servicio de movimiento de datos: Automático (predeterminado), Deshabilitado y Manual.|  
|PolyBase|**Opcional**<br /><br /> /PBSCALEOUT|Especifica si la instancia de SQL Server se usa como parte de un grupo de cálculo de escalabilidad horizontal de PolyBase. <br />Valores admitidos: True, False.|  
|PolyBase|**Opcional**<br /><br /> /PBPORTRANGE|Especifica un intervalo de puertos con un mínimo de seis puertos para los servicios de PolyBase. Ejemplo:<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end

Después de la instalación, debe [habilitar la característica PolyBase](#enable).



**Ejemplo**

En este ejemplo se muestra un script de instalación de ejemplo.  

```cmd
   
Setup.exe /Q /ACTION=INSTALL /IACCEPTSQLSERVERLICENSETERMS /FEATURES=SQLEngine,PolyBase   
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="\<fabric-domain>\Administrator"   
/INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /PBSCALEOUT=TRUE   
/PBPORTRANGE=16450-16460 /SECURITYMODE=SQL /SAPWD="<StrongPassword>"   
/PBENGSVCACCOUNT="<DomainName>\<UserName>" /PBENGSVCPASSWORD="<StrongPassword>"   
/PBDMSSVCACCOUNT="<DomainName>\<UserName>" /PBDMSSVCPASSWORD="<StrongPassword>"  
   
```  

## <a name="enable-polybase"></a><a id="enable"></a> Habilitar PolyBase

Tras la instalación, se debe habilitar PolyBase para acceder a sus características. Use el siguiente comando de Transact-SQL. Las instancias de SQL 2019 implementadas durante la instalación del Clúster de macrodatos tienen esta opción habilitada de forma predeterminada.


```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE;
```


## <a name="post-installation-notes"></a>Notas posteriores a la instalación  

PolyBase instala tres bases de datos de usuario: DWConfiguration, DWDiagnostics y DWQueue. Estas bases de datos son para uso de PolyBase. No las modifique ni las elimine.  
   
### <a name="how-to-confirm-installation"></a><a id="confirminstall"></a> Cómo confirmar la instalación  

Ejecute el siguiente comando: Si PolyBase está instalado, el valor devuelto es 1. De lo contrario, es 0.  

```sql  
SELECT SERVERPROPERTY ('IsPolyBaseInstalled') AS IsPolyBaseInstalled;  
```  

### <a name="firewall-rules"></a>Reglas de firewall  

El programa de instalación de SQL Server PolyBase crea las siguientes reglas de firewall en el equipo:  
   
- SQL Server PolyBase - Motor de base de datos: \<SQLServerInstanceName> (TCP de entrada)  
   
- SQL Server PolyBase -> Servicios de PolyBase: \<SQLServerInstanceName> (TCP de entrada)  

- SQL Server PolyBase -> SQL Browser -> (UDP de entrada)  
   
Durante la instalación, si usa la instancia de SQL Server como parte de un grupo de escalado horizontal de PolyBase, estas reglas están habilitadas. El firewall se abre para permitir conexiones entrantes. Se permiten al Motor de base de datos de SQL Server, al Motor de SQL Server PolyBase, al servicio Movimiento de datos de SQL Server PolyBase y a SQL Browser. Si el servicio de firewall del equipo no se está ejecutando durante la instalación, el programa de instalación de SQL Server no habilita estas reglas. En ese caso, inicie el servicio de firewall del equipo y habilite estas reglas después de la instalación.  
   
#### <a name="to-enable-the-firewall-rules"></a>Para habilitar las reglas de firewall, siga estos pasos:  

1. Abra el **Panel de control**.  

2. Seleccione **Sistema y seguridad** y luego **Firewall de Windows**.  
   
3. Seleccione **Configuración avanzada** y luego **Reglas de entrada**.  
   
4. Haga clic con el botón derecho en la regla deshabilitada y luego seleccione **Habilitar regla**.  
   
### <a name="polybase-service-accounts"></a>Cuentas de servicio de PolyBase

Para cambiar las cuentas de servicio de los servicios Motor de PolyBase y Movimiento de datos de PolyBase, desinstale y vuelva a instalar la característica PolyBase.

## <a name="next-steps"></a>Pasos siguientes  

Consulte [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md).
