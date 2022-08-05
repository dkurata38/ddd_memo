# Stream API

## ここで説明すること

`Collection#stream`メソッドとその呼び出しによって戻る`Stream<T>`型のデータに対して行える一連の操作群について話します。

```java
List<Integer> integerList = Arrays.asList(1, 2, 3, 4);
List<Integer> doubledList = integerList.stream()
  .map(e -> e * 2)
  .collect(Collectors.toList());
```

Stream APIで定義された汎用メソッドと具体的な処理を定義した関数型のインスタンスの組み合わせにより、コレクションに対するさまざまな操作を行える。

## コレクションの要素を変換し、別のコレクションを作る

### 数値のリスト`1, 2, 3, 4`の要素を1つずつ2倍した`doubledList`を作る。

```java
List<Integer> integerList = List.of(1, 2, 3, 4);
List<Integer> doubledList = integerList.stream()
  .map(e -> e * 2)
  .collect(Collectors.toList());
```

### Stream APIの基本的な使い方

1. 始端操作によりコレクションからStream型のインスタンスを生成する。
2. 中間操作によりStream型のインスタンスを操作する。
3. 終端操作によりStreamを閉じ、コレクションなどを戻り値として返す。

### 始端操作

`Collection#stream()`メソッドや`Collection#parallelstream()`メソッドにより、コレクションからStream型のインスタンスを生成できる。

### 中間操作（値の変換）

1. `Stream<R> map(Function<T, R> f)`

集合を別の集合に変換する。
HogeからFugaに変換するときは引数がHogeで戻り値がFugaになるような関数オブジェクトを引数にする。
型推論により、map関数の戻り値は`Stream<Fuga>`となる。
ちなみにmapでプリミティブ型、例えばintを戻す関数を引数にしても、オートボクシングされて`Stream<Integer>`が戻る。

2. `Stream<R> flatMap(Function<T, Stream<R>> f)`

集合を別の集合に変換する。
関数合成。引数にはStreamを返却する関数オブジェクトを指定する。
hogeList.stream().flatMap(hoge -> hoge.getFugaList().stream())
とすると、hoge.getFugaList()で得られた集合同士が合成される。Stream<Hoge>からStream<Fuga>への変換ができる。

### 終端操作



## コレクションのうち特定の要素を取り除いた別のコレクションを作る

数値の



## コレクションの種類

コレクションにはいくつかの種類がある。目的ごとに使い分けができるようになっておきたい。

主な種類は以下の通り。

|  | 性質 | Javaにおけるインターフェース | 
| --- | --- | --- |
| セット | 重複要素のないコレクション | java.util.Setなど |
| リスト | 順序づけられたコレクション | java.util.Listなど |
| マップ | キーと値の組み合わせ。 | java.util.Mapなど |

表を見てわかるが、セットやマップには順序が保証されていないので、並び替えをする意味がない。

## 関数

### 関数とは

引数を受け取り、受け取った引数をもとに処理を行う。一部例外があるが、以下のような性質がある。

+ 同じ引数に対して、必ず同じ戻り値を持つ。
+ 引数以外の入力、戻り値以外の出力を持たない。

Javaにおいてはstaticなメソッドに近い。

関数自体をインスタンスとして扱えるようにするのが関数インターフェースである。

### 関数インターフェースの種類

| インターフェース | 概要 |
| --- | --- |
| java.util.Function<T, R> | T型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.UnaryOperator<T> | T型の引数を受け取り、T型の戻り値を返却する関数。 |
| java.util.Supplier<T> | 引数なしで、T型の戻り値を返却する関数。 |
| java.util.Predicate<T> | 引数なしで、boolan型の戻り値を返却する関数。 |
| java.util.Consumer<T> | T型の引数を受け取り、戻り値を返却しない関数。 |
| java.util.BiFunction<T,U,R> | T型の引数、U型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.BinaryOperator<T,R> | 2つのT型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.BiConsumer<T,U> | T型の引数、U型の引数を受け取り、戻り値を返却しない関数。 |

### 関数型インスタンスの生成方法



## StreamAPIの種類

### 中間操作

#### 値の変換

1. `Stream<R> map(Function<T, R> f)`

集合を別の集合に変換する。
HogeからFugaに変換するときは引数がHogeで戻り値がFugaになるような関数オブジェクトを引数にする。
型推論により、map関数の戻り値は`Stream<Fuga>`となる。
ちなみにmapでプリミティブ型、例えばintを戻す関数を引数にしても、オートボクシングされて`Stream<Integer>`が戻る。

2. `XxxStream mapToXXX(ToXxxFunction<T> f)`

集合をプリミティブ型の集合に変換する。
mapToInt, mapToDoubleなどのメソッドがあり、それぞれIntStreamやDoubleStreamなどを戻す。
特定のプリミティブ型に対する終端操作を利用できるのが便利である。

例：

3. `Stream<R> flatMap(Function<T, Stream<R>> f)`

集合を別の集合に変換する。
関数合成。引数にはStreamを返却する関数オブジェクトを指定する。
hogeList.stream().flatMap(hoge -> hoge.getFugaList().stream())
とすると、hoge.getFugaList()で得られた集合同士が合成される。Stream<Hoge>からStream<Fuga>への変換ができる。

4. `Stream<T> boxed()`

IntStreamなどプリミティブ値を扱うStreamをオートボクシングされた型のStreamに変換する。

```java
IntStream.rangeClosed(1, 7) // <- IntStream
  .boxed() // <- Stream<Integer>
  .map(dayOfMonth -> LocalDate.of(2019, 1, dayOfMonth)) // <- Stream<LocalDate>
```

#### 絞り込み`Stream<T> filter(Predicate<T> p)`

引数の関数がtrueを戻す要素のみに絞り込みをする

#### 並び替え`Stream<T> sorted(Comparator<T> c)`

Comparator型のインスタンスを引数にとる。

##### Comparator型のインスタンス生成方法。

1. Comparatorインターフェースを実装したクラスを作り、インスタンスを生成する。

実装クラスは`int compare(T o1, T o2)`メソッドを実装する必要がある。
2値が等しい場合は値0、o1がo2より小さい場合は0より小さい値、o1がo2より大きい場合は0より大きい値を返す。
どの場合に何を返却するか間違えやすいので、並び替えキーの型に定義されている`compare`関数や`compareTo`メソッドの戻り値を流用するのが無難である。

```java
public class HogeComparator implements Comparator<Hoge> {
  public int compare(Hoge o1, Hoge o2) {
  	return o1.getId().compareTo(o2.getId());
  }
}

public class Main {
  public static void main(String args[]) {
    List<Hoge> hoges = Stream.of(new Hoge(2), new Hoge(1))
      .sorted(new HogeComparator())
      .collect(Collectors.toList());
  }
}
```

2. 無名クラスを使ってインスタンスを生成する。

無名クラスというのはクラス宣言をせずに、クラスやインターフェースを継承したクラスのインスタンスを生成するテクニックである。
1箇所でしか使わないならこの方法でも問題ない。

```java
public class Main {
  public static void main(String args[]) {
    List<Hoge> hoges = Stream.of(new Hoge(2), new Hoge(1))
      .sorted(new Comparetor() {
      	public int compare(Hoge o1, Hoge o2) {
          return o1.getId().compareTo(o2.getId())
      	}
      })
      .collect(Collectors.toList());
  }
}
```

3. Comparetorクラスの関数を使ってインスタンスを生成する。

後で説明する。

##### Comparatorクラスの関数

1. `nullsFirst(Comparator<T> c)`/`nullsLast(Comparator<T> c)`

NullableなインスタンスのStreamに対して、

2. `naturalOrder()`/`reverseOrder()`

Comparableインターフェースを実装したクラスのStreamであれば使える。
Comparableインターフェースの実装クラスに対して昇順か降順かを選択する。


| 関数名 | 意味 |
| `nullsLast()/nullsFirst | nullの要素を前にするか後にするかを選択する |
| naturalOrder/reverseOrder | Comparableインターフェースの実装クラスに対して昇順か降順かを選択する |
| comparing | |
| nullsLast/nullsFirst | |
A. nullsLast/nullsFirst
B. naturalOrder/reverseOrder
C. comparing
D. then





---

# Stream API

## はじめに

## 関数

### サンプルコード

数値を文字列に変換する関数のインスタンスを生成する。

```java
Function<Integer, String> anonymousClass = new Function() {
  public String apply(Integer integer) {
  	return integer.toString();
  }
}
Function<Integer, String> lambda = (Integer i) -> { return i.toString(); }
Function<Integer, String> simpleLambda = i -> i.toString();
```

### 関数とは

引数を受け取り、受け取った引数をもとに処理を行う。一部例外があるが、以下のような性質がある。

+ 同じ引数に対して、必ず同じ戻り値を持つ。
+ 引数以外の入力、戻り値以外の出力を持たない。

Javaにおいてはstaticメソッドに近い。

関数自体をインスタンスとして扱えるようにするのが関数インターフェースである。

### 関数型インスタンスの生成方法

1. 匿名クラスを使う

後述するラムダ式と比べると記述量が多いので、基本的にはおすすめしない。

```java
Function<Integer, String> anonymousClass = new Function() {
  public String apply(Integer integer) {
    return integer.toString();
  }
}
```

2. ラムダ式を使う

以下は最も冗長に記述をしたラムダ式の例。前に説明した匿名クラスのうち、引数とメソッドボディを記述する。

```java
Function<Integer, String> lambda = (Integer i) -> { return i.toString(); }
```

上記のうち下記のルールに従って、よりシンプルに記述することができる。

1. 引数の型は省略することができる。
2. 引数が一つの場合は、丸括弧を省略できる。
3. メソッドボディが1行の場合は、波括弧とメソッドボディ内の`return`とセミコロンを省略できる。

シンプルに記述した例が以下である。

```java
Function<Integer, String> simpleLambda = i -> i.toString();
```

## さまざまな関数

### サンプルコード

```java
// 数値を文字列に変換した結果を戻す
Function<Integer, String> toStringFunction = i -> i.toString();
// 数値を2乗した結果を戻す
UnaryOperator<Integer> squareOperator = i -> i * i;
// 0を戻す
Supplier<Integer> zeroSupplier = () -> 0;
// 数値が負の数かどうかを戻す
Predicate<Integer> isNegativePredicate = i -> i < 0;
// 引数の数値を標準出力する
Consumer<Integer> intPrinter = i -> System.out.println(i);
// 2つの数値をカンマで連結した文字列を戻す
BiFunction<Integer, Integer, String> intCommaConcatnator = (i1 ,i2) -> i1 + "," + i2;
// 2つの数値を足した結果を戻す
BinaryOperator<Integer> sumBinaryOperator = (i1, i2) -> i1 + i2;
// 2つの数値をカンマで連結した文字列を標準出力する
BiConsumer<Integer, Integer> joinedIntPrinter = (i1, i2) -> System.out.println(i1 + "," + i2);
```

### 関数インターフェースの種類

関数インターフェースには`java.util.Function`を基本的なインターフェースとして、用途に合わせて様々なインターフェースが用意されている。

インターフェースと意味を理解しておくと、ライブラリのドキュメントを素早く理解できる助けになることもある。

| インターフェース | 概要 |
| --- | --- |
| Function<T, R> | T型の引数を受け取り、R型の戻り値を返却する関数。 |
| UnaryOperator<T> | T型の引数を受け取り、T型の戻り値を返却する関数。 |
| Supplier<T> | 引数なしで、T型の戻り値を返却する関数。 |
| Predicate<T> | 引数なしで、boolan型の戻り値を返却する関数。 |
| Consumer<T> | T型の引数を受け取り、戻り値を返却しない関数。 |
| BiFunction<T,U,R> | T型の引数、U型の引数を受け取り、R型の戻り値を返却する関数。 |
| BinaryOperator<T> | 2つのT型の引数を受け取り、T型の戻り値を返却する関数。 |
| BiConsumer<T,U> | T型の引数、U型の引数を受け取り、戻り値を返却しない関数。 |

## Stream APIでリストを別の型のリストに変換する

### サンプルコード

リストの要素をそれぞれ2倍したリストを生成する。

```java
List<Integer> integerList = Arrays.asList(1, 2, 3, 4);
List<Integer> doubledList = integerList.stream()
  .map(e -> e * 2)
  .collect(Collectors.toList());
```

### Stream APIを使うおおまかな流れ

## Stream APIでリストから特定の要素を取り除く

Stream APIでリストを集合に変換する

Stream APIでリストをマップに変換する

Stream APIでリストを集計する


---


# Stream API

## ここで説明すること

`Collection#stream`メソッドとその呼び出しによって戻る`Stream<T>`型のデータに対して行える一連の操作群について話します。

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4);
List<Integer> doubledList = list.stream()
  .map(e -> e * 2)
  .collect(Collectors.toList());
```

Stream APIで定義された汎用メソッドと具体的な処理を定義した関数型のインスタンスの組み合わせにより、コレクションに対するさまざまな操作を行える。

## コレクションの要素を変換し、別のコレクションを作る



## コレクションの種類

コレクションにはいくつかの種類がある。目的ごとに使い分けができるようになっておきたい。

主な種類は以下の通り。

|  | 性質 | Javaにおけるインターフェース | 
| --- | --- | --- |
| セット | 重複要素のないコレクション | java.util.Setなど |
| リスト | 順序づけられたコレクション | java.util.Listなど |
| マップ | キーと値の組み合わせ。 | java.util.Mapなど |

表を見てわかるが、セットやマップには順序が保証されていないので、並び替えをする意味がない。

## 関数

### 関数とは

引数を受け取り、受け取った引数をもとに処理を行う。一部例外があるが、以下のような性質がある。

+ 同じ引数に対して、必ず同じ戻り値を持つ。
+ 引数以外の入力、戻り値以外の出力を持たない。

Javaにおいてはstaticなメソッドに近い。

関数自体をインスタンスとして扱えるようにするのが関数インターフェースである。

### 関数インターフェースの種類

| インターフェース | 概要 |
| --- | --- |
| java.util.Function<T, R> | T型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.UnaryOperator<T> | T型の引数を受け取り、T型の戻り値を返却する関数。 |
| java.util.Supplier<T> | 引数なしで、T型の戻り値を返却する関数。 |
| java.util.Predicate<T> | 引数なしで、boolan型の戻り値を返却する関数。 |
| java.util.Consumer<T> | T型の引数を受け取り、戻り値を返却しない関数。 |
| java.util.BiFunction<T,U,R> | T型の引数、U型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.BinaryOperator<T,R> | 2つのT型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.BiConsumer<T,U> | T型の引数、U型の引数を受け取り、戻り値を返却しない関数。 |

### 関数型インスタンスの生成方法



## StreamAPIの種類

### 中間操作

#### 値の変換

1. `Stream<R> map(Function<T, R> f)`

集合を別の集合に変換する。
HogeからFugaに変換するときは引数がHogeで戻り値がFugaになるような関数オブジェクトを引数にする。
型推論により、map関数の戻り値は`Stream<Fuga>`となる。
ちなみにmapでプリミティブ型、例えばintを戻す関数を引数にしても、オートボクシングされて`Stream<Integer>`が戻る。

2. `XxxStream mapToXXX(ToXxxFunction<T> f)`

集合をプリミティブ型の集合に変換する。
mapToInt, mapToDoubleなどのメソッドがあり、それぞれIntStreamやDoubleStreamなどを戻す。
特定のプリミティブ型に対する終端操作を利用できるのが便利である。

例：

3. `Stream<R> flatMap(Function<T, Stream<R>> f)`

集合を別の集合に変換する。
関数合成。引数にはStreamを返却する関数オブジェクトを指定する。
hogeList.stream().flatMap(hoge -> hoge.getFugaList().stream())
とすると、hoge.getFugaList()で得られた集合同士が合成される。Stream<Hoge>からStream<Fuga>への変換ができる。

4. `Stream<T> boxed()`

IntStreamなどプリミティブ値を扱うStreamをオートボクシングされた型のStreamに変換する。

```java
IntStream.rangeClosed(1, 7) // <- IntStream
  .boxed() // <- Stream<Integer>
  .map(dayOfMonth -> LocalDate.of(2019, 1, dayOfMonth)) // <- Stream<LocalDate>
```

#### 絞り込み`Stream<T> filter(Predicate<T> p)`

引数の関数がtrueを戻す要素のみに絞り込みをする

#### 並び替え`Stream<T> sorted(Comparator<T> c)`

Comparator型のインスタンスを引数にとる。

##### Comparator型のインスタンス生成方法。

1. Comparatorインターフェースを実装したクラスを作り、インスタンスを生成する。

実装クラスは`int compare(T o1, T o2)`メソッドを実装する必要がある。
2値が等しい場合は値0、o1がo2より小さい場合は0より小さい値、o1がo2より大きい場合は0より大きい値を返す。
どの場合に何を返却するか間違えやすいので、並び替えキーの型に定義されている`compare`関数や`compareTo`メソッドの戻り値を流用するのが無難である。

```java
public class HogeComparator implements Comparator<Hoge> {
  public int compare(Hoge o1, Hoge o2) {
  	return o1.getId().compareTo(o2.getId());
  }
}

public class Main {
  public static void main(String args[]) {
    List<Hoge> hoges = Stream.of(new Hoge(2), new Hoge(1))
      .sorted(new HogeComparator())
      .collect(Collectors.toList());
  }
}
```

2. 無名クラスを使ってインスタンスを生成する。

無名クラスというのはクラス宣言をせずに、クラスやインターフェースを継承したクラスのインスタンスを生成するテクニックである。
1箇所でしか使わないならこの方法でも問題ない。

```java
public class Main {
  public static void main(String args[]) {
    List<Hoge> hoges = Stream.of(new Hoge(2), new Hoge(1))
      .sorted(new Comparetor() {
      	public int compare(Hoge o1, Hoge o2) {
          return o1.getId().compareTo(o2.getId())
      	}
      })
      .collect(Collectors.toList());
  }
}
```

3. Comparetorクラスの関数を使ってインスタンスを生成する。

後で説明する。

##### Comparatorクラスの関数

1. `nullsFirst(Comparator<T> c)`/`nullsLast(Comparator<T> c)`

NullableなインスタンスのStreamに対して、

2. `naturalOrder()`/`reverseOrder()`

Comparableインターフェースを実装したクラスのStreamであれば使える。
Comparableインターフェースの実装クラスに対して昇順か降順かを選択する。


| 関数名 | 意味 |
| `nullsLast()/nullsFirst | nullの要素を前にするか後にするかを選択する |
| naturalOrder/reverseOrder | Comparableインターフェースの実装クラスに対して昇順か降順かを選択する |
| comparing | |
| nullsLast/nullsFirst | |
A. nullsLast/nullsFirst
B. naturalOrder/reverseOrder
C. comparing
D. then

---
# Stream API

## はじめに

## 関数

### サンプルコード

```java


```

### 関数とは

引数を受け取り、受け取った引数をもとに処理を行う。一部例外があるが、以下のような性質がある。

+ 同じ引数に対して、必ず同じ戻り値を持つ。
+ 引数以外の入力、戻り値以外の出力を持たない。

Javaにおいてはstaticなメソッドに近い。

関数自体をインスタンスとして扱えるようにするのが関数インターフェースである。

### 関数インターフェースの種類

| インターフェース | 概要 |
| --- | --- |
| java.util.Function<T, R> | T型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.UnaryOperator<T> | T型の引数を受け取り、T型の戻り値を返却する関数。 |
| java.util.Supplier<T> | 引数なしで、T型の戻り値を返却する関数。 |
| java.util.Predicate<T> | 引数なしで、boolan型の戻り値を返却する関数。 |
| java.util.Consumer<T> | T型の引数を受け取り、戻り値を返却しない関数。 |
| java.util.BiFunction<T,U,R> | T型の引数、U型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.BinaryOperator<T,R> | 2つのT型の引数を受け取り、R型の戻り値を返却する関数。 |
| java.util.BiConsumer<T,U> | T型の引数、U型の引数を受け取り、戻り値を返却しない関数。 |

### 関数型インスタンスの生成方法

#### 匿名クラスを使う

```

```

#### ラムダ式を使う


## Stream APIでリストを別の型のリストに変換する

Stream APIでリストから特定の要素を取り除く

Stream APIでリストを集合に変換する

Stream APIでリストをマップに変換する

Stream APIでリストを集計する


 


