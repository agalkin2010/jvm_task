# Работа JVM

### Пример кода:

```java
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```



### Загрузка программы:
1. В первую очередь осуществляется Loading: 
     Application ClassLoader пытается найти необходимый класс JvmComprehension у себя в кэше. Если не нашел запрашивает его у Platform ClassLoader. Platform ClassLoader пытается найти необходимый класс у себя в кэше. Если не нашел запрашивает его у Bootstrap ClassLoader. Bootstrap ClassLoader пытается найти необходимый класс у себя в кэше. Если не находит, тогда пытается загрузить его по своему пути. Если загрузить не удалось, тогда возвращается в Platform ClassLoader. Platform ClassLoader пытается загрузить его по своему пути. Если загрузить не удалось, тогда возвращается в Application ClassLoader. Application ClassLoader
пытается загрузить его по своему пути. Если загрузить не удалось тогда возвращается ошибка ClassNotFoundException.
2. После того как необходимые классы найдены (JvmComprehension, системные классы) осуществляется Linking: проверка на валидность кода, подготовка примитивов, связывание ссылок на другие классы.
3. После процесса Linking осуществляется инициализация: выполняются инициализаторы static (main)

### Выполнение программы в Runtime:
1. Создается фрейм main() в стэк.
2. В фрейм main() заносится i=1
3. В куче создается новый объект Object()
4. В фрейм main() заносится ссылка o на на Object()
5. В куче создается новый объект Integer=2
6. В фрейм main() заносится ссылка ii на объект Integer=2
7. Создается новый фрейм printAll() в стэк
8. В фрейме printAll() создаются: o ссылка на на Object(), i=1, ii ссылка на объект Integer=2
9. В куче создается новый объект Integer=700
10. В фрейм printAll() заносится ссылка uselessVar на объект Integer=700
11. В куче создается 3 новые объекта строки (o.toString(), i, ii)
12. В куче создается новая строка из 3 строк(o.toString() + i + ii)
13. Создается новый фрейм println() в стэк
14. В фрейм println() заносится ссылка на строку o.toString() + i + ii
15. В куче создается новая строка "finished"
16. Создается новый фрейм println() в стэк
17. В фрейм println() заносится ссылка на строку "finished"
 
### Сборка мусора:
Периодически происходит остановка выполнения программы для поиска неиспользуемых объектов в программе и удаления их из кучи и дефрагментация используемых объктов в куче (объект Integer=700, строки после вывода в println удалятся из кучи, т. к. дальше не используются в программе).
