---
title: 'Aplicaciones de copia de seguridad de SQL Server: Servicio de instantáneas de volumen (VSS) y objeto de escritura de SQL'
description: Describe el componente del objeto de escritura de SQL y su rol en el proceso de creación y restauración de instantáneas de VSS para las bases de datos de SQL Server.
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.openlocfilehash: 36fbf9ac4010797561e0852b96b6df6fd85b640d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347303"
---
# <a name="sql-server-back-up-applications---volume-shadow-copy-service-vss-and-sql-writer"></a>Aplicaciones de copia de seguridad de SQL Server: Servicio de instantáneas de volumen (VSS) y objeto de escritura de SQL
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

SQL Server admite el Servicio de instantáneas de volumen (VSS) mediante un escritor (objeto de escritura de SQL) para que una aplicación de copia de seguridad de terceros pueda usar el marco de VSS para hacer copias de seguridad de los archivos de las bases de datos. En este artículo se describe el componente del objeto de escritura de SQL y su rol en el proceso de creación y restauración de instantáneas de VSS para las bases de datos de SQL Server. En él también se incluyen detalles sobre cómo configurar y utilizar el objeto de escritura de SQL para trabajar con aplicaciones de copia de seguridad en el marco de VSS.


## <a name="introduction"></a>Introducción

SQL Server permite crear instantáneas a partir de datos de SQL Server con el Servicio de instantáneas de volumen (VSS). Para ello, se proporciona un objeto de escritura conforme a VSS (objeto de escritura de SQL) para que una aplicación de copia de seguridad de terceros pueda usar el marco de VSS para hacer copias de seguridad de los archivos de las bases de datos. En este artículo se describe el componente del objeto de escritura de SQL y su rol en el proceso de creación y restauración de instantáneas de VSS para las bases de datos de SQL Server. En él también se incluyen detalles sobre cómo configurar y utilizar el objeto de escritura de SQL para trabajar con aplicaciones de copia de seguridad en el contexto del marco de VSS.

## <a name="definition-of-terms"></a>Definición de términos

- **Interfaz de dispositivo virtual**: SQL Server ofrece una interfaz de programación de aplicaciones denominada interfaz de dispositivo virtual (VDI) que ayuda a los proveedores de software independientes a integrar SQL Server en sus productos ofreciendo la compatibilidad con las operaciones relativas a las copias de seguridad y restauración. Estas API están diseñadas para ofrecer una confiabilidad y un rendimiento máximos, así como para ser compatibles con todas las funciones relativas a las copias de seguridad y restauración de SQL Server, incluidas todas las capacidades de copias de seguridad interactivas y de instantáneas. Para obtener más información, consulte [Especificación de la interfaz de dispositivo de copia de seguridad virtual de SQL Server 2005](https://www.microsoft.com/download/details.aspx?id=17282). 

- **Solicitante**: proceso, ya sea automatizado o GUI, que solicita que se tomen uno o más conjuntos de instantáneas de uno o más volúmenes originales. En este documento, un solicitante también se usa para implicar una aplicación de copia de seguridad que está creando una instantánea de bases de datos de SQL Server.

## <a name="about-vss"></a>Acerca de VSS

VSS proporciona la infraestructura del sistema para ejecutar aplicaciones de VSS en sistemas Windows. Aunque es muy transparente tanto para el usuario como para el desarrollador, VSS hace lo siguiente:

- Coordina las actividades de los proveedores, escritores y solicitantes en la creación y el uso de instantáneas.
- Facilita el proveedor del sistema predeterminado.
- Implementa la funcionalidad de controlador de bajo nivel necesaria para que cualquier proveedor funcione.

El servicio VSS se inicia a petición, de modo que, para que las operaciones de VSS se realicen correctamente, este servicio debe estar habilitado.

## <a name="vss-components"></a>Componentes de VSS

VSS coordina las actividades de los siguientes componentes cooperativos: 

- Los proveedores poseen los datos de la instantánea y crean instancias de las instantáneas.
- Los escritores son aplicaciones que cambian datos y participan en el proceso de sincronización de instantáneas.
- Los solicitantes inician la creación y la destrucción de instantáneas. Nuestro diseño se centra en el escenario en el que el solicitante es una aplicación de copia de seguridad.

VSS proporciona coordinación entre estas entidades: 

![Diagrama que muestra cómo VSS proporciona coordinación entre estas entidades.](media/sql-server-vss-writer-backup-guide/coordinations.png)

En este diagrama se muestran todos los componentes que participan en una actividad de instantánea de VSS habitual. En este escenario, SQL Server, incluido el objeto de escritura de SQL, actúa como escritor en uno de los cuadros de escritura.  Otros escritores de este tipo pueden ser Exchange Server, etc.

En el resto de este documento, se da por supuesto que el lector está familiarizado con los términos del marco de VSS.  Consulte la documentación sobre el [Servicio de instantáneas de volumen](/windows/win32/vss/volume-shadow-copy-service-overview) para obtener más información.

## <a name="about-sql-writer"></a>Acerca del objeto de escritura de SQL

El objeto de escritura de SQL es un elemento VSS Writer proporcionado por SQL Server. Controla la interacción de VSS con SQL Server. El objeto de escritura de SQL incluye SQL Server como servicio independiente y se instala como parte de la instalación de SQL Server. De forma predeterminada, el objeto de escritura de SQL no está habilitado. Debe habilitarse explícitamente para ejecutarse en el equipo del servidor.

El rol del objeto de escritura de SQL en una operación de copia de seguridad de instantáneas de VSS: 

![Instantánea de VSS](media/sql-server-vss-writer-backup-guide/sql-vss-snapshot.png)



## <a name="configuring-the-sql-writer"></a>Configuración del objeto de escritura de SQL

El servicio del objeto de escritura de SQL está instalado en el sistema como parte de la instalación de SQL Server y está configurado para iniciarse automáticamente al iniciar Windows. 

### <a name="sql-writer-service-account"></a>Cuenta de servicio del objeto de escritura de SQL

Durante la instalación, la cuenta del objeto de escritura de SQL se instalará para usar la cuenta de sistema local. Puesto que el objeto de escritura de SQL necesita comunicarse con SQL Server mediante API de VDI exclusivas, la cuenta del objeto de escritura de SQL debe tener derechos de acceso suficientes para SQL Server y VSS.  La configuración del servicio como una cuenta de sistema local proporciona derechos suficientes para que el servicio se ejecute correctamente.

  > [!NOTE]
  > Para que el servicio del objeto de escritura de SQL funcione correctamente, es importante asegurarse de que la cuenta de sistema local no se quite del rol "sa" de la instancia de SQL Server.

### <a name="enabling-and-starting-sql-writer"></a>Habilitación e inicio del objeto de escritura de SQL

Para iniciar y usar el objeto de escritura de SQL, se debe realizar lo siguiente:

El servicio del objeto de escritura de SQL se puede habilitar marcando este servicio como "Automático". Para abrir los servicios a través del panel de control, haga clic en Inicio, en Panel de control, haga doble clic en Herramientas administrativas y, a continuación, haga doble clic en Servicios. En el panel Servicios, haga doble clic en el servicio del objeto de escritura de SQL y cambie la propiedad "Tipo de inicio" a "Automático".

El servicio también debe iniciarse seleccionando el botón "Iniciar" en la propiedad "Estado del servicio", en la pantalla de propiedades del servicio mencionada anteriormente.

Para mayor comodidad, el servicio del objeto de escritura de SQL realizará estos cambios automáticamente la primera vez que se inicie.

  > [!NOTE]
  > En determinados casos en los que se instala una instancia de SQL Server Express y una aplicación utiliza la característica Instancias de usuario, el objeto de escritura de SQL puede iniciarse automáticamente mediante SQL Server. Esto se hace para facilitar la enumeración de estas instancias de usuario durante una operación de copia de seguridad de VSS. 

## <a name="backuprestore-operations-and-supported-versions"></a>Operaciones de copias de seguridad y restauración y versiones compatibles

### <a name="version-support"></a>Compatibilidad de versiones

El objeto de escritura de SQL se incluye como parte de SQL Server y solo admite instancias de SQL Server. 

El objeto de escritura de SQL enumerará también las instancias de SQL Server Express. El objeto de escritura de SQL también enumerará las instancias de usuario iniciadas por SQL Server Express.


## <a name="supported-backuprestore-operations"></a>Operaciones de restauración y copia de seguridad

SQL Server, con el objeto de escritura de SQL, admitirá los siguientes modos de operaciones de copia de seguridad basadas en VSS:

- No basada en componentes
- Basada en componentes

### <a name="noncomponent-based-backup-operations"></a>Operaciones de copia de seguridad no basadas en componentes

Las copias de seguridad no basadas en componentes seleccionan implícitamente las bases de datos mediante la lista de volúmenes del conjunto de instantáneas. El objeto de escritura de SQL busca bases de datos incompletas y, si encuentra alguna, genera un error. Una base de datos incompleta es aquella en la que se selecciona un subconjunto de archivos mediante la lista de volúmenes.

En el modelo no basado en componentes, solo se admiten bases de datos con el modelo de recuperación simple. No se admite la puesta al día después de una restauración.

### <a name="component-based-backup-operations"></a>Operaciones de copia de seguridad basadas en componentes

Las copias de seguridad basadas en componentes son las que se prefieren y se recomiendan con el objeto de escritura de SQL, ya que la aplicación (aplicación de copia de seguridad de VSS) seleccionará explícitamente las bases de datos de los metadatos que se devuelven desde el objeto de escritura de SQL. El conjunto de instantáneas debe incluir todos los volúmenes necesarios para hacer una copia de seguridad de esas bases de datos. La infraestructura de VSS no agrega automáticamente los volúmenes necesarios para el conjunto de bases de datos seleccionado. Todos los volúmenes de copia de seguridad deben incluirse en el conjunto de instantáneas de volumen. Es responsabilidad de la aplicación de copia de seguridad asegurarse de que todos los volúmenes de respaldo estén incluidos en el conjunto de instantáneas.  El objeto de escritura de SQL detectará las bases de datos incompletas (con volúmenes de respaldo fuera del conjunto de instantáneas) y generará un error en la copia de seguridad.

En el resto de este tema se da por supuesto que se usan copias de seguridad basadas en componentes como parte del proceso de creación de instantáneas de VSS para SQL Server.

## <a name="features-supported-by-sql-writer"></a>Características compatibles con el objeto de escritura de SQL


- **Texto completo**: el objeto de escritura de SQL informa de los contenedores de catálogos de texto completo con especificaciones de archivo recursivas en los componentes de base de datos del documento de metadatos del escritor.  Se incluyen automáticamente en la copia de seguridad si se selecciona el componente de base de datos.
- **Copias de seguridad y restauración diferenciales**: el objeto de escritura de SQL admite la copia de seguridad o restauración diferencial mediante dos mecanismos diferenciales de VSS, que son Archivo parcial y Archivo diferenciado por hora de última modificación.
- **Archivo parcial**:   El objeto de escritura de SQL utiliza el mecanismo Archivo parcial de VSS para informar de los intervalos de bytes modificados en sus archivos de base de datos.  
- **Archivo diferenciado por hora de última modificación**: el objeto de escritura de SQL utiliza el mecanismo Archivo diferenciado por hora de última modificación de VSS para informar de los archivos modificados en los catálogos de texto completo.
- **Restauración con desplazamiento**: el objeto de escritura de SQL admite la nueva especificación de destino de VSS durante la restauración.  La nueva especificación de destino de VSS permite reubicar un archivo de registro o de base de datos, o un contenedor de catálogos de texto completo, como parte de la operación de restauración.
- **Cambio de nombre de una base de datos**: es posible que un solicitante necesite restaurar una base de datos de SQL con un nombre nuevo, especialmente si la base de datos se va a restaurar en paralelo con la base de datos original. El objeto de escritura de SQL permite cambiar el nombre de una base de datos durante la operación de restauración, siempre y cuando la base de datos permanezca dentro de la instancia original de SQL.
- **Copia de seguridad de solo copia**: en ocasiones, es necesario hacer una copia de seguridad que esté destinada a un propósito especial, por ejemplo, cuando se necesita hacer una copia de una base de datos con fines de prueba.  Esta copia de seguridad no debe afectar a los procedimientos de copia de seguridad y restauración generales de la base de datos. Con la opción COPY_ONLY se especifica que la copia de seguridad se hace "fuera de banda" y no debe afectar a la secuencia normal de copias de seguridad. El objeto de escritura de SQL admite el tipo de copia de seguridad de "solo copia" con instancias de SQL Server.
- **Recuperación automática de instantánea de base de datos**:   normalmente, una instantánea de una base de datos de SQL Server obtenida mediante el marco de VSS se encuentra en un estado no recuperado. No se puede tener acceso de forma segura a los datos de la instantánea antes de pasar a través de la fase de recuperación para revertir las transacciones en curso y colocar la base de datos en un estado coherente. Es posible que una aplicación de copia de seguridad de VSS solicite la autorrecuperación de las instantáneas como parte del proceso de creación de instantáneas.

Estas nuevas características y su uso se describen de manera más detallada en la sección Detalles de la opción Copias de seguridad y restauración, en este tema.

### <a name="what-is-not-supported"></a>Elementos no compatibles

- El objeto de escritura de SQL no es compatible con las copias de seguridad de los registros.
- No se admite la copia de seguridad de archivos o grupos de archivos.
- No se admite la restauración de página.
- Las instantáneas de base de datos no son compatibles y se omiten durante la creación de instantáneas de VSS, tanto si son relativas a componentes como si no lo son. 
- Bases de datos cerradas automáticamente o bases de datos con el apagado habilitado.
- Linux no proporciona un marco de VSS y, por lo tanto, el objeto de escritura de SQL no está disponible en Linux.

En la tabla siguiente se enumeran los tipos de copias de seguridad de instantáneas compatibles con el objeto de escritura de SQL o con SQL Server, y que funcionan con el marco de VSS para todas las ediciones de SQL Server.

| **Operación de copia de seguridad o restauración**                 | **Basada en componentes**           | **No basada en componentes** |
|:-------------------------------------------- | :---------------------------- | :--------------------- |
|Copia de seguridad de datos completa </br> (incluido el catálogo de texto completo)| Sí                     | Sí                    |
|Restauración completa                                  | Sí                           | Sí                    |
|Restauración completa (sin recuperación)                    | Sí                           | No                     |
|Copia de seguridad diferencial                           | Sí                           | No                     |
|Restauración diferencial                          | Sí                           | No                     |
|Restauración con desplazamiento                             | Sí                           | No                     |
|Cambio de nombre de una base de datos                               | Sí                           | No                     |
|Copia de seguridad de solo copia                              | Sí                           | No                     |
|Instantáneas recuperadas automáticamente                       | Sí                           | No                     |
|Copia de seguridad de registros                                    | No                            | No                     |
|Instantáneas de base de datos                            | No                            | No                     |
|Bases de datos cerradas automáticamente</br> Bases de datos con apagado | Sí                        | No                     |
|Bases de datos de grupo de disponibilidad                  | Sí                           | No en secundarias        | 




## <a name="snapshot-creation-process"></a>Proceso de creación de instantáneas

El marco de VSS coordina las actividades de un solicitante (una aplicación de copia de seguridad) y el objeto de escritura de SQL durante la creación de instantáneas de SQL Server. Para habilitar esta coordinación, el marco de VSS define las interfaces del escritor y el solicitante.  Estas interfaces deben implementarse mediante aplicaciones y escritores de solicitantes participantes. El objeto de escritura de SQL implementa las interfaces necesarias del escritor. Como parte del proceso de creación de instantáneas, el marco de VSS llama a las interfaces del objeto de escritura de SQL. El objeto de escritura de SQL interactúa con las instancias de SQL Server en el sistema para facilitar la creación de instantáneas.

El marco de VSS define un conjunto de API para su uso por parte de una aplicación solicitante o de copia de seguridad. Un desarrollador de aplicaciones de copia de seguridad debe seguir estos patrones de llamada de API para trabajar con el proceso de creación de instantáneas de marco de VSS. En las secciones siguientes se describe el proceso de creación de instantáneas desde el punto de vista del objeto de escritura de SQL. También detallan algunas de las interacciones internas entre el solicitante, el marco de VSS, el objeto de escritura de SQL y las instancias de SQL Server. Para obtener más información sobre estos pasos y obtener detalles de las interfaces del marco de VSS, consulte la documentación sobre el Servicio de instantáneas de volumen.    

  > [!NOTE]
  > Se da por supuesto que el lector está familiarizado con el marco de VSS y el proceso de creación de copias de seguridad en general. Estas secciones se proporcionan como información adicional sobre la participación del objeto de escritura de SQL en el proceso de creación de copias de seguridad de VSS.

## <a name="snapshot-creation-workflow"></a>Flujo de trabajo de creación de instantáneas

![Flujo de datos](media/sql-server-vss-writer-backup-guide/dataflow-chart.png)

La imagen muestra el diagrama de flujo de datos durante una operación de copia de seguridad o creación de instantáneas basada en componentes. Para comprender con más detalle las tareas básicas relacionadas con la realización de una copia de seguridad, resulta útil dividir esta información general en las siguientes fases:

- Inicialización de la copia de seguridad
- Fase de detección de copias de seguridad
- Tareas previas a la copia de seguridad
- Copia de seguridad real de archivos
- Finalización de la copia de seguridad



### <a name="backup-initialization"></a>Inicialización de la copia de seguridad

Durante esta fase de la copia de seguridad, el solicitante (aplicación de copia de seguridad) se enlaza a la interfaz de la instantánea **IvssBackupComponents** y la inicializa como preparación para la copia de seguridad. También llama a la API de VSS **IVssGatherWriterMetadata** para indicar al marco de VSS que recopile los metadatos de todos los escritores.

El marco de VSS llamará a cada uno de los escritores registrados, incluido el objeto de escritura de SQL para los metadatos del escritor mediante el evento **OnIdentify**. El objeto de escritura de SQL consultará las instancias de SQL Server para obtener la información de los metadatos de copia de seguridad de cada base de datos y, de este modo, crear el documento de metadatos del escritor. Esta fase también se conoce como enumeración de metadatos.

El documento de metadatos del escritor es un documento que contiene información que se pasa del escritor al solicitante (aplicación de copia de seguridad). El documento de metadatos del escritor contiene la siguiente información:

- Identificador de la aplicación y nombre descriptivo.
- Ubicación de los archivos y los componentes.
- Archivos que hay que incluir y excluir de una copia de seguridad.
- Opciones que deben usarse en el momento de la restauración.
- Estos datos se devuelven al solicitante a través del marco de VSS.

### <a name="sql-writer-metadata-document"></a>Documento de metadatos del objeto de escritura de SQL

Se trata de un documento XML creado por un escritor (objeto de escritura de SQL, en este caso) mediante la interfaz **IVssCreateWriterMetadata** y que contiene información sobre el estado y los componentes del escritor. Los detalles estructurales de un documento de metadatos del escritor se describen en la documentación de la API de VSS. A continuación se mencionan algunos de los detalles del documento de metadatos del objeto de escritura de SQL.

- Información de identificación del escritor
    - **Nombre del escritor**: L"SqlServerWriter"
    - **Id. de clase del escritor**: 0xa65faa63, 0x5ea8, 0x4ebc, 0x9d, 0xbd, 0xa0, 0xc4, 0xdb, 0x26, 0x91 y 0x2a 
    - **Id. de instancia del escritor**: L"SQL Server:SQLWriter" 
    - **VSSUsageType**: VSS_UT_USERDATA 
    - **VSSSourceType**: VSS_ST_TRANSACTEDDB 
- Información del nivel del escritor: VSS_APP_BACK_END 
- Especificación del método de restauración: VSS_RME_RESTORE_IF_CAN_REPLACE.
- Esquema de copia de seguridad compatible (IVssCreateWriterMetadata::SetBackupSchema API)
    - VSS_BS_DIFFERENTIAL: copia de seguridad diferencial
    - VSS_BS_TIMESTAMPED: basado en marcas de tiempo para los archivos del catálogo de texto completo.
    - VSS_BS_LAST_MODIFY: copia de seguridad diferencial según la hora de la última modificación.
    - VSS_BS_WRITER_SUPPORTS_NEW_TARGET: admite la nueva opción de ubicación de destino.
    - VSS_BS_WRITER_SUPPORTS_RESTORE_WITH_MOVE: admite la restauración "con desplazamiento".
    - VSS_BS_COPY: admite la opción de copia de seguridad de "solo copia".
- Información de nivel de componente: contiene información específica del nivel de componente proporcionada por el objeto de escritura de SQL.
   - **Tipo**: VSS_CT_FILEGROUP.
   - **Nombre**: nombre del componente (nombre de la base de datos).
   - **Ruta de acceso lógica**: corresponde a la instancia del servidor y tendrá el formato "servidor\nombre de la instancia" para las instancias con nombre y "servidor" para la predeterminada.
   - **Marcas de componentes**
   - **VSS_CF_APP_ROLLBACK_RECOVERY**: indica que las instantáneas de SQL Server siempre requieren una fase de "recuperación" para que los archivos sean coherentes y se puedan utilizar en escenarios que no sean de copia de seguridad (es decir, escenario de reversión de aplicaciones).
   - Se puede seleccionar: true.
   - Se puede seleccionar para restauración: true. 
   - Métodos de restauración admitidos: VSS_RME_RESTORE_IF_CAN_REPLACE.

La única extensión de la estructura del conjunto de componentes en SQL Server es la introducción de los catálogos de texto completo.  Los catálogos de texto completo son directorios de contenedores que no se pueden expresar como archivos de registro o base de datos de VSS, dado que los archivos de registro y la base de datos de VSS no tienen especificación recursiva.  Por lo tanto, el objeto de escritura de SQL usará un componente de grupo de archivos de VSS (VSS_CT_FILEGROUP) para representar el componente de nivel de base de datos y los archivos del grupo de archivos para representar la base de datos, el registro y los archivos del catálogo de texto completo.

Al final de este documento se proporciona un ejemplo del documento de metadatos del escritor.

### <a name="backup-discovery"></a>Detección de copias de seguridad
En esta fase, un solicitante examina el documento de metadatos del escritor y crea y rellena un **documento Componentes de copias de seguridad** con cada componente del que sea necesario hacer una copia de seguridad. También especifica las opciones de copia de seguridad y los parámetros necesarios como parte de este documento. En el caso del objeto de escritura de SQL, cada instancia de base de datos de la que se debe hacer una copia de seguridad es un componente independiente.

#### <a name="backup-components-document"></a>Documento Componentes de copias de seguridad
Se trata de un documento XML creado por un solicitante mediante la interfaz **IVssBackupComponents** en el transcurso de la configuración de una operación de restauración o copia de seguridad. El documento Componentes de copias de seguridad contiene una lista de los componentes incluidos explícitamente, de uno o varios escritores, que participan en una operación de copia de seguridad o restauración. No contiene información de los componentes incluidos implícitamente. En cambio, un documento de metadatos de escritor solo contiene componentes de escritura que pueden participar en una copia de seguridad. Los detalles estructurales de un documento de componentes de copias de seguridad se describen en la documentación de la API de VSS.

#### <a name="prebackup-tasks"></a>Tareas previas a la copia de seguridad
Las tareas previas a la copia de seguridad en VSS se centran en crear una instantánea de los volúmenes que contienen datos para la copia de seguridad. La aplicación de copia de seguridad guardará los datos de la instantánea, no del volumen real.

Los solicitantes normalmente esperan en los escritores durante la preparación de la copia de seguridad y mientras se crea la instantánea. Si el objeto de escritura de SQL está participando en la operación de copia de seguridad, debe configurar sus archivos y también estar a punto para la copia de seguridad y la instantánea.

#### <a name="prepare-for-backup"></a>Preparación para la copia de seguridad
El solicitante deberá establecer el tipo de operación de copia de seguridad que se debe hacer (**IVssBackupComponents::SetBackupState**) y, a continuación, notificará a los escritores a través de VSS para preparar una operación de copia de seguridad con **IVssBackupComponents::PrepareForBackup**.

Al objeto de escritura de SQL se le concede acceso al documento Componentes de copias de seguridad, en el que se detallan las bases de datos de las que se debe hacer una copia de seguridad. Todos los volúmenes de copia de seguridad deben incluirse en el conjunto de instantáneas de volumen. El objeto de escritura de SQL detectará las bases de datos incompletas (con volúmenes de respaldo fuera del conjunto de instantáneas) y generará un error en la copia de seguridad durante el evento posterior a la instantánea.

**Inicio de la instantánea**: el solicitante iniciará el proceso de instantáneas llamando a la interfaz DoSnapshotSet del marco de VSS.

**Creación de la instantánea**: esta fase implica una serie de interacciones entre el marco de VSS y el objeto de escritura de SQL.

1. _Preparación para tomar la instantánea_: el objeto de escritura de SQL llamará a SQL Server con el fin de prepararse para la creación de instantáneas.
1. _Inmovilización_: el objeto de escritura de SQL llamará a SQL Server para inmovilizar todas las E/S de base de datos para cada una de las bases de datos de las que se haga una copia de seguridad en la instantánea. Una vez que el evento de inmovilización vuelva al marco de VSS, VSS creará la instantánea.
1. _Reanudación_: en este evento, el objeto de escritura de SQL llamará a las instancias de SQL Server para "reanudar" las operaciones de E/S normales.


La fase de creación de instantáneas es rápida (menos de 60 segundos) con el fin de evitar el bloqueo de todas las escrituras en la base de datos.

**Posterior a la instantánea**: si es necesaria la autorrecuperación de la instantánea, el objeto de escritura de SQL realizará la autorrecuperación para cada base de datos que se haya seleccionado para figurar en la instantánea. Para obtener una explicación detallada, consulte Instantáneas recuperadas automáticamente.

#### <a name="actual-backup-of-files"></a>Copia de seguridad real de archivos

En esta fase, el solicitante puede trasladar los datos a un medio de copia de seguridad, si es necesario. En esta fase las interacciones son entre el solicitante y el marco de VSS. El objeto de escritura de SQL no está implicado.

1. Obtención del estado del escritor: devuelve el estado de los escritores. Es posible que el solicitante tenga que controlar los errores aquí.
1. Realización de una copia de seguridad.

En este momento, el solicitante puede trasladar los datos a un medio de copia de seguridad, si es necesario.

#### <a name="backup-complete"></a>Copia de seguridad completa

Este evento indicará que la copia de seguridad se ha completado correctamente.

Este también es el momento en el que el objeto de escritura de SQL puede confirmar la copia de seguridad como una base diferencial, si la copia de seguridad actual es una copia de seguridad completa de la base de datos, y no una copia de seguridad de solo copia.


  > [!NOTE]
  > El solicitante debe enviar explícitamente este evento (evento de copia de seguridad completa) para permitir que el objeto de escritura de SQL confirme las copias de seguridad de base diferenciales. Si no se recibe este evento, la copia de seguridad que se creará no será una copia de seguridad "de base diferencial" válida.

#### <a name="save-writer-metadata"></a>Guardado de los metadatos del escritor

El solicitante debe guardar el documento Componentes de copias de seguridad y los metadatos de copia de seguridad de cada componente, junto con los datos de respaldo de la instantánea. Los metadatos de este escritor son necesarios para las operaciones de restauración del objeto de escritura de SQL y SQL Server.

#### <a name="backup-termination"></a>Finalización de la copia de seguridad

El solicitante finaliza la instantánea liberando la interfaz **IVssBackupComponents** o llamando a **IVssBackupComponents::DeleteSnapshots**.

## <a name="restore-process"></a>Proceso de restauración

En esta sección se describe el flujo de trabajo de la operación de restauración y varios de los pasos implicados.

### <a name="restore-operation-workflow"></a>Flujo de trabajo de la operación de restauración

En la siguiente ilustración se muestra el diagrama de flujo de datos durante una operación de restauración de VSS. Para comprender con más detalle las tareas básicas relacionadas con la realización de una restauración, resulta útil dividir esta información general en los siguientes temas:

- Inicialización de la restauración
- Preparación para la restauración
- Restauración real de archivos
- Limpieza y finalización de la restauración

![Flujo del proceso de restauración](media/sql-server-vss-writer-backup-guide/restore-process-flow.png)

En todos los escenarios de restauración basados en componentes de VSS, el objeto de escritura de SQL controla la restauración de bases de datos en dos fases distintas.

- **Previa a la restauración**:  el objeto de escritura de SQL controla la validación, el cierre de identificadores de archivos, etc.
- **Posterior a la restauración**:  el objeto de escritura de SQL conecta la base de datos y bloquea la recuperación, si es necesario.

Entre estas dos fases, la aplicación de copia de seguridad es responsable de mover los datos pertinentes en torno a SQL.

### <a name="restore-initialization"></a>Inicialización de la restauración

Durante la fase de inicialización de una restauración, el solicitante debe tener acceso a los documentos Componentes de copias de seguridad almacenados.

El documento Componentes de copias de seguridad que se genera durante la operación de copia de seguridad se almacena como parte de los datos de copia de seguridad. La aplicación de copia de seguridad debe devolver estos datos al marco de VSS. El objeto de escritura de SQL obtiene acceso a estos datos al principio del proceso de restauración.

#### <a name="prepare-for-restore"></a>Preparación para la restauración

Al preparar una restauración, un solicitante utiliza el documento Componentes de copias de seguridad almacenado para determinar qué se va a restaurar y cómo.  El solicitante seleccionará los componentes que se van a restaurar y establecerá las opciones de restauración apropiadas según sea necesario.

Si una aplicación de copia de seguridad intenta aplicar copias de seguridad diferenciales o de registros encima de la operación de restauración actual (es decir, se necesita "RESTORE WITH NORECOVERY"), se debe establecer la opción siguiente como parte de la creación de componentes para cada base de datos que se está restaurando.

```
IVssBackupComponents::SetAdditionalRestores(true)
```

Una vez que se han establecido todos los detalles necesarios en el documento Componentes de copias de seguridad, el solicitante realiza la llamada **IVssBackupComponents::PreRestore** para generar un evento de restauración previa a través de VSS que controlarán los escritores.

El objeto de escritura de SQL examinará el documento Componentes de copias de seguridad proporcionado para identificar las bases de datos adecuadas y eliminará los archivos adicionales creados desde el momento de la copia de seguridad. También comprueba los espacios en disco y cierra los identificadores de archivos de las bases de datos abiertas para que el solicitante pueda copiar los datos necesarios durante la fase de restauración. Esta fase permite detectar cualquier condición de error temprana antes de que el solicitante haga la propia copia de los archivos. SQL Server también colocará la base de datos en estado de restauración.  A partir de este punto, la base de datos no se puede iniciar hasta que se realice una restauración correcta.

#### <a name="restore-files"></a>Restauración de archivos

Se trata únicamente de una acción específica del solicitante. Es responsabilidad del solicitante (aplicación de copia de seguridad) copiar los archivos de base de datos necesarios (o copiar rangos de datos pertinentes para restauraciones diferenciales) en los lugares adecuados. El objeto de escritura de SQL no está implicado en esta operación.

#### <a name="cleanup-and-termination"></a>Limpieza y finalización

Una vez que todos los datos se hayan restaurado en los lugares correctos, una llamada de un solicitante para notificar que la operación de restauración se ha completado (**IvssBackupComponents::PostRestore**) permitirá que el objeto de escritura de SQL sepa que se pueden iniciar las acciones posteriores a la restauración.  En este momento, el objeto de escritura de SQL realizará la fase de puesta al día de la recuperación tras bloqueo. Si no se solicita la recuperación (es decir, si el solicitante no especifica SetAdditionalRestores(true)), la fase de reversión del paso de recuperación también se llevará a cabo durante esta fase.

## <a name="backup-and-restore-option-details"></a>Detalles de la opción Copias de seguridad y restauración

En esta sección se describen detalladamente todas las opciones de copia de seguridad y restauración admitidas por el objeto de escritura de SQL.

### <a name="requestor-creates-a-volume-shadow-copy"></a>Creación de instantánea de volumen por parte del solicitante

El objeto de escritura de SQL podría estar implicado en el proceso de creación de instantáneas de volumen (fuera del contexto de copias de seguridad y restauración) porque los volúmenes de respaldo de los archivos de base de archivo se han agregado al conjunto de instantáneas de volumen.  En este caso, el objeto de escritura de SQL solo participa en la enumeración de metadatos, la inmovilización, la reanudación, la preparación para la creación de instantáneas y la coordinación posterior a la instantánea. Consulte el diagrama de flujo de datos para obtener más detalles al respecto.

### <a name="full-backuprestore"></a>Copia de seguridad copia de seguridad completa y restauración

El objeto de escritura de SQL admite las operaciones de copias de seguridad y restauración completas en modo basado en componentes y en modo no basado en componentes.

### <a name="noncomponent-based-backup-and-restore"></a>Copias de seguridad y restauración no basadas en componentes

En una copia de seguridad o restauración no basada en componentes, el solicitante especifica un volumen o un árbol de carpetas del que se va a hacer una copia de seguridad o restauración. Se hace una copia de seguridad o restauración de todos los datos del volumen o la carpeta especificados.

#### <a name="backup"></a>Copia de seguridad

En una copia de seguridad no basada en componentes, el objeto de escritura de SQL selecciona implícitamente las bases de datos mediante la lista de volúmenes del conjunto de instantáneas.  El objeto de escritura busca bases de datos incompletas y, si encuentra alguna, genera un error. Una base de datos incompleta es aquella en la que se selecciona un subconjunto de archivos mediante la lista de volúmenes.  La puesta al día (restauración de registros o diferenciales) tras una restauración no se admite a través del objeto de escritura de SQL.

#### <a name="restore"></a>Restauración

El solicitante restaura las bases de datos de las que se ha hecho una copia de seguridad en modo no basado en componentes.  Tras dichas restauraciones no se puede realizar una restauración con puesta al día, como la restauración de registros o la restauración diferencial.

En el caso de las operaciones de restauración no basadas en componentes, la restauración debe realizarse con la instancia de SQL Server sin conexión, o las bases de datos de destino se quitan o desasocian para asegurarse de que los archivos estén sin conexión.  Los archivos se copian en contexto y, a continuación, las bases de datos se conectan. Todo esto se produce fuera del ámbito del objeto de escritura de SQL.

### <a name="component-based-backup-and-restore"></a>Copias de seguridad y restauración basadas en componentes

En una copia de seguridad basada en componentes, el solicitante selecciona explícitamente los componentes de base de datos (a partir de los metadatos que el objeto de escritura de SQL devuelve al cliente) de los que se hará una copia de seguridad o restauración.

#### <a name="backup"></a>Copia de seguridad

En una copia de seguridad basada en componentes, todos los volúmenes de respaldo de las bases de datos seleccionadas deben incluirse en el conjunto de instantáneas de volumen.  De lo contrario, el objeto de escritura de SQL detectará las bases de datos incompletas (con volúmenes de respaldo fuera del conjunto de instantáneas) y generará un error en la copia de seguridad.  Una copia de seguridad completa hace copias de seguridad de los datos de la base de datos y de todos los archivos de registro necesarios para ponerla en un estado transaccionalmente coherente en el momento de la restauración.

#### <a name="full-restore-without-rollforward"></a>Restauración completa sin puesta al día

A veces, una restauración completa de la copia de seguridad de la base de datos se hace sin necesidad de puestas al día adicionales. Esto puede deberse a que no hay metadatos que faciliten la puesta al día o, en ocasiones, a que las puestas al día no son necesarias. En esta sección se describen brevemente las dos situaciones.  

#### <a name="no-metadatano-rollforward"></a>Sin metadatos y sin puesta al día

Si no se guardan metadatos de escritor (metadatos de copia de seguridad basados en componentes) durante la operación de copia de seguridad, la restauración debe realizarse con la instancia de SQL Server sin conexión, o las bases de datos de destino se quitan o desasocian para asegurarse de que los archivos estén sin conexión.  Los archivos se copian en contexto y, a continuación, las bases de datos se conectan. Todo esto se produce fuera del ámbito del objeto de escritura de SQL.

#### <a name="metadata-exist-but-no-additional-rollforward-is-needed"></a>Existencia de metadatos, pero sin necesidad de puesta al día adicional

El solicitante restaura las bases de datos de las que se ha hecho una copia de seguridad en modo basado en componentes, pero no se solicitan puestas al día. En este caso, SQL Server realizará la recuperación tras bloqueo en la base de datos como parte de la restauración.

**Restauración completa con puestas al día adicionales**: el solicitante puede emitir una restauración especificando la opción SetAdditionalRestores (true).  Esta opción indica que el solicitante va a proceder con más restauraciones con puestas al día (como la restauración de registros, la restauración diferencial, etc.). Esto indica a SQL Server que no realice el paso de recuperación al final de la operación de restauración.

Esto solo es posible si los metadatos del escritor se guardaron durante la copia de seguridad y están disponibles para el objeto de escritura de SQL en el momento de la restauración. El servicio SQL Server debe estar en ejecución antes de que el solicitante indique a VSS que realice la actividad de restauración.

El objeto de escritura de SQL espera la siguiente secuencia:

1. Preparación para restaurar cada base de datos: esta actividad implica cerrar todos los identificadores de archivos para permitir que la aplicación solicitante copie o monte los archivos de base de datos.
1. La aplicación solicitante copia o monta los archivos.
1. Finalización de la restauración (con NORECOVERY):  las bases de datos se ponen en línea, pero en el estado "Restaurando".

Las copias de seguridad de SQL convencionales, ya sean diferenciales o de registros, se pueden usar para poner al día la base de datos a través de VDI/T-SQL o aplicando la restauración diferencial mediante el marco de VSS.

### <a name="full-text-support"></a>Compatibilidad con texto completo

El objeto de escritura de SQL informa de los contenedores de catálogos de texto completo con especificaciones de archivo recursivas en los componentes de base de datos del documento de metadatos del escritor.  Se incluyen automáticamente en la copia de seguridad si se selecciona el componente de base de datos.

### <a name="differential-backuprestore"></a>Copias de seguridad y restauración diferenciales

Una operación de copia de seguridad diferencial respalda solo los datos que han cambiado desde la copia de seguridad completa de base más reciente. Una copia de seguridad diferencial solo contiene las partes de los archivos de base de datos que han cambiado. Para hacer este tipo de copia de seguridad, la aplicación de copia de seguridad o el solicitante necesitan información sobre la ubicación de los cambios en los archivos de base de datos, de modo que se pueda hacer una copia de seguridad de las secciones adecuadas de los archivos. Durante una operación de copia de seguridad diferencial, el objeto de escritura de SQL proporciona esta información en el formato especificado por "Información del archivo parcial de VSS". Esta información se puede usar para hacer una copia de seguridad solo de la parte modificada de los archivos de base de datos.

### <a name="backup"></a>Copia de seguridad

El solicitante puede emitir una copia de seguridad diferencial estableciendo la opción diferencial (**VSS_BT_DIFFERENTIAL**) en el documento Componentes de copias de seguridad (**IVssBackupComponents::SetBackupState**) al iniciar una operación de copia de seguridad con VSS.  El objeto de escritura de SQL pasará la información del archivo parcial (devuelta por SQL Server) a VSS.  El solicitante puede obtener esta información de archivo llamando a las API de VSS (**IVssComponent::GetPartialFile**). Esta información de archivo parcial permite que el solicitante elija solo los intervalos de bytes modificados para hacer una copia de seguridad de los archivos de base de datos.

Durante la fase de tareas de copia de seguridad previa, el objeto de escritura de SQL se asegurará de que exista una única base diferencial para cada base de datos seleccionada.

Durante el evento posterior a la instantánea, el objeto de escritura de SQL obtendrá la información del archivo parcial de SQL Server y la agregará al documento Componentes de copias de seguridad mediante la llamada a **IVssComponent::AddPartialFile**.  

  > [!NOTE]
  > El objeto de escritura de SQL solo admite una única línea base diferencial para copias de seguridad diferenciales. No se admiten línea de base múltiples.

### <a name="partial-file-information-format"></a>Formato de la información del archivo parcial

Para cada base de datos de la que se vaya a hacer una copia de seguridad durante una copia de seguridad diferencial, el objeto de escritura de SQL almacenará la información del archivo parcial para cada archivo de base de datos. Esta información la usan el solicitante o la aplicación de copia de seguridad para copiar solo las partes pertinentes del archivo en el medio de copia de seguridad durante la propia copia de seguridad de los archivos. Para obtener más información sobre el formato de esta información de archivo parcial, consulte la documentación del Servicio de instantáneas de volumen.

Un solicitante puede determinar estos archivos llamando a **IVssComponent::GetPartialFileCount** y **IVssComponent::GetPartialFile**.  **IVssComponent::GetPartialFile** devolverá una ruta de acceso y un nombre de archivo que apunten al archivo, así como una cadena de intervalos que indique qué debe respaldarse en el archivo.

Para obtener más detalles sobre la recuperación de información parcial de archivos, consulte la [documentación de VSS](/windows/win32/vss/volume-shadow-copy-service-overview).

### <a name="back-up-files"></a>Realización de una copia de seguridad de los archivos

Durante esta fase, la aplicación de copia de seguridad debe examinar los metadatos del escritor almacenados en el documento Componentes de copias de seguridad y hacer una copia de seguridad solo de las partes pertinentes de los archivos. (En el caso de los archivos de catálogo de texto completo, esta copia de seguridad debe hacerse en función de las marcas de tiempo de los archivos. Esto se describe más adelante en este documento).

Una copia de seguridad diferencial siempre se hará con respecto a la última copia de seguridad de base que exista para la base de datos.  En el momento de la restauración, SQL Server detectará copias de seguridad diferenciales y de base no coincidentes. Por lo tanto, es responsabilidad del administrador del sistema o de la aplicación de copia de seguridad asegurarse de que el diferencial es relativo a la copia de seguridad completa esperada.  Si algún procedimiento fuera de banda ha hecho otra copia de seguridad completa, es posible que la aplicación de copia de seguridad no pueda restaurar el diferencial, ya que no es "posee" la copia de seguridad de base.

Actualmente, si la información de intervalo de bytes (información de archivo parcial) es demasiado grande (más de 64 KB en tamaño de búfer), SQL Server producirá un error en el que se indicará al usuario que haga una copia de seguridad completa.

### <a name="interesting-cases-during-backup"></a>Casos interesantes durante la copia de seguridad

El archivo add/drop/shrink/growth/logical-rename/physical-rename hace casos interesantes en la copia de seguridad.

**Archivos agregados recientemente después de tomar la base**

Estos archivos se incluyen en la especificación parcial porque cada encabezado del archivo de base de datos SQL debe estar en la especificación parcial.  Además de la página de encabezado, todas las páginas asignadas deben incluirse en la especificación parcial.

**Archivos eliminados después de tomar la base**

Una vez tomada la base, se pudieron quitar los archivos de datos.  Estos archivos no se incluyen en el documento de metadatos del escritor durante la copia de seguridad diferencial.  Además, no habrá información parcial asociada al archivo eliminado.

**Archivos reducidos después de tomar la base**

La información parcial no se recopila de los archivos hasta que se han deshabilitado las reducciones de archivos en el servidor.  Esto garantiza que nunca se incluya ninguna información parcial correspondiente a la región reducida de un archivo de datos.  

**Archivos aumentados después de tomar la base**

Si el aumento tuvo lugar antes de que se recopilara la información parcial, esta debe haber incluido las páginas asignadas en la región aumentada.  Si el aumento tuvo lugar después de que se recopilara la información parcial, esta no incluye los cambios en la región aumentada.  En las secciones siguientes veremos que la puesta al día del registro restaurará estos cambios.

**Cambio del nombre lógico del archivo después de tomar la base**

Un cambio de nombre lógico del archivo no afecta a la copia de seguridad o restauración, ya que no se hace referencia al nombre lógico del archivo en ningún lugar del documento de metadatos del escritor o del documento Componentes de copias de seguridad.  (Consulte la sección Documento de metadatos del escritor: un ejemplo para obtener más detalles).

**Cambio del nombre físico del archivo después de tomar la base**

Un cambio de nombre físico del archivo de base de datos no surte efecto hasta que esta se reinicia.  Por lo tanto, la información de configuración de la base de datos o la información de la ruta de acceso del archivo en el búfer de información parcial todavía se basa en las rutas de acceso físicas anteriores, que son las únicas rutas válidas a esos archivos de base de datos en la instantánea.

### <a name="restore"></a>Restauración

Durante una restauración diferencial, los metadatos de copia de seguridad que el solicitante devuelve al objeto de escritura de SQL tienen la información del tipo de copia de seguridad.  Por lo tanto, no se necesita ningún tratamiento especial del objeto de escritura de SQL.  SQL Server determinará que se trata de una restauración diferencial de forma automática.  SQL Server controla dicha restauración diferencial de la misma manera que en una restauración diferencial nativa que no se realiza a través de VSS.

### <a name="prerestore-phase"></a>Fase previa a la restauración

Durante esta fase, SQL Server cambiará el tamaño de todos los archivos al adecuado, en función de los metadatos de archivo de la copia de seguridad diferencial.  Si el archivo aumenta, SQL Server reduce a cero la parte aumentada.  Si se debe crear un nuevo archivo (se ha creado después de tomar la base), SQL Server reduce a cero el nuevo archivo. También se cierran todos los identificadores de archivo para que la aplicación de copia de seguridad pueda sobrescribir los archivos con los datos restaurados del medio de copia de seguridad.

### <a name="restore-files"></a>Restauración de archivos

El cliente debe restaurar los archivos en función de la especificación del archivo parcial.  Los datos se deben restaurar en el mismo desplazamiento o intervalo del archivo de base de datos, tal como se indica en la especificación del archivo parcial almacenada en los metadatos del escritor.

El archivo de base de datos add/drop/growth/shrink/logical-rename/physical-rename vuelve a hacer casos interesantes en el momento de la restauración.

**Si se ha agregado un archivo de base de datos después de tomar la base completa**

Dichos archivos se deben haber creado previamente mediante SQL Server durante la fase de preparación de la restauración.  Debieron ampliarse al tamaño correcto y se debieron reducir a cero.  El cliente solo tiene que establecer los datos según la especificación parcial, que incluye todas las extensiones asignadas.

**Si se quitó un archivo de base de datos después de tomar la base completa**

La información parcial que ha proporcionado SQL Server no incluye información de seguimiento para dichas entregas de archivos.  SQL Server es responsable de detectar los archivos que se van a eliminar, comparando los metadatos de los archivos restaurados con los contenedores existentes y eliminándolos realmente.  Esto se hace antes de la restauración como paso de preparación.

**Si se ha aumentado un archivo de base de datos desde que se tomó la base completa**

Dichos archivos se deben haber ampliado al tamaño adecuado mediante SQL Server durante la fase de preparación de la restauración.  Asimismo, la región ampliada debe haberse reducido a cero mediante SQL Server.  Por lo tanto, el cliente puede establecer de forma segura los datos incluso en la región aumentada según la especificación parcial.  Si el archivo ha aumentado después de tomar la información parcial, los cambios en la región aumentada se restaurarán reproduciendo el registro del que se hizo una copia de seguridad junto con la copia de seguridad diferencial.

**Si se ha reducido un archivo de base de datos desde que se tomó la base completa**

SQL Server es responsable de truncar el archivo al tamaño requerido según los metadatos.  Esto se hace antes de la restauración como paso de preparación.

**Si se ha cambiado el nombre lógico de un archivo de base de datos desde que se tomó la base completa**

Esto no afectaría a la restauración, ya que el nombre lógico no aparece en el documento de metadatos del escritor o en el documento Componentes de copias de seguridad.  El cambio de nombre lógico se restaurará cuando el cliente aplique el cambio en el archivo de base de datos principal, que contiene la información del catálogo del sistema.

**Si se ha cambiado el nombre físico de un archivo de base de datos desde que se tomó la base completa.**

Si en el momento de la copia de seguridad diferencial el cambio de nombre no hubiera surtido efecto, el cliente seguirá restaurando los datos en la ubicación anterior.  Un reinicio de la base de datos posterior a la restauración hará que el cambio de nombre físico surta efecto.  Si en el momento de la copia de seguridad diferencial el cambio de nombre físico del archivo ya había surtido efecto, significa que se realizó una copia de seguridad de los datos parciales (si los hubiera) desde la nueva ruta de acceso física.  

### <a name="post-restore"></a>Posterior a la restauración

Durante los eventos posteriores a la restauración, el objeto de escritura de SQL realizará la operación de fase de puesta al día normal y la recuperación (si SetAdditionalRestores() se establece en "false") de la base de datos.

## <a name="differential-backuprestore-for-full-text-catalogs"></a>Copia de seguridad diferencial y restauración para catálogos de texto completo

Los catálogos de texto completo de SQL Server forman parte de los recursos de la base de datos de los que es necesario hacer una copia de seguridad o restauración, junto con el resto de los archivos de la base de datos.  Una copia de seguridad diferencial es una marca de tiempo basada en el catálogo de texto completo.  La restauración o copia de seguridad diferencial de VSS de SQL Server tiene una única copia de seguridad de base.  En otras palabras: no habrá diferentes bases para los distintos contenedores.  En el caso de la copia de seguridad del catálogo de texto completo de VSS, esto significa que para todos los contenedores de catálogos de texto completo, la copia de seguridad diferencial estará basada en una sola marca de tiempo, a diferencia de la copia de seguridad diferencial de SQL nativa, en la que hay una base de marca de tiempo por contenedor de catálogos de texto completo.

En VSS, esta marca de tiempo se expresa como una propiedad de todo el componente que se establece durante la copia de seguridad completa y se usa durante una copia de seguridad diferencial subsiguiente.  

### <a name="onidentify"></a>OnIdentify

En OnIdentify, el objeto de escritura de SQL llama a IVssCreateWriterMetadata::SetBackupSchema() para establecer el valor VSS_BS_TIMESTAMPED.  Esto indica a la aplicación de copia de seguridad que el objeto de escritura de SQL posee la administración de la base diferencial.

### <a name="setting-the-base-timestamp"></a>Establecimiento de la marca de tiempo de base

La marca de tiempo de base se establece durante una copia de seguridad completa.  En **OnPostSnapshot()** , el escritor invoca a **IVssComponent::SetBackupStamp()** para almacenar la marca de tiempo con el componente en el documento de copia de seguridad.

### <a name="differential-backup"></a>Copia de seguridad diferencial

La aplicación de copia de seguridad recuperará esta marca de tiempo de la copia de seguridad completa de base y hará que dicha marca esté disponible para el escritor llamando a **IVssComponent::GetBackupStamp()** con el fin de recuperar la marca de base de la copia de seguridad de base anterior.  A continuación, hará que esté disponible para el escritor llamando a **IVssBackupComponent::SetPreviousBackupStamp()** .  Seguidamente, el escritor recuperará la marca de tiempo llamando a **IVssComponent::GetPreviousBackupStamp()** y la traducirá en otra que se usará para **IVssComponent::AddDifferencedFilesByLastModifyTime()** .  

**Responsabilidad de la aplicación de copia de seguridad durante la copia de seguridad diferencial**: durante una copia de seguridad diferencial, la aplicación de copia de seguridad es responsable de:

- Hacer una copia de seguridad de cualquier archivo (todo el archivo) cuya marca de tiempo de última modificación sea mayor que la marca de tiempo especificada en la "hora de última modificación" del conjunto de archivos en el componente.
- Realizar un seguimiento y detectar los archivos eliminados.  

**Responsabilidades de la aplicación de copia de seguridad durante una restauración diferencial**: durante una restauración diferencial, la aplicación de copia de seguridad es responsable de:

- Restaurar todos los archivos de los que se ha hecho una copia de seguridad, ya sea creando un archivo nuevo si aún no existe o sobrescribiendo uno existente.
- Aumentar el tamaño del archivo antes de establecer el contenido si el archivo restaurado es mayor que el archivo existente.
- Truncar el archivo al mismo tamaño que el del archivo restaurado si este es más pequeño que el existente.
- Eliminar todos los archivos que deben eliminarse; es decir, aquellos archivos que no deben existir en el momento de la copia de seguridad diferencial.

### <a name="copy-only-backup"></a>Copia de seguridad de solo copia

A veces es necesario hacer una copia de seguridad pensada para un fin especial. Por ejemplo, tal vez necesite hacer una copia de una base de datos con fines de prueba.  Esta copia de seguridad no debe afectar a los procedimientos de copias de seguridad y restauración generales de la base de datos. Con la opción COPY_ONLY se especifica que la copia de seguridad se hace "fuera de banda" y no debe afectar a la secuencia normal de copias de seguridad. El objeto de escritura de SQL admite el tipo de copia de seguridad de "solo copia" con instancias de SQL Server.

Durante la fase de detección de copias de seguridad, el objeto de escritura de SQL indicará su capacidad para hacer una copia de seguridad de solo copia mediante el establecimiento de la opción de esquema de copia de seguridad compatible VSS_BS_COPY mediante la llamada a **IVssCreateWriterMetadata::SetBackupSchema**. El solicitante puede establecer el tipo de copia de seguridad como de solo copia. Para ello, establece la opción VSS_BACKUP_TYPE como VSS_BT_COPY con la llamada a **IVssBackupComponents::SetBackupState**.

Al seleccionar una copia de seguridad de solo copia, se da por supuesto que los archivos del disco se copiarán en un medio de copia de seguridad (por el solicitante), independientemente del estado del historial de copias de seguridad de cada archivo. SQL Server no actualizará el historial de copias de seguridad. Este tipo de copia de seguridad no constituirá una copia de seguridad de base para otras operaciones de copia de seguridad diferenciales, del mismo modo que tampoco alterará el historial de las copias de seguridad diferenciales anteriores.

### <a name="restore-with-move"></a>Restauración con desplazamiento

VSS permite que la aplicación de copia de seguridad (solicitante) especifique un nuevo destino de restauración mediante la llamada a **IVssComponent::SetNewTarget**.  Tanto en PreRestore() como en PostRestore(), el objeto de escritura de SQL comprueba si existe por lo menos un nuevo destino especificado. Es responsabilidad de la aplicación de copia de seguridad copiar físicamente los archivos en la nueva ubicación durante el tiempo real de copia y restauración de los archivos.

La aplicación de copia de seguridad solo puede especificar nuevos destinos para la ruta de acceso física, pero no la especificación del archivo.  Por ejemplo, para un archivo de base de datos ubicado en c:\data\test.mdf, no se puede cambiar el nombre de archivo real, test.mdf.  Solo se puede cambiar la ruta de acceso c:\data.  Para un contenedor de catálogos de texto completo que se encuentra en c:\ftdata\foo, dado que la especificación del archivo en VSS es "*" y la especificación de la ruta de acceso en VSS es c:\ftdata\foo, se puede cambiar la ruta de acceso completa.  

### <a name="database-rename"></a>Cambio de nombre de una base de datos

Es posible que un solicitante necesite restaurar una base de datos de SQL con un nombre nuevo, especialmente si la base de datos se va a restaurar en paralelo con la base de datos original.  El solicitante puede especificar esta opción durante la operación de restauración. Para ello, establece una opción de restauración personalizada como "Nuevo nombre de componente" = <"Nuevo nombre"> mediante la llamada de VSS a **IVssBackupComponents::SetRestoreOptions()** , en el parámetro wszRestoreOptions.

El objeto de escritura de SQL tomará todo el contenido del nuevo valor del nombre del componente y lo usará como el nuevo nombre de la base de datos restaurada. Si no se especifica ninguna opción, SQL restaurará la base de datos con su nombre original (nombre del componente).

  > [!NOTE]
  > El objeto de escritura de SQL no admite actualmente "Cambiar el nombre entre instancias" para trasladar una base de datos a una nueva instancia.

### <a name="auto-recovered-snapshots"></a>Instantáneas recuperadas automáticamente

Normalmente, una instantánea de una base de datos de SQL Server obtenida mediante el marco de VSS se encuentra en un estado no recuperado. No se puede tener acceso de forma segura a los datos de la instantánea antes de pasar a través de la fase de recuperación para revertir las transacciones en curso y colocar la base de datos en un estado coherente. Dado que la instantánea se encuentra en estado de solo lectura, no se puede recuperar mediante el proceso normal de conectar la base de datos.

Es posible recuperar las instantáneas de forma automática como parte del proceso de creación de instantáneas. Como parte del documento de metadatos del escritor, el objeto de escritura de SQL especificará la marca de componente "VSS_CF_APP_ROLLBACK_RECOVERY" para indicar que es necesario realizar una recuperación para la base de datos en la instantánea antes de que se pueda tener acceso a la base de datos al especificar el conjunto de instantáneas. El solicitante puede indicar que la instantánea debe ser una instantánea de reversión de la aplicación (es decir, todos los archivos de base de datos de una instantánea están diseñados para estar en un estado coherente para el uso de la aplicación) o una instantánea de copia de seguridad (una instantánea que se usa para hacer una copia de seguridad de los datos que se restaurarán más adelante en caso de un error del sistema).

El solicitante debe establecer VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY para indicar que se está haciendo una copia de seguridad de este componente para un propósito que no es de copia de seguridad.  A continuación, VSS correlacionará la instrucción VSS_CF_APP_ROLLBACK_RECOVERY que el objeto de escritura de SQL ha especificado en el componente seleccionado con VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY y determinará que se está produciendo la recuperación automática.  VSS hará que se pueda escribir en la instantánea durante un período de tiempo limitado y agregará automáticamente el elemento VSS_VOLSNAP_ATTR_AUTORECOVERY al contexto de la instantánea.

En el caso de SQL Server, la recuperación automática solo debe aplicarse a las instantáneas de reversión de aplicaciones, no a las instantáneas de copia de seguridad. En el caso de las instantáneas de reversión de aplicaciones, un proceso de autorrecuperación se inicia mediante el objeto de escritura de SQL durante el evento posterior a la instantánea. Este proceso realizará lo siguiente para cada una de las bases de datos de SQL Server seleccionadas explícitamente (por el solicitante) en el conjunto de instantáneas:

- Conectar la base de datos de instantánea a la instancia de SQL Server original (es decir, la instancia a la que se adjunta la base de datos original).
- Recuperar la base de datos (esto sucede como parte de la operación "adjuntar").
- Reducir el tamaño de los archivos de registro.

    Esto reduce la cantidad de "copias por escritura" innecesarias que debe realizar el marco de VSS, si el proveedor de VSS es un "proveedor de software". La reducción de los archivos de registro es el comportamiento predeterminado. Se puede deshabilitar si se establece el valor de la siguiente clave del registro en 1.
    
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SQLWriter\
    Settings\DisableLogShrink
    
    Esto puede resultar útil en escenarios en los que la instantánea se puede usar para exportar datos de una página específica (en un momento específico) desde el registro para solucionar un problema en la base de datos en línea.

- Separe la base de datos.

Ahora tenemos una instantánea coherente y recuperada que se puede adjuntar para realizar consultas.

### <a name="multi-database-transactions"></a>Transacciones de varias bases de datos

En algunos casos, las bases de datos de instantáneas pueden contener algunas transacciones de varias bases de datos en curso. Durante la operación de recuperación, el objeto de escritura de SQL conectará la base de datos en las instantáneas con la opción de anulación presunta. Esto revertirá cualquier transacción de varias bases de datos que no se haya confirmado todavía, incluidas las transacciones que estén en un estado preparado para confirmar. Esto puede dar lugar a incoherencias entre las bases de datos del conjunto de instantáneas. Por ejemplo, considere dos bases de datos A y B. Hay una transacción distribuida entre estas dos bases de datos y esta transacción está en estado "Confirmado" en la base de datos A y en estado "Preparado para confirmar" en la base de datos B. Como parte del proceso de autorrecuperación, esta transacción se confirmará en la base de datos A y se revertirá en la base de datos B, lo cual puede originar algunas incoherencias en el conjunto de instantáneas.

El Coordinador de transacciones distribuidas de Microsoft (MS DTC), que se lanzará en el plazo de Longhorn mediante el marco de VSS, solucionará este problema de incoherencias para las transacciones que abarcan las bases de datos de las instancias de SQL Server. La siguiente versión de SQL Server corregirá estas incoherencias para las transacciones que abarcan las bases de datos dentro de una instancia de SQL Server.

### <a name="security-implications-for-autorecovered-snapshots"></a>Implicaciones de seguridad para las instantáneas recuperadas automáticamente

En el caso de las instantáneas de VSS, después de la recuperación automática, los archivos se protegerán mediante listas de control de acceso (ACL) para permitir el acceso solo al grupo integrado especial al que pertenece la cuenta de SQL Server.  Esto implica que el miembro del administrador del cuadro o de ese grupo especial podrá conectar la base de datos. El cliente que solicita adjuntar los archivos de base de datos en una instantánea debe ser miembro de Builtin/Administrators o de la cuenta de SQL Server.

### <a name="special-cases"></a>Casos especiales

En esta sección se describen algunos de los casos especiales que se producen durante las operaciones de copia de seguridad y restauración basadas en el objeto de escritura de SQL.

#### <a name="autoclose-databases"></a>Bases de datos cerradas automáticamente

En el caso de las copias de seguridad no basadas en componentes, se realiza el cierre automático de las bases de datos, al comprobar las condiciones incompletas, pero las bases de datos cerradas automáticamente no se inmovilizan explícitamente durante las operaciones de copia de seguridad.

El escenario esperado aquí es que pueden existir muchas bases de datos cerradas, y queremos minimizar el costo de la instantánea.  Las bases de datos cerradas automáticamente se usan normalmente en configuraciones de bajo perfil en las que los recursos son escasos.

#### <a name="file-list"></a>Lista de archivos

La lista de archivos para cada base de datos se determina durante un paso de enumeración antes del evento de preparación para la copia de seguridad.  Si la lista de archivos de base de datos cambia entre la enumeración y la inmovilización, la base de datos podría quedar incompleta, a menos que la aplicación vuelva a comprobar la lista de archivos. No esperamos que esto sea un problema, pero es algo que los proveedores deben tener en cuenta.

#### <a name="stopped-instances"></a>Instancias detenidas

Si una instancia de SQL Server no se está ejecutando en el momento en el que se produce el paso de enumeración, no se puede seleccionar ninguna de las bases de datos para esas instancias.

Si una instancia se detiene en el intervalo entre la enumeración y el evento de preparación para la copia de seguridad, se omiten todas las bases de datos de la instancia detenida.

#### <a name="system-and-user-databases"></a>Bases de datos del usuario y del sistema

Las bases de datos del sistema de SQL Server incluyen las bases de datos maestras, de modelo y msdb que se incluyen e instalan como parte de SQL Server. En esta sección se describe la forma en la que se controlan estas bases de datos en un proceso de copia de seguridad de instantáneas de VSS.

### <a name="system-databases"></a>Bases de datos del sistema

La base de datos maestra solo se puede restaurar deteniendo la instancia, reemplazando los archivos de base de datos (mediante la aplicación de copia de seguridad) y reiniciando la instancia. No es posible realizar puestas al día.

El objeto de escritura de SQL admite la restauración de las bases de datos de modelo y msdb en línea, sin cerrar la instancia.


#### <a name="simple-recovery-model-user-databases"></a>Bases de datos de usuario con el modelo de recuperación simple

Si la base de datos maestra se restaura junto con las bases de datos de usuario que usan el modelo de recuperación simple, las bases de datos de usuario se pueden restaurar con la misma técnica que la base de datos maestra; es decir, con la instancia apagada, basta con copiar o montar los volúmenes.  Cuando se inicia la instancia de SQL, se recupera todo.

#### <a name="rolling-forward-user-databases"></a>Puesta al día de las bases de datos de usuario

Si las bases de datos de usuario se recuperan y se ponen al día, junto con la recuperación de base de datos maestra, la instancia no debe iniciar ni recuperar las bases de datos maestras y de usuario juntas.

El procedimiento es el siguiente:

1. Asegúrese de que la instancia de SQL Server está detenida.
1. Realice la restauración en dos fases.
    1. Restaure las bases de datos del sistema y las bases de datos de usuario que se deben recuperar al mismo tiempo (es decir, las bases de datos de usuario en modo de recuperación simple) mediante la copia de archivos y montaje del volumen a través de VSS.
        1. Si las bases de datos de usuario que se van a poner al día no se encuentran en el mismo volumen que las del sistema, ese volumen no debe recuperarse en este momento. Este escenario requiere planeación antes de hacer una copia de seguridad.
        1. Si las bases de datos de usuario se encuentran en el mismo volumen que las del sistema, las bases de datos de usuario deben estar ocultas en SQL Server.
    1. Inicie la instancia de SQL Server mediante el parámetro -f.  (Cuando se usa la opción de inicio -f, solo se puede restaurar la base de datos maestra).
        1. Emita una instrucción ALTER DATABASE \<x> SET OFFLINE para cada base de datos que se vaya a poner al día.  (Desconectar la base de datos es una alternativa).
        1. Detenga la instancia de SQL Server.
        1. Inicie la instancia de SQL Server (los archivos de las bases de datos de usuario que se van a poner al día no son visibles para SQL Server).

Use VSS para restaurar las bases de datos de usuario con NORECOVERY, tal como se describe en "Restauración completa con puesta al día".

## <a name="appendix"></a>Apéndice

### <a name="writer-metadata-document--an-example"></a>Documento de metadatos del escritor:  un ejemplo

Base de datos de ejemplo denominada DB1, que pertenece a la instancia de SQL Server Instance1 en el equipo Server1 y que contiene los siguientes archivos de registro y de base de datos:

- Archivo de base de datos denominado "primary" almacenado en c:\db\DB1.mdf
- Archivo de base de datos denominado "secondary" almacenado en c:\db\DB1.ndf
- Archivo de base de datos denominado "log" almacenado en c:\db\DB1.ldf
- Catálogo de texto completo denominado "foo" almacenado en el directorio c:\db\ftdata\foo
- Catálogo de texto completo denominado "bar" almacenado en el directorio c:\db\ftdata\bar

A continuación se indican los metadatos del escritor de la base de datos:

**Componente de grupo de archivos de nivel de base de datos**

- ComponentType: VSS_CT_FILEGROUP
- LogicalPath: "Server1\Instance1"
- ComponentName: "DB1"
- Caption: NULL
- pbIcon: NULL
- cbIcon: 0
- bRestoreMetadata: FALSE
- NotifyOnBackupComplete: TRUE
- Selectable: TRUE
- SelectableForRestore: TRUE
- ComponentFlags: VSS_CF_APP_ROLLBACK_RECOVERY

**Archivo de grupo de archivos**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.mdf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED
- FilegroupFile
- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.ndf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Archivo de grupo de archivos**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.ldf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Archivo de grupo de archivos:**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db\ftdata\foo"
- FileSpec: "*"
- Recursive: TRUE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Archivo de grupo de archivos:**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db\ftdata\bar"
- FileSpec: "*"
- Recursive: TRUE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

Si la instancia de servidor es la predeterminada en el equipo, la ruta de acceso lógica se convierte en una parte: "Server1".


    
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Copias de seguridad de solo copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [Copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [Guía de arquitectura y administración de registros de transacciones de SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
