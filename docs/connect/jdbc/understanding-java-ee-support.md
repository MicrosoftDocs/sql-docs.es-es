---
description: Descripción de la compatibilidad con Java EE
title: Descripción de la compatibilidad con Java EE | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a9448b80-b7a3-49cf-8bb4-322c73676005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6d9e6a008c4cf3e3e9030bf25dfe00e2f10852b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837512"
---
# <a name="understanding-java-ee-support"></a>Descripción de la compatibilidad con Java EE

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En las secciones siguientes se documenta cómo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] proporciona compatibilidad con las características de las API opcionales de Java Platform, Enterprise Edition (Java EE) y JDBC 3.0. Los ejemplos del código fuente que se proporcionan en este sistema de Ayuda proporcionan una referencia adecuada para comenzar a usar estas características.  
  
Primero, asegúrese de que el entorno de Java (JDK, JRE) incluye el paquete javax.sql. Es un paquete necesario para cualquier aplicación JDBC que utilice la API opcional. JDK 1.5 y las versiones posteriores ya contienen este paquete, por lo que no tiene que instalarlo por separado.  
  
## <a name="driver-name"></a>Nombre del controlador

El nombre de clase del controlador es **com.microsoft.sqlserver.jdbc.SQLServerDriver**. Para JDBC Driver 4.1, 4.2 y 6.0, el controlador se encuentra en el archivo **sqljdbc.jar**, **sqljdbc4.jar**, **sqljdbc41.jar** o **sqljdbc42.jar**.

Para el controlador JDBC Driver 6.2, el controlador se encuentra en **mssql-jdbc-6.2.2.jre7.jar** o **mssql-jdbc-6.2.2.jre8.jar**.

Para el controlador JDBC Driver 6.4, el controlador se encuentra en **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** o **mssql-jdbc-6.4.0.jre9.jar**.

Para el controlador JDBC Driver 7.0, el controlador se encuentra en **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar**.

Para el controlador JDBC Driver 7.2, el controlador se encuentra en **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar**.

Para el controlador JDBC Driver 7.4, el controlador se encuentra en **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** o **mssql-jdbc-7.4.1.jre12.jar**.

Para JDBC Driver 8.2, el controlador se encuentra en **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar**.

Para JDBC Driver 8.4, el controlador se encuentra en **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** o **mssql-jdbc-8.4.1.jre14.jar**.

Para JDBC Driver 9.2, el controlador se encuentra en **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** o **mssql-jdbc-9.2.1.jre15.jar**.

El nombre de clase se usa siempre que carga el controlador con la clase DriverManager de JDBC y siempre que especifica el nombre de clase del controlador en cualquier configuración de controlador. Por ejemplo, configurar un origen de datos dentro de un servidor de aplicaciones de Java EE podría requerir que se escribiera el nombre de clase del controlador.  
  
## <a name="data-sources"></a>Orígenes de datos

El controlador JDBC proporciona compatibilidad con los orígenes de datos de Java EE / JDBC 3.0. La clase [SQLServerXADataSource ](../../connect/jdbc/reference/sqlserverxadatasource-class.md) del controlador JDBC Driver se implementa por `com.microsoft.sqlserver.jdbc.SQLServerXADataSource`.  
  
### <a name="datasource-names"></a>Nombres de los orígenes de datos

Puede realizar conexiones a bases de datos mediante orígenes de datos. Los orígenes de datos disponibles con el controlador JDBC se describen en la tabla siguiente:  
  
|Tipo de origen de datos|Nombre de clase y descripción|  
|---------------|--------------------------|  
|DataSource|`com.microsoft.sqlserver.jdbc.SQLServerDataSource` <br/> <br/> Origen de datos que no es un grupo.|  
|ConnectionPoolDataSource|`com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource` <br/> <br/> Origen de datos para configurar los grupos de conexiones de servidor de aplicaciones de JAVA EE. Normalmente se usa cuando la aplicación se ejecuta dentro de un servidor de aplicaciones de JAVA EE.|  
|XADataSource|`com.microsoft.sqlserver.jdbc.SQLServerXADataSource` <br/> <br/> Origen de datos para configurar orígenes de datos de JAVA EE XA. Normalmente se usa cuando la aplicación se ejecuta dentro de un servidor de aplicaciones de JAVA EE y un administrador de transacciones de XA.|  
  
### <a name="data-source-properties"></a>Propiedades del origen de datos

Todos los orígenes de datos permiten establecer y obtener cualquier propiedad que esté asociada al conjunto de propiedades del controlador subyacente.  
  
Ejemplos:  
  
`setServerName("localhost");`  
`setDatabaseName("AdventureWorks");`  
  
A continuación se muestra cómo se conecta una aplicación mediante un origen de datos:  

```java
//initialize JNDI ..  
Context ctx = new InitialContext(System.getProperties());
...
DataSource ds = (DataSource) ctx.lookup("MyDataSource");
Connection c = ds.getConnection("user", "pwd");  
```

Para obtener más información sobre las propiedades de origen de datos, vea [Establecimiento de las propiedades de origen de datos](../../connect/jdbc/setting-the-data-source-properties.md).  
  
## <a name="see-also"></a>Consulte también

[Introducción al controlador JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
