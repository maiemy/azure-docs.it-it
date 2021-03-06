---
title: Funzionalità tecniche di sicurezza in Azure-Microsoft Azure
description: Introduzione ai servizi di sicurezza in Azure che consentono di proteggere i dati, le risorse e le applicazioni nel cloud.
services: security
author: terrylanfear
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.date: 07/13/2020
ms.author: terrylan
ms.openlocfilehash: 3113216b2d8a7572bcb864819a379de6823ee444
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2020
ms.locfileid: "94409876"
---
# <a name="azure-security-technical-capabilities"></a>Funzionalità tecniche per la sicurezza di Azure
Questo articolo fornisce un'introduzione ai servizi di sicurezza in Azure che consentono di proteggere i dati, le risorse e le applicazioni nel cloud e di soddisfare le esigenze di sicurezza dell'azienda.

## <a name="azure-platform"></a>Piattaforma Azure

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) è una piattaforma cloud costituita da servizi di infrastruttura e applicazioni, con servizi dati integrati, analisi avanzata e strumenti e servizi per sviluppatori, ospitata nei data center del cloud pubblico Microsoft. I clienti usano Azure per molti diversi scenari e capacità, dalle funzionalità di calcolo, rete e archiviazione di base a servizi app Web e per dispositivi mobili e scenari cloud completi come Internet delle cose, che possono essere usati con tecnologie open source e distribuiti come cloud ibrido oppure ospitati in un data center del cliente. Azure offre tecnologia cloud in blocchi predefiniti per consentire alle aziende di ridurre i costi, velocizzare l'innovazione e gestire i sistemi in modo proattivo. Quando si creano asset IT o se ne esegue la migrazione in un provider di servizi cloud, si dipende dalla capacità di tale organizzazione di proteggere le applicazioni e i dati con i servizi e i controlli offerti per gestire la sicurezza degli asset basati sul cloud.

Microsoft Azure è l'unico provider di servizi di cloud computing che offre una piattaforma applicativa e un'infrastruttura distribuite come servizio coerenti e sicure affinché i team possano operare con diverse competenze di cloud e diversi livelli di complessità dei progetti, con servizi dati integrati e funzionalità di analisi che consentono di ottenere informazioni dai dati ovunque si trovino, in piattaforme sia Microsoft che non Microsoft e in strumenti e framework aperti, e la possibilità di integrare cloud e locale nonché distribuire i servizi cloud di Azure in data center locali. Nell'ambito di Microsoft Trusted Cloud, i clienti fanno affidamento su Azure per usufruire di livelli di sicurezza, affidabilità, conformità e riservatezza leader del settore, nonché di una vasta rete di persone, partner e processi per il supporto delle organizzazioni nel cloud.

Microsoft Azure consente di:

- Accelerare l'innovazione con il cloud.

- Basare le app e le decisioni aziendali su informazioni dettagliate.

- Creare con facilità e distribuire ovunque.

- Proteggere l'azienda.

## <a name="security-technical-capabilities-to-fulfil-your-responsibility"></a>Funzionalità tecniche di sicurezza per soddisfare le proprie responsabilità

Microsoft Azure offre servizi che consentono di soddisfare le esigenze di sicurezza, privacy e conformità. Nell'immagine seguente sono illustrati i vari servizi di Azure disponibili per la creazione di un'infrastruttura di applicazione sicura e conforme basata sugli standard del settore.

![Funzionalità tecniche per la sicurezza disponibili: quadro generale](./media/technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access"></a>Gestire e controllare l'identità e l'accesso utente

Azure consente di proteggere le informazioni aziendali e personali gestendo le identità e le credenziali degli utenti e controllando l'accesso.

### <a name="azure-active-directory"></a>Azure Active Directory

Le soluzioni Microsoft per la gestione delle identità e degli accessi consentono di proteggere l'accesso ad applicazioni e risorse nel data center aziendale e nel cloud, abilitando livelli aggiuntivi di convalida, ad esempio l'autenticazione a più fattori e i criteri di accesso condizionale. Il monitoraggio delle attività sospette tramite funzioni avanzate di report di sicurezza, controllo e avvisi consente di attenuare i potenziali problemi di sicurezza. [Azure Active Directory Premium](../../active-directory/fundamentals/active-directory-whatis.md) fornisce Single Sign-on a migliaia di app cloud e l'accesso alle app Web eseguite in locale.

I vantaggi della sicurezza di Azure Active Directory (AD) includono la possibilità di:

- È possibile creare e gestire una singola identità per ogni utente in un'azienda ibrida, mantenendo sincronizzati utenti, gruppi e dispositivi.

- Offrire l'accesso Single Sign-On alle applicazioni, incluse migliaia di app SaaS preintegrate.

- Abilitare la sicurezza dell'accesso alle applicazioni grazie all'autenticazione a più fattori basata su regole per applicazioni locali e cloud.

- Consentire l'accesso remoto sicuro ad applicazioni Web locali con il proxy di applicazione di Azure AD.

Il [portale Azure Active Directory](https://aad.portal.azure.com/) è disponibile come parte del portale di Azure. Da questo dashboard è possibile ottenere una panoramica dello stato dell'organizzazione e gestire facilmente la directory, gli utenti o l'accesso alle applicazioni.

![Azure Active Directory](./media/technical-capabilities/azure-security-technical-capabilities-fig2.png)

Di seguito sono riportate le principali funzionalità di gestione delle identità di Azure:

- Single Sign-On

- Autenticazione a più fattori

- Monitoraggio della sicurezza, avvisi e report basati su Machine Learning

- Gestione delle identità e dell'accesso degli utenti

- Registrazione del dispositivo

- Privileged Identity Management

- Identity Protection

#### <a name="single-sign-on"></a>Single Sign-On

[Single Sign-on (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) consente di accedere a tutte le applicazioni e risorse necessarie per le attività aziendali effettuando l'accesso una sola volta con un singolo account utente. Dopo aver effettuato l'accesso, è possibile accedere a tutte le applicazioni necessarie senza dover ripetere una seconda volta l'autenticazione (ad esempio, digitando una password).

Molte organizzazioni si basano su applicazioni SaaS (Software as a Service), ad esempio Microsoft 365, box e Salesforce, per la produttività degli utenti finali. In passato, il personale IT doveva creare e aggiornare singoli account utente in ogni applicazione SaaS e gli utenti dovevano ricordare una password per ogni applicazione SaaS.

[Azure ad estende l'Active Directory locale nel cloud](../../active-directory/manage-apps/what-is-single-sign-on.md), consentendo agli utenti di usare il proprio account aziendale principale non solo per accedere ai dispositivi appartenenti a un dominio e alle risorse aziendali, ma anche a tutte le applicazioni Web e SaaS necessarie per il loro lavoro.

Non solo gli utenti non devono più gestire diversi set di nomi utente e password, ma è anche possibile eseguire il provisioning o deprovisioning automatico dell'accesso alle applicazioni in base ai gruppi aziendali e al relativo stato di dipendente. [Azure ad introduce controlli di governance di sicurezza e accesso](../../active-directory/manage-apps/view-applications-portal.md) che consentono di gestire centralmente l'accesso degli utenti tra le applicazioni SaaS.

#### <a name="multi-factor-authentication"></a>Autenticazione a più fattori

[Azure multi-factor authentication (](../../active-directory/authentication/concept-mfa-howitworks.md) multi-factor authentication) è un metodo di autenticazione che richiede l'uso di più di un metodo di verifica e aggiunge un secondo livello di sicurezza critico agli accessi e alle transazioni degli utenti. L'autenticazione a più fattori [consente di proteggere](../../active-directory/authentication/concept-mfa-howitworks.md) l'accesso ai dati e alle applicazioni, garantendo al tempo stesso agli utenti una procedura di accesso semplice. Offre autenticazione avanzata tramite diverse opzioni di verifica, ad esempio una telefonata, un SMS, una notifica o un codice di verifica dell'app per dispositivi mobili e token OAuth di terze parti.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Monitoraggio della sicurezza, avvisi e report basati su Machine Learning

Monitoraggio della sicurezza, avvisi e report basati su Machine Learning che identificano i modelli di accesso non coerenti per contribuire alla protezione dell'azienda. È possibile usare i report di utilizzo e accesso di Azure Active Directory per ottenere informazioni sull'integrità e sicurezza della directory dell'organizzazione. Con queste informazioni un amministratore di directory può stabilire meglio dove potrebbero esserci possibili rischi per la sicurezza in modo da poterne pianificare adeguatamente la riduzione.

Nel portale di Azure o nel [portale di Azure Active Directory](https://aad.portal.azure.com/) i [report](../../active-directory/reports-monitoring/overview-reports.md) sono classificati come descritto di seguito:

- Report anomalie: contengono eventi di accesso considerati anomali. L'obiettivo è rendere gli utenti consapevoli di tali attività e consentire loro di stabilire se un evento è sospetto.

- Report applicazioni integrate: fornisce informazioni dettagliate sull'uso delle applicazioni cloud nell'organizzazione. Azure Active Directory offre l'integrazione con migliaia di applicazioni cloud.

- Report di errori: segnalano gli errori che possono verificarsi durante il provisioning di account in applicazioni esterne.

- Report specifici dell'utente: Visualizza i dati dell'attività di accesso e del dispositivo per un utente specifico.

- Log attività: contengono un record di tutti gli eventi controllati nelle ultime 24 ore, negli ultimi 7 giorni o negli ultimi 30 giorni, nonché le modifiche alle attività del gruppo e le attività di registrazione e di reimpostazione password.

#### <a name="consumer-identity-and-access-management"></a>Gestione delle identità e dell'accesso degli utenti

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) è un servizio di gestione delle identità globale a disponibilità elevata per le applicazioni rivolte agli utenti con scalabilità fino a centinaia di milioni di identità. Il servizio può essere integrato tra piattaforme mobili e Web. Gli utenti possono accedere a tutte le applicazioni attraverso esperienze personalizzabili usando gli account dei propri social network esistenti o creando nuove credenziali.

In passato, gli sviluppatori di applicazioni che volevano [iscriversi e accedere agli utenti](../../active-directory-b2c/overview.md) nelle proprie applicazioni avrebbero scritto il proprio codice. E dovevano utilizzare database locali o sistemi per archiviare nomi utente e password. Azure Active Directory B2C offre all'organizzazione un modo migliore per integrare la gestione delle identità degli utenti nelle applicazioni con una piattaforma sicura basata sugli standard e un set completo di criteri estendibili.

Con Azure Active Directory B2C, gli utenti possono registrarsi alle applicazioni usando gli account di social networking esistenti (Facebook, Google, Amazon, LinkedIn) o creando nuove credenziali (indirizzo di posta elettronica e password o nome utente e password).

#### <a name="device-registration"></a>Registrazione del dispositivo

[Azure ad registrazione del dispositivo](../../active-directory/devices/overview.md) è la base per gli scenari di [accesso condizionale](../../active-directory/devices/overview.md) basato su dispositivo. Quando un dispositivo viene registrato, Registrazione dispositivo Azure AD fornisce al dispositivo un'identità che viene usata per autenticare il dispositivo quando un utente esegue l'accesso. Il dispositivo autenticato e gli attributi del dispositivo possono quindi essere usati per applicare i criteri di accesso condizionale per le applicazioni ospitate nel cloud e in locale.

In combinazione con una soluzione di [gestione di dispositivi mobili (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) , ad esempio Intune, gli attributi del dispositivo in Azure Active Directory vengono aggiornati con informazioni aggiuntive sul dispositivo. In questo modo è possibile creare regole di accesso condizionale che impongono l'accesso da dispositivi per soddisfare gli standard di sicurezza e conformità.

#### <a name="privileged-identity-management"></a>Privileged Identity Management

[Azure Active Directory (ad) Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md) consente di gestire, controllare e monitorare le identità con privilegi e l'accesso alle risorse in Azure ad, oltre ad altre servizi online Microsoft come Microsoft 365 o Microsoft Intune.

A volte gli utenti devono eseguire operazioni con privilegi in Azure o Microsoft 365 risorse o altre app SaaS. Per questa ragione, è spesso necessario che le organizzazioni concedano agli utenti l'accesso con privilegi permanente in Azure AD. Questo rappresenta un rischio di sicurezza crescente per le risorse ospitate nel cloud poiché le organizzazioni non sono in grado di monitorare completamente le operazioni eseguite dagli utenti con i privilegi amministrativi. Inoltre, se un account utente con accesso privilegiato è compromesso, tale singola violazione può compromettere la sicurezza dell'intero cloud. Gestione identità con privilegi di Azure AD consente di risolvere questo rischio.

Azure AD Privileged Identity Management consente di effettuare le operazioni seguenti:

- Individuare gli utenti amministratori di Azure AD

- Abilitare l'accesso amministrativo "just-in-Time" su richiesta ai Microsoft Online Services, ad esempio Microsoft 365 e Intune

- Ottenere report sulla cronologia degli accessi degli amministratori e sulle modifiche alle assegnazioni degli amministratori

- Ricevere avvisi relativi all'accesso a un ruolo con privilegi

#### <a name="identity-protection"></a>Identity Protection

[Azure ad Identity Protection](../../active-directory/identity-protection/overview-identity-protection.md) è un servizio di sicurezza che offre una visualizzazione consolidata dei rilevamenti dei rischi e potenziali vulnerabilità che interessano le identità dell'organizzazione. Identity Protection usa le funzionalità di rilevamento anomalie di Azure Active Directory esistenti (disponibili tramite i report di attività anomale di Azure AD) e introduce nuovi tipi di rilevamento dei rischi in grado di rilevare le anomalie in tempo reale.

## <a name="secure-resource-access"></a>Proteggere l'accesso alle risorse

La fatturazione costituisce il punto di partenza per il controllo di accesso in Azure. Il proprietario di un account Azure, a cui si accede visitando il [centro account di Azure](https://account.windowsazure.com/subscriptions), è l'amministratore account (AA). Le sottoscrizioni sono contenitori per la fatturazione, ma vengono usate anche come limiti per la sicurezza: ogni sottoscrizione ha un Amministratore del servizio (SA) che può aggiungere, rimuovere e modificare le risorse di Azure nella sottoscrizione tramite il portale di Azure. L'amministratore del servizio predefinito di una nuova sottoscrizione è l'amministratore dell'account, ma quest'ultimo può modificare l'amministratore del servizio nel Centro account di Azure.

![Accesso protetto alle risorse in Azure](./media/technical-capabilities/azure-security-technical-capabilities-fig3.png)

Le sottoscrizioni dispongono anche di un'associazione a una directory. Una directory definisce un set di utenti. Possono essere utenti dell'azienda o dell'istituto di istruzione che ha creato la directory oppure utenti esterni, ossia utenti con account Microsoft. Le sottoscrizioni sono accessibili da un sottogruppo di questi utenti della directory che sono stati nominati Amministratori del servizio (SA) o Co-amministratori (CA); l'unica eccezione è che, per motivi di compatibilità, gli account Microsoft (precedentemente Windows Live ID) possono essere nominati SA o CA senza essere presenti nella directory.

Le aziende orientate sulla sicurezza devono concedere ai propri dipendenti la quantità esatta di autorizzazioni di cui necessitano. un numero eccessivo di autorizzazioni può esporre un account agli attacchi. Un numero di autorizzazioni insufficiente impedisce ai dipendenti di svolgere il proprio lavoro in modo efficiente. Il [controllo degli accessi in base al ruolo di Azure (RBAC di Azure)](../../role-based-access-control/overview.md) consente di risolvere questo problema offrendo una gestione degli accessi con granularità fine per Azure.

![Accesso protetto alle risorse](./media/technical-capabilities/azure-security-technical-capabilities-fig4.png)

Usando il Controllo degli accessi in base al ruolo, è possibile separare i compiti all'interno del team e concedere agli utenti solo la quantità di accesso di cui hanno bisogno per svolgere il proprio lavoro. Invece di concedere a tutti autorizzazioni senza restrizioni per la sottoscrizione o le risorse di Azure, è possibile consentire solo determinate azioni. Ad esempio, usare il Controllo degli accessi in base al ruolo per consentire a un dipendente di gestire le macchine virtuali in una sottoscrizione, mentre un altro utente può gestire i database SQL della stessa sottoscrizione.

![Accesso protetto alle risorse in Azure (controllo degli accessi in base al ruolo)](./media/technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="data-security-and-encryption"></a>Sicurezza e crittografia dei dati

Uno degli aspetti fondamentali della protezione dei dati nel cloud consiste nel tenere conto dei possibili stati in cui possono trovarsi i dati e dei controlli disponibili per tale stato. In relazione alle procedure consigliate per la sicurezza e la crittografia dei dati in Azure, le raccomandazioni riguardano gli stati dei dati seguenti.

- Inattivi: sono inclusi tutti gli oggetti, i contenitori e i tipi di archiviazione di informazioni esistenti in forma statica nei supporti fisici, siano essi dischi magnetici o dischi ottici.
- In transito: quando i dati vengono trasferiti tra componenti, percorsi o programmi, ad esempio sulla rete, attraverso un bus di servizio (da locale a cloud e viceversa, incluse le connessioni ibride, ad esempio ExpressRoute) o durante un processo di input/output, è considerato come in movimento.

### <a name="encryption-at-rest"></a>Crittografia di dati inattivi

La crittografia dei dati inattivi viene descritta in dettaglio in [crittografia dei dati](encryption-atrest.md)inattivi di Azure.

### <a name="encryption-in-transit"></a>Crittografia dei dati in transito

La protezione dei dati in transito deve essere una parte essenziale della strategia di protezione dati. Poiché i dati transitano in modo bidirezionale tra molte posizioni, in generale è consigliabile usare sempre i protocolli SSL/TLS per lo scambio di dati tra posizioni diverse. In alcuni casi è consigliabile isolare l'intero canale di comunicazione tra l'infrastruttura locale e cloud con una rete privata virtuale (VPN).

Per lo spostamento dei dati tra l'infrastruttura locale e Azure, è opportuno considerare le misure di protezione appropriate, ad esempio HTTPS o VPN.

Per le organizzazioni che devono proteggere l'accesso ad Azure da più workstation locali, usare una [VPN da sito a sito di Azure](../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

Per le organizzazioni che hanno bisogno di proteggere l'accesso ad Azure da una workstation locale, usare una [VPN da punto a sito](../../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md).

Set di dati più grandi possono essere spostati su un collegamento WAN ad alta velocità dedicato, ad esempio [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Se si decide di usare ExpressRoute, è possibile anche crittografare i dati a livello di applicazione usando [SSL/TLS](https://support.microsoft.com/kb/257591) o altri protocolli per una maggiore protezione.

Se si interagisce con Archiviazione di Azure tramite il portale di Azure, tutte le transazioni hanno luogo tramite HTTPS. È possibile usare anche l'[API REST di archiviazione](/rest/api/storageservices/) su HTTPS per interagire con [Archiviazione di Azure](https://azure.microsoft.com/services/storage/) e il [database SQL di Azure](https://azure.microsoft.com/services/sql-database/).

Le organizzazioni che non riescono a proteggere i dati in transito sono più vulnerabili agli [attacchi man-in-the-Middle](/previous-versions/office/skype-server-2010/gg195821(v=ocs.14)), all' [intercettazione](/previous-versions/office/skype-server-2010/gg195641(v=ocs.14))e al Hijack della sessione. Questi attacchi possono essere il primo passo per ottenere l'accesso ai dati riservati.

Per altre informazioni sull'opzione VPN di Azure, vedere l'articolo [Pianificazione e progettazione per il gateway VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="enforce-file-level-data-encryption"></a>Applicare la crittografia dei dati a livello di file

[Azure RMS](/azure/information-protection/what-is-azure-rms) usa criteri di crittografia, identità e autorizzazione per proteggere i file e la posta elettronica. Azure RMS opera su più dispositivi, tra cui telefoni, tablet e PC, applicando una protezione all'interno e all'esterno dell'organizzazione. Questa funzionalità è resa possibile dal fatto che Azure RMS aggiunge un livello di protezione che rimane insieme ai dati, anche quando fuoriescono dai confini dell'organizzazione.

Quando si usa Azure RMS per proteggere i file, viene sfruttata una crittografia standard di settore con pieno supporto di [FIPS 140-2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf). Quando si usa Azure RMS per proteggere i dati, la protezione rimane associata al file anche se questo viene copiato in una risorsa di archiviazione che non è sotto il controllo dell'IT, ad esempio un servizio di archiviazione cloud. Lo stesso accade per i file condivisi tramite posta elettronica: il file viene protetto come allegato di un messaggio di posta elettronica, con istruzioni su come aprire l'allegato protetto.
Quando si pianifica l'adozione di Azure RMS, si consiglia quanto segue:

- Installare l'[app RMS sharing](/azure/information-protection/rms-client/sharing-app-windows). Questa app si integra con le applicazioni Office installando un componente aggiuntivo di Office che offre agli utenti un modo semplice per proteggere direttamente i file.

- Configurare le applicazioni e i servizi per supportare Azure RMS

- Creare [modelli personalizzati](/azure/information-protection/configure-policy-templates) che rispecchiano i requisiti aziendali, ad esempio un modello per i dati riservati da applicare in tutti i messaggi di posta elettronica correlati a informazioni riservate.

Le organizzazioni che sono carenti a livello di [classificazione dei dati](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) e protezione dei file potrebbero essere più soggette alla perdita di dati. In assenza di un'adeguata protezione per i file, le organizzazioni non riusciranno a ottenere informazioni di business dettagliate, monitorare i possibili abusi e impedire l'accesso non autorizzato ai file.

> [!Note]
> Per altre informazioni su Azure RMS, leggere l'articolo [Introduzione a Azure Rights Management](/azure/information-protection/requirements).

## <a name="secure-your-application"></a>Proteggere l'applicazione
Mentre Azure è responsabile della sicurezza dell'infrastruttura e della piattaforma su cui l'applicazione viene eseguita, è responsabilità dell'utente proteggere l'applicazione stessa. In altre parole, è necessario sviluppare, distribuire e gestire il codice e i contenuti dell'applicazione in modo sicuro. In caso contrario, il codice o il contenuto dell'applicazione può essere vulnerabile alle minacce.

### <a name="web-application-firewall"></a>Web application firewall
[Web Application Firewall (WAF)](../../web-application-firewall/ag/ag-overview.md) è una funzionalità del [gateway applicazione](../../application-gateway/overview.md) che offre una protezione centralizzata delle applicazioni Web da exploit e vulnerabilità comuni.

Il Web application firewall si basa sulle regole di [OWASP Core Rule Set](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 o 2.2.9. Le applicazioni Web sono sempre più vittime di attacchi che sfruttano le più comuni vulnerabilità note. Per citarne alcuni, tra i più comuni troviamo gli attacchi SQL injection e gli attacchi di scripting intersito. Impedire questo tipo di attacchi nel codice dell'applicazione può risultare un'operazione complessa e potrebbe richiedere una manutenzione rigorosa, l'applicazione di patch e il monitoraggio a più livelli della topologia dell'applicazione. Un Web application firewall centralizzato semplifica notevolmente la gestione della sicurezza e offre agli amministratori delle applicazioni migliori garanzie contro le minacce o le intrusioni. Una soluzione WAF è anche in grado di reagire più velocemente a una minaccia alla sicurezza tramite l'applicazione di patch su una vulnerabilità nota in una posizione centrale, anziché proteggere ogni singola applicazione Web. È possibile convertire facilmente i gateway applicazione esistenti in un gateway applicazione con Web application firewall.

Le vulnerabilità Web per le quali Web application firewall offre protezione includono:

- Protezione dagli attacchi SQL injection

- Protezione dagli attacchi di scripting intersito

- Protezione dai comuni attacchi Web, ad esempio attacchi di iniezione di comandi, richieste HTTP non valide, attacchi HTTP Response Splitting e Remote File Inclusion

- Protezione dalle violazioni del protocollo HTTP

- Protezione contro eventuali anomalie del protocollo HTTP, ad esempio user agent host mancante e accept header

- Prevenzione contro robot, crawler e scanner

- Rilevamento di errori di configurazione dell'applicazione comuni (ad esempio, Apache, IIS e così via)

> [!Note]
> Per un elenco dettagliato delle regole e delle relative protezioni, vedere i set di regole [principali](../../web-application-firewall/ag/ag-overview.md)seguenti:

Azure offre anche diverse funzionalità facili da usare che consentono di proteggere il traffico in ingresso e in uscita per l'app. Consente inoltre ai clienti di proteggere il codice delle proprie applicazioni grazie alla disponibilità di funzionalità esterne per eseguire l'analisi dell'applicazione Web e individuare eventuali vulnerabilità.

- [Configurare l'autenticazione di Azure Active Directory per l'app](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)

- [Proteggere il traffico per l'app abilitando Transport Layer Security (TLS/SSL) - HTTPS](../../app-service/configure-ssl-bindings.md)

  - [Forzare tutto il traffico in ingresso tramite connessione HTTPS](http://microsoftazurewebsitescheatsheet.info/)

  - [Abilitare Strict Transport Security (HSTS)](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)

- [Limitare l'accesso all'app tramite indirizzo IP del client](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Limitare l'accesso all'app tramite comportamento del client: frequenza e concorrenza delle richieste](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Analizzare il codice dell'app Web per le vulnerabilità tramite Tinfoil Security Scanning](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [Configurare l'autenticazione reciproca TLS per richiedere certificati client e connettersi all'app Web](../../app-service/app-service-web-configure-tls-mutual-auth.md)

- [Configurare un certificato client da usare nell'app e connettersi in modo sicuro a risorse esterne](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Rimuovere le intestazioni standard dei server e impedire agli strumenti di rilevare l'impronta digitale dell'app](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Connettere in modo sicuro l'app alle risorse in una rete privata usando una VPN da punto a sito](../../app-service/web-sites-integrate-with-vnet.md)

- [Connettere in modo sicuro l'app alle risorse in una rete privata usando connessioni ibride](../../app-service/app-service-hybrid-connections.md)

Il servizio app di Azure offre la stessa soluzione Antimalware usata da Servizi cloud e Macchine virtuali di Azure. Per altre informazioni, vedere la documentazione di [antimalware](antimalware.md).

## <a name="secure-your-network"></a>Proteggere la rete
Microsoft Azure include una solida infrastruttura di rete per supportare i requisiti di connettività di applicazioni e servizi. La connettività di rete è possibile tra le risorse disponibili in Azure, le risorse locali e quelle ospitate in Azure, nonché da e verso Internet e Azure.

L' [infrastruttura di rete di Azure](/previous-versions/azure/virtual-machines/windows/infrastructure-example) consente di connettere in modo sicuro le risorse di Azure tra loro con le [reti virtuali (reti virtuali)](../../virtual-network/virtual-networks-overview.md). Una rete virtuale è una rappresentazione della propria rete nel cloud. È un isolamento logico della rete nel cloud di Azure dedicato alla sottoscrizione. È possibile connettere le reti virtuali alle reti locali.

![Proteggere la rete (protezione)](./media/technical-capabilities/azure-security-technical-capabilities-fig6.png)

Se è necessario il controllo di accesso di base a livello di rete (in base all'indirizzo IP e ai protocolli TCP o UDP), è possibile usare i [gruppi di sicurezza di rete](../../virtual-network/virtual-network-vnet-plan-design-arm.md). Un gruppo di sicurezza di rete (NSG) è un firewall di filtro dei pacchetti con stato di base e consente di controllare l'accesso in base a [5 tuple](https://www.techopedia.com/definition/28190/5-tuple).

La rete di Azure supporta la personalizzazione del comportamento di routing per il traffico di rete nelle reti virtuali di Azure. A tale scopo è possibile configurare [route definite dall'utente](../../virtual-network/virtual-networks-udr-overview.md) in Azure.

Il [tunneling forzato](https://www.petri.com/azure-forced-tunneling) è un meccanismo che è possibile usare per garantire che i servizi non siano autorizzati a avviare una connessione ai dispositivi su Internet.

Azure supporta la connettività con collegamento WAN dedicato alla rete locale e una rete virtuale di Azure con [ExpressRoute](../../expressroute/expressroute-introduction.md). Il collegamento tra Azure e il sito dell'utente si avvale di una connessione dedicata che non usa la rete Internet pubblica. Se l'applicazione Azure è in esecuzione in più data center, è possibile usare [Gestione traffico di Azure](../../traffic-manager/traffic-manager-overview.md) per indirizzare le richieste degli utenti in modo intelligente tra le istanze dell'applicazione. È anche possibile instradare il traffico ai servizi non in esecuzione in Azure, se sono accessibili da Internet.

## <a name="virtual-machine-security"></a>Sicurezza delle macchine virtuali

[Macchine virtuali di Azure](../../virtual-machines/index.yml) consente di distribuire in modo flessibile un'ampia gamma di soluzioni di elaborazione. Grazie al supporto per Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP e Servizi BizTalk di Azure, è possibile distribuire qualsiasi carico di lavoro, in qualunque linguaggio, praticamente su tutti i sistemi operativi.

Con Azure è possibile usare [software antimalware](antimalware.md) di fornitori di servizi di sicurezza, come Microsoft, Symantec, Trend Micro e Kaspersky, per proteggere le macchine virtuali da file dannosi, adware e altre minacce.

Microsoft Antimalware per Servizi cloud e Macchine virtuali di Azure è una funzionalità di protezione in tempo reale che aiuta a identificare e rimuovere virus, spyware e altro software dannoso. Microsoft Antimalware offre avvisi configurabili quando software dannoso o indesiderato tenta l'installazione o l'esecuzione nei sistemi di Azure.

[Backup di Azure](../../backup/backup-overview.md) è una soluzione scalabile che protegge i dati delle applicazioni senza investimenti di capitale e con costi operativi minimi. Gli errori delle applicazioni possono danneggiare i dati e gli errori umani possono comportare l'introduzione di bug nelle applicazioni. Con Backup di Azure, le macchine virtuali che eseguono Windows e Linux sono protette.

[Azure Site Recovery](../../site-recovery/site-recovery-overview.md) consente di orchestrare la replica, il failover e il ripristino di carichi di lavoro e app in modo che siano disponibili da una località secondaria in caso di inattività di quella primaria.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist"></a>Verifica della conformità: elenco di controllo dei servizi cloud per diligenza

Microsoft ha sviluppato l'[elenco di controllo della due diligence per i servizi cloud](https://aka.ms/cloudchecklist.download) per consentire alle organizzazioni di applicare la due diligence nel valutare la possibilità di passare al cloud. L'elenco di controllo offre alle organizzazioni di qualsiasi tipo e dimensione (sia aziende private che organizzazioni del settore pubblico, inclusi enti pubblici di tutti i livelli e organizzazioni no profit) una struttura per identificare i propri obiettivi e requisiti di prestazioni, servizio, gestione dati e governance. Ciò consente di confrontare le offerte di diversi provider di servizi cloud, ottenendo così una base per un contratto per servizi cloud.

L'elenco di controllo offre una struttura allineata, clausola per clausola, al nuovo standard internazionale per i contratti per servizi cloud, ISO/IEC 19086. Questo standard offre alle organizzazioni un set unificato di considerazioni per facilitare le decisioni in merito all'adozione del cloud e creare un terreno comune per confrontare le offerte di servizi cloud.

L'elenco di controllo promuove un passaggio al cloud accuratamente ponderato, offrendo indicazioni strutturate e un approccio ripetibile e coerente per la scelta di un provider di servizi cloud.

L'adozione del cloud non è più semplicemente una decisione relativa alla tecnologia. Riguardando ogni aspetto di un'organizzazione, i requisiti dell'elenco di controllo coinvolgono tutti i principali decision maker interni: CIO, CISO e i professionisti responsabili degli affari legali, della gestione dei rischi, dell'approvvigionamento e della conformità. In questo modo, aumenta l'efficienza del processo decisionale e le decisioni risultano fondate su valide argomentazioni, con conseguente riduzione delle probabilità di ostacoli imprevisti all'adozione.

Inoltre, l'elenco di controllo:

- Espone i principali argomenti di discussione per i decision maker all'inizio del processo di adozione del cloud.

- Supporta discussioni aziendali esaustive sulle normative e sugli obiettivi specifici dell'organizzazione in materia di riservatezza, informazioni personali e sicurezza dei dati.

- Consente di identificare i potenziali problemi che potrebbero influire su un progetto cloud.

- Offre un set coerente di domande, con termini, definizioni, metriche e risultati finali identici per ogni provider, per semplificare il processo di confronto delle offerte di diversi provider di servizi cloud.

## <a name="azure-infrastructure-and-application-security-validation"></a>Convalida della sicurezza dell'applicazione e dell'infrastruttura di Azure

La [sicurezza operativa di Azure](operational-security.md) si riferisce ai servizi, ai controlli e alle funzionalità disponibili per gli utenti che proteggono i dati, le applicazioni e altri asset in Microsoft Azure.

![Convalida della sicurezza (rilevamento)](./media/technical-capabilities/azure-security-technical-capabilities-fig7.png)

La sicurezza operativa di Azure è basata su un framework che incorpora le conoscenze acquisite tramite varie funzionalità esclusive di Microsoft, tra cui Microsoft Security Development Lifecycle (SDL), il programma Microsoft Security Response Center e una profonda consapevolezza del panorama delle minacce per la sicurezza informatica.

### <a name="microsoft-azure-monitor"></a>Monitoraggio Microsoft Azure

[Monitoraggio di Azure](../../azure-monitor/index.yml) è la soluzione di gestione IT per il cloud ibrido. Usato da solo o per estendere la distribuzione di System Center esistente, i log di monitoraggio di Azure offrono la massima flessibilità e il controllo per la gestione basata sul cloud dell'infrastruttura.

![Monitoraggio di Azure](./media/technical-capabilities/azure-security-technical-capabilities-fig8.png)

Con monitoraggio di Azure è possibile gestire qualsiasi istanza in qualsiasi cloud, tra cui locale, Azure, AWS, Windows Server, Linux, VMware e OpenStack, a un costo inferiore rispetto alle soluzioni competitive. Azure monitor, progettato per il cloud, offre un nuovo approccio alla gestione dell'azienda, che rappresenta il modo più rapido e conveniente per soddisfare nuove esigenze aziendali e gestire nuovi carichi di lavoro, applicazioni e ambienti cloud.

### <a name="azure-monitor-logs"></a>Log di Monitoraggio di Azure

[Log](https://azure.microsoft.com/documentation/services/log-analytics) di monitoraggio di Azure offre servizi di monitoraggio raccogliendo i dati dalle risorse gestite in un repository centrale. Questi dati possono includere eventi, dati sulle prestazioni o dati personalizzati forniti tramite l'API. Dopo essere stati raccolti, i dati sono disponibili per generare avvisi, per l'analisi e per l'esportazione.

![Log di Monitoraggio di Azure](./media/technical-capabilities/azure-security-technical-capabilities-fig9.png)

Questo metodo consente di consolidare i dati di diverse origini per poter combinare i dati dei servizi di Azure con l'ambiente locale esistente. Separa anche nettamente la raccolta dei dati dall'azione eseguita su tali dati in modo che tutte le azioni siano disponibili per tutti i tipi di dati.

### <a name="azure-security-center"></a>Centro sicurezza di Azure

Il [Centro sicurezza di Azure](../../security-center/security-center-introduction.md) consente di prevenire, rilevare e rispondere alle minacce con maggiore visibilità e controllo sulla sicurezza delle risorse di Azure. Offre funzionalità integrate di monitoraggio della sicurezza e gestione dei criteri tra le sottoscrizioni di Azure, facilita il rilevamento delle minacce che altrimenti passerebbero inosservate e funziona con un ampio ecosistema di soluzioni di sicurezza.

Il Centro sicurezza analizza lo stato di sicurezza delle risorse di Azure per identificare le potenziali vulnerabilità di sicurezza. Un elenco di suggerimenti illustra in dettaglio il processo di configurazione dei controlli necessari.

Tra gli esempi sono inclusi:

- Provisioning di antimalware per identificare e rimuovere il software dannoso

- Configurazione dei gruppi di sicurezza di rete e delle regole per controllare il traffico verso le VM

- Provisioning di un Web application firewall per la difesa da attacchi diretti alle applicazioni Web

- Distribuzione degli aggiornamenti di sistema mancanti

- Risoluzione delle configurazioni del sistema operativo che non corrispondono alle baseline consigliate

Il Centro sicurezza raccoglie, analizza e integra automaticamente i dati di log dalle risorse di Azure, dalla rete e dalle soluzioni dei partner, ad esempio programmi antimalware e firewall. Quando vengono rilevate minacce, viene creato un avviso di sicurezza. Ad esempio, è compreso il rilevamento di:

- VM compromesse in comunicazione con indirizzi IP dannosi noti

- Malware avanzato rilevato tramite Segnalazione errori Windows

- Attacchi di forza bruta alle VM

- Avvisi di sicurezza da programmi antimalware e firewall integrati

### <a name="azure-monitor"></a>Monitoraggio di Azure

[Monitoraggio di Azure](../../azure-monitor/overview.md) fornisce puntatori a informazioni su tipi di risorse specifici. Offre anche funzionalità di visualizzazione, query, routing, avviso, ridimensionamento automatico e automazione dei dati sia dall'infrastruttura di Azure (log attività) che da ogni singola risorsa di Azure (log di diagnostica).

Le applicazioni cloud sono complesse e hanno molte parti mobili. Il monitoraggio offre la possibilità di garantire il funzionamento e l'integrità dell'applicazione. Consente anche di prevenire i problemi potenziali o di risolvere quelli precedenti.

![Diagramma che illustra come usare i dati di monitoraggio per ottenere informazioni approfondite sull'applicazione.](./media/technical-capabilities/azure-security-technical-capabilities-fig10.png)
Inoltre, è possibile usare i dati di monitoraggio per ottenere informazioni approfondite sull'applicazione, utili per migliorarne le prestazioni o la manutenibilità oppure per automatizzare azioni che altrimenti richiederebbero un intervento manuale.

Il controllo della sicurezza della rete è fondamentale per rilevare le vulnerabilità e garantire la conformità con il modello di governance normativo e della sicurezza IT. Con la visualizzazione Gruppo di sicurezza, è possibile recuperare il gruppo di sicurezza di rete e le regole di sicurezza configurati, nonché le regole di sicurezza effettive. Con l'elenco delle regole applicate, è possibile determinare le porte aperte e valutare la vulnerabilità della rete.

### <a name="network-watcher"></a>Network Watcher

[Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) è un servizio a livello di area che permette di monitorare e diagnosticare le condizioni al livello di rete da, verso e in Azure. Gli strumenti di visualizzazione e diagnostica di rete disponibili in Network Watcher permettono di comprendere, diagnosticare e ottenere informazioni dettagliate sulla rete in Azure. Il servizio include l'acquisizione pacchetti, l'hop successivo, la verifica del flusso IP, la visualizzazione dei gruppi di sicurezza e i log dei flussi dei gruppi di sicurezza di rete. A differenza del monitoraggio a livello di singole risorse di rete, il monitoraggio a livello di scenario consente una visualizzazione completa delle risorse di rete.

### <a name="storage-analytics"></a>Analisi archiviazione

[Analisi archiviazione](/rest/api/storageservices/fileservices/storage-analytics) possibile archiviare metriche che includono statistiche sulle transazioni aggregate e dati sulla capacità relativi alle richieste a un servizio di archiviazione. Le transazioni vengono segnalate sia a livello di operazione API, sia a livello di servizio di archiviazione, mentre la capacità viene segnalata a livello di servizio di archiviazione. I dati delle metriche possono essere utilizzati per analizzare l'uso del servizio di archiviazione, diagnosticare i problemi relativi alle richieste effettuate al servizio di archiviazione e per migliorare le prestazioni delle applicazioni che usano un servizio.

### <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) è un servizio estendibile di gestione delle prestazioni delle applicazioni (APM) per sviluppatori Web su più piattaforme. che consente di monitorare un'applicazione Web live. Il servizio rileva automaticamente le anomalie nelle prestazioni e include avanzati strumenti di analisi che consentono di diagnosticare i problemi e conoscere come viene usata l'app dagli utenti. Il servizio è progettato per supportare il miglioramento continuo delle prestazioni e dell'usabilità. Funziona per le app su un'ampia gamma di piattaforme, tra cui .NET, Node.js e Java EE, ospitate in locale o nel cloud. Si integra con il processo DevOps e offre punti di connessione per diversi strumenti di sviluppo.

Esegue il monitoraggio di:

- **Frequenza delle richieste, tempi di risposta e percentuali di errore** : trovare le pagine più visitate, gli orari di visita e la posizione degli utenti. Vedere quali pagine abbiano prestazioni migliori. Se i tempi di risposta e le percentuali di errore aumentano di pari passo con le richieste, è probabile che ci sia un problema di assegnazione delle risorse.

- **Tassi di dipendenza, tempi di risposta e percentuali di errore** : trovare quali servizi esterni causino un rallentamento.

- **Eccezioni** : analizzare le statistiche aggregate o selezionare istanze specifiche e approfondire l'analisi dello stack e le richieste correlate. Vengono segnalate eccezioni di server e browser.

- **Visualizzazioni pagina e prestazioni di carico** , segnalate dai browser degli utenti.

- **Chiamate AJAX da pagine Web** : tariffe, tempi di risposta e percentuali di errori.

- **Conteggi di utenti e sessioni.**

- **Contatori delle prestazioni** dai computer server Windows o Linux, ad esempio l'uso di CPU, memoria e rete.

- **Diagnostica dell'host** da Docker o Azure.

- **Log di traccia di diagnostica** dall'app, in modo da poter correlare gli eventi di traccia con le richieste.

- **Metriche ed eventi personalizzati** scritti personalmente nel codice del client o del server, per tenere traccia di eventi aziendali come gli articoli venduti o i giochi vinti.

L'infrastruttura per l'applicazione è in genere costituita da vari componenti, ad esempio una macchina virtuale, un account di archiviazione e una rete virtuale oppure un'app Web, un database, un server di database e servizi di terze parti. Questi componenti non appaiono come entità separate, ma come parti correlate e interdipendenti di una singola entità e devono essere distribuite, gestite e monitorate come gruppo. [Azure Resource Manager](../../azure-resource-manager/management/overview.md) consente di usare le risorse nella soluzione come gruppo.

È possibile distribuire, aggiornare o eliminare tutte le risorse della soluzione con un'unica operazione coordinata. Per la distribuzione viene usato un modello; questo modello può essere usato per diversi ambienti, ad esempio di testing, staging e produzione. Gestione risorse offre funzionalità di sicurezza, controllo e categorizzazione che semplificano la gestione delle risorse dopo la distribuzione.

**Vantaggi dell'utilizzo di Gestione risorse**

Gestione risorse offre numerosi vantaggi:

- È possibile distribuire, gestire e monitorare tutte le risorse per la soluzione come un gruppo, anziché gestire singolarmente tali risorse.

- È possibile distribuire ripetutamente la soluzione nel corso del ciclo di vita dello sviluppo garantendo al contempo che le risorse vengano distribuite in uno stato coerente.

- È possibile gestire l'infrastruttura con modelli dichiarativi, piuttosto che con script.

- È possibile definire le dipendenze tra le risorse in modo che vengano distribuite nell'ordine corretto.

- è possibile applicare il controllo di accesso a tutti i servizi nel gruppo di risorse perché il controllo di accesso basato sui ruoli (RBAC) è integrato in modo nativo nella piattaforma di gestione.

- È possibile applicare tag alle risorse per organizzare in modo logico tutte le risorse nella sottoscrizione.

- È possibile ottenere informazioni dettagliate sulla fatturazione per l'organizzazione visualizzando i costi di un gruppo di risorse che condividono lo stesso tag.

> [!Note]
> Gestione risorse offre un nuovo modo per distribuire e gestire le soluzioni. Se è stato usato il modello di distribuzione precedente e si desidera ottenere informazioni sulle modifiche, vedere [informazioni sulla distribuzione di gestione risorse e distribuzione classica](../../azure-resource-manager/management/deployment-models.md).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla sicurezza, vedere alcuni degli approfondimenti sull'argomento:

- [Controllo e registrazione](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Crimine informatico](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Progettazione e sicurezza operativa](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Crittografia](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Gestione delle identità e degli accessi](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Sicurezza di rete](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Gestione delle minacce](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)