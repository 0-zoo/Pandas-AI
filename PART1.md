### 판다스 자료구조

* 시리즈(Series) : 1차원 배열
* 데이터프레임(DataFrame) : 2차원 배열

* * *

#### 시리즈

##### 시리즈 만들기

시리즈와 딕셔너리의 구조 비슷 -> 딕셔너리를 시리즈로 변환하는 방법 많이 이용

판다스 내장 함수인 Series() 이용, 딕셔너리를 함수의 인자로 전달


```python
# 예제 1-1
# 딕셔너리 -> 시리즈
# 'k:v' 구조를 갖는 딕셔너리를 정의하여 변수 dict_data에 저장
# 딕셔너리를 시리즈 함수의 인자로 전달하여 시리즈로 변환, 시리즈 객체를 변수 sr에 저장

import pandas as pd

# key:value 쌍으로 딕셔너리를 만들고, 변수 dict_data에 저장
dict_data = {'a':1, 'b':2, 'c':3}

# 판다스 Series() 함수로 딕셔너리를 시리즈로 변환, 변수 sr에 저장
sr = pd.Series(dict_data)

# sr의 자료형 출력
print(type(sr))
print('\n')

# 변수 sr에 저장되어 있는 시리즈 객체 출력
print(sr)
```

    <class 'pandas.core.series.Series'>
    
    
    a    1
    b    2
    c    3
    dtype: int64
    

##### 인덱스 구조

종류

* 정수형 위치 인덱스
* 인덱스 이름 또는 인덱스 라벨


```python
# 예제 1-2
# 파이썬 리스트를 시리즈로 변환
# 딕셔너리의 키처럼 인덱스로 변환될 값이 없음 
# -> 인덱스 별도로 지정하지 않으면 디폴트로 정수형 위치 인덱스(0,1,2,..) 자동 지정

import pandas as pd

# 리스트를 시리즈로 변환하여 변수 sr에 저장
list_data = ['2022-11-08', 3.14, 'ABC', 100, True]
sr = pd.Series(list_data)
print(sr)
print('\n')

# 시리즈의 index 속성과 values 속성을 이용하면, 인덱스 배열과 데이터 값의 배열을 불러올 수 있다.
# 인덱스 배열은 변수 idx에 저장. 데이터 값 배열은 변수 val에 저장
idx = sr.index
val = sr.values
print(idx)
print(val)
```

    0    2022-11-08
    1          3.14
    2           ABC
    3           100
    4          True
    dtype: object
    
    
    RangeIndex(start=0, stop=5, step=1)
    
    
    ['2022-11-08' 3.14 'ABC' 100 True]
    

##### 원소 선택

1) 인덱스를 이용하여 시리즈의 원소를 선택

2) 하나의 원소 선택/여러 원소 한꺼번에 선택 가능

3) 인덱스 범위를 지정하여 여러 개의 원소 선택 가능

4) 인덱스 유형에 따른 사용법 

        - 정수형 인덱스 : []안에 숫자 입력
        - 인덱스 이름(라벨) : []안에 이름과 함께 따옴표 입력


```python
# 예제 1-3
# 파이썬 튜플을 시리즈로 변환
# 정수형 위치 인덱스 대신에 인덱스 이름을 따로 지정할 수 있음
# Series() 함수의 index 옵션에 인덱스 이름을 리스트 형태로 직접 전달

# 튜플을 시리즈로 변환(인덱스 옵션 지정)
tup_data = ('영인', '2022-11-08', '여', True)
sr = pd.Series(tup_data, index=['이름', '생년월일', '성별', '학생여부'])
print(sr)
print('\n')

# 원소를 1개 선택
print(sr[0])       # sr의 1번째 원소를 선택(정수형 위치 인덱스)
print(sr['이름'])  # '이름' 라벨을 가진 원소를 선택(인덱스 이름)
print('\n')

# 여러 개의 원소를 선택(인덱스 리스트 활용)
print(sr[[1,2]])
print(sr[['생년월일', '성별']])
print('\n')

# 여러 개의 원소를 선택(인덱스 범위 지정)
print(sr[1:2])
print(sr['생년월일':'성별'])
```

    이름              영인
    생년월일    2022-11-08
    성별               여
    학생여부          True
    dtype: object
    
    
    영인
    영인
    
    
    생년월일    2022-11-08
    성별               여
    dtype: object
    생년월일    2022-11-08
    성별               여
    dtype: object
    
    
    생년월일    2022-11-08
    dtype: object
    생년월일    2022-11-08
    성별               여
    dtype: object
    

#### 데이터프레임

1) 데이터프레임은 2차원 배열

2) 열: 시리즈 객체. 시리즈를 열벡터라고 하면, 
    데이터프레임은 여러 개의 열벡터들이 같은 행 인덱스를 기준으로 줄지어 결합된 2차원 벡터 또는 행렬
    
3) 행과 열을 나타내기 위해 두 가지 종류의 주소 사용. 행 인덱스/열 이름 으로 구분

4) 각 열은 공통의 속성을 갖는 일련의 데이터를 나타냄

5) 각 행은 개별 관측대상에 대한 다양한 속성 데이터들의 모음임 레코드(record)

##### 데이터프레임 만들기

1) 같은 길이(원소 개수 동일) 배열 여러개 필요

2) 판다스 DataFrame() 함수를 사용. 여러 개의 리스트를 원소로 갖는 딕셔너리를 함수에 전달하는 방식을 주로 활용.

3) 딕셔너리의 값(v)에 해당하는 각 리스트가 시리즈로 변환되어 데이터프레임의 각 열이 됨

4) 딕셔너리의 키(k)는 각 시리즈의 이름으로 변환되어, 최종적으로 데이터프레임의 열 이름이 됨


```python
# 예제 1-4
# 원소를 3개씩 담고 있는 리스트를 5개 만든다. 
# 이 5개의 리스트를 원소로 갖는 딕셔너리를 정의하고, DataFrame()함수에 전달하여 5개의 열을 갖는 데이터프레임을 만든다.
# 이때, 딕셔너리의 키가 열 이름이 되고, 값에 해당하는 각 리스트가 데이터프레임의 열이 된다.
# 행 인덱스에는 정수형 위치 인덱스가 자동 지정된다.

# 열 이름을 key로 하고, 리스트를 value로 갖는 딕셔너리 정의(2차원 배열)
dict_data = {'c0':[1,2,3], 'c1':[4,5,6], 'c2':[7,8,9], 'c3':[10,11,12], 'c4':[13,14,15]}

# 판다스 DataFrame() 함수로 딕셔너리를 데이터프레임으로 변환. 변수 df에 저장.
df = pd.DataFrame(dict_data)

# df의 자료형 출력
print(type(df))
print('\n')
# 변수 df에 저장되어 있는 데이터프레임 객체를 출력
print(df)
```

    <class 'pandas.core.frame.DataFrame'>
    
    
       c0  c1  c2  c3  c4
    0   1   4   7  10  13
    1   2   5   8  11  14
    2   3   6   9  12  15
    

##### 행 인덱스/열 이름 설정

* 데이터프레임의 행 인덱스와 열 이름을 사용자가 지정 가능


```python
# 예제 1-5
# '3개의 원소를 갖는 리스트' 2개를 원소로 갖는 리스트(2차원 배열)로 데이터프레임을 만든다.
# 이때, 각 리스트가 행으로 변환되는 점에 유의한다.
# - index옵션: ['준서', '예은'] 배열을 지정
# - columns옵션: ['나이', '성별', '학교'] 배열을 지정

# 행 인덱스/열 이름 지정하여 데이터프레임 만들기
df = pd.DataFrame([[15, '남', '덕영중'], [17, '여', '수리중']],
                 index = ['준서', '예은'],
                 columns = ['나이', '성별', '학교'])

# 행 인덱스, 열 이름 확인하기

print(df)         # 데이터프레임
print('\n')
print(df.index)   # 행 인덱스
print('\n')
print(df.columns) # 열 이름
print('\n')

# 행 인덱스, 열 이름 변경하기
df.index = ['학생1', '학생2']
df.columns = ['연령', '남녀', '소속']

print(df)         # 데이터프레임
print('\n')
print(df.index)   # 행 인덱스
print('\n')
print(df.columns) # 열 이름
```

        나이 성별   학교
    준서  15  남  덕영중
    예은  17  여  수리중
    
    
    Index(['준서', '예은'], dtype='object')
    
    
    Index(['나이', '성별', '학교'], dtype='object')
    
    
         연령 남녀   소속
    학생1  15  남  덕영중
    학생2  17  여  수리중
    
    
    Index(['학생1', '학생2'], dtype='object')
    
    
    Index(['연령', '남녀', '소속'], dtype='object')
    


```python
# 예제 1-6
# rename() 메소드를 적용하면, 행 인덱스 또는 열 이름의 일부를 선택하여 변경 가능.
# 단, 원본 객체를 직접 수정하는 것이 아니라 새로운 데이터프레임 객체를 반환하는 점 유의
# * 원본 객체를 변경하려면, inplace=True 옵션을 사용

# 행 인덱스/열 이름 지정하여 데이터프레임 만들기
df = pd.DataFrame([[15, '남', '덕영중'], [17, '여', '수리중']],
                  index = ['준서', '예은'],
                 columns = ['나이', '성별', '학교'])

# 데이터프레임 df 출력
print(df)
print('\n')

# 열 이름 중, '나이'를 '연령'으로, '성별'을 '남녀'로, '학교'를 '소속'으로 바꾸기
df.rename(columns={'나이':'연령', '성별':'남녀', '학교':'소속'}, inplace=True)

# df의 행 인덱스 중에서, '준서'를 '학생1'로, '예은'을 '학생2'로 바꾸기
df.rename(index={'준서':'학생1', '예은':'학생2'}, inplace=True)

# df 출력(변경 후)
print(df)
```

        나이 성별   학교
    준서  15  남  덕영중
    예은  17  여  수리중
    
    
         연령 남녀   소속
    학생1  15  남  덕영중
    학생2  17  여  수리중
    

##### 행/열 삭제

* drop() 메소드에 행을 삭제할 때는 축(axis) 옵션으로 axis=0을 입력하거나, 별도로 입력하지 않음

* 반면, 축옵션으로 axis=1을 입력하면 열을 삭제함. 동시에 여러 개의 행 또는 열을 삭제하려면, 리스트 형태로 입력함

* 한편, drop() 메소드는 기존 객체를 변경하지 않고 새로운 객체를 반환하는 점에에 유의
        -> 따라서, 원본 객체를 직접 변경하기 위해서는, inplace=True 옵션을 추가함


```python
# 예제 1-7
# 행 삭제

# DataFrame() 함수로 데이터프레임 반환. 변수 df 에 저장
exam_data = {'수학' : [90, 80, 70], '영어' : [98, 89, 95],
            '음악' : [85, 95, 100], '체육' : [100, 90, 90]}

df = pd.DataFrame(exam_data, index=['서준', '우현', '인아'])
print(df)
print('\n')

# 데이터프레임 df를 복제하여 변수 df2에 저장. df2의 1개 행(row) 삭제
df2 = df[:]
df2.drop('우현', inplace=True)
print(df2)
print('\n')

# 데이터프레임 df를 복제하여 변수 df3에 저장. df3의 2개 행(row) 삭제
df3 = df[:]
df3.drop(['우현', '인아'], axis=0, inplace=True)
print(df3)
```

        수학  영어   음악   체육
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    
    
        수학  영어   음악   체육
    서준  90  98   85  100
    인아  70  95  100   90
    
    
        수학  영어  음악   체육
    서준  90  98  85  100
    

    C:\Users\gram\AppData\Local\Temp\ipykernel_14224\2693150306.py:14: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df2.drop('우현', inplace=True)
    C:\Users\gram\AppData\Local\Temp\ipykernel_14224\2693150306.py:20: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df3.drop(['우현', '인아'], axis=0, inplace=True)
    


```python
# 예제 1-8
# 열 삭제

# DataFrame() 함수로 데이터프레임 반환. 변수 df에 저장
exam_data = {'수학' : [90, 80, 70], '영어' : [98, 89, 95],
            '음악' : [85, 95, 100], '체육' : [100, 90, 90]}

df = pd.DataFrame(exam_data, index=['서준', '우현', '인아'])
print(df)
print('\n')

# 데이터프레임 df를 복제하여 변수 df4에 저장. df4의 1개 열(column) 삭제
df4 = df[:]
df4.drop('수학', axis=1, inplace=True)
print(df4)
print('\n')

# 데이터프레임 df를 복제하여 변수 df5에 저장. df5의 2개 열(column) 삭제
df5 = df[:]
df5.drop(['영어', '음악'], axis=1, inplace=True)
print(df5)
```

        수학  영어   음악   체육
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    
    
        영어   음악   체육
    서준  98   85  100
    우현  89   95   90
    인아  95  100   90
    
    
        수학   체육
    서준  90  100
    우현  80   90
    인아  70   90
    

    C:\Users\gram\AppData\Local\Temp\ipykernel_14224\2637185714.py:14: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df4.drop('수학', axis=1, inplace=True)
    C:\Users\gram\AppData\Local\Temp\ipykernel_14224\2637185714.py:20: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df5.drop(['영어', '음악'], axis=1, inplace=True)
    

##### 행 선택

1) loc와 iloc 인덱스를 사용

2) 인덱스 이름을 기준으로 행을 선택할 때는 loc를 이용하고, 정수형 위치 인덱스를 사용할 때는 iloc를 이용


```python
# 예제 1-9

# 1개의 행 선택
# 데이터프레임의 첫 번째 행에는 '서준' 학생의 과목별 점수 데이터가 입력되어 있다.
# '서준' 학생의 과목별 점수 데이터 행으로 추출하면 시리즈 객체가 반환된다.
# loc 인덱서를 이용하려면 '서준' 이라는 인덱스 이름을 직접 입력하고, 
# iloc를 이용할 때는 첫 번째 정수형 위치를 나타내는 0을 입력한다.
# 각각 반환되는 값을 label1 변수와 position1 변수에 저장, 출력하면 다음과 같은 결과를 갖는다.

# DataFrame() 함수로 데이터프레임 반환. 변수 df에 저장
exam_data = {'수학' : [90, 80, 70], '영어' : [98, 89, 95],
            '음악' : [85, 95, 100], '체육' : [100, 90, 90]}

df = pd.DataFrame(exam_data, index=['서준', '우현', '인아'])
print(df)
print('\n')

# 행 인덱스를 사용하여 행 1개 선택
label1 = df.loc['서준']
position1 = df.iloc[0]
print(label1)
print('\n')
print(position1)
```

        수학  영어   음악   체육
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    
    
    수학     90
    영어     98
    음악     85
    체육    100
    Name: 서준, dtype: int64
    
    
    수학     90
    영어     98
    음악     85
    체육    100
    Name: 서준, dtype: int64
        수학  영어  음악   체육
    서준  90  98  85  100
    우현  80  89  95   90
    
    
        수학  영어  음악   체육
    서준  90  98  85  100
    우현  80  89  95   90
        수학  영어  음악   체육
    서준  90  98  85  100
    우현  80  89  95   90
    
    
        수학  영어  음악   체육
    서준  90  98  85  100
    


```python
# 여러 개의 행 선택(인덱스 리스트 활용)
# 2개 이상의 행 인덱스를 배열로 입력하면, 매칭되는 모든 행 데이터를 동시에 추출한다.
# 데이터프레임 df의 첫번째와 두번째 행에 있는 '서준', '우현' 학생을 인덱싱으로 선택해 본다.
# loc 인덱서는 ['서준', '우현'] 과 같이 인덱스 이름을 배열로 전달하고, 
# iloc을 이용할 때는 [0,1]과 같이 정수형 위치를 전달한다.
# 이때, label2 변수와 position2 변수에 저장된 값은 같다.
label2 = df.loc[['서준', '우현']]
position2 = df.iloc[[0,1]]
print(label2)
print('\n')
print(position2)
```

        수학  영어  음악   체육
    서준  90  98  85  100
    우현  80  89  95   90
    
    
        수학  영어  음악   체육
    서준  90  98  85  100
    우현  80  89  95   90
    


```python
# 여러 개의 원소를 선택(인덱스 범위 지정)
# 행 인덱스의 범위를 지정하여 여러 개의 행을 동시에 선택하는 슬라이싱 기법 사용
# 단, 인덱스 이름을 범위로 지정한 label3의 경우에는 범위의 마지막 값인 '우현' 학생의 점수가 포함되지만, 
# 정수형 위치 인덱스를 사용한 position3에는 범위의 마지막 값인 '우현' 학생의 점수가 제외된다.

# 행 인덱스의 범위를 지정하여 행 선택
label3 = df.loc['서준':'우현']
position3 = df.iloc[0:1]
print(label3)
print('\n')
print(position3)
```

        수학  영어  음악   체육
    서준  90  98  85  100
    우현  80  89  95   90
    
    
        수학  영어  음악   체육
    서준  90  98  85  100
    

##### 열 선택

* 열 데이터를 1개만 선택할 때는, 대괄호([]) 안에 열 이름을 따옴표와 함께 입력하거나 

* 도트(.) 다음에 열 이름을 입력하는 두 가지 방식 사용

* 두 번째 방법은 반드시 열 이름이 문자열일 경우에만 가능


```python
# 예제 1-10
# type() 함수를 사용하여, 데이터프레임에서 1개의 열을 선택할 때 반환되는 객체의 자료형을 확인하면 시리즈이다.

# DataFrame() 함수로 데이터프레임 반환. 변수 df에 저장
exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)
print(df)
print(type(df))
print('\n')

# '수학' 점수 데이터만 선택. 변수 math1에 저장
math1 = df['수학']
print(math1)
print(type(math1))
print('\n')

# '영어' 점수 데이터만 선택. 변수 english에 저장
english = df.영어
print(english)
print(type(english))
```

       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    <class 'pandas.core.frame.DataFrame'>
    
    
    0    90
    1    80
    2    70
    Name: 수학, dtype: int64
    <class 'pandas.core.series.Series'>
    
    
    0    98
    1    89
    2    95
    Name: 영어, dtype: int64
    <class 'pandas.core.series.Series'>
    


```python
# '음악', '체육' 점수 데이터를 선택. 변수 music_gym에 저장
music_gym = df[['음악', '체육']]
print(music_gym)
print(type(music_gym))
print('\n')

# '수학' 점수 데이터만 선택. 변수 math2에 저장
math2 = df[['수학']]
print(math2)
print(type(math2))
```

        음악   체육
    0   85  100
    1   95   90
    2  100   90
    <class 'pandas.core.frame.DataFrame'>
    
    
       수학
    0  90
    1  80
    2  70
    <class 'pandas.core.frame.DataFrame'>
    

##### 원소 선택

* 데이터프레임의 행 인덱스와 열 이름[행, 열] 형식의 2차원 좌표로 입력하여 원소 위치를 지정하는 방밥

* 원소가 위치하는 행과 열 좌표를 입력하면 해당 위치의 원소가 반환됨

* 1개의 행과 2개 이상의 열을 선택하거나 반대로 2개 이상의 행과 1개의 열을 선택하는 경우 시리즈 객체가 반환됨.

* 2개 이상의 행과 2개 이상의 열을 선택하면 데이터프레임 객체를 반환함.


```python
# 예제 1-11

# 1개의 원소를 선택
# DataFrame() 함수로 데이터프레임 반환. 변수 df에 저장
exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)

# '이름' 열을 새로운 인덱스로 지정하고, df 객체에 변경 사항 반영
df.set_index('이름', inplace=True)
print(df)

# 데이터프레임 df의 특정 원소 1개 선택('서준'의 '음악' 점수)
a = df.loc['서준', '음악']
print(a)
b = df.iloc[0,2]
print(b)
```

        수학  영어   음악   체육
    이름                  
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    85
    85
    


```python
# 2개 이상의 원소를 선택(시리즈)
# 데이터프레임 df의 특정 원소 2개 이상 선택('서준'의 '음악','체육' 점수)

c = df.loc['서준', ['음악', '체육']]
print(c)
print('\n')

d = df.iloc[0, [2,3]]
print(d)
print('\n')

e = df.loc['서준', '음악':'체육']
print(e)
print('\n')

f = df.iloc[0, 2:]
print(f)
```

    음악     85
    체육    100
    Name: 서준, dtype: int64
    
    
    음악     85
    체육    100
    Name: 서준, dtype: int64
    
    
    음악     85
    체육    100
    Name: 서준, dtype: int64
    
    
    음악     85
    체육    100
    Name: 서준, dtype: int64
    


```python
# 2개 이상의 원소를 선택(데이터프레임)
# df 2개 이상의 행과 열에 속하는 원소들 선택('서준', '우현'의 '음악', '체육' 점수)

g = df.loc[['서준', '우현'], ['음악', '체육']]
print(g)
print('\n')

h = df.iloc[[0,1], [2,3]]
print(h)
print('\n')

i = df.loc['서준':'우현', '음악':'체육']
print(i)
print('\n')

j = df.iloc[0:2, 2:]
print(j)
```

        음악   체육
    이름         
    서준  85  100
    우현  95   90
    
    
        음악   체육
    이름         
    서준  85  100
    우현  95   90
    
    
        음악   체육
    이름         
    서준  85  100
    우현  95   90
    
    
        음악   체육
    이름         
    서준  85  100
    우현  95   90
    

##### 열 추가

1) 추가하려는 열 이름과 데이터 값을 입력. 마지막 열에 덧붙이듯 새로운 열을 추가.

2) 이때 모든 행에 동일한 값이 입력되는 점에 유의.


```python
# 예제 1-12
# '국어' 열 새로 추가
# 모든 학생들의 국어 점수가 동일하게 80점으로 입력되는 과정을 보여준다

exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)
print(df)
print('\n')

# 데이터프레임 df에 '국어' 점수 열(column) 추가. 데이터 값은 80 지정
df['국어'] = 80
print(df)
```

       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    
    
       이름  수학  영어   음악   체육  국어
    0  서준  90  98   85  100  80
    1  우현  80  89   95   90  80
    2  인아  70  95  100   90  80
    

##### 행 추가

* 추가하려는 행 이름과 데이터 값을 loc 인덱서를 사용하여 입력.

* 하나의 데이터 값을 입력하거나 열의 개수에 맞게 배열 형태로 여러 개의 값(배열의 순서대로 열 위치에 하나씩 추가됨)을 입력할 수 있음

* 행 벡터 자체가 배열이므로, 기존 행을 복사해서 새로운 행에 그대로 추가할 수도 있음

* 데이터프레임에 새로운 행을 추가할 때는 기존 행 인덱스와 겹치지 않는 새로운 인덱스를 사용함

* 기존 인덱스와 중복되는 경우 새로운 행을 추가하지 않고 기존 행의 원소값을 변경함

* 행 인덱스를 지정할 때 기존 인덱스의 순서를 따르지 않아도 됨


```python
exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)
print(df)
print('\n')

# 새로운 행 추가 - 같은 원소 값 입력
df.loc[3] = 0
print(df)
print('\n')
```

       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    
    
       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    3   0   0   0    0    0
    
    
    


```python
# 새로운 행 추가 - 원소 값 여러 개의 배열 입력
df.loc[4] = ['동규', 90, 80, 70, 60]
print(df)
print('\n')

# 세로운 행 추가 - 기존 행 복사
df.loc['행5'] = df.loc[3]
print(df)
```

       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    3   0   0   0    0    0
    4  동규  90  80   70   60
    
    
        이름  수학  영어   음악   체육
    0   서준  90  98   85  100
    1   우현  80  89   95   90
    2   인아  70  95  100   90
    3    0   0   0    0    0
    4   동규  90  80   70   60
    행5   0   0   0    0    0
    

##### 원소 값 변경


```python
# '서준'학생의 '체육' 점수를 선택하여 변경
exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)

# '이름' 열을 새로운 인덱스로 지정하고, df 객체에 변경사항 반영
df.set_index('이름', inplace=True)
print(df)
print('\n')

df.iloc[0][3] = 80
print(df)
print('\n')
```

        수학  영어   음악   체육
    이름                  
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    
    
        수학  영어   음악  체육
    이름                 
    서준  90  98   85  80
    우현  80  89   95  90
    인아  70  95  100  90
    
    
    


```python
df.loc['서준']['체육'] = 90
print(df)
print('\n')
```

        수학  영어   음악  체육
    이름                 
    서준  90  98   85  90
    우현  80  89   95  90
    인아  70  95  100  90
    
    
    


```python
df.loc['서준', '체육'] = 100
print(df)
```

        수학  영어   음악   체육
    이름                  
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    


```python
# 데이터프레임의 원소 여러 개를 변경
df.loc['서준', ['음악', '체육']] = 50
print(df)
print('\n')

df.loc['서준', ['음악', '체육']] = 100, 50
print(df)
```

        수학  영어   음악  체육
    이름                 
    서준  90  98   50  50
    우현  80  89   95  90
    인아  70  95  100  90
    
    
        수학  영어   음악  체육
    이름                 
    서준  90  98  100  50
    우현  80  89   95  90
    인아  70  95  100  90
    

##### 행, 열의 위치 바꾸기


```python
exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)
print(df)
print('\n')

# 데이터프레임 df를 전치하기(메소드 활용)
df = df.transpose()
print(df)
print('\n')

# 데이터프레임 df를 다시 전치하기
df = df.T
print(df)
```

       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    
    
          0   1    2
    이름   서준  우현   인아
    수학   90  80   70
    영어   98  89   95
    음악   85  95  100
    체육  100  90   90
    
    
       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    

##### 특정 열을 행 인덱스로 설정

* set_index() 메소드를 사용하여 데이터프레임의 특정 열을 행 인덱스로 설정함


```python
exam_data = {'이름' : ['서준', '우현', '인아'],
            '수학' : [90, 80, 70],
            '영어' : [98, 89, 95],
            '음악' : [85, 95, 100],
            '체육' : [100, 90, 90]}
df = pd.DataFrame(exam_data)
print(df)
print('\n')

# 특정 열(column)을 데이터프레임의 행 인덱스로 설정
ndf = df.set_index(['이름'])
print(ndf)
print('\n')

ndf2 = ndf.set_index('음악')
print(ndf2)
print('\n')

ndf3 = ndf.set_index(['수학', '음악'])
print(ndf3)
```

       이름  수학  영어   음악   체육
    0  서준  90  98   85  100
    1  우현  80  89   95   90
    2  인아  70  95  100   90
    
    
        수학  영어   음악   체육
    이름                  
    서준  90  98   85  100
    우현  80  89   95   90
    인아  70  95  100   90
    
    
         수학  영어   체육
    음악              
    85   90  98  100
    95   80  89   90
    100  70  95   90
    
    
            영어   체육
    수학 음악          
    90 85   98  100
    80 95   89   90
    70 100  95   90
    

##### 행 인덱스 재배열

* reindex() 메소드를 사용하면, 데이터프레임의 행 인덱스를 새로운 배열로 재지정할 수 있음

* 기존 데이터프레임에 존재하지 않는 행 인덱스가 새롭게 추가되는 경우, 그 행의 데이터값은 NaN 값이 입력됨.


```python
dict_data = {'c0':[1,2,3], 'c1':[4,5,6], 'c2':[7,8,9], 'c3':[10,11,12], 'c4':[13,14,15]}

df = pd.DataFrame(dict_data, index=['r0', 'r1', 'r2'])
print(df)
print('\n')

new_index = ['r0', 'r1', 'r2', 'r3', 'r4']
ndf = df.reindex(new_index)
print(ndf)
print('\n')

new_index = ['r0', 'r1', 'r2', 'r3', 'r4']
ndf2 = df.reindex(new_index, fill_value=0)
print(ndf2)
```

        c0  c1  c2  c3  c4
    r0   1   4   7  10  13
    r1   2   5   8  11  14
    r2   3   6   9  12  15
    
    
         c0   c1   c2    c3    c4
    r0  1.0  4.0  7.0  10.0  13.0
    r1  2.0  5.0  8.0  11.0  14.0
    r2  3.0  6.0  9.0  12.0  15.0
    r3  NaN  NaN  NaN   NaN   NaN
    r4  NaN  NaN  NaN   NaN   NaN
    
    
        c0  c1  c2  c3  c4
    r0   1   4   7  10  13
    r1   2   5   8  11  14
    r2   3   6   9  12  15
    r3   0   0   0   0   0
    r4   0   0   0   0   0
    

##### 행 인덱스 초기화

* reset_index() 메소드를 활용하여 행 인덱스를 정수형 위치 인덱스로 초기화함

* 기존 행 인덱스는 열로 이동. 새로운 데이터프레임 객체 반환


```python
dict_data = {'c0':[1,2,3], 'c1':[4,5,6], 'c2':[7,8,9], 'c3':[10,11,12], 'c4':[13,14,15]}

df = pd.DataFrame(dict_data, index=['r0', 'r1', 'r2'])
print(df)
print('\n')

# 행 인덱스를 정수형으로 초기화
ndf = df.reset_index()
print(ndf)
```

        c0  c1  c2  c3  c4
    r0   1   4   7  10  13
    r1   2   5   8  11  14
    r2   3   6   9  12  15
    
    
      index  c0  c1  c2  c3  c4
    0    r0   1   4   7  10  13
    1    r1   2   5   8  11  14
    2    r2   3   6   9  12  15
    

##### 행 인덱스를 기준으로 데이터프레임 정렬

* sort_index() 메소드로 행 인덱스를 기준으로 정렬

* ascending 옵션을 사용하여 오름차순 또는 내림차순 설정


```python
dict_data = {'c0':[1,2,3], 'c1':[4,5,6], 'c2':[7,8,9], 'c3':[10,11,12], 'c4':[13,14,15]}

df = pd.DataFrame(dict_data, index=['r0', 'r1', 'r2'])
print(df)
print('\n')

# 내림차순으로 행 인덱스 정렬
ndf = df.sort_index(ascending=False)
print(ndf)
```

        c0  c1  c2  c3  c4
    r0   1   4   7  10  13
    r1   2   5   8  11  14
    r2   3   6   9  12  15
    
    
        c0  c1  c2  c3  c4
    r2   3   6   9  12  15
    r1   2   5   8  11  14
    r0   1   4   7  10  13
    

#### 산술연산

##### 시리즈 연산

* 시리즈 객체에 어떤 숫자를 더하면 시리즈의 개별 원소에 각각 숫자를 더하고 계산한 결과를 시리즈 객체로 반환


```python
student1 = pd.Series({'국어':100, '영어':80, '수학':90})
print(student1)
print('\n')

# 학생의 과목별 점수를 200으로 나누기
percentage = student1/200

print(percentage)
print('\n')
print(type(percentage))
```

    국어    100
    영어     80
    수학     90
    dtype: int64
    
    
    국어    0.50
    영어    0.40
    수학    0.45
    dtype: float64
    
    
    <class 'pandas.core.series.Series'>
    

* 시리즈 vs 시리즈


```python
student1 = pd.Series({'국어':100, '영어':80, '수학':90})
student2 = pd.Series({'수학':80, '국어':90, '영어':80})

print(student1)
print('\n')
print(student2)
print('\n')

# 두 학생의 과목별 점수로 사칙연산 수행
add = student1 + student2
sub = student1 - student2
mul = student1 * student2
div = student1 / student2
print(type(div))
print('\n')

# 사칙연산 결과를 데이터프레임으로 합치기
result = pd.DataFrame([add, sub, mul, div], index=['덧셈', '뺄셈', '곱셈', '나눗셈'])
print(result)
```

    국어    100
    영어     80
    수학     90
    dtype: int64
    
    
    수학    80
    국어    90
    영어    80
    dtype: int64
    
    
    <class 'pandas.core.series.Series'>
    
    
                  국어        수학      영어
    덧셈    190.000000   170.000   160.0
    뺄셈     10.000000    10.000     0.0
    곱셈   9000.000000  7200.000  6400.0
    나눗셈     1.111111     1.125     1.0
    

##### 연산 메소드 활용

* 객체 사이에 공통 인덱스가 없는 경우 NaN으로 반환.
        -> 이런 상황을 위해 연산 메소드에 fill_value 옵션을 설정


```python
import pandas as pd
import seaborn as sns

titanic = sns.load_dataset('titanic')
df = titanic.loc[:, ['age', 'fare']]
print(df.head())
print('\n')
print(type(df))
print('\n')

# 데이터프레임에 숫자 df 더하기
addition = df + 10
print(addition.head())
print('\n')
print(type(addition))
```

        age     fare
    0  22.0   7.2500
    1  38.0  71.2833
    2  26.0   7.9250
    3  35.0  53.1000
    4  35.0   8.0500
    
    
    <class 'pandas.core.frame.DataFrame'>
    
    
        age     fare
    0  32.0  17.2500
    1  48.0  81.2833
    2  36.0  17.9250
    3  45.0  63.1000
    4  45.0  18.0500
    
    
    <class 'pandas.core.frame.DataFrame'>
    


```python
subtraction = addition - df
print(subtraction.tail())
print('\n')
print(type(subtraction))
```

          age  fare
    886  10.0  10.0
    887  10.0  10.0
    888   NaN  10.0
    889  10.0  10.0
    890  10.0  10.0
    
    
    <class 'pandas.core.frame.DataFrame'>
    
