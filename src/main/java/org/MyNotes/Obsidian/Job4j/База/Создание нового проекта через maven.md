
Откройте командную строку cmd. Перейдите в папку c:\projects\

cd c:\projects\

Эта команда создаст папку job4j_design.
mvn archetype:generate -DgroupId=ru.job4j -DartifactId=job4j_design -DarchetypeArtifactId=maven-archetype-simple -DarchetypeVersion=1.4 -DinteractiveMode=false

Проект создается по шаблону. Этот шаблон имеет избыточный код. Упростим его.
Зайдите в папку job4j_design и откройте через Notepad файл pom.xml
1. Изменим версию jdk на 17.
2. Для сборки проекта пока оставим библиотеку тестирования junit. (в дальнейшем мы заменим эту зависимость более свежей)
3. Удалим блоки build и reporting.  Они не нужны для начального проекта.

Этот проект можно собрать в консоли.
в папке c:\projects\job4j_design выполните команду

mvn install

Открыть проект в IDEA
Если что там докрутить крутилку


