# Домашнее задание к занятию "Таблицы"

## Цель задания

1. Попрактиковаться в применении хеширования
2. Использовать ленивость для улучшения асимптотики алгоритма

------

## Инструкция к заданию

1. Для каждой задачи создайте отдельный реплит, если об обратном не сказано в условии
1. Саму программу пишите в IDEA, реплит используется только для сдачи кода
1. В окне редактора IDEA наберите программный код, решающий поставленную задачу, на основе данной в условии заготовки кода
1. Загружайте файлы из папки src проекта в реплит
1. Отправьте выполненную работу на проверку в личном кабинете Нетологии

------

## Материалы, которые пригодятся для выполнения задания

3. [Как поделиться реплитом для проверки?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitShare.md)
4. [Как автоотформатировать код?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_Format.md)
5. [Как залить проект из IDEA в реплит?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitUpload.md)

------

## Задание 1 (обязательное)
Есть строка `source` и задано число `size`, которое меньше её длины.
Требуется проверить, нет ли таких двух подстрок `source` длины `size`, которые были равны.

Сперва посмотрим на заготовку кода, которой будем тестировать наши алгоритмы.

### Заготовка кода
Используйте этот код в качестве заготовки кода вашего проекта. Менять код в `main` нельзя.

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        String source = "CACABABABCCCAABAC";

        System.out.println(hasRepeats(source, 4)); // true, тк ABAB встречается два раза, хоть эти куски и пересекаются
        System.out.println(hasRepeats(source, 5)); // false
    }

    public static boolean hasRepeats(String source, int size) {
        //...
    }

}
```

### Наивное решение
Первое решение, которое может прийти в голову, это собрать все подстроки заданной длины, сложить в список, а затем перебрать всевозможные пары этих подстрок для поиска совпадения:

```java
    public static boolean hasRepeats(String source, int size) {
        List<String> slices = new ArrayList<>(); // список всех подстрок длины size
        for (int i = 0; i <= source.length() - size; i++) { // перебор всех мест старта подстроки
            String slice = source.substring(i, i + size); // вырезание подстроки
            slices.add(slice); // сохраняем подстроку
        }
        for (int i = 0; i < slices.size(); i++) { // перебираем все пары подстрок для проверки совпадения
            for (int j = i + 1; j < slices.size(); j++) {
                String slice1 = slices.get(i);
                String slice2 = slices.get(j);
                if (slice1.equals(slice2)) {
                    return true;
                }
            }
        }
        return false; // если бы нашли, то вышли бы по return true, а значит повторов нет
    }
```

Но такой алгоритм очень медленный:
* Сбор всех подстрок в список занимает O(n<sup>2</sup>): `substring` линейный относительно длины вырезаемого кусочка из-за копирования символов в новую строку (т.е. условно O(n)), а вырезаем мы O(n) раз, вот и получается O(n) * O(n) = O(n<sup>2</sup>).
* Перебор всех пар подстрок занимает O(n<sup>3</sup>): количество пар у нас O(n<sup>2</sup>), а само посимвольное сравнение у нас линейное, вот и получается O(n<sup>2</sup>) * O(n) = O(n<sup>3</sup>).
* В итоге имеем O(n<sup>2</sup>) + O(n<sup>3</sup>) = O(n<sup>3</sup>), что весьма затратно.

### Решение на хешах
Ускорим этот алгоритим следующей идеей.
Будем складывать подстроки не в список, а в хеш-сет.
Перед добавлением элемента проверим, есть ли уже такой в нём или нет.
Если есть, то мы нашли совпадение.

```java
    public static boolean hasRepeats(String source, int size) {
        Set<String> slices = new HashSet<>(); // множество всех подстрок длины size
        for (int i = 0; i <= source.length() - size; i++) { // перебор всех мест старта подстроки
            String slice = source.substring(i, i + size); // вырезание подстроки
            if (slices.contains(slice)) { // проверка на наличие повтора этой подстроки
                return true; // если уже встречали, значит повторы нет
            } else {
                slices.add(slice);  // иначе запоминаем подстроку и перебираем дальше
            }
        }
        return false; // если бы нашли, то вышли бы по return true, а значит повторов нет
    }
```

Хешсет работает быстро, ему не нужно ничего перебирать, его проверка за O(1).
Асимптотика этого алгоритма будет быстрее - O(n<sup>2</sup>).
Всё из-за медленного (линейного) вырезания подстроки из `source` и таким же линейным подсчётом хеша для неё, который будет использовать хешсет.

### Ещё быстрее
Давайте ускорим предыдущий алгоритм до линейной асимптотики.
Реализация этих идей как раз и будет вашеим заданием.

Сперва выберем алгоритм хеш-функции.
В рамках этой задачи мы будем работать с хешированием текста как с суммой кодов символов этого текста.

```java
String s = "Hello"; // пример строки для расчёта хеша
int hash = 0;
for (int i = 0; i < s.length(); i++) {
    hash += s.charAt(i);
}
System.out.println("Хеш для '" + s + "' равен " + hash);
```

Теперь давайте создадим свой аналог String-а - `LazyString`.
Отличаться он будет двумя принципиальными вещами:
1. Подстрока будет создаваться в большинстве случаев за O(1) вместо O(n)
2. Хеш для соседней подстроки можно будет получить из предыдущей за O(1), а не пересчитывать линейно.

Для того, чтобы взятие подстроки было за O(1), мы прежде всего не будем реально копировать символы этой подстроки в новый объект.
Вместо этого, мы будем просто запоминать из какой строки мы хотели подстроку и на каком месте.
Это можно назвать ленивостью нашей реализации класса для подстроки.
Если нам понадобятся символы нашей подстроки, то мы просто заглянем в соответствующее место нашей исходной строки.

Также мы предоставим два способа создания нашей подстроки:
* Через конструктор `public LazyString(String source, int start, int end)`, который запоминает нужное место и считает и запоминает хеш подстроки (тк этому подсчёту придётся перебрать символы подстроки, то создание через этот конструктор является линейным).
* Через вызов метода `public LazyString shiftRight()`, который вернёт подстроку, соседнюю на один шаг вправо от той, у кого вы вызвали этот метод. Такое создание будет за O(1), тк нам нужно будет лишь сдвинуть границы подстроки который мы запоминаем и пересчитать хеш на основе хеша предыдущей строки, для чего достаточно лишь вычесть код выпавшего символа и прибавить код нового символа.

```java
public class LazyString {
    private String source; // ссылка на исходную строку
    private int start, end; // границы нашей подстроки
    private int hash; // запоминаем хеш чтобы не пересчитывать

    private LazyString() {}

    public LazyString(String source, int start, int end) {
        this.source = source;
        this.start = start;
        this.end = end;

        // ВАШ КОД
        // а тут нужно посчитать hash
        // просто сложите все коды символов нашей подстроки
        // и сохраните в поле hash.
        // из-за этого, создание LazyString через конструктор будет линейным
    }

    public LazyString shiftRight() {
        // Это способ создания новой LazyString через предыдущую, работает за О(1)
        LazyString shifted = new LazyString();
        shifted.source = source;
        shifted.start = start + 1;
        shifted.end = end + 1;

        // ВАШ КОД
        // Вычислите хеш для shifted из хеша для исходной строки
        // и заполните его в shifted.hash.
        // Заметьте, что достаточно просто вычесть код того
        // символа, что исчез и прибавить код того символа, что
        // появился

        return shifted;
    }

    public int length() {
        return end - start;
    }

    public boolean equals(LazyString that) {
        // если не равны по длине, то не равны и вовсе
        if (length() != that.length()) {
            return false;
        }

        // перебираем и сравниваем на равенство все символы
        for (int i = 0; i < length(); i++) {
            char myChar = source.charAt(start + i);
            char thatChar = source.charAt(that.start + i);
            if (myChar != thatChar) { // если хотя бы один не совпал, то не равны
                return false;
            }
        }
        return true;
    }

    @Override
    public int hashCode() {
        return hash; // хеш у нас всегда предпосчитан для каждого объекта, чтобы не тратить на это время
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        LazyString that = (LazyString) o;
        return this.equals(that);
    }

}
```

А метод поиска видоизменится так:
```java
    public static boolean hasRepeats(String source, int size) {
        Set<LazyString> slices = new HashSet<>(); // множество всех подстрок длины size
        LazyString prev = null; // переменная для сохранения предыдущей подстроки
        for (int i = 0; i <= source.length() - size; i++) { // перебор всех мест старта подстроки
            LazyString slice; // вырезание подстроки
            if (prev == null) {
                // первую подстроку создаём конструктором за линейную асимптотику
                // ВАШ КОД
            } else {
                // все остальные через сдвиг вправо от предыдущей подстроки, за O(1)
                // ВАШ КОД
            }
            if (slices.contains(slice)) { // проверка на наличие повтора этой подстроки
                return true; // если уже встречали, значит повторы нет
            } else {
                slices.add(slice);  // иначе запоминаем подстроку и перебираем дальше
            }
            prev = slice; // не забываем обновить переменную для предыдущей подстроки для следующей итерации цикла
        }
        return false; // если бы нашли, то вышли бы по return true, а значит повторов нет
    }
```

В итоге асимптотика улучшится до линейной, тк ленивый `substring` с подсчётом хеша на основе предыдущей подстроки работать будет гораздо быстрее.

------


## Правила приема работы

Прикреплён реплит с решением задачи

------

## Критерии оценки

1. Программа оформлена на основе заготовки кода, предоставленной в условии
1. Программа запускается и отрабатывает без ошибок
2. Программа соответствует всем требованиям из условия задачи
3. Программа работает правильно на всех примерах из условия
4. Программный код соответствует пройденным требованиям к качеству кода
5. Программа спроектирована достаточно логично и правильно, не противоречит общепринятым в производстве практикам и традициям
6. Программа реализует достаточный для зачёта по эффективности алгоритм
7. При наличии недочётов, в зависимости от их серьёзности и количества, работа может быть отправлена на доработку или принята; решение принимается на основе экспертной оценки работы.

## Решение

[Задание 1](https://replit.com/@NewAge1979/Example61-2#Main.java)