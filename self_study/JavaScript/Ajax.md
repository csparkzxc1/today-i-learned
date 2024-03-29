# Ajax

### 1. Ajax(Asynchronous JavaScript and XML)

전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터 로드 기법.



### 1.1. http-server  설치 및 포트 열기

- nom i -D http-server (디렉토리내 서버 설치)
- "start": "http-server -o -p 3000 -a localhost"-. -o: 부라우저 열기 -. -p:포트 넘버(디폴트 8080) -. -a: Address(디폴트 0.0.0.0)

### 1.2. XMLHttpRequest 객체 생성

```javascript
var xhr = new XMLHttpRequest();
```

> IE&+ 부터 사용가능: createXHR IE5+ : `new ActiveXObject('Msxml2.XMLHttp')`



### 1.3. Ajax 서버 요청 매서드 open, send

- open(method, url, async) : 요청 준비 매서드 -. method : HTTP(GET, POST, PUT, DELETE) 메서드 설정 -. url: 통신 파일 경로 설정(서버에 있는 파일의 주소) -. async: 비동기(async: true), 동기(sync: false, deprecated) 통신 설정 (디폴트는 true이며 삭제 가능)
- send(): 준비된 요청을 전달하는 매서드



### 1. 4. Ajax server 응답 상태 메시지

```
xhr.onreadystatechange = function() {  // on 이벤트 발생으로 함수 호출
  if ( this.status === 200 ) { //  상태값 검사
    서버 실행 결과 처리 코드 작성
  }
}
```

- 2XX : Success	     200 서버가 응답을 성공적으로 완료
- 3XX : Redirection      304 응답내용이 이전 응답 내용과 동일
- 4XX : Client errors    404 페이지를 찾을 수 없다. 
- 5XX : Server error     500 서버 내부에서 오류 발생

----



###  비동기 통신

> 요청시 코드가 실행 될 때의 이벤트 설정 필요 이벤트 전파, 차단, 위임 위주 비동기 처리에서는 데이터를 받았을 때 코드르르 실행해야 한다. 비동기로 데이터를 받았다는 수신을 `이벤트로 받아서` 처리해야 한다. 



```javascript
(function(glibal){
  ...
  if(xhr.status === 200){
    console.log('통신 성공!');
    //
    print_callback_data.innerText = xhr.responseText;
    console.log('xhr.response: ', xhr.response);
    console.log('xhr.responseText: ', xhr.responseText);
    console.log('xhr.responseXML: ', xhr.responseXML);
    console.log('xhr.responseType: ', xhr.responseType); 
    } else {
      console.log('통신실패!');
    }
});
```



### readyState

> 비동기 통신 상태를 숫자로 반환 - 기본이 0이고 숫자가 들어감. 완료된 4번이 중요함.

```
0 : 초기화되지 않은 상태
1 : 로딩 중
2 : 로드 됨
3 : 인터렉티브
4 : 완료
```



### onreadystatechange

기본값. function을 참조시켜 콜백함수로 활용

cf. xhr 객체를 통해서 서버로부터 전송받은 상태의 값이 200(데이터 통신 확인), readystate 상태의 값이 4(모든 데이터를 불러온 것 확인)라면 onreadystatechange 함수 내에거 작동시킨다.



### GET요청(데이터 불러오기)

```javascript
// 비동기 설정
xhr.open('GET', './data/db.txt');
xhr.send(null);

// 명시적으로 아무것도 보내지 않음
// 동기 방식(기다림..)
// 데이터가 도착하면 성공?실패?를 파악하여 처리
// 조건 분기가 바로 시작되지 않고 기다리고 있음
xhr.onreadystatechange = function() {
  if ( this.status === 200 && xhr.readyState === 4) {
    console.log('통신 성공!');
    print_callback_data.innerText = xhr.responseText;
  } else { 
  console.log('데이터 로드중...');
  print_callback_data.innerText = '데이터 로드중...';
  }
};
```

`she.send`를 명시적으로 불러오지 않으면 서버에 요청 안했으므로 요청을 하는 이벤트만 기다리고만 있음.

- 확인 코드 if문 중첩 방식으로 표현

```
if (this.status === 200) {
  if (xhr.readyState === 4) {
    console.log('통신 성공!');
    print_callback_data.innerText = xhr.responseText;
  } else { 
  	console.log('데이터 로드 중...');
  	print_callback_data.innerText = '데이터 로드 중...';
  	}
  ...
};
```



### XML 방식(대상 찾아오기)

``` javascript
var html_template = '<ul>';

console.log('this.responseXML:', thsi.responseXML);
var xmlDoc = this.responseXML;
// xml은 document이므로 DOM API를 써야함

xmlDoc.querySelectorAll('user > results').forEach(function(result){
  var title = result.querySelector('name title').textContent;
  var first = result.querySelector('name first').textContent;
  var last = result.querySelector('name last').textContent;
  
  // 첫 글자를 대문자로 변경
  title = title.replace(/^./, function($1) {
    return $1.toUpperCase();
  });
  var name = title +'. '+ first + last
  html_template+='<li>'+name+'</li>';
});
html_template += '</ul>';
```

- xml의 경우 dom 탐색을 해서 객체를 찾아온다.

  `this.responseXML: #document`

- 그냥 텍스트로 처리할 수 없으므로 this.response/this.responseText를 사용할 수 없음

---



### JSON 실습



##### Ajax 통신으로 JSON 파일 요청

```javascript
xhr.open('GET', './data/people.json');
```



##### 서버에서 받아온 JSON 데이터 변환

`JSON.parse()` : string 으로 전달받은 JSON텍스트 값을 객체로 변환하여 리턴함

```javascript
xhr.onreadystatechange = function() {
  if (this.status === 200 && this.readyState === 4) {
    var list = JSON.parse(this.reponse);
  }
};
```



##### 변환한 객체 값을 사용하여 리스트 목록에 추가

``` javascript
// ...
list.results.forEach(function(results) {
  var title = results.name.title;
  var first = results.name.first;
  var last = results.name.last;
  title = title.replace(/^./, function($1) {
    return $1.toUpperCase();
  });
})
var name = title + '. ' + first + ' ' + last;
ul.insertAdjacentHTML('beforeend', '<li>' + name + '</li>');
// ....
};
```



---



### POST  방식

- 사용자가 입력한 데이터를 서버에 전송해서 DataBase에 저장, GET방식과 다르게 파라미터 값을 받아와서 전달하는 부분 필요.

- 파라미터 값을 서버로 전달할 때, 의도치 않은 전달을 피하기 위해 encodeURIComponent 처리를 해 주는 것을 권장함.

  ​

### JSON Server 사용한 실습

- 추가적인 설정없이 REST API 테스트를 위한 서비스 제공하는 서버



##### 설치 & 실행

```
$ npm i -D json-server
```

```
$ json-server -w DB/db.json
```

##### 데이터 요청

```
# id 값으로 데이터 요청
http://localhost:3000/music-list/1

# artist 값으로 데이터 요청
# 요청한 결과에 맞는 데이터만 반환 (artist의 값이 Rihanna 인 데이터만 반환)
http://localhost:3000/music-list?artist=Rihanna

# 질의(query) 값으로 데이터 요청
# 대소문자 구분 없이 검색 값을 포함하는 데이터 반환(rihanna를 포함하는 데이터들 반환)
http://localhost:3000/music-list?q=rihanna
```

