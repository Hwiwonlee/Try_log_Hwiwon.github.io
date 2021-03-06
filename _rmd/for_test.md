
```r
# step 1. input csv files


# step 2. check the datasets

str(gj_2002)
str(gj_2003)
head(gy_2002)
head(gy_2003)
str(jk_2002)
str(jk_2003)

# 12.4 data handling부터 하는 게 맞는 것 같음.
## dim
gj_2002 # 294,729 x 3
gj_2003 # 310,210 x 3
gy_2002 # 3,794,437 x 3
gy_2003 # 4,353,681 x 3
jk_2002 # 514,866 x 3
jk_2003 # 514,531 x 3

## NA
sum(is.na(gj_2002))
sum(is.na(gj_2003))
sum(is.na(gy_2002))
sum(is.na(gy_2003))
sum(is.na(jk_2002)) # 514866개 NA
sum(is.na(jk_2003)) # 513211개 NA 

sum(is.na(jk_2002[, 1]))
sum(is.na(jk_2002[, 2]))
sum(is.na(jk_2002[, 3])) # 514866


sum(is.na(jk_2003[, 1]))
sum(is.na(jk_2003[, 2]))
sum(is.na(jk_2003[, 3])) # 513211

## : 자격 data의 3열에서 AN 발견 > '사망 연월일'이 없는 것

## Duplicated values
gj_2002$PERSON_ID[duplicated(gj_2002$PERSON_ID)]
gj_2003$PERSON_ID[duplicated(gj_2003$PERSON_ID)]

length(gy_2002$PERSON_ID[duplicated(gy_2002$PERSON_ID)]) # 3345683개 중복 ID
length(gy_2003$PERSON_ID[duplicated(gy_2003$PERSON_ID)]) # 3895450개 중복 ID

jk_2002$PERSON_ID[duplicated(jk_2002$PERSON_ID)]
jk_2003$PERSON_ID[duplicated(jk_2003$PERSON_ID)]

## : 진료 data에서 중복 진료 기록 발견 

# step 3. solve the problems

# Q1.
nrow(gj_2002) + nrow(gj_2003) ## 294729 + 310210 = total 검진 횟수 604939
m_gj <- merge(gj_2002, gj_2003, by = "PERSON_ID") ## 중복 ID 체크 
nrow(m_gj) ## 중복 ID 90144명

## 12.3 approach
nrow(gj_2002) # 2002년 : 294729명
nrow(gj_2003) # 2003년 : 310210명

gj_2002 %>% merge(gj_2003, by = "PERSON_ID") %>% nrow() # 2002년과 2003년에 검진받은 사람 : 90114명

# 따라서 02-03년에 건강검진을 받은 사람은
nrow(gj_2002) + nrow(gj_2003) - 90114 # 514825명

# 12.4 duplicate를 이용한 중복체크 시행 후 다시 해보기.
length(rbind(gj_2002, gj_2003)$PERSON_ID[duplicated(rbind(gj_2002, gj_2003)$PERSON_ID)]) # 02+03년 중복 검진자는 마찬가지로 90144명

# Q2.
gj_1 <- (nrow(gj_2002) + nrow(gj_2003)) - nrow(m_gj)
gj_2 <- nrow(m_gj)
mean_gj <- ((1*gj_1) + (2*gj_2)) / 2  ## 평균 
sd_gj <- sqrt((((gj_1-mean_gj)^2) + ((gj_2-mean_gj)^2)) / 2) ## 표준편차 
# 따라서 347541.5 ± 217056.7 

## 12.3 approach : 위와 다름. 
mean(nrow(gj_2002), nrow(gj_2003))
sd(c(nrow(gj_2002), nrow(gj_2003))) # 따라서 294729 ± 10946.72번

## 12. 6 approach 
mean(nrow(gj_2002), nrow(gj_2003))!=mean(c(nrow(gj_2002), nrow(gj_2003)))
# mean(nrow(gj_2002), nrow(gj_2003))으로 선언하면 arg에는 nrow(gj_2002)만 들어가서 nrow(gj_2002)가 return됨.
# mean(c(nrow(gj_2002), nrow(gj_2003)))가 맞음. 


# Q3.
str(jk_2002)
sum(is.na(jk_2002$DTH_MDY)) ## 02년의 경우 사망일자가 모두 NA
str(jk_2003)
sum(is.na(jk_2003$DTH_MDY)) ## 03년의 경우 514531 - 513211 = 1320명의 사망자 존재. 
DTH_jk_2003 <- na.omit(jk_2003)
str(DTH_jk_2003)
m_jk <- merge(jk_2002, jk_2003, by = "PERSON_ID")
str(m_jk)
total_jk = nrow(jk_2002) + nrow(jk_2003) - nrow(m_jk) ## jk dataset 기준 중복을 제외한 총 obs 수
D_percent = nrow(DTH_jk_2003) / total_jk ## jk dataset 기준 사망자 비율 
m_D_jk03_gj02 <- merge(DTH_jk_2003, gj_2002, by = "PERSON_ID")
m_D_jk03_gj03 <- merge(DTH_jk_2003, gj_2003, by = "PERSON_ID")
str(m_D_jk03_gj02) ## 1047명 
str(m_D_jk03_gj03) ## 319명, 사망자 총합 1320명을 초과한다. 
m_D_find_overlap <- merge(m_D_jk03_gj02, m_D_jk03_gj03, by = "PERSON_ID")
str(m_D_find_overlap) ## 46명, 02, 03년에 검진받고 03년에 사망한 환자로 보임. 
head(m_D_jk03_gj02)
mean(as.Date(as.character(m_D_jk03_gj02$DTH_MDY), "%Y%m%d") - as.Date(as.character(m_D_jk03_gj02$HME_DT), "%Y%m%d") + 
       as.Date(as.character(m_D_jk03_gj03$DTH_MDY), "%Y%m%d") - as.Date(as.character(m_D_jk03_gj03$HME_DT), "%Y%m%d"))
sd(as.Date(as.character(m_D_jk03_gj02$DTH_MDY), "%Y%m%d") - as.Date(as.character(m_D_jk03_gj02$HME_DT), "%Y%m%d") + 
       as.Date(as.character(m_D_jk03_gj03$DTH_MDY), "%Y%m%d") - as.Date(as.character(m_D_jk03_gj03$HME_DT), "%Y%m%d"))

## 12.3 approach
# 3.1 사망자 (사망일이 있는 경우) (N, %)
na.omit(jk_2002) # 사망자 0명 
sum(is.na(jk_2002['DTH_MDY'])) # 사망자 0명 다시 확인
na.omit(jk_2003) # 03년, 1320명의 사망자 확인

# 3.2 첫 검진으로부터 사망일까지의 평균기간(일) (Mean ± SD)
na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID') %>% nrow() # 02년 검진, 03년 사망 : 1047명
na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID') %>% nrow() # 03년 검진, 03년 사망 : 319명
na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID') %>% merge(gj_2003, key='PERSON_ID') # 02, 03년 검진, 03년 사망 : 0명, 따라서 1047명, 319명의 첫검진은 중복되지 않음.

# 3.2.1 03년의 사망자는 1320명인데, 위의 merge의 사망자를 합치면 1366명이다. 46명은 어디서 나온걸까? 
# na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID') %>% merge(gj_2003, key='PERSON_ID')의 결과가 0명이라는건
# 사망자의 중복검진 case도 없다는 얘기다. 이게 exercise data라 차이가 난다고 생각하자. 
# 12. 6 approach : 실제로 중복이 있다. 
rbind(na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID'), na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID')) %>%
  distinct(PERSON_ID, .keep_all = TRUE) # 'distinct'를 이용한 unique value의 total은 1320명

rbind(na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID'), na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID'))[
  which(duplicated(rbind(na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID'), na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID'))$PERSON_ID) | 
    duplicated(rbind(na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID'), na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID'))$PERSON_ID, fromLast = TRUE))
        , ] %>%
  arrange(PERSON_ID) # 46명에 대한 중복자료, 총 92개의 value를 확인했다. 
# 그러니까 전체는 1320명이 맞고 merge의 사망자가 1366명인건 02년, 03년 검진받은 사람이 총 46명 있기 때문이다. 

# 02, 03년 중복 검진한 경우 '최초 검진날짜'만 뽑으려면? 
ol_32$DTH_MDY = as.Date(as.character(ol_32$DTH_MDY), format = "%Y%m%d")
ol_32 %>% distinct(PERSON_ID, .keep_all = TRUE)
ol_32 # 더 이른 검진날짜가 중복값 중 앞 쪽에 오도록 정렬시킨 후 distinct를 이용해 unique value만 뽑아내면 된다. 확인 완료.

Q_32 = tbl_df(rbind(na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID'), na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID')))
Q_32$DTH_MDY = as.Date(as.character(Q_32$DTH_MDY), format = "%Y%m%d")
Q_32 = Q_32 %>% arrange(HME_DT)


ol_Q32 = Q_32[which(duplicated(Q_32$PERSON_ID) | duplicated(Q_32$PERSON_ID, fromLast = TRUE)), ]
ot_Q32 = Q_32[-which(duplicated(Q_32$PERSON_ID) | duplicated(Q_32$PERSON_ID, fromLast = TRUE)), ]

mean(c(abs(ol_Q32$HME_DT - ol_Q32$DTH_MDY), abs(ot_Q32$HME_DT - ot_Q32$DTH_MDY)))
sd(c(abs(ol_Q32$HME_DT - ol_Q32$DTH_MDY), abs(ot_Q32$HME_DT - ot_Q32$DTH_MDY)))

mean(c(abs(rbind(ol_Q32, ot_Q32)$HME_DT - rbind(ol_Q32, ot_Q32)$DTH_MDY)))
sd(c(abs(rbind(ol_Q32, ot_Q32)$HME_DT - rbind(ol_Q32, ot_Q32)$DTH_MDY)))


gj02_d03 = tbl_df(na.omit(jk_2003) %>% merge(gj_2002, key='PERSON_ID'))
gj03_d03 = tbl_df(na.omit(jk_2003) %>% merge(gj_2003, key='PERSON_ID'))

mean_gj02_d03 = mean(as.Date(as.character(gj02_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj02_d03$HME_DT), format = "%Y%m%d"))
mean_gj03_d03 = mean(as.Date(as.character(gj03_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj03_d03$HME_DT), format = "%Y%m%d"))

mean(300.6514, 100.0439) # 검진일로부터의 평균 사망일 
sd(c(300.6514, 100.0439)) # 검진일로부터의 사망일의 표준편차, 틀림. 검진일로부터의 평균 사망일의 표준편차임. 

mean(as.double(mean_gj02_d03), as.double(mean_gj03_d03))
sd(c(as.double(mean_gj02_d03), as.double(mean_gj03_d03)))


# 검진일로부터의 평균 사망일 
mean(mean(as.integer(as.Date(as.character(gj02_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj02_d03$HME_DT), format = "%Y%m%d"))), 
      mean(as.integer(as.Date(as.character(gj03_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj03_d03$HME_DT), format = "%Y%m%d"))))

# 검진일로부터의 사망일의 표준편차 
sd(c(mean(as.integer(as.Date(as.character(gj02_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj02_d03$HME_DT), format = "%Y%m%d"))), 
     mean(as.integer(as.Date(as.character(gj03_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj03_d03$HME_DT), format = "%Y%m%d")))))

# 12. 6 approach
mean(c(300.6514, 100.0439))
mean(c(as.double(mean_gj02_d03), as.double(mean_gj03_d03)))
mean(c(mean(as.integer(as.Date(as.character(gj02_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj02_d03$HME_DT), format = "%Y%m%d"))), 
      mean(as.integer(as.Date(as.character(gj03_d03$DTH_MDY), format = "%Y%m%d") - as.Date(as.character(gj03_d03$HME_DT), format = "%Y%m%d")))))

## Indexing에 따른 output의 차이.
## a["DTH_MDY"]로 as.Date하려다가 30분 넘게 날림. as.Date는 value 하나씩 바꾸는 건데, a["DTH_MDY"]로 arg를 넣으면
## value가 아니라 data frame이 들어가서 작동이 안된다. 그러나 a$DTH_MDY는 vector-wise의 indexing이므로 as.Date에 적절히 들어간다. 

head(a$DTH_MDY)
class(a$DTH_MDY)
head(a["DTH_MDY"])
class(a["DTH_MDY"])

# Q4. 
## 12.3 approach
# 4.1 뇌졸중 (MAIN_SICK = I60 ~ I64)을 진단받은 자 (N, %)

c = gy_2002[c(grep(c("160"), gy_2002$MAIN_SICK), grep(c("161"), gy_2002$MAIN_SICK), grep(c("162"), gy_2002$MAIN_SICK), grep(c("163"), gy_2002$MAIN_SICK), grep(c("164"), gy_2002$MAIN_SICK)), ]
d = gy_2003[c(grep(c("160"), gy_2003$MAIN_SICK), grep(c("161"), gy_2003$MAIN_SICK), grep(c("162"), gy_2003$MAIN_SICK), grep(c("163"), gy_2003$MAIN_SICK), grep(c("164"), gy_2003$MAIN_SICK)), ]
head(c)
## 중복 체크 : 1) 02 검진 02 진단 03 다시 검진, 2) 02검진 02진단 02 다시 검진 
c$PERSON_ID[duplicated(c$PERSON_ID)]
filter(c, PERSON_ID == "19605309")

nrow(c) + nrow(d) # 뇌졸중 진단자, 29436명 
(nrow(c) + nrow(d))/(nrow(gy_2002) + nrow(gy_2003)) # 전체 진단 환자의 0.0036%

## 12.4 approach
CI02 = gy_2002[c(grep(c("160"), gy_2002$MAIN_SICK), grep(c("161"), gy_2002$MAIN_SICK), grep(c("162"), gy_2002$MAIN_SICK), grep(c("163"), gy_2002$MAIN_SICK), grep(c("164"), gy_2002$MAIN_SICK)), ]
CI03 = gy_2003[c(grep(c("160"), gy_2003$MAIN_SICK), grep(c("161"), gy_2003$MAIN_SICK), grep(c("162"), gy_2003$MAIN_SICK), grep(c("163"), gy_2003$MAIN_SICK), grep(c("164"), gy_2003$MAIN_SICK)), ]

CI02 = gy_2002[grep("16(0|1|2|3|4)", gy_2002$MAIN_SICK), ] # 12. 5
CI02 = gy_2002[grep("16(0|1|2|3|4)", gy_2002$MAIN_SICK), ] # 12. 5 REX를 이용한 깔끔한 표현

# CI02와 위 코드가 일치하는지 확인
sum(arrange(CI02, PERSON_ID, RECU_FR_DT, MAIN_SICK) == arrange(gy_2002[grep("16(0|1|2|3|4)", gy_2002$MAIN_SICK), ], PERSON_ID, RECU_FR_DT, MAIN_SICK))
sum(arrange(CI03, PERSON_ID, RECU_FR_DT, MAIN_SICK) == arrange(gy_2003[grep("16(0|1|2|3|4)", gy_2003$MAIN_SICK), ], PERSON_ID, RECU_FR_DT, MAIN_SICK))
# 일치함

CI02 # 15,009 x 3
CI03 # 14,427 x 3

## 12. 4 중복 진단? 
length(CI02$PERSON_ID[duplicated(CI02$PERSON_ID)]) # 02년 CI진단 : 4948명 중복 
length(CI03$PERSON_ID[duplicated(CI03$PERSON_ID)]) # 03년 CI진단 : 5208명 중복 

CI02 %>% merge(CI03, key="PERSON_ID") ## CI02와 CI03 간 중복은 없음. 

### CI02와 CI03 내의 중복이 정말인가? 
### CI02의 경우 
ol_CI02 = CI02[duplicated(CI02$PERSON_ID), ] # duplicated 결과를 저장 
sum(duplicated(CI02$PERSON_ID)) # 4948개 확인

arrange(ol_CI02, PERSON_ID) # 10001506가 한 개뿐인데?
sum(ol_CI02$PERSON_ID == 10074992) # 1. 진짜 한 개다. 

### 혹시 dulicated를 시행하면 중복값의 하나를 제외하고 가져오는 건가? 
### 가령, CI02에 10074992가 2개가 있는데, duplicated 결과를 indexing으로 불러오면 하나만 가져와지는거지. 
sum(CI02$PERSON_ID == 10074992) # CI02는 10074992가 중복값으로 되어 있지만 
sum(ol_CI02$PERSON_ID == 10074992) # ol_CI02는 10074992가 단일값으로 되어 있다. 

#### 왜 이런 결과가 생길까? 그리고 어떻게 하면 중복값만 정확히 따로 뺄 수 있을까? ####

### 1) 왜 이런 결과가 생길까?
k = c(1,2,3,4,5,6,1)
duplicated(k) # 중복값의 '최초값'은 FALSE이다. 그러니까 중복된 횟수만큼의 값이 return되는 게 dulicated()이다. 

### 2) 어떻게 해결할 수 있을까? 
duplicated(k) | duplicated(k, fromLast = T) # 최초값도 TRUE로 만들었다. 
# 앞에서부터 중복검사 or 뒤에서부터 중복검사로 해결할 수 있었다. [출처](https://lightblog.tistory.com/18)



#### 다시 과제로 ####
CI02[which(duplicated(CI02$PERSON_ID) | duplicated(CI02$PERSON_ID, fromLast=T) ), ] %>% 
  arrange(PERSON_ID) # 7,537 x 3

CI03[which(duplicated(CI03$PERSON_ID) | duplicated(CI03$PERSON_ID, fromLast=T) ), ] %>% 
  arrange(PERSON_ID) # 7,689 x 3

# ID가 중복된 것만 따로 저장. 
ol_CI02 = CI02[which(duplicated(CI02$PERSON_ID) | duplicated(CI02$PERSON_ID, fromLast=T) ), ]
ol_CI03 = CI03[which(duplicated(CI03$PERSON_ID) | duplicated(CI03$PERSON_ID, fromLast=T) ), ]

ot_CI02 = CI02[-which(duplicated(CI02$PERSON_ID) | duplicated(CI02$PERSON_ID, fromLast=T) ), ]
ot_CI03 = CI03[-which(duplicated(CI03$PERSON_ID) | duplicated(CI03$PERSON_ID, fromLast=T) ), ]

nrow(ol_CI02) + nrow(ot_CI02)
nrow(CI02) # nrow(ol_CI02) + nrow(ot_CI02) = nrow(CI02)

nrow(ol_CI03) + nrow(ot_CI03)
nrow(CI03) # nrow(ol_CI03) + nrow(ot_CI03) = nrow(CI03)

sum(unique(ot_CI02) != ot_CI02) # unique(ot_CI02)와 ot_CI02가 같음을 확인 = ot_CI02는 unique값만 들어있다. 
sum(unique(ot_CI03) != ot_CI03) # unique(ot_CI03)와 ot_CI03가 같음을 확인 = ot_CI03는 unique값만 들어있다. 

# CI02와 CI03간 중복값은 없으며 
# CI02는 7,537개의 중복값, CI03은 7,689개의 중복값을 갖고 있다. 

## 중복진단 끝.

# 다시, 뇌졸중을 진단받은 총 사람수는 ?
# 전체 사람 수 구하기
gy_rbind = rbind(gy_2002, gy_2003)

nrow(gy_rbind[which(duplicated(gy_rbind$PERSON_ID) | duplicated(gy_rbind$PERSON_ID, fromLast=T) ), ]) + 
  nrow(gy_rbind[-which(duplicated(gy_rbind$PERSON_ID) | duplicated(gy_rbind$PERSON_ID, fromLast=T) ), ])
nrow(gy_rbind) # gy_rbind에서 id 중복된 ID개수 + gy_rbind에서 id 중복되지 ID개수 = gy_rbind의 ID개수 

# 전체 사람 수는 466029 + 23247 = 489276
length(unique(gy_rbind[which(duplicated(gy_rbind$PERSON_ID) | duplicated(gy_rbind$PERSON_ID, fromLast=T) ), ]$PERSON_ID)) +
  length(unique(gy_rbind[-which(duplicated(gy_rbind$PERSON_ID) | duplicated(gy_rbind$PERSON_ID, fromLast=T) ), ]$PERSON_ID))

# 뇌졸중 진단받은 사람의 수 19280
nrow(ot_CI02) + nrow(ot_CI03) + length(unique(ol_CI02$PERSON_ID)) + length(unique(ol_CI03$PERSON_ID))

# 전체 환자의 0.0394%가 뇌졸중 진단을 받았다. 
(nrow(ot_CI02) + nrow(ot_CI03) + length(unique(ol_CI02$PERSON_ID)) + length(unique(ol_CI03$PERSON_ID)))/ 
  (length(unique(gy_rbind[which(duplicated(gy_rbind$PERSON_ID) | duplicated(gy_rbind$PERSON_ID, fromLast=T) ), ]$PERSON_ID)) +
     length(unique(gy_rbind[-which(duplicated(gy_rbind$PERSON_ID) | duplicated(gy_rbind$PERSON_ID, fromLast=T) ), ]$PERSON_ID)))
  

# 4.2 첫 검진으로부터 첫번째 뇌졸중 진단일까지의 평균기간(일) (Mean ± SD)
e = gj_2002 %>% merge(c, key = 'PERSON_ID')
f = gj_2003 %>% merge(c, key = 'PERSON_ID')

gj_2002 %>% merge(c, key = 'PERSON_ID') %>% merge(d, key = 'PERSON_ID') 
gj_2002 %>% merge(d, key = 'PERSON_ID') %>% merge(gj_2003, key = 'PERSON_ID')

g = gj_2002 %>% merge(d, key = 'PERSON_ID')
h = gj_2003 %>% merge(d, key = 'PERSON_ID')

mean_e = mean(as.Date(as.character(e$RECU_FR_DT), format = "%Y%m%d") - as.Date(as.character(e$HME_DT), format = "%Y%m%d"))
mean_f = mean(as.Date(as.character(f$RECU_FR_DT), format = "%Y%m%d") - as.Date(as.character(f$HME_DT), format = "%Y%m%d"))
mean_g = mean(as.Date(as.character(g$RECU_FR_DT), format = "%Y%m%d") - as.Date(as.character(g$HME_DT), format = "%Y%m%d"))
mean_h = mean(as.Date(as.character(h$RECU_FR_DT), format = "%Y%m%d") - as.Date(as.character(h$HME_DT), format = "%Y%m%d"))


mean(69.19327, 419.1863, 288.5308, 55.64664)
sd(c(69.19327, 419.1863, 288.5308, 55.64664))

# 12.4 approach
## 1) 뇌졸중 진단 데이터 정리 
ot_CI02$RECU_FR_DT = as.Date(as.character(ot_CI02$RECU_FR_DT), format = "%Y%m%d")
ot_CI03$RECU_FR_DT = as.Date(as.character(ot_CI03$RECU_FR_DT), format = "%Y%m%d")
ol_CI02$RECU_FR_DT = as.Date(as.character(ol_CI02$RECU_FR_DT), format = "%Y%m%d")
ol_CI03$RECU_FR_DT = as.Date(as.character(ol_CI03$RECU_FR_DT), format = "%Y%m%d")

ot_CI02 = ot_CI02 %>% arrange(RECU_FR_DT, PERSON_ID)
ot_CI03 = ot_CI02 %>% arrange(RECU_FR_DT, PERSON_ID)
ot_ol_CI02 = arrange(ol_CI02, RECU_FR_DT, PERSON_ID) %>% distinct(PERSON_ID, .keep_all = T) # 2,589 x 3, length(unique(ol_CI02$PERSON_ID))와 같음. 
ot_ol_CI03 = arrange(ol_CI03, RECU_FR_DT, PERSON_ID) %>% distinct(PERSON_ID, .keep_all = T) # 2,481 x 3, length(unique(ol_CI03$PERSON_ID))와 같음. 


## 2) 검진 데이터 정리 
gj_2002$HME_DT = as.Date(as.character(gj_2002$HME_DT), format = "%Y%m%d")
gj_2003$HME_DT = as.Date(as.character(gj_2003$HME_DT), format = "%Y%m%d")

gj_2002 = gj_2002 %>% arrange(HME_DT, PERSON_ID)
gj_2003 = gj_2003 %>% arrange(HME_DT, PERSON_ID)

# 통합 데이터, gj_CI 생성 
gj_CI = rbind(gj_2002, gj_2003) %>% 
  merge(rbind(ot_CI02, ot_CI03, ot_ol_CI02, ot_ol_CI03), key = 'PERSON_ID') %>% 
  distinct(PERSON_ID, .keep_all = T) %>%
  arrange(HME_DT, RECU_FR_DT)

sum(duplicated(gj_CI$PERSON_ID)) 
# 다행히 검진 + CI dataset에는 중복값이 없다. 
# 12. 5 아니, 중복값이 있는데 distinct()로 걸러진거다. 

gj_CI2 = rbind(gj_2002, gj_2003) %>% 
  merge(rbind(ot_CI02, ot_CI03, ot_ol_CI02, ot_ol_CI03), key = 'PERSON_ID') %>% 
  arrange(HME_DT, RECU_FR_DT)

str(gj_CI); str(gj_CI2)
sum(duplicated(gj_CI$PERSON_ID)) 
sum(duplicated(gj_CI2$PERSON_ID)) 


# 3) 새로운 문제 : gj_CI filtering하기. 

"gj_CI의 문제 : 검진일이 진단일보다 빠른 obs가 존재한다. 
아마 이전에 검진했던 결과가 나온 후 다시 검진받은 듯.
파편화된 dataset이라 생기는 문제인 것 같다. "

# 검진일이 진단일보다 빠른 obs만 뽑아내기
result = rep(NA, nrow(gj_CI)) # dummy space 

# for + if sentence
for (i in 1:nrow(gj_CI)){
  
  "검진일-진단일이 음수면 result에 index를 저장하기."
  
  if (gj_CI[i, ]$HME_DT - gj_CI[i, ]$RECU_FR_DT < 0) {
    result[i] = i
  }
}

result1 <- na.omit(result) # 공간확보를 위해 선언했던 NA 제거 

gj_CI[result1, ] %>% arrange(RECU_FR_DT) # result1을 index로 gj_CI의 data filtering 


# 4) 평균과 표준편차 계산하기 
mean(abs(gj_CI[result1, ]$HME_DT - gj_CI[result1, ]$RECU_FR_DT)) # 평균 : 134.0417
sd(abs(gj_CI[result1, ]$HME_DT - gj_CI[result1, ]$RECU_FR_DT)) # 표준편차 : 121.044

```
