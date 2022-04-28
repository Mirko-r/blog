---

layout: post
title: "Come far funzionare una GPU esterna su Raspberry Pi"
categories: Tutorial

---

## I problemi di ARM e Broadcom con PCI Express

Nel 2020, dopo che Raspberry ha rilasciato l'ultima versione della sua cheda. Ho iniziato le ricerche per far funzionare una scheda grafica esterna con un raspberry pi 4.

![exgc](https://www.jeffgeerling.com/sites/default/files/images/amd-radeon-gpu-with-raspberry-pi-compute-module-4.jpeg)

Dato che non avevo mai lavorato con un chip ARM, avevo molto da imparare su PCI Express, lo stato dei driver delle schede grafiche in Linux e il supporto PCI Express su vari SoC ARM.Dopo aver fallito nel far funzionare Nvidia GT710 o AMD 5450, ho iniziato a testare GTX 750 Ti, RX 550 e SM750, tutte con architetture e supporto driver molto diversi. Dopo aver fallito nel far funzionare quelle schede, ho testato una GTX 1080 più recente e ho persino fatto una pazzia con una AMD Radeon RX 6700 XT sul Pi, ma purtroppo  non ha funzionato.

Lungo la strada, dozzine di persone (dagli ingegneri AMD, agli appassionati di ARM, ai colleghi hobbisti come me) hanno esplorato gli angoli bui e polverosi del BCM2711, il SoC ARM di Broadcom che alimenta il Raspberry Pi Compute Module 4.

![SoC](https://eji4evk5kxx.exactdn.com/wp-content/uploads/2016/02/Raspberry_Pi_3_Large.jpg?lossy=1&w=2560)

Quello che hanno scoperto è che il root complex PCIe del BCM2711 è fondamentalmente rotto, almeno quando si tratta di alcune operazioni di memoria su Linux a 64 bit. Alcuni hanno ipotizzato che la rottura non potesse essere aggirata, ma come disse una volta Winston Churchill:

>Il successo è inciampare da un fallimento all'altro senza perdere l'entusiasmo.

Questo problema in particolare, con oltre 490 commenti al momento della stesura di questo documento, documenta dozzine di guasti in una posizione centrale, al punto che potrebbero essere classificati e aggirati in una serie di patch al driver radeon open source.

## Come far funzionare una GPU AMD

![pcie-driver](https://miro.medium.com/max/1400/0*Om7grCpC_DEvbp8s)



