# 개요
이 저장소는 빅데이터분석기사 실기 시험을 대비하기 위해 제작한 개인 실습 자료입니다.
실제 기출 문제 형식을 참고하여, ChatGPT를 활용해 문제 지문과 데이터셋(CSV)을 생성하고, 이를 기반으로 직접 풀이 과정을 구현하였습니다.


# 📝 1유형

<details>
<summary><h2>📌 문제 목록 보기</h2></summary>

<h3 style="font-weight:normal;">1.</h3>
<h3 style="font-weight:normal;">
각 연도별로 사망률이 가장 높은 질병명을 구하고, 해당 질병들의 사망자수 평균을 소수점 첫번째 자리에서 반올림하여 구하시오. (사망률 = 사망자수 / 환자수)
</h3>

<details>
<summary>코드</summary>
target = df[(df['거주지'] == '도시') & (df['성별'] == '남성') & (df['연령'] >= 60)]
answer = target['의료비'].mean()
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">2.</h3>
<h3 style="font-weight:normal;">
도시 거주자 중 60세 이상 남성의 평균 의료비를 구하시오.
</h3>

<details>
<summary>코드</summary>
target = df.sort_values(by=['연도', '매출'], ascending=[False, False]).groupby('연도').head(2)['매출'].sum()
target
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">3.</h3>
<h3 style="font-weight:normal;">
각 연도별로 매출 상위 2개 제품의 매출 합계를 구하시오.
</h3>

<details>
<summary>코드</summary>
target = df.sort_values(by=['연도', '매출'], ascending=[False, False]).groupby('연도').head(2)['매출'].sum()
target
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">4.</h3>
<h3 style="font-weight:normal;">
누적 재고량이 처음으로 5000을 초과한 월을 구하시오.
</h3>

<details>
<summary>코드</summary>
df['월'] = pd.to_datetime(df['월'])
df['month'] = df['월'].dt.month
target = df[df['누적재고'] > 5000]['month'].iloc[0]
target
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">5.</h3>
<h3 style="font-weight:normal;">
부서별로 연도별 급여 인상률의 평균을 계산한 후, 인상률의 편차(표준편차)가 가장 작은 부서를 구하시오.  (즉, 가장 일관되게 인상률이 높은 부서를 찾는 문제)

- 인상률 = (이번 해 연봉 - 전년도 연봉) / 전년도 연봉
- 첫 해(2018년)는 인상률 계산에서 제외
</h3>

<details>
<summary>코드</summary>
# 전년도 연봉 추가 (부서별 shift)
df['전년도연봉'] = df.groupby('부서')['연봉'].shift(1)

# 인상률 계산
df['인상률'] = (df['연봉'] - df['전년도연봉']) / df['전년도연봉']

# 각 부서별 인상률의 표준편차 계산
std_by_dept = df.groupby('부서')['인상률'].std()

# 인상률의 표준편차가 가장 작은 부서
answer = std_by_dept.idxmin()
print("가장 일정한 인상률을 가진 부서:", answer)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">6-1.</h3>
<h3 style="font-weight:normal;">
도시 거주자 중 60세 이상 여성의 방문횟수 평균을 소수 둘째 자리까지 반올림하여 나타내시오.
</h3>

<details>
<summary>코드</summary>
answer = round(df[(df['거주지'] == '도시') & (df['연령'] >= 60) & (df['성별'] == '여성')]['방문횟수'].mean(), 2)
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">6-2.</h3>
<h3 style="font-weight:normal;">
각 연도별 질병 사망률을 계산하고, 그중 사망률이 가장 높은 질병 이름을 연도, 질병, 사망률 형태로 출력하시오.  (사망률 = 사망자 수 / 전체 환자 수)
</h3>

<details>
<summary>코드</summary>
# 연도-질병별 전체 환자수와 사망자 수 계산
df_rate = df.groupby(['연도', '질병']).agg(
    사망자수=('사망여부', 'sum'),
    인원수=('사망여부', 'count')
).reset_index()

# 사망률 계산
df_rate['사망률'] = df_rate['사망자수'] / df_rate['인원수']

# 연도별 최고 사망률 질병 추출
answer = df_rate.sort_values(['연도', '사망률'], ascending=[True, False]) \
                .groupby('연도').head(1)[['연도', '질병', '사망률']].reset_index(drop=True)

display(answer)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">7.</h3>
<h3 style="font-weight:normal;">
각 연도별로, 반품률이 가장 높은 상품명을 구하시오.  (반품률 = 반품된 건수 / 전체 리뷰 건수)
</h3>

<details>
<summary>코드</summary>
df_rate = df.groupby(['연도', '상품']).agg(반품된건수=('반품여부', 'sum'), 리뷰건수=('반품여부', 'size')).reset_index()
df_rate['반품률'] = df_rate['반품된건수'] / df_rate['리뷰건수']
answer = df_rate.sort_values(by=['연도', '반품률'], ascending=False).groupby('연도').head(1)[['연도', '상품']].reset_index(drop=True)
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">8.</h3>
<h3 style="font-weight:normal;">
각 과목별로, 최근 4년간(2020~2023) 평균 성적이 가장 높은 학교 이름을 과목, 학교 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df_melt = pd.melt(df, id_vars=['학교', '연도'], value_vars=['국어', '영어', '수학', '과학'], var_name='과목', value_name='점수')
df_melt = df_melt.groupby(['학교', '과목']).agg(과목평균=('점수', 'mean')).reset_index()
answer = df_melt.sort_values(by=['과목', '과목평균'], ascending=False).groupby('과목').head(1)[['과목', '학교']].reset_index(drop=True)
answer
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
melt = pd.melt(df, id_vars=['연도', '지역', '구분'], value_vars=['가스(m³)', '수도(m³)', '전기(kWh)'], var_name='에너지원', value_name='총')

melt1 = melt[melt['구분'] == '사용량']
melt2 = melt[melt['구분'] == '요금']

merge = pd.merge(melt1, melt2, on=['연도', '지역', '에너지원'], suffixes=['사용량', '요금'])

grouped = merge.groupby(['연도', '에너지원'])[['총사용량', '총요금']].sum().reset_index()
target = grouped.groupby('연도')['총사용량'].idxmax()
answer = grouped.loc[target, ['연도', '에너지원', '총요금']]
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">10.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 다음 규칙을 적용하여 월별 총 구매금액을 계산하고, 그 중 2023년 총 구매금액이 상위 10%에 해당하는 고객들과 고객 수를 출력하시오.
(이 때, 구매금액이 결측인 경우에는 같은 고객, 같은 카테고리의 연도별 평균 구매금액으로 채울 것 (단, 평균값도 결측이면 전체 카테고리 평균 구매금액으로 채움))
</h3>

<details>
<summary>코드</summary>
# 구매금액이 결측치인 행 대치
df['구매금액'] = df['구매금액'].fillna(df.groupby(['연도', '고객ID', '카테고리'])['구매금액'].transform('mean'))

# 평균 구매금액 값도 결측치인 행 대치
df['구매금액'] = df['구매금액'].fillna(df['구매금액'].mean())

# 월별 총 구매금액 먼저 구하기
monthly_sum = df.groupby(['고객ID', '연도', '월'])['구매금액'].sum().reset_index()

# 다시 연도별 총 구매금액 계산
grouped = monthly_sum.groupby(['고객ID', '연도'])['구매금액'].sum().reset_index()

target = grouped[grouped['연도'] == 2023]['구매금액'].quantile(0.9)
answer = grouped[(grouped['연도'] == 2023) & (grouped['구매금액'] >= target)]['고객ID'].values.tolist()

print("조건에 맞는 고객들:")
for i in range(len(answer)):
    print(answer[i])


print("\n조건에 맞는 고객 수:", len(answer))
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">11-1.</h3>
<h3 style="font-weight:normal;">
연령대(20대, 30대, 40대, 50대, 60대 이상)를 구분하는 연령대 컬럼을 만들고, 각 연령대별로 콜레스테롤 평균을 계산하시오. 
(단, 콜레스테롤 결측치는 같은 지역 내 연령대 평균으로 채울 것. (평균값도 결측이면 전체 평균으로 채움))
최종 출력은 연령대, 평균 콜레스테롤 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['연령대'] = (df['연령'] // 10 * 10).astype(str) + '대'

# 지역, 연령대 기준으로 콜레스테롤 각 결측치 대치
df['콜레스테롤'] = df['콜레스테롤'].fillna(df.groupby(['연령대', '지역'])['콜레스테롤'].transform('mean'))

# 아직도 콜레스테롤이 결측치인 값은 콜레스테롤 전체 평균으로 대치
df['콜레스테롤'] = df['콜레스테롤'].fillna(df['콜레스테롤'].mean())

answer = df.groupby('연령대')['콜레스테롤'].mean().reset_index()
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">11-2.</h3>
<h3 style="font-weight:normal;">
혈압, 혈당, 콜레스테롤 컬럼에 대해 **표준화(z-score standardization)**를 적용하여 새로운 컬럼을 추가하시오. (이 때, 혈당은 혈당 전체 평균으로 대치)
표준화된 컬럼명은 혈압_zscore, 혈당_zscore, 콜레스테롤_zscore로 하시오. 이후 전체 데이터에서 혈압_zscore > 1.5를 만족하는 데이터의 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
from scipy.stats import zscore

df['혈당'] = df['혈당'].fillna(df['혈당'].mean())
target_col = ['혈압', '혈당', '콜레스테롤']
change_col_name = ['혈압_zscore', '혈당_zscore', '콜레스테롤_zscore']

for new_col, col in zip(change_col_name, target_col):
    df[new_col] = zscore(df[col])

answer = len(df[df['혈압_zscore'] > 1.5])
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">12-1.</h3>
<h3 style="font-weight:normal;">
각 카테고리별, 성별로 평균 주문금액을 계산하시오. (단, 주문금액 결측치는 같은 카테고리, 성별 그룹 평균으로 채운 후 계산하고, 평균값도 결측이면 전체 평균으로 채움)
최종 출력은 카테고리, 성별, 평균 주문금액 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['주문금액'] = df['주문금액'].fillna(df.groupby(['카테고리', '성별'])['주문금액'].transform('mean'))
df['주문금액'] = df['주문금액'].fillna(df['주문금액'].mean())

answer = df.groupby(['카테고리', '성별'])['주문금액'].mean().reset_index()
display(answer)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">12-2.</h3>
<h3 style="font-weight:normal;">
구매수량에 대해 최소-최대 정규화(min-max scaling) 를 적용하여 구매수량_scaled 컬럼을 추가하시오.
이후 구매수량_scaled >= 0.9 를 만족하는 데이터의 개수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
from sklearn.preprocessing import MinMaxScaler

minmax = MinMaxScaler()

df['구매수량_scaled'] = minmax.fit_transform(df[['구매수량']])
print(len(df[df['구매수량_scaled'] >= 0.9]))
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">13-1.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 불만제기 경험 여부(불만제기 여부가 한 번이라도 1인 경우 "Y", 그렇지 않으면 "N")를 나타내는 파생 컬럼을 생성하시오.
이후 2023년에 불만제기 경험이 "Y"인 고객 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
target = df[df['불만제기여부'] == 1].groupby("고객ID")['고객ID'].unique().index.tolist()
df['불만제기경험여부'] = df['고객ID'].apply(lambda x: 'Y' if x in target else 'N')

answer = df[(df['연도'] == 2023) & (df['불만제기경험여부'] == 'Y')]['고객ID'].nunique()
answer
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">13-2.</h3>
<h3 style="font-weight:normal;">
연령대(10대, 20대, 30대, 40대, 50대, 60대 이상) 컬럼을 생성하고, 각 연령대별 주문수량 평균과 주문금액 평균을 구하시오.
최종 출력은 연령대, 평균 주문수량, 평균 주문금액 형태로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['연령대'] = df['나이'].apply(lambda x: str(60) + '대 이상' if x >= 60 else str(x // 10 * 10) + '대')
answer = df.groupby('연령대')[['주문수량', '주문금액']].mean().reset_index()
display(answer)
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
df['업무만족도'] = df['업무만족도'].fillna(df.groupby('부서')['업무만족도'].transform('mean'))
df['업무만족도'] = df['업무만족도'].fillna(df['업무만족도'].mean())
df1 = df[df['근속연수'].notna()].copy()  # df.dropna(subset=['근속연수'])
answer1 = len(df1[(df1['업무만족도'] <= df1['업무만족도'].quantile(0.25)) & (df1['성과등급'] == 'A')])
print(answer1)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">14-2.</h3>
<h3 style="font-weight:normal;">
근속연수가 10년 이상이고 교육참여횟수가 전체 평균 이상인 직원들을 필터링한 후, 해당 직원들의 부서별 평균 연봉을 구하시오.
이때 연봉 평균이 세 번째로 높은 부서의 평균 연봉을 정수로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df2 = df[(df['근속연수'] >= 10.0) & (df['교육참여횟수'] >= df['교육참여횟수'].mean())].copy()
answer2 = df2.groupby('부서')['연봉'].mean().sort_values(ascending=False).values[2]
print(int(answer2))
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">14-3.</h3>
<h3 style="font-weight:normal;">
각 부서별로 업무만족도 기준 상위 20% 직원들의 평균 근속연수를 계산하시오. (단, 업무만족도가 결측인 직원은 제외하며, 근속연수가 결측인 직원도 제외한다)
이후 가장 평균 근속연수가 높은 부서명을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df3 = df.dropna(subset=['업무만족도', '근속연수']).copy()
target3 = df3.groupby('부서')['업무만족도'].transform(lambda x: x.quantile(0.8))
df3_filtered = df3[df3['업무만족도'] >= target3]
answer3 = df3_filtered.groupby('부서')['근속연수'].mean().sort_values(ascending=False).index[0]
print(answer3)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">15-1.</h3>
<h3 style="font-weight:normal;">
각 제품군별로 연도와 분기 기준 평균 반품률(반품수량 / 판매수량)을 구하시오.
최종 출력은 제품군, 연도, 분기, 평균 반품률 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df_grouped = df.groupby(['제품군', '연도', '분기'])[['판매수량', '반품수량']].sum().reset_index()
df_grouped['평균반품률'] = df_grouped['반품수량'] / df_grouped['판매수량']
answer1 = df_grouped[['제품군', '연도', '분기', '평균반품률']]
display(answer1)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">15-2.</h3>
<h3 style="font-weight:normal;">
각 연도별로 지역과 제품군을 기준으로 매출액 총합을 계산하고, 가장 매출이 높은 지역-제품군 조합을 연도별로 1개씩 출력하시오.
최종 출력은 연도, 지역, 제품군, 총 매출액 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
df_grouped = df.groupby(['연도', '지역', '제품군'])['매출액'].sum().reset_index()
target3 = df_grouped.groupby('연도')['매출액'].transform(lambda x: x.max())
answer3 = df_grouped.loc[df_grouped['매출액'] == target3, ['연도', '지역', '제품군', '매출액']]
answer3
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">16.</h3>
<h3 style="font-weight:normal;">
다음은 2020~2023년까지의 학교별 과목별 평균 성적 데이터이다.
각 과목별로 4년간 평균 성적이 가장 높은 학교명을 구하고, 최종 출력은 과목, 학교 칼럼으로 구성된 테이블로 출력하시오.
</h3>

<details>
<summary>코드</summary>
melt = pd.melt(df, id_vars='학교', value_vars=['국어', '수학', '영어', '과학'], var_name='과목', value_name='점수')
grouped = melt.groupby(['학교', '과목'])['점수'].mean().reset_index()
target = grouped.groupby('과목')['점수'].idxmax()
answer = grouped.loc[grouped.index.isin(target), ['과목', '학교']]
answer
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
df['구매금액'] = df['구매금액'].fillna(df.groupby(['성별', '카테고리'])['구매금액'].transform('mean'))
df['구매금액'] = df['구매금액'].fillna(df['구매금액'].mean())

df['연령대'] = df['연령'].apply(lambda x: str(60) + '대 이상' if (x // 10 * 10) >= 60 else str(x // 10 * 10) + '대')
answer1 = df.groupby('연령대')[['구매금액', '리뷰점수']].agg({'구매금액': 'mean', '리뷰점수': 'mean'}) \
            .rename(columns={'구매금액': '평균구매금액', '리뷰점수': '평균리뷰점수'}).reset_index()
answer1
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">17-2.</h3>
<h3 style="font-weight:normal;">
2023년에 한 번이라도 불만제기를 한 고객ID는 ‘불만경험 있음’, 그렇지 않은 경우 ‘불만경험 없음’ 으로 분류하는 파생 컬럼을 만들고,
2023년 불만경험 있음 고객 중 평균 구매수량이 가장 높은 지역을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df2 = df[df['연도'] == 2023].copy()
target2 = df2[df2['불만제기'] != 0]['고객ID'].unique()
df['불만경험여부'] = df['고객ID'].apply(lambda x: True if x in target2 else False)  # df['불만경험여부'] = df['고객ID'].isin(target2)
answer2 = df[df['불만경험여부'] == True].groupby('지역')['구매수량'].mean().idxmax()
answer2
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
df['구매금액'] = df['구매금액'].fillna(df.groupby(['성별', '상품군'])['구매금액'].transform('mean'))
df['구매금액'] = df['구매금액'].fillna(df['구매금액'].mean())
df['리뷰점수'] = df['리뷰점수'].fillna(df['리뷰점수'].mode())

df['연령대'] = df['연령'].apply(lambda x: str(x // 10 * 10) + '대 이상' if x >= 60 else str(x // 10 * 10) + '대')
answer1 = df.groupby('연령대')[['구매금액', '리뷰점수']].mean().rename(columns={'구매금액': '평균구매금액', '리뷰점수': '평균리뷰점수'}).reset_index()
display(answer1)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">18-2.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 한 번이라도 반품한 이력이 있으면 'Y', 없으면 'N' 으로 새로운 파생변수를 생성하시오.
그 후, 2023년 가입자 중 'Y'로 분류된 고객의 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['가입년도'] = df['가입일'].str.extract(r'(\d\d\d\d)-')
target = df[df['반품여부'] == 1]['고객ID'].unique().tolist()
df['반품이력'] = df['고객ID'].apply(lambda x: 'Y' if x in target else 'N')

answer2 = df[(df['가입년도'] == '2023') & (df['반품이력'] == 'Y')]['고객ID'].nunique()
print(answer2)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">18-3.</h3>
<h3 style="font-weight:normal;">
상품군별로 리뷰점수가 4점 이상인 데이터만 필터링한 후, 리뷰점수 평균이 가장 높은 상품군 이름을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['리뷰점수'] = df['리뷰점수'].fillna(df['리뷰점수'].mode()[0])
target = df[df['리뷰점수'] >= 4.0][['상품군', '리뷰점수']]
answer3 = target.groupby('상품군')['리뷰점수'].mean().idxmax()
answer3
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">19-1.</h3>
<h3 style="font-weight:normal;">
배송만족도가 결측인 경우에는 동일 결제방식 그룹의 배송만족도 평균으로 채우고, 여전히 결측인 경우 전체 평균으로 채운 후,
다음과 같은 기준으로 배송만족도를 기준으로 상·중·하 등급을 부여하시오.	
	4 이상: 상
	3 이상 4 미만: 중
	3 미만: 하

이후, 등급별로 전체 평균 구매금액을 구하고 두 칼럼으로 구성된 테이블을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['배송만족도'] = df['배송만족도'].fillna(df.groupby('결제방식')['배송만족도'].transform('mean'))
df['배송만족도'] = df['배송만족도'].fillna(df['배송만족도'].mean())
df['등급'] = df['배송만족도'].apply(lambda x: '상' if x >= 4.0 else ('중' if x >= 3.0 else '하'))
answer1 = df.groupby('등급')['구매금액'].mean().reset_index().rename(columns={'구매금액': '평균구매금액'})
answer1
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">19-2.</h3>
<h3 style="font-weight:normal;">
각 고객ID별로 리뷰를 한 상품 비율을 계산하시오. (리뷰작성=1 이면 리뷰한 것으로 간주)
이후 리뷰 비율이 70% 이상인 고객ID의 수를 출력하시오.
</h3>

<details>
<summary>코드</summary>

# 방법1
cdf = pd.crosstab(df['고객ID'], df['리뷰작성'], dropna=False).fillna(0)
cdf['리뷰비율'] = cdf[1] / (cdf[0] + cdf[1])
answer3 = len(cdf[cdf['리뷰비율'] >= 0.7])
print("고객 수:", answer)


# 방법2
review_stats = df.groupby('고객ID')['리뷰작성'].agg(['sum', 'count']).reset_index()
review_stats['리뷰비율'] = review_stats['sum'] / review_stats['count']
answer = len(review_stats[review_stats['리뷰비율'] >= 0.7])
print("고객 수:", answer)


# 방법3
pivot = df.pivot_table(index='고객ID', values='리뷰작성', aggfunc=['sum', 'count']).reset_index()
pivot.columns = ['고객ID', 'sum', 'count']
pivot['리뷰비율'] = pivot['sum'] / pivot['count']
answer = len(pivot[pivot['리뷰비율'] >= 0.7])
print("고객 수:", answer)
</details>


<br><br><br><br>

<h3 style="font-weight:normal;">19-3.</h3>
<h3 style="font-weight:normal;">
2023년 데이터만 사용하여, 각 결제방식별로 반품율(반품여부가 1인 비율) 을 구하고, 반품율이 가장 높은 결제방식명을 출력하시오.
</h3>

<details>
<summary>코드</summary>
df['구매일'] = pd.to_datetime(df['구매일'])
df['구매년도'] = df['구매일'].dt.year

df_filtered = df[df['구매년도'] == 2023]
grouped = df_filtered.groupby('결제방식')['반품여부'].agg(['sum', 'count'])
grouped.columns = ['sum', 'count']
grouped['반품비율'] = grouped['sum'] / grouped['count']
answer3 = grouped.sort_values(by='반품비율', ascending=False).index[0]
answer3
</details>

</details>
