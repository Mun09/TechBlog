# 폰트사이즈 초기화 에러

2025.4.21

앱을 껐다 키면 폰트사이즈 또는 컬러가 초기화되는 문제 발견.



2025.4.22

문제 원인 발견

원인:  앱이 설치되고 최초실행에서 안드로이드에서는 컬러를 -1로 설정했는데 앱에서는 컬러가 디폴트(0, 하얀색)으로 해놨음. 평소에는 문제가 안되는데, 설정창을 열면 컬러를 -1로 가져와서 예외처리를 해줬는데, 그 과정에서 color가 잘못되면 폰트사이즈도 못바꾸게 해놨음. 그래서 계속 초기화됨.

해결방안: 초기 컬러를 하얀색으로설정.
