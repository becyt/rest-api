# rest-api

Documentación para el uso de la API de los catálogos de la Biblioteca Electrónica de Ciencia y Tecnología

## Versiones

- v1-beta: 2016-dic. Inicio de la API. Creación y testeo

## Reportar problemas, sugerencias

Contactar a: aapollaro@mincyt.gob.ar

## Licencia

El uso de esta API está bajo una Licencia MIT.

## Consideraciones generales

Esta API fue diseñada para que las bibliotecas de las instituciones habilitadas puedan consumir los datos de los catálogos de la Biblioteca Electrónica de Ciencia y Tecnología. Los datos fueron relevados, curados y cargados por el equipo de la Biblioteca y se refieren a datos bibliográficos de los títulos (título, ISSN, ISBN, edición, etc.) con sus correspondientes URLs a las plataformas editoriales de acuerdo a la contratación que establece el Ministerio con las editoriales.

La API devuelve resultados en formato JSON. La documentación sobre el esquema de metadatos que devuelve el objeto JSON se encuentra en desarrollo.

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
| `/resources` | devuelve una lista de todos los recursos de información suscriptos y de acceso abierto de la Biblioteca Electrónica. |

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
| `/standarads/{number}` | devuelve información acerca de un determinado título de estándar que coincida con el número de identificación. |
| `/resources?id={resource_id}` | devuelve información acerca de un recurso de información. |

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

<small>Encontrar los títulos que contienen los términos _argentin_ (_argentina_, _argentino_, _argentinos_, _argentinas_) y _acta_ (_acta_, _actas_, _abstracta_, etc) en el título:</small>

```
http://biblioteca.mincyt.gob.ar/api/items?query.title=argentin+acta
```


#### Filtros

Los filtros permiten acotar los resultados, comienzan con el prefijo `{filter.}`. Los siguientes filtros están disponibles:


| parámetro | colección | opciones | descripción |
|:----------|:---------:|:--------:|:------------|
| `filter.type` | _items_ | `{j, b, s}` | identifica el tipo de colección: revistas `{j}`; libros `{b}`; estándares `{s}`. |
|`filter.openAccess`| _journals_ | `{1}` | filtra por aquellos los títulos de revistas que son solo de acceso abierto. |
|`filter.resource` | _journals_, _books_ | `{id_del_recurso}` | filtra títulos de una determinada plataforma. |
|`filter.discipline` | _journals_, _books_ | `{id_de_la_disciplina}` | filtra títulos de una determinada disciplina de acuerdo a la indización de la Biblioteca Electrónica. |

#####Múltiple filtros

Pueden combinarse en una única búsqueda y pueden combinarse con los parámetros de búsqueda `{query.}`.
