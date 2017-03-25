# VueJs(Progressive Framework)

### Vuejs 유용성

- 한글로 작성된 레퍼런스 제공
- 상대적으로 간단한 API, 직관적인 사용성, 쉬운 템플릿 활용
- HTML, CSS, Js를 학습한 후 시작하기에 용이(학습접근성)
- 규모가 작은 앱도 처리 가능한 최소의 핵심 스택 구성 가능(유연성)
- 빠른 가상 DOM제공(고성능)



### Progressive: 다른 것과 혼합하여 사용하거나 부분적으로 채택할 수 있음



##### 1. 렌더링(Rendering)

##### 1.1. 선언적 렌더링(Declarative Rendering) 

jQuery처럼 불러오기만 하면(선언) VueJs를 사용할 수 있기 때문에 휠씬 쉽고 접근이 용이함

##### 1.2. 반응형 렌더링(RR) 

Dom의 문제점: 사용자가 인풋을 했을 때 re-rednering을 해야 하는데, 이러한 활동은 비싸고 성능에 지장을 줌. 또한, 상태가 변경되면서 오류가 잦음.

=> 데이터가 변경되면 이에 반응하여 뷰가 즉시 변경됨.



##### 2. 컴포넌트 시스템

##### 2.1.  페이지 단위 개발 대신 컴포넌트 단위 개발을 가능하게 함

##### 2.2. 커스텀 엘리먼트: 와 같은 방식으로 태그를 직접 만들어서 사용할 수 있음(가상DOM에서 위와 같은 태그를 쓰면 실제 DOM에서 표준 코드로 모두 바꿔줌)

##### 2.3. 컴포넌트 Communication이 가능함(props in, events out): parent가 child에게 props를 pass하고 child는 events를 방출함



##### 3. 클라이언트 기반 라우팅(Client-Side Rounting)

##### 클라이언트 사이드에서 라우팅 설정을 용이하게 할 수 있도록 플러그인 제공



##### 4. 대형 프로젝트 상태 관리 매니저 제공 (Vuex)

##### 대규모 프론트엔드 앱의 문제점 컴포넌트 간의 상태 정보 교환이 어려움 -> 중앙화된 상태 매니저를 제공해줌



##### 5. Build System & Development Experience 

ES20015 모듈을 간단히 import할 수 있음

template, style등을 한 파일 안에 모두 포함 가능 



##### 6. 브라우저 IE 9+ 지원



##### 7. Virtual DOM: HTML 자체가 template역할을 함( 최대한 렌더링되는 횟수를 줄일 수 있음)



##### 8 고속 렌더링: vanilla Js가 1x일때, Vue2.0은 1.37x



### VueJs 주요 특징

- 낮은 학습 비용(비교적 쉽게 느껴지는 API, HTML 기반 템플릿 문법 지원)
- 고속 렌더링 속도
- 초경량 프레임워크
- 컴포넌트 지향 UI
- Reactive System 데이터가 변경되면 뷰 또한 즉시 변경되는 반응형 시스템
- 데이터 바인딩(모델 데이터를 템플릿에 바인딩해줌)



시작하기

```javascript
<script src="https://unpkg.com/vue"></script>
```



### 데이터 바인딩 비교: Vanilla Js / jQuery/ VueJs



### vanilla JS

```javascript
<script data-type="domscript">

  function prettify(str){
    return str.replace(/{/g,'{\n&nbsp;&nbsp;').replace(/}/g,'\n}');
  }

  ;(function(){
    var demo, demo__input, demo__print_area, demo__print, demo__print_data;

    function init(){
      demo = document.querySelector('.demo');
      demo__input = document.querySelector('.demo__input');
      demo__print_area = document.querySelector('.demo__print-area');
      demo__print = document.querySelector('.demo__print');
      demo__print_data = document.querySelector('.demo__print-data');

      updataView(data.message, true);
      bindEvents();
    }

    function updataView(value, init){
      if(init) { demo__input.value = value; }

      demo__print_area.textContent = value;
      window.setTimeout(function(){
        printData(data);
      }, 100);
    }

    function updateData(value){
      data.message = value;
    }

    function printData(data){
      demo__print_data.innerHTML = prettify(JSON.stringify(data));
    }

    function updateInputField(e){
      var user_input = e.target.value;
      updataView(user_input);
      updateData(user_input);
    }

    function bindEvents(){
      demo__input.addEventListener('keyup', updateInputField);
    }

    init();
  })();
</script>

```

함수를 역할 별로 쪼개서 관리한다.



### jQuery

```

```



### VueJs

객체를 일일이 찾고 원하는 값을 할당할 필요없이 vue객체를 생성한 후 v-model 디렉티브를 활용하여 연결하기만 하면 끝!

- 선언적 렌더링 (마운트할 data, templete에 붙여준다. {{}} 보간법 )
- 반응형, 데이터의 변동상황이 바로 바로 적용됨

```html
<div class="demo">
  <input type="text" class="demo__input" v-model="message">
  <p class="demo__print">사용자가 입력한 값은 <span class="demo__print-area">{{ message}}</span>입니다.</p>
  <hr>
  <pre class="demo__print-data">{{this.$data}}</pre>
</div>	
```

```javascript
<script data-type="vue">
	new Vue({
      'el': '.demo',
      'data': data
	});
</script>
```



### div-vue-tools(크롬 개발자 도구)

구글 플러그 인 설치를 하면 ,브라우저 내에서 뷰가 있는 파일이 열려있을시 활성화 된다. (활성화 되면 색이 들어옴 , 비활성화 회색 표시)

관리 도구 파일 url에 대한 엑세스 허용(체크) : 로컬 영역에서도 작업가능



----



# 템플릿 & 디렉티브

### 템플릿 문법

Vue는 템플릿을 사용하여 대다수의 경우 HTML을 작성할 것을 권장합니다.

그러나 JavaScript가 완전히 필요한 상황이 있습니다. 바로 여기에서 템플릿에 더 가까운 컴파일인 render 함수를 사용할 수 있습니다.

```javascript
Vue.component('anchored-heading', {

	render: function(createElement) {
      
	}})

```



anchored-heading 안에 Hello world!와 같이 slot속성 없이 자식을 패스 할 때 그 자식들은 $slots.default에 있는 컴포넌트 인스턴스에 저장 된다는 것을 알아야 합니다.



### 디렉티브

```
디렉티브는 v- 접두사가 있는 특수 속성입니다.
디렉티브 속성 값은 단일 Javascript 표현식이 됩니다. (v-for 제외)
디렉티브의 역할은 표현식의 값이 변경될 때 사이드이펙트를 반응적으로 DOM에 적용하는 것입니다.
```



- v-once
  - 디렉티브감지 시스템을 사용하고 싶지 않은 경우 한번만 렌더링 한다.
  - 표현식이 필요하지 않다.
- v-text
  - 엘리먼트의 textContent를 업데이트 합니다. textContent의 일부를 갱신하려면 {{ Mustache }}를 사용해야 합니다.
- v-text와 {{ comment }} 비교
  - v- 접두사 있는 특수 속성: v- = "표현식"
  - 보간법을 주로 쓴다. v-text는 기존 데이터에 덮어쓰는 방식으로 행하기 때문에 {{comment}} 보간법이 더 용이하다.
- v-html
  - 엘리먼트의 innerHTML을 업데이트 합니다. 
  - 내용은 일반 HTML으로 삽입되므로 Vue템플릿으로 컴파일 되지 않습니다.
  - 보안에 취약하여 잘 사용하지 않음

### v-show, v-if

##### v-show, v-if는 모두 수시로 변경이 되는 상황에서 사용한다.

- v-show
  - 엘리먼트를 조건부로 표시하기 위한 또 다른 옵션 디렉티브이다.
  - v-show가 있는 엘리먼트는 항상 렌더링 되고 DOM에 남아있다.
  - v-show는 단순히 엘리먼트에 display CSS속성을 토글한다.
  - v-show는 <template> 구문을 지원하지 않으며 v-else와도 작동하지 않는다.

```html
<h1 v-show="ok">안녕하세요</h1>
```



##### style을 display:none; 상태로만 만듬( 단, type: false)

- v-if

  - 표현식 값의 참 거짓을 기반으로 엘리먼트 조건부 렌더링 합니다.

  - 엘리먼트 및 포함된 디렉티브 / 컴포넌트는 토글하는 동안 삭제되고 다시 작성됩니다. 

  - 엘리먼트가 <template>엘리먼트인 경우 그 내용은 조건부 블록이 됩니다.

    ​


조건 거짓일 경우 if는 마크업상 존재하지 않음,

show는 마크업상 존재하고 display: none; 상태일 뿐이다.



- v-for
  - Array | Object | number | string 을 쓸 수 있다.

```html
<li v-for="feature in vue_features" class="vue_features__item">{{vue_features.indexOf(feature)+1}}
//js 문법, 메소드를 활용하여 index위치를 적용할 수 있지만 자체적으로 

<li v-for="(feature,index) in vue_features" class="vue_features__item">{{index + 1}}{{feature}}</li>
(item,index) 값을 이렇게 쓸 수 있다. 


ii)객체의 경우 value,key,index를 가져와 쓸 수 있다. 

<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, key, index) in object"></div>

수업 예제) 

 <template v-for="(feature, index) of vue_features">
        <li class="vue-features__item">
          {{ feature.context }}
        </li>
        <li class="vue-features__item zebra">
          {{ feature.desc }}
        </li>
      </template>
```



### V-cloak & v-pre



##### v-cloak

```css
[v-clock] {
  display: none;
}
```

```html
<div v-cloak>
	{{ message }}
</div>
```

- 중괄호 자체를 안보이게 하기위해 사용
- Vue 컴포넌트가 컴파일되기전에 중괄호가 보이는 것을 방지한다. 



##### v-pre

```html
<span v-pre>{{ 이 부분은 컴파일 되지 않습니다. }}</span>
```

- 해당 엘리먼트와 모든 자식 엘리먼트에 대한 컴파일을 건너 뛴다.
- mustache 태그가 나타난다.
- 디렉티브가 없는 많은 수의 노드를 뛰어 넘으면 컴파일 속도가 빨라진다.










