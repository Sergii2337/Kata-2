# Notas Kata2

### Codificación del texto

Todo texto está codificado en un formato y tenemos que tener eso en cuenta

- El formato (charset) indica cómo se pasa de bytes a letras
- Lo habitual hoy es usar UTF-8
- Si se guarda en un formato y se lee en otro -> aparecen caracteres extraños como "Ã±" en lugar de "ñ"

### Streams

**Stream:** "Riachuelo/Arroyo" Pasar bytes

- Los bytes van llegando poco a poco, no se tiene todo el archivo a la vez
- InputStream -> leer
- OutputStream -> escribir

- Fileinputstream como está implementada la lista
- Socketinputstream otra forma

- Las dos son InputStream, pero cada una obtiene los bytes de un sitio distinto
- FileInputStream -> de un archivo
- SocketInputStream -> de la red
- Al código que las usa no le importa de dónde vienen los datos, se leen igual

### Interfaces

- Streams -> trabajan con bytes
- Reader y Writer -> trabajan con texto (caracteres)
- Por eso en Reader y Writer sí hay que indicar el formato

#### Reader

```java
try(Reader reader = new FileReader(file, format)) { // FileReader(file, Aquí se puede poner el formato a truncar)
	System.out.println(reader.readAllAsString());
}
```

- format -> el charset, por ejemplo StandardCharsets.UTF_8
- readAllAsString() -> lee todo el texto de una vez (método nuevo, no está en versiones antiguas de Java)
- Para leer línea a línea -> BufferedReader con readLine()

#### Writer

- Funciona igual que Reader, pero para escribir

```java
try(Writer writer = new FileWriter(file, format)) {
	writer.write("Hola");
}
```

- Por defecto sobrescribe el contenido del archivo
- Para añadir al final sin borrar -> FileWriter(file, format, true)

### File y URL

```java
File file = new File(Nombre del archivo) // Creamos el archivo para trabajar con el
URL url = new URL(direccion)
```

- new File() no crea el archivo en el disco, solo guarda la ruta
- Para comprobar si existe -> file.exists()
- url.openStream() -> devuelve un InputStream, se lee igual que un archivo
- new URL() está obsoleto en las versiones nuevas de Java -> usar URI.create(direccion).toURL()

### Abrir y cerrar archivos

```java
Abrir archivos -> InputStream is = new FileInputStream(file) // <- NO PONER FileInputStream

		  is.close() // --> SIEMPRE QUE ABRIMOS UN ARCHIVO HAY QUE CERRARLO
```

- NO PONER FileInputStream -> se refiere a la parte izquierda
- La variable se declara como InputStream (tipo general), no como la clase concreta
- Si cambia el origen de los datos, solo hay que modificar la parte derecha

> **SIEMPRE QUE ABRIMOS UN ARCHIVO HAY QUE CERRARLO**

- Si no se cierra -> el archivo queda ocupado
- Problema de close() manual -> si se produce una excepción antes, el archivo nunca se cierra

#### Try with resources

Forma correcta de cerrar el archivo:

```java
try(InputStream is = new FileInputStream(file)){ // --> Cuando salga del try se cierra [TRY WITH RESOURCES]
	is.readAllBytes();
}
```

- Se cierra automáticamente aunque haya un error dentro del try
- Funciona con cualquier clase que implemente AutoCloseable
- Se pueden abrir varios recursos en el mismo try separados por ;
