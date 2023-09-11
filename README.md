# URI-parse
Parsing di string URI

Il progetto comprende una libreria che permette l'analisi e la scomposizione di stringhe in formato URI nelle varie componenti. Queste componenti vengono poste all'interno di una struttura se l'URI è formato correttamente. 

Sintassi URI
Il progetto prevede l'analisi di URI la cui sintassi è stata semplificata ed è riportata
- URI ::= URI1 | URI2
- URI1 ::= scheme ‘:’ [authorithy] [[‘/’] [path] [‘?’ query] [‘#’ fragment]]
- URI2 ::= scheme `:’ scheme-syntax
- scheme ::= <identificatore>
- authorithy ::= ‘//’ [ userinfo ‘@’ ] host [‘:’ port]
- userinfo ::= <identificatore>
- host ::= <identificatore-host> [‘.’ <identificatore-host>]* | indirizzo-IP
- port ::= <digit>+
- indirizzo-IP ::= <NNN.NNN.NNN.NNN – con N un digit>
- path ::= <identificatore> [‘/’ <identificatore>]*
- query ::= <caratteri senza ‘#’>+
- fragment ::= <caratteri>+
- <identificatore> ::= <caratteri senza ‘/’, ‘?’, ‘#’, ‘@’, e ‘:’>+
- <identificatore-host> ::= <caratteri senza ‘.’, ‘/’, ‘?’, ‘#’, ‘@’, e ‘:’>+
- <digit> ::= ‘0’ | ‘1’ | ‘2’ | ‘3’ | ‘4’ | ‘5’ | ‘6’ | ‘7’ | ‘8’ | ‘9’
- scheme-syntax ::= <sintassi speciale – si veda sotto>

Sintassi speciali
- Schema mailto: scheme-syntax ::= [userinfo [‘@’ host]]
- Schema news: scheme-syntax ::= [host]
- Schemi tel e fax: scheme-syntax ::= [userinfo]
- Schema zos: 
path ::= <id44> [‘(’ <id8> ‘)’]
id44 ::= (<caratteri alfanumerici> | ‘.’)+
id8 ::= (<caratteri alfanumerici>)+
 Gli altri campi (userinfo, host, port, query, fragment), sono 
da riconoscere normalmente come nella produzione URI1.

dove la lunghezza di id44 è al massimo 44 e quella di id8 è al massimo 8. Inoltre, id44 e 
id8 devono iniziare con un carattere alfabetico; id44 non può terminare con un ‘.’.

Nella creazione della libreria Lisp la stringa fornita in input viene convertita in una lista che contiene i caratteri. Questi vengono controllati uno ad uno attraverso la ricorsione: analizzato il primo carattere, esso viene salvato nell'apposita lista posta all'interno di una lista che contiene tutti i campi dell'uri, e infine viene chiamata la funzione sul resto della lista. Nell'implementazione della struttura in cui salvare le componenti dell'URI è stata utilizzata una lista, che a sua volta contiene tante liste tanti quanti sono i campi dell'uri.

Uso della libreria

Richiede **LispWorks**.
Bisogna caricare il file e successivamente far partire il comando:

(defparameter *nome* (uri-parse *stringa-uri*))
(uri-display *nome*)

Funzioni
- uri-parse: analizza l'URI e fornisce la struct se l'URI è formato correttamente
- uri-display: fornisce tutte le informazioni inserite nella struct
- uri-scheme: ritorna lo scheme
- uri-userinfo: ritorna lo userinfo
- uri-host: ritorna l'host
- uri-port: ritorna la porta
- uri-path: ritorna il percorso 
- uri-query: ritorna la query
- uri-fragment: ritorna il fragment

Tutti ritornano delle stringhe eccetto port che ritorna integer, provenenienti dalla struct associata all'URI.

!!Attenzione!!
Il programma prevede tante chiamate ricorsive (e non) tanti quanti sono i caratteri. Ne consegue che maggiore è il numero di caratteri e il livello di debug in cui si scende, maggiori sono le probabilità che il programma vada in stackoverflow. In questo caso usare il comando 

:top

per uscire dal debug e di disporre di maggiore memoria.
