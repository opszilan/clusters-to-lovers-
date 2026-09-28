# Clusters to lovers: Trope, TF-IDF e Sentiment Analysis nelle recensioni BookTok di Emily Henry
**Autrice:** Zilan Polat
**Corso:** Tecnologie dei dati e del linguaggio
**Anno Accademico:** 2025/2026
📖 Descrizione del Progetto
Il progetto si propone di valutare se, tramite tecniche di Natural Language Processing (NLP) non supervisionate e basate su regole, sia possibile:
 * Riconoscere i principali trope narrativi più discussi dai lettori nei romanzi romance di Emily Henry (Beach Read, Book Lovers, Happy Place, Funny Story).
 * Misurare in modo affidabile il sentiment espresso nelle recensioni (BookTok).
L'analisi mette a confronto metodi basati su regole (Regex), rappresentazioni vettoriali e clustering (TF-IDF + K-Means) ed estrazione automatica della polarità (VADER Sentiment Analysis + probabilità con Softmax a temperatura variabile).
🛠️ Metodologia
Il flusso di lavoro segue una pipeline strutturata in 5 fasi principali:
[Dataset Sintetico] ➔ [1. Pre-processing & Regex] ➔ [2. TF-IDF] ➔ [3. K-Means] ➔ [4. VADER + Softmax] ➔ [5. Valutazione]

 * Pre-processing e classificazione per trope (Regex):
   * Normalizzazione del testo (lowercase, rimozione di URL, punteggiatura e numeri).
   * Estrazione dei trope tramite espressioni regolari basate su liste di parole chiave:
     * Enemies to lovers: enemies to lovers, hate to love, rivals
     * Second chance: second chance, exes, past relationship
     * Fake dating: fake dating, fake relationship, pretend
     * Banter / Dialogue: banter, witty, dialogue, funny, chemistry
     * Emotional depth: cried, emotional, heartbreaking, vulnerable, depth
 * Estrazione delle Feature (TF-IDF):
   * Vettorizzazione delle recensioni pulite estraendo le prime 50 parole più rilevanti (con rimozione delle stop words inglesi).
 * Clustering non supervisionato (K-Means):
   * Selezione del numero ottimale di cluster K tramite il Metodo del Gomito (Elbow Method) e il punteggio di Silhouette.
   * Scelta di K = 4 per raggruppare le recensioni nello spazio vettoriale TF-IDF.
 * Sentiment Analysis e Softmax:
   * Calcolo del punteggio di sentiment compound mediante il modello VADER.
   * Applicazione della funzione Softmax con temperatura (t) sui punteggi di sentiment per rimappare la distribuzione di probabilità:
     * Temperatura bassa (t = 0.2): distribuzione deterministica centrata sul valore massimo.
     * Temperatura alta (t = 1.5): distribuzione appiattita (maggiore incertezza).
 * Valutazione Statistica:
   * Calcolo della matrice di confusione tra trope reale (ground truth) e trope predetto da Regex.
   * Matrice di correlazione di Pearson tra rating, sentiment e i singoli trope.
📊 Risultati Principali
 * TF-IDF e Lessico Distintivo:
   * Le parole con il punteggio TF-IDF medio più elevato sono risultate essere: emotional, lovers, romance, felt, fake, second, banter, chance, plot, trope.
 * Clustering (K-Means con K=4):
   * Cluster 2 coincide interamente con il trope second chance.
   * Cluster 0 si concentra quasi interamente su fake dating.
   * Essendoci 5 trope e K=4, nei cluster rimanenti (Cluster 1 e 3) alcuni trope sono confluiti insieme (es. banter ed emotional depth).
   * Il clustering è guidato dal lessico condiviso e non dalla polarità: recensioni positive e negative dello stesso trope finiscono nello stesso cluster.
 * Sentiment Analysis:
   * Forte correlazione di Pearson tra il rating assegnato e il sentiment VADER (r = 0.89).
   * I trope sono risultati quasi indipendenti dal sentiment (correlazione massima r = 0.22 per second chance ed emotional depth).
 * Matrice di Confusione (Regex vs Ground Truth):
   * Accuratezza del 100% (diagonale perfetta).
⚠️ Nota Metodologica Importante (Mischia Traccia 10 e Traccia 13 / Valutazione Circolare)
Nel corso dello sviluppo è stato identificato un limite metodologico strutturale fondamentale:
 * Incrocio tra Traccia 10 (Regex/Supervisionato) e Traccia 13 (Clustering/Non Supervisionato): Il progetto ha combinato la classificazione esplicita dei trope tramite regex e sentiment (Traccia 10) con l'analisi non supervisionata di clustering e TF-IDF (Traccia 13).
 * Costruzione del Dataset Sintetico e Circolarità: Per l'esperimento è stato generato un dataset di 270 recensioni sintetiche ottenuto moltiplicando 15 volte un set di 18 recensioni-base (bilanciate tra positive, miste e negative).
 * Valutazione Circolare: Poiché le recensioni sintetiche sono state scritte integrando esplicitamente le stesse parole chiave usate successivamente nelle regole regex per la predizione, l'accuratezza del 100% ottenuta nella matrice di confusione è circolare e non dimostra la capacità del modello di generalizzare su dati reali. Su un corpus reale di BookTok, ironia, slang e l'assenza di keywords esplicite ridurrebbero notevolmente tali prestazioni.
🚀 Limiti e Prossimi Passi
Limiti
 * Dataset sintetico e rigido: Le recensioni seguono un tono coerente per costruzione, senza la variabilità tipica del linguaggio umano.
 * VADER generico: Manca la taratura sul lessico informale di BookTok (slang, emoji, termini come "cried" usati con accezione estremamente positiva).
 * Classificazione ad etichetta singola: L'algoritmo assegna il "primo trope trovato", mentre nei libri romance reale più trope coesistono contemporaneamente.
Prossimi Passi
 * Validare la pipeline su un corpus reale estratto da Goodreads o TikTok (BookTok).
 * Sostituire VADER con un modello di linguaggio (es. BERT) sottoposto a fine-tuning sul dominio del romance.
 * Implementare una classificazione multi-label per intercettare romanzi multi-trope.
 * Confrontare K-Means con algoritmi di clustering gerarchico o basati su sentence embeddings (es. SBERT).
📂 Struttura del Repository
 * Clusters_to_lovers.ipynb - Notebook Python contenente il codice sorgente dell'analisi.
 * Clusters_to_lovers_slides.pdf - Slide di presentazione del progetto.
 * grafico_elbow_silhouette.png - Grafico con il metodo del gomito e il Silhouette Score (K=2 \dots 10).
 * grafico_1_matrice_confusione.png - Matrice di confusione Regex vs Ground Truth.
 * README.md - Documentazione del progetto.
> Nota sull'uso dell'IA: Il codice del notebook è stato sviluppato con il supporto di un assistente IA; l'impostazione concettuale del progetto, la pipeline metodologica e l'analisi critica dei risultati sono a cura dell'autrice.
>
> 
