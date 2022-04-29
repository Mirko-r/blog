---

layout: post
title: "Come far funzionare una GPU esterna su Raspberry Pi"
categories: Tutorial

---

## I problemi di ARM e Broadcom con PCI Express

Nel 2020, dopo che Raspberry ha rilasciato l'ultima versione della sua scheda. Ho iniziato le ricerche per far funzionare una scheda grafica esterna con un raspberry pi 4.

![exgc](https://www.jeffgeerling.com/sites/default/files/images/amd-radeon-gpu-with-raspberry-pi-compute-module-4.jpeg)

Dato che non avevo mai lavorato con un chip ARM, avevo molto da imparare su PCI Express, lo stato dei driver delle schede grafiche in Linux e il supporto PCI Express su vari SoC ARM.Dopo aver fallito nel far funzionare Nvidia GT710 o AMD 5450, ho iniziato a testare GTX 750 Ti, RX 550 e SM750, tutte con architetture e supporto driver molto diversi. Dopo aver fallito nel far funzionare quelle schede, ho testato una GTX 1080 più recente e ho persino fatto una pazzia con una AMD Radeon RX 6700 XT sul Pi, ma purtroppo  non ha funzionato.

Lungo la strada, dozzine di persone (dagli ingegneri AMD, agli appassionati di ARM, ai colleghi hobbisti come me) hanno esplorato gli angoli bui e polverosi del BCM2711, il SoC ARM di Broadcom che alimenta il Raspberry Pi Compute Module 4.

![SoC](https://eji4evk5kxx.exactdn.com/wp-content/uploads/2016/02/Raspberry_Pi_3_Large.jpg?lossy=1&w=2560)

Quello che hanno scoperto è che il PCIe del BCM2711 è fondamentalmente "rotto", almeno quando si tratta di alcune operazioni di memoria su Linux a 64 bit. Alcuni hanno ipotizzato che la rottura non potesse essere aggirata, ma come disse una volta Winston Churchill:

>Il successo è inciampare da un fallimento all'altro senza perdere l'entusiasmo.

Questo problema in particolare, con oltre 490 commenti al momento della stesura di questo documento, documenta dozzine di guasti in una posizione centrale, al punto che potrebbero essere classificati e aggirati in una serie di patch al driver radeon open source.

## Come far funzionare una GPU AMD

![pcie-driver](https://miro.medium.com/max/1400/0*Om7grCpC_DEvbp8s)

Prima di partire, ti serve:

- Un Raspberry Pi 4 
- Raspberry Pi Compute Module 4 IO Board (o qualsiasi IO board con uno slot PCIE).
- Adattore da PCIEx1 a x16
- Una scheda video AMD Radeon 5000/6000/7000

### Preparare il sistema operativo

La patch corrente si basa sul precedente fork Linux 5.10.y mantenuto da Raspberry Pi, quindi è necessario eseguire il flashing di una copia del sistema operativo Raspberry Pi dell'inizio di quest'anno **non l'ultimo**. ho scaricato `2022-01-28-raspios-bullseye-arm64-full.zip` da [qui](https://downloads.raspberrypi.org/raspios_full_arm64/images/raspios_full_arm64-2022-01-28/), e poi ho usato Raspberry Pi Imager per flasharla su una scheda SD.

Una volta acceso il Raspberry, installo i driver AMD con

```bash
$ sudo apt install -y firmware-amd-graphics
```

Quindi sono andato alla mia workstation principale e ho cross-compilato il Kernel del Raspberry.
 > Perchè cross-compilare?<br> Bene, una compilazione richiede tra i 6 ei 10 minuti sulla mia workstation principale. Su un Raspberry Pi 4,  il processo richiede quasi un'ora.
 
Per semplificarti le cose, inserisci nella blacklist il driver radeon prima di copiare il kernel cross-compilato sul Pi. Crea un file chiamato `/etc/modprobe.d/blacklist-radeon.conf` e inserisci:

```bash
blacklist radeon
```

Quindi copia il kernel cross-compilato su Pi. Abbiamo quasi finito, ma per far funzionare Xorg e altri compositori come Weston, devi anche sovrascrivere la libreria `memcpy`:

```bash
# Scarica la libreria memcpy modificata di Coreforge.
wget https://gist.githubusercontent.com/Coreforge/91da3d410ec7eb0ef5bc8dee24b91359/raw/1b72d428b2fe1cba459d5ae7f73663483743ff55/memcpy_unaligned.c

# Compila la libreria e spostala in /usrl/local/bin
gcc -shared -fPIC -o memcpy.so memcpy_unaligned.c
sudo mv memcpy.so /usr/local/lib/memcpy.so

# Crea un file `ld.so.preload` per dire a Linux di usare la nostra versione di `memcpy`.
sudo nano /etc/ld.so.preload

# Scrivi in  ld.so.preload:
/usr/local/lib/memcpy.so
```

### Caricare i driver

Ora riavvia il Raspberry Pi. Dopo il riavvio, puoi aprire il terminale ed eseguire `dmesg --follow` per vedere cosa sta succedendo .

Per caricare il driver `radeon` esegui:

```bash
modprobe radeon
```

Dopo 10 o 20 secondi, se hai un monitor collegato alla scheda Radeon, dovrebbe funzionare durante il caricamento del driver. In genere si imposta il sistema operativo Pi per l'avvio su console (CLI) anziché sul sistema grafico, poiché è più stabile.

Per una migliore stabilità eseguendo Xorg (`startx` per avviare) o Weston (`weston-launch` per avviare), dovresti anche aggiungere le seguenti opzioni al tuo `/boot/cmdline.txt` (nella stessa riga delle altre opzioni) e riavviare:

```bash
radeon.uvd=0 pci=noaer,nomsi radeon.msi=0 radeon.pcie_gen2=0 pcie_aspm=off radeon.aspm=0 radeon.runpm=0 radeon.dpm=0
```

Le patch e le opzioni della riga di comando sono ancora attivamente esaminate. Segui [questa issue](https://github.com/geerlingguy/raspberry-pi-pcie-devices/issues/4) per ulteriori progressi: Test GPU (VisionTek Radeon 5450 1GB).

## Cosa funziona?

### Xorg

![ww1](https://www.jeffgeerling.com/sites/default/files/images/pi-desktop-janky-amd-radeon.jpg)

In sintesi: porte DisplayPort, VGA, HDMI e DVI. La riga di comando (console), Xorg e Weston (un'implementazione di riferimento di Wayland), nonché alcuni benchmark 3D e applicazioni che utilizzano OpenGL.

Ma Xorg mostra soprattutto molti "glitch" nel suo output (vedi sopra), specialmente quando si interagisce con diversi elementi dello schermo.

### Weston

![weston](https://www.jeffgeerling.com/sites/default/files/images/weston-smoother-amd-radeon-pi.jpg)

Weston (nella foto sopra) non aveva gli stessi glitch, ma funzionava un po' lento e spesso si bloccava dopo un po'.

## Cosa non funziona?

Molte cose continuano a non funzionare, dal momento che ogni caratteristica specifica di una determinata linea di schede richiederebbe più lavoro per esaminare il codice e trovare problemi di memoria.

Ad esempio, l'accelerazione H.264 è attualmente disabilitata, quindi l'utilizzo della scheda come acceleratore di transcodifica video `ffmpeg` non funzionerà. Inoltre, supponendo che potremmo far funzionare i driver di Nvidia (o più realisticamente, `nouveau`, dal momento che Nvidia non apre i loro driver), cose come i core CUDA sarebbero comunque inaccessibili.

E anche dopo più lavoro, è improbabile che tu possa fare qualcosa come giocare a un gioco AAA su un Pi con una GPU esterna. Molte cose vanno contro questa possibilità in questo momento:

- La linea x1 Gen 2.0 non fornisce molta larghezza di banda.
- La maggior parte (tutti?) dei giochi AAA sono compilati per piattaforme X86, non per ARM/ARM64. Box86/Box64 farebbero fatica, specialmente con un driver grafico hackerato.
- Cercare di risolvere i livelli di incompatibilità con (a) ARM64, (b) Linux e (c) un driver modificato per una vecchia GPU non supportata è un compito piuttosto folle.

## E ora?
Innanzitutto, sono sicuro che molte persone che hanno letto questo post hanno avuto i seguenti pensieri in testa:
- Che ne dici di eseguire [Windows su Raspberry](https://www.worproject.ml/)? Dopotutto, Windows è molto meglio per i giochi!
- Dovresti provare ad alimentare la scheda in modo diverso; la CM4 IO Board può fornire solo fino a 25W!
- E i SoC Rockchip o Allwinner? O l'M1 di Apple?

Bene, in tutti e tre i casi precedenti, queste strade sono state discusse ed esplorate un bel po', e ti assicuro che di solito sono vicoli ciechi:

### Windows on ARM

Windows su Raspberry esegue Windows per ARM, che non supporta i driver GPU ARM in alcun modo. E i driver GPU di Windows sono anche più ottusi dei driver Linux, il che significa che sarebbe più difficile per un ragazzo a caso come me eseguirne il debug. E infine, è improbabile che Microsoft aggiusti i bug hardware PCIe sul BCM2711 poiché comunque non supportano nemmeno Windows sul Raspberry Pi!

### Alimentazione

I problemi che stiamo riscontrando non sono legati all'alimentazione e ho anche testato tutte le schede grafiche in riser alimentate con alimentatori da 650 W e 750 W.

### Altri SoCs
Esistono alcune piattaforme ARM che supportano GPU esterne, come alcune schede Solid-Run. Ma la maggior parte dei SoC ARM, specialmente quelli destinati all'uso mobile/embedded, hanno PCIe "rotto" (proprio come il Broadcom BCM2711) e si imbattono in problemi molto simili (se non identici) con cose come le schede grafiche.

Pgwipeout ha esplorato alcuni bus PCIe dei SoC Rockchip e ha detto questo:

>Sappiamo già che a BRCM non interessa rispettare le specifiche e la loro implementazione è gravemente interrotta.<br>Sembra che anche la serie rk35xx non sia conforme, ma non così male.<br>Sto cercando di restringere esattamente il livello di gravità, in modo da poterlo documentare mentre finalizziamo il supporto PCIe per la linea principale.

Finora sembra che stiamo raggiungendo vicoli ciechi simili con la gestione della memoria MMIO e PCIe su tutti i SoC su SBC a basso costo, almeno all'attuale generazione.

La mia speranza è che i futuri chip di Broadcom possano avere un'implementazione migliore, che potrebbe almeno funzionare con l'ultima generazione di dispositivi PCIe e non richiedere soluzioni alternative come evitare `writeq` su sistemi operativi a 64 bit.

Allo stato attuale, alcuni dispositivi (come i nuovi HBA LSI) possono aggirare i problemi con penalità minime sulle prestazioni, mentre altri dispositivi (come le schede grafiche e Coral TPU di Google) lavorano molto malei.
