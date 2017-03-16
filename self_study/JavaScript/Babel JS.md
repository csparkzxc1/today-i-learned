# Babel JS Compiler

### 1. Babel CLI

##### 1.1. Babel install

By `NPM` or `yarn` install babel-cli module at local

```
#NPM 
$ npm install --save-dev babel-cli

#yarn
$yarn add --dev babel-cli
```

 after installed devDependencies added at package.json file.

```
{
  "devDependencies": {
    "babel-cli": "^6.0.0"
  }
}
```



##### 1.2. Babel-Preset-Env module install

- Babel support ECMAScript by Syntax Transformers. 
- Using by this plugin, we can use All of the Web browsers.

```
# NPM
$ npm i -D babel-preset-env

# yarn
$ yarn add --dev babel-preset-env
```



##### 1.3. babelrc file

```
{
  "presets": ["env"]
}
```

```
{
  "presets": [
    ["env", {
      "targets": {
        "browsers": ["last 2 versions", "safari >= 7"]
      }
    }]
  ]
}
```



##### 2. Babel-cli 

- file compile

  ```
  # 기본 사용 방법
  $ babel <file.js>

  # 출력 파일 설정 방법
  # 옵션: -o, --out-file
  $ babel <input.js> -o <output.js>

  # 워치(Watch, 변경사항 관찰) 설정
  # 옵션: -w, --watch
  $ babel <input.js> -w -o <output.js>

  # 소스맵 설정
  # 옵션: -s, --source-maps true
  $ babel <input.js> -o <output.js> -s # [true, false, inline]

  # 인라인 소스맵 설정
  # 옵션: --source-maps inline
  $ babel <input.js> -o <ouput.js> --source-maps inline
  ```

    

  ​

  디렉토리 컴파일

  ​

  ```
  # 옵션: -d, --output-dir

  $ babel <input_dir> -d <output_dir>

  # 디렉토리 파일을 한 개의 파일로 컴파일 설정

  $ babel <input_dir> -o <output.js> -s



  ```

  ​

  ****

  ##### 파일 무시 설정

  ```
  # 옵션: --ignore

  $ babel <input_dir> -d <output_dir> --ignore specs.js,test.js

  ```

  ​

  ##### 플러그인 설정

  ```
  # 옵션: --plugins=

  $ babel <input.js> -o <output.js> --plugins=es2015

  ```

  ​

  ##### 프리셋(Presets) 설정

  ```
  # 옵션: --presets=

  $ babel <input.js> -o <output.js> --presets=add-module-exports,transform-es2015-modules-amd

  ```

  ​

  ​

