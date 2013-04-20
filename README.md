# Appunti Ruby
--------------

``irb`` o ``pry``, console interattive simili a ``python`` e ``ipython``

gli oggetti hanno i metodi ``.class`` (mostra il nome della classe) e ``.methods`` (elenca tutti i metodi dell’oggetto).

## Definizione di metodi

Posso definire un metodo così:

	def add_and_power a, b
		(a + b) ** (a + b)
	end

questo metodo diventa metodo della classe ``main`` (``self.main``, è di tipo ``Object``)


## Simboli

Un simbolo è un identificatore unico. Si definisce un simbolo con la notazione ``:mysymbol``.

Per dimostrare l'univocità dei simboli, consideriamo l'esempio

	a = :foo
	b = :foo
	a.equal? b
	=> true

controesempio con le stringhe

	s1 = "Matteo"
	s2 = "Matteo"
	s1 == s2
	=> true

Il contenuto delle stringhe è lo stesso. Tuttavia confrontanto con ``equal?`` ecco cosa si ottiene:

	s1.equal? s2
	=> false

Quindi gli oggetti non sono gli stessi.

## User input/output

il metodo ``gets`` e ``puts`` rispettivamente ricevono in input e stampano in output dei dati. Riscriviamo l'esempio add_and_power in versione interattiva

	def add_and_power a, b
		(a + b) ** (a + b)
	end

	puts "input 1:"
	in1 = gets

	puts "input 2:"
	in2 = gets

	puts add_and_power in1.to_i, in2.to_i

# Flusso di esecuzione
----------------------

## ``if/else``

Il caso più semplice di ``if/else`` può essere questo

	if age < 18
		puts "it's a young person"
	else
		puts "it's an adult"
	end

E' possibile anche assegnare il risultato di un ``if/else`` ad unavariabile

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

L'operatore ``if`` può anche essere messo *dopo* il blocco di esecuzione, aumentando l'espressività del codice:

	doSomething if condition == true

Esiste anche la condizione ``unless``, il cui significato è l'esatto opposto di ``if``. Considerando l'esempio precedente, si avrebbe:

	doSomething unless condition == false

## Operatore ternario

L'operatore ternario è identico a PHP e Javascript:

	output = age < 18 ? "He's a teenager" : "He's adult"

## ``case/when`` (o ``switch/case``)