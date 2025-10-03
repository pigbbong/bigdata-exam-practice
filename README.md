# 개요
이 저장소는 빅데이터분석기사 실기 시험을 대비하기 위해 제작한 개인 실습 자료입니다.
실제 기출 문제 형식을 참고하여, ChatGPT를 활용해 문제 지문과 데이터셋(CSV)을 생성하고, 이를 기반으로 직접 풀이 과정을 구현하였습니다.


# 📝 1유형

<details>
<summary><h2>📌 문제 목록 보기</h2></summary>

**1.** <h3 style="font-weight:normal;">
각 연도별로 사망률이 가장 높은 질병명을 구하고,<br>
해당 질병들의 사망자수 평균을 소수점 첫번째 자리에서 반올림하여 구하시오.
</h3>
<p><i>(사망률 = 사망자수 / 환자수)</i></p>

<details>
<summary>코드</summary>

df['사망률'] = df['사망자수'] / df['환자수']
<br>
target = df.groupby('연도')['사망률'].idxmax().values
<br>
answer = round(df[df.index.isin(target)]['사망자수'].mean())
<br>
answer

</details>


**2.** <h3 style="font-weight:normal;">
도시 거주자 중 60세 이상 남성의 평균 의료비를 구하시오.
</h3>

<details>
<summary>코드</summary>
target = df[(df['거주지'] == '도시') & (df['성별'] == '남성') & (df['연령'] >= 60)]
<br>
answer = target['의료비'].mean()
<br>
answer
</details>


**3.** <h3 style="font-weight:normal;">
각 연도별로 매출 상위 2개 제품의 매출 합계를 구하시오.
</h3>

<details>
<summary>코드</summary>
</details>


**4.** <h3 style="font-weight:normal;">
누적 재고량이 처음으로 5000을 초과한 월을 구하시오.
</h3>

<details>
<summary>코드</summary>
</details>


**5.** <h3 style="font-weight:normal;">
부서별로 연도별 급여 인상률의 평균을 계산한 후,<br>
인상률의 편차(표준편차)가 가장 작은 부서를 구하시오.  
(즉, 가장 일관되게 인상률이 높은 부서를 찾는 문제)
</h3>
<p><i>- 인상률 = (이번 해 연봉 - 전년도 연봉) / 전년도 연봉<br>
- 첫 해(2018년)는 인상률 계산에서 제외</i></p>

<details>
<summary>코드</summary>
</details>


**6-1.** <h3 style="font-weight:normal;">
도시 거주자 중 60세 이상 여성의 방문횟수 평균을<br>
소수 둘째 자리까지 반올림하여 나타내시오.
</h3>

<details>
<summary>코드</summary>
</details>


**6-2.** <h3 style="font-weight:normal;">
각 연도별 질병 사망률을 계산하고,<br>
그중 사망률이 가장 높은 질병 이름을 연도, 질병, 사망률 형태로 출력하시오.
</h3>
<p><i>(사망률 = 사망자 수 / 전체 환자 수)</i></p>

<details>
<summary>코드</summary>
</details>


**7.** <h3 style="font-weight:normal;">
각 연도별로, 반품률이 가장 높은 상품명을 구하시오.
</h3>
<p><i>(반품률 = 반품된 건수 / 전체 리뷰 건수)</i></p>

<details>
<summary>코드</summary>
</details>


**8.** <h3 style="font-weight:normal;">
각 과목별로, 최근 4년간(2020~2023) 평균 성적이<br>
가장 높은 학교 이름을 과목, 학교 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
</details>


**9.** <h3 style="font-weight:normal;">
각 연도별로 가장 많이 소비된 에너지원(전기/가스/수도)을 구하고,<br>
그 에너지원별로 해당 연도에서 발생한 총 요금의 합계를 구하시오.
</h3>
<p><i>- "사용량" 데이터 기준으로 가장 많이 소비된 에너지원 선정<br>
- 선정된 에너지원에 대해 → 해당 연도 "요금" 총합 구함<br>
- 연도별로 결과는 1개씩 출력 (예시: 연도 / 에너지원 / 총 요금)</i></p>

<details>
<summary>코드</summary>
</details>


**10.** <h3 style="font-weight:normal;">
각 고객ID별로 다음 규칙을 적용하여 월별 총 구매금액을 계산하고,<br>
그 중 2023년 총 구매금액이 상위 10%에 해당하는 고객들과 고객 수를 출력하시오.
</h3>
<p><i>(구매금액 결측 시: 같은 고객, 같은 카테고리의 연도별 평균으로 채움.<br>
평균도 없으면 전체 카테고리 평균으로 채움)</i></p>

<details>
<summary>코드</summary>
</details>


**11-1.** <h3 style="font-weight:normal;">
연령대(20대, 30대, 40대, 50대, ...)를 구분하는 연령대 컬럼을 만들고,<br>
각 연령대별로 콜레스테롤 평균을 계산하시오.
</h3>
<p><i>(콜레스테롤 결측치는 같은 지역 내 연령대 평균으로 채움.<br>
평균도 없으면 전체 평균으로 채움)<br>
최종 출력: 연령대, 평균 콜레스테롤</i></p>

<details>
<summary>코드</summary>
</details>


**11-2.** <h3 style="font-weight:normal;">
혈압, 혈당, 콜레스테롤 컬럼에 대해 표준화(z-score)를 적용하여<br>
새로운 컬럼을 추가하시오.
</h3>
<p><i>(혈당 결측치는 전체 평균으로 대체)<br>
표준화된 컬럼명: 혈압_zscore, 혈당_zscore, 콜레스테롤_zscore<br>
이후 혈압_zscore > 1.5 인 데이터의 수 출력</i></p>

<details>
<summary>코드</summary>
</details>


**12-1.** <h3 style="font-weight:normal;">
각 카테고리별, 성별로 평균 주문금액을 계산하시오.
</h3>
<p><i>(주문금액 결측치는 같은 그룹 평균으로 채움.<br>
평균도 없으면 전체 평균)<br>
최종 출력: 카테고리, 성별, 평균 주문금액</i></p>

<details>
<summary>코드</summary>
</details>


**12-2.** <h3 style="font-weight:normal;">
구매수량에 대해 최소-최대 정규화(min-max scaling)를 적용하여<br>
구매수량_scaled 컬럼을 추가하시오.
</h3>
<p><i>이후 구매수량_scaled ≥ 0.9 를 만족하는 데이터 개수 출력</i></p>

<details>
<summary>코드</summary>
</details>


**13-1.** <h3 style="font-weight:normal;">
각 고객ID별로 불만제기 경험 여부를 나타내는 파생 컬럼을 생성하시오.<br>
(한 번이라도 1이면 "Y", 아니면 "N")
</h3>
<p><i>이후 2023년에 불만제기 경험이 "Y"인 고객 수 출력</i></p>

<details>
<summary>코드</summary>
</details>


**13-2.** <h3 style="font-weight:normal;">
연령대(10대, 20대, 30대, 40대, 50대, 60대 이상) 컬럼을 생성하고,<br>
각 연령대별 주문수량 평균과 주문금액 평균을 구하시오.
</h3>
<p><i>최종 출력: 연령대, 평균 주문수량, 평균 주문금액</i></p>

<details>
<summary>코드</summary>
</details>


**14-1.** <h3 style="font-weight:normal;">
업무만족도가 결측인 직원은 부서 평균으로,<br>
부서 평균도 결측이면 전체 평균으로 채운다.
</h3>
<p><i>근속연수가 결측인 직원은 제거 후,<br>
업무만족도 Q1 이하 & 성과등급 A인 직원 수 출력</i></p>

<details>
<summary>코드</summary>
</details>


**14-2.** <h3 style="font-weight:normal;">
근속연수 ≥ 10년이고, 교육참여횟수가 전체 평균 이상인 직원들에 대해<br>
부서별 평균 연봉을 구하시오.
</h3>
<p><i>평균 연봉이 세 번째로 높은 부서의 값(정수) 출력</i></p>

<details>
<summary>코드</summary>
</details>


**14-3.** <h3 style="font-weight:normal;">
각 부서별로 업무만족도 기준 상위 20% 직원들의 평균 근속연수를 계산하시오.
</h3>
<p><i>(업무만족도·근속연수 결측 직원 제외)<br>
이후 가장 평균 근속연수가 높은 부서명 출력</i></p>

<details>
<summary>코드</summary>
</details>


**15-1.** <h3 style="font-weight:normal;">
각 제품군별로 연도·분기 기준 평균 반품률을 구하시오.<br>
(반품률 = 반품수량 / 판매수량)
</h3>
<p><i>최종 출력: 제품군, 연도, 분기, 평균 반품률</i></p>

<details>
<summary>코드</summary>
</details>


**15-2.** <h3 style="font-weight:normal;">
각 연도별로 지역·제품군 기준 매출액 총합을 계산하고,<br>
가장 매출이 높은 조합을 출력하시오.
</h3>
<p><i>최종 출력: 연도, 지역, 제품군, 총 매출액</i></p>

<details>
<summary>코드</summary>
</details>


**16.** <h3 style="font-weight:normal;">
2020~2023년 과목별 평균 성적이 가장 높은 학교명을 구하시오.
</h3>
<p><i>최종 출력: 과목, 학교</i></p>

<details>
<summary>코드</summary>
</details>


**17-1.** <h3 style="font-weight:normal;">
구매금액 결측치는 성별·카테고리 평균,<br>
없으면 전체 평균으로 대체한다.
</h3>
<p><i>연령대별 평균 구매금액과 평균 리뷰점수를 구하시오.<br>
최종 출력: 연령대, 평균 구매금액, 평균 리뷰점수</i></p>

<details>
<summary>코드</summary>
</details>


**17-2.** <h3 style="font-weight:normal;">
2023년에 한 번이라도 불만제기를 한 고객ID는 "불만경험 있음",<br>
그렇지 않은 경우 "불만경험 없음"으로 분류한다.
</h3>
<p><i>불만경험 있음 고객 중 평균 구매수량이 가장 높은 지역 출력</i></p>

<details>
<summary>코드</summary>
</details>


**18-1.** <h3 style="font-weight:normal;">
구매금액 결측치는 성별·상품군 평균,<br>
없으면 전체 평균으로 대체한다.
</h3>
<p><i>연령대별 평균 구매금액과 평균 리뷰점수를 구하시오.<br>
최종 출력: 연령대, 평균 구매금액, 평균 리뷰점수</i></p>

<details>
<summary>코드</summary>
</details>


**18-2.** <h3 style="font-weight:normal;">
각 고객ID별로 반품 이력이 있으면 "Y", 없으면 "N".
</h3>
<p><i>2023년 가입자 중 "Y" 고객 수 출력</i></p>

<details>
<summary>코드</summary>
</details>


**18-3.** <h3 style="font-weight:normal;">
상품군별로 리뷰점수가 4점 이상인 데이터만 필터링하여,<br>
리뷰점수 평균이 가장 높은 상품군을 출력하시오.
</h3>

<details>
<summary>코드</summary>
</details>


**19-1.** <h3 style="font-weight:normal;">
배송만족도 결측치는 동일 결제방식 그룹 평균,<br>
없으면 전체 평균으로 채운다.
</h3>
<p><i>- 4 이상: 상<br>
- 3 이상 4 미만: 중<br>
- 3 미만: 하<br>
등급별 평균 구매금액 출력</i></p>

<details>
<summary>코드</summary>
</details>


**19-2.** <h3 style="font-weight:normal;">
각 고객ID별 리뷰 비율(리뷰작성=1 비율)을 계산하시오.
</h3>
<p><i>리뷰 비율 ≥ 70% 인 고객 수 출력</i></p>

<details>
<summary>코드</summary>
</details>


**19-3.** <h3 style="font-weight:normal;">
2023년 데이터만 사용하여 결제방식별 반품율을 계산하고,<br>
반품율이 가장 높은 결제방식명을 출력하시오.
</h3>

<details>
<summary>코드</summary>
</details>

</details>
