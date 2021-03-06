---
title: Exemplo de Script CLI do Azure - montar o disco do sistema operacional | Microsoft Docs
description: Exemplo de Script CLI do Azure - montar o disco do sistema operacional
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: fca1844657821a8ce20deefe0056321c365ce689
ms.contentlocale: pt-br
ms.lasthandoff: 05/15/2017

---

# <a name="troubleshoot-a-vms-operating-system-disk"></a>Solucionar problemas de um disco de sistema operacional de VMs

Esse script monta o disco do sistema operacional de uma máquina virtual com falha ou um problema como um disco de dados para uma segunda máquina virtual. Isso pode ser útil ao solucionar problemas de disco problemas ou recuperação de dados. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[principal](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Criação rápida de VM")]

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir para criar um grupo de recursos, uma máquina virtual e todos os recursos relacionados. Cada comando na tabela redireciona para a documentação específica do comando.

| Command | Observações |
|---|---|
| [az vm show](https://docs.microsoft.com/cli/azure/vm#show) | Retorne a lista de máquinas virtuais. Nesse caso, a opção de consulta é usada para retornar o disco de sistema operacional da máquina virtual. Este valor é adicionado a um nome de variável 'uri'. |
| [az vm delete](https://docs.microsoft.com/cli/azure/vm#delete) | Exclui uma máquina virtual. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Cria uma máquina virtual.  |
| [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) | Anexa um disco a uma máquina virtual. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Retorna os endereços IP de uma máquina virtual. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Os exemplos de script da CLI de máquina virtual adicionais podem ser encontrados na [documentação da VM Linux do Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

