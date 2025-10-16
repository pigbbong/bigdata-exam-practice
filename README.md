# 개요
본 저장소는 **빅데이터분석기사 실기 대비용 학습 정리 레포지토리**로,  
기출 및 예상 문제를 ChatGPT를 활용해 직접 구성하고,  
각 문제별 접근 방식과 풀이 과정을 기록한 개인 학습 아카이브입니다.  

CSV 파일은 각각 type1(1유형), type2(2유형), type3(3유형) 디렉토리에 있으며, 다운로드 or 깃허브 경로를 지정하고, pd.read_csv()를 사용해 불러와 문제 풀이에 활용하실 수 있습니다.

CSV 파일 번호를 문제 번호에 맞춰서 푸시면 됩니다. ex) 1번 문제 -> 1.csv, 6-1, 6-2번 문제 -> 6.csv

각 문제별로 제가 직접 작성한 풀이 코드를 포함하였으나, 제 방식이 유일한 정답은 아닙니다.  
상황과 접근 방식에 따라 더 나은 코드가 있을 수 있으며, 이 저장소의 코드는 학습 참고용으로 활용하시길 바랍니다.


# 📝 1유형

<details>
<summary><h2>📌 문제 목록 보기</h2></summary>

<h3 style="font-weight:normal;">1.</h3>
<h3 style="font-weight:normal;">
각 연도별로 사망률이 가장 높은 질병명을 구하고, 해당 질병들의 사망자수 평균을 소수점 첫번째 자리에서 반올림하여 구하시오. (사망률 = 사망자수 / 환자수)
</h3>

<details>
<summary>코드</summary>

df['사망률'] = df['사망자수'] / df['환자수']<br>
target = df.groupby('연도')['사망률'].idxmax().values<br>
answer = round(df[df.index.isin(target)]['사망자수'].mean())<br>
answer
<br><br>

>779
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">2.</h3>
<h3 style="font-weight:normal;">
도시 거주자 중 60세 이상 남성의 평균 의료비를 소수점 둘째자리까지 반올림하여 구하시오.
</h3>

<details>
<summary>코드</summary>
target = df[(df['거주지'] == '도시') & (df['성별'] == '남성') & (df['연령'] >= 60)]<br>
answer = target['의료비'].mean()<br>
answer

<br><br>
>560229.24
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">3.</h3>
<h3 style="font-weight:normal;">
각 연도별로 매출 상위 2개 제품의 매출 합계를 구하시오.
</h3>

<details>
<summary>코드</summary>
target = df.sort_values(by=['연도', '매출'], ascending=[False, False]).groupby('연도').head(2)['매출'].sum()<br>
target

<br><br>
>44076
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">4.</h3>
<h3 style="font-weight:normal;">
누적 재고량이 처음으로 5000을 초과한 월을 구하시오.
</h3>

<details>
<summary>코드</summary>
df['월'] = pd.to_datetime(df['월'])<br>
df['month'] = df['월'].dt.month<br>
target = df[df['누적재고'] > 5000]['month'].iloc[0]<br>
target
	
<br><br>
>9
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">5.</h3>
<h3 style="font-weight:normal;">
부서별로 연도별 급여 인상률의 평균을 계산한 후, 인상률의 편차(표준편차)가 가장 작은 부서를 구하시오.  (즉, 가장 일관되게 인상률이 높은 부서를 찾는 문제)</h3>

<h4 style="font-weight:normal;">
- 인상률 = (이번 해 연봉 - 전년도 연봉) / 전년도 연봉
<br>
- 첫 해(2018년)는 인상률 계산에서 제외
</h4>

<details>
<summary>코드</summary>
<span style="color:gray;"># 전년도 연봉 추가 (부서별 shift)</span><br>
df['전년도연봉'] = df.groupby('부서')['연봉'].shift(1)<br><br>

<span style="color:gray;"># 인상률 계산</span><br>
df['인상률'] = (df['연봉'] - df['전년도연봉']) / df['전년도연봉']<br><br>

<span style="color:gray;"># 각 부서별 인상률의 표준편차 계산</span><br>
std_by_dept = df.groupby('부서')['인상률'].std()<br><br>

<span style="color:gray;"># 인상률의 표준편차가 가장 작은 부서</span><br>
answer = std_by_dept.idxmin()<br>
print("가장 일정한 인상률을 가진 부서:", answer)

<br><br>
>가장 일정한 인상률을 가진 부서: 인사부
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">6-1.</h3>
<h3 style="font-weight:normal;">
도시 거주자 중 60세 이상 여성의 방문횟수 평균을 소수 둘째 자리까지 반올림하여 나타내시오.
</h3>

<details>
<summary>코드</summary>
answer = round(df[(df['거주지'] == '도시') & (df['연령'] >= 60) & (df['성별'] == '여성')]['방문횟수'].mean(), 2)<br>
answer
	
<br><br>
>2.38
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">6-2.</h3>
<h3 style="font-weight:normal;">
각 연도별 질병 사망률을 계산하고, 그중 사망률이 가장 높은 질병 이름을 연도, 질병, 사망률 형태로 출력하시오.  (사망률 = 사망자 수 / 전체 환자 수)
</h3>

<details>
<summary>코드</summary>
<span style="color:gray;"># 연도-질병별 전체 환자수와 사망자 수 계산</span><br>
df_rate = df.groupby(['연도', '질병']).agg(<br>
&nbsp;&nbsp;사망자수=('사망여부', 'sum'),<br>
&nbsp;&nbsp;인원수=('사망여부', 'count')<br>
).reset_index()<br><br>

<span style="color:gray;"># 사망률 계산</span><br>
df_rate['사망률'] = df_rate['사망자수'] / df_rate['인원수']<br><br>

<span style="color:gray;"># 연도별 최고 사망률 질병 추출</span><br>
answer = df_rate.sort_values(['연도', '사망률'], ascending=[True, False]) \ <br>
&nbsp;&nbsp;.groupby('연도').head(1)[['연도', '질병', '사망률']].reset_index(drop=True)<br>
display(answer)

<br><br>
| 연도 |  질병   |  사망률   |
|:----:|:------:|:---------:|
| 2018 | 고혈압 | 0.142857  |
| 2019 | 심장병 | 0.142857  |
| 2020 |   암   | 0.111111  |
| 2021 | 심장병 | 0.133333  |
| 2022 |   암   | 0.111111  |
| 2023 |   암   | 0.166667  |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">7.</h3>
<h3 style="font-weight:normal;">
각 연도별로, 반품률이 가장 높은 상품명을 구하시오.  (반품률 = 반품된 건수 / 전체 리뷰 건수)
</h3>

<details>
<summary>코드</summary>
df_rate = df.groupby(['연도', '상품']).agg(반품된건수=('반품여부', 'sum'), 리뷰건수=('반품여부', 'size')).reset_index()<br>
df_rate['반품률'] = df_rate['반품된건수'] / df_rate['리뷰건수']<br>
answer = df_rate.sort_values(by=['연도', '반품률'], ascending=False).groupby('연도').head(1)[['연도', '상품']].reset_index(drop=True)<br>
answer
	
<br><br>
| 연도 |   상품   |
|:----:|:-------:|
| 2023 | 스마트폰 |
| 2022 | 스마트폰 |
| 2021 | 냉장고   |
| 2020 | 노트북   |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">8.</h3>
<h3 style="font-weight:normal;">
각 과목별로, 최근 4년간(2020~2023) 평균 성적이 가장 높은 학교 이름을 과목, 학교 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df_melt = pd.melt(df, id_vars=['학교', '연도'], value_vars=['국어', '영어', '수학', '과학'], var_name='과목', value_name='점수')<br>
df_melt = df_melt.groupby(['학교', '과목']).agg(과목평균=('점수', 'mean')).reset_index()<br>
answer = df_melt.sort_values(by=['과목', '과목평균'], ascending=False).groupby('과목').head(1)[['과목', '학교']].reset_index(drop=True)<br>
answer
	
<br><br>
|  과목  |  학교  |
|:------:|:------:|
|  영어  | 서울고 |
|  수학  | 부산고 |
|  국어  | 광주고 |
|  과학  | 광주고 |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">9.</h3>
<h3 style="font-weight:normal;">
각 연도별로 **가장 많이 소비된 에너지원(전기/가스/수도)**을 구하고, 그 에너지원별로 해당 연도에서 발생한 총 요금의 합계를 구하시오.
- 이 때, "사용량" 데이터 기준으로 가장 많이 소비된 에너지원 선정
- 선정된 에너지원에 대해 → 해당 연도 "요금" 총합 구함
- 연도별로 결과는 1개씩 출력 (예시: 연도 / 에너지원 / 총 요금)
</h3>

<details>
<summary>코드</summary>
melt = pd.melt(df, id_vars=['연도', '지역', '구분'], value_vars=['가스(m³)', '수도(m³)', '전기(kWh)'], var_name='에너지원', value_name='총')<br><br>

melt1 = melt[melt['구분'] == '사용량']<br>
melt2 = melt[melt['구분'] == '요금']<br><br>

merge = pd.merge(melt1, melt2, on=['연도', '지역', '에너지원'], suffixes=['사용량', '요금'])<br><br>

grouped = merge.groupby(['연도', '에너지원'])[['총사용량', '총요금']].sum().reset_index()<br>
target = grouped.groupby('연도')['총사용량'].idxmax()<br>
answer = grouped.loc[target, ['연도', '에너지원', '총요금']]<br>
answer

<br><br>
| 연도 |  에너지원  | 총요금 |
|:----:|:----------:|:------:|
| 2020 | 전기(kWh)  |  3800  |
| 2021 | 전기(kWh)  |  3500  |
| 2022 | 전기(kWh)  |  3850  |
| 2023 | 전기(kWh)  |  4000  |
</details>


<br><br><br><br>


<h3 style="font-weight:normal;">10.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 다음 규칙을 적용하여 월별 총 구매금액을 계산하고, 그 중 2023년 총 구매금액이 상위 10%에 해당하는 고객들과 고객 수를 출력하시오.
(이 때, 구매금액이 결측인 경우에는 같은 고객, 같은 카테고리의 연도별 평균 구매금액으로 채울 것 (단, 평균값도 결측이면 전체 카테고리 평균 구매금액으로 채움))
</h3>

<details>
<summary>코드</summary>
<span style="color:gray;"># 구매금액이 결측치인 행 대치</span><br>
df['구매금액'] = df['구매금액'].fillna(df.groupby(['연도', '고객ID', '카테고리'])['구매금액'].transform('mean'))<br><br>

<span style="color:gray;"># 평균 구매금액 값도 결측치인 행 대치</span><br>
df['구매금액'] = df['구매금액'].fillna(df['구매금액'].mean())<br><br>

<span style="color:gray;"># 월별 총 구매금액 먼저 구하기</span><br>
monthly_sum = df.groupby(['고객ID', '연도', '월'])['구매금액'].sum().reset_index()<br><br>

<span style="color:gray;"># 다시 연도별 총 구매금액 계산</span><br>
grouped = monthly_sum.groupby(['고객ID', '연도'])['구매금액'].sum().reset_index()<br><br>

target = grouped[grouped['연도'] == 2023]['구매금액'].quantile(0.9)<br>
answer = grouped[(grouped['연도'] == 2023) & (grouped['구매금액'] >= target)]['고객ID'].values.tolist()<br><br>

print("조건에 맞는 고객들:")<br>
for i in range(len(answer)):<br>
&nbsp;&nbsp;&nbsp;&nbsp;print(answer[i])<br><br>

print("\n조건에 맞는 고객 수:", len(answer))

<br><br>
>조건에 맞는 고객들:<br>
>CUST025<br>
>CUST028<br>
>CUST033<br>
>CUST061<br>
>CUST062<br>
>CUST065<br>
>CUST071<br>
>CUST077<br>
>CUST088<br>
>CUST095<br><br>
>
>조건에 맞는 고객 수: 10
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">11-1.</h3>
<h3 style="font-weight:normal;">
연령대(20대, 30대, 40대, 50대, ...)를 구분하는 연령대 컬럼을 만들고, 각 연령대별로 콜레스테롤 평균을 계산하시오. 
(단, 콜레스테롤 결측치는 같은 지역 내 연령대 평균으로 채울 것. (평균값도 결측이면 전체 평균으로 채움))
최종 출력은 연령대, 평균 콜레스테롤 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['연령대'] = (df['연령'] // 10 * 10).astype(str) + '대'<br><br>

<span style="color:gray;"># 지역, 연령대 기준으로 콜레스테롤 각 결측치 대치</span><br>
df['콜레스테롤'] = df['콜레스테롤'].fillna(df.groupby(['연령대', '지역'])['콜레스테롤'].transform('mean'))<br><br>

<span style="color:gray;"># 아직도 결측치인 값은 전체 평균으로 대치</span><br>
df['콜레스테롤'] = df['콜레스테롤'].fillna(df['콜레스테롤'].mean())<br><br>

answer = df.groupby('연령대')['콜레스테롤'].mean().reset_index()<br>
answer

<br><br>
| 연령대 |  콜레스테롤  |
|:------:|:------------:|
|  20대  | 196.598853   |
|  30대  | 200.362484   |
|  40대  | 196.960751   |
|  50대  | 196.091550   |
|  60대  | 197.436339   |
|  70대  | 202.869849   |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">11-2.</h3>
<h3 style="font-weight:normal;">
혈압, 혈당, 콜레스테롤 컬럼에 대해 **표준화(z-score standardization)**를 적용하여 새로운 컬럼을 추가하시오. (이 때, 혈당은 혈당 전체 평균으로 대치)
표준화된 컬럼명은 혈압_zscore, 혈당_zscore, 콜레스테롤_zscore로 하시오. 이후 전체 데이터에서 혈압_zscore > 1.5를 만족하는 데이터의 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
from scipy.stats import zscore<br><br>

df['혈당'] = df['혈당'].fillna(df['혈당'].mean())<br>
target_col = ['혈압', '혈당', '콜레스테롤']<br>
change_col_name = ['혈압_zscore', '혈당_zscore', '콜레스테롤_zscore']<br><br>

for new_col, col in zip(change_col_name, target_col):<br>
&nbsp;&nbsp;&nbsp;&nbsp;df[new_col] = zscore(df[col])<br><br>

answer = len(df[df['혈압_zscore'] > 1.5])<br>
answer

<br><br>
>68
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">12-1.</h3>
<h3 style="font-weight:normal;">
각 카테고리별, 성별로 평균 주문금액을 계산하시오. (단, 주문금액 결측치는 같은 카테고리, 성별 그룹 평균으로 채운 후 계산하고, 평균값도 결측이면 전체 평균으로 채움)
최종 출력은 카테고리, 성별, 평균 주문금액 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['주문금액'] = df['주문금액'].fillna(df.groupby(['카테고리', '성별'])['주문금액'].transform('mean'))<br>
df['주문금액'] = df['주문금액'].fillna(df['주문금액'].mean())<br><br>

answer = df.groupby(['카테고리', '성별'])['주문금액'].mean().reset_index()<br>
display(answer)

<br><br>
| 카테고리   | 성별 |   주문금액    |
|:----------:|:---:|:-------------:|
| Books      |  F  |  98127.343750 |
| Books      |  M  | 102518.404762 |
| Clothing   |  F  | 101762.051429 |
| Clothing   |  M  |  94524.674419 |
| Electronics|  F  | 104034.844156 |
| Electronics|  M  |  94069.086957 |
| Home       |  F  | 109268.200000 |
| Home       |  M  | 104047.201439 |
| Toys       |  F  | 105476.724138 |
| Toys       |  M  |  98706.119403 |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">12-2.</h3>
<h3 style="font-weight:normal;">
구매수량에 대해 최소-최대 정규화(min-max scaling) 를 적용하여 구매수량_scaled 컬럼을 추가하시오.
이후 구매수량_scaled >= 0.9 를 만족하는 데이터의 개수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
from sklearn.preprocessing import MinMaxScaler<br><br>

minmax = MinMaxScaler()<br><br>

df['구매수량_scaled'] = minmax.fit_transform(df[['구매수량']])<br>
print(len(df[df['구매수량_scaled'] >= 0.9]))

<br><br>
>2
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">13-1.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 불만제기 경험 여부(불만제기 여부가 한 번이라도 1인 경우 "Y", 그렇지 않으면 "N")를 나타내는 파생 컬럼을 생성하시오.
이후 2023년에 불만제기 경험이 "Y"인 고객 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
target = df[df['불만제기여부'] == 1].groupby("고객ID")['고객ID'].unique().index.tolist()<br>
df['불만제기경험여부'] = df['고객ID'].apply(lambda x: 'Y' if x in target else 'N')<br><br>

answer = df[(df['연도'] == 2023) & (df['불만제기경험여부'] == 'Y')]['고객ID'].nunique()<br>
answer

<br><br>
>71
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">13-2.</h3>
<h3 style="font-weight:normal;">
연령대(10대, 20대, 30대, 40대, 50대, 60대 이상) 컬럼을 생성하고, 각 연령대별 주문수량 평균과 주문금액 평균을 구하시오.
최종 출력은 연령대, 평균 주문수량, 평균 주문금액 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['연령대'] = df['나이'].apply(lambda x: str(60) + '대 이상' if x >= 60 else str(x // 10 * 10) + '대')<br>
answer = df.groupby('연령대')[['주문수량', '주문금액']].mean().reset_index()<br>
display(answer)
	
<br><br>
| 연령대     | 주문수량  |   주문금액    |
|:---------:|:--------:|:-------------:|
| 10대      | 1.985714 | 18390.685714  |
| 20대      | 2.022923 | 20046.002865  |
| 30대      | 2.086835 | 19513.540616  |
| 40대      | 2.126582 | 20165.712025  |
| 50대      | 1.953079 | 19724.384164  |
| 60대 이상 | 1.920981 | 20225.686649  |
</details>


<br><br><br><br>


<h3 style="font-weight:normal;">14-1.</h3>
<h3 style="font-weight:normal;">
업무만족도가 결측인 직원은 부서별 평균 업무만족도로 채운다. (단, 부서별 평균도 결측이면 전체 평균으로 채운다)
이후, 근속연수가 결측인 직원은 제거한다.
그 다음, 업무만족도의 사분위수 기준 Q1 이하인 직원 중 성과등급이 A인 직원 수를 구하시오.
</h3>

<details>
<summary>코드</summary>
df['업무만족도'] = df['업무만족도'].fillna(df.groupby('부서')['업무만족도'].transform('mean'))<br>
df['업무만족도'] = df['업무만족도'].fillna(df['업무만족도'].mean())<br>
df1 = df[df['근속연수'].notna()].copy()<br><br>

answer1 = len(df1[(df1['업무만족도'] <= df1['업무만족도'].quantile(0.25)) & (df1['성과등급'] == 'A')])<br>
print(answer1)

<br><br>
>72
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">14-2.</h3>
<h3 style="font-weight:normal;">
근속연수가 10년 이상이고 교육참여횟수가 전체 평균 이상인 직원들을 필터링한 후, 해당 직원들의 부서별 평균 연봉을 구하시오.
이때 연봉 평균이 세 번째로 높은 부서의 평균 연봉을 정수로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df2 = df[(df['근속연수'] >= 10.0) & (df['교육참여횟수'] >= df['교육참여횟수'].mean())].copy()<br>
answer2 = df2.groupby('부서')['연봉'].mean().sort_values(ascending=False).values[2]<br>
print(int(answer2))
	
<br><br>
>6476
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">14-3.</h3>
<h3 style="font-weight:normal;">
각 부서별로 업무만족도 기준 상위 20% 직원들의 평균 근속연수를 계산하시오. (단, 업무만족도가 결측인 직원은 제외하며, 근속연수가 결측인 직원도 제외한다)
이후 가장 평균 근속연수가 높은 부서명을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df3 = df.dropna(subset=['업무만족도', '근속연수']).copy()<br>
target3 = df3.groupby('부서')['업무만족도'].transform(lambda x: x.quantile(0.8))<br>
df3_filtered = df3[df3['업무만족도'] >= target3]<br><br>

answer3 = df3_filtered.groupby('부서')['근속연수'].mean().sort_values(ascending=False).index[0]<br>
print(answer3)

<br><br>
>Finance
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">15-1.</h3>
<h3 style="font-weight:normal;">
각 제품군별로 연도와 분기 기준 평균 반품률(반품수량 / 판매수량)을 구하시오.
최종 출력은 제품군, 연도, 분기, 평균 반품률 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df_grouped = df.groupby(['제품군', '연도', '분기'])[['판매수량', '반품수량']].sum().reset_index()<br>
df_grouped['평균반품률'] = df_grouped['반품수량'] / df_grouped['판매수량']<br><br>

answer1 = df_grouped[['제품군', '연도', '분기', '평균반품률']]<br>
display(answer1)

<br><br>
| 제품군 | 연도 | 분기 | 평균반품률 |
|:-----:|:----:|:---:|:---------:|
| 가전  | 2022 | Q1  | 0.018182  |
| 가전  | 2022 | Q2  | 0.023077  |
| 가전  | 2023 | Q2  | 0.020000  |
| 가전  | 2023 | Q4  | 0.026316  |
| 식품  | 2022 | Q3  | 0.020000  |
| 식품  | 2023 | Q3  | 0.019048  |
| 전자  | 2022 | Q1  | 0.020833  |
| 전자  | 2022 | Q4  | 0.005556  |
| 전자  | 2023 | Q1  | 0.027273  |
| 전자  | 2023 | Q2  | 0.011765  |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">15-2.</h3>
<h3 style="font-weight:normal;">
각 연도별로 지역과 제품군을 기준으로 매출액 총합을 계산하고, 가장 매출이 높은 지역-제품군 조합을 연도별로 1개씩 출력하시오.
최종 출력은 연도, 지역, 제품군, 총 매출액 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df_grouped = df.groupby(['연도', '지역', '제품군'])['매출액'].sum().reset_index()<br>
target3 = df_grouped.groupby('연도')['매출액'].transform(lambda x: x.max())<br>
answer3 = df_grouped.loc[df_grouped['매출액'] == target3, ['연도', '지역', '제품군', '매출액']]<br>
answer3
	
<br><br>
| 연도 | 지역 | 제품군 |   매출액   |
|:----:|:----:|:------:|:----------:|
| 2022 | 서울 | 전자   | 23500000  |
| 2023 | 서울 | 가전   | 17300000  |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">16.</h3>
<h3 style="font-weight:normal;">
다음은 2020~2023년까지의 학교별 과목별 평균 성적 데이터이다.
각 과목별로 4년간 평균 성적이 가장 높은 학교명을 구하고, 최종 출력은 과목, 학교 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
melt = pd.melt(df, id_vars='학교', value_vars=['국어', '수학', '영어', '과학'], var_name='과목', value_name='점수')<br>
grouped = melt.groupby(['학교', '과목'])['점수'].mean().reset_index()<br>
target = grouped.groupby('과목')['점수'].idxmax()<br>
answer = grouped.loc[grouped.index.isin(target), ['과목', '학교']]<br>
answer
	
<br><br>
| 과목 |              학교              |
|:----:|:-----------------------------:|
| 영어 | Dawson, Ruiz and Aguirre      |
| 과학 | Morris, Ross and Santana      |
| 수학 | Parrish-Powell                |
| 국어 | Pierce-Roman                  |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">17-1.</h3>
<h3 style="font-weight:normal;">
구매금액이 결측인 경우, 같은 성별·카테고리 그룹의 평균 구매금액으로 채우고, 해당 그룹 평균도 결측이면 전체 평균 구매금액으로 대체하시오.
그 후, 연령대(10대, 20대, ..., 60대 이상) 를 나누고, 연령대별 평균 구매금액과 평균 리뷰점수를 계산하시오.
최종 출력은 연령대, 평균 구매금액, 평균 리뷰점수 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['구매금액'] = df['구매금액'].fillna(df.groupby(['성별', '카테고리'])['구매금액'].transform('mean'))<br>
df['구매금액'] = df['구매금액'].fillna(df['구매금액'].mean())<br><br>

df['연령대'] = df['연령'].apply(lambda x: str(60) + '대 이상' if (x // 10 * 10) >= 60 else str(x // 10 * 10) + '대')<br>
answer1 = df.groupby('연령대')[['구매금액', '리뷰점수']].agg({'구매금액': 'mean', '리뷰점수': 'mean'}) \
.rename(columns={'구매금액': '평균구매금액', '리뷰점수': '평균리뷰점수'}).reset_index()<br>
answer1

<br><br>
| 연령대   |   평균구매금액    | 평균리뷰점수 |
|:-------:|:----------------:|:------------:|
| 10대    | 228403.518437    | 3.054167     |
| 20대    | 245874.691944    | 2.957692     |
| 30대    | 282802.035068    | 3.110390     |
| 40대    | 220573.732107    | 2.876923     |
| 50대    | 224191.891860    | 2.926263     |
| 60대 이상 | 231402.739518    | 2.947778     |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">17-2.</h3>
<h3 style="font-weight:normal;">
2023년에 한 번이라도 불만제기를 한 고객ID는 ‘불만경험 있음’, 그렇지 않은 경우 ‘불만경험 없음’ 으로 분류하는 파생 컬럼을 만들고,
2023년 불만경험 있음 고객 중 평균 구매수량이 가장 높은 지역을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df2 = df[df['연도'] == 2023].copy()<br>
target2 = df2[df2['불만제기'] != 0]['고객ID'].unique()<br>
df['불만경험여부'] = df['고객ID'].apply(lambda x: True if x in target2 else False)<br><br>

answer2 = df[df['불만경험여부'] == True].groupby('지역')['구매수량'].mean().idxmax()<br>
answer2

<br><br>
>'인천'
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">18-1.</h3>
<h3 style="font-weight:normal;">
구매금액이 결측인 경우 성별-상품군 그룹 평균으로 대체, 남은 결측치는 전체 평균으로 대체하시오.
이후 연령대를 10대 단위로 구분 (20대, 30대, ..., 60대 이상) 하여, 연령대별 평균 구매금액과 평균 리뷰점수를 구하시오.
최종 출력은 연령대, 평균 구매금액, 평균 리뷰점수 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['구매금액'] = df['구매금액'].fillna(df.groupby(['성별', '상품군'])['구매금액'].transform('mean'))<br>
df['구매금액'] = df['구매금액'].fillna(df['구매금액'].mean())<br><br>

df['연령대'] = df['연령'].apply(lambda x: str(x // 10 * 10) + '대 이상' if x >= 60 else str(x // 10 * 10) + '대')<br>
answer1 = df.groupby('연령대')[['구매금액', '리뷰점수']].mean().rename(columns={'구매금액': '평균구매금액', '리뷰점수': '평균리뷰점수'}).reset_index()<br>
display(answer1)

<br><br>
| 연령대   |   평균구매금액    | 평균리뷰점수 |
|:-------:|:----------------:|:------------:|
| 20대    | 57024.448168     | 3.146341     |
| 30대    | 59161.923237     | 3.230088     |
| 40대    | 53897.458934     | 3.152542     |
| 50대    | 53794.968391     | 3.186992     |
| 60대 이상 | 50942.592508     | 3.133333     |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">18-2.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 한 번이라도 반품한 이력이 있으면 'Y', 없으면 'N' 으로 새로운 파생변수를 생성하시오.
그 후, 2023년 가입자 중 'Y'로 분류된 고객의 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['가입년도'] = df['가입일'].str.extract(r'(\d\d\d\d)-')<br>
target = df[df['반품여부'] == 1]['고객ID'].unique().tolist()<br>
df['반품이력'] = df['고객ID'].apply(lambda x: 'Y' if x in target else 'N')<br><br>

answer2 = df[(df['가입년도'] == '2023') & (df['반품이력'] == 'Y')]['고객ID'].nunique()<br>
print(answer2)

<br><br>
>32
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">18-3.</h3>
<h3 style="font-weight:normal;">
상품군별로 리뷰점수가 4점 이상인 데이터만 필터링한 후, 리뷰점수 평균이 가장 높은 상품군 이름을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['리뷰점수'] = df['리뷰점수'].fillna(df['리뷰점수'].mode()[0])<br>
target = df[df['리뷰점수'] >= 4.0][['상품군', '리뷰점수']]<br>
answer3 = target.groupby('상품군')['리뷰점수'].mean().idxmax()<br>
answer3
	
<br><br>
>'도서'
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">19-1.</h3>
<h3 style="font-weight:normal;">
배송만족도가 결측인 경우에는 동일 결제방식 그룹의 배송만족도 평균으로 채우고, 여전히 결측인 경우 전체 평균으로 채운 후,
다음과 같은 기준으로 배송만족도를 기준으로 상·중·하 등급을 부여하시오.	
	4 이상: 상
	3 이상 4 미만: 중
	3 미만: 하

이후, 등급별로 전체 평균 구매금액을 구하고 두 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['배송만족도'] = df['배송만족도'].fillna(df.groupby('결제방식')['배송만족도'].transform('mean'))<br>
df['배송만족도'] = df['배송만족도'].fillna(df['배송만족도'].mean())<br>
df['등급'] = df['배송만족도'].apply(lambda x: '상' if x >= 4.0 else ('중' if x >= 3.0 else '하'))<br><br>

answer1 = df.groupby('등급')['구매금액'].mean().reset_index().rename(columns={'구매금액': '평균구매금액'})<br>
answer1

<br><br>
| 등급 |   평균구매금액   |
|:---:|:---------------:|
| 상  | 154022.003984   |
| 중  | 152182.707692   |
| 하  | 154345.755020   |
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">19-2.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 리뷰를 한 상품 비율을 계산하시오. (리뷰작성=1 이면 리뷰한 것으로 간주)
이후 리뷰 비율이 70% 이상인 고객ID의 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
<span style="color:gray;"># 방법 1: </span><br>
cdf = pd.crosstab(df['고객ID'], df['리뷰작성'], dropna=False).fillna(0)<br>
cdf['리뷰비율'] = cdf[1] / (cdf[0] + cdf[1])<br>
answer1 = len(cdf[cdf['리뷰비율'] >= 0.7])<br>
print("고객 수:", answer1)<br><br>

<span style="color:gray;"># 방법 2: </span><br>
review_stats = df.groupby('고객ID')['리뷰작성'].agg(['sum', 'count']).reset_index()<br>
review_stats['리뷰비율'] = review_stats['sum'] / review_stats['count']<br>
answer2 = len(review_stats[review_stats['리뷰비율'] >= 0.7])<br>
print("고객 수:", answer2)<br><br>

<span style="color:gray;"># 방법 3: </span><br>
pivot = df.pivot_table(index='고객ID', values='리뷰작성', aggfunc=['sum', 'count']).reset_index()<br>
pivot.columns = ['고객ID', 'sum', 'count']<br>
pivot['리뷰비율'] = pivot['sum'] / pivot['count']<br>
answer3 = len(pivot[pivot['리뷰비율'] >= 0.7])<br>
print("고객 수:", answer3)

<br><br>
>고객 수: 47
</details>

<br><br><br><br>

<h3 style="font-weight:normal;">19-3.</h3>
<h3 style="font-weight:normal;">
2023년 데이터만 사용하여, 각 결제방식별로 반품율(반품여부가 1인 비율) 을 구하고, 반품율이 가장 높은 결제방식명을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['구매일'] = pd.to_datetime(df['구매일'])<br>
df['구매년도'] = df['구매일'].dt.year<br><br>

df_filtered = df[df['구매년도'] == 2023]<br>
grouped = df_filtered.groupby('결제방식')['반품여부'].agg(['sum', 'count'])<br>
grouped['반품비율'] = grouped['sum'] / grouped['count']<br>
answer3 = grouped.sort_values(by='반품비율', ascending=False).index[0]<br>
answer3

<br><br>
>'포인트'
</details>

</details>




# 📝 2유형

<details>
<summary><h2>📌 문제 목록 보기</h2></summary>
<h2 style="font-weight:normal;">회귀</h2>
<h3 style="font-weight:normal;">1.</h3> 
<h3 style="font-weight:normal;">1. 훈련 데이터로 학습한 모델을 테스트 데이터에 적용하여 예측한 결과를 제출하시오. (Target: salary, 평가 지표: RMSE)</h3>

train = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/train_employee.csv")<br>
test = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/test_employee.csv")

<details>
<summary>코드</summary>
import pandas as pd<br>
import numpy as np<br>
from sklearn.model_selection import train_test_split<br>
from sklearn.preprocessing import OneHotEncoder<br>
from sklearn.ensemble import RandomForestRegressor<br>
from sklearn.metrics import mean_squared_error<br><br>

<span style="color:gray;"># 데이터 확인</span><br>
<span style="color:gray;"># print(train.head(), "\n")</span><br>
<span style="color:gray;"># print(train.info())</span><br>
<span style="color:gray;"># print(train.isna().sum())</span><br><br>

<span style="color:gray;"># 데이터 분할</span><br>
train_X = train.drop(columns='salary', axis=1)<br>
train_y = train['salary']<br>
test_X = test<br><br>

<span style="color:gray;"># test ID 추출</span><br>
test_ID = test_X['ID']<br><br>

<span style="color:gray;"># 필요 없는 칼럼 제거</span><br>
drop_col = ['ID', 'name']<br>
train_X = train_X.drop(columns=drop_col)<br><br>

<span style="color:gray;"># 검증용 데이터 분할</span><br>
train_X, valid_X, train_y, valid_y = train_test_split(train_X, train_y, test_size=0.25, random_state=42)<br><br>

<span style="color:gray;"># 수치형, 범주형 변수 구분</span><br>
num_columns = train_X.select_dtypes('number').columns.tolist()<br>
cat_columns = train_X.select_dtypes('object').columns.tolist()<br><br>

<span style="color:gray;"># 원-핫 인코딩</span><br>
onehotencoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')<br><br>

train_X_encoder = onehotencoder.fit_transform(train_X[cat_columns])<br>
valid_X_encoder = onehotencoder.transform(valid_X[cat_columns])<br>
test_X_encoder = onehotencoder.transform(test_X[cat_columns])<br><br>

<span style="color:gray;"># 데이터 결합</span><br>
train_X_preprocessed = np.concatenate([train_X_encoder, train_X[num_columns]], axis=1)<br>
valid_X_preprocessed = np.concatenate([valid_X_encoder, valid_X[num_columns]], axis=1)<br>
test_X_preprocessed = np.concatenate([test_X[num_columns], test_X_encoder], axis=1)<br><br>

<span style="color:gray;"># 랜덤포레스트 회귀모형 학습</span><br>
rf = RandomForestRegressor(random_state=42)<br>
rf.fit(train_X_preprocessed, train_y)<br><br>

<span style="color:gray;"># 예측</span><br>
train_predict = rf.predict(train_X_preprocessed)<br>
valid_predict = rf.predict(valid_X_preprocessed)<br><br>

<span style="color:gray;"># 성능 평가</span><br>
train_rmse = np.sqrt(mean_squared_error(train_y, train_predict))<br>
valid_rmse = np.sqrt(mean_squared_error(valid_y, valid_predict))<br><br>

print("train RMSE:", round(train_rmse, 3))<br>
print("valid RMSE:", round(valid_rmse, 3))<br><br>

<span style="color:gray;"># 테스트 데이터 예측 및 제출 파일 생성</span><br>
test_pred = rf.predict(test_X_preprocessed)<br>
result = pd.DataFrame({'ID': test_ID, 'pred': test_pred})<br>
result.to_csv('result.csv', index=False)<br>
print(result.head())<br>
</details>

<h3 style="font-weight:normal;">2.</h3> 
<h3 style="font-weight:normal;">훈련 데이터로 학습한 모델을 테스트 데이터에 적용하여 예측한 결과를 제출하시오. (Target: price, 평가 지표: RMSE)</h3>

train = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/train_apartment.csv")<br>
test = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/test_apartment.csv")

<details>
<summary>코드</summary>
import pandas as pd<br>
import numpy as np<br>
from sklearn.model_selection import train_test_split<br>
from sklearn.preprocessing import OneHotEncoder<br>
from sklearn.impute import SimpleImputer<br>
from sklearn.ensemble import RandomForestRegressor<br>
from sklearn.metrics import mean_squared_error<br><br>

<span style="color:gray;"># 데이터 확인</span><br>
<span style="color:gray;"># print(train.head(), "\n")</span><br>
<span style="color:gray;"># print(train.info())</span><br>
<span style="color:gray;"># print(train.isna().sum())</span><br><br>

<span style="color:gray;"># 종속변수와 독립변수 분리</span><br>
train_X = train.drop(columns='price', axis=1)<br>
train_y = train['price']<br>
test_X = test.copy()<br><br>

<span style="color:gray;"># test ID 추출</span><br>
test_ID = test_X['ID']<br><br>

<span style="color:gray;"># 필요 없는 칼럼 제거</span><br>
drop_col = ['ID']<br>
train_X = train_X.drop(columns=drop_col)<br><br>

<span style="color:gray;"># 수치형, 범주형 변수 구분</span><br>
num_columns = train_X.select_dtypes('number').columns.tolist()<br>
cat_columns = train_X.select_dtypes('object').columns.tolist()<br><br>

<span style="color:gray;"># 검증용 데이터 분할</span><br>
train_X, valid_X, train_y, valid_y = train_test_split(train_X, train_y, test_size=0.2, random_state=42)<br><br>

<span style="color:gray;"># 결측값 평균 대치</span><br>
imputer = SimpleImputer(strategy='mean')<br><br>

train_X[num_columns] = imputer.fit_transform(train_X[num_columns])<br>
valid_X[num_columns] = imputer.transform(valid_X[num_columns])<br>
test_X[num_columns] = imputer.transform(test_X[num_columns])<br><br>

<span style="color:gray;"># 원-핫 인코딩</span><br>
onehotencoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')<br><br>

train_X_encoder = onehotencoder.fit_transform(train_X[cat_columns])<br>
valid_X_encoder = onehotencoder.transform(valid_X[cat_columns])<br>
test_X_encoder = onehotencoder.transform(test_X[cat_columns])<br><br>

<span style="color:gray;"># 데이터 결합</span><br>
train_X_preprocessed = np.concatenate([train_X_encoder, train_X[num_columns]], axis=1)<br>
valid_X_preprocessed = np.concatenate([valid_X_encoder, valid_X[num_columns]], axis=1)<br>
test_X_preprocessed = np.concatenate([test_X_encoder, test_X[num_columns]], axis=1)<br><br>

<span style="color:gray;"># 랜덤포레스트 회귀모형 학습</span><br>
rf = RandomForestRegressor(random_state=42)<br>
rf.fit(train_X_preprocessed, train_y)<br><br>

<span style="color:gray;"># 예측</span><br>
train_predict = rf.predict(train_X_preprocessed)<br>
valid_predict = rf.predict(valid_X_preprocessed)<br><br>

<span style="color:gray;"># 성능 평가</span><br>
train_rmse = np.sqrt(mean_squared_error(train_y, train_predict))<br>
valid_rmse = np.sqrt(mean_squared_error(valid_y, valid_predict))<br><br>

print("train RMSE:", round(train_rmse, 3))<br>
print("valid RMSE:", round(valid_rmse, 3))<br><br>

<span style="color:gray;"># 테스트 데이터 예측 및 제출 파일 생성</span><br>
test_pred = rf.predict(test_X_preprocessed)<br>
result = pd.DataFrame({'ID': test_ID, 'pred': test_pred})<br>
result.to_csv('result.csv', index=False)<br>
print(result.head())<br>
</details>


<h3 style="font-weight:normal;">3.</h3> 
<h3 style="font-weight:normal;">훈련 데이터로 학습한 모델을 테스트 데이터에 적용하여 예측한 결과를 제출하시오. (Target: mpg, 평가 지표: MAE) (결측값은 0으로 나타냄) </h3>

train = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/train_fuel.csv")<br>
test = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/test_fuel.csv")

<details>
<summary>코드</summary>
import pandas as pd<br>
import numpy as np<br>
from sklearn.model_selection import train_test_split<br>
from sklearn.preprocessing import OneHotEncoder<br>
from sklearn.ensemble import RandomForestRegressor<br>
from sklearn.metrics import mean_absolute_error<br><br>

<span style="color:gray;"># 데이터 확인</span><br>
<span style="color:gray;"># print(train.head(), "\n")</span><br>
<span style="color:gray;"># print(train.info())</span><br><br>

<span style="color:gray;"># 결측치 있는 칼럼 확인</span><br>
cols_with_zero = [col for col in train.columns if (train[col] == 0).any()]<br>
print(cols_with_zero)<br><br>

<span style="color:gray;"># 결측값 대치</span><br>
for col in cols_with_zero:<br>
&nbsp;&nbsp;&nbsp;&nbsp;train[col] = train[col].apply(lambda x: train[col].median() if x == 0 else x)<br>
&nbsp;&nbsp;&nbsp;&nbsp;test[col] = test[col].apply(lambda x: test[col].median() if x == 0 else x)<br><br>

<span style="color:gray;"># 종속변수와 독립변수 분리</span><br>
train_X = train.drop(columns='mpg', axis=1)<br>
train_y = train['mpg']<br>
test_X = test.copy()<br><br>

<span style="color:gray;"># test ID 추출</span><br>
test_ID = test_X['ID']<br><br>

<span style="color:gray;"># 필요 없는 칼럼 제거</span><br>
drop_col = ['ID']<br>
train_X = train_X.drop(columns=drop_col)<br><br>

<span style="color:gray;"># 수치형, 범주형 변수 구분</span><br>
num_columns = train_X.select_dtypes('number').columns.tolist()<br>
cat_columns = train_X.select_dtypes('object').columns.tolist()<br><br>

<span style="color:gray;"># 검증용 데이터 분할</span><br>
train_X, valid_X, train_y, valid_y = train_test_split(train_X, train_y, test_size=0.2, random_state=42)<br><br>

<span style="color:gray;"># 원-핫 인코딩</span><br>
onehotencoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')<br><br>

train_X_encoder = onehotencoder.fit_transform(train_X[cat_columns])<br>
valid_X_encoder = onehotencoder.transform(valid_X[cat_columns])<br>
test_X_encoder = onehotencoder.transform(test_X[cat_columns])<br><br>

<span style="color:gray;"># 데이터 결합</span><br>
train_X_preprocessed = np.concatenate([train_X_encoder, train_X[num_columns]], axis=1)<br>
valid_X_preprocessed = np.concatenate([valid_X_encoder, valid_X[num_columns]], axis=1)<br>
test_X_preprocessed = np.concatenate([test_X_encoder, test_X[num_columns]], axis=1)<br><br>

<span style="color:gray;"># 랜덤포레스트 회귀모형 학습</span><br>
rf = RandomForestRegressor(random_state=42)<br>
rf.fit(train_X_preprocessed, train_y)<br><br>

<span style="color:gray;"># 예측</span><br>
train_predict = rf.predict(train_X_preprocessed)<br>
valid_predict = rf.predict(valid_X_preprocessed)<br><br>

<span style="color:gray;"># 성능 평가</span><br>
train_rmse = mean_absolute_error(train_y, train_predict)<br>
valid_rmse = mean_absolute_error(valid_y, valid_predict)<br><br>

print("train RMSE:", round(train_rmse, 3))<br>
print("valid RMSE:", round(valid_rmse, 3))<br><br>

<span style="color:gray;"># 테스트 데이터 예측 및 제출 파일 생성</span><br>
test_pred = rf.predict(test_X_preprocessed)<br>
result = pd.DataFrame({'ID': test_ID, 'pred': test_pred})<br>
result.to_csv('result.csv', index=False)<br>
print(result.head())<br>
</details>

<h2 style="font-weight:normal;">분류</h2>
<h3 style="font-weight:normal;">1.</h3> 
<h3 style="font-weight:normal;">훈련 데이터로 학습한 모델을 테스트 데이터에 적용하여 예측한 결과를 제출하시오. (Target: churn, 평가 지표: AUC)</h3> 

train = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/train_customer.csv")<br>
test = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/test_customer.csv")

<details>
<summary>코드</summary>
import pandas as pd<br>
import numpy as np<br>
from sklearn.model_selection import train_test_split<br>
from sklearn.preprocessing import OneHotEncoder<br>
from sklearn.ensemble import RandomForestClassifier<br>
from sklearn.metrics import roc_auc_score<br><br>

<span style="color:gray;"># 데이터 확인</span><br>
<span style="color:gray;"># print(train.head(), "\n")</span><br>
<span style="color:gray;"># print(train.info())</span><br>
<span style="color:gray;"># print(train['churn'].value_counts(), "\n")</span><br><br>

<span style="color:gray;"># 종속변수와 독립변수 분리</span><br>
train_X = train.drop(columns='churn', axis=1)<br>
train_y = train['churn']<br>
test_X = test.copy()<br><br>

<span style="color:gray;"># 필요 없는 칼럼 제거</span><br>
drop_col = ['ID']<br>
train_X = train_X.drop(columns=drop_col)<br><br>

<span style="color:gray;"># 수치형, 범주형 변수 구분</span><br>
num_columns = train_X.select_dtypes('number').columns.tolist()<br>
cat_columns = train_X.select_dtypes('object').columns.tolist()<br><br>

<span style="color:gray;"># 검증용 데이터 분할</span><br>
train_X, valid_X, train_y, valid_y = train_test_split(train_X, train_y, test_size=0.2, random_state=42)<br><br>

<span style="color:gray;"># 원-핫 인코딩</span><br>
onehotencoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')<br><br>

train_X_encoder = onehotencoder.fit_transform(train_X[cat_columns])<br>
valid_X_encoder = onehotencoder.transform(valid_X[cat_columns])<br>
test_X_encoder = onehotencoder.transform(test_X[cat_columns])<br><br>

<span style="color:gray;"># 데이터 결합</span><br>
train_X_preprocessed = np.concatenate([train_X_encoder, train_X[num_columns]], axis=1)<br>
valid_X_preprocessed = np.concatenate([valid_X_encoder, valid_X[num_columns]], axis=1)<br>
test_X_preprocessed = np.concatenate([test_X_encoder, test_X[num_columns]], axis=1)<br><br>

<span style="color:gray;"># 랜덤포레스트 분류모델 학습</span><br>
rf = RandomForestClassifier(random_state=42)<br>
rf.fit(train_X_preprocessed, train_y)<br><br>

<span style="color:gray;"># 예측</span><br>
valid_pred_proba = rf.predict_proba(valid_X_preprocessed)[:, 1]<br><br>

<span style="color:gray;"># AUC 평가</span><br>
auc = roc_auc_score(valid_y, valid_pred_proba)<br>
print("Validation AUC:", round(auc, 3))<br><br>

<span style="color:gray;"># 테스트 데이터 예측 및 제출 파일 생성</span><br>
test_pred = rf.predict(test_X_preprocessed)<br>
result = pd.DataFrame({'ID': test['ID'], 'pred': test_pred})<br>
result.to_csv('result.csv', index=False)<br>
print(result.head())<br>
</details>


<h3 style="font-weight:normal;">2.</h3> 
<h3 style="font-weight:normal;">훈련 데이터로 학습한 모델을 테스트 데이터에 적용하여 예측한 결과를 제출하시오. (Target: default, 평가 지표: f1_score)</h3> 

train = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/train_credit.csv")<br>
test = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/test_credit.csv")

<details>
<summary>코드</summary>
import pandas as pd<br>
import numpy as np<br>
from sklearn.model_selection import train_test_split<br>
from sklearn.preprocessing import OneHotEncoder<br>
from sklearn.ensemble import RandomForestClassifier<br>
from sklearn.metrics import f1_score<br><br>

<span style="color:gray;"># 데이터 확인</span><br>
<span style="color:gray;"># print(train.head(), "\n")</span><br>
<span style="color:gray;"># print(train.info())</span><br>
<span style="color:gray;"># print(train.isna().sum())</span><br>
<span style="color:gray;"># print(train['default'].value_counts(), "\n")</span><br><br>

<span style="color:gray;"># default 그룹별로 결측값 대치</span><br>
means = train.groupby('default')['credit_score'].mean()<br>
train['credit_score'] = train.apply(lambda x: means[x['default']] if pd.isna(x['credit_score']) else x['credit_score'], axis=1)<br><br>

<span style="color:gray;"># 종속변수와 독립변수 분리</span><br>
train_X = train.drop(columns='default', axis=1)<br>
train_y = train['default']<br>
test_X = test.copy()<br><br>

<span style="color:gray;"># 필요 없는 칼럼 제거</span><br>
drop_col = ['ID']<br>
train_X = train_X.drop(columns=drop_col)<br><br>

<span style="color:gray;"># 수치형, 범주형 변수 구분</span><br>
num_columns = train_X.select_dtypes('number').columns.tolist()<br>
cat_columns = train_X.select_dtypes('object').columns.tolist()<br><br>

<span style="color:gray;"># 검증용 데이터 분할</span><br>
train_X, valid_X, train_y, valid_y = train_test_split(train_X, train_y, test_size=0.25, random_state=42)<br><br>

<span style="color:gray;"># 원-핫 인코딩</span><br>
onehotencoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')<br><br>

train_X_encoder = onehotencoder.fit_transform(train_X[cat_columns])<br>
valid_X_encoder = onehotencoder.transform(valid_X[cat_columns])<br>
test_X_encoder = onehotencoder.transform(test_X[cat_columns])<br><br>

<span style="color:gray;"># 데이터 결합</span><br>
train_X_preprocessed = np.concatenate([train_X_encoder, train_X[num_columns]], axis=1)<br>
valid_X_preprocessed = np.concatenate([valid_X_encoder, valid_X[num_columns]], axis=1)<br>
test_X_preprocessed = np.concatenate([test_X_encoder, test_X[num_columns]], axis=1)<br><br>

<span style="color:gray;"># 랜덤포레스트 분류모델 학습</span><br>
rf = RandomForestClassifier(random_state=42)<br>
rf.fit(train_X_preprocessed, train_y)<br><br>

<span style="color:gray;"># 예측</span><br>
valid_pred = rf.predict(valid_X_preprocessed)<br><br>

<span style="color:gray;"># F1-score 평가</span><br>
f1 = f1_score(valid_y, valid_pred, average='macro')<br>
print("Validation F1-score:", round(f1, 3))<br><br>

<span style="color:gray;"># 테스트 데이터 예측 및 제출 파일 생성</span><br>
test_pred_proba = rf.predict_proba(test_X_preprocessed)[:, 1]<br>
result = pd.DataFrame({'ID': test['ID'], 'pred_proba': test_pred_proba})<br>
result.to_csv('result.csv', index=False)<br>
print(result.head())<br>
</details>

<h3 style="font-weight:normal;">3.</h3> 
<h3 style="font-weight:normal;">훈련 데이터로 학습한 모델을 테스트 데이터에 적용하여 예측한 결과를 제출하시오. (Target: satisfaction, 평가 지표: recall)<br>
그리고 평가 지표 점수가 0.95 이상이 나오게 모델을 학습하시오.</h3> 

train = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/train_satisfaction.csv")<br>
test = pd.read_csv("https://raw.githubusercontent.com/pigbbong/bigdata-exam-practice/main/type2/test_satisfaction.csv")

<details>
<summary>코드</summary>
import pandas as pd<br>
import numpy as np<br>
from sklearn.model_selection import train_test_split<br>
from sklearn.preprocessing import OneHotEncoder<br>
from sklearn.ensemble import RandomForestClassifier<br>
from sklearn.metrics import recall_score<br><br>

<span style="color:gray;"># 데이터 확인</span><br>
<span style="color:gray;"># print(train.head(), "\n")</span><br>
<span style="color:gray;"># print(train.info())</span><br>
<span style="color:gray;"># print(train.isna().sum())</span><br>
<span style="color:gray;"># print(train['satisfaction'].value_counts(), "\n")</span><br><br>

<span style="color:gray;"># 종속변수와 독립변수 분리</span><br>
train_X = train.drop(columns='satisfaction', axis=1)<br>
train_y = train['satisfaction']<br>
test_X = test.copy()<br><br>

<span style="color:gray;"># 필요 없는 칼럼 제거</span><br>
drop_col = ['ID']<br>
train_X = train_X.drop(columns=drop_col)<br><br>

<span style="color:gray;"># 수치형, 범주형 변수 구분</span><br>
num_columns = train_X.select_dtypes('number').columns.tolist()<br>
cat_columns = train_X.select_dtypes('object').columns.tolist()<br><br>

<span style="color:gray;"># 검증용 데이터 분할 (층화 적용)</span><br>
train_X, valid_X, train_y, valid_y = train_test_split(train_X, train_y, test_size=0.2, random_state=42, stratify=train_y)<br><br>

<span style="color:gray;"># 원-핫 인코딩</span><br>
onehotencoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')<br><br>

train_X_encoder = onehotencoder.fit_transform(train_X[cat_columns])<br>
valid_X_encoder = onehotencoder.transform(valid_X[cat_columns])<br>
test_X_encoder = onehotencoder.transform(test_X[cat_columns])<br><br>

<span style="color:gray;"># 데이터 결합</span><br>
train_X_preprocessed = np.concatenate([train_X_encoder, train_X[num_columns]], axis=1)<br>
valid_X_preprocessed = np.concatenate([valid_X_encoder, valid_X[num_columns]], axis=1)<br>
test_X_preprocessed = np.concatenate([test_X_encoder, test_X[num_columns]], axis=1)<br><br>

<span style="color:gray;"># 랜덤포레스트 분류모델 학습</span><br>
rf = RandomForestClassifier(random_state=42)<br>
rf.fit(train_X_preprocessed, train_y)<br><br>

<span style="color:gray;"># 예측</span><br>
valid_pred = rf.predict(valid_X_preprocessed)<br><br>

<span style="color:gray;"># Recall 평가</span><br>
recall = recall_score(valid_y, valid_pred, average='macro')<br>
print("Recall:", round(recall, 3))<br><br>

<span style="color:gray;"># 테스트 데이터 예측 및 제출 파일 생성</span><br>
test_pred = rf.predict(test_X_preprocessed)<br>
result = pd.DataFrame({'ID': test['ID'], 'pred': test_pred})<br>
result.to_csv('result.csv', index=False)<br>
print(result.head())<br>
</details>

</details>









# 📝 3유형

<details>
<summary><h2>📌 문제 목록 보기</h2></summary>


<h3 style="font-weight:normal;">1-1.</h3>  
<h3 style="font-weight:normal;">  
연령대별로 질병유무와의 독립성 여부를 검정하시오.  
<br>  
검정 방법을 선택하여 적절한 검정을 수행하고, 검정 결과를 해석하시오. (유의수준은 0.05로 가정한다)  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import chi2_contingency<br><br>
cdf = pd.crosstab(df['연령대'], df['질병유무'])<br>
chi2_stats, p, _, _ = chi2_contingency(cdf)<br>
print("검정통계량:", chi2_stats)<br>
print("p_values:", p)<br><br>
if p < 0.05:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;print("귀무가설 기각")<br>
else:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;print("귀무가설 채택")

<br><br>

> 검정통계량: 24.71328589749828<br>
> p_values: 1.7725414243948196e-05<br>
> 귀무가설 기각
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">1-2.</h3>  
<h3 style="font-weight:normal;">  
연령대, 성별, 교육수준을 독립변수로 설정하고 다중회귀모형을 구축하고,  
<br>  
유의미한 영향을 주는 변수의 개수와 R-squared 값을 구하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import ols<br><br>
model = ols("질병유무 ~ C(연령대) + C(성별) + C(교육수준)", data=df).fit()<br><br>
pvalues = model.pvalues[1:]<br><br>
print(f"유의미한 영향을 주는 변수의 개수: {len(pvalues[pvalues < 0.05])}개")<br>
print("R-squared:", model.rsquared)

<br><br>
> 유의미한 영향을 주는 변수의 개수: 2개<br>
> R-squared: 0.09380851372083054
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">2-1.</h3>  
<h3 style="font-weight:normal;">  
직업군에 따라 만성질환유무와의 독립성 여부를 검정하고,  
<br>  
적절한 검정을 수행하고, 검정 결과를 해석하시오. (유의수준은 0.05로 가정한다)  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import chi2_contingency<br><br>
cdf = pd.crosstab(df['직업군'], df['만성질환유무'])<br>
chi2_stats, p, ddof, expected = chi2_contingency(cdf)<br><br>
if p < 0.05:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("직업군에 따른 만성질환유무는 서로 연관되어있음")<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("직업군에 따라 만성질환유무는 서로 관계가 없음")

<br><br>
> 직업군에 따른 만성질환유무는 서로 연관되어있음
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">2-2.</h3>  
<h3 style="font-weight:normal;">  
수면시간을 종속변수로 설정하고 직업군, 결혼여부, 운동빈도, 스트레스수준을 독립변수로 설정한 다중회귀모형을 구축하시오.  
<br>  
그리고 유의미한 영향을 주는 변수의 개수를 구하고, R-squared 값을 구하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import ols<br><br>
model = ols("수면시간 ~ C(직업군) + C(결혼여부) + C(운동빈도) + 스트레스수준", data=df).fit()<br><br>
pvalues = model.pvalues[1:]<br><br>
print(f"모델에 유의미한 영향을 주는 변수의 개수: {len(list(pvalues[pvalues < 0.05]))}개\n")<br>
print("R-squared:", model.rsquared)

<br><br>
> 모델에 유의미한 영향을 주는 변수의 개수: 0개<br>
> R-squared: 0.01919543404363533
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">2-3.</h3>  
<h3 style="font-weight:normal;">  
만성질환유무를 예측하는 로지스틱 회귀모형을 구축하시오. (직업군, 결혼여부, 운동빈도, 수면시간, 스트레스수준을 독립변수로 설정하시오.)  
<br>  
그리고 모형의 전반적 유의성 검정을 위해 LR 검정(Likelihood Ratio Test) 결과를 제시하고, 통계적 유의성을 평가하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import logit<br><br>
model = logit("만성질환유무 ~ C(직업군) + C(결혼여부) + C(운동빈도) + 스트레스수준", data=df).fit()<br><br>
print("LR Test Statistics:", -2 * (model.llnull - model.llf))<br>
print("LR Test p-value:", model.llr_pvalue)

<br><br>
> LR Test Statistics: 44.45408393664076<br>
> LR Test p-value: 4.669023162177591e-07
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">3-1.</h3>  
<h3 style="font-weight:normal;">  
스마트폰사용시간에 영향을 미치는 요인을 분석하는 회귀모형을 구축하시오. (독립변수: 지역, 평일_인터넷사용시간, 주말_인터넷사용시간)  
<br>  
그리고 다음 값을 출력하시오:  
</h3>  

<h4 style="font-weight:normal;">- 회귀계수(β) 값 전체</h4>  
<h4 style="font-weight:normal;">- p-value 값 전체</h4>  
<h4 style="font-weight:normal;">- 유의미한 변수의 개수</h4>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import ols<br><br>
model = ols("스마트폰사용시간 ~ C(지역) + 평일_인터넷사용시간 + 주말_인터넷사용시간", data=df).fit()<br><br>
coef = model.params[1:]<br>
pvalues = model.pvalues[1:]<br><br>
print("회귀계수 값:")<br>
print(coef)<br>
print("\n")<br>
print("p-values:")<br>
print(pvalues)<br>
print("\n")<br>
print("유의미한 변수의 개수:")<br>
print(len(pvalues[pvalues < 0.05]))

<br><br>

>회귀계수 값: <br>
>C(지역)[T.광주]   -0.002578 <br>
>C(지역)[T.대구]   -0.203613 <br>
>C(지역)[T.부산]   -0.085643 <br>
>C(지역)[T.서울]   -0.083842 <br>
>평일_인터넷사용시간    -0.026694 <br>
>주말_인터넷사용시간     0.042047 <br><br>

>p-values: <br>
>C(지역)[T.광주]    0.989130 <br>
>C(지역)[T.대구]    0.273061 <br>
>C(지역)[T.부산]    0.646854 <br>
>C(지역)[T.서울]    0.668706 <br>
>평일_인터넷사용시간     0.651553 <br>
>주말_인터넷사용시간     0.331137 <br><br>

>유의미한 변수의 개수: 0 <br>
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">3-2.</h3>  
<h3 style="font-weight:normal;">  
소셜미디어이용 여부를 예측하는 로지스틱 회귀모형을 구축하시오. (독립변수: 지역, 평일_인터넷사용시간, 주말_인터넷사용시간, 스마트폰사용시간)  
<br>  
그리고 다음 값을 출력하시오:  
</h3>  

<h4 style="font-weight:normal;">- 회귀계수(β) 값 전체</h4>  
<h4 style="font-weight:normal;">- p-value 값 전체</h4>  
<h4 style="font-weight:normal;">- 유의미한 변수의 개수</h4>  
<h4 style="font-weight:normal;">- 각 변수의 오즈비(odds ratio) 값 전체 (exp(β)로 계산)</h4>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import logit<br>
import numpy as np<br><br>
model = logit("소셜미디어이용 ~ C(지역) + 평일_인터넷사용시간 + 주말_인터넷사용시간 + 스마트폰사용시간", data=df).fit()<br><br>
coef = model.params[1:]<br>
pvalues = model.pvalues[1:]<br><br>
print("회귀계수 값:")<br>
print(coef)<br>
print("\n")<br>
print("p-values:")<br>
print(pvalues)<br>
print("\n")<br>
print("유의미한 변수의 개수:")<br>
print(len(pvalues[pvalues < 0.05]))<br>
print("\n")<br>
print("오즈비:")<br>
print(np.exp(model.params[1:]))

<br><br>
>회귀계수 값: <br>
>C(지역)[T.광주]   -0.159362 <br>
>C(지역)[T.대구]    0.082727 <br>
>C(지역)[T.부산]    0.239479 <br>
>C(지역)[T.서울]   -0.716144 <br>
>평일_인터넷사용시간     0.042339 <br>
>주말_인터넷사용시간     0.333166 <br>
>스마트폰사용시간       0.486477 <br><br>



>p-values: <br>
>C(지역)[T.광주]    0.671304 <br>
>C(지역)[T.대구]    0.820795 <br>
>C(지역)[T.부산]    0.509214 <br>
>C(지역)[T.서울]    0.089156 <br>
>평일_인터넷사용시간     0.723037 <br>
>주말_인터넷사용시간     0.000183 <br>
>스마트폰사용시간       0.000006 <br><br>



>유의미한 변수의 개수: 2 <br><br>


>오즈비: <br>
>C(지역)[T.광주]    0.852688 <br>
>C(지역)[T.대구]    1.086245 <br>
>C(지역)[T.부산]    1.270587 <br>
>C(지역)[T.서울]    0.488633 <br>
>평일_인터넷사용시간     1.043248 <br>
>주말_인터넷사용시간     1.395379 <br>
>스마트폰사용시간       1.626576 

</details>  

<br><br><br><br>



<h3 style="font-weight:normal;">4-1.</h3>  
<h3 style="font-weight:normal;">  
성별에 따른 평균 체중에 유의미한 차이가 있는지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  

from scipy.stats import shapiro, levene, ttest_ind, mannwhitneyu<br><br>
male = df[df['성별'] == '남']['체중'].reset_index(drop=True)<br>
female = df[df['성별'] == '여']['체중'].reset_index(drop=True)<br><br>
male_stat, male_p = shapiro(male)<br>
female_stat, female_p = shapiro(female)<br><br>
if (male_p >= 0.05) & (female_p >= 0.05):<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("두 집단이 정규성을 만족함")<br><br>
    &nbsp;&nbsp;&nbsp;&nbsp;l_stat, l_p = levene(male, female)<br><br>
    &nbsp;&nbsp;&nbsp;&nbsp;if l_p >= 0.05:<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("두 집단의 등분산성이 만족되므로 독립 t-검정을 시행함")<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;t_stat, t_p = ttest_ind(male, female)<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("검정통계량:", t_stat)<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("pvalues:", t_p)<br>
    &nbsp;&nbsp;&nbsp;&nbsp;else:<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("두 집단이 등분산성을 만족하지 않으므로 Welch's t-검정일 시행함")<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;t_stat, t_p = ttest_ind(male, female, equal_var=False)<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("검정통계량:", t_stat)<br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("pvalues:", t_p)<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("두 집단 중 정규성을 만족하지 않은 집단이 있으므로 비모수 검정을 시행함")<br>
    &nbsp;&nbsp;&nbsp;&nbsp;u_stat, u_p = mannwhitneyu(male, female)<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("검정통계량:", u_stat)<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("pvalues:", u_p)

<br><br>

> 두 집단이 정규성을 만족함<br>
> 두 집단의 등분산성이 만족되므로 독립 t-검정을 시행함<br>
> 검정통계량: 1.208913892570682<br>
> pvalues: 0.227654240467682
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">4-2.</h3>  
<h3 style="font-weight:normal;">  
흡연여부와 운동여부가 독립적인지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import chi2_contingency<br><br>
cdf = pd.crosstab(df['흡연여부'], df['운동여부'])<br>
chi2_stats, chi2_p, ddof, expected = chi2_contingency(cdf)<br>
print("Statistics:", chi2_stats)<br>
print("p-values:", chi2_p)

<br><br>
>Statistics: 3.052418082722308<br>
>p-values: 0.08061703166032919
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">5-1.</h3>  
<h3 style="font-weight:normal;">  
학력수준에 따라 직무만족도의 평균에 차이가 있는지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import shapiro, levene, f_oneway<br><br>
for group in df['학력수준'].unique():<br>
    stat, p = shapiro(df[df['학력수준'] == group]['직무만족도'])<br>
    print(f"[{group}] 정규성검정 p-value: {p}")<br><br>

groups = [df[df['학력수준'] == g]['직무만족도'] for g in df['학력수준'].unique()]<br>
stat, p = levene(*groups)<br>
print("등분산성 검정 p-value:", p)<br><br>
stat, p = f_oneway(*groups)<br>
print("ANOVA stats:", stat)<br>
print("ANOVA p-value:", p)

<br><br>
>[고졸] 정규성검정 p-value: 0.28955228892252133<br>
>[대학원졸] 정규성검정 p-value: 0.6482530202159127<br>
>[대졸] 정규성검정 p-value: 0.10586676249673443<br>
>등분산성 검정 p-value: 0.35191382138446<br>
>ANOVA stats: 0.17646311829870298<br>
>ANOVA p-value: 0.8383048677370049
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">5-2.</h3>  
<h3 style="font-weight:normal;">  
재택근무 여부와 이직 경험 여부가 독립적인지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import chi2_contingency<br><br>
cdf = pd.crosstab(df['재택근무여부'], df['이직경험여부'])<br>
chi2_stats, p, ddof, expected = chi2_contingency(cdf)<br><br>
print("chi2_stats:", chi2_stats)<br>
print("p-value:", p)

<br><br>
>chi2_stats: 0.0<br>
>p-value: 1.0<br>
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">5-3.</h3>  
<h3 style="font-weight:normal;">  
주당근무시간이 정규성을 만족하는지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import shapiro<br><br>
stat, p = shapiro(df['주당근무시간'])<br>
print("statictis:", stat)<br>
print("p-value:", p)<br><br>
if p < 0.05:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("정규성 불만족")<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("정규성 만족")

<br><br>
>statictis: 0.9935192674208136<br>
>p-value: 0.13854889195838865
>정규성 만족
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">6-1.</h3>  
<h3 style="font-weight:normal;">  
업무성과 점수를 종속변수로 설정하고, 독립변수로 부서, 직급, 근속연수, 주당근무시간을 사용하여 다중회귀모형을 구축하시오.  
<br>  
그리고 다음 값들을 출력하시오:  
</h3>  

<h4 style="font-weight:normal;">- 회귀계수(β) 값 전체 출력</h4>  
<h4 style="font-weight:normal;">- p-value 값 전체 출력</h4>  
<h4 style="font-weight:normal;">- 유의미한 변수의 개수 출력 (유의 수준은 0.05로 설정)</h4>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import ols<br><br>
model = ols("업무성과점수 ~ C(부서) + C(직급) + 근속연수 + 주당근무시간", data=df).fit()<br><br>
print("회귀계수 값:")<br>
print(model.params[1:])<br>
print("\n")<br>
print("p-value 값:")<br>
print(model.pvalues[1:])<br>
print("\n")<br>
pvalues = model.pvalues[1:]<br>
significant_pvalue = len(pvalues[pvalues < 0.05])<br>
print("유의미한 변수 개수:", significant_pvalue)

<br><br>
>회귀계수 값:<br>
>C(부서)[T.영업]    3.860968<br>
>C(부서)[T.인사]   -0.487387<br>
>C(부서)[T.재무]    0.215100<br>
>C(직급)[T.대리]   -4.393583<br>
>C(직급)[T.부장]    3.177155<br>
>C(직급)[T.사원]   -3.560375<br>
>C(직급)[T.차장]    1.359326<br>
>근속연수           1.402535<br>
>주당근무시간         0.824601<br><br>


>p-value 값:<br>
>C(부서)[T.영업]    1.453251e-07<br>
>C(부서)[T.인사]    5.003544e-01<br>
>C(부서)[T.재무]    7.583646e-01<br>
>C(직급)[T.대리]    2.328691e-08<br>
>C(직급)[T.부장]    1.061265e-04<br>
>C(직급)[T.사원]    1.146126e-05<br>
>C(직급)[T.차장]    8.517250e-02<br>
>근속연수           8.578075e-44<br>
>주당근무시간         1.005734e-45<br><br>


>유의미한 변수 개수: 6
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">6-2.</h3>  
<h3 style="font-weight:normal;">  
위에서 구축한 OLS 회귀모형의 잔차(residuals)가 정규성을 만족하는지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import shapiro<br><br>
residuals = model.resid<br>
stat, p = shapiro(residuals)<br><br>
if p < 0.05:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("정규분포 불만족")<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("정규분포 만족")<br><br>
print("statistics:", stat)<br>
print("p-value:", p)

<br><br>
>정규분포 만족<br>
>statistics: 0.9974856994141608<br>
>p-value: 0.809330150039685
</details>  

<br><br><br><br>



<h3 style="font-weight:normal;">7.</h3>  
<h3 style="font-weight:normal;">  
두 식이요법 그룹 간 공복 혈당의 평균 차이가 통계적으로 유의한지를 판단하기 위한 적절한 검정을 수행하고,  
<br>  
그때의 검정통계량 값을 구하여라.  
<br>  
(단, 양측 검정을 수행하고, 신뢰수준은 95%로 하며, 소수 셋째 자리까지 반올림할 것)  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import shapiro, levene, mannwhitneyu, ttest_ind<br><br>
group1 = df[df['DietGroup'] == 'A']['Glucose']<br>
group2 = df[df['DietGroup'] == 'B']['Glucose']<br><br>
<span style="color:gray;"># 정규성 검정</span><br>
stat1, p1 = shapiro(group1)<br>
stat2, p2 = shapiro(group2)<br><br>
if p1 >= 0.05 and p2 >= 0.05:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("두 집단의 정규성이 만족됨")<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("정규성을 만족하지 않는 집단이 있음")<br><br>
<span style="color:gray;"># 등분산성 검정</span><br>
l_stat, l_p = levene(group1, group2)<br><br>
if l_p >= 0.05:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("두 집단의 등분산성을 이루고 있음")<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("두 집단이 등분산성을 만족하지 않음")<br><br>
<span style="color:gray;"># 독립표본 t-검정 시행</span><br>
t_stat, t_p = ttest_ind(group2, group1)<br><br>
print(t_stat.round(3))

<br><br>
>두 집단의 정규성이 만족됨<br>
>두 집단의 등분산성을 이루고 있음<br>
>3.981
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">8.</h3>  
<h3 style="font-weight:normal;">  
세 가지 마케팅 전략 간 고객 지출 금액에 차이가 있는지를 검정하고,  
<br>  
그때의 분산분석 F-통계량 값을 소수 셋째 자리까지 반올림하여 출력하시오.  
<br>  
(신뢰수준 95%, 양측 검정)  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import f_oneway<br><br>
group1 = df[df['Strategy'] == 'A']['Spending']<br>
group2 = df[df['Strategy'] == 'B']['Spending']<br>
group3 = df[df['Strategy'] == 'C']['Spending']<br><br>
f_stats, f_p = f_oneway(group1, group2, group3)<br>
print("F-통계량:", f_stats.round(3))<br>
print("pvalue:", f_p.round(3))

<br><br>
>F-통계량: 10.938<br>
>pvalue: 0.0
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">9.</h3>  
<h3 style="font-weight:normal;">  
마케팅 전략, 연령대, 그리고 이들의 상호작용 효과가 고객 지출 금액에 유의한 영향을 주는지를 검정하시오.  
<br>  
각 요인의 분산분석 F-통계량 값을 소수 셋째 자리까지 반올림하여 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
import statsmodels.api as sm<br>
from statsmodels.formula.api import ols<br><br>
<span style="color:gray;"># 상호작용 포함 모델</span><br>
model = ols("Spending ~ C(Strategy) * C(AgeGroup)", data=df).fit()<br><br>
<span style="color:gray;"># 분산분석 테이블</span><br>
anova = sm.stats.anova_lm(model, typ=2)<br><br>
<span style="color:gray;"># F-통계량 추출 및 출력</span><br>
print("C(Strategy) statistics:", anova.loc['C(Strategy)', 'F'].round(3))<br>
print("C(AgeGroup) statistics:", anova.loc['C(AgeGroup)', 'F'].round(3))<br>
print("Interaction statistics:", anova.loc['C(Strategy):C(AgeGroup)', 'F'].round(3))

<br><br>
>C(Strategy) statistics: 14.629<br>
>C(AgeGroup) statistics: 2.19<br>
>Interaction statistics: 1.914
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">10-1.</h3>  
<h3 style="font-weight:normal;">  
신약 처치 그룹(Treatment)에 따라 혈압(BloodPressure)의 분산이 달라지는지 판단하고자 한다.  
<br>  
Control, DrugA, DrugB 그룹 중 두 그룹을 선택하여 분산이 큰 쪽을 분자로 하는 F-검정을 수행하시오.  
<br>  
그리고 F-검정 통계량을 소수 셋째 자리까지 반올림하여 출력하시오.  
</h3>  

<details>
<summary>코드</summary>

```python
import numpy as np
from scipy.stats import f

group1 = df[df['Treatment'] == 'Control']['BloodPressure']
group2 = df[df['Treatment'] == 'DrugA']['BloodPressure']
group3 = df[df['Treatment'] == 'DrugB']['BloodPressure']

group2_var = np.var(group2, ddof=1)
group3_var = np.var(group3, ddof=1)

n2 = len(group2)
n3 = len(group3)

if group2_var > group3_var:
    f_stats = group2_var / group3_var
    df2 = n2 - 1
    df3 = n3 - 1
    pvalue = f.sf(f_stats, df2, df3)
else:
    f_stats = group3_var / group2_var
    df2 = n2 - 1
    df3 = n3 - 1
    pvalue = f.sf(f_stats, df3, df2)

print("F_statistics:", f_stats.round(3))
print("p-value:", pvalue.round(3))
```

>F_statistics: 1.946<br>
>p-value: 0.012
</details>  



<br><br><br><br>


<h3 style="font-weight:normal;">10-2.</h3>  
<h3 style="font-weight:normal;">  
성별(Gender)에 따른 혈당(Glucose) 분산이 동일한지 검정하고,  
<br>  
F-검정 통계량을 소수 셋째 자리까지 반올림하여 출력하시오. (분산이 더 큰 쪽을 분자로 사용할 것)  
</h3>  

<details>  
<summary>코드</summary>  
import numpy as np<br>
from scipy.stats import f<br><br>
group1 = df[df['Gender'] == 'Male']['Glucose']<br>
group2 = df[df['Gender'] == 'Female']['Glucose']<br><br>
group1_var = np.var(group1, ddof=1)<br>
group2_var = np.var(group2, ddof=1)<br><br>
n1 = len(group1)<br>
n2 = len(group2)<br><br>
df1 = n1 - 1<br>
df2 = n2 - 1<br><br>
if group1_var > group2_var:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;f_stats = group1_var / group2_var<br>
    &nbsp;&nbsp;&nbsp;&nbsp;pvalue = f.sf(f_stats, df1, df2)<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;f_stats = group2_var / group1_var<br>
    &nbsp;&nbsp;&nbsp;&nbsp;pvalue = f.sf(f_stats, df2, df1)<br><br>
print("F-statistics:", f_stats.round(3))<br>
print("pvalue:", pvalue.round(3))<br><br>
<span style="color:gray;"># 합동 분산량 계산</span><br>
pooled_var = ((n1 - 1) * group1_var + (n2 - 1) * group2_var) / (n1 + n2 - 2)<br><br>
print("합동 분산량 (Pooled Variance):", round(pooled_var, 3))

<br><br>
>F-statistics: 1.666<br>
>pvalue: 0.014<br>
>합동 분산량 (Pooled Variance): 214.932
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">10-3.</h3>  
<h3 style="font-weight:normal;">  
DrugB 그룹의 혈압(BloodPressure) 분산이 Control 또는 DrugA보다 통계적으로 더 큰지를 보기 위해,  
<br>  
DrugB-Control, DrugB-DrugA 두 쌍에 대해 각각 F-검정을 수행하고,  
<br>  
각각의 F-검정 통계량을 소수 셋째 자리까지 반올림하여 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
import numpy as np<br><br>
group1 = df[df['Treatment'] == 'DrugB']['BloodPressure']<br>
group2 = df[df['Treatment'] == 'DrugA']['BloodPressure']<br>
group3 = df[df['Treatment'] == 'Control']['BloodPressure']<br><br>
group1_var = np.var(group1, ddof=1)<br>
group2_var = np.var(group2, ddof=1)<br>
group3_var = np.var(group3, ddof=1)<br><br>
print("DrugB와 DrugA의 F검정통계량:", (group1_var / group2_var).round(3))<br><br>
if group1_var / group2_var > 1.0:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("DrugB의 검정통계량이 DrugA보다 크다.", end='\n\n')<br>
else:<br>
    p&nbsp;&nbsp;&nbsp;&nbsp;rint("DrugB의 분산이 DrugA보다 작다.", end='\n\n')<br><br>
print("DrugB와 Control의 F검정통계량:", (group1_var / group3_var).round(3))<br><br>
if group1_var / group3_var > 1.0:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("DrugB의 검정통계량이 Control보다 크다.")<br>
else:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;print("DrugB의 분산이 Control보다 작다.")<br>

<br><br>
>DrugB와 DrugA의 F검정통계량: 1.946<br>
>DrugB의 검정통계량이 DrugA보다 크다.<br><br>
>
>DrugB와 Control의 F검정통계량: 2.986<br>
>DrugB의 검정통계량이 Control보다 크다.
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">11-1.</h3>  
<h3 style="font-weight:normal;">  
성별에 따라 키의 평균에 차이가 있는지 검정하시오.  
<br>  
적절한 검정을 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import shapiro, levene, ttest_ind<br><br>
male = df[df['성별'] == '남']['키']<br>
female = df[df['성별'] == '여']['키']<br><br>
<span style="color:gray;"># print(shapiro(male))</span><br>
<span style="color:gray;"># print(shapiro(female))</span><br>
<span style="color:gray;"># -> 두 집단 모두 정규성을 만족한다.</span><br><br>
<span style="color:gray;"># print(levene(male, female))</span><br>
<span style="color:gray;"># -> 두 집단의 등분산성을 만족하지 않으므로 welch's t-검정을 시행한다.</span><br><br>
stats, p = ttest_ind(male, female, equal_var=False)<br><br>
print("statistics:", stats)<br>
print("pvalue:", p)

<br><br>
>statistics: -0.17549603962612095<br>
>pvalue: 0.8607868693135134
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">11-2.</h3>  
<h3 style="font-weight:normal;">  
흡연 여부와 운동 여부가 서로 독립적인지 검정하시오.  
<br>  
적절한 검정을 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import chi2_contingency<br><br>
cdf = pd.crosstab(df['흡연여부'], df['운동여부'])<br>
chi2_stats, chi2_p, _ , _ = chi2_contingency(cdf)<br>
print("statistics:", chi2_stats)<br>
print("pvalues:", chi2_p)<br>
</details>  

<br><br>
>statistics: 2.1122034295222973<br>
>pvalues: 0.14612877750522046

<br><br><br><br>



<h3 style="font-weight:normal;">11-3.</h3>  
<h3 style="font-weight:normal;">  
혈압이 정규성을 만족하는지 검정하시오.  
<br>  
적절한 검정을 선택하여 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import shapiro<br><br>
blpr = df['혈압']<br>
stat, p = shapiro(blpr)<br>
print("statistics:", stat)<br>
print("pvalues:", p)<br>

<br><br>
>statistics: 0.9935618194246021<br>
>pvalues: 0.0866211621977162
</details>



<br><br><br><br>


<h3 style="font-weight:normal;">11-4.</h3>  
<h3 style="font-weight:normal;">  
연령대에 따라 체중의 분산이 동일한지 검정하시오.  
<br>  
적절한 검정을 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import levene<br><br>
<span style="color:gray;"># 각 그룹별 체중 데이터 추출</span><br>
group_20 = df[df['연령대'] == '20대']['체중']<br>
group_30 = df[df['연령대'] == '30대']['체중']<br>
group_40 = df[df['연령대'] == '40대']['체중']<br>
group_50 = df[df['연령대'] == '50대']['체중']<br><br>
<span style="color:gray;"># Levene 등분산성 검정 수행</span><br>
stat, p = levene(group_20, group_30, group_40, group_50)<br><br>
print("Levene statistics:", stat)<br>
print("p-value:", p)

<br><br>
>Levene statistics: 2.524537878254589<br>
>p-value: 0.057262483899653514
</details>  


<br><br><br><br>


<h3 style="font-weight:normal;">11-5.</h3>  
<h3 style="font-weight:normal;">  
이 데이터셋에서 연령대 변수의 분포가 다음 이론적 분포와 일치하는지 검정하시오.  
</h3>  
<br>  
기대 이론적 분포 (비율 기준):  
<h4 style="font-weight:normal;">- 20대: 20%</h4>  
<h4 style="font-weight:normal;">- 30대: 30%</h4>  
<h4 style="font-weight:normal;">- 40대: 30%</h4>  
<h4 style="font-weight:normal;">- 50대: 20%</h4>  
<br>  
<h3 style="font-weight:normal;">  
적절한 검정을 수행하고, 검정 통계량과 p-value를 출력하시오.  
</h3>  

<details>  
<summary>코드</summary>  
from scipy.stats import chisquare<br><br>
<span style="color:gray;"># 관측 빈도 구하기</span><br>
observed = df['연령대'].value_counts().reindex(['20대', '30대', '40대', '50대']).values<br><br>
<span style="color:gray;"># 전체 샘플 수</span><br>
total_n = len(df)<br><br>
<span style="color:gray;"># 이론적 기대비율</span><br>
expected_ratio = {'20대': 0.2, '30대': 0.3, '40대': 0.3, '50대': 0.2}<br><br>
<span style="color:gray;"># 기대빈도 계산</span><br>
expected = [total_n * expected_ratio[age] for age in ['20대', '30대', '40대', '50대']]<br><br>
<span style="color:gray;"># 카이제곱 적합도 검정 수행</span><br>
stat, p = chisquare(f_obs=observed, f_exp=expected)<br><br>
print("Chi-square statistics:", stat)<br>
print("p-value:", p)

<br><br>
>Chi-square statistics: 10.720833333333333<br>
>p-value: 0.013335305093221296
</details>  

<br><br><br><br>



<h3 style="font-weight:normal;">12.</h3>  
<h3 style="font-weight:normal;">  
연봉에 영향을 주는 요인을 분석하는 회귀모형을 구축하시오. (독립변수는 종속변수를 제외한 모든 변수들을 사용할 것)  
<br>  
그리고 다음을 출력하시오:  
</h3>  

<h4 style="font-weight:normal;">- 회귀계수 (β) 값 전체</h4>  
<h4 style="font-weight:normal;">- p-value 값 전체</h4>  
<h4 style="font-weight:normal;">- 유의미한 변수의 개수 (p-value &lt; 0.05인 변수 개수)</h4>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import ols<br><br>
model = ols("연봉 ~ C(전공) + C(학위) + 연구년수 + 연간논문수", data=df).fit()<br><br>
print(model.params[1:])<br>
print("\n")<br>
print(model.pvalues[1:])<br>
print("\n")<br><br>
pvalues = model.pvalues[1:]<br>
target_num = len(pvalues[pvalues < 0.05])<br>
print("유의미한 변수의 개수:", target_num)


<br><br>
>coef<br>
>C(전공)[T.사회]   -10.174765<br>
>C(전공)[T.인문]    -9.808244<br>
>C(전공)[T.자연]   -10.346413<br>
>C(학위)[T.석사]    -7.111037<br>
>C(학위)[T.학사]   -14.510797<br>
>연구년수            1.265994<br>
>연간논문수           1.199229<br><br>

>pvalue<br>
>C(전공)[T.사회]     4.179690e-57<br>
>C(전공)[T.인문]     1.042821e-52<br>
>C(전공)[T.자연]     6.193599e-59<br>
>C(학위)[T.석사]     8.130448e-41<br>
>C(학위)[T.학사]    4.824165e-112<br>
>연구년수            4.837947e-36<br>
>연간논문수           5.287497e-12<br><br>

>유의미한 변수의 개수: 7
</details>  


<br><br><br><br>



<h3 style="font-weight:normal;">13.</h3>  
<h3 style="font-weight:normal;">  
이직의도를 예측하는 로지스틱 회귀모형을 적합하고 다음을 출력하시오:  
</h3>  

<h4 style="font-weight:normal;">- 유의미한 변수의 개수</h4>  
<h4 style="font-weight:normal;">- 오즈비 (odds ratio) 값 전체 (exp(β))</h4>  
<h4 style="font-weight:normal;">- 잔차 이탈도 (Residual Deviance)</h4>  
<h4 style="font-weight:normal;">- 이탈도 차이 기반의 모형 적합도 검정 (LR test 통계량)</h4>  

<details>  
<summary>코드</summary>  
from statsmodels.formula.api import logit<br>
import numpy as np<br><br>
model = logit("이직의도 ~ C(전공) + C(학위) + 연구년수 + 연간논문수", data=df).fit()<br><br>
<span style="color:gray;"># 유의미한 변수의 개수 (p-value < 0.05인 변수 개수)</span><br>
target_num = len(pvalue[pvalue < 0.05])<br>
print("\n")<br>
print(f"유의미한 변수의 개수: {target_num}개")<br>
print("\n")<br><br>
<span style="color:gray;"># 오즈비 (odds ratio) 값 전체 (exp(β))</span><br>
print("odds ratio:\n", np.exp(model.params[1:]))<br>
print("\n")<br><br>
<span style="color:gray;"># 잔차 이탈도 (Residual Deviance)</span><br>
print("Residual Deviance:", -2 * model.llf)<br>
print("\n")<br><br>
<span style="color:gray;"># 이탈도 차이 기반의 모형 적합도 검정 (LR test 통계량)</span><br>
print("LR test statistics:", -2 * (model.llnull - model.llf))

<br><br>

>유의미한 변수의 개수: 1개<br><br>
>
>
>odds ratio:<br>
>C(전공)[T.사회]     130.411568<br>
>C(전공)[T.인문]     113.323960<br>
>C(전공)[T.자연]      96.562353<br>
>C(학위)[T.석사]      25.142587<br>
>C(학위)[T.학사]    1278.187320<br>
>연구년수              0.561776<br>
>연간논문수             0.491720<br><br>
>
>Residual Deviance: 230.95439446109864<br><br>
>
>LR test statistics: 318.8234774085586
</details>  

</details>

