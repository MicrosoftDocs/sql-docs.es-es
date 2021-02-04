---
description: Requisitos del sistema para SQL Server Native Client
title: Requisitos del sistema
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- system requirements [SQL Server Native Client]
- data access [SQL Server Native Client], system requirements
- SQL Server Native Client, system requirements
- SQLNCLI, system requirements
ms.assetid: 1c8e2f8a-a440-44da-8e3a-af632d34c52c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 27e6a98231e66a24a66233092e3e7e245a4e3d74
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536409"
---
# <a name="system-requirements-for-sql-server-native-client"></a>Requisitos del sistema para SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Para utilizar las características de acceso a datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como MARS, debe tener instalado el software siguiente:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client en su cliente.  
  
-   Una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en su servidor.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client requiere Windows Installer 3,1. Windows Installer 3,1 ya está instalado en [!INCLUDE[msCoName](../../includes/msconame-md.md)] los sistemas operativos Windows. Para el resto de plataformas necesita instalarlo explícitamente. Para obtener más información, vea [Windows Installer 3,1 Redistributable (V2)](https://www.microsoft.com/download/details.aspx?id=25).  
  
> [!NOTE]  
>  Asegúrese de que inicia sesión con privilegios de administrador antes de instalar este software.  
  
## <a name="operating-system-requirements"></a>Requisitos del sistema operativo  
 Para obtener una lista de sistemas operativos compatibles con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, consulte [directivas de soporte técnico para SQL Server Native Client](../../relational-databases/native-client/applications/support-policies-for-sql-server-native-client.md).  
  
## <a name="sql-server-requirements"></a>Requisitos de SQL Server  
 Para utilizar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client para tener acceso a los datos en las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe tener instalada una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] admite las conexiones de todas las versiones de MDAC, Componentes de Windows Data Access y todas las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Cuando una versión del cliente anterior se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los tipos de datos del servidor que el cliente no conoce se asignan a tipos que son compatibles con la versión del cliente. Para obtener más información, vea Compatibilidad de tipo de datos para versiones del cliente, más adelante en este tema.  
  
## <a name="cross-language-requirements"></a>Requisitos de idiomas  
 La versión en inglés de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client es compatible con todas las versiones traducidas de los sistemas operativos admitidos. Las versiones traducidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Cliente son compatibles con sistemas operativos traducidos que estén en el mismo idioma que la versión traducida de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Las versiones traducidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client también se admite en las versiones en inglés de los sistemas operativos compatibles siempre que se instale la configuración de idioma correspondiente.  
  
 Para actualizaciones:  
  
-   Las versiones en inglés de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client se pueden actualizar a cualquier versión traducida de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   Las versiones traducidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client se pueden actualizar a cualquier versión traducida de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client en el mismo idioma.  
  
-   Las versiones traducidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client se pueden actualizar a la versión en inglés de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   Las versiones traducidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client no se pueden actualizar a las versiones traducidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client en un idioma distinto.  
  
## <a name="data-type-compatibility-for-client-versions"></a>Compatibilidad de tipo de datos para las versiones del cliente  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client asignan los nuevos tipos de datos a los tipos de datos anteriores que son compatibles con clientes de nivel inferior, como se muestra en la tabla siguiente.  
  
 Las aplicaciones OLE DB y ADO pueden utilizar la palabra clave de la cadena de conexión **DataTypeCompatibility** con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client para trabajar con tipos de datos más antiguos. Cuando **DataTypeCompatibility=80**, los clientes de OLE DB se conectarán con la versión de flujo TDS de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], en lugar de la versión de TDS. Esto significa que el servidor realizará la conversión de nivel inferior y los tipos de datos posteriores para [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], en lugar de a través de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. También significa que las características disponibles en la conexión se limitarán al conjunto de funciones de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Los intentos de utilizar nuevos tipos de datos o funciones se detectan lo más pronto posible en las llamadas API y se devuelven los errores a la aplicación que realiza la llamada, en lugar de intentar pasar las solicitudes no válidas al servidor.  
  
 No hay ningún control **DataTypeCompatibility** para ODBC.  
  
 IDBInfo::GetKeywords siempre devolverá una lista de palabras clave que corresponde a la versión de servidor de la conexión y no se verá afectado por **DataTypeCompatibility**.  
  
|Tipo de datos|SQL Server Native Client<br /><br /> SQL Server 2005|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|Windows Data Access Components, MDAC y<br /><br /> aplicaciones OLE DB de SQL Server Native Client con DataTypeCompatibility=80|  
|---------------|--------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|  
|CLR UDT (\<= 8Kb)|udt|Udt|Varbinary|  
|varbinary(max)|varbinary|varbinary|Imagen|  
|ntext|varchar|varchar|Texto|  
|nvarchar(max)|NVARCHAR|NVARCHAR|Ntext|  
|Xml|Xml|Xml|Ntext|  
|UDT CLR (> 8 KB)|udt|varbinary|Imagen|  
|date|date|varchar|Varchar|  
|datetime2|datetime2|varchar|Varchar|  
|datetimeoffset|datetimeoffset|varchar|Varchar|  
|time|time|varchar|Varchar|  
  
## <a name="see-also"></a>Consulte también  
 [Programación de SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [Instalar SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
  
