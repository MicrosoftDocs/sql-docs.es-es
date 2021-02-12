---
title: 'Lección 1: Crear una base de datos de suscriptor de ejemplo | Microsoft Docs'
description: Aprenda a crear una pequeña base de datos de "suscriptor" para almacenar los datos de suscripción que usará una suscripción controlada por datos.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 47a882b7-efe5-4ee6-bef4-06118eb56903
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8be94f5ae47a7b7545cd553299673c823993402a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062990"
---
# <a name="lesson-1-creating-a-sample-subscriber-database"></a>Lección 1: Creación de una base de datos de suscriptor de ejemplo

En esta lección del tutorial de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] , creará una pequeña base de datos "suscriptor" para almacenar los datos de suscripción que usados por una suscripción controlada por datos. Cuando la suscripción se haya procesado, el servidor de informes recupera estos datos y los usa para personalizar los resultados del informe. Por ejemplo, las filas de datos incluyen números de pedido específicos para usar como filtros y qué formato de archivo tendrán los informes generados cuando se creen.  
  
En esta lección se da por hecho que está usando [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] para crear una base de datos de SQL Server.  
  
### <a name="to-create-a-sample-subscriber-database"></a>Para crear una base de datos de suscriptor de ejemplo  
  
1.  Inicie [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]y abra una conexión a una instancia del [!INCLUDE[ssDEnoversion_md](../includes/ssdenoversion-md.md)].  
  
2.  Haga clic con el botón derecho en Bases de datos y seleccione **Nueva base de datos...** .  
  
3.  En el cuadro de diálogo Nueva base de datos, en **Nombre de la base de datos**, escriba *Subscribers*. 
4. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  Haga clic en el botón **Nueva consulta** de la barra de herramientas.  
  
6.  Copie las siguientes instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] en la consulta vacía:  
  
    ```  
    Use Subscribers  
    CREATE TABLE [dbo].[OrderInfo] (  
        [SubscriptionID] [int] NOT NULL PRIMARY KEY ,  
        [Order] [nvarchar] (20) NOT NULL,  
        [FileType] [bit],  
        [Format] [nvarchar] (20) NOT NULL ,  
    ) ON [PRIMARY]  
    GO  
  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('1', 'so43659', '1', 'IMAGE')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('2', 'so43664', '1', 'MHTML')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('3', 'so43668', '1', 'PDF')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('4', 'so71949', '1', 'Excel')  
    GO  
    ```  
  
7.  Haga clic en **! Ejecutar** en la barra de herramientas.  
  
8.  Use una instrucción SELECT para comprobar que tiene tres filas de datos. Por ejemplo: `select * from OrderInfo`  
  
## <a name="next-steps"></a>Pasos siguientes  
+ Ha creado correctamente los datos de suscripción que controlarán la distribución de informes y cambiarán los resultados del informe para cada suscriptor. 
+ Después, modificará las propiedades del origen de datos del informe para usar las credenciales almacenadas. 
+ Además, modificará el diseño de informe para que incluya un parámetro usará la suscripción con los datos del suscriptor. [Lección 2: Modificación de las propiedades del origen de datos de informe](../reporting-services/lesson-2-modifying-the-report-data-source-properties.md).  

## <a name="next-steps"></a>Pasos siguientes

[Crear una suscripción controlada por datos](../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)  
[Crear una base de datos](../relational-databases/databases/create-a-database.md)  
[Crear un informe de tabla básico](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
