---
title: Definir o recurso filho no modelo do Azure | Microsoft Docs
description: Mostra como definir o tipo de recurso e o nome do recurso filho em um modelo do Azure Resource Manager
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: d7560b689d7cea56d40ffa2db9542f74a649f9c1
ms.lasthandoff: 03/03/2017


---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Definir o nome e o tipo do recurso filho no modelo do Resource Manager
Ao criar um modelo, você frequentemente precisa incluir um recurso filho que está relacionado a um recurso pai. Por exemplo, o modelo pode incluir um banco de dados e um servidor SQL. O servidor SQL é o recurso pai e o banco de dados é o recurso filho. 

O formato do tipo do recurso filho é: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

O formato do nome do recurso filho é: `{parent-resource-name}/{child-resource-name}`

No entanto, o tipo e o nome são especificados de forma diferente no modelo dependendo de estarem aninhados no recurso pai ou sozinhos no nível superior. Este tópico mostra como lidar com as duas abordagens.

## <a name="nested-child-resource"></a>Recurso filho aninhado
A maneira mais fácil de definir um recurso filho é aninhando-o dentro do recurso pai. O exemplo a seguir mostra um Banco de Dados SQL aninhado em um SQL Server.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Para o recurso filho, o tipo é definido como `databases` , mas seu tipo de recurso completo é `Microsoft.Sql/servers/databases`. O `Microsoft.Sql/servers/` não é fornecido porque ele é presumido do tipo de recurso pai. O nome do recurso filho é definido como `exampledatabase` , mas o nome completo inclui o nome do pai. O `exampleserver` não é fornecido porque ele é presumido do recurso pai.

## <a name="top-level-child-resource"></a>Recurso filho de nível superior
Você pode definir o recurso filho no nível superior. Você pode usar essa abordagem se o recurso pai não estiver implantado no mesmo modelo ou se quiser usar o `copy` para criar vários recursos filho. Com essa abordagem, forneça o tipo de recurso completo e inclua o nome do recurso pai no nome do recurso filho.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

O banco de dados é um recurso filho para o servidor mesmo que eles estejam definidos no mesmo nível do modelo.

## <a name="next-steps"></a>Próximas etapas
* Para ver recomendações sobre como criar modelos, consulte [Práticas recomendadas para criação de modelos do Azure Resource Manager](resource-manager-template-best-practices.md).
* Para obter um exemplo de como criar vários recursos filho, consulte [Implantar várias instâncias de recursos nos modelos do Azure Resource Manager](resource-group-create-multiple.md).

