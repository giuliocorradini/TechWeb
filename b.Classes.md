# Classi in Python

Ricordiamo che gli attributi di istanza dinamici esistono nel momento in cui li uso, create nel "costruttore" come lo
chiamiamo con abuso di linguaggio, anche se ci riferiamo al metodo init.

La differenza tra gli attributi di istanza e di classe è esplicitata dalla variabile `self`. Ma ci sono altre nuance
dell'OOP che Python utilizza.

## Incapsulamento

In Java si tende a rendere private tutte le variabili di classe, e si usano getter e setter per accedere in modo da
stabilire le regole di scritture dei nostri attributi.

> There's no privacy in Python

Tuttavia un attributo può essere "dichiarato" in tre modi diversi, e l'interprete può fare mangling per venirci
incontro.

- `self.raggio`: attributo pubblico

- `self._raggio`: il singolo underscore indica che non bisognerebbe accedere

- `self.__raggio`: l'interprete fa mangling del nome aggiungendo `_Classe` davanti al nome. Accedibile quindi solo
dall'interno della classe.

```py
class Classe:
    def __init__(self, a, b, c):
        self.a = a
        self._b = b
        self.__c = c

ogg = Classe(1,2,3)

try:
    print(ogg.__c)
except:
    print("Non puoi accedere oh")
finally:
    print(ogg._Classe__c)
```

Secondo la filosofia dell'_import this_ non bisognerebbe usare getter/setter perché è tutto codice boilerplate. Ma se
dobbiamo fissare dei domini di appartenenza degli attributi ci vuole un modo furbo per farlo.

### Property

Il decoratore `@property` usato come annotazione crea un alias di un attributo, che potrebbe essere a sua volta un
attributo di istanza o classe, e mi permette di definire un getter e un setter.

La cosa comoda di property è che l'interprete chiama in automatico il getter e il setter.

## Ereditarietà

In Python non esistono interfacce, o il concetto di classe e metodi astratti (anche se si possono implementare con
metaclassi). Inoltre posso ereditare da più classi a differenza del Java dove posso solo estendere più interfacce.

Senza estensioni si può sollevare un'eccezione del tipo `NotImplementedError`, mentre il pass viene chiamato fallimento
silenzioso. Tuttavia sto istanziando una classe astratto e finché non chiamo quel metodo non mi accorgo di ciò.

È stato introdotto il modulo `abc` per creare delle vere e proprie classi astratte, che a tempo di parsing del codice
riesca a capire se sto usando in modo sbagliato (non previsto) le mie classi astratte.

## Attributi statici

Un attributo è statico se non dipende dallo stato della singola istanza, quindi è accessibile con lo stesso valore
da tutte le istanze. Per risolvere in Python basta omettere il self.

```py
print(o.static_attr)
print(ClassA.static_attr)
```

In Python il primo accesso è consentito, anche in Java ma quantomeno viene sollevato un warning di _dynamic access to
static attribute_.

## Metodi statici

Siamo nell'ambito dei design pattern. Abbiamo metodi statici puri e metodi statici di classe. L'uso principale che se
ne fa è per i **factory method**, ovvero metodi che creano degli oggetti per una classe.

Facciamo finta di avere una classe Persona, per un ufficio anagrafe. Qualcuno mi dice quanti anni ha per la creazione
dell'oggetto.

```py
class Persona:
    def __init__(self, nome, eta):
        self.nome = nome
        self.eta = eta

    ...
```

Ottima idea, ma adesso arriva qualcuno che mi dice in che anno è nato, io nel costruttore tuttavia mi aspetto l'età.
Lo posso ricavare dalla data corrente, quindi non modifico il costruttore ma prevedo un metodo statico di tipo classmethod
che si occupa per me della creazione.

```py
class Persona:
    ...

    @classmethod
    def da_anno(cls, nome, anno):
        return cls(nome, date.today().year - anno)
```

Rispetto a Java è molto più ovvio quando uno di questi metodi è una factory.

## Ereditarietà multipla

Google suggerisce di non utilizzare l'ereditarietà multipla in C++, anche se è perfettamente possibile farlo.
