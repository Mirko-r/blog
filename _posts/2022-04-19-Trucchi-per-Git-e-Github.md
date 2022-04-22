---

layout: post
title: "Trucchi per Git e Github"
categories: trucchi

---

*Una collezione di features nascoste e non di Git e Github.*

## Github

> GitHub è un servizio di hosting per progetti software. Il nome deriva dal fatto che "GitHub" è una implementazione dello strumento di controllo versione distribuito Git.

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

```
https://github.com/Mirko-r/blog/commits/main?author=Mirko-r
```

---

### Clonare un repository

Quando si clona un repo il `.git` al fondo può essere tralasciato

```
$> git clone https://github.com/Mirko-r/blog
```

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
 
 ```
 https://github.com/rails/rails/compare/master...4-1-stable
 ```
 
 ![range img 1](https://camo.githubusercontent.com/d5e35a2c90ebae61da245ce12c54a84469c50e2792fd71f5b5a7762424f4b495/687474703a2f2f692e696d6775722e636f6d2f744952434f734b2e706e67)
 
 il valore `{range}` può essere cambiato per cose tipo:
 
 ```
https://github.com/rails/rails/compare/master@{1.day.ago}...master
https://github.com/rails/rails/compare/master@{2014-10-04}...master
```


Ecco un esempio di `{range}` con la data `YYYY-MM-DD`

![range date](https://camo.githubusercontent.com/421a6e6a2a43eb9ac111e968574a13ea8f7c122b85e0326d6702c82285cff6ca/687474703a2f2f692e696d6775722e636f6d2f3564747a45537a2e706e67)

I branches possono essere comparati anche per `diff` o `path` :

```https://github.com/rails/rails/compare/master...4-1-stable.diff
https://github.com/rails/rails/compare/master...4-1-stable.patch
```



#### Comparare i branches tra differenti fork 
 Per usare Github per comparare branches tra differenti frok cambia l'url così
 
 ```
 https://github.com/{user}/{repo}/compare/{foreign-user}:{branch}...{own-branch}
 ```
 
 Per esmpio: 

```
https://github.com/rails/rails/compare/byroot:master...master
```

![branch tra differenti fork](https://camo.githubusercontent.com/ba19bca61911b7ef26407cd70ae653d294436ba04027e80435b92e18df1bf5cd/687474703a2f2f692e696d6775722e636f6d2f513157367163422e706e67)

---

### Gists

I [Gists](https://gist.github.com/) sono un metodo semplice per lavorare con semplici porzioni di codice senza creare un repo intero 

![Gists](https://camo.githubusercontent.com/b2b68822342ca596cbd83fc3f56b9be18636247e619481a519ce8da765e2bf64/687474703a2f2f692e696d6775722e636f6d2f566b4b49314c432e706e673f31)

Aggiungi `.plib` alla fine di qualiasi gists url per vere un la versione HTML-only per essere integrata in qualsiasi sito.

I Gists possono essere trattati come ogni repo ed essere clonati.

```
$ git clone https://gist.github.com/tiimgreen/10545817
```

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

```
https://github.com/rails/rails/blob/master/activemodel/lib/active_model.rb#L53-L60
```


![linehg](https://camo.githubusercontent.com/b441e45e8f703462f5ddfa86aaa49d199870c1251ba252f017153e94aa7bd754/687474703a2f2f692e696d6775722e636f6d2f3841686a72437a2e706e67)

 ---
 
### Chiudere le issues tramite messaggio di commit
 
Se un dato commit chiude una issue, qualsiasi keyword `fix/fixed/fixes/close/closed/closes/resolve/resolved/resolves`, seguita dal numero della issue, chiuderà la stessa.
 
 ```
 $ git commit -m "Fix screwup, fixes #12
 ```
 
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

Nelle issues,  pull requests e documenti Markdown le checkboxes possono essere aggiunte seguendo la seguente sintassi, **fai attenzione agli spazi**

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

---

### Diffs

#### Mappe "diffabili" 
 Ogni volta che vedi un commit o una pull request che contiene geodata. Github renderizzerà una rappresentazione visuale di cosa è cambiato.
 
[![Diffable Maps](https://f.cloud.github.com/assets/282759/2090660/63f2e45a-8e97-11e3-9d8b-d4c8078b004e.gif)](https://github.com/benbalter/congressional-districts/commit/2233c76ca5bb059582d796f053775d8859198ec5)

#### Renderizzare e fare il diff di immagini

Github può mostrare tutti i maggiori formati di immagini, JPG, PNG, GIF e PSD. In aggiunta, ci sono molti modi per comparare le differenze tra le versioni di questi formati.

[![Diffable PSD](https://cloud.githubusercontent.com/assets/2546/3165594/55f2798a-eb56-11e3-92e7-b79ad791a697.gif)](https://github.com/blog/1845-psd-viewing-diffing)

---
### Hub

[Hub](https://github.com/github/hub) è un wrapper di git da riga di comando, che ti fornisce features e comandi extra che ti permettono di lavorare con Github più facilmente.

`$ hub clone tiimgreen/toc`

---
### Octicons

L'icona di GitHub, Octicons, è ora open source

![octicons](https://camo.githubusercontent.com/0af1e3241d7517e49c172e4a9f03b9bbce7247a33e41a5aa0efc9605f4ec99ff/68747470733a2f2f6f672e6769746875622e636f6d2f6f637469636f6e732f6f637469636f6e734031323030783633302e706e67)

---

### Github pack per gli sviluppatori 

Se sei uno studenti, e rispettii tutti i requisiti per avere qesto pack. Ti da un credito gratis, prove gratis e acesso in anticipo per software che ti aiutano quando programmi.

![studenti pack](https://camo.githubusercontent.com/c2f8441ca6085caf3c90c1ef04a2b6b24ed7af9074b4550d1b11601586f3da35/687474703a2f2f692e696d6775722e636f6d2f397275334b34332e706e67)

---

### Siti di Github 

| Title | Link |
| ----- | ---- |
| GitHub Explore |[github.com/explore](https://github.com/explore) |
| GitHub Blog |[github's blog](https://github.com/blog) |
| GitHub Help |[help.github.com](https://help.github.com/) |
| GitHub Training | [training.github.com](https://training.github.com/) |
| GitHub Developer | [developer.github.com](https://developer.github.com/) |
| Github Education (Free Micro Account and other stuff for students) | [education.github.com](https://education.github.com/ )|
| GitHub Best Practices | [Best Practices List](https://www.datree.io/resources/github-best-practices) |

---

### Chiavi ssh

Puoi vedere tutte le chiavi ssh di un utente in chiaro visitando:

`https://github.com/{user}.keys`

### Immagine del profilo 

Puoi avere l'immagine del profilo di un utente visitando:

`https://github.com/{user}.png`

____

## Git

>Git è un software per il controllo di versione distribuito utilizzabile da interfaccia a riga di comando, creato da Linus Torvalds nel 2005.

--- 

### Rimuovere tutti i file eliminati dall'albero di lavoro

Quando rimuovi tanti file usando `rm` puoi usare questo comando per rimuoverli tutti dall'albero di lavoro.

`$ git rm $(git ls-files -d)`

Per esempio:

```bash
$ git status
On branch master
Changes not staged for commit:
	deleted:    a
	deleted:    c

$ git rm $(git ls-files -d)
rm 'a'
rm 'c'

$ git status
On branch master
Changes to be committed:
	deleted:    a
	deleted:    c
```

---

### Branch precedente

Per muoversi nel branch precedente in Git:

```bash
$ git checkout -
# Switched to branch 'master'

$ git checkout -
# Switched to branch 'next'

$ git checkout -
# Switched to branch 'master'
```

---

### Commit vuoti

i commit possono essere pushati senza modifiche nel codice con `--alow-empty`:

`$ git commit -m "Big-ass commit" --allow-empty`

Qualche caso d'uso (sensato):
- Annotare l'inizio dello sviluppo di un nuovo software/feature
- Documentare cambiamenti non collegati al codice
- Comunicare con le persone usando il repo
- Primo commit di un repo ` git commit -m "Initial commit" --allow-empty`

---

### Styled Git status

Eseguendo:

`$ git status`

Il risultato sarà:

![gs](https://camo.githubusercontent.com/79baa54cdc4691d29cb460671b781fa3965f7b8a1679745f1622dced815a875d/687474703a2f2f692e696d6775722e636f6d2f716a50797658622e706e67)

Aggiungendo `-sb`:

`$ git status -sb`

Il risultato sarà:

![gssb](https://camo.githubusercontent.com/4c43c56d8661235e55b27f30c308a502912ee8db87ed475e40460f30c37716f6/687474703a2f2f692e696d6775722e636f6d2f4b304f59336e6d2e706e67)

---

### Styled Git log

Eseguendo:

```bash
$ git log --all --graph --pretty=format:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold
blue)<%an>%Creset' --abbrev-commit --date=relative
```

il risultato sarà:

![gl](https://camo.githubusercontent.com/41f05fe47382428969483b6f15016b63eebba0616601bc789f704e3c4735f52d/687474703a2f2f692e696d6775722e636f6d2f3538654f746b572e706e67)

---

### Configurazioni

Il file `.gitconfig` contiene le configurazioni per Git

#### Aliases

Gli aliases aiutano a definire comandi personali che richiamano ad altri comandi.
Per esmpio puoi settare `git a` per eseguire `git add --all`.

Per aggiungere alias in Git, devi editare il file `~/.gitconfig` nel seguente formato:

```
[alias]
  co = checkout
  cm = commit
  p = push
  # Show verbose output about tags, branches or remotes
  tags = tag -l
  branches = branch -a
  remotes = remote -v
```

O scrivendo direttamente nel terminale 

```
$ git config --global alias.new_alias git_function
```

Per un alias con comandi multipli usa le virgolette:

```
$ git config --global alias.ac 'add -A . && commit'
```

Qualche alias utile:

| Alias | Comando |Cosa scrivere da CLI |
| --- | --- | --- |
| `git cm` | `git commit` | `git config --global alias.cm commit` |
| `git co` | `git checkout` | `git config --global alias.co checkout` |
| `git ac` | `git add . -A` `git commit` | `git config --global alias.ac '!git add -A && git commit'` |
| `git st` | `git status -sb` | `git config --global alias.st 'status -sb'` |
| `git tags` | `git tag -l` | `git config --global alias.tags 'tag -l'` |
| `git branches` | `git branch -a` | `git config --global alias.branches 'branch -a'` |
| `git cleanup` | `git branch --merged \| grep -v '*' \| xargs git branch -d` | `git config --global alias.cleanup "!git branch --merged \| grep -v '*' \| xargs git branch -d"` |
| `git remotes` | `git remote -v` | `git config --global alias.remotes 'remote -v'` |
| `git lg` | `git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --` | `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"` |

#### Auto correzione

Git ti da suggerrmenti per comandi con errori, se l'auto correzione è abilitata, li corregerà ed eseguirà automaticamente. L'auto correzione si abilita specificando un intero che è il delay in secondi prima che git eseguirà il comando corretto.

Per esmpio, se scrivi `git comit`, il risultato sarà:

```bash
$ git comit -m "Message"
# git: 'comit' is not a git command. See 'git --help'.

# Did you mean this?
#   commit
```

L'auto correzione può essere abilitatà così (con un delat di 1.5 secondi):

`$ git config --global help.autocorrect 15`

Quindi ora scrivendo `git comit`, il risultato sarà:

```bash
$ git comit -m "Message"
# WARNING: You called a Git command named 'comit', which does not exist.
# Continuing under the assumption that you meant 'commit'
# in 1.5 seconds automatically...
```
#### Colori

Per aggiungere più colori all'output di Git:

`$ git config --global color.ui 1`

--- 
