---
title: API de extensibilidad
description: Obtenga información sobre las API de extensibilidad de Azure Data Studio, que habilitan extensiones para interactuar con otros elementos de Azure Data Studio (como el Explorador de objetos).
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: d795d96d47fa7f07744c492d8639cdecdc2e23fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052161"
---
# <a name="azure-data-studio-extensibility-apis"></a>API de extensibilidad de Azure Data Studio

Azure Data Studio proporciona una API que las extensiones pueden usar para interactuar con otros elementos de Azure Data Studio, como el Explorador de objetos. Estas API están disponibles en el archivo [`src/sql/azdata.d.ts`](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.d.ts) y se describen a continuación.

## <a name="connection-management"></a>Administración de las conexiones
`azdata.connection`

### <a name="top-level-functions"></a>Funciones de nivel superior

- `getCurrentConnection(): Thenable<azdata.connection.Connection>` Obtiene la conexión actual en función del editor activo o de la selección del Explorador de objetos.

- `getActiveConnections(): Thenable<azdata.connection.Connection[]>` Obtiene una lista de todas las conexiones del usuario que están activas. Devuelve una lista vacía si no hay ninguna conexión de este tipo.

- `getCredentials(connectionId: string): Thenable<{ [name: string]: string }>` Obtiene un diccionario que contiene las credenciales asociadas a una conexión. De lo contrario, se devolverán como parte del diccionario de opciones en un objeto `azdata.connection.Connection`, pero se eliminarán de ese objeto. 

### `Connection`
- `options: { [name: string]: string }` El diccionario de las opciones de conexión
- `providerName: string` El nombre del proveedor de conexión (por ejemplo, "MSSQL")
- `connectionId: string` El identificador único para la conexión

### <a name="example-code"></a>Código de ejemplo
```
> let connection = azdata.connection.getCurrentConnection();
connection: {
    providerName: 'MSSQL',
    connectionId: 'd97bb63a-466e-4ef0-ab6f-00cd44721dcc',
    options: {
        server: 'mairvine-sql-server',
        user: 'sa',
        authenticationType: 'sqlLogin',
        ...
    },
    ...
}
> let credentials = azdata.connection.getCredentials(connection.connectionId);
credentials: {
    password: 'abc123'
}

```

## <a name="object-explorer"></a>Explorador de objetos

`azdata.objectexplorer`


### <a name="top-level-functions"></a>Funciones de nivel superior
- `getNode(connectionId: string, nodePath?: string): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Obtiene un nodo del Explorador de objetos correspondiente a la conexión y ruta de acceso especificadas. Si no se especifica ninguna ruta de acceso, devuelve el nodo de nivel superior para la conexión especificada. Si no hay ningún nodo en la ruta de acceso especificada, devuelve `undefined`. Nota: El `nodePath` de un objeto se genera mediante el back-end del servicio de herramientas de SQL y es difícil de construir a mano. Las futuras mejoras de la API permitirán obtener nodos basados en los metadatos que se proporcionen sobre el nodo, como el nombre, el tipo y el esquema.

- `getActiveConnectionNodes(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Obtiene todos los nodos de conexión del Explorador de objetos activos.

- `findNodes(connectionId: string, type: string, schema: string, name: string, database: string, parentObjectNames: string[]): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Busca todos los nodos del Explorador de objetos que coinciden con los metadatos especificados. Los argumentos `schema`, `database` y `parentObjectNames` deben ser `undefined` cuando no son aplicables. `parentObjectNames` es una lista de objetos primarios que no son de base de datos, del nivel más alto al más bajo en el Explorador de objetos, en el que se encuentra el objeto deseado. Por ejemplo, al buscar una columna "column1" que pertenezca a una tabla "schema1.table1" y la base de datos "database1" con el identificador de conexión `connectionId`, llame a `findNodes(connectionId, 'Column', undefined, 'column1', 'database1', ['schema1.table1'])`. Consulte también la [lista de tipos que Azure Data Studio admite de forma predeterminada](https://github.com/Microsoft/azuredatastudio/wiki/Object-Explorer-types-supported-by-FindNodes-API) para esta llamada API.

### <a name="objectexplorernode"></a>ObjectExplorerNode
- `connectionId: string` El identificador de la conexión en la que existe el nodo

- `nodePath: string` La ruta de acceso del nodo, tal y como se usa para una llamada a la función `getNode`

- `nodeType: string` Una cadena que representa el tipo de nodo

- `nodeSubType: string` Una cadena que representa el subtipo de nodo

- `nodeStatus: string` Una cadena que representa el estado del nodo

- `label: string` La etiqueta del nodo tal y como aparece en el Explorador de objetos

- `isLeaf: boolean` Si el nodo es un nodo hoja y, por tanto, no tiene ningún elemento secundario

- `metadata: azdata.ObjectMetadata` Metadatos que describen el objeto representado por este nodo

- `errorMessage: string` Mensaje que se muestra si el nodo está en un estado de error

- `isExpanded(): Thenable<boolean>` Si el nodo está expandido actualmente en el Explorador de objetos

- `setExpandedState(expandedState: vscode.TreeItemCollapsibleState): Thenable<void>` Establece si el nodo está expandido o contraído. Si el estado se establece en Ninguno, no se cambiará el nodo.

- `setSelected(selected: boolean, clearOtherSelections?: boolean): Thenable<void>` Establece si el nodo está seleccionado. Si `clearOtherSelections` es true, borre las demás selecciones al crear la nueva selección. Si es false, deje las selecciones existentes. `clearOtherSelections` tiene como valor predeterminado true cuando `selected` es true y false cuando `selected` es false.

- `getChildren(): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Obtiene todos los nodos secundarios de este nodo. Devuelve una lista vacía si no hay elementos secundarios.

- `getParent(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Obtiene el nodo primario de este nodo. Devuelve undefined si no hay ningún primario.

### <a name="example-code"></a>Código de ejemplo

```cs
private async interactWithOENode(selectedNode: azdata.objectexplorer.ObjectExplorerNode): Promise<void> {
    let choices = ['Expand', 'Collapse', 'Select', 'Select (multi)', 'Deselect', 'Deselect (multi)'];
    if (selectedNode.isLeaf) {
        choices[0] += ' (is leaf)';
        choices[1] += ' (is leaf)';
    } else {
        let expanded = await selectedNode.isExpanded();
        if (expanded) {
            choices[0] += ' (is expanded)';
        } else {
            choices[1] += ' (is collapsed)';
        }
    }
    let parent = await selectedNode.getParent();
    if (parent) {
        choices.push('Get Parent');
    }
    let children = await selectedNode.getChildren();
    children.forEach(child => choices.push(child.label));
    let choice = await vscode.window.showQuickPick(choices);
    let nextNode: azdata.objectexplorer.ObjectExplorerNode = undefined;
    if (choice === choices[0]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Expanded);
    } else if (choice === choices[1]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Collapsed);
    } else if (choice === choices[2]) {
        selectedNode.setSelected(true);
    } else if (choice === choices[3]) {
        selectedNode.setSelected(true, false);
    } else if (choice === choices[4]) {
        selectedNode.setSelected(false);
    } else if (choice === choices[5]) {
        selectedNode.setSelected(false, true);
    } else if (choice === 'Get Parent') {
        nextNode = parent;
    } else {
        let childNode = children.find(child => child.label === choice);
        nextNode = childNode;
    }
    if (nextNode) {
        let updatedNode = await azdata.objectexplorer.getNode(nextNode.connectionId, nextNode.nodePath);
        this.interactWithOENode(updatedNode);
    }
}

vscode.commands.registerCommand('mssql.objectexplorer.interact', () => {
    azdata.objectexplorer.getActiveConnectionNodes().then(activeConnections => {
        vscode.window.showQuickPick(activeConnections.map(connection => connection.label + ' ' + connection.connectionId)).then(selection => {
            let selectedNode = activeConnections.find(connection => connection.label + ' ' + connection.connectionId === selection);
            this.interactWithOENode(selectedNode);
        });
    });
});
```

## <a name="proposed-apis"></a>API propuestas

Hemos agregado API propuestas para permitir que las extensiones muestren la interfaz de usuario personalizada en los cuadros de diálogo, los asistentes y las pestañas de documentos, entre otras funcionalidades. Consulte el [archivo de tipos de API propuestas](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.proposed.d.ts) para obtener más documentación, aunque tenga en cuenta que estas API están sujetas a cambios en cualquier momento. Puede encontrar ejemplos de cómo usar algunas de estas API en la [extensión de ejemplo "sqlservices"](https://github.com/Microsoft/azuredatastudio/tree/main/samples/sqlservices).


