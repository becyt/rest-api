##becyt-api-format

Documentación sobre el formato de salida de la API.

###Message

Información del estado de la consulta.

```
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
