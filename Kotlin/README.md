# Kotlin

---

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
