# エンティティと値オブジェクト
ここではドメインオブジェクトのうち, 混同しやすい値オブジェクトとエンティティの違いについて説明する.

相違点は, 何をもって同じオブジェクトだと見なすかがという点である.
値オブジェクトの同一性は値の属性そのものによって担保される一方で, エンティティは属性が可変であるため独自の識別しによって担保される.
値オブジェクト"Name"とエンティティ"Person"を例に挙げて具体的に説明する.

## 値オブジェクト
まずNameクラスの同一性について考える.
Nameクラスを以下のように定義する.

```java
class Name {
	private String lastName;
	private String firstName;

	public static Name of(String lastName, String firstName) {
		Name name = new Name();
		name.lastName = lastName;
		name.firstName = firstName;
		return name;
	}

	public String getFullName() {
		return Null;
	}
}
```

ここで`Name name1` と `Name name2`を比較するとき, どのような条件下で`name1 = name2`が成り立つだろうか.
同姓同名という言葉から想起されるように, `lastName`と`firstName`の両属性ともに等しい時, name1とname2が等しいと言える.
このようにクラス内の属性によって同一性を考えられるものを値オブジェクトと呼ぶ.


## エンティティ
では次にPersonクラスの同一性について考える.
```java
class Person {
	private Name name;
	public static Person of(Name name) {
		Person person = new Person();
		person.name = name;
		return person;
	}

	public Name getName() {
		return name;
	}
}
```

Nameクラス同様, どのような条件下で `Person person1`と`Person person2`で`person1 = person2`が成立するかを考える.
果たして属性が等しい, つまり同姓同名なら同一人物とみなせるだろうか.
いいえ, 同姓同名というだけで同じ人だとみなすことはできない. そのうえ, 名前は変わる可能性もあるので同一性を判断する材料にするには不確実である.
仮に名前属性に`Kurata Daisuke`という値を持つ`Person person1`が, 結婚により名前属性が`Nakajo Daisuke`という値をもつ`Person person2`に変わったとする.
`person1`も`person2`も同一人物を指しているので等式が成立するはずである.
つまり属性が等しいからと言って, その属性を持つエンティティも等しいというわけではない.

このように時間軸によって属性が書き換わる可能性がある, 書き換わっても同じデータと言える, 任意の属性だけで同一性を説明できないものをエンティティという.
エンティティには通常, 識別するための識別子が与えられ, この識別子によって同一性が担保される.
