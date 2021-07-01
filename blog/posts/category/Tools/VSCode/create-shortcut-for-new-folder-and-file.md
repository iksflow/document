# 새 폴더, 파일 생성 단축키 만들기.

* 탐색기에 선택된 디렉토리에 파일 생성하기 : cmd + n
* 탐색기에 선택된 디렉토리에 폴더 생성하기 : cmd + f
* 탐색기로 포커스 이동하기 : cmd + shift + e

Mac 기준으로 설명.  
1. cmd+shift+p를 누르고 Preferences:Open Keyboard Shortcuts(JSON)를 검색해서 파일을 엽니다.
2. 아래 코드를 추가합니다.  

    ```JSON
    [
        // 기존 설정값에 아래 내용을 추가합니다.
        {
            "key": "cmd+n",
            "command": "explorer.newFile",
            "when": "explorerViewletFocus"
        },
        {
            "key": "cmd+f",
            "command": "explorer.newFolder",
            "when": "explorerViewletFocus"
        }
    ]
    ```
    * 속성에 대한 설명
        * key : 설정할 단축키
        * command : 액션
        * when : 단축키로 액션이 실행되는 조건입니다. explorerViewletFocus의 경우 탐색기에 포커스가 위치할 때에만 단축키가 동작하게 합니다.