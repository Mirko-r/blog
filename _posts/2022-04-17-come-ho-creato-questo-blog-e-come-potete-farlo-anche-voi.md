---

layout: post
title: "Come ho creato questo blog e come potete farlo anche voi"
categories: Tutorial

---

>**Se sei familiare con Github Pages potresti vedere alcune cose "incorrette"... questo è perchè sto cercando di mantenerlo semplice**

"Come inizio con il blogging?"

"Come tiro su un blog?"

"Quale piattaforma dovrei usare?"

Questo ero io tempo fa, ho provato diverse piattaforme, e tutte erano abbastanza valide; ma nessuna di queste faceva, per una ragione o per un'altra quello che io volevo che facesse.

Così ho provato a farlo con [Github Pages](https://pages.github.com/), è un servizio offerto da Github dove puoi hostare il tuo sito statico, quindi senza backend, e puoi farlo **gratis**. 

Trovo Github Pages fantastico, ma quando inizi a imparare "come fare un blog con Jekyll", le cose iniziano a diventare abbastanza confusionarie... Inizi a imparare Ruby e Jekyll a
e come farli funzionare su Windows, imparare linguaggi come YAML e Liquid... le cose si fanno difficili in fretta.

### Questo è cos'ho fatto

Ho preso questo blog partendo da un template, tolto le cose non necessarie, e iniziato a scoprire le features interessanti.

Questo è come ho fatto

---

## Setup

* __Creare un account Github__ :

Se non hai un account creane uno [qui](https://github.com/join)
>_**nota**: Il nome utente che sceglerai farà parte del link_
<br>

* __Aprire il template__:

Copia questo [link](https://github.com/chadbaldwin/simple-blog-bootstrap/generate), e segui le istruzioni al prossimo punto
<br>

* __Setup del repo__:

>_**nota**: Se non metti correttamente il nome,tutto ciò non funzionerà_

Il tuo repo dovrà chimarsi così: [username].github.io, fai anche attezione a lasciarlo pubblico e non privato. Ora puoi cliccare su "Create repository".

<img style="width:100%;height:100%;" data-uri="82682" src="https://chadbaldwin.net/img/createblog/create_blog_copy_template.gif">

<br>

---
**Congratulazioni!** Hai appena creato un blog SQL. O almeno, è solo il template di default.<br> Dobbiamo ancora aggiungere cose personali come il tuo nome, una bio, i link per i social, e sai... magari anche dei posts. 
<P>
Ci sarà qualche minuto di ritardo ma GitHub riconoscerà che hai creato una GitHub Pages repo, e automaticamente creerà il sito. E lo aggiornerà ogni volta che farai una modifica
</p>
<p>
Una volta passati un paio di minuti, vai a controllare il tuo sito! Il link sarà semplicemente il nome del repository.
</p>
Come questo: https://chadblogtest.github.io

---

## Customization

Ora che hai il tuo blog probabilmente non vuoi che il tuo sito abbia "Default Author Name" dapertutto, probabilmente vuoi anche cambiare il nome del blog in qualcos'altro da "My blogs name".

Per cambiare queste cose dobbiamo modificare questo file: `config.yml`

Se non vuoi mettere un email, o qualsiasi altro account social, puoi semplicemente commentarli con `#`

<img style="width:100%;height:100%;" data-uri="82682" src="https://chadbaldwin.net/img/createblog/create_blog_edit_config.gif">

Dopo, vorrai modificare la home page. Qui puoi scrivere una tua descrizione, o magari un riassunto su quali argomenti tratta il tuo blog.

Per editare la home page, bisogna fare la stessa cosa di prima ma con `index.md`. 

<img style="width:100%;height:100%;" data-uri="82682" src="https://chadbaldwin.net/img/createblog/create_blog_edit_home_page.gif">

---

## Il tuo primo post

I post si trovano nella cartella `_post`, il file che trovi dentro é un sample.

Ma prima spieghiamo alcune cose da sapere:

* GitHub Pages usa un software chiamato "Jekyll" per far funzionare il dietro le quinte, converte il tuo post in HTML che puoi vedere nel browser.
* I post vanno chiamati seguendo sempre questo schema:  `yyyy-mm-dd-your-blog-post-name.md`
* I post sono scritti in markdown, se non lo conosci, non preoccuparti, guarda [questo post](https://guides.github.com/features/mastering-markdown/) per imprarae le basi.

Questo é tutto quello di cui hai bisogno per partire
