# 데이터 
 - games.csv (팀-경기 단위)
 - games_details_shortver.csv (선수-경기 단위)
 - ranking_stver.csv (팀-시즌 단위)


# 지표 종류
 - 원천 지표(Raw Metrics)
 - 파생 지표(Engineered Metrics)


## games_details_shortver.csv (선수-경기 단위)

### 원천 지표

#### 출전
   - MIN (출전 시간)

#### 득점/슛
 - PTS = 팀이 한 경기에서 기록한 총 득점
 - FGM, FGA = 성공/시도한 야투 개수
 - FG_PCT = 야투 성공률(FGM/FGA)
 - FG3M, FG3A, FG3_PCT = 성공/시도한 3점 슛, 3점 성공률 (FG3M/GF3A)
 - FTM, FTA, FT_PCT = 성공/시도한 자유투, 자우투 성공률(FTM/FTA)

#### 플레이메이킹
 - AST = 어시스트

#### 리바운드
 - OREB = 공격 리바운드
 - DREB = 수비 리바운드
 - REB = 총 리바운드( OREB + DREB)

#### 수비
 - STL = 스틸(수비 중 패스나 드리블을 가로채서 상대 공격권을 바로 빼앗는 행위)
 - BLK = 블록(슛 직접  쳐내는 수비)

#### 실수/파울
 - TO = 턴오버
 - PF = 개인 파울 

#### 임팩트
 - PLUS_MINUS = 해당 팀(또는 선수)이 코트에 있을 때 우리 팀 득점 − 상대 팀 득점

### 파생 지표 

#### 시간 보정
 - Minutes_float
 - PTS_per36 = PTS / MIN * 36
 - REB_per36, AST_per36, STL_per36, BLK_per36

#### 슈팅 효율
 - eFG%(3점 가치를 반영한 슈팅 효율)
   = (FGM + 0.5 * FG3M) / FGA
 - TS%(2점, 3점, 자유투까지 포함한 종합 득점 효율)
   = PTS / (2 * (FGA + 0.44 * FTA))
 - 3PA Rate(전체 슛 중 3점 비중)
   = FG3A / FGA
 - FT Rate(슛 대비 자유투 유도 능력)
   = FTA / FGA

#### 플레이 스타일
 - AST_TO = AST / TO(어시스트 대비 턴오버)
 - USG Proxy (단순)(공격 시 얼마나 자주 관여하는가,
   슛, 자유투, 턴오버를 통해 공격 점유 성향)
   = (FGA + 0.44*FTA + TO) / MIN

#### 리바운드 성향
 - OREB_RATIO = OREB / REB(공격 리바운드 비율)
 - DREB_RATIO = DREB / REB(수비 리바운드 비율)

#### 임팩트 조합
 - IMPACT_SCORE (예시)(한 경기에서의 종합 기여도, 득점, 리바운드, 패스, 수비 기여를 더하고
   턴오버는 감점)
   = PTS + REB + AST + STL + BLK - TO
 - PER Proxy(출전시간 대비 얼마나 많은 기여를 했는가)
   = (PTS + REB + AST + STL + BLK) / MIN

**계산식**
- 슈팅 효율
```
eFG% =

(FGM + 0.5 * FG3M) / FGA
```
```
TS% = 

PTS / (2 * (FGA + 0.44 * FTA))
```
```
3PA Rate = 

FG3A / FGA
```
```
FT Rate = 

FTA / FGA
```
- 플레이 스타일 
```
AST_TO = AST / TO
```
```
USG Proxy (단순) = 
(FGA + 0.44*FTA + TO) / MIN
```
- 리바운드 성향
```
OREB_RATIO = OREB / REB

DREB_RATIO = DREB / REB
```
-임팩트 조합(예시)
```
IMPACT_SCORE =
PTS + REB + AST + STL + BLK - TO
```
```
PER Proxy =
(PTS + REB + AST + STL + BLK) / MIN
```

## games.csv (팀-경기 단위)

### 원천 지표
 - PTS_home, PTS_away = 홈, 원정 득점 
 - FG_PCT_home, FG_PCT_away = 햐투 홈/원정 
 - FG3_PCT_home, FG3_PCT_away = 3점 홈/원정 
 - FT_PCT_home, FT_PCT_away = 자유투 홈/원정
 - AST_home, AST_away = 팀 평균 어시스트 홈/원정
 - REB_home, REB_away = 팀 평균 리바운드 홈/원정 
 - HOME_TEAM_WINS = 해당 경기에서 홈팀이 이겼는지 (0/1)

### 파생 지표

#### 경기 지배력
 - Point_Diff = PTS_home - PTS_away
 - AST_Diff = AST_home - AST_away
 - REB_Diff = REB_home - REB_away

#### 슈팅 우위
 - FG_PCT_DIFF = 야투 지배력
 - FG3_PCT_DIFF = 3점 지배력
 - FT_PCT_DIFF = 자유투 지배력 

#### 홈코트 효과
 - HOME_ADV_SCORE =
   (PTS_home - PTS_away) + 
   (AST_home - AST_away) + 
   (REB_home - REB_away)

#### 승리 확률용 Feature
 - ABS_POINT_DIFF = 득점 차이의 절댓값
 - TOTAL_POINTS = PTS_home + PTS_away(총 득점)

**계산식**
- 홈코트 효과
```
HOME_ADV_SCORE = 
(PTS_home - PTS_away) + 
(AST_home - AST_away) + 
(REB_home - REB_away)
```

## ranking_stver.csv (팀-시즌 단위)

### 원천 지표
 - W = 시즌 승리 횟수
 - L = 시즌 패배 횟수
 - W_PCT = winning percentage(승리 퍼센트)
 - HOME_RECORD = 홈 경기 성적
 - ROAD_RECORD = 어웨이 성적 

### 파생 지표

#### 홈 의존도
 - HOME_WIN_PCT = 홈 승률
 - ROAD_WIN_PCT = 원정 승률
 - HOME_DEPENDENCY(홈 의존도)
   = HOME_WIN_PCT - ROAD_WIN_PCT

#### 시즌 파워 지표
 - NET_WINS = W - L(승패차이)
 - LOG_WINS = log(W+1)(승수 분포 스케일 완화)

**계산식**
```
HOME_DEPENDENCY =
HOME_WIN_PCT - ROAD_WIN_PCT
```

## ☑️ 크로스 파일 결합 파생지표

### (선수 → 팀 → 시즌)
 - 팀 평균 선수 효율
 - TEAM_AVG_TS%
 - TEAM_AVG_eFG%
 - TEAM_AVG_PTS_per36
 - TEAM_AVG_AST_TO

### 팀 선수 구성 비율
스코어러형 비율
수비형 비율
플레이메이커형 비율

### 승률 예측용 매트릭스
```
X = [TEAM_AVG_TS%, TEAM_AVG_AST_TO,
     TEAM_AVG_REB_per36,
     HOME_DEPENDENCY]
y = W_PCT
```

🎯 핵심 원천 지표

PTS, FGA, FG3A, FTA, AST, REB, STL, BLK, TO, PLUS_MINUS

🔥 핵심 파생 지표

TS%, eFG%, per36, AST/TO, FG3A Rate, HOME_DEPENDENCY

🚀 추천 우선순위

1️⃣ Minutes 파싱 + per36
2️⃣ TS%, eFG%
3️⃣ 팀 평균 선수 효율
4️⃣ 홈 의존도