# Notas Kata2

### Codificación del texto

Todo texto está codificado en un formato y tenemos que tener eso en cuenta

El formato (charset) indica cómo se pasa de bytes a letras. Lo habitual hoy es usar UTF-8.

Si se guarda en un formato y se lee en otro, aparecen caracteres extraños como "Ã±" en lugar de "ñ".

### Streams

**Stream:** "Riachuelo/Arroyo" Pasar bytes

Es decir, los bytes van llegando poco a poco, no se tiene todo el archivo a la vez.

InputStream sirve para leer y OutputStream para escribir.

- Fileinputstream como está implementada la lista
- Socketinputstream otra forma

Las dos son InputStream, pero cada una obtiene los bytes de un sitio distinto: una de un archivo y la otra de la red.

La ventaja es que al código que las usa no le importa de dónde vienen los datos, se leen igual.

### Interfaces

Los streams trabajan con bytes, mientras que Reader y Writer trabajan con texto (caracteres) -> por eso aquí sí hay que indicar el formato

#### Reader

```java
try(Reader reader = new FileReader(file, format)) { // FileReader(file, Aquí se puede poner el formato a truncar)
	System.out.println(reader.readAllAsString());
}
```

El format es el charset, por ejemplo StandardCharsets.UTF_8

readAllAsString() lee todo el texto de una vez (es un método nuevo, no está en versiones antiguas de Java)

Para leer línea a línea -> BufferedReader con readLine()

#### Writer

Funciona igual que Reader, pero para escribir

```java
try(Writer writer = new FileWriter(file, format)) {
	writer.write("Hola");
}
```

Por defecto sobrescribe el contenido que hubiera en el archivo.

Para añadir al final sin borrar -> FileWriter(file, format, true)

### File y URL

```java
File file = new File(Nombre del archivo) // Creamos
