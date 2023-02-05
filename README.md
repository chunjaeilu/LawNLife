# 공공API 활용 앱 만들기 :: 공공서비스 조회 시스템

## GOAL
> 행정안전부_대한민국 공공서비스 정보 API 활용
> 
> 대상연령, 담당부처, 키워드 등을 검색했을 때 해당하는 데이터를 출력
> 
> 사용자 위주의 간결하고 깔끔한 UI/UX

## ISSUES

### 방대한 DB 처리
> 총 9919개의 방대한 DB를 한번에 불러오면 트래픽 초과 오류 발생
>
> 한 페이지에서 불러올 수 있는 데이터는 최대 100개

#### solution :  숫자 배열 생성 및 활용
- 1~100 까지 숫자를 인자로 갖는 배열 생성
- `fetch` url에 들어가는 page 변수값으로 활용
  ```javascript
  const numArr100 = Array.from({ length: 100 }, (_, index) => index + 1);
  ```
- `numArr100` 변수를 인자로 하는 반복문 안에 `fetch()` 포함시킴
  ```javascript
  for (const page of numArr100) {
    API_URL = `${URL}${api_list[0]}?&page=${page}&perPage=100&serviceKey=${API_KEY}`;
    const res = await fetch(API_URL);
    const data = await res.json();
    ...
  }
  ```

### 검색 결과 전역변수에 기억시키기
> 반복문 안에 반복문(filter로 검색어와 일치하는 인자 찾기)을 작성했을 때 검색결과가 전역에 저장되지 않는 문제
>
> 
