> [๐ก#](#casting) ์์บ์คํ๊ณผ ๋ค์ด์บ์คํ์ ์ฐจ์ด์ ๋ํด ์ค๋ชํด์ฃผ์ธ์.
> - Upcasting์ด๋ ๋ถ๋ชจ ํด๋์ค๋ฅผ ์์๋ฐ์ ์์ ํด๋์ค์ ๊ฐ์ฒด๋ฅผ ๋ถ๋ชจ ํด๋์ค ํ์ ๋ ํผ๋ฐ์ค ๋ณ์๊ฐ ๊ฐ๋ฆฌํค๋๋ก ํ๋ ๊ฒ์ ์๋ฏธํฉ๋๋ค. ์ด ๋, ์์ ํด๋์ค์ ํ๋์๋ ์ ๊ทผํ  ์ ์์ต๋๋ค. 
> - Downcasting์ด๋ Upcasting๋ ๊ฐ์ฒด๋ฅผ ๋ค์ ์์ ํด๋์ค ํ์์ ๋ ํผ๋ฐ์ค ๋ณ์๊ฐ ๊ฐ๋ฆฌํค๋๋ก ํ๋ ๊ฒ์ ์๋ฏธํฉ๋๋ค. ์ด ๋ ๋ช์์  ํ๋ณํ์ ํด์ฃผ์ด์ผ ํ๋ฉฐ, ์์ ํด๋์ค์ ๋ชจ๋  ํ๋์ ์ ๊ทผ ๊ฐ๋ฅํฉ๋๋ค.
> 
> [๐ก#](#string_pool) Stringํ ๊ฐ์ฒด๋ฅผ ""๋ก ๋ง๋ค์์ ๋์ new ํค์๋๋ฅผ ์ด์ฉํด์ ๋ง๋ค์์ ๋์ ์ฐจ์ด์ ์? (String Pool)
> - ํฐ๋ฐ์ดํ๋ฅผ ์ด์ฉํด์ String ๊ฐ์ฒด๋ฅผ ๋ง๋ค์์ ๋ ์๋ String Pool์ ์ ์ฅ๋ฉ๋๋ค. ์ด ๋, ์ด๋ฏธ ๋ฑ๋ก๋์๋ String์ธ์ง ํ์ธํ๋ ๊ณผ์ ์ ๊ฑฐ์นฉ๋๋ค. new ํค์๋๋ฅผ ์ด์ฉํ์ ๋ ์๋ String Pool์ด ์๋ Heap์ String ๊ฐ์ฒด๊ฐ ์ ์ฅ๋๋ฉฐ, ๊ฐ์ String ๊ฐ์ ๊ฐ์ง๊ณ  ์๋๋ผ๋ ๋ค๋ฅธ ๊ฐ์ฒด๋ก ์์ฑ๋ฉ๋๋ค.
>
> [๐ก#](#wrapper_class) Wrapper Class์ ๋ํด ์ค๋ชํด์ฃผ์ธ์. + ์คํ  ๋ฐ์ฑ๊ณผ ์ธ๋ฐ์ฑ
> - java์ primitive data type๊ณผ ๋์๋๋ Collection Class๋ฅผ wrapper class๋ผ๊ณ  ํฉ๋๋ค.
> - primitive data๋ฅผ wrapper class ์ธ์คํด์ค๋ก ๋ณํํ๋ ์์์ boxing, ๊ทธ ๋ฐ๋๋ฅผ unboxing์ด๋ผ๊ณ  ํฉ๋๋ค.

# Casting

: Type ๋ณํ์ ๋งํ๋ค. Java์ Reference Data Type์ ๋ณํ์๋ **Upcasting, Downcasting**์ด ์กด์ฌํ๋ค. ๊ธฐ๋ณธ์ ์ผ๋ก ๊ด๋ จ์ด ์๋ ์ฐธ์กฐํ ๋ฐ์ดํฐ ํ์๋ผ๋ฆฌ๋ ๋ณํ์ด ๋ถ๊ฐ๋ฅํ๋ค. ์์ ๊ด๊ณ(extends)๋ฅผ ๊ฐ์ง๊ฑฐ๋, ๊ตฌํ ๊ด๊ณ(implements)๋ฅผ ๊ฐ์ก์ ๋ Casting์ด ๊ฐ๋ฅํ๋ค.

<br>

    Super Class
        โฒ
        |
     Sub Class

<br>

## Upcasting

Super Class๋ฅผ ์์๋ฐ์ ํด๋์ค๋ฅผ Sub Class๋ผ๊ณ  ํ  ๋, **Upcasting์ Super Class ํ ๋ ํผ๋ฐ์ค ๋ณ์์ Sub Class ๊ฐ์ฒด๋ฅผ ์ ์ฅํ๋ ๊ฒ์ ๋งํ๋ค.** Sub Class๋ Super Class์ ๋นํด ํญ์ ๊ฐ๊ฑฐ๋ ๋ ๋ง์ ํ๋๋ฅผ ๊ฐ์ง๋ค.

<br>

    Class Item {
      String name;
      Item(String name) {
        this.name = name;
      }
    }

    Class Book {
      String isbn;
      Book(String name, String isbn) {
        super(name);
        this.isbn = isbn;
      }
    }

    ...
    public static void main(String[] args){
      Item item = new Book("์ฑ์๋๋ค", "a1234");

      System.out.println(item.name);  //์ฑ์๋๋ค
      System.out.println(item.isbn);  //Compile Error
    }

Sub Classํ ๊ฐ์ฒด๊ฐ ์์ฑ๋์ด ํ์ ์ ์ฅ๋๊ณ , Super Class ํ์์ ๋ ํผ๋ฐ์ค ๋ณ์๊ฐ ํด๋น ๊ฐ์ฒด๋ฅผ ๊ฐ๋ฆฌํจ๋ค. Super Class ํ์์ ๋ณ์๋ Sub Class์์ ์๋กญ๊ฒ ์ ์๋ ํ๋์ ์ ๊ทผํ  ์ ์๊ธฐ ๋๋ฌธ์ ์์ ์ฝ๋์ ๋ง์ง๋ง ์ค์ ์ปดํ์ผ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค.

<br>

## Downcasting

DownCasting์ Upcasting์ ๋ฐ๋ ์ฉ์ด๋ก, **Super Classํ์ผ๋ก Upcasting๋์๋ Sub Class ํ์ ๊ฐ์ฒด๋ฅผ ๋ค์ Sub Classํ์ ๋ ํผ๋ฐ์ค ๋ณ์๊ฐ ๊ฐ๋ฆฌํค๊ฒ ๋ง๋๋ ๊ฒ**์ ์๋ฏธํ๋ค.
์ด ๋, Upcasting์ ๋ฐ๋๋ผ๊ณ  ํด์ Super Classํ ๊ฐ์ฒด๋ฅผ ์์ฑํด์ Sub Class ํ์ ๋ ํผ๋ฐ์ค ๋ณ์์ ์ ์ฅํ๋ฉด ์ ๋๋ค. ์์์ ๊ฐ์กฐํ ๊ฒ ์ฒ๋ผ, Sub Class๋ ํญ์ Super Class๋ณด๋ค ๊ฐ๊ฑฐ๋ ๋ง์ ํ๋๋ฅผ ๊ฐ์ง๊ธฐ ๋๋ฌธ์ด๋ค.

    ...
    public static void main(String[] args){
      Item item = new Book("์ฑ์๋๋ค", "a1234");
      Book book = (Book)item;

      System.out.println(book.name);  //์ฑ์๋๋ค
      System.out.println(book.isbn);  //a1234
    }

๋ช์์  ํ๋ณํ์ ํด์ฃผ์ง ์์๋ Upcasting์ ํด์ค ์ ์์ง๋ง, Downcasting์ ๋ฐ๋์ ๋ช์์ ์ผ๋ก ํ๋ณํ์ ํด์ฃผ์ด์ผ ํ๋ค.
๋ํ Upcasting ๋ ๋ณ์์ ๋ํด์๋ง ์งํํ  ์ ์๋ค.

# String Pool

Java String API(java.lang.String)๋ Java์์ Primitive Data Type๊ณผ Reference Data Type์ ์ค๊ฐ ์ฑ๊ฒฉ์ ๋๋, ํน๋ณํ ํด๋์ค์ด๋ค.

String์ value๋ immutable ํน์ฑ์ ๊ฐ์ง๋ค. ์ฆ, ์๋ก ๋ค๋ฅธ ๊ฐ์ฒด์ ํ ๋น๋๋ ๊ฐ์ String ๊ฐ์ฒด์ ๋ํด ๋ณ๋์ ๊ณต๊ฐ์ ์ง์ ํ์ง ์์๋ ๋๋ค๋ ๊ฒ ์ด๋ค.

## String Interning
๋ฐ๋ผ์ JVM์ Java String Pool์ด๋ผ๋ ํน๋ณํ ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ์ ๋์ด String ๋ฌธ์์ด์ ๊ด๋ฆฌํ๋ค

String Pool์ด๋ผ๋ ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ์ ์ค๋ณต๋์ง ์๋ String ๋ฌธ์์ด์ ์ ์ฅํ๊ณ , ํด๋น ๋ฌธ์์ด์ ์ ๊ทผํ  ๋ ๋ง๋ค ๋์ผํ ๋ฉ๋ชจ๋ฆฌ ์ฃผ์๋ฅผ ๋ฐํํ๋ฉด ํ ๋นํด์ผ ํ๋ ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ์ ์ ์ฝํ  ์ ์๋ค.

์ด๋ฌํ ํ๋ก์ธ์ค๋ฅผ interning์ด๋ผ๊ณ  ํ๋ค.

 

์ค์ ๋ก ์ฐ๋ฆฌ๊ฐ String ๋ณ์๋ฅผ ์์ฑํ๊ณ  ๊ฐ์ ํ ๋นํ๋ฉด JVM์ String pool์์ ๋์ผํ value๋ฅผ ๊ฐ์ง String์ด ์๋์ง ํ์ํ๋ค

๐ ์ด๋ฏธ ์กด์ฌํ๋ค๋ฉด, ์๋ก์ด ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ์ ํ ๋นํ๋ ๊ฒ์ด ์๋ ํด๋น String์ reference๋ฅผ ๋ฐํํ๋ค


 
๐ ์กด์ฌํ์ง ์๋๋ค๋ฉด, pool์ ์๋ก์ด String์ด ํ ๋น๋๊ณ  ํด๋น reference๋ฅผ ๋ฐํํ๋ค

    String constantString1 = "2jin";
    String constantString2 = "2jin";

    assertThat(constantString1)
      .isSameAs(constantString2);

## ์์ฑ์๋ฅผ ํตํด ํ ๋น๋ String ๊ฐ์ฒด
""(ํฐ๋ฐ์ดํ)๋ฅผ ์ด์ฉํด ํ ๋นํ์ ๋์๋ ๋ค๋ฅด๊ฒ, new ์ฐ์ฐ์๋ฅผ ์ด์ฉํ String ๊ฐ์ฒด ์์ฑ์ String Pool์ ์ ์ฅ๋์ง ์๊ณ  Java ์ปดํ์ผ๋ฌ๊ฐ ์๋ก์ด ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ Heap์ ํ ๋นํ๋ค.

๋๊ฐ์ String value๋ฅผ ๊ฐ์ง ๊ฐ์ฒด๋ฅผ ์ฌ๋ฌ ๊ฐ ์์ฑํด๋, String pool๋ก ๊ด๋ฆฌํ  ๋์ ๋ฌ๋ฆฌ ๋ณ๋์ ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ์ ๊ณ์ํด์ ํ ๋นํ๋ ๊ฒ์ด๋ค.

    String constantString = "2jin";
    String newString = new String("2jin");
    
    assertThat(constantString).isNotSameAs(newString);

## String Literal ๐ String Object
new ํค์๋๋ฅผ ์ฌ์ฉํ String Object๊ฐ ์์ฑ๋  ๋, Heap ๋ฉ๋ชจ๋ฆฌ์๋ ํญ์ ์๋ก์ด Object๊ฐ ํ ๋น๋๋ค.

๋ฐ๋ฉด ํฐ๋ฐ์ดํ๋ฅผ ์ด์ฉํ String Literal์ ์์ฑํ  ๋ ์๋, String Pool์ ์กด์ฌํ๋ ๊ธฐ์กด์ ๊ฐ์ฒด๋ฅผ ๋ฐํํ๊ณ  ์์ ์ ์๋ก์ด ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ ์ฌ์ฌ์ฉ์ ์ํด String Pool์ ์ ์ฅํ๋ค.

String Pool๊ณผ Heap ๋๋ค์ ์ ์ฅ๋๋ value๋ ๋๋ค String Object๋ก ๋์ผํ์ง๋ง, ๊ฐ์ฅ ํฐ ์ฐจ์ด์ ์ **new ํค์๋๋ฅผ ์ฌ์ฉํ  ์ ํญ์ ์๋ก์ด ๊ฐ์ฒด๊ฐ ์์ฑ๋๋ค**๋ ๊ฒ์ด๋ค.

![image](https://user-images.githubusercontent.com/30489264/131838576-fb5c6253-21c9-4e88-9915-733920fbdc45.png)

String Literal๊ณผ String Object์ ์ฐจ์ด์ ์ ์๋์ compare ์์ ๋ฅผ ํตํด ํ์ธํ  ์ ์๋ค

    String first = "2jin"; 
    String second = "2jin"; 
    System.out.println(first == second); // True

    String third = new String("2jin");
    String fourth = new String("2jin"); 
    System.out.println(third == fourth); // False

    String fifth = "2jin";
    String sixth = new String("2jin");
    System.out.println(fifth == sixth); // False

first์ second๋ฅผ ๋น๊ตํ์ ๋, ๋ ๋ค String Literal์ ์ฌ์ฉํ์ผ๋ฏ๋ก String Pool์ ๊ฐ์ ๊ฐ์ฒด๋ฅผ ์ฐธ์กฐํ๋ค.

๋ฐ๋ผ์ ํผ์ฐ์ฐ์์ identity๋ฅผ ๋น๊ตํ๋ == ์ฐ์ฐ์๋ฅผ ์ฌ์ฉํ์ ๋ True๋ฅผ ๋ฐํํ๋ค.
 
third์ fourth๋ฅผ ๋น๊ตํ์ ๋, ๋ ๋ค new ํค์๋๋ฅผ ์ฌ์ฉํ์ฌ String Object๋ฅผ ์๋กญ๊ฒ ์์ฑํ์ผ๋ฏ๋ก Heap์๋ ๋ ๊ฐ์ String Object๊ฐ ์์ฑ๋๊ณ  ๋น์ฐํ ๋ค๋ฅธ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ์ฐธ์กฐํ๊ณ  ์๋ค.

firth์ sixth๋ฅผ ๋น๊ตํ์ ๋ ์๋ firth๋ String Pool, sixth๋ Heap์ ์๋ ๊ฐ์ฒด๋ฅผ ์ฐธ์กฐํ๋ฏ๋ก ๋์ผํ์ง ์๋ค.

<br>

# Wrapper Class

Wrapper Class๋ primitive data type์ ์บก์ํํ๋ ํด๋์ค์ด๋ฉฐ, ๋ค๋ฅธ ํด๋์ค์ ๊ฐ์ฒด ์ธ์คํด์ค ๋ฐ ๋ฉ์๋๋ฅผ ๋ง๋ค๊ธฐ ์ํด ์ฌ์ฉํ๋ค.

Primitive Data Type์ Object์ฒ๋ผ ์ฌ์ฉํ๊ธฐ ์ํด Class๋ก ๋ฐ์ธ๋ฉ์ํค๋ ์ญํ ์ด๋ผ๊ณ  ํ  ์ ์๋ค.

1๏ธโฃ primitive type์ class์ generic์ผ๋ก ์ฌ์ฉํ  ์ ์๊ณ , 2๏ธโฃObject์ ํ์ ํด๋์ค๋ฅผ ์๊ตฌํ๋ ๋ฉ์๋๊ฐ ์์ ๋

์ผ๋ฐ primitive data type์ Wrapper class๋ก ๊ฐ์ธ์ ์ฌ์ฉํ๋ค.

์ผ๋จ primitive data type์ ์ด์ฉํ๋ฉด ์ผ์  ๋จ์์ ๋ฐ์ดํฐ ์ ์ฅ์ ์ํด Array๋ง์ ์ด์ฉํ  ์ ์์ง๋ง, ์์ ๋ฐ์ดํฐ ํ์์ wrapper class๋ก ๊ฐ์ธ๋ฉด java์ ๋ค์ํ collection์ ์ด์ฉํ  ์ ์๋ค.

๋ฐ๋ผ์ Wrapper Class๋ ์ฃผ๋ก Primitive Data Type์ ์ ์ฅํ  Java API์ Collection ํด๋์ค์ ๊ฐ์ฒด๋ฅผ ๋ง๋๋๋ฐ ์ฌ์ฉ๋๋ค.

<br>

|Primitive Data Type|Wrapper Class|
|-|-|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|
|char|Character|
|boolean|Boolean|

<br>

![image](https://user-images.githubusercontent.com/30489264/131839686-ee3027fc-8852-48ff-86ff-3e2a2b9d0892.png)

์ด ๋ primitive data๋ฅผ wrapper class ์ธ์คํด์ค๋ก ๋ณํํ๋ ์์์ Boxing, ๊ทธ ๋ฐ๋๋ฅผ Unboxing์ด๋ผ๊ณ  ํ๋ค.

JDK 1.5๋ฒ์  ๋ถํฐ๋ Java Compiler๊ฐ ๋ฐ์ฑ๊ณผ ์ธ๋ฐ์ฑ์ ์๋์ผ๋ก ์ฒ๋ฆฌํด์ฃผ๋ Auto Boxing, Auto Unboxing์ด ์ง์๋๋ค

    public class Wrapper02 {
    public static void main(String[] args) {
        Integer num1 = new Integer(7); // ๋ฐ์ฑ
        Integer num2 = new Integer(3); // ๋ฐ์ฑ

 
        int int1 = num1.intValue();    // ์ธ๋ฐ์ฑ
        int int2 = num2.intValue();    // ์ธ๋ฐ์ฑ
 
โ       Integer result1 = num1 + num2; // 10 
โก      Integer result2 = int1 - int2; // 4
โข      int result3 = num1 * int2;     // 21
    }
}

๋ฌผ๋ก  Wrapper Class์ Instance์ value๊ฐ ๊ฐ๋ค๊ณ  ํด์ (==)์ฐ์ฐ์์ ๊ฒฐ๊ณผ๊ฐ์ด true์ธ ๊ฒ์ ์๋๋ค(์ธ์คํด์ค์ ์ฃผ์ ๊ฐ์ ๋น๊ตํ๊ธฐ ๋๋ฌธ).

wrapper class์ instance์ ๋๋ฑ ๋น๊ต๋ฅผ ํ๊ธฐ ์ํด์๋ equals() ๋ฉ์๋๋ฅผ ์ฌ์ฉํด์ผ ํ๋ค.

์ด ๋ ๋ชํํ ๊ตฌ๋ถํด์ผ ํ๋ ๊ฒ์ Primitive Wrapper Class๊ฐ Primitive Type์์ ์๋ฏธํ์ง ์๋๋ค๋ ์  ์ด๋ค.

Wrapper Class๋ data type์ด ํ ๋น๋ ๋ณ์์๋ ๋ฌ๋ฆฌ, primitive data type์ ์์๋ฐ์๊ณผ ๋์์ ์บก์ํ๋ก ์จ๊ธฐ๋ฉด์ ์ธ์คํด์คํ๋ ๊ฐ์ฒด ๋ฐ ๋ฉ์๋๋ฅผ ์ ์ํ๋ค.

<br>

### โ Wrapper Class์ Boxing, Unboxing ์์ฉ

```
for (Integer i = 0; i < 10; i++ ){
	...
}
```

์์ ๊ฐ์ ์ฝ๋๊ฐ ๋์ํ๋์ง, ๋์ํ๋ค๋ฉด ์ด๋ค ๋ฐฉ์์ธ์ง ์ค๋ชํ๋ผ๋ ๋ฉด์  ์ง๋ฌธ์ ๋ค์์ต๋๋ค

> ์ฐ์  ์ฐ์ฐ์ ์ํ ํด๋์ค๊ฐ ์๋๊ธฐ ๋๋ฌธ์, ์ธ์คํด์ค์ ์ ์ฅ๋ ๊ฐ์ ๋ณ๊ฒฝํ  ์ ์๋ค.
> - ๊ฐ์ ๋ํ ๋ณ๊ฒฝ์ ๋ถ๊ฐ๋ฅํ์ง๋ง, ์๋ก์ด ๊ฐ(๊ฐ์ฒด)์ ํ ๋น์ด๋ ์ฐธ์กฐ๋ ๊ฐ๋ฅํ๋ค.

๋ผ๋ ์์ฐ๋์ ๋ต๋ณ์ ๋ณด๊ณ , ์์ ์ฝ๋์ ๋ํ ๋์์ ์๊ฐํด๋ดค์ต๋๋ค.

1. ์ ์ธ๋ถ <code>Integer i = 0</code>์์ 0์ด auto boxing๋์ด i์ ํ ๋น๋๋ค.
2. ์ฆ๊ฐ๋ถ <code>i++</code>์ ๋ง๋  ๋, i์์ ๋ด๊ฒจ์๋ ๊ฐ์ ๊บผ๋ด์ ++ (primitive data type์๋ง ๊ฐ๋ฅํ๋ฏ๋ก unboxing)ํ ๋ค i์ ๋ค์ ๋ฃ๋๋ค.
3. ์ด ๋, ์ ์ธ๋ถ์์ ์์ฑํ ๊ฐ์ฒด์ value ๊ฐ์ด ๋ณ๊ฒฝ๋๋ ๊ฒ์ด ์๋, ์๋ก์ด ๊ฐ(๊ฐ์ฒด)๊ฐ ํ ๋น๋์ด i์ ๋ค์ด๊ฐ๋ ๊ฒ ์ด๋ค.


```
Integer k = 0;
for (Integer i = k; i < 3; i++ ){
	System.out.println(i);
}
System.out.println(k);

// ์ถ๋ ฅ ๊ฒฐ๊ณผ
// 0
// 1
// 2
// 3
// 0
```

๋ฐ๋ผ์ ์์ ์ฝ๋๋ k์ ๊ฐ์ ๋ณํจ์ด ์๋ค.

<br>

# ์ฐธ๊ณ ์๋ฃ

> https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dlaxodud2388&logNo=221642221204
> www.tcpschool.com/java/java_api_wrapper
> en.wikipedia.org/wiki/Primitive_wrapper_class_in_Java
> docs.oracle.com/javase/7/docs/api/java/lang/Integer.html