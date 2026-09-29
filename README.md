# Clusters to Lovers: trope, TF-IDF e sentiment analysis nelle recensioni BookTok di Emily Henry

**Autrice:** Zilan Polat
**Corso:** Tecnologie dei Dati e del Linguaggio — Prof. Alfio Ferrara
**Anno Accademico:** 2025/2026

**Nota metodologica:** il codice del notebook è stato scritto con il supporto di un assistente AI; l'impostazione del progetto, le scelte metodologiche, l'analisi critica dei risultati e la stesura di questo documento sono a cura dell'autrice.

## Obiettivo

Il progetto analizza un dataset sintetico di recensioni BookTok su quattro romanzi di Emily Henry (*Beach Read*, *Book Lovers*, *Happy Place*, *Funny Story*) per rispondere a una domanda: **con strumenti di NLP relativamente semplici, è possibile riconoscere i trope romantici più discussi e misurare il sentiment dei lettori, comprese le recensioni critiche?**

## Dataset

270 recensioni sintetiche ottenute da 18 recensioni-base (7 positive, 5 miste, 6 negative) ripetute 15 volte e assegnate casualmente ai quattro titoli. Ogni recensione ha un trope di riferimento (ground truth) e un rating da 1 a 5.

## Pipeline

1. **Pre-processing e mappatura dei trope** — pulizia del testo e classificazione a regole (regex) su cinque trope: *enemies to lovers*, *second chance*, *fake dating*, *banter/dialogue*, *emotional depth*.
2. **TF-IDF** — pesatura delle parole più distintive del corpus.
3. **Clustering K-Means** — raggruppamento nello spazio TF-IDF, con K scelto guardando Metodo del Gomito e Silhouette Score (K = 4).
4. **Sentiment analysis (VADER)** e **softmax con temperatura** (t = 0.2 e t = 1.5) applicata ai punteggi.
5. **Valutazione** — matrice di confusione (trope reale vs predetto) e correlazione di Pearson tra rating, sentiment e trope.
6. **Word cloud** e sentiment medio per titolo.
7. **Esportazione** di grafici, dataset processato (`.csv`) e README.

## Risultati principali

- **Classificazione a regole:** la matrice di confusione è perfettamente diagonale (270 su 270). Il risultato dipende dalla costruzione del dataset: ogni recensione-base contiene la keyword del proprio trope e, per *emotional depth*, il trope di default copre anche le recensioni senza keyword. Non dimostra quindi che il metodo generalizzi a recensioni reali.
- **TF-IDF:** le parole con il punteggio medio più alto sono *emotional*, *lovers*, *romance*, *felt*, *fake*.
- **Clustering (K = 4):** cluster di 60, 105, 45 e 60 recensioni. Il cluster 2 coincide interamente con *second chance* e il cluster 0 è quasi tutto *fake dating*; gli altri due raggruppano più trope. Con 5 trope e K = 4 almeno due categorie devono confluire. Il clustering segue il lessico condiviso, non il tono: recensioni positive e negative dello stesso trope finiscono spesso insieme.
- **Rating e sentiment:** la correlazione di Pearson tra rating e sentiment VADER è forte (r = 0.89): il punteggio VADER cresce con il voto, anche sulle recensioni critiche. Va letta con cautela, perché nel dataset sintetico rating e tono delle recensioni sono coerenti per costruzione.
- **Trope e sentiment:** legami deboli (massimo r = 0.22, per *second chance* ed *emotional depth*); anche i legami tra trope e rating sono deboli (tra 0.01 e 0.20).

## Limiti

- Il dataset è **sintetico** e ripete 18 recensioni-base 15 volte, con rating coerente col tono per costruzione: non riflette la variabilità lessicale reale di BookTok e rende più facile la correlazione rating–sentiment.
- La valutazione della classificazione a regole è **circolare**: l'accuratezza del 100% non si generalizza a testi reali, ironici o ambigui.
- VADER è un lessico generico in inglese, non calibrato sul gergo di BookTok (slang, emoji), quindi i punteggi su testi reali possono essere poco affidabili.
- I risultati non sono validati su un corpus di recensioni reali.

## File generati dal notebook

| File | Contenuto |
|---|---|
| `grafico_elbow_silhouette.png` | Metodo del Gomito e Silhouette Score per la scelta di K |
| `grafico_1_matrice_confusione.png` | Matrice di confusione: ground truth vs predizione regex |
| `grafico_2_tfidf_topwords.png` | Top 10 parole per punteggio TF-IDF medio |
| `grafico_3_correlazione.png` | Matrice di correlazione di Pearson |
| `grafico_wordcloud_sentiment.png` | Word cloud delle parole più ricorrenti |
| `grafico_sentiment_by_book.png` | Sentiment medio per titolo |
| `emily_henry_reviews_processed.csv` | Dataset completo con tutte le feature calcolate |

## Come eseguire

Il notebook è pensato per Google Colab: eseguire le celle in ordine dall'alto verso il basso. La prima cella installa le dipendenze non incluse di default (`vaderSentiment`, `wordcloud`).
