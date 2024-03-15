# Python parallelism

L'interprete ha il GIL: global interpreter lock. Il risultato è che non si riesce ad avere un vero parallelismo.

Per operazioni di tipo I/O intensive si può fare asynchronous programming. Il building block della programmazione
asincrono è la coroutine, che come risultato restituisce dei Futures ovvero una promessa che in futuro si potrà risolvere
la promessa e avere il valore richiesto.

Il template per implementare un Future non esiste in Python, ma il concetto rimane lo stesso.

Le cose che hanno senso essere aspettate sono lunghe e impredicibili temporalmente: richieste HTTP, accessi su disco.

## asyncio

Async indica nella firma del metodo che sto definendo una coroutine, mentre await indica che chiamo un'altra coroutine
ovvero una funzione "da aspettare".

Fa tutto parte del modulo `asyncio`.

### gather

Schedula ed esegue gli awaitable object uno dopo l'altro.

### run

Simile a gather ma esegue ed **aspetta** il risultato della coroutine. È il punto di incontro tra i programmi non async
e le coroutine async

### None

Equivalente del null, fa parte dei nulltype ed è un singleton.

### Call a coroutine

Facciamo finta di avere una coroutine e di chiamarla

```py
async def funz_async():
    await asyncio.sleep()

a = funz_async()
a() # la call effettiva
```

La funzione viene effettivamente chiamata, ma viene sollevata un'eccezione di tipo `RuntimeWarning` visto che non "raccolgo"
il futuro della funzione.

Per lanciare correttmanete la funzione bisogna usare `run` dal modulo `asyncio`. Quindi **le funzione asincrone non si
chiamano come le classiche funzioni "sincrone"**.

Cosa succede invece se uso `gather`? In teoria non aspetto che la coroutine termini, quindi gather ritorna un oggetto
di tipo `GatheringFuture` ovvero la promessa di avere un risultato.

## ASGI

Django può comunicare con i webserver attraverso il protocollo ASGI. Server per Websockets che permette di aumentare la
capacità dinamica del nostro server.

ASGI è il successore di WSGI e permette di implementare web server che gestiscono canali asincroni, come Websockets.
Rispetto a WSGI utilizza `asyncio`.
