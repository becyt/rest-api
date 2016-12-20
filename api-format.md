#becyt-api-format

Documentación sobre el formato de salida de la API.

##Message

Información del estado de la consulta.

```json
"message": {
	"status": "ok",
	"ip": "200.9.244.16",
	"access-node": {
		"id": "MINCYT",
		"name": "Ministerio de Ciencia, Tecnología e Innovación Productiva"
	},
	"items-count": 22180,
	"page": 1,
	"items-per-page": 20
},
```

| campo | descripción |
|:------|:------------|
| `{status}` | estado de la API. Si la consulta es correcta el `{status}` es "ok". De lo contrario es "error". |
| `{ip}` | dirección IP desde la cual se está accediendo. |
| `{access-node}` | institución habilitada desde la que se accede. Con identificador `{id}` y nombre desarrollado `{name}`.
| `{items-count}` | cantidad de ítems devueltos. |
| `{page}` | número de página en la paginación. |
| `{items-per-page}` | cantidad de ítems devueltos.  |

##Items

Información que describe cada `{item}`.

```json
"items": [
			{
				"title": "#Tear",
				"item-type": "j",
				"identifiers": {
					"type": "ISSN",
					"print": null,
					"online": "2238-8079"
				}
			},
	... ]
```

| campo | descripción |
|:------|:------------|
| `{title}` | título de la publicación periódica, libro, etc. |
| `{item-type}` | tipo de ítem: "j" = revistas; "b" = libros; "s" = estándar. |
| `{identificadores.type}` | tipo de identificador. "ISSN" para las revistas, "ISBN" para los libros, "number" para los estádares. |
| `{identificadores.print}` | para las revistas y libros, el identificador de la versión impresa. |
| `{identificadores.online}` | para las revistas y libros, el identificador de la versión electrónica o en línea. |

##journals

Metadatos para cada título de una publicación periódica.

```json
"journals": [
	{
		"title": {
			"proper": "IEEE Transactions on Acoustics, Speech and Signal Processing",
			"subtitle": null,
			"complete": "IEEE Transactions on Acoustics, Speech and Signal Processing",
			"parallel": null
		},
		"dates": {
			"start": "1974",
			"end": "1990"
		},
		"issn": {
			"print": "0096-3518",
			"online": null
		},
		"publisher": "IEEE",
		"country": {
			"id": "US",
			"name": "Estados Unidos"
		},
		"verified": "y",
		"status": "closed",
		"history": [
			{
				"title": "IEEE Transactions on Audio and Electroacoustics",
				"relationship": "Es continuación de"
			},
			{
				"title": "IEEE Transactions on Signal Processing",
				"relationship": "Continúa como"
			}
		],
		"openAccess": "n",
		"coverSrc": "/images/covers/journals/8757.gif",
		"subscriptions": [subscription1, subscription2, subscription3, ...],
		"disciplines": [discipline1, discipline2, ...]
	}
]
```

| campo | descripción |
|:------|:------------|
| `{title}` | título de la publicación periódica. La opción de título completo `{complete}` es la suma del título propiamente dicho `{proper}` y el subtítulo `{subtitle}`. `{parallel}` se usa para los títulos paralelos (en otro idioma). |
| `{dates}` | años de inicio `{start}` y fin  `{end}` de la publicación. |
| `{issn}` | contiene las versiones impresas `{print}` y electrónica y/o en línea `{online}`. |
| `{publisher}` | casa editorial o publicadora; organismo responsable de la edición de la revista. Puede no coincidir con la editora actual. |
| `{country}` | país de edición. `{id}` es el código ISO 3166-1 alpha-2, `{name}` es el nombre en español del país. |
| `{verified}` | si el número de ISSN ha sido verificado contra una fuente de autoridad. Acepta valores `"y"` o `"n"`. |
| `{history}` | historia de la revista. Enlista cada uno de los títulos que tienen relación histórica. Se indican detalle del título `{title}` de cada una, así como también la relación `{relationship}`. |
| `{openAccess}` | informa si la revista es de Accesso Abierto `"y"` o no `"n"`. |
| `{coverSrc}` | dirección donde se encuentra la imagen de la portada. |
| `{subscriptions}`| lista de las suscripciones. |
| `{disciplines}`| indización de la revista. Lista de las disciplinas temáticas. |
