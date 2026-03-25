## Comparativa entre CatalogoCloud original y CatalogoCloud v2
# 1. Líneas de código ahorradas al usar grupos
En el XSD original, la estructura de hardware dentro de <servidor> incluía:

<cpu>

<ram>

<gpu> (opcional)

<discos> con sus <disco>

Atributos del servidor (id, rack, estado)

Todo esto estaba repetido dentro de cada definición de <servidor>.

En el XSD v2 se aplicaron:

Un attributeGroup para los atributos del servidor.

Un group para los componentes de hardware.

Reglas minOccurs y maxOccurs para GPU y discos.

Resultado de la comparación
Versión	Líneas dedicadas a hardware + atributos
XSD original	~52 líneas
XSD v2	~33 líneas
Ahorro	≈ 19 líneas

El ahorro proviene de eliminar duplicación y centralizar la definición en grupos reutilizables.

# 2. Error que muestra VS Code al duplicar un ID de servidor
En el XSD v2 se añadió la restricción:

xml
<xs:unique name="UnicoID">
    <xs:selector xpath="centro_datos/servidor"/>
    <xs:field xpath="@id"/>
</xs:unique>
Esto obliga a que ningún servidor pueda tener el mismo ID en todo el documento.

Si en el XML se repite un ID, por ejemplo:
xml
<servidor id="srv-web-01" ...>
...
<servidor id="srv-web-01" ...>   <!-- duplicado -->
VS Code muestra el siguiente error:

Código
Duplicate key value 'srv-web-01' for identity constraint 'UnicoID' of element 'catalogo_cloud'.
Este mensaje confirma que la validación XSD está funcionando correctamente.
