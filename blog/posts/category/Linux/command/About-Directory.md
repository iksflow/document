# 디렉터리 관련 커맨드

### pwd (Present Working Directory)  
현재 작업중인 디렉터리 경로를 보여준다.

### cd (Change Directory)  
작업 디렉터리를 변경한다.

### mkdir (Make Directory)  
새 디렉터리를 생성한다.

### rmdir (Remove Directory)
디렉터리를 삭제한다.

### ls (List)

* 지정한 경로의 파일과 디렉터리 리스트를 출력한다.  
* 형식 
```sh
ls [option] 지정경로
```

```sh
[iksflow@localhost ~]$ ls -al study
total 32
drwxrwxr-x.  5 iksflow iksflow 4096 Aug 26 08:52 .
drwx------. 24 iksflow iksflow 4096 Aug 31 12:17 ..
drwxrwxr-x.  7 iksflow iksflow 4096 Aug 26 08:52 part1
drwxrwxr-x.  3 iksflow iksflow 4096 Aug 26 12:41 part2
drwxrwxr-x.  2 iksflow iksflow 4096 Aug 19 12:30 process
-rw-r--r--.  1 root    root    2175 Aug 25 09:02 su_dash.txt
-rwxrwxrwx.  1 root    root    3095 Aug 25 09:01 su.txt
-rw-rw-r--.  1 iksflow iksflow 2173 Aug 25 09:04 user.txt

```

### touch
파일의 마지막접근시간(access time), 수정시간(modification time), 변경시간(change time)과 같은 파일스탬프 정보를 수정한다.

### rm
파일을 삭제한다.

### cp
파일을 복사한다.

### mv 
파일을 이동한다.

### head
지정한 파일의 앞 부분을 출력한다.  옵션을 설정하지 않는경우 처음 10줄을 출력한다.
```sh
head [option] file
```

### tail
지정한 파일의 끝 부분을 출력한다. 옵션을 지정하지 않는경우 마지막 10줄을 출력한다.
주로 로그파일을 확인하는 경우 많이 사용된다.