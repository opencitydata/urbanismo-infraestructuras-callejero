Consultas SPARQL que responden a las "user stories"


Consultas de callejero

USERSTORY1. Me interesa conocer cuáles son las calles que se han creado más recientemente, y en qué barrios están, para poder determinar si es viable abrir una panadería en alguna de ellas.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX esadm: <http://vocab.linkeddata.es/datosabiertos/def/sector-publico/territorio#>
PREFIX dcterms: <http://purl.org/dc/terms/>
SELECT DISTINCT ?via ?barrio ?fechaCreacion WHERE{
?via a escjr:Via .
?via dcterms:date ?fechaCreacion .
?via esadm:barrio ?barrio
} ORDER BY DESC(?fechaCreacion)
```

USERSTORY2: La concejalía de parques y jardines quiere tener siempre información actualizada sobre todas las calles y tramos de calles para poder tener bien posicionados todos los árboles que tienen que podar.
USERSTORY7: Estoy haciendo un estudio de localización de franquicias, y además de las calles estoy interesado en los tramos de calle, porque quiero tener un modelo de afluencia de público que considere tramos de calle.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
SELECT DISTINCT ?viaTramo WHERE{
{?viaTramo a escjr:Via} UNION {?viaTramo a escjr:TramoVia}
}
```

USERSTORY6: Quiero hacer propaganda de un producto (por ejemplo, entregar muestras de un nuevo yogur) y quiero saber cuáles son las plazas, bulevares o paseos marítimos que hay en un municipio. Si además tengo información de afluencia de personas por horas, mucho mejor.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX tipovia: <http://vocab.linkeddata.es/datosabiertos/kos/urbanismo-infraestructuras/tipo-via/>
PREFIX dcterms: <http://purl.org/dc/terms/>
SELECT DISTINCT ?via ?tipoVia WHERE{
?via a escjr:Via .
?via escjr:tipoVia ?tipoVia .
FILTER (?tipoVia IN (tipovia:BULEV, tipovia:PASEO, tipovia:PLAZA))
}
```

USERSTORY9. Una clínica dentista necesita conocer los portales que están asociados a cada calle para realizar un buzoneo publicitario sobre su servicio.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX dcterms: <http://purl.org/dc/terms/>
SELECT DISTINCT ?portal ?via WHERE{
?portal a escjr:Portal .
?portal escjr:via ?via .
?via a escjr:Via .
}
```

USERSTORY10. Soy una asociación y quiero quejarme del mal estado del pavimento de una calle y necesito saber a qué junta o distrito pertenece la calle y portal para poder realizar la reclamación en la junta pertinente.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX esadm: <http://vocab.linkeddata.es/datosabiertos/def/sector-publico/territorio#>
SELECT DISTINCT ?portal ?via ?distrito ?junta WHERE{
?portal a escjr:Portal .
?portal escjr:via ?via .
?via a escjr:Via .
OPTIONAL {?portal esadm:distrito ?distrito}.
OPTIONAL {?portal esadm:juntaAdministrativa ?junta} .
}
```

USERSTORY11. Soy una asociación de un barrio y necesito saber las calles que pertenecen a un barrio para presentarme a sus vecinos.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX esadm: <http://vocab.linkeddata.es/datosabiertos/def/sector-publico/territorio#>
SELECT DISTINCT ?via ?barrio WHERE{
?via a escjr:Via .
?via esadm:barrio ?barrio
}
```

USERSTORY12. Soy un desarrollador de aplicaciones que necesita obtener las coordenadas de un portal para poder representarlo en un mapa.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
PREFIX geo: <http://www.w3.org/2003/01/geo/wgs84_pos#>
SELECT DISTINCT ?portal ?lat ?long ?wkt WHERE{
?portal a escjr:Portal .
OPTIONAL {?portal geosparql:hasGeometry ?geosp . ?geosp geosparql:asWKT ?wkt } .
OPTIONAL {?portal geo:geometry ?geo . ?geo geo:lat ?lat . ?geo geo:long ?long } .
}
```

USERSTORY13. Soy una empresa de marketing y necesito conocer los portales de las calles que están a menos de 500 metros de un portal determinado.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
PREFIX geof: <http://www.opengis.net/def/function/geosparql/>
SELECT DISTINCT ?portal2 ?wkt2 WHERE{
<<URIPortal>> geosparql:hasGeometry ?geosp1 .
?geosp1 geosparql:asWKT ?wkt1 .
?portal2 a escjr:Portal .
?portal2 geosparql:hasGeometry ?geosp2 .
?geosp2 geosparql:asWKT ?wkt2 .
FILTER (geof:sfWithin(?wkt2,geof:buffer(?wkt1,500)))
}
```

USERSTORY16. Soy historiador y estoy realizando un estudio y necesito conocer los nombres oficiales y populares de los barrios, calles…
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
PREFIX geonames: <http://www.geonames.org/ontology#>
SELECT DISTINCT ?via ?nombrePopular ?nombreOficial WHERE{
?via a geonames:Feature .
OPTIONAL {?via geonames:colloquialName ?nombrePopular }.
OPTIONAL {?via geonames:name ?nombreOficial }.
}
```

USERSTORY17. Soy una empresa de reparto y necesito los tipos de calle, avenidas, plazas, pasajes, caminos, etc... para localizar de forma correcta una dirección.
```sparql
PREFIX escjr: <http://vocab.linkeddata.es/datosabiertos/def/urbanismo-infraestructuras/callejero#>
SELECT DISTINCT ?tipoVia WHERE{
?via a escjr:Via .
?via escjr:tipoVia ?tipoVia .
}
```


