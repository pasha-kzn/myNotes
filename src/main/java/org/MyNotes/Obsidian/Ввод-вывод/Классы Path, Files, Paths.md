Обновление NIO API в JDK 7 появилось неспроста - для его появления были серьезные предпосылки, главной из которых были недостатки класса File пакета java.io. Основными из них были:
- слабый функционал по количеству методов (Например, отсутствовала возможность копировать/перемещать файлы).
- мало обратной связи в виде отсутствия у многих методов полезных выбрасываемых исключений при возникновении ошибок, так как большинство методов в случае ошибки просто возвращали false без возможности продиагностировать ошибку.
- в классе File есть практически одинаковые методы, различающиеся только по типу возвращаемого значения, в которых можно было запутаться. Например, методы getAbsoluteFile() и getAbsolutePath().
- плохая поддержка метаданных. Файл может включать атрибуты безопасности, данные о владельце файла и т.д. В частности, из-за этого не во всех операционных системах работает метод renameTo().

Для поддержки кода, написанного с помощью класса File, существуют методы toFile() и toPath() для перевода в объекты File и Path:
`Path path = Path.of("path/paths/path.txt");`
`File file = path.toFile();` 
`System.out.println(file);`
`Path pathAgain = file.toPath();` 
`System.out.println(pathAgain);`

Демонстрация методов работы с объектами типа Path

`import java.io.IOException;`
`import java.nio.file.Files;`
`import java.nio.file.Path;`
`import java.nio.file.Paths;`

`public class PathExample {`
    `public static void main(String[] args) throws IOException {`
        `Path directory = Paths.get("path/paths");`
        `Files.createDirectories(directory);`
        `Path path = Path.of("path/paths/path.txt");`
        `Files.createFile(path);`
        `System.out.println("Файл/директория существует?: " + Files.exists(path));`
        `System.out.println("Это директория?: " + Files.isDirectory(path));`
        `System.out.println("Это файл?: " + Files.isRegularFile(path));`
        `System.out.println("Имя файла: " + path.getFileName());`
        `System.out.println("Путь к файлу абсолютный?: " + path.isAbsolute());`
        `System.out.println("Родительская директория файла: " + path.getParent());`
        `System.out.println("Абсолютный путь к файлу: " + path.toAbsolutePath());`
        `System.out.println("Абсолютный путь к директории: " + directory.toAbsolutePath());`
        `System.out.println("Доступен для чтения?: " + Files.isReadable(path));`
        `System.out.println("Доступен для записи?: " + Files.isWritable(path));`
    `}`
`}`

**Перемещение файла.**
Начиная с версии JDK 7 появилась возможность переместить файл с помощью метода move().

`import java.io.IOException;`
`import java.nio.file.Files;`
`import java.nio.file.Path;`
`import java.nio.file.Paths;`

`public class PathExample {`
    `public static void main(String[] args) throws IOException {`
        `Path directory = Paths.get("path/paths");`
        `Files.createDirectories(directory);`
        `Path path = Path.of("path/paths/path.txt");`
        `Files.createFile(path);`
        `Files.move(path, Path.of("path/path.txt"));`
    `}`
`}`

**Удаление файла.**
Удалить файл можно с помощью метода Files.delete().

`import java.io.IOException;`
`import java.nio.file.Files;`
`import java.nio.file.Path;`
`import java.nio.file.Paths;`

`public class PathExample {`
    `public static void main(String[] args) throws IOException {`
        `Path directory = Paths.get("path/paths");`
        `Files.createDirectories(directory);`
        `Path path = Path.of("path/paths/path.txt");`
        `Files.createFile(path);`
        `Files.delete(directory);`
    `}`
`}`

**Методы получения информации о файлах внутри директории.**
Аналог этих методов в NIO API - это метод Files.newDirectoryStream().

`import java.io.IOException;`
`import java.nio.file.DirectoryStream;`
`import java.nio.file.Files;`
`import java.nio.file.Path;`
`import java.nio.file.Paths;`

`public class PathExample {`
    `public static void main(String[] args) throws IOException {`
        `Path directory = Paths.get("path/paths");`
        `Files.createDirectories(directory);`
        `Path target = Paths.get("path");`
        `Path pathOne = Path.of("path/paths/path1.txt");`
        `Files.createFile(pathOne);`
        `Path pathTwo = Path.of("path/path2.txt");`
        `Files.createFile(pathTwo);`
        `DirectoryStream<Path> paths = Files.newDirectoryStream(target);`
        `paths.forEach(System.out::println);`
    `}`
`}`

В примере выше мы создаем директорию paths внутри директории path, а также по одному файлу внутри каждой из этих директорий. Метод newDirectoryStream() возвращает поток, содержащий файлы и директории, находящиеся в директории path (без вложенных). В следующих строках мы получаем этот поток и выводим все его элементы на печать:

`DirectoryStream<Path> paths = Files.newDirectoryStream(target);`
`paths.forEach(System.out::println);`

В выводе видим все элементы, которые содержит директория path - директорию paths и файл path2.txt (вложенные файлы не учитываются):

[![](https://job4j.ru/api/images/imageTaskPreview?imageId=78093)](https://job4j.ru/api/images/imageTaskSource?imageId=78093)