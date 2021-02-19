node 버전을 올릴경우 이전 프로젝트(낮은 node버전)이 안돌아갈수도 있다.
이럴경우 nvm(node version manager)을 사용해 프로젝트별로 node 버전을 바꿔가면서 사용하자

node 버전별로 설치

nvm install 8.14.4
nvm install 14.15.4
...

설치할때마다 설치한 노드버전이 default 버전이 되는데,
상황에 맞추어서 node 버전을 사용

nvm use 14.15.4 or nvm use 8.14.4
바꾼후 터미널에서 노드버전 확인.
