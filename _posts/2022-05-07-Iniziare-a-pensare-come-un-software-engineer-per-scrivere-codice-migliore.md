---

layout: post
title: "Iniziare a pensare come un software engineer per scrivere codice migliore"
categories: Programmazione

---

## Qualità del codice 

Nella vita di tutti i giorni interagiamo molto con il software e ormai dipendiamo da essi. Pensa ad un'app bancaria, ma che non ha un backend ben fatto, potrebbe benissimo traferire i nostri soldi a qualcuno o semplcemente mostrarne di meno. Un piccolo bug può rovinarci la vita.

Un codice di qualità tende a produrre software più affidabile, più facile da mantenere e aggiornare e soprattutto con meno bug.

![qualityofcde](https://cdn.hashnode.com/res/hashnode/image/upload/v1639468826578/Rx2sAQQPK.png?auto=compress,format&format=webp)

## Gli obiettivi

Definire il codice come di buona o scarsa qualità è una cosa molto soggettiva ed estremamente giudicante. Per cercare di essere un po' più obbiettivi, dovresti guardare il tuo codice e cercare di capire esattamente che cosa vuoi che esso faccia. Tom Longs suggerisce che un codice che ha queste 4 cose sia di alta qualità:
 
 1. **Funziona:** Il codice **deve** funzionare.
 2. **Continua a funzionare:** il codice **deve** continuare a funzionare tutto il tempo, senza interruzoni o errori.
 3. **Adattabile alle esigenze:** è raro che il codice venga scritto una sola volta e mai modificato.
 4. **Non reinventare la ruota:** non è neccessario creare da 0 un sistema per importare i file quando c'è una libreria che lo fa già. 
  
 
## I pilastri
 
 I 4 obiettivi che abbiamo appena esaminato ci aiutano a concentrarci su ciò che stiamo cercando di raggiungere, ma non forniscono consigli specifici su cosa fare nella nostra programmazione quotidiana. Ecco alcune strategie che ci aiuteranno a raggiungere facilmente questi obiettivi.
 
 1. **Rendi il codice leggibile:** se il nostro codice ha una scarsa leggibilità, altri ingegneri dovranno dedicare molto tempo a cercare di decifrarlo. C'è anche un'alta probabilità che possano interpretare erroneamente ciò che fa o perdere dettagli importanti. Se ciò accade, è meno probabile che vengano individuati bug durante la revisione del codice ed è più probabile che vengano introdotti nuovi bug quando qualcun altro deve modificare il nostro codice per aggiungere nuove funzionalità. Commentare il codice a volte può essere una soluzione, ma è più facile e sicuro rendere leggibile il codice stesso.
 2. **Rendi il codice difficile da usare in modo improprio:** il codice che scriviamo viene spesso chiamato da altro codice. Ci aspettiamo che l'altro codice inserisca determinate cose, come argomenti di input o metta il sistema in un determinato stato prima di chiamare. Se le cose sbagliate vengono inserite nel nostro codice, le cose potrebbero esplodere; il sistema si arresta in modo anomalo, un database viene danneggiato in modo permanente o alcuni dati importanti vengono persi. Anche se le cose non esplodono, ci sono buone probabilità che il codice non funzioni.
 3. **Rendi il codice modulare:** modularità significa che un oggetto o un sistema è composto da componenti più piccoli che possono essere scambiati o sostituiti indipendentemente. Semplicemente, questo significa che devi creare il tuo codice proprio come un giocattolo LEGO. Altri dovrebbero essere in grado di rimuovere e aggiungere elementi senza danneggiare il codice.
 4. **Rendi il codice riutilizzabile e generalizzabile:** riutilizzabilità significa che qualcosa può essere utilizzato per risolvere lo stesso problema ma in più scenari. E generalizzabilità significa che qualcosa può essere utilizzato per risolvere molteplici problemi concettualmente simili che sono sottilmente diversi. Se otteniamo un trapano a mano, è anche generalizzabile perché può essere utilizzato per praticare fori, può essere utilizzato anche per avvitare.
 
## Scrivere codice di alta qualità ci rallenta?

Leggendo tutte queste cose potresti aver pensato, ma tutto questo non mi rallenta? In rete ho trovato un bell'esempio:

Stai montando uno scaffale a casa. Hai due modi per farlo,

Il modo corretto: fissiamo staffe al muro perforando e avvitando in qualcosa di solido come i montanti del muro o la muratura. Quindi montiamo lo scaffale su queste staffe. Tempo impiegato: 40 minuti

Modo velce: compriamo della colla e incolliamo lo scaffale al muro. Tempo impiegato: 10 minuti

![scaffale](https://cdn.hashnode.com/res/hashnode/image/upload/v1640324588983/2LBrhpL45.png?auto=compress,format&format=webp)

Potrebbe sembrare che farlo nel modo corretto sia una perdita di 20 minuti, ma rispetto al tempo e alla seccatura dei "bug" creati in modo frettoloso, il modo corretto ci fa risparmiare un sacco di tempo.

Pertanto, scrivere codice di alta qualità non ti rallenta mai. È semplicemente un ottimo software.

## Lavorare con altri software engineer

![eng](https://cdn.hashnode.com/res/hashnode/image/upload/v1640324302318/nJpU0r8I7.png?auto=compress,format&format=webp)

La creazione di software è solitamente lavoro di squadra. Un'azienda che produce software può avere centinaia di sviluppatori che creano centinaia di progetti e spesso potresti dover lavorare con il codice di altre persone così come loro lavoreranno con il tuo.

![wwoe](https://cdn.hashnode.com/res/hashnode/image/upload/v1639486653422/PJL7fFlnB.png?auto=compress,format&format=webp)

Pertanto, durante la scrittura del codice; anche se lavori da solo, devi preoccuparti di queste cose:

### Le cose che sono ovvie per te non sono ovvie per gli altri

Quando ti metti a scrivere del codice, probabilmente hai già passato ore o giorni a pensare al problema che stai risolvendo. Potresti aver attraversato diverse fasi di progettazione, test dell'esperienza utente, feedback sul prodotto o segnalazioni di bug. Potresti avere così familiarità con la tua logica che le cose sembrano ovvie e hai a malapena bisogno di pensare al motivo per cui qualcosa è così com'è o perché stai risolvendo il problema in questo modo.

Ma che dire dei tuoi compagni di squadra? Dovranno interagire con il tuo codice, apportarvi modifiche o apportare modifiche a qualcosa da cui dipende. Non avranno avuto il vantaggio  tempo per capire il problema e pensare a come risolverlo. Le cose che sono ovvie per te non sono ovvie per loro.

Pertanto, è utile considerare sempre questo e assicurarsi che il codice spieghi come dovrebbe essere utilizzato, cosa fa e perché lo sta facendo.

### Col tempo, ti dimenticherai del tuo codice

Il pezzo di codice che hai appena scritto è nella tua mente e non penserai mai che lo dimenticherai. Ma non siamo macchine, dimentichiamo le cose. Questo non avrà importanza fino a quando non arriverà una nuova funzionalità, o un bug ti verrà assegnato tra un anno, potresti dover modificare il codice che hai scritto e potresti non ricordarne più tutti i dettagli.

Guardare il codice che hai scritto un anno o due fa non è molto diverso dal guardare il codice scritto da qualcun altro. Assicurati che il tuo codice sia comprensibile anche a qualcuno con poco o nessun contesto. Non solo farai un favore a tutti gli altri, ma lo farai anche a te stesso futuro.

## Unit test

![ut](https://cdn.hashnode.com/res/hashnode/image/upload/v1640324480608/d_hLdlsOw.png?auto=compress,format&format=webp)

L'unit test prevede il test di ciascuna unità o singolo componente dell'applicazione software. È il primo livello di test funzionale. L'obiettivo alla base dell'unit test è convalidare i componenti dell'unità con le loro prestazioni. 

## Come fare un buon unit test?

Anche se potresti pensare che non sia molto importante, è la parte più importante della creazione di un buon software. Quando non sta andando bene, può portare a molti problemi come bug inosservati, impossibilità di mantenere, ecc. Quindi sapere cosa rende un buon unit test è importante.

- **Rileva accuratamente le "rotture":** se il codice è rotto, un test dovrebbe fallire. E un test dovrebbe fallire solo se il codice è effettivamente rotto (non vogliamo falsi allarmi).
- **Errori ben spiegati:** se il codice non funziona, l'errore del test dovrebbe fornire una chiara spiegazione del problema.
- **Codice del test comprensibile:** altri ingegneri devono essere in grado di capire cosa sta testando esattamente un test e come lo sta facendo.
- **Facile e veloce da eseguire:** gli ingegneri di solito hanno bisogno di eseguire unit test abbastanza spesso durante il loro lavoro quotidiano. Un unit test lento o difficile da eseguire farà perdere molto tempo.
