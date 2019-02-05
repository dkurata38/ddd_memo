# ドメイン駆動設計

## Entity と ValueObject
ここではJavaのオブジェクトのうち, 混同しやすいEntityとValueObjectの違いについて説明する.
まず共通点から先におさらいする.
エンティティと値オブジェクトはともに状態をもつPojoのクラスとして表現される.
次に本題の相違点を考える.
相違点は, 何をもって同じオブジェクトだと認められるかが異なる.
Entityの同一性は識別子(ID)によって担保され, Value Objectの同一性は値の属性そのものによって担保される.
ここで人を表すPersonクラスと名前を表すNameクラスについて考えたい.
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

Name name1 と Name name2を比較するときどのような条件で等しいと言えるか.
同姓同名という言葉から想起されるように, lastName と firstNameともに等しいときのみ, name1とname2が等しいと言える.
このようにクラス内の属性によって同一性を考えられるものをValue Objectと呼ぶ.

では以下のPersonクラスについてはどうだろうか.
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

果たして属性が等しい, つまり同姓同名なら同じ人とみなせるだろうか.
答えは否. 同姓同名の人も世の中にいる可能性がある.
その前提に立てば, 名前だけを同一性の判断材料にするのは誤りだ.
逆にどの属性があればよいだろうか. 確かなことは言えない.
なぜならすべての属性が一致する人が過去現在を通じていないと証明することはできず, 逆に時間とともに変わる可能性があるからです.
たとえば, 自分の前世だったら属性が限りなく自分と同じようなきがします.
同じアプリに2人が登録していた場合, どちらがどちらのデータかを判別できません.
また属性が時間とともに変わったからといって, 変わる前と変わった後で異なる人のデータかというとそうではないです.
結婚で名前が変わったからと行って, 前後で別人のデータであると判断をするのは誤りと言えます.
このように時間軸によって属性が書き換わる可能性がある, 書き換わっても同じデータと言える, 任意の属性だけで同一性を説明できないものをEntityという.
Entityには通常, 識別するための識別子が与えられる.
