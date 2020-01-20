_This is my very first blog post from April 2002. I published it blogspot.com and ported here as an historical reference._

# Cómo pensar como un pythonista

Esta página es una traducción del artículo *How to think like a Pythonista* de Michael Hudson.
Las referencias a primera persona en los comentarios se referieren, lógicamente, a él.

En <a href="http://www.aleax.it/">www.aleax.it</a> encontrarán documentación de Python en italiano,
escrita por Alex Martelli autor original de parte del artículo que aqui reproduzco.
Puede serles útil si dicho idioma les es mas sencillo de entender que el inglés.

--N.de T.


Cómo pensar como un Pythonista.

* La Pregunta
* Mi Respuesta
* La Respuesta de Alex
* Un Cliente Satisfecho

Hace poco (abril del 2002), un no-iniciado en Python buscaba respuestas en
comp.lang.python por ciertos extraños comportamientos del lenguaje.
Dada la recurrencia del error entre los recién llegados a Python, reproduzco aquí la duda y las respuestas.

Hola,

Algo que me gusta mucho de Python es que las sentencias
funcionan tal y como uno espera que lo hagan. Tomemos por
ejemplo el uso de dict.values() para los diccionarios:
Si conservamos el resultado de dict.values() y
posteriormente modificamos el diccionario, el resultado
conservado permanece inalterado.

    >>> dict = {'a':1,'b':2}
    >>> list = dict.values()
    >>> list
    [1, 2]
    >>> dict['a']=3
    >>> list
    [1, 2]
    >>> dict
    {'a': 3, 'b': 2}

Sin embargo, si el diccionario tiene listas como entradas
en el diccionario, obtengo un comportamiento contraintuitivo
(que, hace poco, hizo que mi codigo dejara de funcionar):
Si modifico el dict, la lista que previamente habia sido
creada mediante dict.values() aparece actualizada
automágicamente. Un bonito rasgo, ¡pero nada que hubiera
esperado!

    >>> dict = {'a':[1],'b':[2]}
    >>> list = dict.values()
    >>> list
    [[1], [2]]
    >>> dict['a'].append(3)
    >>> dict
    {'a': [1, 3], 'b': [2]}
    >>> list
    [[1, 3], [2]]

Parece como si en el primer caso una copia es devuelta
mientras en el segundo caso una referencia a la lista es
que se devuelve. Muy bien, pero de acuerdo a la filosofía
de Python no debería preocuparme si trabajara con listas
en un diccionario o cualquier otra cosa. No lo encuentro
intuitivo si el comportamiento depende del tipo de
valores que pongo en un diccionario.

¿Quién esta equivocado aquí: mi intuición o Python? Si
fuera mi intuición ¿Cómo podría entrenar mi pensamiento
sobre el modelo de ejecucion de Python para que mi
intuicion mejore? ;-)


Demás está decir que es la intuicion del interesado la que estuvo en falta, pero el autor lejos está (estuvo) de ser el unico en tener este malentendido.

Suerte para él, otros dos programadores con mucha experiencia en Python, Alex Martelli y yo, nos encontrábamos en un día con un particular humor pedagógico (1), y escribimos algunos largos articulos explicando en forma algo diferente donde estaba equivocado.

Estuve despotricando sobre pensar en terminos de "nombres, objetos y enlaces" (a), algo que no hago lo suficiente, y dibuje algunos diagramas ASCII explicando que es lo hay bajo la superficie de los distintos ejemplos que tenian al autor original confundido.


>> Algo que me gusta mucho de Python es que las
>> sentencias funcionan tal y como uno espera que lo
>> hagan.

Bueno. Python funciona mucho como yo espero que lo haga,
pero no esta claro si eso dice mas sobre mi que sobre Python.

Al final del mensaje, dices:

> ¿Quien esta equivocado aqui: mi intuición o Python? Si
> fuera mi intuición ¿Cómo podría entrenar mi pensamiento
> sobre el modelo de ejecucion de Python para que mi
> intuicion mejore? ;-)

Eres tú :) Como no puedo leer mi email por el momento[1],
no tengo otra forma mejor que perder el tiempo que
dibujarle algunos gráficos.

Primero, alguna terminologia. En realidad, lo primero de
todo es alguna anti-terminologia; encuentro la palabra
"variable" particularmente inútil dentro del contexto de
Python. Prefiero "nombres", "ligaduras" y "objetos".

Los nombres tienen esta forma:

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp0.blogger.com/_z25S_uWyezg/RiTPGOtmWxI/AAAAAAAAARU/4KbYrovBElw/s1600-h/g1.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054392387752057618" src="http://bp0.blogger.com/_z25S_uWyezg/RiTPGOtmWxI/AAAAAAAAARU/4KbYrovBElw/s200/g1.png" style="display:block; margin:0 10px 10px 0; text-align:center;cursor:pointer; cursor:hand;" /></a>

Los nombres viven en los espacios de nombres (namespaces),
pero no es de importancia para el tema que nos ocupa dado
que el unico un espacio de nombre en juego es el asociado
con el ciclo leer-evaluar-mostrar del interprete. De hecho
los nombres son solo jugadores menores en este drama; las
ligaduras y los objetos son las estrellas.

Las ligaduras tienen esta forma:

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp3.blogger.com/_z25S_uWyezg/RiTPV-tmWyI/AAAAAAAAARc/BV7YH0ZGAQ4/s1600-h/g2.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054392658334997282" src="http://bp3.blogger.com/_z25S_uWyezg/RiTPV-tmWyI/AAAAAAAAARc/BV7YH0ZGAQ4/s200/g2.png" style="display:block; margin:0 10px 10px 0; text-align:center;cursor:pointer; cursor:hand;" /></a>

La punta izquierda de las ligaduras pueden ser sujetadas
a los nombres u otros "lugares" como los atributos de
objetos y entradas en listas o en diccionarios. La punta
derecha está siempre sujetada a objetos[2]

Los objetos tienen esta forma:

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp0.blogger.com/_z25S_uWyezg/RiTPhOtmWzI/AAAAAAAAARk/6vjaOEmrVt4/s1600-h/g3.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054392851608525618" src="http://bp0.blogger.com/_z25S_uWyezg/RiTPhOtmWzI/AAAAAAAAARk/6vjaOEmrVt4/s200/g3.png" style="display:block; margin:0 10px 10px 0;text-align:center;cursor:pointer; cursor:hand;" /></a>

Esto pretende ser la cadena de caracteres "bar". Otros
tipos de objetos serán dibujados en forma diferente, pero
espero que entenderá lo que busco.

> Tomemos por ejemplo el uso de dict.values() para los
> diccionarios: Si conservamos el resultado de dict.values()
> y posteriormente modificamos el diccionario, el resultado
> conservado permanece inalterado.
>
>     >>> dict = {'a':1,'b':2}

Luego de esta sentencia, parecería apropiado dibujar este
diagrama:

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp3.blogger.com/_z25S_uWyezg/RiTP_-tmW0I/AAAAAAAAARs/1R4dRT8tlJ0/s1600-h/g4.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054393379889503042" src="http://bp3.blogger.com/_z25S_uWyezg/RiTP_-tmW0I/AAAAAAAAARs/1R4dRT8tlJ0/s200/g4.png" style="display:block; margin:0 10px 10px 0; text-align:center;cursor:pointer; cursor:hand;" /></a>


>     >>> list = dict.values()

Ahora este:

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp2.blogger.com/_z25S_uWyezg/RiTQNutmW1I/AAAAAAAAAR0/iWdpm-jtsnE/s1600-h/g5.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054393616112704338" src="http://bp2.blogger.com/_z25S_uWyezg/RiTQNutmW1I/AAAAAAAAAR0/iWdpm-jtsnE/s200/g5.png" style="display:block; margin:0 10px 10px 0; text-align:center;cursor:pointer; cursor:hand;" /></a>


>     >>> list
>     [1, 2]

Lo que por supuesto, no es sorpresa.

>     >>> dict['a']=3

Ahora esto:

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp2.blogger.com/_z25S_uWyezg/RiTQXutmW2I/AAAAAAAAAR8/KdVk_2AEroM/s1600-h/g6.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054393787911396194" src="http://bp2.blogger.com/_z25S_uWyezg/RiTQXutmW2I/AAAAAAAAAR8/KdVk_2AEroM/s200/g6.png" style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;" /></a>

>     >>> list
>     [1, 2]
>     >>> dict
>     {'a': 3, 'b': 2}

Esto último tampoco deberia sorprender; solamente hay que
seguir las flechas (ligaduras) del gráfico superior.

> Sin embargo, si el diccionario tiene listas como entradas
> en el diccionario, obtengo un comportamiento
> contraintuitivo (que, hace poco, hizo que mi codigo dejara
> de funcionar): Si modifico el dict, la lista que previamente
> habia sido creada mediante dict.values() aparece actualizada
> automágicamente. Un bonito rasgo, &#161;pero nada que hubiera
> esperado!

Esto sucede porque no esta pensando en terminos de Nombres,
Objetos y Ligaduras.

>     >>> dict = {'a':[1],'b':[2]}

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp0.blogger.com/_z25S_uWyezg/RiTQhOtmW3I/AAAAAAAAASE/J6HAvppza4c/s1600-h/g7.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054393951120153458" src="http://bp0.blogger.com/_z25S_uWyezg/RiTQhOtmW3I/AAAAAAAAASE/J6HAvppza4c/s200/g7.png" style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;" /></a>

>     >>> list = dict.values()

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp1.blogger.com/_z25S_uWyezg/RiTQyetmW4I/AAAAAAAAASM/CEudqUzAmwo/s1600-h/g8.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054394247472896898" src="http://bp1.blogger.com/_z25S_uWyezg/RiTQyetmW4I/AAAAAAAAASM/CEudqUzAmwo/s320/g8.png" style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;" /></a>

>     >>> list
>     [[1], [2]]

De nuevo no hay sorpresas aqui.

>     >>> dict['a'].append(3)

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://bp0.blogger.com/_z25S_uWyezg/RiTQ8OtmW5I/AAAAAAAAASU/_Yo_P4jCrzc/s1600-h/g9.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5054394414976621458" src="http://bp0.blogger.com/_z25S_uWyezg/RiTQ8OtmW5I/AAAAAAAAASU/_Yo_P4jCrzc/s320/g9.png" style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;" /></a>


>     >>> dict
>     {'a': [1, 3], 'b': [2]}
>     >>> list
>     [[1, 3], [2]]

Y ahora esto tampoco deberia sorprender.

> Parece como si en el primer caso una copia es devuelta
> mientras en el segundo caso una referencia a la lista es
> que se devuelve. Muy bien, pero de acuerdo a la filosofia
> de Python no deberia preocuparme si trabajara con listas
> en un diccionario o cualquier otra cosa. No lo encuentro
> intuitivo si el comportamiento depende del tipo de
> valores que pongo en un diccionario.

Si luego de haber visto los graficos presentados no se ha dado
cuenta de donde vienen sus malosentendidos, no estoy seguro si
mucha mas prosa podría ayudar.

Salud,
Michael Hudson.

[1] Alguien sabe donde fue el starship?
[2] Dispararé a cualquiera que mencione UnboundLocalError en
este momento.

Alex tomo una estrategia diferente, verbosa, para explicar que Python no copia cuando no tiene que hacerlo,
relatando una anecdota muy bonita sobre una estatua en Boloña y sugieriendo al interesado lecturas de Borges,
Calvino, Wittgenstein o Korzibsky.


> Hola,
>
> Algo que me gusta mucho de Python es que las
> sentencias funcionan tal y como uno espera que
> lo hagan. Tomemos por ejemplo el uso de dict.values()
> para los diccionarios: Si conservamos el resultado de
> dict.values() y posteriormente modificamos el diccionario,
> el resultado conservado permanece inalterado.

El método .values() de un diccionario retorna una nueva
lista de los valores. Esto es de alguna manera inevitable,
desde que los diccionarios normalmente no _tienen_ la lista
de sus valores, y la tienen que construir al vuelo cuando
usted la solicita. No es una copia -- es un nuevo objeto
lista.

Sin embargo, Python no copia excepto en situaciones donde
una copia es específicamente definida para que se haga.
El metodo .values() se encuentra en esa situación en una
forma vaga, como fue mencionado... un nuevo objeto, en vez de
una copia de cualquier otro existente.

En general, siempre que sea posible, Python devuelve
referencias a los mismos objetos que ya tiene alrededor, en
vez de copialos; si AÚN quiere una copia solicitela -- vea
el módulo copy si quiere hacerlo en una forma general.
Por supuesto, construir nuevos objetos es un caso diferente.

Si esto es contraintuitivo, que así sea -- realmente no hay
alternativas para el caso general sin imponer un gran coste,
haciendo copias de todo solo "por las dudas". MUCHO mejor es
hacer copias solo ante requerimientos explícitos (y objetos
nuevos, cuando no existen objetos que podrían ser copiados o
referenciados)

Por supuesto que hay casos que estan en el medio -- como las
rebanadas (slices)

Las secuencias estándar le dan un nuevo objeto cuando solicita
una rebanada; esto solo importa para listas (para los objetos
inmutables a Ud. no le deberia interesar si obtiene copias o
no) Una lista no tiene la capacidad de "compartir una parte
de si misma", y cuando se le solicita una rebanada le devuelve
una copia, una nueva lista (en general, por supuesto, también
lo hace cuando se le solicita una rebanada-de-todo, lalista[:]
-- caso limite donde un nuevo objeto pueder ser visto como
una copia de uno existente)

El muy justo popular paquete Numeric, por otro lado, define el
tipo array que SI es capaz de compartir alguno o todos sus
datos entre diferentes objetos array -- una rebanada de un
array en Numeric comparte sus datos con el array desde se
obtuvo la rebanada. Es un nuevo objeto, fíjese:

    >>> import Numeric
    >>> a=Numeric.array(range(6))
    >>> b=a[:]
    >>> id(a)
    136052568
    >>> id(b)
    136052728
    >>>

pero esos dos objetos distintos a y b sí comparten datos:

    >>> a
    array([0, 1, 2, 3, 4, 5])
    >>> b
    array([0, 1, 2, 3, 4, 5])
    >>> a[3]=23
    >>> b
    array([ 0, 1, 2, 23, 4, 5])
    >>>

Cada comportamiento tiene detrás un excelente pragmatismo
--las listas son _mucho_ mas simples al no tener que
preocuparse de compartir datos, los array tienen casos de
uso muy diferentes -- pero es difícil no sorprenderse
cuando dos objetos tan similares difieren en esos detalles.

Pero todas las copias que sí "suceden", ej. el caso limite
de las rebanadas o cualquier otro caso (con UNA excepción
que mencionaré mas adelante) son siempre copias playas
(shallow).

Python NUNCA se embarca en la ENORME tarea de la copia
_profunda_ a menos que muy específicamente se solicite
--específicamente con la función deepcopy del modulo copy.
La copia PROFUNDA es cosa seria --la función deepcopy
tiene que estar alerta de los ciclos, reproducir
cualquier identidad de referencias, potencialmente seguir
las referencias a cualquier profundidad, recursivamente
--tiene que reproducir fielmente el grafo de los objetos
referenciando uno a otro con ilimitada complejidad.
Funciona, pero por supuesto nunca será tan rápida como la
simple tarea de la copia playa (que a su vez nunca será
tan rápida como manejar directamente una referencia mas a
un objeto existente, siempre que este último curso de
acción sea posible)

Aparentemente esto es lo que lo ha atrapado a ud aquí:

> Sin embargo, si el diccionario tiene listas como entradas
> en el diccionario, obtengo un comportamiento contraintuitivo
> (que, hace poco, hizo que mi codigo dejara de funcionar):
> Si modifico el dict, la lista que previamente habia sido
> creada mediante dict.values() aparece actualizada
> automágicamente. Un bonito rasgo, &#161;pero nada que hubiera
> esperado!

Realmente no -- si ud. cambia _objetos a los cuales dict
refiere_ (en vez de cambiar el dict en si), luego las otras
referencias a los-mismos-objetos permanecen como referencias
a esos mismos objetos -- si los objetos mutan, ud. ve a los
objetos mutados desde cualesquier referencia a ellos que
pueda estar usando.

>     >>> dict = {'a':[1],'b':[2]}
>     >>> list = dict.values()
>     >>> list
>     [[1], [2]]

No use nombres de los tipos built-in como variables: ud. SE
quemará algún día si lo hace. dict, list, str, tuple, file,
int, long, float, unicode... no use esos identificadores para
sus propios propósitos, por mas tentador que pueda ser (una
"molestia atractiva", seguro.) Si no toma el hábito de evitar
eso, un día estara intentando (por ejemplo) construir una
lista con x=list('chau') y obtendra errores intrigantes...
porque habrá reenlazado el idenficador 'list' para referir a
cierto objeto list en vez de al tipo list en si.

Use alist, somedict, myfile, lo que sea... nada que ver con
su problema, solo otro simple consejo !-)

>     >>> dict['a'].append(3)

Esto no "modifica el diccionario" -- el objeto diccionario
aún contiene exactamente las mismas referencias, a los
objetos con los mismos id (dos objetos cadena, las claves
y dos objetos listas, los valores). Está cambiando (mutando)
uno de esos objetos, pero ese es otro tema. Podría
modificar dicho objeto lista a traves de cualquier
referencia a él, ejemplo:

    >>> alist=list('ciao')
    >>> adict={'a':alist}
    >>> adict
    {'a': ['c', 'i', 'a', 'o']}
    >>> alist.pop()
    'o'
    >>> adict
    {'a': ['c', 'i', 'a']}
    >>>

Si ud. quería que el diccionario adict refiriera a una
COPIA (una "toma", si prefiere) de los contenidos de alist,
tendría que haberlo dicho:

    >>> import copy
    >>> alist=list('ciao')
    >>> adict={'a':copy.copy(alist)}
    >>> adict
    {'a': ['c', 'i', 'a', 'o']}
    >>> alist.pop()
    'o'
    >>> adict
    {'a': ['c', 'i', 'a', 'o']}
    >>>

y luego la cadena-reprensentación del objeto diccionario podría
ser aislada de cualquier cambio a la lista que referencia el
nombre alist. La cadena que la representa delega parte de su
tarea a los objetos a los cuales el objeto diccionario refiere,
entonces, si quiere aislarlo, ud. necesita copias --de hecho
tal vez profundas (... bueno no, realmente no, pero... :-)

>     >>> dict
>     {'a': [1, 3], 'b': [2]}
>     >>> list
>     [[1, 3], [2]]
>
> Parece como si en el primer caso una copia es devuelta
> mientras en el segundo caso una referencia a la lista es
> que se devuelve.

Nop. SIEMPRE Referencias. .values() no devuelve una
referencia a un objeto existente NI una copia a un objeto
existente, porque en este caso hay "objeto existente"
--por lo que siempre devuelve un NUEVO objeto, construido
adrede de acuerdo a sus especificaciones.

> ... de acuerdo a la filosofia de Python no deberia
> preocuparme si trabajara con listas en un diccionario o
> cualquier otra cosa. No lo encuentro intuitivo si el
> comportamiento depende del tipo de valores que pongo
> en un diccionario.

No hay tales dependencias. Solo una enorme diferencia
entre cambiar un objeto y cambiar (mutar) algún OTRO
objeto al que el primero refiere.

En Boloña hace mas de 100 añas teniamos una estatua de un
héroe local representado apuntando con su dedo hacia
adelante --presumiblemente hacia el futuro, pero dado el
lugar donde fue emplazado, los vecinos rápidamente la
identificaron como "la estatua que apunta al Hotel
Belfiore". Un día un empresario compró el edificio del
hotel y lo reformó -- en particular, donde solia estar el
hotel ahora hay un restaurante, el Da Carlo.

De pronto, "la estatua que apunta al Hotel Belfiore" devino
en "la estatua que apunta al Da Carlo"...! Asombroso, no?
Considerando que el mármol no es muy fluido y que el
monumento no ha sido movido ni molestado de ninguna forma...?

A propósito, esta es una anécdota real (excepto que no
estoy seguro sobre los nombres del hotel y del restaurante
--podría equivocarme en ellos), pero aún pienso que puede
ayudar aquí. El diccionario, o la estatua, no ha sido
modificada de ninguna forma, aun cuando los objetos a los
que refiere/apunta pueden haber mutado hasta quedar
irreconocibles, y que el nombre con el que la gente lo
conoce (la cadena-representación del diccionario) pueda asi
cambiar. Aquel nombre o representación estaba y esta
refiriendo a un no-intrínseco, no-persistente,
característica "casual" de la estatua, o diccionario...

> ¿Quien esta equivocado aqui: mi intuicion o Python? Si
> fuera mi intuicion ¿Cómo podría entrenar mi pensamiento
> sobre el modelo de ejecucion de Python para que mi
> intuicion mejore? ;-)

Su intuición, que le llevó por el mal camino (Python hace
sólo lo que debería hacer), puede ser entrenada de diversas
maneras. Las obras de J. L. Borges e I. Calvino, si le
gusta la ficción que sea razonablemente sofisticada y hasta
muy placentera, son buenas apuestas. Si le gusta la
no-ficción escrita por ingenieros luchando duro para disipar
algunos de los errores de los filósofos, Wittgenstein y
Korzibsky son excelentes.

No estoy bromeando. Me doy cuenta que muchos Pythonistas no
se interesan por ninguno de los dos generos. En ese caso,
este grupo (comp.lang.python) y sus archivos, los ensayos de
GvR y /F, y los fuentes de Python, también pueden ser
lecturas interesantes.

Alex

El ensayo de /F al que se refería Alex es probablemente éste (e incluso si no lo es, debería usted leerlo.)
Habla sobre algunos de estos temas en un forma mas tersa.

GvR es Guido van Rossum y /F es Fredrik Lundh --N.de T.

Y solo para probar que valió la pena hacer todo esto, nuestro no-iniciado se transformó en un cliente satisfecho:


Estiamdo Michael, estimado Alex,

¡¡¡Son excelentes profesores!!!

Michael, realmente me has ayudado en entender el punto con
tus gráficos. &#161;Un montón de gracias por tu trabajo artístico!

Alex, la anécdota sobre la estatua apuntando al Hotel Belfiore
hizo obvio el error de mi intuición! Me gustó y nunca mas
volveré a olvidarlo! Gracias por tu respuesta!

Pienso que hoy aprendí muchisísmo en mi camino para
convertirme en buen Pythónico!

Espero que ustedes también encuentren útiles estas respuestas.

