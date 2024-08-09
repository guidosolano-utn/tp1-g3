```

BNF Pascal
https://condor.depaul.edu/ichu/csc447/notes/wk2/pascal.html

Los conrchetesenta indica que la secuencia puede repetirse cero o mas veces.


<programa> ::= program <identificador> ; <bloque> .

<identificador> ::= <letra> {<letra o dígito>}

<letra o dígito> ::= <letra> | <dígito>

<bloque> ::= <parte declaración de etiqueta> <parte definición de constante> <parte definición de tipo> <parte declaración de variable> <parte declaración de procedimiento y función> <parte de instrucción>

<parte declaración de etiqueta> ::= <vacío> | label <etiqueta> {, <etiqueta>} ;

<etiqueta> ::= <entero sin signo>

<parte definición de constante> ::= <vacío> | const <definición de constante> { ; <definición de constante>} ;

<definición de constante> ::= <identificador> = <constante>

<constante> ::= <número sin signo> | <signo> <número sin signo> | <identificador de constante> | <signo> <identificador de constante> | <cadena>

<número sin signo> ::= <entero sin signo> | <real sin signo>

<entero sin signo> ::= <dígito> {<dígito>}

<real sin signo> ::= <entero sin signo> . <entero sin signo> | <entero sin signo> . <entero sin signo> E <factor de escala> | <entero sin signo> E <factor de escala>

<factor de escala> ::= <entero sin signo> | <signo> <entero sin signo>

<signo> ::= + | -

<identificador de constante> ::= <identificador>

<cadena> ::= '<carácter> {<carácter>}'

<parte definición de tipo> ::= <vacío> | type <definición de tipo> {;<definición de tipo>};

<definición de tipo> ::= <identificador> = <tipo>

<tipo> ::= <tipo simple> | <tipo estructurado> | <tipo puntero>

<tipo simple> ::= <tipo escalar> | <tipo subrango> | <identificador de tipo>

<tipo escalar> ::= (<identificador> {,<identificador>})

<tipo subrango> ::= <constante> .. <constante>

<identificador de tipo> ::= <identificador>

<tipo estructurado> ::= <tipo array> | <tipo record> | <tipo set> | <tipo file>

<tipo array> ::= array [<tipo índice> {,<tipo índice>}] of <tipo componente>

<tipo índice> ::= <tipo simple>

<tipo componente> ::= <tipo>

<tipo record> ::= record <lista de campos> end

<lista de campos> ::= <parte fija> | <parte fija> ; <parte variante> | <parte variante>

<parte fija> ::= <sección de record> {;<sección de record>}

<sección de record> ::= <identificador de campo> {, <identificador de campo>} : <tipo> | <vacío>

<tipo variante> ::= case <campo etiqueta> <identificador de tipo> of <variante> { ; <variante>}

<campo etiqueta> ::= <identificador de campo> : | <vacío>

<variante> ::= <lista de etiquetas de caso> : ( <lista de campos> ) | <vacío>

<lista de etiquetas de caso> ::= <etiqueta de caso> {, <etiqueta de caso>}

<etiqueta de caso> ::= <constante>

<tipo set> ::= set of <tipo base>

<tipo base> ::= <tipo simple>

<tipo file> ::= file of <tipo>

<tipo puntero> ::= ^ <identificador de tipo>

<parte declaración de variable> ::= <vacío> | var <declaración de variable> {; <declaración de variable>} ;

<declaración de variable> ::= <identificador> {,<identificador>} : <tipo>

<parte declaración de procedimiento y función> ::= {<declaración de procedimiento o función> ;}

<declaración de procedimiento o función> ::= <declaración de procedimiento> | <declaración de función>

<declaración de procedimiento> ::= <encabezado de procedimiento> <bloque>

<encabezado de procedimiento> ::= procedure <identificador> ; | procedure <identificador> ( <sección de parámetro formal> {;<sección de parámetro formal>} );

<sección de parámetro formal> ::= <grupo de parámetros> | var <grupo de parámetros> | function <grupo de parámetros> | procedure <identificador> { , <identificador>}

<grupo de parámetros> ::= <identificador> {, <identificador>} : <identificador de tipo>

<declaración de función> ::= <encabezado de función> <bloque>

<encabezado de función> ::= function <identificador> : <tipo resultado> ; | function <identificador> ( <sección de parámetro formal> {;<sección de parámetro formal>} ) : <tipo resultado> ;

<tipo resultado> ::= <identificador de tipo>

<parte de instrucción> ::= <instrucción compuesta>

<instrucción> ::= <instrucción no etiquetada> | <etiqueta> : <instrucción no etiquetada>

<instrucción no etiquetada> ::= <instrucción simple> | <instrucción estructurada>

<instrucción simple> ::= <instrucción de asignación> | <instrucción de procedimiento> | <instrucción go to> | <instrucción vacía>

<instrucción de asignación> ::= <variable> := <expresión> | <identificador de función> := <expresión>

<variable> ::= <variable completa> | <variable componente> | <variable referenciada>

<variable completa> ::= <identificador de variable>

<identificador de variable> ::= <identificador>

<variable componente> ::= <variable indexada> | <designador de campo> | <buffer de archivo>

<variable indexada> ::= <variable array> [<expresión> {, <expresión>}]

<variable array> ::= <variable>

<designador de campo> ::= <variable record> . <identificador de campo>

<variable record> ::= <variable>

<identificador de campo> ::= <identificador>

<buffer de archivo> ::= <variable archivo>

<variable archivo> ::= <variable>

<variable referenciada> ::= <variable puntero>

<variable puntero> ::= <variable>

<expresión> ::= <expresión simple> | <expresión simple> <operador relacional> <expresión simple>

<operador relacional> ::= = | <> | < | <= | >= | > | in

<expresión simple> ::= <término> | <signo> <término> | <expresión simple> <operador de adición> <término>

<operador de adición> ::= + | - | or

<término> ::= <factor> | <término> <operador de multiplicación> <factor>

<operador de multiplicación> ::= * | / | div | mod | and

<factor> ::= <variable> | <constante sin signo> | ( <expresión> ) | <designador de función> | <set> | not <factor>

<constante sin signo> ::= <número sin signo> | <cadena> | <identificador de constante> | nil

<designador de función> ::= <identificador de función> | <identificador de función> ( <parámetro real> {, <parámetro real>} )

<identificador de función> ::= <identificador>

<set> ::= [ <lista de elementos> ]

<lista de elementos> ::= <elemento> {, <elemento> } | <vacío>

<elemento> ::= <expresión> | <expresión> .. <expresión>

<instrucción de procedimiento> ::= <identificador de procedimiento> | <identificador de procedimiento> ( <parámetro real> {, <parámetro real> } )

<identificador de procedimiento> ::= <identificador>

<parámetro real> ::= <expresión> | <variable> | <identificador de procedimiento> | <identificador de función>

<instrucción go to> ::= goto <etiqueta>

<instrucción vacía> ::= <vacío>

<vacío> ::= 

<instrucción estructurada> ::= <instrucción compuesta> | <instrucción condicional> | <instrucción repetitiva> | <instrucción con>

<instrucción compuesta> ::= begin <instrucción> {; <instrucción> } end;

<instrucción condicional> ::= <instrucción if> | <instrucción case>

<instrucción if> ::= if <expresión> then <instrucción> | if <expresión> then <instrucción> else <instrucción>

<instrucción case> ::= case <expresión> of <elemento de lista de case> {; <elemento de lista de case> } end

<elemento de lista de case> ::= <lista de etiquetas de case> : <instrucción> | <vacío>

<lista de etiquetas de case> ::= <etiqueta de case> {, <etiqueta de case> }

<instrucción repetitiva> ::= <instrucción while> | <instrucción repeat> | <instrucción for>

<instrucción while> ::= while <expresión> do <instrucción>

<instrucción repeat> ::= repeat <instrucción> {; <instrucción>} until <expresión>

<instrucción for> ::= for <variable de control> := <lista for> do <instrucción>

<variable de control> ::= <identificador>

<lista for> ::= <valor inicial> to <valor final> | <valor inicial> downto <valor final>

<valor inicial> ::= <expresión>

<valor final> ::= <expresión>

<instrucción con> ::= with <lista de variables record> do <instrucción>

<lista de variables record> ::= <variable record> {, <variable record>}

```
