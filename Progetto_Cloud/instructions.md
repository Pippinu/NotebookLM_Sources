1. Introduzione al Progetto
Il presente documento delinea la progettazione e l'implementazione di un'applicazione serverless su Amazon Web Services (AWS) per la generazione di immagini tramite un modello di Stable Diffusion. L'obiettivo principale è dimostrare una solida comprensione delle architetture cloud native, delle best practice per la scalabilità e dell'ottimizzazione delle performance, come richiesto dalla valutazione del corso (rif. "Final Project.pdf" e "Performance evaluation.pdf").

Il progetto si basa sulla seguente architettura:

API Gateway: Per gestire le richieste API RESTful degli utenti.
AWS Lambda: Per l'esecuzione del codice di generazione immagini, ospitando un contenitore Docker con il modello Stable Diffusion.
Amazon S3: Per lo storage durabile e scalabile delle immagini generate.
Amazon DynamoDB: Per la persistenza dei metadati associati alle immagini (prompt testuale, URL S3).
Meccanismo di Cache per la Similarità Testuale: Una componente innovativa per ottimizzare le performance, evitando rigenerazioni ridondanti di immagini per prompt semanticamente simili.
Questo approccio mira a presentare una soluzione all'avanguardia che non solo soddisfa i requisiti funzionali, ma eccelle anche nella sua robustezza architetturale, scalabilità e gestione efficiente delle risorse.

2. Architettura Dettagliata del Sistema
L'architettura proposta sfrutta i servizi serverless di AWS per garantire elevata disponibilità, scalabilità automatica e un modello di costo "pay-per-use".

Diagramma Architetturale (Concept - da disegnare a parte)

Utente --> API Gateway --> AWS Lambda (con Docker Stable Diffusion)
                                |       ^
                                |       | (Vettorizzazione/Similarità)
                                V       |
                                S3 (Immagini)
                                |
                                V
                                DynamoDB (Metadati + Vettori di Similarità)
Componenti AWS Utilizzati:

AWS API Gateway:

Funzione: Agisce come il "front-door" dell'applicazione, gestendo tutte le richieste HTTP in ingresso.
Configurazione: Sarà configurato un endpoint RESTful (es., /generate-image) che accetterà richieste POST contenenti il prompt testuale dell'utente nel payload JSON.
Integrazione: Direttamente integrato con la funzione AWS Lambda, fungendo da proxy per le richieste.
Validazione: Implementeremo la validazione dello schema per le richieste in ingresso, garantendo che il formato del prompt sia corretto.
Error Handling: Mappature per gestire e presentare errori in modo user-friendly.
AWS Lambda (con Contenitore Docker di Stable Diffusion):

Core del Sistema: La funzione Lambda eseguirà il codice di generazione delle immagini. La scelta di utilizzare un'immagine Docker su Lambda è cruciale per la gestione delle dipendenze complesse e dei modelli di grandi dimensioni tipici delle applicazioni ML/AI, superando i limiti tradizionali dei layer Lambda.
Contenitore Docker: L'immagine Docker conterrà:
Un'immagine base leggera (es., python:3.x-slim-buster).
Il modello Stable Diffusion (o una sua versione ottimizzata/quantizzata) pre-scaricato e incluso per ridurre i cold start.
Tutte le librerie Python necessarie (es., diffusers, transformers, torch, Pillow, boto3).
Il codice applicativo Python per:
Ricevere il prompt dall'evento Lambda.
Invocare il modello Stable Diffusion per generare l'immagine.
Salvare l'immagine generata temporaneamente nel filesystem di Lambda (/tmp).
Caricare l'immagine su S3.
Generare un embedding vettoriale del prompt per la similarità testuale.
Salvare i metadati (prompt originale, URL S3 dell'immagine, timestamp, vettore di similarità) su DynamoDB.
Restituire l'URL dell'immagine S3 e il prompt come risposta all'API Gateway.
Configurazione Lambda: Memoria allocata elevata (es., 3-4 GB o più, a seconda dei test) per garantire performance adeguate al modello ML. Timeout generoso (es., 60-90 secondi) per accomodare il tempo di inferenza.
Ottimizzazione Container: Strategie per ridurre al minimo la dimensione dell'immagine Docker e ottimizzare il caricamento del modello per minimizzare i cold start.
Amazon S3 (Simple Storage Service):

Storage Immagini: Un bucket S3 dedicato per archiviare le immagini generate. S3 offre durabilità, scalabilità illimitata e accesso a basso costo.
Permissions: Politiche di bucket e ruoli IAM configurati per permettere alla funzione Lambda di scrivere le immagini e agli utenti di leggerle (se necessario, con URL pre-firmati o policy pubbliche limitate).
Amazon DynamoDB:

Database NoSQL: Tabella per archiviare i metadati delle immagini generate.
Schema: La tabella avrà attributi come:
ImageID (Chiave Primaria, UUID per univocità).
Prompt (Stringa, il testo originale usato per la generazione).
ImageUrl (Stringa, l'URL S3 dell'immagine generata).
Timestamp (Numerico, momento della generazione).
SimilarityVector (Lista di numeri, il vettore di embedding del prompt per la cache).
Scalabilità: DynamoDB si adatta automaticamente ai carichi di lavoro (modalità on-demand) o può essere provisioned per carichi prevedibili.
Cache per Similarità Testuale (Implementazione Innovativa):

Obiettivo: Ridurre il tempo di risposta e il carico computazionale della Lambda evitando di generare nuovamente immagini per prompt semanticamente simili.
Meccanismo:
Quando un prompt arriva, la Lambda calcola il suo embedding vettoriale (una rappresentazione numerica della semantica del testo) usando un modello NLP (es., Sentence Transformers).
Questo vettore viene confrontato (tramite cosine similarity) con i vettori SimilarityVector già memorizzati in DynamoDB.
Se viene trovata una corrispondenza con un grado di similarità superiore a una soglia predefinita (es., 0.9), l'URL dell'immagine preesistente viene recuperato da DynamoDB e restituito immediatamente.
Se nessuna corrispondenza significativa è trovata, l'immagine viene generata e i nuovi metadati, incluso il nuovo SimilarityVector, vengono salvati in DynamoDB.
Benefici: Significativo miglioramento della latenza per le richieste "cache-hit" e riduzione dei costi operativi di Lambda (meno inferenze ML).
3. Implementazione per il 30L
Per raggiungere il massimo dei voti (30L), l'implementazione deve essere non solo funzionale ma anche ottimizzata e documentata in termini di performance.

Ottimizzazione del Contenitore Docker:
Minimalismo: Utilizzare un'immagine base Linux (es. Alpine o slim-buster) per ridurre la dimensione.
Multi-stage Builds: Se necessario, per installare dipendenze e poi copiare solo l'essenziale nel runtime finale.
Cache del Modello: Includere il modello Stable Diffusion direttamente nell'immagine Docker (se di dimensioni gestibili da Lambda, max 10GB per container) per minimizzare il tempo di caricamento (cold start).
Logica della Cache di Similarità:
Scelta del Modello di Embedding: Valutare modelli leggeri ed efficienti per la vettorizzazione testuale.
Soglia di Similarità: Sperimentare e definire una soglia ottimale per la cosine similarity per bilanciare accuratezza e hit rate della cache.
Gestione Cold Start: Se il modello di embedding è anch'esso pesante, assicurarsi che venga caricato efficientemente all'avvio della Lambda.
Gestione degli Errori e Logging:
Implementare un robusto try-except block all'interno della Lambda.
Utilizzare CloudWatch Logs per registrare eventi, errori e informazioni di debug.
Mappature di errore sull'API Gateway per fornire risposte chiare agli utenti in caso di problemi interni.
4. Metriche di Performance e Scalabilità
La valutazione delle performance è un pilastro fondamentale di questo progetto, come indicato nel documento "Performance evaluation.pdf".

Strumenti di Monitoraggio AWS CloudWatch:

Lambda: Monitoreremo Invocations (numero di chiamate), Errors (tasso di errore), Duration (tempo di esecuzione, con attenzione ai cold start e warm start), e Throttles (invocazioni rifiutate per limiti di concorrenza).
API Gateway: Count (numero di richieste), Latency (tempo di risposta end-to-end), 4XXError (errori client), 5XXError (errori server).
DynamoDB: ReadCapacityUnits e WriteCapacityUnits (utilizzo delle risorse), SuccessfulRequestLatency (latenza delle operazioni sul database).
S3: BucketSizeBytes e NumberOfObjects per monitorare l'utilizzo dello storage.
Custom Metrics: Se necessario, potremmo implementare metriche custom per tracciare specifici aspetti della logica di cache (es., CacheHitCount, CacheMissCount).
Strumenti di Load Testing:

JMeter / Locust / Artillery: Utilizzeremo uno di questi strumenti per generare carichi di lavoro simulati sull'API Gateway.
Scenari di Test:
Baseline Test: Poche richieste per misurare la latenza in condizioni di basso carico e identificare i tempi di cold start della Lambda.
Constant Load Test: Un numero fisso di richieste al secondo per un periodo prolungato per valutare la stabilità del sistema e la sua capacità di mantenere la performance sotto carico costante.
Ramp-up Test: Aumento graduale del numero di richieste al secondo per osservare come il sistema scala, quando si verificano throttling o degrado delle performance.
Peak Load Test: Un picco elevato di richieste in breve tempo per simulare un "flash crowd" e testare la resilienza.
Cache Effectiveness Test: Scenari con un'alta percentuale di prompt simili a quelli già processati, confrontando la latenza e il consumo di risorse con scenari di "cache miss".
Analisi dei Risultati:

Presentazione di grafici chiari e intuitivi che mostrano l'andamento delle metriche chiave sotto diversi carichi.
Discussione dettagliata del comportamento del sistema: come la Lambda scala, l'impatto dei cold start, la latenza di DynamoDB e S3, e soprattutto, il beneficio quantificabile della cache di similarità in termini di riduzione della latenza e del carico sulla funzione Lambda.
Identificazione di potenziali colli di bottiglia e discussione di possibili miglioramenti futuri.
5. Report e Presentazione
Il report scritto e la presentazione orale saranno strutturati per evidenziare i punti di forza del progetto e l'analisi critica delle performance.

Report Scritto:

Introduzione: Contesto del progetto, obiettivi e panoramica della soluzione.
Architettura della Soluzione: Diagramma architetturale dettagliato e descrizione di ogni componente AWS con le motivazioni delle scelte di design.
Dettagli Implementativi: Focus sulla logica della funzione Lambda, l'integrazione del container Docker di Stable Diffusion, la gestione dei dati su S3 e DynamoDB, e l'implementazione della logica di cache. Includerà frammenti di codice pertinenti.
Setup Sperimentale: Descrizione degli strumenti di test utilizzati, dei carichi di lavoro simulati e delle metriche raccolte.
Risultati Sperimentali e Analisi: La sezione più importante. Presentazione di grafici da CloudWatch e dai tool di load testing, con un'analisi approfondita del comportamento del sistema sotto carico e dell'impatto della cache.
Conclusioni, Limitazioni e Lavori Futuri: Riepilogo degli apprendimenti chiave, identificazione delle limitazioni attuali e proposte per futuri sviluppi.
Best Practices: Evidenziazione delle best practice AWS adottate (sicurezza IAM, ottimizzazione costi, scalabilità serverless).
Presentazione Orale (15 minuti + 5 minuti Q&amp;A):

Slide concise e visuali.
Focus su: Problema -> Soluzione Architetturale -> Dettagli Chiave Implementativi (es., Docker su Lambda, la cache) -> Risultati di Performance e Scalabilità (con grafici e analisi) -> Conclusioni e Futuri Sviluppi.
Breve demo live se possibile.
Preparazione a rispondere a domande critiche sulla scalabilità, i costi e le scelte tecnologiche.
6. Divisione del Lavoro per il Team (3 Persone)
Una divisione chiara delle responsabilità è fondamentale per un lavoro di squadra efficiente.

Membro 1: Il "Cloud Architect" / Gestore Infrastruttura & API Gateway

Responsabilità: Setup generale dell'ambiente AWS Learner Lab, configurazione di API Gateway (endpoint, validazioni, integrazione Lambda), creazione e gestione di S3 Bucket e DynamoDB Table, definizione e gestione dei Ruoli IAM, setup iniziale di CloudWatch Monitoring, coordinamento generale e integrazione.
Obiettivo: Fornire un'infrastruttura AWS stabile e configurata correttamente, con API accessibile.
Membro 2: Il "Backend Developer" / Lambda & Integrazione Data Stores

Responsabilità: Sviluppo del codice Python della funzione Lambda (logica core, gestione eventi), integrazione con S3 per l'upload delle immagini, integrazione con DynamoDB per la scrittura dei metadati. Gestione degli errori all'interno della Lambda e formattazione delle risposte per l'API Gateway.
Obiettivo: Garantire il corretto funzionamento della logica applicativa principale e la persistenza dei dati.
Membro 3: Il "Machine Learning Engineer" / Cache & Ottimizzazione Performance

Responsabilità: Creazione e ottimizzazione dell'immagine Docker per Stable Diffusion (scelta modello, riduzione dimensioni), implementazione della logica di vettorizzazione testuale (embedding) e della cache di similarità all'interno della Lambda. Sviluppo ed esecuzione degli script di load testing, raccolta e analisi dei dati di performance, preparazione dei grafici e della sezione "Risultati Sperimentali" del report e della presentazione.
Obiettivo: Assicurare l'efficienza computazionale del modello, la funzionalità della cache e dimostrare empiricamente la scalabilità e le performance del sistema.
7. Strategie per Lavorare in Concorrenza (AWS Learner Lab)
Per massimizzare l'efficienza e minimizzare i conflitti in un ambiente condiviso come AWS Learner Lab:

Comunicazione Continua:
Daily Stand-ups: Brevi meeting giornalieri per sincronizzarsi, discutere progressi e blocchi.
Canale di Messaggistica: Un canale dedicato (es., WhatsApp, Slack) per comunicazioni rapide.
Controllo Versione (Git):
Repository Condiviso: Utilizzo di un sistema di controllo versione (GitHub, GitLab) con branch dedicati per le feature.
Merge Frequenti: Integrare il codice frequentemente sul branch main o dev per identificare e risolvere i conflitti rapidamente.
Commit Descrittivi: Messaggi di commit chiari per tracciare le modifiche.
Gestione delle Risorse AWS (Cruciale per Learner Lab):
Coordinamento delle Modifiche: Comunicare sempre prima di apportare modifiche a risorse AWS condivise (API Gateway, S3, DynamoDB, IAM Roles) per evitare sovrascritture o interruzioni.
Test Locali: Incoraggiare il testing di singoli componenti in locale (es. container Docker, logica Lambda) prima del deployment su AWS per ridurre la contesa sull'ambiente di test.
Documentazione Condivisa: Un documento centrale per le decisioni architetturali, i nomi delle risorse AWS, le configurazioni chiave e le istruzioni operative.
Preparazione del Report in Parallelo: Iniziare a redigere il report e le slide della presentazione man mano che il progetto avanza, dividendo le sezioni in base alle responsabilità.