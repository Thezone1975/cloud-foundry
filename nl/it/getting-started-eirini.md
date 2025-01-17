---

copyright:
  years: 2017, 2018
lastupdated: "2019-04-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Introduzione all'anteprima tecnica Eirini 
{: #getting-started-eirini}

CFEE ({{site.data.keyword.cfee_full}}) è una piattaforma cloud per ospitare applicazioni e servizi nel cloud. {{site.data.keyword.cfee_full_notm}} rende più semplice ridimensionare le applicazioni man mano che il consumo cresce, semplificando il runtime, il middleware e l'infrastruttura in modo che ti puoi concentrare sullo sviluppo delle applicazioni.
{: shortdesc}

Questa esercitazione introduttiva mostra come creare un {{site.data.keyword.cfee_full_notm}} dal piano dell'anteprima tecnica Eirini, aggiungere utenti, creare un'organizzazione e gli spazi, distribuire le applicazioni e associare queste applicazioni ai servizi.
Vedi il [progetto Eirini](https://www.cloudfoundry.org/project-eirini/) per informazioni sul progetto incubatore Eirini dalla Cloud Foundry Foundation.

## Informazioni sulle autorizzazioni
{: #permissions}

Hai bisogno delle politiche di accesso corrette prima di creare delle istanze dell'{{site.data.keyword.cfee_full_notm}}. Per ulteriori informazioni, consulta [Autorizzazioni](https://cloud.ibm.com/docs/cloud-foundry/permissions.html).

## Passo 1: assicurati che l'account {{site.data.keyword.Bluemix_notm}} possa creare risorse dell'infrastruttura
{: #accountprep-environment}

Le istanze CFEE vengono distribuite nei nodi di lavoro Kubernetes dal servizio Kubernetes.  [Prepara il tuo account IBM Cloud](https://cloud.ibm.com/docs/cloud-foundry/prepare-account.html) per verificare di poter creare le risorse dell'infrastruttura necessarie per un'istanza CFEE.

## Passo 2: crea la tua istanza CFEE
{: #create-environment}

Prima di creare il tuo CFEE, assicurati di essere nell'account {{site.data.keyword.Bluemix_notm}} in cui vuoi creare l'ambiente e di disporre delle politiche di accesso necessarie (passo 1 in alto).

1.  Apri il {{site.data.keyword.Bluemix_notm}} catalogo [](https://cloud.ibm.com/catalog).

2.  Individua il servizio {{site.data.keyword.cfee_full_notm}} nel catalogo e fai clic su di esso per aprirne la pagina della panoramica.  Tale prima pagina fornisce una panoramica delle funzioni principali del servizio. Fai clic su **Continue**.

3.  Configura l'istanza CFEE da creare:
    * Seleziona il piano _Anteprima tecnica Eirini_.
    * Immetti un **Nome** per l'istanza CFEE.
    * Seleziona un **Gruppo di risorse** nel quale viene raggruppato l'ambiente. Solo i gruppi di risorse a cui hai accesso nell'account IBM Cloud corrente verranno elencati nell'elenco a discesa _Gruppi di risorse_, il che significa che devi disporre dell'autorizzazione per accedere ad almeno un gruppo di risorse nell'account per poter creare un CFEE.
    * Seleziona l'**Area geografica** e l'**Ubicazione** in cui deve essere eseguito il provisioning dell'istanza del servizio. Vedi l'elenco di [data center e ubicazioni di provisioning disponibili](https://cloud.ibm.com/docs/cloud-foundry/index.html#provisioning-targets){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") in base all'area geografica per CFEE e i servizi di supporto. 

4. Viene eseguito il provisioning delle istanze CFEE su un cluster Kubernetes. Viene eseguito il provisioning delle celle Cloud Foundry all'interno delle zone di lavoro nel cluster Kuberentes. Puoi configurare l'ubicazione, l'hardware e la crittografia del cluster:
    * Seleziona l'isolamento hardware per il cluster Kubernetes:   
      * _Condiviso virtuale_: le risorse dell'infrastruttura nel cluster, ad esempio l'hypervisor e l'hardware fisico, vengono distribuite tra te e gli altri clienti IBM, ma ogni nodo di lavoro è un tuo singolo tenant.
      * _Dedicato virtuale_: i nodi di lavoro nel cluster sono ospitati sull'infrastruttura dedicata al tuo account.
    * I dati nel cluster sono crittografati per impostazione predefinita. Hai la possibilità di disattivare la _Crittografa disco locale_.
    * Seleziona l'**Area geografica** e la **Regione** in cui sarà eseguito il provisioning del cluster Kubernetes.
    * Seleziona la **Zona** in cui saranno distribuiti i nodi di lavoro Kubernetes. Le VLAN disponibili vengono automaticamente richiamate per le zone selezionate. Se non è disponibile alcuna VLAN, saranno create automaticamente.
    A partire da CFEE v2.2.0, le istanze CFEE possono operare dietro a una **rete isolata**. Puoi eseguire il provisioning di un'istanza CFEE in una rete isolata selezionando le VLAN private nella rete isolata. Dopo aver creato il CFEE, gli indirizzi IP per le istanze Compose for PostgreSQL e Cloudant Object Storage di CFEE, nonché i programmi di bilanciamento del carico dell'applicazione ([ALB](https://cloud.ibm.com/docs/containers/cs_ingress.html#private_ingress)) del cluster Kubernetes, devono essere instradati nella rete isolata dall'amministratore della rete per il CFEE per diventare operativi.
    
    Dopo aver creato il CFEE, verrà visualizzato il cluster Kubernetes in cui è stato eseguito il provisioning dell'ambiente, (come qualsiasi altra risorsa nel tuo account IBM Cloud) nel [dashboard](https://cloud.ibm.com/dashboard/apps/) {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, vedi la [documentazione del servizio Kubernetes](https://cloud.ibm.com/docs/containers/cs_why.html#cs_ov).

5.  Configura la capacità del CFEE:
    * Seleziona il **Numero di celle** per il CFEE. Queste sono le celle dell'applicazione che ospiteranno le applicazioni distribuite nel CFEE.  
    * Seleziona la **Dimensione del nodo**, che determina la capacità delle celle Cloud Foundry (CPU e memoria).
    
    Oltre alle celle dell'applicazione che hai precedentemente specificato, vengono create e riservate ulteriori _celle del piano di controllo_ per l'operatività e il controllo della piattaforma Cloud Foundry nel tuo CFEE. 

    **Nota:** ti consigliamo che lo spanning della VLAN sia abilitato se viene eseguito il provisioning dei nodi di lavoro nel cluster Kubernetes su sottoreti diverse.  I nodi di lavoro su sottoreti differenti potrebbero impedire l'interconnettività se lo spanning della VLAN è disabilitato.  Per ulteriori informazioni, vedi la documentazione dello [spanning della VLAN](https://cloud.ibm.com/docs/containers/cs_subnets.html#vlan-spanning).
    
6.  Nei campi **Compose for PostgreSQL**, seleziona una delle organizzazioni pubbliche e seleziona quindi uno degli spazi disponibili in tale organizzazione. Verrà eseguito il provisioning dell'istanza di Compose for PostgreSQL nello spazio selezionato. Il servizio Compose for PostgreSQL è una dipendenza obbligatoria del servizio CFEE.

    **Nota:** in _Compose for PostgreSQL_ solo le organizzazioni nell'ubicazione in cui intendi eseguire il provisioning dell'istanza CFEE (passo 4 in alto) e a cui hai accesso sono disponibili per la selezione.  All'interno di una specifica organizzazione, sono disponibili per la selezione solo gli spazi a cui hai un accesso di _sviluppatore_. 

7.  Il **Riepilogo ordine** sul lato destro della pagina riflette il costo dell'istanza CFEE e dei servizi ausiliari insieme al totale mensile stimato.

8.  Fai clic su **Crea**, che apre il dashboard dell'ambiente e indica lo stato e l'avanzamento della creazione.

**Nota:** una volta avviato il processo di creazione, viene visualizzata una riga tabella per il CFEE nel dashboard {{site.data.keyword.Bluemix_notm}}, nell'_Elenco risorse_ e nel [dashboard Ambienti Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry?filter=cf_environments).  Lo stato nella riga tabella indica quando è completata la creazione.

Il processo automatizzato che crea l'ambiente distribuisce l'infrastruttura in un cluster Kubernetes e la configura per renderla pronta per l'uso. Il processo dura 90 - 120 minuti.

Dopo che il CFEE è stato creato, riceverai diverse email che confermano il provisioning del CFEE e dei servizi di supporto, nonché delle email che ti informano dello stato degli ordini corrispondenti.

Quando crei un'istanza CFEE, nello stesso account IBM Cloud vengono create tre istanze del servizio di supporto aggiuntive. Tali istanze del servizio di supporto prendono il nome da quello dell'istanza CFEE. Pertanto, la creazione di un CFEE determina la creazione di un totale di quattro istanze del servizio nell'account IBM Cloud:
* Istanza CFEE ("_cfeename_").
* Cluster Kubernetes ("_cfeename_-cluster"). Il cluster fornisce l'infrastruttura in cui viene eseguito il provisioning dell'istanza CFEE.
* Istanza Cloud Object Storage ("_cfeename_-cos"). L'istanza viene utilizzata per archiviare i dati generati durante la creazione dei contenitori dell'applicazione CFEE (ad esempio, pacchetti di applicazioni caricati, pacchetti di build ed eseguibili compilati).
* Istanza Compose for PostgreSQL ("_cfeename_-postgres"). L'istanza viene utilizzata per archiviare i dati Cloud Foundry nell'istanza CFEE (ad esempio controllo della distribuzione delle applicazioni e degli avvii e arresti degli eventi, conservazione dei record di connessioni di servizi, applicazioni, spazi, organizzazioni e appartenenze degli utenti CFEE). 

## Passo 3: crea organizzazioni e spazi
{: #create-orgsandspaces}

Dopo aver creato l'{{site.data.keyword.cfee_full_notm}}, consulta [Creazione di organizzazioni e spazi](https://cloud.ibm.com/docs/cloud-foundry/orgs-spaces.html) per informazioni su come strutturare l'ambiente creando le organizzazioni e gli spazi. Le applicazioni in un {{site.data.keyword.cfee_full_notm}} vengono indirizzate all'interno di spazi specifici. A sua volta, uno spazio si trova all'interno di un'organizzazione specifica. I membri di un'organizzazione condividono un piano di quota, le applicazioni, le istanze dei servizi e i domini personalizzati.

Quando crei un'organizzazione assegni una quota ad essa.  La quota imposta i limiti sulle risorse (memoria, cpu, ecc.) disponibili per tale organizzazione. Puoi assegnare una quota differente in un momento successivo. Esiste una serie di quote preconfigurate disponibili in ogni CFEE, ma puoi anche creare le tue quote personalizzate nella pagina **Quote** dell'interfaccia utente CFEE.  Puoi rendere una quota personalizzata la quota _Predefinita_ richiamando l'azione _Copia come predefinita_ dal menu delle quote, che copia i valori della quota personalizzata in quella predefinita.

**Nota:** la pagina _Organizzazioni Cloud Foundry_ disponibile dal menu **_Gestisci > Account > Organizzazioni Cloud Foundry_** situata nella parte superiore dell'intestazione di IBM Cloud è destinata esclusivamente alle organizzazioni pubbliche di IBM Cloud, **non alle organizzazioni CFEE**. Le organizzazioni CFEE vengono gestite all'interno della pagina _Organizzazioni_ di un'istanza CFEE.  Apri l'interfaccia utente CFEE e fai clic sulla pagina **_Organizzazioni_** per creare e gestire le organizzazioni per tale CFEE.

## Passo 4: aggiungi utenti a organizzazioni e spazi
{: #add-users}

[Aggiungi gli utenti](https://cloud.ibm.com/docs/cloud-foundry/add-users.html) a tali organizzazioni e spazi in specifiche assegnazioni di ruoli che controllano il loro livello di accesso.  Prima che gli utenti possano essere aggiunti come membri di organizzazioni e spazi in un CFEE, è necessario che tali utenti abbiano un accesso precedente all'istanza CFEE. Consulta [Autorizzazioni](https://cloud.ibm.com/docs/cloud-foundry/permissions.html).

## Passo 5: distribuisci e visualizza applicazioni
{: #deploy-apps}

Quando sei pronto, puoi [distribuire l'applicazione](https://console.bluemix.net/docs/cloud-foundry/deploy-apps.html) con l'interfaccia riga di comando (CLI) {{site.data.keyword.Bluemix_notm}}.  Visualizza l'elenco delle applicazioni distribuite nell'interfaccia utente, nel contesto di uno spazio CFEE specifico o globalmente in tutte le istanze CFEE.  Puoi anche avviare, arrestare o eliminare le applicazioni da quelle viste.

## Passo 6: crea o aggiungi istanze del servizio IBM Cloud a spazi CFEE
{: #service-instances}

[Crea i servizi](https://console.bluemix.net/docs/cloud-foundry/add-serv-inst.html#workingwith-services#creating-services-inspace) oppure [Aggiungi dei servizi esistenti](https://console.bluemix.net/docs/cloud-foundry/add-serv-inst.html#workingwith-services#adding-services-inspace) disponibili nell'account IBM Cloud.  Una volta che un'istanza del servizio è disponibile in uno spazio CFEE, puoi eseguirne il bind (associazione) alle applicazioni CFEE distribuite in tale spazio.

## Passo 7: associa mediante bind applicazioni a istanze del servizio
{: #bind-apps}

[Associa la tua applicazione](https://console.bluemix.net/docs/cloud-foundry/binding.html) a un alias dell'istanza del servizio per poter utilizzare le funzioni del servizio.

Nel [dashboard Cloud Foundry](https://console.bluemix.net/dashboard/cloudfoundry/overview){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") puoi vedere una vista consolidata di tutte le applicazioni e tutti i servizi, sia quelli *pubblici* che quelli *aziendali*.  Una volta all'interno del dashboard Cloud Foundry, fai clic su *Pubblico* nel riquadro di navigazione a sinistra per visualizzare le applicazioni e i servizi pubblici disponibili nell'account IBM Cloud.  Fai clic su *Azienda* per visualizzare tutti gli ambienti CFEE, le applicazioni distribuite negli spazi CFEE e i servizi disponibili agli spazi CFEE.
{:tip}

## Passo 8: installa la console Stratos per gestire le applicazioni (facoltativo)
{: #install-stratos}

La console Stratos è uno strumento open source basato sul web per lavorare con Cloud Foundry. L'applicazione della console Stratos può essere facoltativamente installata e utilizzata in un ambiente CFEE specifico per gestirne le organizzazioni, gli spazi e le applicazioni.

Gli utenti con i ruoli di editor e amministratore IBM Cloud nell'istanza CFEE possono installare l'applicazione della console Stratos in tale istanza CFEE.

Per installare l'applicazione della console Stratos:

1. Apri l'istanza CFEE in cui vuoi installare la console Stratos.
2. Fai clic su **Installa Stratos Console** nella pagina della panoramica. Il pulsante è visibile solo agli utenti con autorizzazioni di amministratore o di editor per tale istanza CFEE.
3. Nella finestra di dialogo Installa Stratos Console, seleziona un'opzione di installazione. Puoi installare l'applicazione Stratos in una cella Cloud Foundry o nel cluster Kubernetes. Seleziona una versione della console Stratos e il numero di istanze dell'applicazione da installare. Se installi l'applicazione della console Stratos in una cella Cloud Foundry, ti vengono richiesti l'organizzazione e lo spazio in cui distribuire l'applicazione.
4. Fai clic su **Installa**.

Per installare l'applicazione sono necessari circa 5 minuti. Una volta completata l'installazione, viene visualizzato il pulsante **Stratos Console** al posto del pulsante _Installa Stratos Console_ nella pagina della panoramica. Il pulsante _Stratos Console_ avvia la console ed è disponibile per tutti gli utenti con accesso all'istanza CFEE. Le assegnazioni di appartenenza all'organizzazione e allo spazio possono limitare ciò che un utente può visualizzare e gestire nella console Stratos.

Per avviare la console Stratos:

1. Apri l'istanza CFEE in cui è stata installata la console Stratos.
2. Fai clic su **Stratos Console** nella pagina della panoramica.
3. La console Stratos viene aperta in una scheda del browser separata. Quando apri la console per la prima volta, ti viene richiesto di accettare due avvertenze consecutive per via dei certificati autofirmati.
4. Fai clic su **Login** per aprire la console. Non è richiesta alcuna credenziale poiché l'applicazione utilizza le tue credenziali {{site.data.keyword.Bluemix_notm}}.


## Risorse aggiuntive
{: #additional-resources}

Puoi eseguire alcune attività di gestione in un CFEE utilizzando i comandi CLI `ibmcloud CFEE`. I comandi ti consentono di ottenere informazioni su un'istanza CFEE, così come di gestire le tue organizzazioni e i tuoi spazi. Consulta i [Riferimenti ai comandi IBM Cloud CLI CFEE](https://console.cloud.ibm.com/docs/cli/reference/ibmcloud/cli_cfee.html#ibmcloud_commands_cfee){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").

Puoi trovare i video su discussioni e dimostrazioni approfondite su vari argomenti CFEE in [CFEE video playlist](https://ibm.biz/CFEE-Playlist){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").

Le informazioni sull'API CFEE sono disponibili nella [documentazione dell'API](https://cloud.ibm.com/apidocs/cfaas){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
