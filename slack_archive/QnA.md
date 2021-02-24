> 우아한테크코스 be-3기-질문방 채널에서 공유된 질문 및 답변 중 도움이 되었던 부분을 삭제되기 전 모아두었습니다. 😀


# About IntegerCache
https://meetup.toast.com/posts/185

# About EnumMap - written By 나봄(윤보민)
우테코에 있으면서 어떤 api를 좋으니까 사용하는 것보다 왜 더 좋을까 라는 고민을 하는 훈련을 하다보니
이번 로또 미션을 하다가 enum 을 사용해서 Map 자료구조를 EnumMap 을 사용하면서 어떤 점이 HashMap 보다 더 나을 지에 대해 알아봤어요!
알고보니 EnumMap 은 해싱 작업은 따로 하지 않고 순서(ordinal) 를 그대로 키로 잡기 때문이더라고요!  그냥 신기했어서 공유합니다!!! 헤헤헤헤

https://www.notion.so/EnumMap-vs-HashMap-8bc2595fa3644713987b38a864191ee1

# About DTO - written By 손너잘(손민성)
저도 이번 미션 하면서 DTO에 대해 생각을 많이 해봤는데요. 아래 자료들에서 답을 많이 찾았습니다.
제일 첫 링크는 DTO가 처음 등장한 마틴 파울러의 저서 pdf파일이에요. DTO에 관련된 부분만 한번 읽어보시면 좋아요.
그리고 테코톡에 올라온 DTO vs VO 영상들도 개념잡기에 좋았어요!

https://github.com/himanshugpt/ebooks-1/blob/master/Patterns%20of%20Enterprise%20Application%20Architecture%20-%20Martin%20Fowler.pdf
https://www.baeldung.com/entity-to-and-from-dto-for-a-java-spring-application
https://javacan.tistory.com/entry/methods-about-exporting-domain-object-to-view
https://buildplease.com/pages/repositories-dto/
https://stackoverflow.com/questions/21554977/should-services-always-return-dtos-or-can-they-also-return-domain-models

# 값 검증은 어디에서 하는게 맞을까? - written By 현구막(최현구)
## Question
```
// old
public Money(final String value) {
    this.value = Integer.parseInt(value);
    validateNegative(this.value);
}
// new
public Money(final int value) {
    this.value = value;
    validateNegative(this.value);
}
```
int 원시 값을 감싸는 Money 객체의 인자에 대해서, “parseInt 정도는 외부에서 해주는게 Money 도메인이 자신의 할 일에 더욱 집중할 수 있을 것 같다.” 는 리뷰를 받았어요! 그런데 parseInt가 불가능한 문자열 검증이 어디에 포함되어야 하는지 잘 모르겠어요…
View에서 진행하자니 ‘View에 로직이 포함되는거 아닌가?’ 라는 생각이 들고,
Money를 만드는 외부 도메인에서 진행하자니 Money가 사용할 값을 대신 검증해준다는게 오묘하게 느껴져요…
도와주세요!!

## Answers
- 저는 parseInt도 Money객체 생성의 한 과정이라고 생각해서 old가 더 맞다는 생각이 들어요. 만약 밖으로 빼게 된다면 입력이 숫자가 아닐 경우에 대한 검증이 도메인 밖에서 일어나야 되는데 괜찮을지 모르겠어요:미소짓는_얼굴:  
- "입력 받은 값을 적절한 타입으로 변환해서 controller에게 전달해준다." 라고 생각하면 InputView에서 가능하지 않을까요?:생각하는_얼굴: 간단한 타입 변환과 그에 대한 유효성 검사는 가능하다고 생각해요!
- https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html#BigDecimal-java.lang.String-
- 저는 클라이언트가 int로 바꿔 Money를 생성하게 하기 보다 클라이언트의 편의를 위해 String을 인자로 받는 생성자를 제공했을 것 같아요. :미소짓는_얼굴:


# 매개변수 타입을 쓸 경우 왜 invariant일 까? - written By 루트(김준근)
## Question
제네릭 공부하다가 좀 이상하다는 생각이 들어서 올려봅니다.  메서드를 호출할 때, 메서드 인자로 자식 클래스를 사용하는 건 문제가 없습니다. 하지만 인자가 리스트일 때는 상황이 좀 달라집니다. 자식 클래스의 리스트를 인자로 받으면 앞의 상황과는 다르게 인자로 받을 수 없다는 오류가 발생합니다.  제네릭을 쓰면 해결할 수 있다는 사실은 알고 있지만 이 상황이 뭔가 어색하게 느껴집니다. 이렇게 설계한 이유가 있을까요? 아니면 자바가 쓰레기인걸까요?:땀_흘리는_웃는_얼굴:
```
  function (new TestClass("aa"));
}
// 정상 작동
void function(Object o) {
  sout(o.toString());
}

  List<TestClass> list = Arrays.asList(new TestClass("aa"));
  function(list); // 오류 발생
}
// 오류 발생
void function(List<Object> o){
  sout(o.toString());
}
```
## Answers
- 배열 같은 경우에는 자식 클래스를 사용할 수 있는 것 같은데 그렇다면 반대로 생각해볼 수 도 있겠네요ㅎㅎ 왜 배열은 공변성을 가지게 되었는가?ㅎㅎ https://stackoverflow.com/questions/18666710/why-are-arrays-covariant-but-generics-are-invariant
- 제네릭을 만든 이유가, Object를 사용하면 개발자의 캐스팅이 필연적이고 이로인한 런타임 에러가 발생할 수 있다는 이유가 존재하네요. 따라서 제네릭에서 타입을 설정하면 그 타입만 사용하도록 한 듯 합니다. 하위클래스 까지 허용하면 캐스팅이 필요해질 상황이 발생할 수도 있고 이는 제네릭의 철학과 맞지 않으니까요. 단 와일드카드나 extends등의 제네릭의 경우는 개발자가 서브클래스가 들어갈 수 있다는 완벽한 인지 하에 작성했다는 생각이 들어가서 하위클래스를 허용하는 것 같네요.
- 배열이 왜 공변성을 갖는가에 대한 답을 좀 찾아봤습니다.
  결론은 다형성 때문이다!인것 같아요.
  아래의 링크에 잘 설명되어 있네요..
  https://en.wikipedia.org/wiki/Covariance_and_contravariance_%28computer_science%29#Covariant_arrays_in_Java_and_C.23
  위 링크와 제 설명을 읽기 전에 공변성과 반변성에 대해서 자세히 이해를 해야지 위키의 내용이 이해가 될 것 같아요
  https://edykim.com/ko/post/what-is-coercion-and-anticommunism/
  이 내용이 그 밑거름이 될 듯 합니다.
  위키 내용 기반으로 설명해 보겠습니다.
  왜 배열이 공변성을 가졌는가, 배열이 반변성을 가졌다고 가정해 봅니다. Cat과 Animal은 서로 상속관계 입니다. Cat <: Animal 이죠. 이때의 반변성의 정의에 따르면 Animal은 Cat 이다. 라는게 성립됩니다.
  따라서 만일 우리가 Cat[] cats = new Cat[10]; 라고 배열을 정의하면
  cats[0] = animal;
  이 가능해 집니다. 이때, 프로그래머는 cats[0] 을 했을 때 cat이 나오길 기대할 겁니다. 하지만 animal이 Dog 일 수도 있죠. 따라서 반변성의 경우에는 read에서 안전하지 않은 결과를 도출하게 됩니다.
  이제 공변성의 배열을 생각해 봅시다. 공변성의 배열은 우리가 평소 사용하는것과 동일합니다. 하지만 이 배열이 안전할까요? animals[0] = cat; 우리는 위와같은 코드를 항상 작성하지만, 사실 이는 write에 취약합니다. animals 배열이 실은 Dog의 배열일 수도 있거든요.
  그러면 이런 결론이 나옵니다.
  공변성은 write에 취약하고, 반변성은 read에 취약하다.
  그렇다면 둘 다에 안전한건 뭘까요?
  바로 불변성입니다. Generic이 이같은 경우죠.
  그러면 왜 배열은 안전한 불변성이 아닌 공변성을 택했을까요?
  그건 다형성과 연관이 있다고 합니다.
  만일 배열이 불변이라면, 다형성을 활용할 수 없기 때문에 array와 관련된 api들을 각 타입마다 만들어줘야 하는 힘듦이 있다고 하네요 (위 문서에서는 equals랑 shuffle을 예시로 들었군요). 문서에는 나와있지 않지만, 이러한 이유가 반변성 대신 공변성을 채택한 이유중 하나라고도 생각되네요.
  그렇다면 공변성은 write에 취약한데, 자바에서 이를 어떻게 처리할까요?
  ```String[] a = new String[1];
  Object[] b = a;
  b[0] = 1;
  ```
  위 코드에서 제일 마지막 코드는 에러를 발생시킵니다. 읽기에는 문제가 없죠, 쓰기에만 문제가 있죠(공변성이기 때문에)
  왜 에러가 났는가 하면, Integer가 String이 아니기 때문이라고 했네요. 어? 공변성은 이런 에러를 못잡는다 했는데 어떻게 잡았을까요?
  만일 b가 실제로 new Object[]로 만들어진 Object 배열이면 상관 없지만, String[] 구현체가 들어가 있습니다. 그래서 내부적으로 입력이 String인지 확인하는 절차를 거친다고 하네요
  하지만 이러한 방식은 엄격한 시스템이 컴파일 타입에 잡을 수 있는 에러를 런타임에 잡게되고(예를들면, 배열을 불변으로 만든 시스템..?) 어레이에 데이터를 쓸때마다 타입검사가 일어나니까 성능저하가 발생한다고 합니다. 그래서 제네릭 이나왔다고 하네요.
  와... 파고파고 들어가니까 자바 역사까지 공부해버렸네요..... 학부때 프로그래밍 언어론 수업 공부하는것 같기도 하고... 이게 야크쉐이빙인가요..? 
- https://www.notion.so/Java-33813ec47b9d43669317f816fc2abdc3 
