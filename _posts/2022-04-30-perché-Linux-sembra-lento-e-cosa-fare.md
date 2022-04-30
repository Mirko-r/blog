---

layout: post
title: "Perché Linux sembra lento e cosa fare"
categories: Tutorial Linux

---

![linux](https://images.pling.com/img/00/00/18/86/88/1056847/138251-1.png)

Le performance desktop di Linux sono sempre state un problema, e sono la causa vecchie guerre tra gli sviluppatori di Linux. Oggi, voglio mostrarti come ho migliorato drasticamente le prestazioni del mio laptop.

## Il problema

Qual é la prima causa dei problemi di performance?

Rispondiamo con altre domande:

- Hai notato quanto é lento il tuo PC dopo aver estratto un grande archivo?
- Hai notato che dopo aver sospeso il PC e averlo risvegliato molti programmi ci mettono un po' prima di tornare scattanti come prima?
- Usi Amarok? Se si, avrai sicuramente notato la lentezza del sistema dopo che Amarok ha scansionato la tua libreria.

Questi problemi di performance ci sono quasi sempre. In effetti, sarei disposto a scommettere che questa è la causa per cui la maggior parte delle persone trova Linux lento.

### La radice del problema

Ecco la mia scoperta principale: le prestazioni desktop hanno ben poco a che fare con gli scheduler e le prestazioni reali. Ha tutto a che fare con la memorizzazione nella cache del disco e le prestazioni percepite. **Perché infatti le prestazioni reali sono ottime**

Ora, abbi pazienza per un secondo, perché quello che sto per spiegare ti farà capire quanto segue. Sulla base della mia conoscenza limitata del kernel Linux (correggimi se sbaglio, specialmente sulla terminologia), ci sono due tipi di cache distinti:

- block cache
- inode/dentry cache

Il primo problema é che la block cache compete con le applicazioni per la RAM. Il secondo problema é che la block cache compete con l'inode/dentry cache.

Su un normale computer desktop Linux, quando sei a corto di RAM, Linux cercherà in modo aggressivo di liberare memoria per ingrandire le cache. Di solito lo fa eseguendo il paging su disco di parti inutilizzate di applicazioni. Questo non è proprio ottimale.

## Il modo in cui Linux utilizza la RAM è importante per le prestazioni

Se hai 1 GB di RAM (o meno) e decomprimi un archivio da 300 MB, il kernel Linux scambierà parti delle tue applicazioni su disco nel tentativo di ospitare sia il file da 300 MB che i blocchi dei file non compressi che stanno per essere scritti su disco. Riducendo la cache inode/dentry.

Linux, ovviamente, pensa che stia facendo una buona cosa; l'archivio si decomprime, in effetti, piuttosto rapidamente grazie a questo. Ovviamente,mentre (e dopo) si è verificata la decompressione, l'avvio di un'applicazione o l'apertura di una nuova finestra del browser richiedono improvvisamente anni e le tue applicazioni aperte si bloccano per un paio di secondi prima reagire.

Questo perché Linux soddisfa le esigenze del programma di decompressione e le tue che stai dicendo al tuo computer di fare altre cose nel mentre. Linux inizia a confondersi tra la lettura dell'archivio, la scrittura di dati su disco e il ripristino delle parti delle applicazioni che stai utilizzando. 

Non bene. Suppongo che le impostazioni predefinite possano essere molto buone per carichi di lavoro con molte attività ripetute, ma decisamente non adatte a situazioni desktop.

## Noi vogliamo le performance percepite

Prendiamo ad esempio un file manager. Concentriamoci su Konqueror. Supponiamo di premere il pulsante Home sul pannello, che mostra una delle istanze di Konqueror e gli chiede di sfogliare la mia home directory.

Se hai molta RAM, non sarà un problema: sia Konqueror che il contenuto della tua home directory saranno in memoria, quindi sarà incredibilmente veloce. Ma se sei piuttosto a corto di memoria, è una questione diversa: ciò che accade dopo determina se il tuo computer sia lento o veloce.

Se Konqueror è stato cancellato dal file di paging, sembrerà bloccato (o impiegherà più tempo per "avviarsi") per un paio di secondi, finché Linux non avrà eseguito il paging dei percorsi di codice necessari. Se, al contrario, la mia home directory è stata eliminata dalla cache della RAM, Konqueror apparirà istantaneamente e sarà reattivo, mentre la home directory viene caricata.

Preferirei di gran lunga aspettare il caricamento della directory piuttosto che dover aspettare che Konqueror si sblocchi. La differenza è che nel primo scenario posso chiudere la finestra, utilizzare i menu, navigare tra i controlli della finestra, modificare l'URL, annullare l'operazione; nel secondo caso, sono fregato fino a quando Linux non decide di impaginare completamente tutto ciò di cui ha bisogno.

Questo è ciò che sto dicendo: prestazioni percepite, non throughput. Per me è importante poter usare il file manager mezzo secondo dopo aver premuto il pulsante Home. Non mi importa che, a causa di questa preferenza, la home directory impieghi effettivamente un secondo in più per terminare il caricamento.

Variazioni di questo modello possono essere trovate ovunque: nelle finestre di dialogo di apertura dei file, nelle applicazioni multimediali con raccolte, praticamente ovunque un'operazione richieda una sorta di rapporto sullo stato di avanzamento.

## Le soluzioni
In giro per internet, ma soprattutto Reddit, ho trovato due soluzioni:

### Ottimizzazione della swappiness

"Swappiness" è il nome che gli sviluppatori del kernel Linux hanno dato alla preferenza tra il paging delle applicazioni su disco e (in pratica) la riduzione della cache. Se è vicino a 0, Linux preferirà mantenere le applicazioni nella RAM e non aumentare le cache. Se è vicino a 100, Linux preferirà sostituire le applicazioni e allargare il più possibile le cache. L'impostazione predefinita è 60.

L'ironia di questa preferenza è che, in effetti, il paging di un'applicazione inutilizzata generalmente produce un incremento netto delle prestazioni, poiché la cache aiuta davvero molto quando è necessario, ma questo incremento netto delle prestazioni si traduce in un calo netto delle prestazioni percepite, poiché di solito non ti interessa se un file viene decompresso pochi secondi dopo, ma ti interessa molto quando le tue applicazioni non rispondono istantaneamente.

Su un computer desktop, vuoi che lo swappiness sia il più vicino possibile a zero. Il motivo per farlo è perché questo "immunizzerà" il tuo computer dalla carenza di memoria causata da manipolazioni temporanee di file di grandi dimensioni (pensa a copiare un file video di grandi dimensioni su un altro disco). La cache sarà comunque il più grande possibile, ma non sostituirà le applicazioni in esecuzione.

Non mi credi? Se hai due gigabyte di riserva sulla tua partizione /, esegui i seguenti comandi in ordine:

```bash
sync
echo 3 > /proc/sys/vm/drop_caches
dd if=/dev/zero of=/tmp/testfile count=1 bs=900M
find / > /dev/null
cp /tmp/testfile /tmp/testfile2
```

Nel mentre che vengono eseguiti non fare nulla. Quello che fanno questi comandi è piuttosto semplice: crea un grande file da 900 MB, tenta di trovare tutti i file sul tuo file system, quindi copia il file ancora una volta.

Finito? Ora fai qualcosa con il tuo computer.

Cosa è successo? Atrocemente lento, giusto? Hai notato come le applicazioni si sono bloccate per un po', mentre la spia del disco rigido lampeggiava freneticamente?

OK, aggiustiamo la swappiness. Come root, esegui:

```bash
sysctl -w vm.swappiness=1
```

e riprova i comandi sopra. Un enorme differenza, vero?

Questo perché, con lo swappiness disattivato, il kernel Linux non tenta più di ampliare la cache eseguendo il paging delle applicazioni a meno che il tuo PC non stia riscontrando una carenza di memoria estremamente elevata.

Per rendere permanente la modifica, scrivi `vm.swappines=1` nel file `/etc/sysctl.conf`.

### Le cache del filesystem é più importante delle altre cache

Abbiamo già stabilito che la cache del filesystem è importante perché, senza di essa, anche la navigazione dei file procede molto lentamente. Ora impareremo come dire a Linux che vogliamo che preferisca la cache inode/dentry ad altre cache.

Abbiamo già stabilito che la cache del filesystem è importante perché, senza di essa, anche la navigazione dei file procede molto lentamente. Ora impareremo come dire a Linux che vogliamo che preferisca la cache inode/dentry ad altre cache.

Prova questo:

```bash
sync
echo 3 > /proc/sys/vm/drop_caches
dd if=/dev/zero of=/tmp/testfile count=1 bs=900M

sysctl -w vm.vfs_cache_pressure=100
find / > /dev/null
cp /tmp/testfile /tmp/testfile2
time find / > /dev/null

sysctl -w vm.vfs_cache_pressure=50
find /  > /dev/null
cp /tmp/testfile2 /tmp/testfile3
time find / > /dev/null

rm -f /tmp/testfile /tmp/testfile2 /tmp/testfile3
```

Questa serie di comandi fa sostanzialmente quello che facevano quelli di prima, solo che questa volta tenta di misurare il tempo necessario per trovare tutti i file sul disco, dopo aver creato file di grandi dimensioni. Lo fa due volte: una con le impostazioni predefinite per la preferenza della cache e una con le impostazioni fortemente inclinate per favorire la cache inode/dentry.

Controllare l'ouput. La seconda volta ha richiesto meno tempo? Ha decisamente ridotto i tempi di oltre la metà sul mio computer desktop. Quando questa variabile è più vicina a 1, il kernel preferirà proteggere la cache inode/dentry liberando prima la block cache.

Per rendere permanente la modifica, metti `vm.vfs_cache_pressure=50` nel file `/etc/sysctl.conf`.

I valori vicini a 100 non forniscono alcun guadagno. Valori prossimi allo zero possono causare un'enorme attività di scambio durante le scansioni di file system di grandi dimensioni.

## Questo è tutto

Linux desktop è abbastanza buono. Tuttavia, ci sono alcune situazioni in cui potrebbe migliorare. Fortunatamente, Linux è pronto all'uso con le cose necessarie per ottimizzarlo per le prestazioni desktop.

Se vuoi scoprire come si comportano le tue applicazioni in relazione all'utilizzo effettivo del disco, ti raccomando [iogrind](https://wiki.gnome.org/Apps/Iogrind)
