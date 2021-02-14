---
description: Visualización de relaciones varios a varios en jerarquías derivadas (Master Data Services)
title: Visualización de relaciones varios a varios en jerarquías derivadas
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8b2a9c43-40e0-48f7-a6a9-325beb9f27da
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f550a109d5271a216614d0566ed6f3a4d87be056
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336301"
---
# <a name="show-many-to-many-relationships-in-derived-hierarchies-master-data-services"></a>Visualización de relaciones varios a varios en jerarquías derivadas (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En las jerarquías derivadas se muestran relaciones uno a varios y, ahora, también se pueden ver en ellas relaciones varios a varios.  
  
## <a name="many-to-many-m2m-relationships"></a>Relaciones de varios a varios (M2M)  
 Se puede modelar una relación de varios a varios (M2M) entre dos entidades mediante el uso de una tercera entidad que proporcione una asignación entre ellas.  
  
 ![mds_hierarchies_manytomany](../master-data-services/media/mds-hierarchies-manytomany.png "mds_hierarchies_manytomany")  
  
 En el ejemplo anterior, hay una relación de M2M entre las entidades **Employee** y **TrainingClass** , que proporciona la entidad de asignación **ClassRegistration**. Es posible que un empleado esté registrado como alumno en varias clases, y cada clase puede contener diversos alumnos.  
  
 Antes, las jerarquías derivadas no podían modelar relaciones de M2M. A partir de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], puede crear una jerarquía derivada en la que se muestren, por ejemplo, alumnos por clase, o bien invertir la relación y mostrar las clases agrupadas por alumno.  
  
 En primer lugar, vaya a la página de administración de la jerarquía derivada y cree una nueva jerarquía derivada:  
  
 ![mds_hierarchies_add_derived_hierarchy](../master-data-services/media/mds-hierarchies-add-derived-hierarchy.png "mds_hierarchies_add_derived_hierarchy")  
  
 Después, agregue niveles a la nueva jerarquía derivada, desde el final hasta el principio. En este ejemplo, deseamos mostrar alumnos (empleados) agrupados por clase. Por lo tanto, la entidad **Employee** constituye el nivel hoja de la jerarquía y se agrega en primer lugar:  
  
 ![mds_hierarchies_edit_derived_hierarchy_one](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-one.PNG "mds_hierarchies_edit_derived_hierarchy_one")  
  
 En la captura de pantalla anterior, observe que la entidad **Employee** figura en **Niveles actuales** , en la parte intermedia, como el único nivel. En la **vista previa** de la jerarquía derivada del lado derecho podrá ver una lista de todos los miembros de la entidad **Employee** . En la sección **Niveles disponibles** del lado izquierdo se muestran los niveles que se pueden agregar encima del nivel superior actual (**Employee**). La mayoría de ellos son atributos basados en dominio (DBA) de la entidad **Employee** , incluido el DBA **Department** .  
  
 A partir de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], existe un nuevo tipo de nivel que modela las relaciones de M2M, por ejemplo: **Class (asignado mediante ClassRegistration.Student)**. El nombre del nivel es más detallado que los demás a fin de reflejar la información adicional necesaria para describir la relación de asignación de forma inequívoca. Arrastre este nivel y colóquelo en el nivel **Employee** , que se encuentra en la sección **Niveles actuales** :  
  
 ![mds_hierarchies_edit_derived_hierarchy_two](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-two.PNG "mds_hierarchies_edit_derived_hierarchy_two")  
  
 Ahora, en la vista previa, se muestran los empleados agrupados por las clases de aprendizaje en las que están inscritos. Como se trata de una relación de M2M, cada miembro secundario puede tener varios elementos primarios. En el ejemplo anterior, el empleado **6 {Hillman, Reinout N}** está inscrito como alumno en dos clases: **1 {Master Data Services 101}** y **4 {Career-Limiting Moves}**.  
  
 Esta relación de asignación también puede mostrarse invertida; es decir, con las clases agrupadas por alumno:  
  
 ![mds_hierarchies_available_entities_and_hierarchies](../master-data-services/media/mds-hierarchies-available-entities-and-hierarchies.PNG "mds_hierarchies_available_entities_and_hierarchies")  
  
 Una vez más, vemos como un elemento secundario puede aparecer bajo más de un primario: la clase de aprendizaje **1 {Master Data Services 101}** se muestra bajo **6 {Hillman, Reinout N}** y **40 {Ford, Jeffrey L}**.  
  
 Los miembros de la entidad de asignación **ClassRegistration** no aparecen en ningún lugar de la jerarquía derivada. Se utilizan simplemente para definir las relaciones entre los miembros primarios y secundarios de la jerarquía.  
  
 Puede editar la relación de M2M modificando los miembros de entidades de asignación; para ello, realice una de las siguientes acciones. La relación de M2M es de solo lectura en la página **Derived Hierarchy Explorer** (Explorador de jerarquías derivadas).  
  
-   Modifique los miembros de entidades de asignación en la página **Entity Explorer** (Explorador de entidades) mediante el complemento Master Data Services para Excel, o bien usando el almacenamiento provisional de datos.  
  
-   Arrastre y coloque nodos secundarios entre los elementos primarios en la página **Derived Hierarchy Explorer**(Explorador de jerarquías derivadas).  
  
     Con este método, se modifican los miembros existentes cuando sea posible y se agregan nuevos cuando resulte necesario. Los miembros existentes no se eliminan.  
  
     Por ejemplo, con la entidad de asignación ClassRegistration, al mover un alumno al nodo no utilizado, se cambia a NULL el valor de atributo de clase del miembro de la entidad de asignación correspondiente, pero no se elimina dicho miembro. Por el contrario, al mover un estudiante del nodo no utilizado a alguna clase, si existe un miembro de asignación correspondiente al alumno con la clase NULL, ese miembro se modifica cambiando la clase de NULL al nuevo elemento primario. Si no se encuentra ningún miembro de este tipo, se agrega uno.  
  
     Gracias a este proceso, se evita la eliminación de miembros a fin de evitar la eliminación no deseada de otros datos de usuario (por ejemplo, si la entidad de asignación contiene otros atributos, aparte de los dos que definen la relación de elementos primarios y secundarios). Los usuarios deben eliminar los elementos explícitamente en la entidad de asignación.  
  
 El nuevo nivel de M2M puede aparecer en cualquier parte de una jerarquía derivada donde se permita un nivel de atributo basado en dominios (DBA). Un nivel de M2M puede estar en la parte superior, como en los ejemplos anteriores. Puede encontrarse encima o debajo de un nivel de DBA, incluidos los niveles recursivos. Puede estar en un nivel de límite de jerarquía explícita (en desuso). Es posible encadenar varias relaciones de M2M en la misma jerarquía derivada.  
  
 Asimismo, se pueden ocultar los niveles de M2M, al igual que los demás niveles de una jerarquía derivada.  
   
### <a name="m2m-relationship-in-sample-model"></a><a name="M2MSample"></a> Relación de M2M en el modelo de ejemplo  
Para obtener una demostración de una relación de M2M, vea la jerarquía derivada Region Climate del modelo de ejemplo Customer que se incluye con [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)].   
  
Como se muestra en la siguiente imagen, el nombre del nivel que modela esta relación es ![mds_Number1](../master-data-services/media/mds-number1.png)**Climate (asignado mediante RegionClimate.Region)**. ![mds_Number2](../master-data-services/media/mds-number2.png)**Preview** muestra las regiones agrupadas por los tipos de climas con los que se asocian. Se trata de una relación de M2M porque hay regiones (miembros secundarios) que están asociadas a varias climas (elementos primarios). Por ejemplo, ![mds_Number3](../master-data-services/media/mds-number3.png)**APCR {Asia Pacifico}** está asociada a ![mds_Number4](../master-data-services/media/mds-number4.png)**A {Tropical}** y ![mds_Number5](../master-data-services/media/mds-number5.png)**B {Dry}**.  
  
![mds_M2MRelationship_Example_CustomerModel](../master-data-services/media/mds-m2mrelationship-example-customermodel.png)  
  
Para obtener instrucciones sobre cómo implementar el modelo de ejemplo Customer y otros modelos de ejemplo incluidos con [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consulte [Implementar modelos y datos de ejemplo](~/master-data-services/sql-server-samples-model-deployment-packages-mds.md).   
  
## <a name="one-many-relationship"></a>Relación uno a varios  
 Un miembro de una jerarquía derivada puede ser el elemento primario de muchos miembros secundarios, pero normalmente no puede tener más de un elemento primario (para ver las excepciones, consulte [Seguridad de miembros](#bkmk_member_security)). Por ejemplo, supongamos que hay dos entidades: Employee y Department, donde cada empleado pertenece a un solo departamento. Esta relación se modela agregando a la entidad Employee un atributo basado en dominio (DBA) que haga referencia a la entidad Department:  
  
 ![mds_hierarchies_onetomany](../master-data-services/media/mds-hierarchies-onetomany.png "mds_hierarchies_onetomany")  
  
 Se trata de una relación uno a varios porque cada empleado pertenece a un único departamento, pero cada departamento puede tener varios empleados. Se puede crear una jerarquía derivada en la que se muestre a los empleados agrupados por departamento:  
  
 ![mds_hierarchies_dh_screenshot](../master-data-services/media/mds-hierarchies-dh-screenshot.png "mds_hierarchies_dh_screenshot")  
  
##  <a name="member-security"></a><a name="bkmk_member_security"></a> Seguridad de miembros  
 No se puede usar una jerarquía que permita la duplicación de los miembros (permite a un miembro a tener más de un elemento primario) para asignar permisos de seguridad de miembros. Por ejemplo:  
  
-   Un jerarquía derivada recursiva que no delimite recursiones nulas (cada miembro del nivel recursivo aparece bajo la raíz y su elemento primario recursivo).  
  
-   Una jerarquía derivada recursiva con un nivel por encima del recursivo (cada miembro este último aparece bajo su elemento primario no recursivo y su primario recursivo).  
  
-   Una jerarquía derivada con un nivel de M2M (es posible asignar un elemento secundario a muchos primarios).  
  
## <a name="collections"></a>Colecciones  
 Las colecciones y las jerarquías explícitas se encuentran en desuso. El procedimiento almacenado de conversión (udpConvertCollectionAndConsolidatedMembersToLeaf) convierte los miembros de la colección en miembros hoja y crea jerarquías derivadas de varios a varios para capturar información de pertenencia a la colección.  
  
## <a name="see-also"></a>Consulte también  
 [Jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
