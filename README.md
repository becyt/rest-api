# rest-api

Documentación para el uso de la API de los catálogos de la Biblioteca Electrónica de Ciencia y Tecnología.

## Versiones

- v1-beta: 2016-dic. Inicio de la API. Creación y testeo.

## Reportar problemas, sugerencias

Contactar a: aapollaro@mincyt.gob.ar.

## Licencia

El uso de esta API está bajo una Licencia MIT.

## Consideraciones generales

Esta API fue diseñada para que las bibliotecas de las instituciones habilitadas puedan consumir los datos de los catálogos de la Biblioteca Electrónica de Ciencia y Tecnología. Los datos fueron relevados, curados y cargados por el equipo de la Biblioteca y se refieren a datos bibliográficos de los títulos (título, ISSN, ISBN, edición, etc.) con sus correspondientes URLs a las plataformas editoriales de acuerdo a la contratación que establece el Ministerio con las editoriales.

La API devuelve resultados en formato JSON. La documentación sobre el esquema de metadatos que devuelve el objeto JSON se encuentra [aquí](https://github.com/becyt/rest-api/master/api-format.md).

## Requerimientos para su uso

Esta API no es de uso público ya que está dirigida a desarrolladores de las [instituciones habilitadas](http://biblioteca.mincyt.gob.ar/instituciones/index) de la Biblioteca Electrónica. Es necesario acceder desde una IP habilitada.
Para solicitar autorización, contactar al equipo de la Biblioteca a: biblioteca@mincyt.gob.ar.

## Punto de partida
La URL de inicio es:
```
http://biblioteca.mincy.gob.ar/api
```

## Colecciones
Las colecciones principales para la primera versión de la API son:
- items
- journals
- books
- standards

Pueden ser accedidas de la siguiente manera:


```
http://biblioteca.mincy.gob.ar/api/{colleción}
```

| colección | descripción |
|:----------|:------------|
| `/ìtems` | devuelve una lista de todos los títulos contenidos en los catálogos (revistas, libros y estándares), 20 por página. |
| `/journals` | devuelve una lista de todos los títulos contenidos en el catálogo de revistas. |
| `/books` | devuelve una lista de todos los títulos contenidos en el catálogo de libros. |
| `/standards` | devuelve una lista de todos los títulos contenidos en el catálogo de estándares. |

#### Ejemplo
<small>Traer todos los títulos de toda la colección. Los resultados son siempre 20 resultados por página.</small>
```
http://biblioteca.mincy.gob.ar/api/items
```

### Identificadores
Las colecciones pueden ser usadas junto a identificadores para obtener los metadatos de un determinado título.
Pueden ser usados de la siguiente manera:


| colección | descripción |
|:--------- |:------------|
| `/ìtems?id={identificador}` | devuelve los metadatos de un determinado ítem que coincide con el `{identificador}` que puede ser un `ISSN` (impreso o electrónico) para las revistas, un `ISBN` (impreso o electrónico) para los libros y un `número de identificación` para los estándares. |
| `/journals?id={issn}` | devuelve información acerca de un determinado título de publicación periódica que coincida con el ISSN (impreso o electrónico). |
| `/books?id={isbn}` | devuelve información acerca de un determinado título de libro que coincida con el ISBN (impreso o electrónico). |
| `/standards/?id={number}` | devuelve información acerca de un determinado título de estándar que coincida con el número de identificación. |

####Ejemplos

<small>Los metadatos de la revista _Arquivos Brasileiros de Cardiologia_, se obtienen tanto con el ISSN impreso como el electrónico:</small>
```
http://biblioteca.mincyt.gob.ar/api/journals?id=0066-782X

http://biblioteca.mincyt.gob.ar/api/journals?id=1678-4170
```

<small>Metadatos correspondientes a *ScienceDirect*:</small>

```
http://biblioteca.mincyt.gob.ar/api/resources?id=SCIENCEDIRECT
```


### Parámetros
#### Búsquedas

Por el momento solo está disponible la búsqueda por palabras del título. El parámetro comienza con el párametro _query_ seguido de un punto (.) y _title_. Los términos se concatenan uniéndolos a través del operador _más_ (+).

| parámetro | colección | valores | descripción |
|:----------|:---------:|:-------:|:------------|
| `query.title` | _todas_ | `{string}` | devuelve aquellos títulos cuyas palabras que coincidan en el título. |

##### Ejemplos

<small>Encontrar los títulos que contienen el término _microbiology_ en el título:</small>

```
http://biblioteca.mincyt.gob.ar/api/items?query.title=microbiology
```

<small>Encontrar los títulos que contienen los términos _argentin_ (_argentina_, _argentino_, _argentinos_, _argentinas_) y _acta_ (_acta_, _pactan_, _abstracta_, _fractals_, _retractais_, etc) en el título:</small>

```
http://biblioteca.mincyt.gob.ar/api/items?query.title=argentin+acta
```


#### Filtros

Los filtros comienzan con el prefijo `{filter.}` y permiten acotar los resultados de búsqueda. Los siguientes filtros están disponibles:


| parámetro | colección | opciones | descripción |
|:----------|:---------:|:--------:|:------------|
| `filter.type` | _items_ | `{j, b, s}` | identifica el tipo de colección: revistas `{j}`; libros `{b}`; estándares `{s}`. |
| `filter.openAccess`| _journals_ | `{1}` | filtra por aquellos los títulos de revistas que son solo de acceso abierto. |
| `filter.resource` | _journals_, _books_ | `{id_del_recurso}` | filtra títulos de una determinada plataforma. |
| `filter.discipline` | _journals_, _books_ | `{id_de_la_disciplina}` | filtra títulos de una determinada disciplina de acuerdo a la indización de la Biblioteca Electrónica. |

#####Múltiple filtros

Pueden combinarse en una única búsqueda y pueden combinarse con los parámetros de búsqueda `{query.}`. Para conocer cuáles son los `{id}` identificadores utilice las [listas de referencias](#listas-de-referencia).

#####Ejemplos

<small>Encontrar títulos de revistas de acceso abierto que contengan el término _physics_ en el título.</small>

```
http://biblioteca.mincyt.gob.ar/api/journals?filter.openAccess=1&query.title=physics
```

<small>Encontrar títulos de revistas de la editorial _Wiley_ que sean de _desarrollo sustentable_.</small>

```
http://biblioteca.mincyt.gob.ar/api/journals?filter.discipline=DES-SUS&filter.resource=WILEY
```

##Listas de referencia

Las listas de referencia sirven para conocer cuáles son los identificadores que la Biblioteca Electrónica utiliza para realizar búsquedas y filtros.

| lista | descripción |
|:------|:------------|
| `/resources` | devuelve una lista de recursos de información suscritos y de acceso abierto de la Biblioteca Electrónica con su correspondiente identificador. |
| `/disciplines` | devuelve la lista FoS de la OCDE con las grandes áreas, áreas y subáreas con sus correspondientes identificadores. |

###Búsquedas

Para encontrar el identificador de un determinado recurso o disciplina está  disponible el parámetro `{query}` que busca contenido en el nombre.

####Ejemplo
<small>Encontrar el `{id}` que corresponda a la disciplina con el término de búsqueda _mamiferos_. La disciplina en este caso es _mastozoología_ con ID `{MASTOZOOL}`.</small>

```
http://biblioteca.mincyt.gob.ar/api/disciplines?query=mamiferos
```
