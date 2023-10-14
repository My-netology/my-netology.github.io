# Домашнее задание к занятию "Структура программы"

## Цель задания

1. Создадите свои собственные команды (статические методы) в Java
1. Напишете программы, состоящие из нескольких файлов и папок
1. Попрактикуетесь в программном считывании информации с консоли

------

## Инструкция к заданию

1. Для каждой задачи создайте отдельный реплит, если об обратном не сказано в условии
1. В открывшемся окне редактора кода наберите программный код, решающий поставленную задачу
1. Запустите реплит, убедитесь что всё отрабатывает без ошибок
1. Отправьте выполненную работу на проверку в личном кабинете Нетологии

------

## Материалы, которые пригодятся для выполнения задания

1. [Как поделиться реплитом для проверки?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitShare.md)

------

## Задание 1 (обязательное)

Ваша задача написать калькулятор для расчёта пошлины для провоза товара.
Пошлина будет рассчитывается по следующему принципу: 
1. 1 копейка за каждый рубль цены товара
2. 100 рублей за каждый килограмм товара

Копейки в итоговой сумме пошлины можно не учитывать, оставляя только рубли.
Например, для товара ценой в 546 рублей и весом 3 кг размер пошлины составит 5.46 + 3 * 100 = 305 рублей.

Программа должна приветствовать пользователя, спрашивать стоимость в рублях и вес товара в кг (всё целые числа), а в ответ сообщать размер пошлины.

Логика рассчёта пошлины по характеристикам товара должна быть вынесена в отдельный статический метод. Он должен:
1. Принимать два параметра - цену и вес товара (целые числа)
2. Возвращать посчитанную сумму пошлины

**Дополнительно (НЕобязательно к исполнению):** Для того чтобы пользователь мог вводить данные на той же строке, что и сообщение программы, выводите это сообщение через `System.out.print()` вместо `System.out.println()`. В таком случае, программа не будет делать перенос строки в конце сообщения. 

### Пример работы программы
```text
Введите цену товара (в руб.): 546
Введите вес товара (в кг.): 3
Размер пошлины (в руб.) составит: 305
```

### Шаги реализации
1. Создайте пустой реплит-проект

2. В `main` заведите сканнер, не забудьте его заимпортить

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
    }
}
```

3. Запросите в `main` у пользователя цену и вес товара, сохраните данные в переменные:

```java
System.out.print("Введите цену товара (в руб.):");
int price = scanner.nextInt();
...
```

4. Создайте статический метод для расчёта пошлины, разместите его до или после метода `main` (`???` замените на нужный java-код):

```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        ...
    }
    
    public ??? ??? calculateCustoms(???) {
        ...
        return ???;
    }
}
```

5. В `main` вызовите этот метод, передав считанные от пользователя данные

6. Сохраните результат вызванного метода в переменную, выведите значение из неё на экран

------

## Задание 2 (НЕобязательное)

:warning: _Эта задача - усложнение первой задачи, поэтому делается в том же реплите. Как итог вы сдаёте один реплит, в которой либо первая, либо вторая задача._

Вынесите статиечский метод в отдельный класс `CustomsService`, который лежал бы в пакете `ru.netology.service`.

Ставка пошлины на вес товара должна быть вынесена в статическое поле класса `CustomsService`.
Обратите внимание, что в таких случаях, когда статическое поле содержит какое-то глобальное неизменяющееся во время работы программы значение, то при объявлении такого поля также указывают модификатор `final` (после слова `static` и до типа поля - так обозначается неизменяемость ячейки, ей нельзя будет поменять значение в другом месте программы), а в имени поля используют не камелкейс, а заглавные буквы с разделением через подчёркивание. Например, имя `someFieldName` превратится в `SOME_FIELD_NAME`.

------

## Правила приема работы

Прикреплена только одна ссылка на один реплит с решением либо первой, либо второй задачи.

------

## Критерии оценки

1. Программа запускается и отрабатывает без ошибок
2. Программа соответствует всем требованиям из условия задачи
3. Программа работает правильно на всех примерах из условия
4. Программный код соответствует пройденным требованиям к качеству кода
5. Программа спроектирована достаточно логично и правильно, не противоречит общепринятым в производстве практикам и традициям
6. Программа реализована только с использованием пройденных интсрументов и не содержит в себе побочного не упомянутого в условии функционала
7. При наличии недочётов, в зависимости от их серьёзности и количества, работа может быть отправлена на доработку или принята; решение принимается на основе экспертной оценки работы.

## Решение

[Задание 1 и 2](https://replit.com/@NewAge1979/Example21#Main.java)