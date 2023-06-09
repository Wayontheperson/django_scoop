# 앱 모듈

## 1. 공통 모듈
장고 앱 생성시 생성되는 앱과 약속적으로 사용되는 모듈 이름 

- admin.py
- forms.py
- management/
- migrations/
- models.py
- templatetags/
- tests/
- urls.py
- views.py

<br>

## 2. 비공통 모듈 
사용자에 따라 만드는 모듈 중 일반적으로 만드는 모듈 이름

- behaviors.py : 모델 믹스인 위치에 대한 옵션
- constants.py : 앱레벨에서 이용되는 세팅을 저장하는 장소의 이름
- decorators.py : 데코레이터 모은 모듈
- db/ : 여러프로젝트에서 이용되는 커스텀 모델이나 컴포넌트
- fields.py : 폼 필드 이용에 쓰인다. 하지만 db/패키지 생성으로도 충분하지 못한 필드가 존재할 때 모델 필드에 이용되기도 한다.
- factories.py : 테스트 데이터 팩토리 파일
- helpers.py : 헬퍼 함수 뷰와 모델을 가볍게 하기 위해 뷰에서 추출한 코드를 저장하는 장소 (utils.py와 비슷)
- managers.py : models.py가 너무 커질 경우, 일반적인 해결책으로 커스텀 모델 매니저가 여기로 이동된다.
- signales.py : 커스텀 시그널을 제공하는 것에 대한 대안으로 커스텀 시그널을 넣기에 유용한 장소
- utils.py : helpers.py와 동일
- viewmixins.py 뷰 믹스인을 이 모듈로 이전함으로써 뷰 모듈과 패키지를 더 가볍게 할 수 있다.

