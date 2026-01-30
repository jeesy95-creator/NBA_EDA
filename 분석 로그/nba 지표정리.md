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
 - PTS
 - FGM, FGA
 - FG_PCT
 - FG3M, FG3A, FG3_PCT
 - FTM, FTA, FT_PCT

#### 플레이메이킹
 - AST

#### 리바운드
 - OREB
 - DREB
 - REB

#### 수비
 - STL
 - BLK

#### 실수/파울
 - TO
 - PF

#### 임팩트
 - PLUS_MINUS

### 파생 지표 

#### 시간 보정
 - Minutes_float
 - PTS_per36 = PTS / MIN * 36
 - REB_per36, AST_per36, STL_per36, BLK_per36

#### 슈팅 효율
 - eFG%
 - (FGM + 0.5 * FG3M) / FGA
 - TS%
 - PTS / (2 * (FGA + 0.44 * FTA))
 - 3PA Rate
 - FG3A / FGA
 - FT Rate
 - FTA / FGA

#### 플레이 스타일
 - AST_TO = AST / TO
 - USG Proxy (단순)
 - (FGA + 0.44*FTA + TO) / MIN

#### 리바운드 성향
 - OREB_RATIO = OREB / REB
 - DREB_RATIO = DREB / REB

#### 임팩트 조합
 - IMPACT_SCORE (예시)
 - PTS + REB + AST + STL + BLK - TO
 - PER Proxy
 - (PTS + REB + AST + STL + BLK) / MIN

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
 - PTS_home, PTS_away
 - FG_PCT_home, FG_PCT_away
 - FG3_PCT_home, FG3_PCT_away
 - FT_PCT_home, FT_PCT_away
 - AST_home, AST_away
 - REB_home, REB_away
 - HOME_TEAM_WINS

### 파생 지표

#### 경기 지배력
 - Point_Diff = PTS_home - PTS_away
 - AST_Diff = AST_home - AST_away
 - REB_Diff = REB_home - REB_away

#### 슈팅 우위
 - FG_PCT_DIFF
 - FG3_PCT_DIFF
 - FT_PCT_DIFF

#### 홈코트 효과
 - HOME_ADV_SCORE =
   (PTS_home - PTS_away) + 
   (AST_home - AST_away) + 
   (REB_home - REB_away)

#### 승리 확률용 Feature
 - ABS_POINT_DIFF
 - TOTAL_POINTS = PTS_home + PTS_away

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
 - W
 - L
 - W_PCT
 - HOME_RECORD
 - ROAD_RECORD

### 파생 지표

#### 홈 의존도
 - HOME_WIN_PCT
 - ROAD_WIN_PCT
 - HOME_DEPENDENCY
 - HOME_WIN_PCT - ROAD_WIN_PCT

#### 시즌 파워 지표
 - NET_WINS = W - L
 - LOG_WINS = log(W+1)

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