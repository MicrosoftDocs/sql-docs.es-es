---
title: Solución de problemas de Clústeres de macrodatos (BDC) con cuadernos de Jupyter y Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Solución de problemas de BDC con cuadernos de Jupyter y Azure Data Studio en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 550c56a7ceb82b1215e6e921adb68a1438e87464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048245"
---
# <a name="troubleshooting-big-data-clusters-bdc-with-notebooks"></a>Solución de problemas de Clústeres de macrodatos (BDC) con cuadernos

Esta página es un índice de los cuadernos para Clústeres de macrodatos de SQL Server. Estos cuadernos ejecutables (.ipynb) están diseñados para SQL Server 2019 como ayuda para la solución de problemas de Clústeres de macrodatos.

Cada cuaderno está diseñado para comprobar sus propias dependencias. Una operación de tipo **Ejecutar todas las celdas** se realizará correctamente o generará una excepción con una sugerencia con hipervínculo a otro cuaderno para resolver la dependencia que falta. Siga el hipervínculo de la sugerencia al cuaderno siguiente, presione **Ejecutar todas las celdas** y, cuando el proceso finalice correctamente, vuelva al cuaderno original y presione **Ejecutar todas las celdas**.

Una vez que se instalan todas las dependencias y que la operación **Ejecutar todas las celdas** produce un error, cada cuaderno analizará los resultados y, siempre que sea posible, generará una sugerencia con hipervínculo a otro cuaderno para ayudarlo a resolver el problema.


## <a name="troubleshooting-big-data-cluster-bdc"></a>Solución de problemas del Clúster de macrodatos (BDC)

Esta sección contiene un conjunto de cuadernos para obtener registros de un Clúster de macrodatos (BDC) de SQL Server.

|Nombre |Descripción |
|---|---|---|---|
|TSG100: solucionador de problemas del clúster de macrodatos|Información general de todos los cuadernos disponibles sobre la solución de problemas de un Clúster de macrodatos (BDC) y de cuándo usarlos  |
|TSG101: solucionador de problemas de SQL Server|Información general de todos los cuadernos disponibles sobre la solución de problemas de SQL Server y de cuándo usarlos  |
|TSG102: solucionador de problemas de HDFS|Información general de todos los cuadernos disponibles sobre la solución de problemas de HDFS y de cuándo usarlos  |
|TSG103: solucionador de problemas de Spark|Información general de todos los cuadernos disponibles sobre la solución de problemas de Spark y de cuándo usarlos  |
|TSG104: control del solucionador de problemas|Información general de todos los cuadernos disponibles sobre la solución de problemas del controlador y de cuándo usarlos  |
|TSG105: solucionador de problemas de puerta de enlace|Información general de todos los cuadernos disponibles sobre la solución de problemas de la puerta de enlace de Knox y de cuándo usarlos  |
|TSG106: solucionador de problemas de aplicaciones|Información general de todos los cuadernos disponibles sobre la solución de problemas de App-Deploy y de cuándo usarlos  |



## <a name="diagnose-issues-from-big-data-clusters-bdc"></a>Diagnóstico de problemas de Clústeres de macrodatos (BDC)

Un conjunto de cuadernos para diagnosticar situaciones y estados con un Clúster de macrodatos.

|Nombre |Descripción |
|---|---|---|---|
|TSG002: CrashLoopBackoff|Este TSG se conectará a cada contenedor cuyo último intento de entrar en un estado "En ejecución" dio error y obtendrá los registros del contenedor actual y anterior. Esto resulta útil para depurar los problemas de CrashLoopBackOff que se indican en el comando kubectl get pods.|
|TSG025: explorador de FSM, estado de FSM del controlador de consultas|Use este cuaderno para conectarse a la base de datos de controlador y examinar el estado de la máquina de estado finito (FSM). Use este cuaderno para enumerar las máquinas en estado activo e identificar los flujos de trabajo bloqueados.|
|TSG026: conexión al nodo del grupo de datos (para ejecutar T-SQL)|Use este cuaderno para conectarse al nodo del grupo de datos (para ejecutar T-SQL).|
|TSG027: observación de la implementación del clúster|Use este cuaderno para observar la implementación del clúster; proporciona orientación para solucionar problemas de creación del clúster de macrodatos de SQL Server, suelen ser útiles los siguientes comandos a la hora de identificar las causas subyacentes. |
|TSG029: búsqueda de volcados de memoria en el clúster|Use este cuaderno para buscar volcados de memoria principales y minivolcados de procesos como SQL Server o el controlador en un clúster de macrodatos.|
|TSG032: uso de la CPU y de la memoria para todos los contenedores|Use este cuaderno para comprobar el uso de la CPU y de la memoria para todos los contenedores.|
|TSG037: Determinación de la réplica principal de hospedaje del pod del grupo maestro|Use este cuaderno para determinar la réplica principal de hospedaje del pod del grupo maestro para el clúster de macrodatos cuando está habilitada la alta disponibilidad del grupo maestro.|
|TSG044: ejecución de sqlcmd en el contenedor del grupo maestro|Use este cuaderno para conectarse a un nodo del grupo maestro directamente mediante T-SQL.|
|TSG055: Temporización de Curl a sparkhead|Use este cuaderno para diagnosticar el paso para comprender cuál es el tiempo de respuesta de Curl desde el pod del controlador hasta el pod de sparkhead.|
|TSG060: espacio en disco del volumen persistente para todas las PVC del BDC|Use este cuaderno para conectarse a cada contenedor y obtenga el espacio en disco usado o disponible para cada volumen persistente (PV) asignado a cada notificación de volúmenes persistentes (PVC) de un clúster de macrodatos (BDC).|
|TSG078: ¿es correcto el clúster?|Use este cuaderno para comprobar si el estado del Clúster de macrodatos (BDC) es correcto.|
|TSG079: Generación del volcado principal del controlador|Use este cuaderno para generar el volcado principal del controlador.|
|TSG086: Ejecución en la parte superior de todos los contenedores|Use este bloc de notas para la ejecución en la parte superior de todos los contenedores.|
|TSG087: Uso de la CLI hadoop fs en el pod namenode|Use este cuaderno para usar la CLI de hadoop fs en el pod de namenode.|
|TSG108: Visualización de la asignación de configuración de actualización del controlador|Use este cuaderno para solucionar el error al ejecutar una actualización de Clúster de macrodatos mediante **azdata bdc upgrade**.|
|TSG112: Comprobaciones previas a la implementación de Active Directory|Use este cuaderno para validar que una configuración de Clúster de macrodatos (BDC) es válida para una implementación de Active Directory (AD).|
|TSG115: Traductor de registro de seguridad de SQL Server en Linux|Use este cuaderno para analizar los registros generados por los registradores secuirty.ldap y security.kerberos para SQL Server en Linux. Para habilitar estos registradores, coloque las líneas siguientes en /var/opt/mssql/logger.ini en el equipo que ejecuta SQL Server en Linux. Tenga en cuenta que este archivo distingue mayúsculas de minúsculas.|
|TSG116: Traductor de registro de compatibilidad de seguridad de BDC de SQL|Use este cuaderno para analizar los registros generados por el servicio de soporte técnico de seguridad en BDC de SQL. Para obtener los registros, se copiarán los registros de depuración del clúster y se extraerán. Siga los pasos que se indican a continuación: ejecute "azdata bdc debug copy-logs -n <namespace> *". Esto creará varios archivos .tar.gz: extraiga el contenido de los registros de depuración-* <namespace>-<date>-<time>.tar.gz. Busque el registro de compatibilidad de seguridad almacenado en ./<namespace>/control-<…>/security-support/supervisol/log/secsupp-stderr---<…>.log.|
|TSG119: Comprobaciones posteriores a la implementación de Active Directory|Este cuaderno está diseñado para validar la configuración de BDC después de una implementación de AD. Comprobará la existencia de entradas DNS para todos los puntos de conexión con un atributo dnsName y estas entradas DNS deben ser registros de host, no alias (es decir, registros A, no registros CNAME). También la existencia de cuentas AD conocidas y si están habilitadas o no y la existencia de los SPN esperados.|






## <a name="repair-issues-from-big-data-clusters-bdc"></a>Reparación de problemas de Clústeres de macrodatos (BDC)

Un conjunto de cuadernos para reparar situaciones y estados conocidos de un Clúster de macrodatos de SQL Server.

|Nombre |Descripción |
|---|---|---|---|
|TSG005: bucle de reenvío detectado|Use este cuaderno para abordar el bucle de reenvío detectado, puesto que la utilidad dnsmasq puede colocar un bucle invertido local en resolv.conf, lo que puede hacer que los pods del controlador pasen a un estado CrashLoopBackOff durante la implementación inicial del clúster: https://askubuntu.com/questions/627899/nameserver-127-0-1-1-in-resolv-conf-wont-go-away|
|TSG011: reinicio del servidor de sparkhistory|Use este cuaderno para reiniciar el servidor de sparkhistory, ya que el proceso de Java de sparkhistory puede dejar de responder durante el inicio. Si se reinicia el servidor de sparkhistory (supervisorctl restart sparkhistory), podría resolverse este problema.|
|TSG018: terminación del proceso sqlservr en el grupo maestro| Use este cuaderno cuando T-SQL SHUTDOWN no vuelva a recorrer correctamente el proceso ./sqlservr. Use este cuaderno para terminar el proceso principal sqlservr que se reiniciará automáticamente mediante el proceso de front-end ./sqlservr.|
|TSG024: Namenode está en modo seguro| Use este cuaderno cuando HDFS entre en modo seguro. Por ejemplo, si se vuelven a recorrer demasiados pods demasiado rápido en el bloque de almacenamiento, el modo seguro puede habilitarse automáticamente.|
|TSG028: reinicio de nodemanager en todos los nodos del bloque de almacenamiento| Use este cuaderno cuando necesite reiniciar el administrador de nodos en todos los nodos del grupo de almacenamiento.|
|TSG038: error en la creación del BDC debido a que falta la clave del documento| Use este cuaderno cuando haya errores en la creación del BDC debido a que falta la clave del documento.|
|TSG039: Nombre de objeto no válido "role_permissions"| Use este cuaderno cuando tenga un problema de objeto no válido debido al permiso de rol en Knox gateway.log.|
|TSG040: no se pudieron obtener los nombres de archivo del controlador con error| Use este cuaderno cuando aparece un error 504 Tiempo de espera agotado para la puerta de enlace al obtener los nombres de archivo del controlador.|
|TSG041: no se puede crear un contexto de E/S asincrónico (aumente sysctl fs.aio-max-nr)| Use este cuaderno cuando no se pueda crear un contexto de E/S asincrónico (aumente sysctl fs.aio-max-nr).|
|TSG045: número máximo de discos de datos que se pueden adjuntar a una VM de este tamaño (AKS)| Use este cuaderno cuando se permita conectar un número máximo de discos de datos a una máquina virtual de este tamaño (AKS).|
|TSG047: ConfigException: Solo se esperaba un objeto con nombre| Use este cuaderno cuando tenga ConfigException, que espera solo un objeto con nombre.|
|TSG048: la implementación se bloquea al esperar que el pod del controlador esté activo| Use este cuaderno cuando la implementación se bloquee al esperar que el pod del controlador esté activo.|
|TSG050: la creación del clúster no responde e indica que el tiempo de espera se ha agotado en espera de que los volúmenes se adjunten o se monten para el pod| Use este cuaderno cuando la creación del clúster no responde e indica que el tiempo de espera se ha agotado en espera de que los volúmenes se adjunten o se monten para el pod.|
|TSG052: error al intentar obtener el DNS de master-svc y futuro reintento| Use este cuaderno cuando la creación del clúster no responde e indica que el tiempo de espera se ha agotado en espera de que los volúmenes se adjunten o se monten para el pod.|
|TSG057: No se pudo iniciar el servicio del controlador .System.TimeoutException| Use este cuaderno cuando inicie el servicio del controlador y obtenga System.TimeoutException.|
|TSG067: no se pudo completar la instalación de kube config| Use este cuaderno cuando se produzca un error al instalar kubeconfig.|
|TSG074: eliminación de implementaciones de aplicaciones| Use este cuaderno cuando tenga problemas para eliminar aplicaciones en el clúster de BDC.|
|TSG075: FailedCreatePodSandBox debido a que el CNI NetworkPlugin no pudo configurar el pod| Use este cuaderno cuando obtenga la excepción FailedCreatePodSandBox porque NetworkPlugin cni no pudo configurar el pod.|
|TSG080: eliminación de sesiones de Spark con azdata| Use este cuaderno cuando obtenga problemas al eliminar sesiones de Spark.|
|TSG109: Establecimiento de los tiempos de espera de actualización| Use este cuaderno cuando tenga problemas con la actualización de BDC.|
|TSG110: Azdata devuelve ApiError| Use este cuaderno cuando Azdata devuelva ApiError.|

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md) para obtener más información sobre los clústeres de macrodatos.
