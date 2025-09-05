##### Класс File

Класс File предназначен для работы с файлами и директориями в файловой системе. Объект этого класса представляет собой абстрактное описание пути к файлу или директории. С помощью File можно создавать новые файлы и директории на диске, однако сам класс не предоставляет методов для изменения или чтения содержимого файла. Вместо этого он позволяет получать информацию о файле, такую как дата создания, размер, права доступа и путь к файлу или директории. Объекты класса File неизменяемы, то есть после создания объект не может изменить свой путь

**Методы класса File**

`import java.io.File;`
`import java.io.IOException;`

`public class FileExample {`
    `public static void main(String[] args) throws IOException {`
        `File directory = new File("src/main/java/ru/job4j/io/files");`
        `directory.mkdir(); // Создает директорию, если она не существует`
        `File file = new File("src/main/java/ru/job4j/io/files/file.txt");`
        `file.createNewFile(); // Создает новый файл, если он не существует`

        `// Вывод свойств файла и директории`
        `System.out.println("Файл/директория существует?: " + file.exists());`
        `System.out.println("Это директория?: " + file.isDirectory());`
        `System.out.println("Это файл?: " + file.isFile());`
        `System.out.println("Имя файла: " + file.getName());`
        `System.out.println("Путь к файлу: " + file.getPath());`
        `System.out.println("Путь к файлу абсолютный?: " + file.isAbsolute());`
        `System.out.println("Относительный путь к родительской директории файла: " + file.getParent());`
        `System.out.println("Абсолютный путь к файлу: " + file.getAbsoluteFile());`
        `System.out.println("Абсолютный путь к директории: " + directory.getAbsolutePath());`
        `System.out.println("Доступен для чтения?: " + file.canRead());`
        `System.out.println("Доступен для записи?: " + file.canWrite());`
        `System.out.println("Длина файла (в байтах): " + file.length());`

        `// Переименование файла`
        `File newFile = new File("src/main/java/ru/job4j/io/files/newFile.txt");`
        `System.out.println("Переименование файла в newFile. Успешно?: " + file.renameTo(newFile));`
    `}`
`}`

**Работа с директориями и файлами**

`import java.io.File;`
`import java.io.IOException;`

`public class DirectoryExample {`
    `public static void main(String[] args) throws IOException {`
        `File directory = new File("src/main/java/ru/job4j/io/files/directory");` 
        `directory.mkdirs(); // Создает директорию, включая все промежуточные директории`
        `File target = new File("src/main/java/ru/job4j/io/files");` 
        `File fileOne = new File("src/main/java/ru/job4j/io/files/file1.txt");` 
        `fileOne.createNewFile(); // Создает новый файл`
        `File fileTwo = new File("src/main/java/ru/job4j/io/files/directory/file2.txt");` 
        `fileTwo.createNewFile(); // Создает новый файл`
        `String[] list = target.list(); // Получаем имена файлов и директорий в target`
        `for (String fileName : list) {`
            `System.out.println(fileName);`
        `}`
        `File[] listFiles = target.listFiles(); // Получаем объекты File для файлов и директорий в target`
        `for (File file : listFiles) {`
            `System.out.println(file);`
        `}`
    `}`
`}`

В данном примере метод list() возвращает массив строк с именами файлов и директорий, содержащихся в директории files:
Метод listFiles() возвращает массив объектов типа File, представляющих файлы и директории, содержащиеся в директории files:


**Перемещение файла**

До JDK 7 не было стандартного метода для перемещения файлов между директориями, однако существуют два основных подхода для выполнения этой задачи:

- Если нужно переместить содержимое файла, можно использовать метод File.renameTo(), который переименует файл, тем самым "перемещая" его в новую локацию. Имейте в виду, что этот метод может не работать во всех файловых системах.
    
- Если требуется переместить файл в другую директорию, можно сначала скопировать содержимое файла в новый файл в целевой директории, а затем удалить исходный файл.
    

С приходом NIO API в JDK 7 и позже, появились новые методы для работы с файлами и директориями, которые будут рассмотрены в следующих уроках.

---

**Удаление файла**

Файл или директорию можно удалить с помощью метода delete(). Важно помнить, что директорию можно удалить только если она пуста.