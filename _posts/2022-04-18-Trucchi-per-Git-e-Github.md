> Una collezione di features nascoste e no di Git e Github.

## Github

---

### Ignora gli spazi bianchi
Aggiungendo `?w=1` a qualsiasi url diff, si rimuoveranno gli spazi bianchi, facendoti vedere solo il codice effettivo che hai modificato

![Diff without whitespace](https://camo.githubusercontent.com/797184940defadec00393e6559b835358a863eeb/68747470733a2f2f6769746875622d696d616765732e73332e616d617a6f6e6177732e636f6d2f626c6f672f323031312f736563726574732f776869746573706163652e706e67)

---

### Regolare il tab space
Aggiungendo `?ts=4` a un url o ad un file di diff, esso apparirà con 4 caratteri tab anzichè 8 come default. Il numero può essere regolato in base alle tue preferenze. Questo trucco non funziona con i file RAW o Gist, ma [un estensione di Chrome](https://chrome.google.com/webstore/detail/tab-size-on-github/ofjbgncegkdemndciafljngjbdpfmbkn) può automatizzare questo processo.

Questo è un esempio di file Go senza `?ts=4`

![GO file without ts](https://camo.githubusercontent.com/989d4b5ee2571e0f431a90e662d5cfa02a181577278a7e9dfeb81b5ed7270d27/687474703a2f2f692e696d6775722e636f6d2f474954314672302e706e67)

Questo è un esempio di file Go con `?ts=4`

![Go file with ts](https://camo.githubusercontent.com/2251b322f5aed944907cf9f5a0921e988b3d60daba117b182f6710a34eea7055/687474703a2f2f692e696d6775722e636f6d2f3730464c3448392e706e67)

---

### Storia dei commit per autore

Per vedere tutti i commit di un autore in un repo aggiungere `?author=user` all'url

`https://github.com/Mirko-r/blog/commits/main?author=Mirko-r`

---

### Clonare un repository

Quando si clona un repo il `.git` al fondo può essere tralasciato

`$> git clone https://github.com/Mirko-r/blog`

---

### Branches

#### Comprarare tutti i branches con un altro 

Se vai nella sezione branches 

`https://github.com/user/repo//branches`

vedrai tutti i branches di cui non è stato fatto il merge nel branch principale.

Da qui puoi accedere alla pagina di comparazione o rimuovere un branch solo cliccando un pulsante.


#### Comparare i branches
 Per comprarare i branches con Github, cambia l'url così
 
 `https://github.com/user/repo/compare/{range}`
 
 Dove `{range} = master...4-1-stable`
 
 Per esempio:
 
 `https://github.com/rails/rails/compare/master...4-1-stable`
 
 ![range img 1](https://camo.githubusercontent.com/d5e35a2c90ebae61da245ce12c54a84469c50e2792fd71f5b5a7762424f4b495/687474703a2f2f692e696d6775722e636f6d2f744952434f734b2e706e67)
 
 il valore `{range}` può essere cambiato per cose tipo:
 
 `
https://github.com/rails/rails/compare/master@{1.day.ago}...master
https://github.com/rails/rails/compare/master@{2014-10-04}...master`

Ecco un esempio di `{range}` con la data `YYYY-MM-DD`

![range date](https://camo.githubusercontent.com/421a6e6a2a43eb9ac111e968574a13ea8f7c122b85e0326d6702c82285cff6ca/687474703a2f2f692e696d6775722e636f6d2f3564747a45537a2e706e67)

I branches possono essere comparati anche per `diff` o `path` :

`https://github.com/rails/rails/compare/master...4-1-stable.diff
https://github.com/rails/rails/compare/master...4-1-stable.patch`


#### Comparare i branches tra differenti fork 
 Per usare Github per comparare branches tra differenti frok cambia l'url così
 
 `https://github.com/{user}/{repo}/compare/{foreign-user}:{branch}...{own-branch}`
 
 Per esmpio: 

`https://github.com/rails/rails/compare/byroot:master...master`

![branch tra differenti fork](https://camo.githubusercontent.com/ba19bca61911b7ef26407cd70ae653d294436ba04027e80435b92e18df1bf5cd/687474703a2f2f692e696d6775722e636f6d2f513157367163422e706e67)

---

### Gists

I [Gists](https://gist.github.com/) sono un metodo semplice per lavorare con semplici porzioni di codice senza creare un repo intero 

![Gists](https://camo.githubusercontent.com/b2b68822342ca596cbd83fc3f56b9be18636247e619481a519ce8da765e2bf64/687474703a2f2f692e696d6775722e636f6d2f566b4b49314c432e706e673f31)

Aggiungi `.plib` alla fine di qualiasi gists url per vere un la versione HTML-only per essere integrata in qualsiasi sito.

I Gists possono essere trattati come ogni repo ed essere clonati.

`$ git clone https://gist.github.com/tiimgreen/10545817`

![Gists-terminal](https://camo.githubusercontent.com/e3ba38985748fd910017b002f4d406c82528ff8657fadaef1490449cfe8993b3/687474703a2f2f692e696d6775722e636f6d2f4263467a6162702e706e67)

Questo vuol dire che puoi modificarli e fare il push:

```bash
$ git commit
$ git push
Username for 'https://gist.github.com':
Password for 'https://tiimgreen@gist.github.com':
```

Comunque, I Gists non supportano le directories. Tutti i file vanno aggiunti nel root del repo.

---

### Git.io

[Git.io](git.io) è un semplice url shortener per Github 

![Git.io](https://camo.githubusercontent.com/30d4e3343efecda9f30ee4eaa2671b10424969662e0eac58736d0f6e0faadd9d/687474703a2f2f692e696d6775722e636f6d2f364a55666263472e706e673f31)

Puoi anche usarlo con Curl:

```bash
$ curl -i http://git.io -F "url=https://github.com/..."
HTTP/1.1 201 Created
Location: http://git.io/abc123

$ curl -i http://git.io/abc123
HTTP/1.1 302 Found
Location: https://github.com/...
``` 

---

### Shortcut

Quando sei in un repo le shortcut ti aiuano a muoverti più velocemente

*  `t`  aprirà il file explorer
* `w` aprirà il selettore dei branches
* `s` aprirà la barra di ricera, premendo `↓ ` cercherai in tutto GitHub
* `l` editerà i labels nelle issues esistenti

Premi `?` per vedere tutte le shortcut

![Shortcut](https://camo.githubusercontent.com/8de5a286c28a44d71ee1aad9482f6bf5be099e38c7f19f2ea90fdfcd2e35e1c0/687474703a2f2f692e696d6775722e636f6d2f79355a664e456d2e706e67)

---

### Evidenziare le linee nei repo

Aggiungendo `#L52` alla fine dell'url Github evidenzierà la linea 52 del file

Funziona anche con in range, es: `#L53-L60`

`https://github.com/rails/rails/blob/master/activemodel/lib/active_model.rb#L53-L60`

![linehg](https://camo.githubusercontent.com/b441e45e8f703462f5ddfa86aaa49d199870c1251ba252f017153e94aa7bd754/687474703a2f2f692e696d6775722e636f6d2f3841686a72437a2e706e67)

 ---
 
### Chiudere le issues tramite messaggio di commit
 
 Se un dato commit chiude una issue, qualsiasi keyword `fix/fixed/fixes/close/closed/closes/resolve/resolved/resolves`, seguita dal numero della issue, chiuderà la stessa.
 
 `$ git commit -m "Fix screwup, fixes #12"`
 
 Chiuderà la issue 12
 
 ![closed#12](https://camo.githubusercontent.com/311d05dd7e1497828729205599b201d5c9a73d81e4a040c9c83dc484796930b9/687474703a2f2f692e696d6775722e636f6d2f556831675a64782e706e67)
 
 ---
 
### Cross-link issues
 
Se vuoi linkare ad un'altra issue nello stesso repo, semplicemnte digita `#` e il numero della issue, e sarà auto-linkata 
 
 ![cross-link](https://camo.githubusercontent.com/447e39ab8d96b553cadc8d31799100190df230a8/68747470733a2f2f6769746875622d696d616765732e73332e616d617a6f6e6177732e636f6d2f626c6f672f323031312f736563726574732f7265666572656e6365732e706e67)
 
 ---
 
### Conversazioni chiuse
 
 Le pull requests e le issues possono ora essere chiuse dall'autore o dai collaboratori del repo
 
 ![locked convs](https://cloud.githubusercontent.com/assets/2723/3221693/bf54dd44-f00d-11e3-8eb6-bb51e825bc2c.png)
 
 Questo significa le persone che non sono collaboratori non potranno commentare 
 
 ![cannot comment](https://cloud.githubusercontent.com/assets/2723/3221775/d6e513b0-f00e-11e3-9721-2131cb37c906.png)
 
 ---
 
### Filtri
 
Sia le issues che le Pull requests permettono i filtri, ma **solo** nell'interfaccia grafica
 
Se voglio filtrare le issues per il label "activerecord":
 
`is:issue label:activerecord`
 
ma possono anche filtrare per il issue che **non** contengono questo label:
 
`is:issue -label:activerecord` 
 
Nel caso che sia un pull request:

`is:pr label:activerecord`

---

### Syntax highlighting nei file Markdown 

Per esempio:

Voglio usare la sintassi di ruby, scriverò:

```
```ruby
require 'tabbit'
table = Tabbit.new('Name', 'Email')
table.add_row('Tim Green', 'tiimgreen@gmail.com')
puts table.to_s```
```

Il risultato sarà:

```ruby
require 'tabbit'
table = Tabbit.new('Name', 'Email')
table.add_row('Tim Green', 'tiimgreen@gmail.com')
puts table.to_s
```

---

### Mettere velocemente una licenza

Quando crei un repo Github ti da la possibilità di mettere una licenza:

![license on create](https://camo.githubusercontent.com/2991dfedfd775a81bc9bd95b84fe64cf4cb6772009b8821afe54e521b620fbee/687474703a2f2f692e696d6775722e636f6d2f4368716a3446672e706e67)

Puoi aggiungerla in un repo già esistente creando un nuovo file e chiamandolo `LICENSE`, Github ti darà le opzioni dei vari templates:

![LICENSE](https://camo.githubusercontent.com/54d5ef806a98799ac564b397c4daaaa0ef269d0924b551db40ed82328fd95aaa/687474703a2f2f692e696d6775722e636f6d2f66546a516963742e706e67)

---

### Lista di task

Nelle issues,  pull requests e documenti Markdown le checkboxes possono essere aggiunte seguendo la seguente sintassi, **faii attenzione agli spazi**

```
- [ ] Be awesome
- [ ] Prepare dinner
  - [ ] Research recipe
  - [ ] Buy ingredients
  - [ ] Cook recipe
- [ ] Sleep
```

Il risultato sarà:

![tasklist](https://camo.githubusercontent.com/7192b7f5fae5af26010aec837af98d7dc9bcb4a1a9c3618b36d76b5477ff319c/687474703a2f2f692e696d6775722e636f6d2f6a4a42586873592e706e67)

O per il markdown:

- [ ] Be awesome
- [ ] Prepare dinner
  - [ ] Research recipe
  - [ ] Buy ingredients
  - [ ] Cook recipe
- [ ] Sleep

Quando voglio spuntare una casella, il codice sarà:

```
- [ ] Mercury
- [x] Venus
- [x] Earth
  - [x] Moon
- [x] Mars
  - [ ] Deimos
  - [ ] Phobos
```

Il risultato sarà:

- [ ] Mercury
- [x] Venus
- [x] Earth
  - [x] Moon
- [x] Mars
  - [ ] Deimos
  - [ ] Phobos
 
---

### Link relativi

I link relativi sono raccomandati nel tuo file Markdown quando devi linkare conetenuti interni 
```markdwon
[Link to a header](#awesome-section)
[Link to a file](docs/readme)
``` 

---

### Anullare una pull request

Dopo aver fatto il merge di una pull request, potresti accorgerti di aver sbagliato, o che sia stata una pessima decisione fare il merge di quella pull request.

Puoi annullarla cliccando il pulsante **Revert** nella parte destra del commit in una pull request, per creare una nuova pull request con le modifiche annullate.

![revertpr](https://camo.githubusercontent.com/0d3350caf2bb1cba53123ffeafc00ca702b1b164/68747470733a2f2f6769746875622d696d616765732e73332e616d617a6f6e6177732e636f6d2f68656c702f70756c6c5f72657175657374732f7265766572742d70756c6c2d726571756573742d6c696e6b2e706e67)

