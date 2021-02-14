---
title: 'Implementación en modo de Active Directory: requisitos previos'
titleSuffix: SQL Server Big Data Cluster
description: Configuración de Active Directory para clústeres de macrodatos de SQL Server
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3424863924338a0d3f02102609052d6c159e1043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048025"
---
# <a name="deploy-big-data-clusters-2019-in-active-directory-mode-prerequisites"></a>Implemente [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el modo de Active Directory: Prerrequisitos

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este documento se explica cómo preparar un clúster de macrodatos (BDC) de SQL Server en el modo de autenticación de Active Directory. El clúster usa un dominio de AD existente para la autenticación.

>[!Note]
>Antes de la versión SQL Server 2019 CU5, existe una restricción en los clústeres de macrodatos para que solo se pueda implementar un clúster en un dominio de Active Directory. Esta restricción se quita con la versión CU5. Vea [Concepto: implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en modo de Active Directory](active-directory-deployment-background.md) para obtener información detallada sobre las capacidades nuevas. Los ejemplos de este artículo se ajustan para adaptarse a ambos casos de uso de implementación.

## <a name="background"></a>Información previa

Para habilitar la autenticación Active Directory (AD), el BDC crea automáticamente los usuarios, los grupos, las cuentas de máquina y los nombres de entidad de seguridad de servicio (SPN) que necesitan los distintos servicios del clúster. Para proporcionar cierta independencia respecto de estas cuentas y posibilitar el uso de permisos de ámbito, recomendamos crear una unidad organizativa (UO) antes de la implementación del clúster. Todos los objetos de AD relacionados con BDC se crearán durante la implementación. 

## <a name="pre-requisites"></a>Requisitos previos

### <a name="organizational-unit-ou"></a>Unidad organizativa (OU)
Una unidad organizativa (UO) es una subdivisión dentro de una instancia de Active Directory en la que se colocan usuarios, grupos e incluso otras unidades organizativas. Las unidades organizativas de idea general se pueden usar para reflejar la estructura funcional o empresarial de una organización. En este artículo crearemos una unidad organizativa denominada `bdc` como ejemplo. 

>[!NOTE]
>La unidad organizativa (UO) representa límites administrativos y permite a los clientes controlar el ámbito de autoridad de los administradores de datos. 

Puede seguir [principios de diseño de unidad organizativa](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) para decidir cuál es la mejor estructura para trabajar con unidades organizativas dentro de la organización.

### <a name="ad-account-for-bdc-domain-service-account"></a>Cuenta de AD para la cuenta de servicio de dominio del BDC

Para poder crear todos los objetos necesarios en Active Directory automáticamente, el BDC necesita una cuenta de AD que tenga permisos específicos para crear usuarios, grupos y cuentas del equipo dentro de la unidad organizativa (UO) proporcionada. En este artículo se explicará cómo configurar el permiso de esta cuenta de AD. Usamos una llamada a la cuenta de AD `bdcDSA` como ejemplo en este artículo.

### <a name="auto-generated-active-directory-objects"></a>Objetos de Active Directory generados automáticamente
La implementación de BDC genera automáticamente los nombres de cuenta y de grupo. Cada una de las cuentas representa un servicio en BDC, que las administrará mientras el clúster de BDC esté en uso. Esas cuentas poseen los nombres de entidad de seguridad de servicio (SPN) necesarios para cada servicio.  Para obtener una lista completa de cuentas, grupos y servicios generados automáticamente de AD, consulte [Objetos de Active Directory generados automáticamente](active-directory-objects.md).

>[!IMPORTANT]
>En función de la directiva de expiración de contraseñas establecida en el controlador de dominio, las contraseñas de estas cuentas pueden expirar. La directiva de expiración predeterminada es de 42 días. No hay ningún mecanismo para rotar las credenciales para todas las cuentas del clúster de macrodatos, por lo que el clúster dejará de funcionar cuando se cumpla el período de expiración. Para solucionar este problema, actualice la directiva de expiración de las cuentas de servicio del clúster de macrodatos a "La contraseña nunca expira" en el controlador de dominio. Esta acción se puede realizar antes o después de la fecha de expiración. En el último caso, Active Directory reactivará las contraseñas expiradas.
>
>En la imagen siguiente se muestra dónde se establece esta propiedad en Usuarios y equipos de Active Directory.
>
>:::image type="content" source="media/deploy-active-directory/image25.png" alt-text="Establecimiento de directivas de expiración de contraseñas":::

En los pasos siguientes se da por hecho que ya cuenta con un controlador de dominio de Active Directory. Si no se tiene un controlador de dominio, la [guía](https://social.technet.microsoft.com/wiki/contents/articles/37528.create-and-configure-active-directory-domain-controller-in-azure-windows-server.aspx) siguiente incluye pasos que pueden resultar útiles.

## <a name="create-ad-objects"></a>Creación de objetos de AD

Antes de implementar un BDC con la integración de AD, haga lo siguiente:

1. Cree una unidad organizativa (UO) donde se almacenarán todos los objetos de AD relacionados con BDC. También puede optar por elegir una UO existente durante la implementación.
1. Cree una cuenta de AD para el BDC o use una cuenta existente y proporcione a esta cuenta de AD del BDC los permisos adecuados dentro de la unidad organizativa (UO) proporcionada.

### <a name="create-a-user-in-ad-for-bdc-domain-service-account"></a>Creación de un usuario en AD para la cuenta de servicio de dominio del BDC

El clúster de macrodatos requiere una cuenta con permisos específicos. Antes de continuar, asegúrese de tener una cuenta de AD existente o cree una nueva cuenta, que puede usar el clúster de macrodatos para configurar los objetos necesarios.

Para crear un nuevo usuario en AD, puede hacer clic con el botón derecho en el dominio o la UO y seleccionar **Nuevo** > **Usuario**:

![Cuadro de diálogo Usuarios de Active Directory](./media/deploy-active-directory/image12.png)

En este artículo se hará referencia a este usuario como la *cuenta de servicio de dominio del BDC*.

### <a name="create-an-ou"></a>Creación de una unidad organizativa

En el controlador de dominio, abra **Usuarios y equipos de Active Directory**. En el panel izquierdo, haga clic con el botón derecho en el directorio en el que quiera crear la UO y seleccione **Nuevo** \> **Unidad organizativa** y, después, siga las indicaciones del asistente para crearla. También puede crear una UO con PowerShell:

```powershell
New-ADOrganizationalUnit -Name "<name>" -Path "<Distinguished name of the directory you wish to create the OU in>"
```

En los ejemplos de este artículo se usa `bdc` para el nombre de la UO.

![Unidad organizativa de Active Directory](./media/deploy-active-directory/image13.png)

![Objeto nuevo: unidad organizativa](./media/deploy-active-directory/image14.png)

### <a name="set-permissions-for-an-ad-account"></a>Establecimiento de permisos para una cuenta de AD

Tanto si se ha creado un nuevo usuario de AD como si se usa uno existente, existen ciertos permisos que el usuario debe tener. Esta cuenta es la cuenta de usuario que el controlador del BDC usará al unir el clúster a AD.

La cuenta de servicio de dominio (DSA) del BDC debe ser capaz de crear usuarios, grupos y cuentas de equipo en la UO. En los pasos siguientes, el nombre que hemos asignado a la cuenta de servicio de dominio del BDC es `bdcDSA`. Se puede elegir cualquier nombre para esta cuenta.

1. En el controlador de dominio, abra **Usuarios y equipos de Active Directory**.

1. En el panel izquierdo, navegue hasta el dominio y, luego, hasta la UO que usará `bdc`.

1. Haga clic con el botón derecho en la UO y seleccione **Propiedades**.

1. Asegúrese de haber seleccionado **Características avanzadas** haciendo clic con el botón derecho en la UO y seleccionando **Ver**, y vaya a la pestaña Seguridad.

    ![Propiedades de objetos BDC](./media/deploy-active-directory/image15.png)

1. Haga clic en **Agregar...** y agregue el usuario **bdcDSA**.

    ![Agregar propiedades de objetos BDC](./media/deploy-active-directory/image16.png)

    ![Seleccionar objeto](./media/deploy-active-directory/image17.png)

1. Seleccione el usuario **bdcDSA**, borre todos los permisos y haga clic en **Avanzado**.

1. Haga clic en **Agregar**.

    ![Hacer clic en Agregar](./media/deploy-active-directory/image18.png)

    - Haga clic en **Seleccionar una entidad de seguridad**, inserte **bdcDSA** y haga clic en Aceptar.

    - Establezca **Tipo** en **Permitir**.

    - Establezca **Se aplica a** en **Este objeto y todos los descendientes**.

        ![Establecer Permitir para propiedades](./media/deploy-active-directory/image19.png)

    - Desplácese hasta la parte inferior y haga clic en **Borrar todo**.

    - Desplácese hacia la parte superior y seleccione lo siguiente:
       - **Leer todas las propiedades**
       - **Escribir todas las propiedades**
       - **Crear objetos de equipo**
       - **Eliminar objetos de equipo**
       - **Crear objetos de grupo**
       - **Eliminar objetos de grupo**
       - **Crear objetos de usuario**
       - **Eliminación de objetos de usuario**

    - Haga clic en **Aceptar**

- Haga clic en **Agregar**.

    - Haga clic en **Seleccionar una entidad de seguridad**, inserte **bdcDSA** y haga clic en Aceptar.

    - Establezca **Tipo** en **Permitir**.

    - Establezca **Se aplica a** en **Objetos de equipo descendientes**.

    - Desplácese hasta la parte inferior y haga clic en **Borrar todo**.

    - Desplácese hacia la parte superior y seleccione **Restablecer contraseña**.

    - Haga clic en **Aceptar**

- Haga clic en **Agregar**.

    - Haga clic en **Seleccionar una entidad de seguridad**, inserte **bdcDSA** y haga clic en Aceptar.

    - Establezca **Tipo** en **Permitir**.

    - Establezca **Se aplica a** en **Objetos de usuario descendientes**.

    - Desplácese hasta la parte inferior y haga clic en **Borrar todo**.

    - Desplácese hacia la parte superior y seleccione **Restablecer contraseña**.

    - Haga clic en **Aceptar**

- Haga clic en **Aceptar** dos veces más para cerrar los cuadros de diálogo abiertos.

## <a name="next-steps"></a>Pasos siguientes

[Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el modo de Active Directory](active-directory-deploy.md)

[Solución de problemas de integración de Active Directory de un Clúster de macrodatos de SQL Server](troubleshoot-active-directory.md)

[Concepto: Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en modo de Active Directory](active-directory-deployment-background.md)
