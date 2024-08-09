<sup>PHP :</sup> 
PHP tal y como se conoce hoy en día es en realidad el sucesor de un producto llamado PHP/FI. Creado en 1994 por Rasmus Lerdorf, la primera encarnación de PHP era un conjunto simple de ficheros binarios Common Gateway Interface (CGI) escritos en el lenguaje de programación C. Originalmente utilizado para rastrear visitas de su currículum online, llamó al conjunto de scripts "Personal Home Page Tools", más frecuentemente referenciado como "PHP Tools". Con el paso del tiempo se quiso más funcionalidad, y Rasmus reescribió PHP Tools, produciendo una implementación más grande y rica. Este nuevo modelo fue capaz de interaccionar con bases de datos, y mucho más, proporcionando un entorno de trabajo sobre cuyos usuarios podían desarrollar aplicaciones web dinámicas sencillas tales como libros de visitas. En junio de 1995, Rasmus » publicó el código fuente de PHP Tools, lo que permitió a los desarrolladores usarlo como considerasen apropiado. Esto también permitió -y animó- a los usuarios a proporcionar soluciones a los errores del código, y generalmente a mejorarlo.

En septiembre de ese mismo año, Rasmus amplió PHP y -por un corto periodo de tiempo- abandonó el nombre de PHP. Ahora, refiriéndose a las herramientas como FI (abreviatura de "Forms Interpreter"), la nueva implementación incluía algunas de las funciones básicas de PHP tal y como la conocemos hoy. Tenía variables como las de Perl, interpretación automática de variables de formulario y sintaxis incrustada HTML. La sintaxis por sí misma era similar a la de Perl, aunque mucho más limitada, simple y algo inconsistente. De hecho, para embeber el código en un fichero HTML, los desarrolladores tenían que usar comentarios de HTML. Aunque este método no era completamente bien recibido, FI continuó gozando de expansión y aceptación como una herramienta CGI --- pero todavía no completamente como lenguaje. Sin embargo, esto comenzó a cambiar al mes siguiente; en octubre de 1995 Rasmus publicó una versión nueva del código. Recordando el nombre PHP, ahora era llamado (resumidamente) "Personal Home Page Construction Kit," y fue la primera versión que presumía de ser, en aquel momento, considerada como una interfaz de scripts avanzada. El lenguaje fue deliberadamente diseñado para asemejarse a C en estructura, haciéndolo una adopción sencilla para desarrolladores familiarizados con C, Perl, y lenguajes similares. Habiendo sido así bastante limitado a sistemas UNIX y compatibles con POSIX, el potencial para una implementación de Windows NT estaba siendo explorada.

El código fue completamente rehecho de nuevo, y en abril de 1996, combinando los nombres de versiones anteriores, Rasmus introdujo PHP/FI. Esta implementación de segunda generación comenzó realmente a desarrollar PHP desde un conjunto de herramientas dentro de un lenguaje de programación de derecho propio. Incluía soporte interno para DBM, mSQL, y bases de datos Postgres95, cookies, soporte para funciones definidas por el usuario, y mucho más. Ese mes de junio, PHP/FI brindó una versión 2.0. Sin embargo, un interesante hecho sobre esto, es que sólo había una única versión completa de PHP 2.0. Cuando finalmente pasó de la versión beta en noviembre de 1997, el motor de análisis subyacente ya estaba siendo reescrito por completo.

Aunque vivió una corta vida de desarrollo, continuó gozando de un crecimiento de popularidad en el aún joven mundo del desarrollo. En 1997 y 1998, PHP/FI tenía un culto de varios miles de usuarios en todo el mundo. Una encuesta de Netcraft en mayo de 1998 indicó que cerca de 60,000 dominios reportaron que tenían cabeceras que contenían "PHP", indicando en efecto que el servidor host lo tenía instalado. Este número se correspondía con aproximadamente el 1% de todos los dominios de Internet del momento. A pesar de estas impresionantes cifras, la maduración de PHP/FI estaba condenada por limitaciones; mientras habían varios contribuidores menores, aún era desarrollado principalmente por un individuo.


<sub>Su EBNF:</sub> 

##-------------------------------------------------------------------------------------##
```
# PHP 5.2.0 EBNF Syntax
# Converted from the yacc syntax, see file Zend/zend_language_parser.y
# by Umberto Salsi <salsi@icosaedro.it>.

# This file last updated: 2007-03-16

# Added the "define();" statement.
# Added some symbols from the scanner.

# FIXME: missing definition for T_INLINE_HTML.
# FIXME: missing definition for T_ENCAPSED_AND_WHITESPACE.
# FIXME: missing definition for T_CHARACTER.

PHP_SOURCE_TEXT = { inner_statement | halt_compiler_statement };

halt_compiler_statement = "__halt_compiler" "(" ")" ";" ;

inner_statement = statement
	| function_declaration_statement
	| class_declaration_statement ;

inner_statement_list = { inner_statement } ;

statement = "{" inner_statement_list "}"
	| "if" "(" expr ")" statement {elseif_branch} [else_single]
	| "if" "(" expr ")" ":" inner_statement_list {new_elseif_branch}
	  [new_else_single] "endif" ";"
	| "while" "(" expr ")" while_statement
	| "do" statement "while" "(" expr ")" ";"
	| "for" "(" for_expr ";" for_expr ";" for_expr ")" for_statement
	| "switch" "(" expr ")" switch_case_list
	| "break" [expr] ";"
	| "continue" [expr] ";"
	| "return" [expr_without_variable | variable] ";"
	| "global" global_var {"," global_var} ";"
	| "static" static_var { "," static_var } ";"
	| "echo" echo_expr_list ";"
	| T_INLINE_HTML
	| expr ";"
	| "use" use_filename ";" # FIXME: not implemented
	| "unset" "(" variable {"," variable} ")" ";"
	| "foreach" "(" (variable|expr_without_variable)
	  "as" foreach_variable ["=>" foreach_variable] ")"
	  foreach_statement
	| "declare" "(" declare_list ")" declare_statement
	| ";" # empty statement
	| "try" "{" inner_statement_list "}" catch_branch {catch_branch}
	| "throw" expr ";" ;

catch_branch = "catch" "(" fully_qualified_class_name T_VARIABLE ")" "{"
	  inner_statement_list "}" ;

use_filename = T_CONSTANT_ENCAPSED_STRING
	| "(" T_CONSTANT_ENCAPSED_STRING ")" ;

function_declaration_statement = "function" ["&"] T_STRING
	"(" parameter_list ")" "{" inner_statement_list "}" ;

class_declaration_statement = class_entry_type T_STRING
	[extends_from] [implements_list] "{" {class_statement} "}"
	| "interface" T_STRING [interface_extends_list] "{" {class_statement} "}" ;

class_entry_type = [ "abstract" | "final" ] "class" ;

extends_from = "extends" fully_qualified_class_name ;

interface_extends_list = "extends" interface_list ;

implements_list = "implements" interface_list ;

interface_list = fully_qualified_class_name { "," fully_qualified_class_name } ;

foreach_variable = ["&"] variable ;

for_statement = statement
	| ":" inner_statement_list "endfor" ";" ;

foreach_statement = statement
	| ":" inner_statement_list "endforeach" ";" ;

declare_statement = statement
	| ":" inner_statement_list "enddeclare" ";" ;

declare_list = T_STRING "=" static_scalar { "," T_STRING "=" static_scalar } ;

switch_case_list = "{" [";"] {case_list} "}"
	| ":" [";"] {case_list} "endswitch" ";" ;

case_list = "case" expr [":"|";"] inner_statement_list
	| "default" [":"|";"] inner_statement_list ;

while_statement = statement
	| ":" inner_statement_list "endwhile" ";" ;

elseif_branch = "elseif" "(" expr ")" statement ;

new_elseif_branch = "elseif" "(" expr ")" ":" inner_statement_list ;

else_single = "else" statement ;

new_else_single = "else" ":" inner_statement_list ;

parameter_list = [ parameter {"," parameter} ] ;

parameter = [T_STRING | "array"] ["&"] T_VARIABLE ["=" static_scalar] ;

function_call_parameter_list = [ function_call_parameter
	{ "," function_call_parameter } ] ;

function_call_parameter = expr_without_variable
	| variable
	| "&" w_variable ;

global_var = T_VARIABLE
	| "$" r_variable
	| "$" "{" expr "}" ;

static_var = T_VARIABLE [ "=" static_scalar ] ;

class_statement = variable_modifiers class_variable_declaration
	{"," class_variable_declaration} ";"
	| "const" class_constant_declaration {"," class_constant_declaration} ";"
	| {modifier} "function" ["&"] T_STRING "(" parameter_list ")"
	  method_body ;

method_body = ";"
	| "{" inner_statement_list "}" ;

variable_modifiers = "var" | modifier {modifier} ;

modifier = "public" | "protected" | "private" | "static" | "abstract"
	| "final" ;

class_variable_declaration = ("var" | modifier {modifier}) T_VARIABLE ["=" static_scalar] ;

class_constant_declaration = T_STRING "=" static_scalar ;

echo_expr_list = expr {"," expr} ;

for_expr = [ expr {"," expr} ] ;

expr_without_variable = "list" "(" assignment_list ")" "=" expr
	| variable "=" expr
	| variable "=" "&" variable
	| variable "=" "&" "new" class_name_reference [ctor_arguments]
	| "new" class_name_reference [ctor_arguments]
	| "clone" expr
	| variable ("+=" | "-=" | "*=" | "/=" | ".=" | "%=" | "&=" | "|=" |
	  "^=" | "<<=" | ">>=" ) expr
	| rw_variable "++"
	| "++" rw_variable
	| rw_variable "--"
	| "--" rw_variable
	| expr ("||" | "&&" | "or" | "and" | "xor" | "|" | "&" | "^" | "." |
	  "+" | "-" | "*" | "/" | "%" | "<<" | ">>" | "===" | "!==" |
	  "<" | "<=" | ">" | ">=" ) expr
	| ("+" | "-" | "!" | "~") expr
	| expr "instanceof" class_name_reference
	| "(" expr ")"
	| expr "?" expr ":" expr
	| internal_functions
	| "(int)" expr
	| "(double)" expr
	| "(float)" expr
	| "(real)" expr
	| "(string)" expr
	| "(array)" expr
	| "(object)" expr
	| "(bool)" expr
	| "(boolean)" expr
	| "(unset)" expr # FIXME: not implemented
	| "exit" [exit_expr]
	| "die" [exit_expr]
	| "@" expr
	| scalar
	| "array" "(" [array_pair_list] ")"
	| "`" encaps_list "`"
	| "print" expr ;

function_call = T_STRING "(" function_call_parameter_list ")"
	| fully_qualified_class_name "::" T_STRING
	  "(" function_call_parameter_list ")"
	| fully_qualified_class_name "::" variable_without_objects
	  "(" function_call_parameter_list ")"
	| variable_without_objects "(" function_call_parameter_list ")" ;

fully_qualified_class_name = T_STRING ;

class_name_reference = T_STRING
	| dynamic_class_name_reference ;

dynamic_class_name_reference = base_variable "->" object_property
	  { "->" object_property }
    | base_variable ;

exit_expr = "(" [expr] ")" ;

ctor_arguments = "(" function_call_parameter_list ")" ;

common_scalar = T_LNUMBER | T_DNUMBER | T_CONSTANT_ENCAPSED_STRING
	| "__LINE__" | "__FILE__" | "__CLASS__" | "__METHOD__" | "__FUNCTION__" ;

# FIXME: very bad syntax, $x = + + + 4; is valid!
static_scalar = common_scalar
	| T_STRING
	| "+" static_scalar
	| "-" static_scalar
	| "array" "(" [static_array_pair_list] ")"
	| static_class_constant ;

static_class_constant = T_STRING "::" T_STRING ;

scalar = T_STRING
	| T_STRING_VARNAME
	| class_constant
	| common_scalar
	| "\"" encaps_list "\""
	| "'" encaps_list "'"
	| T_START_HEREDOC encaps_list T_END_HEREDOC ;

static_array_pair_list = static_array_pair { "," static_array_pair } [","] ;

static_array_pair = static_scalar ["=>" static_scalar] ;

expr = r_variable | expr_without_variable ;

r_variable = variable ;

w_variable = variable ;

rw_variable = variable ;

variable = base_variable_with_function_calls [ "->" object_property
	  method_parameters { "->" object_property method_parameters } ] ;

method_parameters = "(" function_call_parameter_list ")" ;

variable_without_objects = reference_variable
	| simple_indirect_reference reference_variable ;

static_member = fully_qualified_class_name "::" variable_without_objects ;

base_variable_with_function_calls = base_variable | function_call ;

base_variable = reference_variable
	| simple_indirect_reference reference_variable
	| static_member ;

reference_variable = compound_variable { selector } ;

compound_variable = T_VARIABLE | "$" "{" expr "}" ;

selector = "[" [expr] "]" | "{" expr "}" ;

object_property = variable_name { selector }
	| variable_without_objects ;

variable_name = T_STRING | "{" expr "}" ;

simple_indirect_reference = "$" {"$"} ;

assignment_list = [assignment_list_element] {"," [assignment_list_element]} ;

assignment_list_element = variable
	| "list" "(" assignment_list ")" ;

array_pair_list = array_pair {"," array_pair} [","] ;

array_pair = "&" w_variable
	| expr "=>" "&" w_variable
	| expr "=>" expr ;

encaps_list =
	{
		encaps_var
		| T_STRING
		| T_NUM_STRING
		| T_ENCAPSED_AND_WHITESPACE
		| T_CHARACTER
		| T_BAD_CHARACTER
		| "["
		| "]"
		| "{"
		| "}"
		| "->"
	} ;

encaps_var = T_VARIABLE [ "[" encaps_var_offset "]" ]
	| T_VARIABLE "->" T_STRING
	| "${" expr "}"
	| "${" T_STRING_VARNAME "[" expr "]" "}"
	| T_CURLY_OPEN variable "}" ;

encaps_var_offset = T_STRING | T_NUM_STRING | T_VARIABLE ;

internal_functions = "isset" "(" variable {"," variable} ")"
	| "empty" "(" variable ")"
	| "include" expr
	| "include_once" expr
	| "eval" "(" expr ")"
	| "require" expr
	| "require_once" expr ;

class_constant = fully_qualified_class_name "::" T_STRING ;


#
# Some tokens from the scanner (see file Zend/zend_language_scanner.l):
#

LABEL = (letter | "_") {letter | digit | "_"} ;
T_STRING = LABEL;
T_BAD_CHARACTER = "\x00".."\x08" | "\x0b" | "\x0c" | "\x0e".."\x1f" ;
T_VARIABLE = "$" T_STRING ;
T_LNUMBER = octal | decimal | hexadecinal ;
octal = "0" {"0".."7"} ;
decimal = "1".."9" {digit} ;
hexadecinal = "0x" hexdigit {hexdigit} ;
digit = "0".."9" ;
hexdigit = digit | "a".."f" | "A".."F" ;
letter = "a".."z" | "A".."Z" | "\x7f".."\xff" ;

T_DNUMBER = DNUM | EXPONENT_DNUM;
DNUM = digit ["."] digit {digit} | digit {digit} ["."] {digit};
EXPONENT_DNUM = (LNUM | DNUM) ("e"|"E") ["+"|"-"] LNUM;
LNUM = digit {digit};

T_CURLY_OPEN = "${";

T_CONSTANT_ENCAPSED_STRING = single_quoted_constant_string | double_quoted_constant_string;
# FIXME
single_quoted_constant_string =
	"'" { "any char except ' and \\" | "\\" "any char" } "'";
# FIXME
double_quoted_constant_string =
	"\"" { "any char except $ \" and \\" | "\\" "any char" } "\"";

T_STRING_VARNAME = LABEL;
T_NUM_STRING = LNUM | hexadecinal;
T_START_HEREDOC = "<<<" {" "|"\t"} LABEL NEWLINE;
NEWLINE = "\r"|"\n"|"\r\n";

T_END_HEREDOC = "FIXME: here at the beginning of the line"
	LABEL [";"] NEWLINE;
```
##-------------------------------------------------------------------------------------##





*Quicksort*: Ordena el arreglo dividiéndolo en sublistas con respecto a un pivote y luego ordena recursivamente las sublistas.
```
######################  Quicksort ########################################

<?php
function quickSort(array &$array): void {
    $length = count($array);
    if ($length < 2) {
        return; // Base case: a list of 0 or 1 elements is already sorted
    }

    $pivot = $array[intval($length / 2)];
    $left = $right = [];

    foreach ($array as $item) {
        if ($item < $pivot) {
            $left[] = $item;
        } elseif ($item > $pivot) {
            $right[] = $item;
        }
    }

    quickSort($left); // Recursively apply quicksort to the left part
    quickSort($right); // Recursively apply quicksort to the right part

    $array = array_merge($left, [$pivot], $right); // Merge the results
}

$arraySize = 100000;
$array = range(1, $arraySize);
shuffle($array);
$startTime = microtime(true);
quickSort($array);
$endTime = microtime(true);

$durationMilliseconds = ($endTime - $startTime) * 1000;
echo "Duración del QuickSort: " . number_format($durationMilliseconds, 4) . " milisegundos con un arreglo de " . $arraySize . " elementos\n";
?>
#####################  Quicksort ########################################
```
**Output: Duración del QuickSort: 69.5720 milisegundos con un arreglo de 100000 elementos**


*MergeSort*:  Divide el arreglo en mitades hasta llegar a listas simples y luego fusiona estas listas ordenadamente.
```
#####################  MergeSort ########################################

<?php
function mergeSort(array &$array): void {
    $length = count($array);
    if ($length < 2) {
        return; // Base case: a list of 0 or 1 elements is already sorted
    }

    $middle = intval($length / 2);
    $left = array_slice($array, 0, $middle);
    $right = array_slice($array, $middle);

    mergeSort($left); // Recursively apply mergesort to the left part
    mergeSort($right); // Recursively apply mergesort to the right part

    $array = merge($left, $right); // Merge the results
}

function merge(array $left, array $right): array {
    $result = [];
    while (count($left) > 0 && count($right) > 0) {
        if ($left[0] <= $right[0]) {
            $result[] = array_shift($left);
        } else {
            $result[] = array_shift($right);
        }
    }
    return array_merge($result, $left, $right);
}

$arraySize = 100000;
$array = range(1, $arraySize);
shuffle($array);
$startTime = microtime(true);
mergeSort($array);
$endTime = microtime(true);

$durationMilliseconds = ($endTime - $startTime) * 1000;
echo "Duración del MergeSort: " . number_format($durationMilliseconds, 4) . " milisegundos con un arreglo de " . $arraySize . " elementos\n";
?> 
#####################  MergeSort ########################################
```
**Output: Duración del MergeSort: 158.8380 milisegundos con un arreglo de 100000 elementos**


*Burbuja*: Compara e intercambia elementos adyacentes repetidamente hasta que el arreglo esté ordenado
```
#####################  Burbuja ########################################
 
<?php
function bubbleSort(array &$array): void {
    $n = count($array);
    
    for ($i = 0; $i < $n - 1; $i++) {
        for ($j = 0; $j < $n - $i - 1; $j++) {
            if ($array[$j] > $array[$j + 1]) {
                $temp = $array[$j];
                $array[$j] = $array[$j + 1];
                $array[$j + 1] = $temp;
            }
        }
    }
}

$arraySize = 100;
$array = range(1, $arraySize);
shuffle($array);
$startTime = microtime(true);
bubbleSort($array);
$endTime = microtime(true);

$durationMilliseconds = ($endTime - $startTime) * 1000;
echo "Duración del Bubble Sort: " . number_format($durationMilliseconds, 4) . " milisegundos con un arreglo de " . $arraySize . " elementos\n";
?> 

#####################  Burbuja ########################################
```
**Output: Duración del Bubble Sort: 0.2952 milisegundos con un arreglo de 100 elementos**
