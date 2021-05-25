# Kotlin

### 코틀린(Kotlin)과 자바(Java)의 차이점

1. 코틀린은 자바와 높은 상호 운영성(Java Compatible)을 가지고 있다.
    - 현재 Android API, JVM Lib, Java Framework를 그대로 사용 가능하며, Java → Kotlin 코드 변환 도구도 제공한다.
2. 자바보다 높은 안정성을 가지고 있다. 가장 큰 예로 Null Pointer Exception에서 자유롭다.
3. 자바보다 간결하다.

val : 불변 변수

var : 가변 변수

## Collection

---

코틀린의 컬렉션(Collection)은 쉽게 말해 배열 정도로 보시면 됩니다.

코틀린에서는 배열을 크게 2가지로 구분하여 정의하고 있습니다. 불변과 가변입니다. 즉 읽기만 가능한 형태가 있고, 읽고 쓰기가 가능한 형태가 있습니다.

**List<T> : 불변, 읽기전용(read only)**

→ List<T>는 한번 정의되면 그 이후로 변경이 불가능합니다. 따라서 add와 같이 수정하는 메소드가 존재하지 않습니다.

```kotlin
fun main(args:Array<String>){

	val list = listOf(1,2,3,"String")

	// size
	println(list.size)

	// contains
	if(list.contains(2)){
		println(ture)
	}
	else{
		println(false)
	}

	// indexOf : value의 index 찾기
	println(list.indexOf(2))
	
	// list[2] : 특정 index의 value 찾기
	println(list[2])
}
```

**MutableList<T> : 가변(read&write)**

→ List<T>와 달리 변경이 가능합니다. List<T>의 메소드에 'add'와 'remove'가 추가되어 있습니다.

```kotlin
val list = mutableListOf(1,2,3,"String")
var s = list.size

for(i in 0 until s){
	print(list[i])
  print(" ")
}
println()

// add
list.add("추가된 변수")

s = list.size
for(i in 0 until s){
	print(list[i])
	print(" ")
}
println()

// removeAt(index) : index에 해당하는 노드 지우기
list.removeAt(0)

s = list.size
for(i in 0 until s){
	print(list[i])
	print(" ")
}
println()

// addAll 한번에 다수 노드 추가
list.addAll(listOf("추가1", "추가2", 2,3,4))

s = list.size
for(i in 0 until s){
	print(list[i])
	print(" ")
}
println()
```

### hashMap<K, V>

hashMap은 Key와 Value의 매핑으로 이루어진 리스트

```kotlin
val map = hashMapOf(1 to "ABCD", 2 to "EFGH", 3 to "IJKL")

// value(값) 추가
map[4] = "MNOP" 

// map.remove(index) : index의 값 제거
map.remove(2) 

// 추가가 되긴함, 해시맵 처음 정의할 때key와 value의 타입을 정의하지 않아도 되지만,
// 신경써야 할 부분이 많아짐
map["2"] = "QRST"

//replace(index, value)
map.replace(2, "asdasdasd") // 인덱스가 Int형이 아니라 String형으로 변경되었기 때문에 변화없음
// 에러가 나는지 변화가 없는지 확인 필요!

```

없는 index에 대해 replace나 remove를 하였을 때 error가 발생하는지, 아무 변화 없는지 파악이 필요해 보입니다.

항상 타입과 관련해서 조심하자! 해시맵 선언부에 index, value의 type을 정의하지 않아도 되지만, 신경써야될 부분이 많다! → 오히려 해주는게 더 직관적이고 코드 관리할 때 좋아보입니다.

---

## Class

---

특별한 기능이나 형태를 가진 두 종류의 클래스에 대해 알아봅시다.

### DataClass

데이터를 다루는데에 최적화된 클래스로 5가지 기능을 내부적으로 생성해줍니다.

1. equals() : 객체 내 모든 값(파라미터)이 같은지 판단하는 함수
2. hashcode() : 객체의 내용에서 고유한 코드를 생성
3. toString() : 포함된 속성을 보기 쉽게 나타내는 함수
4. copy() : 객체 복사
5. componentX() : 속성을 순서대로 반환하는 함수 (component1(), component2()...)

```kotlin
fun main() {

    val a = General("보영", 212)
    
    println(a == General("보영", 212))
    println(a.hashCode())
    println(a)
    
    val b = Data("루다", 306)
    
    println(b == Data("루다", 306))
    println(b.hashCode())
    println(b)
    
    println(b.copy())
    println(b.copy("아린"))
    println(b.copy(id = 618))
}

class General(val name: String, val id: Int)

data class Data(val name: String, val id: Int)

[출처] [Kotlin] Data class / Enum class|작성자 사이시옷
```

### Enum Class

Enumerated type. 열거형 클래스, 순차적으로 적용되기 때문에 원하는 상수 value(0, 1, 2)로 값 확인 가능

**특징**

- valueOf 메소드로 열거형을 확인 가능
- enumValueOf 메소드로 열거형 클래스 확인
- 클래스 내 메서드 구현 가능(비교 대상은 객체 자기 자신 this 사용!)
	
---

### REFERENCE

[https://m.blog.naver.com/PostView.naver?blogId=acornedu&logNo=221091338894&proxyReferer=https:%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.naver?blogId=acornedu&logNo=221091338894&proxyReferer=https:%2F%2Fwww.google.com%2F)

