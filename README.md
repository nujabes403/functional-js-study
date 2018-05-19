# functional-js-study


#1.Iterator
```
/* 
  iterator는 어떤 자료구조를 순회(traverse)하는데 쓰이는 pointer라고 생각하면 된다. 
  (ex: database에서의 cursor 역할)
  next method가 구현되어 있으면 Iterator
*/

// 함수 실행 시 iterator 반환 
// var iter1 = iterator1([1, 2, 3])
// iter1.next()로 조회 (traverse)
// * iterator1은 [1, 2, 3] 이라는 자료구조를 순회한다. 순회할 때 쓰이는 pointer의 역할을 next가 한다.
const iterator1 = (data, index = 0) => ({
  next: () => ({ value: data[index++], done: index > data.length })
})

// 함수 실행 시 iterator 반환 
// var iter2 = iterator2()
// iter2.next()로 조회 (traverse)
// * iterator2은 0부터 시작하는 숫자 값의 자료구조를 순회한다. 순회할 때 쓰이는 pointer의 역할을 next가 한다.
const iterator2 = (count = 0) => ({
  next: () => ({ value: count++, done: false })
})

// 함수 실행 시 iterator 반환 
// var iter3 = iterator3(0, 5)
// iter3.next()로 조회 (traverse)
// * iterator3은 0부터 시작하면서 length까지 범위의 값을 갖는 자료구조를 순회한다. 순회할 때 쓰이는 pointer의 역할을 next가 한다.
const iterator3 = (count = 0, length) => ({
  next: () => ({ value: count++, done: count > length })
})
```

#2. Iterable
```
/* 
  [Symbol.iterator] 라는 특정한(specific)이름의 iterator가 있으면 Iterable
  [Symbol.iterator]는 iterator이므로 위에서 봤듯이 당연히 next method가 구현되어 있어야 함.
  for of 구문으로 순회(traverse) 가능
*/

// 함수 실행시 iterable 반환
// var _iterable1 = iterable1(0, 5)
// for (const v of _iterable1) { connsole.log(v) } // 0, 1, 2, 3, 4
// * iterable1에서 반환된 값은 [Symbol.iterator]라는 특정한 이름의 method가 구현되어 있기 때문에 iterable 이다.
// * [Symbol.iterator]는 next method가 구현되어있기 때문에 iterator이다.
const iterable1 = (count = 0, length) => ({
  [Symbol.iterator]: function () {
    return {
      next: () => ({ value: count++, done: count > length })
    }
  }
})

// 함수 실행시 iterable 반환
// var _iterable2 = iterable2([1, 2, 3])
// for (const v of _iterable2) { connsole.log(v) } // 1, 2, 3
// * iterable2에서 반환된 값은 [Symbol.iterator]라는 특정한 이름의 method가 구현되어 있기 때문에 iterable 이다.
// * [Symbol.iterator]는 iterator1에서 구현된 next method를 그대로 가져와서 사용했다. 여튼 next method가 구현되어있으니 iterator이다.
const iterable2 = (data, index = 0) => ({
  [Symbol.iterator]: function() {
    return {
      next: iterator1(data, index).next
    }
  }
})
```

#3. Iterator이면서 Iterable
```
/*
  (1)위에서 말했듯, Iterator의 조건은 next method가 구현되어있어야 할 것.
  (2)위에서 말했듯, Iterable의 조건은 [Symbol.iterator] 라는 이름의 iterator가 구현되어 있어야 할 것.
  
  (1)에 따라서 일단 next method를 구현해보자.
  {
    next: () => { }
  }
  next를 만들어주어서 (1)에 조건을 충족했다. 즉 위 객체는 iterator라고 말할 수 있다.
  
  (2)에 따라서 [Symbol.iterator] 라는 이름의 iterator를 구현해보자.
  {
    next: () => { },
    [Symbol.iterator]: function() {
      
    }
  }
  그런데 여기서 [Symbol.iterator]는 iterator로서 구현되어야 하기 때문에, 얘도 next가 필요하다.
  하지만 우리는 이미 위 객체에서 next를 구현해놓았기 때문에 이 next를 가져다 쓰고 싶다.
  {
    next: () => { },
    [Symbol.iterator]: function() {
      return this
    }
  }
  그러려면 그냥 위처럼 return this를 해주면 된다.
  
  ES6에서는 특별하게 
  이렇게 [Symbol.iterator] 라는 iterator가 자기자신을 리턴할 때 
  (잘 정의된 iterable) 'well-formed iterable'이라고 부른다.
*/

// (1) ???는 next method가 구현되어 있다. 따라서 iterator이다.
// (2) ???는 [Symbol.iterator]라는 이름의 iterator가 구현되어 있다. 따라서 iterable이다.
// (3) [Symbol.iterator]라는 iterator는 자기자신을 반환한다. 따라서 well-formed iterable이다.
var ??? = {
  [Symbol.iterator]: function() { 
    return this 
  },
  next: () => index < args.length ? { value: args[index++] } : { done: true }
}
```
