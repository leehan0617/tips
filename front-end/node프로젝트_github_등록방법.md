1. github에 repository를 새로 만든다. (README.md, licence, gitignore설정 ...)
2. 로컬에 프로젝트를 생성한다. (node 프로젝트일경우 npm init or yarn init으로 시작해서 디렉토리가 만들어질듯.)
3. 로컬 프로젝트에 이 프로젝트의 원격 저장소 주소를 지정한다.(이 프로젝트는 이 github 주소에 등록할거야라고 하는것이다.)
명령어로는 
```
git remote add origin "https://github.com/leehan0617/tips" 이걸
```
치면된다. ("" 안의 주소는자신의 github 프로젝트 url로 수정)
4. 등록이 정상적으로 되면 
```
git pull origin master
```
을 실행 (깃헙에 등록했던 README.md, licence 정보, gitignore 파일...최초 등록했던 파일들을 로컬파일들과 병합, 만약 중복된 파일이 있다면 충돌날 수 있다.)
5. 생성 완료 그 뒤로는 add , commit, push 동일하게 사용하면 된다. 
