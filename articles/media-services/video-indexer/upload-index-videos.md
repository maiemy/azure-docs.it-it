---
title: Caricare e indicizzare video con Video Indexer
titleSuffix: Azure Media Services
description: Questo argomento illustra come usare le API per caricare e indicizzare i video con Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/12/2020
ms.author: juliako
ms.custom: devx-track-csharp
ms.openlocfilehash: 014c9759756a1da922a5141f064991827d593208
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2020
ms.locfileid: "94630261"
---
# <a name="upload-and-index-your-videos"></a>Caricare e indicizzare i video  

Una volta caricato il video, Video Indexer (facoltativamente) codificare il video, illustrato nell'articolo. Al momento della creazione di un account di Video Indexer, è possibile scegliere un account di valutazione gratuito (in cui si ottiene un certo numero di minuti di indicizzazione gratuita) o un'opzione a pagamento (in cui non si è limitati dalla quota). Con la versione di valutazione gratuita, Video Indexer offre fino a 600 minuti di indicizzazione gratuita per gli utenti di siti Web e fino a 2400 minuti di indicizzazione gratuita per gli utenti di API. Con l'opzione a pagamento, si crea un account di Video Indexer [collegato alla sottoscrizione di Azure e un account di Servizi multimediali di Azure](connect-to-azure.md). Si paga per minuti indicizzati. per altre informazioni, vedere [prezzi di servizi multimediali](https://azure.microsoft.com/pricing/details/media-services/).

Durante il caricamento di video con l'API Video Indexer, sono disponibili le opzioni di caricamento seguenti: 

* caricare il video da un URL (scelta consigliata),
* inviare il file video come matrice di byte nel corpo della richiesta,
* usare l'asset di Servizi multimediali di Azure esistente indicando l'[ID asset](../latest/assets-concept.md) (supportato solo negli account a pagamento).

Questo articolo illustra come caricare e indicizzare i video con queste opzioni:

* [Sito Web di Video Indexer](#upload-and-index-a-video-using-the-video-indexer-website) 
* [API di Video Indexer](#upload-and-index-with-api)

## <a name="supported-file-formats-for-video-indexer"></a>Formati di file supportati per Video Indexer

Per un elenco dei formati di file che è possibile usare con Video Indexer, vedere l'articolo [Contenitore di input/formati di file](../latest/media-encoder-standard-formats.md#input-containerfile-formats).

## <a name="video-files-storage"></a>Archiviazione file video

- Con un account di Video Indexer a pagamento, viene creato un account di Video Indexer connesso alla sottoscrizione di Azure e a un account di servizi multimediali di Azure. Per altre informazioni, vedere [creare un account video Indexer connesso ad Azure](connect-to-azure.md).
- I file video vengono archiviati in archiviazione di Azure da servizi multimediali di Azure. Non esiste alcun limite di tempo.
- È sempre possibile eliminare i file audio e video, nonché tutti i metadati e le informazioni dettagliate estratti da tali file per Video Indexer. Dopo aver eliminato un file da Video Indexer, il file, i metadati e le informazioni dettagliate vengono rimossi definitivamente da Video Indexer. Se tuttavia è stata implementata la propria soluzione di backup in Archiviazione di Azure, il file rimane nella risorsa di archiviazione di Azure.
- Il persistenza di un video è identico, indipendentemente dal fatto che il caricamento venga eseguito dal sito Web Video Indexer o tramite l'API di caricamento.
   
## <a name="upload-and-index-a-video-using-the-video-indexer-website"></a>Caricare e indicizzare un video tramite il sito Web Video Indexer

> [!NOTE]
> Il nome del video non può contenere più di 80 caratteri.

1. Accedere al sito Web di [Video Indexer](https://www.videoindexer.ai/).
1. Per caricare un video, premere il pulsante o il collegamento **Upload** (Carica).

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/video-indexer-upload.png" alt-text="Upload":::
1. Dopo il caricamento del video, Video Indexer avvia l'indicizzazione e l'analisi.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/progress.png" alt-text="Stato del caricamento":::
1. Al termine dell'analisi si riceverà una notifica con un collegamento al video e una breve descrizione del relativo contenuto, Ad esempio: persone, argomenti, OCR.

## <a name="upload-and-index-with-api"></a>Caricare e indicizzare con l'API

Usare l'API di [caricamento video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) per caricare e indicizzare i video in base a un URL. L'esempio di codice seguente include il codice commentato che Mostra come caricare la matrice di byte. 

### <a name="configurations-and-params"></a>Configurazioni e parametri

Questa sezione descrive alcuni dei parametri facoltativi e i casi in cui è utile impostarli. Per informazioni sui parametri più aggiornati, vedere l'API di [caricamento video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) .

#### <a name="externalid"></a>externalID 

Questo parametro consente di specificare un ID che verrà associato al video. L'ID può essere applicato all'integrazione del sistema esterno di gestione di contenuti video (VCM). I video situati nel portale di Video Indexer si possono cercare usando l'ID esterno specificato.

#### <a name="callbackurl"></a>callbackUrl

Un URL che viene usato per notificare al cliente (con una richiesta POST) gli eventi seguenti:

- Modifica stato indicizzazione: 
    - Proprietà:    
    
        |Nome|Descrizione|
        |---|---|
        |id|ID video|
        |state|Lo stato del video|  
    - Esempio: https: \/ /test.com/notifyme?projectName=MyProject&ID = 1234abcd&state = processed
- Persona identificata nel video:
  - Proprietà
    
      |Nome|Descrizione|
      |---|---|
      |id| ID video|
      |faceId|L'ID viso che appare nell'indice video|
      |knownPersonId|L'ID utente univoco all'interno di un modello di viso|
      |personName|Il nome della persona|
        
    - Esempio: https: \/ /test.com/notifyme?projectName=MyProject&ID = 1234abcd&FaceId = 12&knownPersonId = CCA84350-89B7-4262-861C-3CAC796542A5&PersonName = Inigo_Montoya 

##### <a name="other-considerations"></a>Altre considerazioni

- Video Indexer restituisce tutti i parametri esistenti forniti nell'URL originale.
- L'URL specificato deve essere codificato.

#### <a name="indexingpreset"></a>indexingPreset

Usare questo parametro se registrazioni non elaborate o esterne contengono rumore di fondo. Questo parametro si usa per configurare il processo di indicizzazione. È possibile specificare i valori seguenti:

- `AudioOnly`: indicizzare ed estrarre informazioni dettagliate usando solo l'audio (ignorando il video)
- `VideoOnly` -Indicizzare ed estrarre informazioni dettagliate usando solo video (ignorando l'audio)
- `Default`: indicizzare ed estrarre informazioni dettagliate usando audio e video
- `DefaultWithNoiseReduction`: indicizzare ed estrarre informazioni dettagliate sia dall'audio che dal video, applicando gli algoritmi di riduzione del rumore al flusso audio

> [!NOTE]
> Video Indexer copre fino a due tracce di audio. Se sono presenti più tracce audio nel file, verranno considerate come una traccia.<br/>
Se si desidera indicizzare le tracce separatamente, sarà necessario estrarre il file audio pertinente e indicizzarlo come `AudioOnly` .

Il prezzo dipende dall'opzione di indicizzazione selezionata.  

#### <a name="priority"></a>priority

I video vengono indicizzati da Video Indexer in base alle rispettive priorità. Usare il parametro **priority** per specificare la priorità dell'indice. Sono validi i valori seguenti: **Basso** , **Normale** (predefinito) e **Alto**.

Il parametro **priority** è supportato solo per gli account a pagamento.

#### <a name="streamingpreset"></a>streamingPreset

Una volta caricato il video, facoltativamente Video Indexer lo codifica. Procede quindi con l'indicizzazione e l'analisi del video. Quando Video Indexer termina l'analisi, si riceve una notifica con l'ID del video.  

Quando si usano le API [Upload video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) o [Re-Index Video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?), uno dei parametri facoltativi è `streamingPreset`. Se si imposta `streamingPreset` su `Default`, `SingleBitrate` o `AdaptiveBitrate`, viene attivato il processo di codifica. Dopo il completamento dei processi di indicizzazione e codifica, il video viene pubblicato in modo che sia possibile eseguirne lo streaming. L'endpoint di streaming da cui si vuole trasmettere il video deve essere nello stato **In esecuzione**.

Per SingleBitrate, viene applicato il costo del codificatore standard per l'output. Se l'altezza del video è maggiore o uguale a 720, Video Indexer lo codifica come 1280x720. In caso contrario, come 640x468.
L'impostazione predefinita è [codifica compatibile](../latest/content-aware-encoding.md)con il contenuto.

Per poter eseguire i processi di indicizzazione e codifica, l'[account di Servizi multimediali di Microsoft Azure connesso al proprio account di Video Indexer](connect-to-azure.md) richiede unità riservate. Per altre informazioni, vedere [Panoramica del ridimensionamento dell'elaborazione multimediale](../previous/media-services-scale-media-processing-overview.md). Poiché si tratta di processi a elevato utilizzo di calcolo, è consigliabile il tipo di unità S3. Il numero di unità riservate definisce il numero massimo di processi che è possibile eseguire in parallelo. In linea generale è consigliabile usare 10 unità riservate S3. 

Se si vuole solo indicizzare il video, ma non codificarlo, impostare `streamingPreset` su `NoStreaming`.

#### <a name="videourl"></a>videoUrl

URL del file audio/video da indicizzare. L'URL deve puntare a un file multimediale; le pagine HTML non sono supportate. Il file può essere protetto da un token di accesso fornito come parte dell'URI e l'endpoint che gestisce il file deve essere protetto con il protocollo TLS 1.2 o versione successiva. L'URL deve essere codificato. 

Se `videoUrl` non viene specificato, Video Indexer richiede che il file venga passato come contenuto di un corpo multipart/form.

### <a name="code-sample"></a>Esempio di codice

Il frammento di codice C# seguente illustra l'uso di tutte le API di Video Indexer.

**Istruzioni per l'esecuzione dell'esempio di codice seguente**

Dopo aver copiato questo codice nella piattaforma di sviluppo, sarà necessario specificare due parametri: chiave di autenticazione e URL video di gestione API.

* Chiave API: la chiave API è la chiave di sottoscrizione di gestione API personale, che consente di ottenere un token di accesso per eseguire operazioni sull'account Video Indexer. 

    Per ottenere la chiave API, passare a questo flusso:

    * Passare a https://api-portal.videoindexer.ai/
    * Accedi
    * Passa alla **Products**  ->  **Authorization**  ->  **sottoscrizione di autorizzazione** per i prodotti
    * Copiare la **chiave primaria**
* URL video: URL del file video/audio da indicizzare. L'URL deve puntare a un file multimediale; le pagine HTML non sono supportate. Il file può essere protetto da un token di accesso fornito come parte dell'URI e l'endpoint che gestisce il file deve essere protetto con il protocollo TLS 1.2 o versione successiva. L'URL deve essere codificato.

Il risultato dell'esecuzione dell'esempio di codice include un URL widget Insight e un URL Widget Player che consentiranno di esaminare le informazioni dettagliate e i video caricati rispettivamente. 


```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var apiKey = "..."; // replace with API key taken from https://aka.ms/viapi

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    
    // take the relevant account, here we simply take the first, 
    // you can also get the account via accounts.First(account => account.Id == <GUID>);
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Console.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =new byte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(new ByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video ID from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Console.WriteLine("Uploaded");
    Console.WriteLine("Video ID:");
    Console.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Console.WriteLine("");
        Console.WriteLine("State:");
        Console.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Console.WriteLine("");
            Console.WriteLine("Full JSON:");
            Console.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Console.WriteLine("");
    Console.WriteLine("Search:");
    Console.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Console.WriteLine("Insights Widget url:");
    Console.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
     Console.WriteLine("");
     Console.WriteLine("Player Widget url:");
     Console.WriteLine(playerWidgetLink);
     Console.WriteLine("\nPress Enter to exit...");
     String line = Console.ReadLine();
     if (line == "enter")
     {
         System.Environment.Exit(0);
     }

}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```

### <a name="common-errors"></a>Errori comuni

L'operazione di caricamento può restituire i codici di stato elencati nella tabella seguente.

|Codice di stato|ErrorType (nel corpo della risposta)|Descrizione|
|---|---|---|
|409|VIDEO_INDEXING_IN_PROGRESS|Lo stesso video è già in fase di elaborazione nell'account specificato.|
|400|VIDEO_ALREADY_FAILED|Lo stesso video ha restituito un errore di elaborazione nell'account specificato meno di 2 ore prima. I client API devono attendere almeno 2 ore prima di caricare nuovamente un video.|
|429||Gli account di valutazione sono consentiti 5 caricamenti al minuto. Gli account a pagamento sono consentiti 50 caricamenti al minuto.|

## <a name="uploading-considerations-and-limitations"></a>Considerazioni e limiti sul caricamento
 
- Il nome del video non può contenere più di 80 caratteri.
- Quando si carica il video in base all'URL (scelta preferita), l'endpoint deve essere protetto con il protocollo TLS 1.2 o una versione successiva.
- Le dimensioni di caricamento con l'opzione URL sono limitate a 30 GB.
- La lunghezza dell'URL della richiesta è limitata a 6144 caratteri, in cui la lunghezza dell'URL della stringa di query è limitata a 4096 caratteri.
- Le dimensioni di caricamento con l'opzione matrice di byte sono limitate a 2 GB.
- L'opzione di matrice di byte raggiunge il timeout dopo 30 minuti.
- L'URL specificato nel `videoURL` parametro deve essere codificato.
- L'indicizzazione degli asset di Servizi multimediali ha la stessa limitazione dell'indicizzazione dall'URL.
- Video Indexer ha un limite di durata massima di 4 ore per un singolo file.
- L'URL deve essere accessibile, ad esempio un URL pubblico. 

    Se è un URL privato, il token di accesso deve essere specificato nella richiesta.
- L'URL deve puntare a un file multimediale valido e non a una pagina Web, ad esempio un collegamento alla `www.youtube.com` pagina.
- In un account a pagamento è possibile caricare fino a 50 film al minuto e in un account di prova gratuito fino a 5 film al minuto.

> [!Tip]
> È consigliabile usare .NET framework versione 4.6.2. o versione successiva perché le versioni precedenti di .NET Framework non usano per impostazione predefinita TLS 1.2.
>
> Se è necessario usare versioni di .NET Framework precedenti, aggiungere una riga al codice prima di effettuare la chiamata API REST:  <br/> System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls12;

## <a name="next-steps"></a>Passaggi successivi

[Esaminare l'output del Video Indexer di Azure prodotto dall'API](video-indexer-output-json-v2.md)
