# 개요
이 저장소는 빅데이터분석기사 실기 시험을 대비하기 위해 제작한 개인 실습 자료입니다.
실제 기출 문제 형식을 참고하여, ChatGPT를 활용해 문제 지문과 데이터셋(CSV)을 생성하고, 이를 기반으로 직접 풀이 과정을 구현하였습니다.


# 📝 1유형

<details>
<summary>문제 목록 보기</summary>

1. 각 연도별로 사망률이 가장 높은 질병명을 구하고, 해당 질병들의 사망자수 평균을 소수점 첫번째 자리에서 반올림하여 구하시오.
(사망률 = 사망자수 / 환자수)

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


</details>
