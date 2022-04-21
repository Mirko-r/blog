> In informatica, il problema del produttore-consumatore (conosciuto anche con il nome di problema del buffer limitato o bounded-buffer problem) è un esempio classico di sincronizzazione tra processi. Il problema descrive due processi, uno produttore (in inglese producer) ed uno consumatore (consumer), che condividono un buffer comune, di dimensione fissata. Compito del produttore è generare dati e depositarli nel buffer in continuo. Contemporaneamente, il consumatore utilizzerà i dati prodotti, rimuovendoli di volta in volta dal buffer. Il problema è assicurare che il produttore non elabori nuovi dati se il buffer è pieno, e che il consumatore non cerchi dati se il buffer è vuoto.

## Il problema

Costruire un programma che simuli il funzionamento di un buffer utilizzando l’algoritmo del produttore e del consumatore.

Il buffer sarà simulato con un vettore di interi di dimensione N (impostare una define con un valore a piacere). Inizialmente il buffer è vuoto (lo si inizializzi con zeri). Bisognerà ipotizzare anche una variabile, di nome prodotti, che che punti sempre alla prima cella libera per la produzione (inizialmente, essendo vuoto il buffer, la variabile punta alla cella del vettore 0).

### PRODUTTORE

Produce n numeri casuali che vanno da 1 a 10 e li posiziona nelle celle del buffer. Anche il numero di celle da riempire (n) sarà casuale. Durante il flusso di produzione il consumatore dovrà stare in attesa su un semaforo rosso. Il flusso di produzione sarà il seguente:

- imposto il numero di celle da riempire utilizzando una funzione che genere numeri casuali da 1 a N. Lo salvo in una variabile np
- genero un numero casuale che va da 1 a 1513
- lo inserisco nella cella individuata dalla variabile prodotti
- incremento la variabile prodotti. 
- se la variabile prodotti è uguale a N il produttore ha finito il suo lavoro
- decremento la variabile np
- se la variabile n è uguale a zero ho finito la produzione altrimenti ritorno al punto 2.
 
 Alla fine della produzione il produttore stamperà i valori contenuti nel buffer e metterà a verde il semaforo del consumatore.

### CONSUMATORE

Mette zero in n celle del buffer, dove n è un numero casuale che va da 1 a N, partendo dalla cella puntata dalla variabile prodotti -1. Durante il flusso di consumo il produttore dovrà stare in attesa su un semaforo rosso. Il flusso del consumatore sarà il seguente:

- genero un numero casuale che va da da 1 a N e lo salvo in una variabile nc
- decremento la variabile prodotti
- se prodotti è uguale a -1, metto prodotti uguale a zero e finisco il flusso consumo
- metto zero nella cella individuata da prodotti
- decremento nc
- se nc è uguale a zero finisco il consumo altrimento torno al punto 2

Alla fine della produzione il consumatore stamperà i valori contenuti nel buffer e metterà a verde il semaforo del produttore

## La soluzione

### Lo scheletro del programma

Nel programma serviranno due thread che chiameremo `th_cons` (consumatore) e `th_prod` (produttore)

La libreria che ci servirà è `pthread.h`, i due thread sono due funzioni con puntatori a void, lo scheletro quindi sarà:

```c

#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

void *th_prod(void *args); //produttore
void *th_cons(void *args); //consumatore

int main(){
	return 0;
}
```

---

### Il mutex e il main del programma

I thread in questo programma non possono partire "a caso", ci serve quindi un semoforo, un mutex che viene dichiarato con la variabil globale `pthread_mutex_t`. Nel `main` dovremmo quindi dichiarare una variabile `pthread_t` per i thread, inizializzare i trhead e i mutex, inizializzare a 0 il buffer, e distruggere i mutex, l'unica libreria che ci servirà sarà `string.h` per la funzione `memset`.

Il codice sarà:

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>

pthread_mutex_t mtt_prod, mtt_cons;

void *th_prod(void *args); //produttore
void *th_cons(void *args); //consumatore

int main(){

	pthread_t tid_prod, tid_cons;

	pthread_create(&tid_prod, NULL, th_prod, NULL);
	pthread_create(&tid_cons, NULL, th_cons, NULL);

	if(pthread_mutex_init(&mtt_cons, NULL)) return 1;
	if(pthread_mutex_init(&mtt_prod, NULL)) return 1;

	pthread_mutex_trylock(&mtt_cons);
	pthread_mutex_unlock(&mtt_prod);

	memset(buf, 0, N*sizeof(buf[0])); 

	pthread_join(tid_prod, NULL);
	pthread_join(tid_cons, NULL);

	pthread_mutex_destroy(&mtt_prod);
	pthread_mutex_destroy(&mtt_cons);
	
	return 0;
}
```
---

### Buffer e funzioni 

Dobbiamo ora creare un buffer di N elementi e  una variabile "prodotti", che saranno variabili globali, dobbiamo poi creare una funzione per generare un numero casuale da 1 a N e un'altra funzione per stampare il contenuto del buffer.

Il codice per la funzione che genera un numero casuale è molto semplice:

```c
int genera_numero(){
	return rand()%N+1; // da 1 a N
}
```

Quello per stampare il contenuto del buffer è altrettanto semplice:

```c
void stampa_buf(){
	printf("\n\nValori nel buffer: \n\t");
	for(int i = 0; i<N; i++){
		printf(" %d ,", buf[i]);
	}

	printf("\n\n");
	usleep(200000);
}
```

---

### Il codice del produttore

Per il produttore dobbiamo fare:
- un ciclo infinito `for(;;)` o `while(true)`
- bloccare il mutex del produttore con ``` pthread_mutex_lock(&mtt_prod);``` 
- chiamiare la funzione ```genera_numero()``` e inserire il risultato nella variabile `np`
- generare un numero casuale che va da 1 a 1513
- lnserirlo  nella cella individuata dalla variabile prodotti
- incrementare la variabile prodotti. 
- controllare se `prodotti == N`
- decrementare la variabile np

Il codice non è troppo complesso

```c
void *th_prod(void *args){
	for(;;){
		pthread_mutex_lock(&mtt_prod);

		int np = genera_numero();

		while(np>0){
			buf[prodotti] = rand()%1513 + 1;
			prodotti ++;
			if(prodotti == N){
				np = 0;
			}

			np --;
		}

		printf("\nPRODUTTORE: \n");

		stampa_buf();

		pthread_mutex_unlock(&mtt_cons);
	}
	pthread_exit(NULL);
}
```

---
### Il codice del consumatore
Per il consumatore, invece, dobbiamo fare:
- bloccare il mutex del consumatore con ``` pthread_mutex_lock(&mtt_cons);``` 
- chiamiare la funzione ```genera_numero()``` e inserire il risultato nella variabile `nc`
- decrementare la variabile prodotti
- controllare se `prodotti == -1`
- mettere zero nella cella individuata da prodotti
- decrementare nc

Come per il produttore, il codice non è troppo complesso:

```c
void *th_cons(void *args){
	for(;;){
		pthread_mutex_lock(&mtt_cons);

		int nc = genera_numero();

		while(nc>0){

			if(prodotti == -1){
				prodotti = 0;
				nc = 0;
			}
			buf[prodotti] = 0;

			nc --;

		}

		printf("\nCONSUMATORE: \n");

		stampa_buf();

		pthread_mutex_unlock(&mtt_prod);
	}
	pthread_exit(NULL);
}
```