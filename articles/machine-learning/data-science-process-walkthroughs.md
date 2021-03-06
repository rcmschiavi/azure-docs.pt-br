---
title: "Instruções passo a passo do Processo de Ciência de Dados de Equipe | Microsoft Docs"
description: "O passo a passo mostra como combinar ferramentas e serviços de nuvem e locais em um fluxo de trabalho ou pipeline para criar um aplicativo inteligente."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aa63d5a5-25ee-4c4b-9a4c-7553b98d7f6e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: bradsev
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 6df3fc533b0c375cd85743a971c778071629bb7e
ms.contentlocale: pt-br
ms.lasthandoff: 07/01/2017


---
# <a name="team-data-science-process-walkthroughs"></a>Passo a passo do Processo de Ciência de Dados de Equipe
Os **passo a passos de ponta a ponta** detalhados aqui demonstram as etapas do Processo de Ciência de Dados de Equipe para **cenários específicos**. Eles ilustram como combinar a nuvem, ferramentas locais e serviços em um fluxo de trabalho ou pipeline para criar um **aplicativo inteligente**. Os passo a passos são agrupados por **plataforma** e usam o Spark, HDInsight (Hadoop), Azure Data Lake e SQL Server.


## <a name="hdinsight-spark-using-pyspark-and-scala"></a>HDInsight Spark usando o PySpark e o Scala

### <a name="data-science-using-python-with-spark-on-azure"></a>Ciência de Dados usando o Python com Spark no Azure
O passo a passo [Ciência de Dados usando o Spark no Azure HDInsight](machine-learning-data-science-spark-overview.md) usa o Processo de Ciência de Dados de Equipe em um cenário que usa um [cluster Spark do Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) para armazenar, explorar e apresentar dados de engenharia do conjunto de dados publicamente disponível de corridas de táxi e tarifas em NYC. 

### <a name="data-science-using-scala-with-spark-on-azure"></a>Ciência de dados usando o Scala com Spark no Azure
O passo a passo [Ciência de Dados usando o Scala com o Spark no Azure](machine-learning-data-science-process-scala-walkthrough.md) mostra como usar o Scala para tarefas de aprendizado de máquina supervisionado com a biblioteca de aprendizado de máquina do Spark (MLlib) e pacotes SparkML em um cluster Spark do Azure HDInsight. Ele explica as tarefas que constituem o [Processo de ciência de dados](http://aka.ms/datascienceprocess): ingestão e exploração de dados, visualização, engenharia de recursos, modelagem e consumo de modelo. Os modelos criados incluem a regressão logística e linear, florestas aleatórias e árvores aumentadas gradientes.


## <a name="hdinsight-hadoop"></a>HDInsight Hadoop 

### <a name="use-hdinsight-hadoop-clusters"></a>Usar os clusters HDInsight Hadoop
O passo a passo [Team Data Science Process in action: using HDInsight Hadoop clusters (Processo de Ciência de Dados de Equipe em ação: usando clusters Hadoop do HDInsight)](machine-learning-data-science-process-hive-walkthrough.md) usa um [cluster Hadoop do Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) para armazenar, explorar e apresentar dados de engenharia de um conjunto de dados publicamente disponível de corridas de táxi e tarifas em NYC

### <a name="use-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Usar Clusters Hadoop do Azure HDInsight em um conjunto de dados de 1 TB
O passo a passo [Processo de Ciência de Dados de Equipe em ação: usando clusters Hadoop do Azure HDInsight em um conjunto de dados de 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) apresenta um cenário que usa um [cluster Hadoop do Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) para armazenar, explorar e apresentar dados de engenharia, bem como reduzir dados de um conjuntos de dados da [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) publicamente disponível.


## <a name="azure-data-lake"></a>Azure Data Lake

### <a name="use-azure-data-lake-store-and-analytics"></a>Usar o Azure Data Lake Store e Analytics
O artigo [Ciência de Dados escalonáveis no Azure Data Lake: um passo a passo de ponta a ponta](machine-learning-data-science-process-data-lake-walkthrough.md) mostra como usar o Azure Data Lake para tarefas de exploração de dados e classificação binária em um exemplo do conjunto de dados de táxis de NYC para prever se uma corrida será paga ou não por um cliente. 


## <a name="sql-server-and-sql-data-warehouse"></a>SQL Server e SQL Data Warehouse 

### <a name="use-sql-data-warehouse"></a>Usar o SQL Data Warehouse
O passo a passo [Team Data Science Process in action: using SQL Data Warehouse](machine-learning-data-science-process-sqldw-walkthrough.md) (Processo de Ciência de Dados de Equipe em ação: usando o SQL Data Warehouse) mostra como compilar e implantar modelos de classificação e regressão de aprendizado de máquina usando o SQL DW (SQL Data Warehouse) em um conjunto de dados publicamente disponível de corridas de táxi e tarifas em NYC.

### <a name="use-sql-server"></a>Usar o SQL Server
O passo a passo [Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md) (Processo de Ciência de Dados de Equipe em ação: usando o SQL Server) mostra como compilar e implantar modelos de classificação e regressão de aprendizado de máquina usando o SQL Server e um conjunto de dados publicamente disponível de corridas de táxi e tarifas em NYC.

### <a name="use-r-with-sql-server-r-services"></a>Usar o R com os serviços de R do SQL Server
O artigo [Passo a passo de ponta a ponta de ciência de dados usando os Serviços de R do SQL Server](https://msdn.microsoft.com/library/mt612857.aspx) fornece aos cientistas de dados uma combinação do código R, dos dados do SQL Server e das funções personalizadas do SQL para compilar e implantar um modelo de R para o SQL Server.

### <a name="use-t-sql-with-sql-server-r-services"></a>Usar o T-SQL com os serviços de R do SQL Server
O passo a passo [Análise avançada no banco de dados para desenvolvedores do SQL](https://msdn.microsoft.com/library/mt683480.aspx) proporciona aos programadores de SQL uma experiência de criar uma solução de análise avançada com o Transact-SQL usando os Serviços de R do SQL Server para operacionalizar uma solução de R.


### <a name="use-t-sql-with-sql-server-python-services"></a>Usar o T-SQL com os Serviços do Python no SQL Server
O passo a passo [Análise do Python no banco de dados para desenvolvedores do SQL](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) fornece aos programadores do SQL a experiência de criação de uma solução de aprendizado de máquina no SQL Server. Ele demonstra como incorporar o Python em um aplicativo adicionando um código do Python aos procedimentos armazenados.

## <a name="whats-next"></a>O que vem a seguir?
Para obter uma visão geral dos tópicos que orientam as tarefas que compõem o processo de ciência de dados no Azure, veja [Processo de Ciência de Dados](http://aka.ms/datascienceprocess). 


