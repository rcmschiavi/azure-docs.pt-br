---
title: Escala de aumento do Apache Kafka - Azure HDInsight | Microsoft Docs
description: Saiba como configurar discos gerenciados para o cluster Apache Kafka no Azure HDInsight para aumentar a escalabilidade.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/14/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 1b3e0d06c8b25158e421f02b587b4ae4836d80ad
ms.contentlocale: pt-br
ms.lasthandoff: 07/13/2017

---

# Configurar o armazenamento e a escalabilidade para o Apache Kafka no HDInsight
<a id="configure-storage-and-scalability-for-apache-kafka-on-hdinsight" class="xliff"></a>

Saiba como configurar o número de discos gerenciados usados pelo Apache Kafka no HDInsight.

O Kafka no HDInsight usa o disco local das máquinas virtuais no cluster HDInsight. Como o Kafka tem E/S bastante pesadas, os [Azure Managed Disks](../storage/storage-managed-disks-overview.md) são usados para fornecer a alta taxa de transferência e fornecer mais armazenamento por nó. Se os discos rígidos virtuais (VHD) tradicionais tiverem sido usados para o Kafka, cada nó estará limitado a 1 TB. Com os discos gerenciados, você pode usar vários discos para alcançar 16 TB para cada nó no cluster.

O diagrama a seguir fornece uma comparação entre o Kafka no HDInsight antes dos discos gerenciados e o Kafka no HDInsight com os discos gerenciados:

![Diagrama mostrando o Kafka no HDInsight usando um único vhd por vm versus vários discos gerenciados por vm](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## Configurar discos gerenciados: portal do Azure
<a id="configure-managed-disks-azure-portal" class="xliff"></a>

1. Siga as etapas de [Criar um cluster HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md) para compreender as etapas comuns para criar um cluster usando o portal. Não conclua o processo de criação do portal.

2. Na folha __Tamanho do cluster__, use o campo __Discos por nó de trabalho__ para configurar o número de discos.

    > [!NOTE]
    > O tipo de disco gerenciado pode ser __Standard__ (HDD) ou __Premium__ (SSD). Os discos Premium são usados com as VMs das séries DS e GS. Todos os outros tipos VM usam o padrão.

    ![Imagem da folha de tamanho de cluster com os discos por nó de trabalho realçados](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## Configurar discos gerenciados: modelo do Gerenciador de Recursos
<a id="configure-managed-disks-resource-manager-template" class="xliff"></a>

Para controlar o número de discos usados por nós de trabalho em um cluster do Kafka, use a seção a seguir do modelo:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Você pode encontrar um modelo completo que demonstra como configurar os discos gerenciados em [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## Próximas etapas
<a id="next-steps" class="xliff"></a>

Para saber mais sobre como trabalhar com o Kafka no HDInsight, veja os seguintes documentos:

* [Usar MirrorMaker para criar uma réplica de Kafka no HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Usar Apache Storm com Kafka no HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Usar o Apache Spark com o Kafka no HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Conectar-se ao Kafka no HDInsight (preview) por meio de uma Rede Virtual do Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [Blog do HDInsight em discos gerenciados com o Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
