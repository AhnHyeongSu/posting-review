# 유용한 인텔리제이 단축키 (Mac 편)

맥을 사용한지 어연 두 달.. 인텔리제이를 사용하는데 은근 단축키도 다르고 헤깔린다. 인텔리제이와 맥 단축키를 책상옆에 붙여놨지만 그냥 사용하는것만 사용한다. 코드리뷰를 할 때마다 마우스를 사용하며 버벅이는 나를 보며 선배가 안타까운 눈으로 쳐다본다.

나름 단축키를 이제 많이 알았다고 생각하지만 파도파도 끝이 없다.
그래도 이렇게 살 순 없다. 기왕 이렇게 된거 여기에 정리를 해보자.

---

### 인텔리제이(맥) 단축키 요약 본
![인텔리제이-맥](https://user-images.githubusercontent.com/37948906/120479572-0c887d80-c3e9-11eb-82ff-099334f788b2.jpeg)

### 인텔리제이(윈도우) 단축키 요약본
![인텔리제이-윈도우](https://user-images.githubusercontent.com/37948906/120479587-10b49b00-c3e9-11eb-8bcb-3a2318e036db.jpeg)

---

## 인텔리제이 유용한 단축키
### 1. 코드 자동완성
개발자의 필수품(?) 코드 자동완성. 기본은 사용가능한 모든 리스트를 보여주고, 똑똑한 코드 완성은 그 중에 진짜 사용할 것 같은 친구들만 나열한다.
#### Basic code completion (기본 코드 완성)
##### Mac) ^ + Space
![스크린샷 2021-06-02 오후 9 34 22](https://user-images.githubusercontent.com/37948906/120480742-5756c500-c3ea-11eb-8302-acea941a221d.png)

#### Smart code completion (똑똑한 코드 완성)
##### Mac) ^ + Shift + Space
![스크린샷 2021-06-02 오후 9 34 16](https://user-images.githubusercontent.com/37948906/120480757-59b91f00-c3ea-11eb-8bf3-84c50f0e77ef.png)

### 2. Rename (이름 바꾸기)
함수나 클래스, 변수 이름 등을 관련된 파일 모두 통째로 다 바꿀 수 있다.
##### Mac) Shift + F6
![스크린샷 2021-06-02 오후 9 43 51](https://user-images.githubusercontent.com/37948906/120482087-bf59db00-c3eb-11eb-8df0-cdef6440e8b3.png)

### 3. Search everywhere (전체 파일 검색)
빠르게 파일이나 클래스를 찾거나 이동할 때 사용한다.
##### Mac) Shift + Shift
![스크린샷 2021-06-02 오후 9 49 34](https://user-images.githubusercontent.com/37948906/120482796-70f90c00-c3ec-11eb-84d3-5b0ea1b3c04e.png)

### 4. Generate Code (코드 생성하기)
인텔리제이에서 제공하는 Generate를 사용할 때 쓴다. (생성자, toString, Override, Implement, Test code 등등)
##### Mac) Command + N / ^ + Enter
![스크린샷 2021-06-02 오후 9 53 17](https://user-images.githubusercontent.com/37948906/120483391-05fc0500-c3ed-11eb-9ca3-5541e64bc665.png)


### 5. Parameter Info (파라미터 정보)
파라미터 정보를 확인할 때 사용한다.
##### Mac) Command + P
![스크린샷 2021-06-02 오후 10 02 32](https://user-images.githubusercontent.com/37948906/120484661-414b0380-c3ee-11eb-9eed-e96c079eff4e.png)

### 6. Recent files popup (최근 파일 팝업)
최근에 사용한 파일 리스트를 팝업으로 띄울 때 사용한다.
##### Mac) Command + E
![스크린샷 2021-06-02 오후 10 04 46](https://user-images.githubusercontent.com/37948906/120484971-90913400-c3ee-11eb-9bf3-44705929cd5d.png)

### 7. Reformat (자동 정렬)
자동 정렬할 때 사용한다.
##### Mac) Command + Option + L
![스크린샷 2021-06-02 오후 10 09 13](https://user-images.githubusercontent.com/37948906/120485580-36dd3980-c3ef-11eb-97a4-cb7d75d0e0e8.png)
![스크린샷 2021-06-02 오후 10 09 25](https://user-images.githubusercontent.com/37948906/120485574-35ac0c80-c3ef-11eb-95d3-0012d62974e2.png)

##### 추가) 저장 시 자동 정렬 설정하는 방법 (Save Action 매크로)

<details>
<div markdown="1">

단축키를 누르면 자동 정렬이 되지만, 가끔 까먹고 안할 때가 있다. ~~(사실 코드를 짤 때부터 예쁘게 짜면 된다.)~~ 자동으로 저장할 때 정렬해주면 편하지 않을까? ~~(가끔 정렬하면 더 구린 경우 팀원들에게 욕을 먹을 수 있으니 주의한다. 그래도 일단 궁금하니까 찾아본다.)~~

1. Save Action 플러그인을 설치한다.
  
![스크린샷 2021-05-17 오후 9 42 14](https://user-images.githubusercontent.com/37948906/118490428-e3b18880-b758-11eb-9dad-a7ccd2f5bd8b.png)

2. Save Action 플러그인의 옵션을 킨다

![스크린샷 2021-05-17 오후 9 46 54](https://user-images.githubusercontent.com/37948906/118491057-941f8c80-b759-11eb-92ce-970c55ab093b.png)

[General]

- Activate save actions on save (before saving each file, performs the configured actions below)
  - 저장시 활성화 (각 파일을 저장하기 전에 아래에 구성된 작업 수행)
- Activate save actions on shorcut (default "CTRL + SHIFT + S")
  - 단축키로 저장했을 때 활성화 (기본값 "CTRL + SHIFT + S")
- Activate save actions on batch
  - 일괄 작업 시 활성화
- No action if compile errors(applied per file)
  - 컴파일 오류 발생시 조치 없음 (파일별로 적용)

[Formatting Actions]
- Optimize imports
  - import 최적화
- Reformat file
  - file 재정렬

1. java 파일만 자동 정렬하도록 설정한다. (필요하면 다른 것 추가)

![스크린샷 2021-05-17 오후 9 47 17](https://user-images.githubusercontent.com/37948906/118491036-908c0580-b759-11eb-9411-487263e8f342.png)

![스크린샷 2021-05-17 오후 9 47 07](https://user-images.githubusercontent.com/37948906/118491052-9386f600-b759-11eb-8c09-9439f068e8f3.png)

</div>
</details>

### 8. Go to declaration (선언으로 바로가기)
##### Mac) Command + B

### 9. Go to implementation (구현으로 바로가기)
##### Mac) Command + Option + B

### 10. File structure popup
현재 파일 전체 구조 보기 (파일 내에 메서드 이동할 때 편함)
##### Mac) Command + F12
![스크린샷 2021-06-02 오후 10 17 01](https://user-images.githubusercontent.com/37948906/120486735-48731100-c3f0-11eb-9745-9abbd64f181e.png)

### 11. Find usages
사용되고 있는 리스트 확인하기
##### Mac) Option + F7
![스크린샷 2021-06-02 오후 10 20 07](https://user-images.githubusercontent.com/37948906/120487281-c9caa380-c3f0-11eb-89c5-eb5742949b7c.png)

### 12. 개인적으로 등록해서 쓰고 있는 단축키
![스크린샷 2021-06-02 오후 10 22 26](https://user-images.githubusercontent.com/37948906/120487588-0f876c00-c3f1-11eb-980a-19aa52168763.png)


## 마치면서
혹시 인텔리제이에서 자주 사용하시는 유용한 단축키가 또 어떤 것이 있나요? 공유 해주세요! 😄