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
File file = new File(Nombre del archivo) // Creamos el archivo para trabajar con el
URL url = new URL(direccion)
```

new File() no crea el archivo en el disco, solo guarda la ruta.

Para comprobar si existe -> file.exists()

Con la URL se usa url.openStream(), que devuelve un InputStream, así que se lee igual que un archivo.

new URL() está obsoleto en las versiones nuevas de Java -> es mejor usar URI.create(direccion).toURL()

### Abrir y cerrar archivos

```java
Abrir archivos -> InputStream is = new FileInputStream(file) // <- NO PONER FileInputStream

		  is.close() // --> SIEMPRE QUE ABRIMOS UN ARCHIVO HAY QUE CERRARLO
```

Lo de no poner FileInputStream se refiere a la parte izquierda: la variable se declara como InputStream (el tipo general) y no como la clase concreta. Así, si más adelante cambia el origen de los datos, solo hay que modificar la parte derecha.

> **SIEMPRE QUE ABRIMOS UN ARCHIVO HAY QUE CERRARLO**

Si no se cierra, el archivo queda ocupado.

El problema de llamar a close() manualmente es que, si se produce una excepción antes, el archivo nunca llega a cerrarse.

#### Try with resources

Forma correcta de cerrar el archivo:

```java
try(InputStream is = new FileInputStream(file)){ // --> Cuando salga del try se cierra [TRY WITH RESOURCES]
	is.readAllBytes();
}
```

El archivo se cierra automáticamente aunque se produzca un error dentro del try.

Funciona con cualquier clase que implemente AutoCloseable, y se pueden abrir varios recursos en el mismo try separándolos con ;
