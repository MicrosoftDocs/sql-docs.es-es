---
description: Microsoft ActiveX Data Objects (ADO)
title: Microsoft Objetos de datos ActiveX (ADO) | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, about
ms.assetid: 2fa6237b-44b8-4b6c-9952-5acd80a54e20
author: rothja
ms.author: jroth
ms.openlocfilehash: 511b556971496633abd6d347ab416b75a8f9f27c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036045"
---
# <a name="microsoft-activex-data-objects-ado"></a>Microsoft ActiveX Data Objects (ADO)

Objetos de datos ActiveX es un modelo de programación, lo que significa que no depende de ningún motor de back-end determinado. En la actualidad, sin embargo, el único motor que admite el modelo ADO es OLE DB. Hay muchos proveedores OLE DB nativos, así como un proveedor OLE DB para ODBC. ADO se utiliza en C++ y Visual Basic programas para conectarse a SQL Server y otras bases de datos. Por supuesto, también funciona para conectarse a Azure SQL Database en la nube.

En cada sección de este artículo se describe un componente de ADO.

> [!NOTE]
> ADO.NET es diferente de ADO. ADO.NET, y muchos otros controladores de conexión de SQL y sus lenguajes, se describen a partir de [SQL Server controladores](../connect/sql-connection-libraries.md).

  
## <a name="ado"></a>ADO  
 Microsoft Objetos de datos ActiveX (ADO) permite a las aplicaciones cliente obtener acceso a los datos de diversos orígenes y manipularlos a través de un proveedor de OLE DB. Sus principales ventajas son la facilidad de uso, la alta velocidad, una sobrecarga de memoria baja y una pequeña superficie de disco. ADO admite características clave para compilar aplicaciones cliente/servidor y basadas en Web.  
  
## <a name="ado-md"></a>ADO MD  
 Microsoft ActiveX Data Objects (Multidimensional) (ADO MD) proporciona acceso sencillo a los datos multidimensionales de lenguajes como Microsoft Visual Basic y Microsoft Visual C++. ADO MD extiende a Microsoft Objetos de datos ActiveX (ADO) incluir objetos específicos de datos multidimensionales, como los objetos CubeDef y Cellset. Con ADO MD puede examinar esquemas multidimensionales, consultar un cubo y recuperar los resultados.  
  
 Como ADO, ADO MD usa un proveedor de OLE DB subyacente para obtener acceso a los datos. Para trabajar con ADO MD, el proveedor debe ser un proveedor de datos multidimensionales (MDP), tal y como se define en el OLE DB para la especificación de OLAP. MDPs presentar datos en vistas multidimensionales en lugar de proveedores de datos tabulares (TDPs) que presenten datos en vistas tabulares. Consulte la documentación del proveedor de OLE DB OLAP para obtener información más detallada sobre la sintaxis y los comportamientos específicos que admite el proveedor.  
  
## <a name="rds"></a>RDS  
 El servicio de datos remoto (RDS) es una característica de ADO, con la que puede trasladar datos de un servidor a una aplicación cliente o una página web, manipular los datos en el cliente y devolver actualizaciones al servidor en un único viaje de ida y vuelta.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al  [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="adox"></a>M  
 Microsoft Objetos de datos ActiveX Extensions para lenguaje de definición de datos y seguridad (ADOX) es una extensión del modelo de programación y objetos de ADO. ADOX incluye objetos para la creación y modificación de esquemas, así como para la seguridad. Dado que se trata de un enfoque basado en objetos para la manipulación de esquemas, puede escribir código que funcione con distintos orígenes de datos, independientemente de las diferencias en sus sintaxis nativas.  
  
 ADOX es una biblioteca complementaria para los objetos principales de ADO. Expone objetos adicionales para crear, modificar y eliminar objetos de esquema, como tablas y procedimientos. También incluye los objetos de seguridad para mantener a los usuarios y grupos, y para conceder y revocar los permisos en los objetos.  
  
## <a name="documentation"></a>Documentación  
 [Características de diseño de seguridad de ADO](./guide/ado-security-design-issues.md)  
  
 [Programador de ADO ' s guía para el uso de objetos ADO](./guide/ado-programmer-s-guide.md)  
  
 Una introducción al uso de ADO, RDS, ADO MD y ADOX.  
  
 [Referencia del programador de ADO](./reference/ado-programmer-s-reference.md)  
  
 Esta sección de la documentación de ADO contiene temas para cada objeto, colección, propiedad, propiedad dinámica, método, evento y enumeración de ADO, RDS, ADO MD y ADOX.  
  
 [Glosario de términos de ADO](./ado-glossary.md)  
  
## <a name="support"></a>Soporte técnico  
 Para obtener ayuda gratuita con los problemas de ADO, intente publicar en el grupo de noticias público de ADO. Este grupo de noticias lo supervisan los profesionales de soporte técnico de los servicios de soporte técnico de Microsoft (PSS) que cubren ADO y otros desarrolladores de ADO con experiencia.  
  
 Puede encontrar más información sobre las opciones de soporte técnico en el sitio web de ayuda y soporte técnico de Microsoft.