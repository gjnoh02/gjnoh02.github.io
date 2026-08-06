+++
date = '2026-08-06T21:07:00+09:00'
draft = false
title = '8월 6일 블로그'
+++
8월 6일 한 일
1. 프로젝트 회의에서 나온 새로운 기능 구현
(새로운 기능 추가 -> controller, service, entity, dto수정)

2. 이전 내용에서 논리적 오류 있는 부분 개선
(일주일 사이 점수 7일이 안채워지면 이미 된 날짜로만 점수계산)


조회된 일기들의 점수를 계산 -> 총점 계산
        for (Diary diary : weeklyDiaries) {
            Weight weight = weightRepository.findById(Long.valueOf(diary.getWeightId()))
                    .orElseThrow(() -> new IllegalArgumentException("가중치 정보를 찾을 수 없습니다."));

            totalScoreSum += calculateDailyScore100(diary, weight);
            validDaysCount++;
        }

 일기가 하루도 없으면 0.0, 있으면 평균 계산 (소수점 첫째 자리까지) -> 존재하는 날까지로 수정
        double average = (validDaysCount == 0) ? 0.0 : Math.round((totalScoreSum / validDaysCount) * 10.0) / 10.0;

        return StatisticsResponseDto.WeeklyAverageResponse.builder()
                .totalWeightedAverage(average)
                .build();
    
3. 자바에서 Http 헤더 바디정보 추출 해보기




