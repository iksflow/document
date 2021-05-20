# /bin/sh: node: command not found 오류 해결하기
<br/><br/>


## 1. 발생 원인
node.js가 설치되어있지 않아서 발생하는 문제이다.  
git을 설치하지 않으면 git commit과 같은 git 명령어를 실행할 수 없는 것 처럼, node.js를 설치해야 node 명령어를 사용할 수 있다.  
나는 VSCode Extension인 Code Runner를 설치해서 JS파일을 실행하려는 상황에 마주했다.  
<br/><br/>

## 2. Node.js 설치하기
* 설치된 node.js 버전 확인하기
    ```sh
    iksflow@iksflow frontend % node -v
    zsh: command not found: node
    ```
    당연하게도 설치가 안되어 있기 때문에 오류가 발생한다.  

* Homebrew를 통해 node.js 설치하기
    ```sh
    
    ```
