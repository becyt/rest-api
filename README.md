# rest-api

Documentación para el uso de la API de los catálogos de la Biblioteca Electrónica de Ciencia y Tecnología

## Versiones

- v1: 2016-dic. Inicio de la API.

## Reportar problemas, sugerencias

Contactar a: aapollaro@mincyt.gob.ar

## Licencia

El uso de esta API está bajo una Licencia MIT.

## Condideraciones generales

Esta API fue diseñada para que las bibliotecas de las instituciones habilitadas puedan consumir los datos de los catálogos de publicaciones peridicas, libros, estándares y conferencias de la Biblioteca Electrónica de Ciencia y Tecnología. Los datos fueron relevados, curados y cargados por el equipo de la Biblioteca y se refieren a datos bibliográficos de los títulos (título, ISSN, ISBN, edición, etc.) con sus correspondientes URLs a las plataformas editoriales de acuerdo a la contratación que establece el Ministerio con las editoriales.

La API devuelve resultados en formato JSON. La documentación sobre el esquema de metadatos que devuelve el objeto JSON se encuentra [aquí](https://github.com/teleuko/becyt-rest-api/blob/master/api_format.md).

## Requerimientos para su uso

Esta API no es de uso público ya que está dirigida a desarrolladores de las [instituciones habilitadas](http://biblioteca.mincyt.gob.ar/instituciones/index) de la Biblioteca Electrónica. Es necesario

- ser un usuario registrado en el sitio web de la Biblioteca Electronica.
- acceder desde una IP habilitada.
Para solicitar autorización, contactar al equipo de la Biblioteca a: biblioteca@mincyt.gob.ar.

La _clave de autorización_ debe agregarse como otro parámetro dentro de la URL en todas las búsquedas de modo que pueda traer resultados válidos, de otro modo la API devuelve _error_.

```
authKey={clave_de_autorización}
```

## Punto de partida
La URL de inicio es:
```
http://biblioteca.mincy.gob.ar/api
```

## Colecciones
Las colecciones principales de la API son:
- items
- journals
- books
- standards

Pueden ser usados de la siguiente manera:


```
http://biblioteca.mincy.gob.ar/api/{colleción}?authKey={clave_de_autorización}
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
http://biblioteca.mincy.gob.ar/api/items?authKey={clave_de_autorización}
```

### Identificadores
Las colecciones pueden ser usadas junto a identificadores para obtener los metadatos de un determinado título.
Pueden ser usados de la siguiente manera:


| colección | descripción |
|:--------- |:------------|
| `/ìtems/{identificador}` | devuelve los metadatos de un determinado ítem que coincide con el `{identificador}` que puede ser un `ISSN` (impreso o electrónico) para las revistas, un `ISBN` (impreso o electrónico) para los libros y un `número de identificación` para los estándares. |
| `/journals/{issn}` | devuelve información acerca de un determinado título de publicación periódica que coincida con el ISSN (impreso o electrónico). |
| `/books/{isbn}` | devuelve información acerca de un determinado título de libro que coincida con el ISBN (impreso o electrónico). |
| `/standarads/{number}` | devuelve información acerca de un determinado título de estándar que coincida con el número de identificación. |
| `/resources/{resource_id}` | devuelve información acerca de un recurso de información. |

####Ejemplos

<small>Metadatos de la revista con ISSN 0066-782X (_Arquivos Brasileiros de Cardiologia_):</small>
```
http://biblioteca.mincyt.gob.ar/api/journals/0066-782X
```

<small>El mismo resultado se obtiene también usando el ISSN 1678-4170:</small>
```
http://biblioteca.mincyt.gob.ar/api/journals/1678-4170
```

<small>Metadatos correspondientes a *ScienceDirect*:</small>

```
http://biblioteca.mincyt.gob.ar/api/resources/SCIENCEDIRECT
```


### Parámetros
#### Búsquedas

Por el momento solo está disponible la búsqueda por palabras del título. El parámetro comienza con el párametro _query_ seguido de un punto (.) y _title_.

| parámetro | colección | valores | descripción |
|:----------|:---------:|:-------:|:------------|
| `query.title` | _todas_ | `{string}` | devuelve aquellos títulos cuyas palabras que coincidan en el título. |

##### Ejemplos

<small>Encontrar los títulos que contienen el término _microbiology_ en el título:</small>

```
http://biblioteca.mincyt.gob.ar/api/items?authKey={Clave}&query.title=microbiology
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

Pueden combinarse en una única búsqueda.
