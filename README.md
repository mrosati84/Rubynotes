# Appunti Ruby
--------------

``irb`` o ``pry``, console interattive simili a ``python`` e ``ipython``

gli oggetti hanno i metodi ``.class`` (mostra il nome della classe) e ``.methods`` (elenca tutti i metodi dell’oggetto).

## Definizione di metodi

Posso definire un metodo così:

```ruby
def add_and_power a, b
	(a + b) ** (a + b)
end
```

questo metodo diventa metodo della classe ``main`` (``self.main``, è di tipo ``Object``)


## Simboli

Un simbolo è un identificatore unico. Si definisce un simbolo con la notazione ``:mysymbol``.

Per dimostrare l'univocità dei simboli, consideriamo l'esempio

```ruby
a = :foo
b = :foo
a.equal? b
=> true
```

controesempio con le stringhe

```ruby
s1 = "Matteo"
s2 = "Matteo"
s1 == s2
=> true
```

Il contenuto delle stringhe è lo stesso. Tuttavia confrontanto con ``equal?`` ecco cosa si ottiene:

```ruby
s1.equal? s2
=> false
```

Quindi gli oggetti non sono gli stessi.

## User input/output

il metodo ``gets`` e ``puts`` rispettivamente ricevono in input e stampano in output dei dati. Riscriviamo l'esempio add_and_power in versione interattiva

```ruby
def add_and_power a, b
	(a + b) ** (a + b)
end

puts "input 1:"
in1 = gets

puts "input 2:"
in2 = gets

puts add_and_power in1.to_i, in2.to_i
```

# Flusso di esecuzione
----------------------

## ``if/else``

Il caso più semplice di ``if/else`` può essere questo

```ruby
if age < 18
	puts "it's a young person"
else
	puts "it's an adult"
end
```

E' possibile anche assegnare il risultato di un ``if/else`` ad unavariabile

```ruby
output = if age < 10
			"It's a young person"
		elsif age < 18
			"It's a teenager"
		elsif age < 45
			"It's an adult"
		elsif age < 95
			"It's an old person"
		end

puts output
```

L'operatore ``if`` può anche essere messo *dopo* il blocco di esecuzione, aumentando l'espressività del codice:

	doSomething if condition == true

Esiste anche la condizione ``unless``, il cui significato è l'esatto opposto di ``if``. Considerando l'esempio precedente, si avrebbe:

	doSomething unless condition == false

## Operatore ternario

L'operatore ternario è identico a PHP e Javascript:

```ruby
output = age < 18 ? "He's a teenager" : "He's adult"
```

## ``case/when`` (o ``switch/case``)

Consideriamo l'esempio

```ruby
print "Tell me a car model"
car_model = gets.strip

output = case car_model
			when "Focus", "Fiesta" then "Ford"
			when "Ibiza" then "Seat"
			when "Civic" then "Honda"
			else "Unknown model"
		end

puts "The company for model #{car_model} is ", output
```

Abbiamo usato il metodo ``print`` invece di ``puts`` perché il primo non aggiunge automaticamente un carattere newline alla fine della stringa.

## Loops

In Ruby esistono 7 modi diversi di creare dei loop. Il primo modo è usando la parola chiave ``loop``. Tutti i loop prendono in argomento un blocco.

```ruby
number = 0
loop do
	break if number > 15
	puts "The number inside the loop is #{number}"
	number += 1
end
```

Questo tipo di loop non è molto usato. E' possibile riscrivere questo loop usando la parola chiave ``until``:

```ruby
number = 0
until number > 15 do
	puts "The number inside the loop is #{number}"
	number += 1
end
```

E' ancora possibile riscrivere questo esempio usando questa volta la parola chiave ``while``. Il comportamento è opposto al precedente, ed è necessario invertire la condizione del loop da ``>`` a ``<=``:

```ruby
number = 0
while number <= 15 do
	puts "The number inside the loop is #{number}"
	number += 1
end
```

Possiamo anche appoggiarci ad un metodo della classe ``Fixnum`` per eseguire loop:

```ruby
16.times do |number|
	puts "The number inside the loop is #{number}"
end
```

Un altro metodo è quello di scorrere gli elementi di una lista, ad esempio:

```ruby
list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

list.each do |number|
	puts "The number inside the loop is #{number}"
end
```

Un modo migliore di sfruttare le liste è quello di usare un ``range``. Un ``range`` è un elenco di numeri, o di lettere, che può essere costruito usando l'operatore ``..``, che restituisce un oggetto di tipo ``Range``

```ruby
(0..15).each do |number|
	puts "The number inside the loop is #{number}"
end
```

E' possibile definire un range usando ``...`` invece di ``..``, la differenza sta nel fatto che il primo operatore *esclude* l'ultimo elemento del range. Riferendosi all'esempio precedente quindi, sarà necessario scrivere ``(0...16)`` al posto di ``(0..15)``.

L'ultimo modo possibile per costruire dei loop è l'uso di ``for in``. Per esempio, usando sempre la classe ``Range``, un loop ``for`` potrebbe essere così costruito:

```ruby
for number in 0..15 do
	puts "The number inside the loop is #{number}"
end
```

## Blocks

I blocchi servono per incapsulare funzionalità, e favorire un tipo di sviluppo funzionale. I blocchi possono essere definiti in due modi. Il modo più usato è tramite ``do`` ``end``

```ruby
do
	# some code
end
```

Il secondo modo è tramite parentesi graffe:

```ruby
{
	# some code
}
```

Consideriamo il seguente esempio per capire come funziona il passaggio di blocchi a funzioni:

```ruby
def form &block
	puts "<form>"
	yield
	puts "</form>"
end

def paragraph text
	puts "<p>" + text + "</p>"
end

def quote text
	puts "<blockquote>" + text + "</blockquote>"
end

form do
	paragraph "This is a paragraph"
	quote "This is a blockquote"
end
```

Questo esempio funziona solo se alla funzione ``form`` viene effettivamente passato un blocco.
Nel caso in cui nessun blocco venga passato alla funzione, si riceverebbe un errore. Modifichiamo quindi la funzione ``form`` in modo che possa non ricevere un blocco

```ruby
def form &block
	puts "<form>"
	yield if block_given?
	puts "</form>"
end

# ...
```

ora possiamo eseguire la funziona che senza passarle necessariamente un blocco.

# Procs e lambdas

In Ruby, procs e lambdas sono essenzialmente dei blocchi, ma invece di essere dichiarati in modo anonimo, vengono assegnati ad una variabile.
Prendiamo l'esempio precedente, usando però i proc.

Esistono tre modi possibili per dichiarare un proc:

```ruby
myproc = Proc.new do
	# proc code
end
```

oppure

```ruby
myproc = proc do
	# proc code
end
```

e infine con la notazione *thin arrow*

```ruby
myproc = -> do
	# proc code
end

# alternativamente, usando le parentesi graffe

myproc = -> {
	# proc code
}
```

Riscriviamo ora l'esempio usando un oggetto proc. L'oggetto proc potrà anche ricevere un parametro

```ruby
def form_with_proc p
	puts "<form>"
	p.call true
	puts "</form>"
end

def paragraph text
	puts "<p>" + text + "</p>"
end

def quote text
	puts "<p>" + text + "</p>"
end

myproc = proc do |only_quotes|
	paragraph "This is a paragraph" unless only_quotes
	quote "This is a quote"
end
```

