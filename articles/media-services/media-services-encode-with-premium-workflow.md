---
title: "Codificação avançada com o Fluxo de Trabalho do Media Encoder Premium | Microsoft Docs"
description: "Saiba como codificar com fluxo de trabalho do Media Encoder Premium. Os exemplos de código são escritos em C# e usam a SDK dos Serviços de Mídia para .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 0f4c87ac-810a-4d42-8df8-923dff2016c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 6dcc79a2adf81c82d245c99116f28eb4db983396
ms.lasthandoff: 12/08/2016


---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Codificação avançada com fluxo de trabalho do Media Encoder Premium
> [!NOTE]
> O processador de mídia do Fluxo de Trabalho do Media Encoder Premium analisado neste tópico não está disponível na China.
>
>

Para solucionar dúvidas sobre o codificador premium, envie um email para o mepd em Microsoft.com.

## <a name="overview"></a>Visão geral
Os Serviços de Mídia do Microsoft Azure estão apresentando o processador de mídia do **Fluxo de trabalho do Codificador de Mídia Premium** . Esse processador oferece recursos avançados de codificação para seus fluxos de trabalho premium sob demanda.

Os tópicos a seguir descrevem os detalhes relacionados ao **Fluxo de Trabalho do Media Encoder Premium**:

* [Formatos suportados pelo Fluxo de trabalho do Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – Discute os formatos de arquivo e codecs com suporte do **Fluxo de trabalho do Media Encoder Premium**.
* [Visão geral e comparação dos codificadores de mídia sob demanda do Azure](media-services-encode-asset.md) compara os recursos de codificação do **Fluxo de Trabalho do Media Encoder Premium** e do **Media Encoder Standard**.

Este tópico demonstra como codificar com o **Fluxo de Trabalho do Media Encoder Premium** usando o .NET.

As tarefas de codificação para o **Fluxo de trabalho do Media Encoder Premium** exigem um arquivo de configuração separado, chamado de arquivo de Fluxo de trabalho. Esses arquivos têm uma extensão .workflow e são criados usando a ferramenta [Designer de Fluxo de Trabalho](media-services-workflow-designer.md) .

## <a name="encode"></a>Codificar
As tarefas de codificação para o **Fluxo de trabalho do Media Encoder Premium** exigem um arquivo de configuração separado, chamado de arquivo de Fluxo de trabalho. Esses arquivos têm uma extensão .workflow e são criados usando a ferramenta [Designer de Fluxo de Trabalho](media-services-workflow-designer.md) .

Você também pode obter os arquivos de fluxo de trabalho padrão [aqui](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). A pasta também contém a descrição desses arquivos.

Os arquivos de fluxo de trabalho devem ser carregados para sua conta de serviços de mídia como um ativo e esse ativo deve ser passado para a tarefa de codificação.

O exemplo a seguir demonstra como codificar com o **Fluxo de trabalho do Media Encoder Premium**.

As seguintes etapas são executadas:

1. Criar um ativo e carregar um arquivo de fluxo de trabalho.
2. Criar um ativo e carregar um arquivo de mídia de origem.
3. Obter o processador de mídia "Fluxo de trabalho do Media Encoder Premium".
4. Criar um trabalho e uma tarefa.

    Na maioria dos casos, a cadeia de caracteres de configuração para a tarefa está vazia (como no exemplo a seguir). Há alguns cenários avançados (que exigem que você defina propriedades de tempo de execução dinamicamente) em que você forneceria uma cadeia de caracteres XML para a tarefa de codificação. Exemplos desses cenários são: criação de uma sobreposição, união de mídia paralela ou sequencial, colocação de legenda.
5. Adicionar dois ativos de entrada à tarefa.

    a. 1º – o ativo de fluxo de trabalho.

    b. 2º – o ativo de vídeo.

    **Observação**: o ativo de fluxo de trabalho deve ser adicionado à tarefa antes do ativo de mídia.
   A cadeia de caracteres de configuração para essa tarefa deve estar vazia.
6. Envie o trabalho de codificação.

O exemplo a seguir é um exemplo completo. Para saber mais sobre como configurar com desenvolvimento dos Serviços de Mídia com o .NET, consulte [Desenvolvimento de serviços de mídia com o .NET](media-services-dotnet-how-to-use.md)

     using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;

    namespace MediaEncoderPremiumWorkflowSample
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;

            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");


            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);

                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

            }

            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

                var fileName = Path.GetFileName(singleFilePath);

                var assetFile = asset.AssetFiles.Create(fileName);

                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                Console.WriteLine("Upload {0}", assetFile.Name);

                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);

                return asset;
            }

            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job.
                // This output is specified as AssetCreationOptions.None, which
                // means the output asset is not encrypted.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();

                // If job state is Error the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }

                return job.OutputMediaAssets[0];
            }

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }

            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();

                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");

                Console.Write(builder.ToString());
            }


            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

                return processor;
            }
        }
    }


Para solucionar dúvidas sobre o codificador premium, envie um email para o mepd em Microsoft.com.

## <a name="media-services-learning-paths"></a>Roteiros de aprendizagem dos Serviços de Mídia
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fornecer comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

