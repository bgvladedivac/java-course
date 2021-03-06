## Обектно-ориентирано програмиране с Java

#### Част II

_23.10.2019_

---

#### Предната лекция говорихме за:

@ul

- Класове и обекти
- Абстрактни класове и интерфейси
- Фундаменталните ООП принципи
  - Енкапсулация
  - Наследяване
  - Полиморфизъм
- `java.lang.Object`

@ulend

---

#### Днес ще разгледаме:

@ul

- Интерфейсите в повече детайли
- Четвъртия фундаментален ООП принцип - Абстракция
- Статични член-променливи и статични методи
- Enums
- Изключения - въведение

@ulend

---

```java
public interface Animal {
    void move();
    void communicate();
}

public class Human implements Animal {
    private String name;

    public Human(String name) {
        this.name = name;
    }
    public void move() {
        System.out.println("I am walking using two legs");
    }
    public void communicate() {
        System.out.println("I speak");
    }
}

public class Cat implements Animal {
    public void move() {
        System.out.println("I am walking using 4 toes");
    }
    public void communicate() {
        System.out.println("I mew");
    }
}
```

@snap[south span-100]
@[1-4](interface Animal)
@[6-18](class Human)
@[20-27](class Cat)
@snapend

---

#### Методи на интерфейсите

@ul

- Методите на интерфейсите са `public` и `abstract` по подразбиране
- Модификаторите `public` и `abstract` (заедно или поотделно) могат да бъдат указани и експлицитно
    - дали да бъдат експлицитно указани, е въпрос на стил - но е добре да сме консистентни
- Тъй като методите са абстрактни, не могат да бъдат декларирани като `final`

@ulend

---

#### Интерфейси и наследяване

@ul

- Интерфейсите могат да се наследяват
- Един интерфейс може да наследява множество интерфейси

@ulend

---

#### Интерфейсите и имплементаците им

@ul

- Интерфейсите не могат да се инстанцират
- Можем да инстанцираме (конкретни) класове, които ги имплементират
- Можем да присвояваме инстанция на клас на променлива от тип интерфейс, който класът имплементира
- Можем да проверяваме с `instanceof` дали клас имплементира даден интерфейс
- Един клас може да имплементира множество интерфейси

@ulend

---

#### Абстракция

@ul

- Абстракция означава, моделирайки в обектно-ориентиран език за програмиране обекти от реалния или виртуалния свят, да се ограничим само до съществените им за конкретната задача характеристики и да се абстрахираме (пропуснем) в модела несъществените или нерелевантни за задачата
    - Пример: моделирайки студент, да го характеризираме само с име и факултетен номер, абстрахирайки се от всички други характеристики на студента в реалния свят (напр. цвят на очите)

@ulend

---

#### Абстракция

@ul

- Абстракция също означава да работим с нещо, което знаем как да използваме, без да знаем как работи вътрешно. Всяка конкретна имплементация на поведение е скрита в своя обект, за външния свят е видимо само поведението (т.е. интерфейсът)
- Принципът за абстракция се постига в Java чрез интерфейси и абстрактни класове

@ulend

---

### Статични член-променливи и статични методи

---

#### Статични член-променливи и статични методи

- Те са част от класа, а не от конкретна негова инстанция (обект)
- Могат да се достъпват без да е създаден обект: само с името на класа, точка, името на статичната член-променлива или метод

<br/>

```java
System.out.println(Math.PI);
System.out.println(Math.pow(3, 2));
```

@snap[south span-100]
@[1-1](3.141592653589793)
@[2-2](9.0)
@snapend

---

#### Статични член-променливи

@ul

- Статичните член-променливи имат едно-единствено копие, което се споделя от всички инстанции на класа
  - ако са константи, пестим памет (няма смисъл да се мултиплицират във всяка инстанция)
  - ако са променливи, всяка инстанция "вижда" и променя една и съща стойност, което е механизъм за комуникация между всички инстанции на дадения клас

@ulend

---

#### Статични методи

@ul

- Статичните методи имат достъп само до статичните член-променливи и други статични методи на класа
- Нестатичните методи имат достъп както до статичните, така и до нестатичните членове на класа

@ulend

---

```java
public class Utils {
    public static final double PI = 3.14;
    private static int radius = 10;
    private String fact5 = "5!";

    public static long fact(int n) {
        if (n == 1) { return 1; } else { return n * fact(n - 1); }
    }
    public String getFact() {
        return fact5;
    }

    public static void main(String[] args) {
        System.out.println("Perimeter is " + 2 * Utils.PI * radius);
        System.out.println(new Utils().getFact() + "=" + Utils.fact(5));
        Utils.getFact();
    }
}
```

@snap[south span-100]
@[2](constant)
@[3](static member)
@[4](non-static member)
@[6-8](static method)
@[9-11](non-static method)
@[16](won't compile)
@snapend

---

#### Интерфейси и имплементациите им

@ul

- Ако даден клас декларира, че имплементира интерфейс, той трябва или
  - да даде дефиниции на *всичките* му методи, или
  - да бъде деклариран като абстрактен
- Като следствие, ако променим сигнатурата на метод(и) на интерфейса, или ако добавим нов(и) метод(и), трябва да променим и всички имплементиращи интерфейса класове
- Това често е проблем, а понякога е и невъзможно (нямаме контрол върху всички имплементации)

@ulend

---

#### Default методи в интерфейсите (от Java 8)

@ul

- Default-ен метод в интерфейс е метод, който
  - има имплементация
  - има модификатора `default` в декларацията си
- Имплементиращите класове имплицитно ползват default-ната имплементация на методите, но могат и да я предефинират

@ulend

---

#### Default методи в интерфейсите (от Java 8)

@ul

- Клас може да имплементира произволен брой интерфейси
- Ако два или повече от тях съдържат `default` метод с еднаква сигнатура, класът трябва задължително да предефинира този метод
- В предефинирания метод може експлицитно да се укаже, default-ната имплементация от кой интерфейс да се ползва. В този случай, синтаксисът е, `<имеНаИнтерфейс>.super.<имеНаDefaultМетод>()`

@ulend

---

#### Статични методи в интерфейсите (от Java 8)

@ul

- От Java 8, интерфейсите могат да съдържат и статични методи с имплементация
- Една класическа употреба е, за *factory* методи (ще говорим за тях в лекцията за design patterns)

@ulend

---

#### Private методи в интерфейсите (от Java 9)

@ul

- От Java 9, интерфейсите могат да съдържат и `private` методи с имплементация
- Изполват се, когато в интерфейса има два ли повече default-ни или статични метода, чиято имплементация частично се дублира
    - тогава изнасяме повтарящия се код в `private` метод, за да предотвратим code duplication

@ulend

---

#### Интерфейсите - обобщение

@ul

- Интерфейсите могат да съдържат
  - Публични, абстрактни методи без имплементация
  - static final член-променливи == константи
  - `default` и `static` методи с имплементация (от Java 8)
  - `private` методи (от Java 9)

@ulend

---

#### Интересни частни случаи на интерфейси

@ul

- Интерфейс, който
    - не съдържа нито един метод, се нарича *маркерен*
    - има точно един публичен абстрактен метод, се нарича *функционален*

@ulend

---

### Enums

---

#### Enum

@ul

- Специален референтен тип (клас), представящ фиксирано множество от инстанции-константи
- Нарича се enum(eration), защото инстанциите се дефинират чрез изброяване

@ulend

---

```java
public enum Day {
    SUNDAY,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
}
```

---

#### Enum

@ul

- Всеки enum неявно наследява абстрактния клас `java.lang.Enum`
  - не може да наследява явно друг клас, защото би било множествено наследяване
  - може да имплементира интерфейси
- Тялото на enum класа може да съдържа член-променливи и методи
- Ако има конструктор, той трябва да е package-private или private
- Той автоматично създава константите в дефиницията на enum-a. Не може да се извиква явно

@ulend

---

```java
public class EnumExample {
    private Day day;
    public EnumExample(Day day) { this.day = day; }
    public void tellItLikeItIs() {
        String message = switch (day) {
            case MONDAY -> "Mondays are bad.";
            case FRIDAY -> "Fridays are better.";
            case SATURDAY, SUNDAY -> "Weekends are best.";
            default -> "Midweek days are so-so.";
        };
        System.out.println(message);
    }
    public static void main(String[] args) {
        EnumExample example = new EnumExample(Day.TUESDAY);
        example.tellItLikeItIs();
    }
}
```

---

#### Enum

@ul

- Компилаторът добавя автоматично и няколко специални статични методи:
  - `values()` - връща масив, съдържащ всички стойности в enum, в реда, в който са изброени в декларацията
  - `valueОf(String name)` - връща enum константата по даденото име

@ulend

---

#### Сорс файлове на абстрактни класове, интерфейси и enums

@ul

- Важат същите правила като при конкретните класове:
  - абстрактните класове, интерфейсите и enum-ите се записват във файл
    - с име, съвпадащо с името на абстрактния клас/интерфейс/enum
    - с разширение .java

@ulend

---

#### Именуване: конвенции и добри практики

@ul

- Имената на пакети, класове, интерфейси, член-променливи, методи, локални променливи трябва да са говорещи и да спазват установените конвенции

@ulend

---

#### Именуване: конвенции и добри практики

@ul

- пакети
  - само малки букви, със смислена йерархия
  - `bg.sofia.uni.fmi.mjt`
- класове, абстрактни класове, интерфейси, enums
  - съществителни, започващи с главна буква (upper camel case)
  - `Student`, `GameBoard`

@ulend

---

#### Именуване: конвенции и добри практики

@ul

- методи
  - глаголи, започващи с малка буква (camel case)
  - `reverseString()`, `calculateSalary()`
- променливи
  - започващи с малка буква (camel case)
- константи
  - all-caps, с подчертавки между думите
  - `MAX_NAME_LENGTH`

@ulend

---

### Изключения

---

#### Изключение (Exception)

@ul

- Събитие (проблем), което се случва по време на изпълнение на дадена програма и нарушава нормалната последователност на изпълнение на инструкциите ѝ
- Още един начин за комуникация на метод с извикващите го: връщана стойност при нормално изпълнение и изключение при проблем

@ulend

---

#### Например...

@ul

- Подали сме невалидни входни данни
- Опитваме се да отворим несъществуващ файл
- Мрежата се е разкачила по време на комуникация
- Свършила е паметта на виртуалната машина
- ...

@ulend

---

#### Как се генерира ("хвърля") изключение?

```java
public Object pop() {
    if (size == 0) {
        throw new EmptyStackException();
    }

    // [...]
}
```

---

#### Как се обработва ("лови") изключение?

```java
try {
    // код, който може да хвърли изключение
} catch (Exception e) {
    // обработваме изключението ("exception handler")
    // може да има повече от един catch блок
} finally {
    // при нужда, някакви заключителни операции
    // finally блокът е optional, но ако го има,
    // се изпълнява задължително щом влезем в try-a
}
```

---

#### Catch block chain

```java
try { 
    // [...]
} catch (MostSpecificException mse) {
    // [...]
} catch (MoreGeneralException mge) {
    // [...]
} ... {
    // [...]
} catch (LeastSpecificException lse) {
    // [...]
}
```

---

#### Multi catch block

```java
try {
    // [...]
} catch (IOException | SQLException e) {
    // [...]
}
```

---

#### Стек на извикванията (call stack)

![Call stack](images/03.1-call-stack.jpg)

---

## Въпроси

@snap[south span-100]
@fab[github] [fmi/java-course](https://github.com/fmi/java-course)
@snapend
