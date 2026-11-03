# Notas Kata2

### Codificación del texto

Todo texto está codificado en un formato y tenemos que tener eso en cuenta

El formato (charset) es lo que dice cómo pasar de bytes a letras. Lo normal hoy es usar UTF-8.
Si guardas en uno y lees en otro te salen las cosas raras tipo "Ã±" en vez de "ñ" -> ojo con eso

### Streams

**Stream:** "Riachuelo/Arroyo" Pasar bytes

O sea, los bytes van llegando poco a poco, no tienes todo el archivo de golpe.
InputStream para leer y OutputStream para escribir.

- Fileinputstream como está implementada la lista
- Socketinputstream otra forma

Las dos son InputStream pero cada una saca los bytes de un sitio: una de un archivo y la otra de la red.
Lo bueno es que a tu código le da igual de dónde vengan, lee igual.

### Interfaces

Los streams van con bytes, Reader y Writer van con texto (caracteres) -> por eso aquí sí hay que decir el formato

#### Reader

```java
try(Reader reader = new FileReader(file, format)) { // FileReader(file, Aquí se puede poner el formato a truncar)
	System.out.println(reader.readAllAsString());
}
```

El format es el charset, p.ej. StandardCharsets.UTF_8
readAllAsString() te lo lee todo de una (es nuevo, en Java antiguo no está)
Si quieres ir línea a línea -> BufferedReader con readLine()

#### Writer

Lo mismo que Reader pero al revés, para escribir

```java
try(Writer writer = new FileWriter(file, format)) {
	writer.write("Hola");
}
```

Cuidado que por defecto machaca lo que hubiera en el archivo.
Si quieres añadir al final -> FileWriter(file, format, true)

### File y URL

```java
File file = new File(Nombre del archivo) // Creamos el archivo para trabajar con el
URL url = new URL(direccion)
```

Ojo: new File() no crea nada en el disco, solo guarda la ruta. Para saber si existe -> file.exists()
Con la URL haces url.openStream() y te da un InputStream, así que se lee igual que un archivo
new URL() está deprecated en las versiones nuevas -> mejor URI.create(direccion).toURL()

### Abrir y cerrar archivos

```java
Abrir archivos -> InputStream is = new FileInputStream(file) // <- NO PONER FileInputStream

		  is.close() // --> SIEMPRE QUE ABRIMOS UN ARCHIVO HAY QUE CERRARLO
```

Lo de no poner FileInputStream es a la izquierda: declaras la variable como InputStream (lo general) y no como la clase concreta.
Así si luego cambias de dónde lees solo tocas la parte de la derecha.

> **SIEMPRE QUE ABRIMOS UN ARCHIVO HAY QUE CERRARLO**

Si no lo cierras el archivo se queda pillado.
Y el problema de hacer el close() a mano es que si salta una excepción antes, nunca llega a cerrarse.

#### Try with resources

Forma correcta de cerrar el archivo:

```java
try(InputStream is = new FileInputStream(file)){ // --> Cuando salga del try se cierra [TRY WITH RESOURCES]
	is.readAllBytes();
}
```

Se cierra solo aunque pete algo dentro. Vale para cualquier cosa que implemente AutoCloseable
y puedes meter varios dentro del try separados por ;
