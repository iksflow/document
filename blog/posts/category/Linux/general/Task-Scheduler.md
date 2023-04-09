# 작업 일정 관리
at, cron


## cron
at은 1회성 예약 작업이지만 cron은 설정한 주기에 맞게 반복적으로 작업 예약이 가능하다.
크론 서비스는 작업 실행을 담당하는 cron 데몬(crond)과 예약 작업정보가 있는 설정파일들로 구성되어 실행된다.

동작방식

/etc/crontab: 시스템 전역 사용 목적
/etc/crond: 시스템 유틸 등 개별 패키지에 의한 작업 예약
/var/spool/cron: 사용자 정의 작업

/etc/cron.hourly
/etc/cron.daily
/etc/cron.weekly
/etc/cron.monthly


## cron 사용 제한하기
Root이외의 사용자가 cron 작업을 잘못 사용해서 시스템 리소스에 영향을 주는 경우가 생기는 상황이 발생할 수도 있다.  
이런 경우를 대비해 cron 작업에 대해 제한하는 기능이 있다.  
/etc/cron.allow 파일은 cron작업을 생성할 수 있는 유저의 목록을 정의한다

## anacron
anacron은 시스템이 중단되더라도 미처리된 예약작업을 실행할 수 있게 해준다.  
마지막으로 실행한 예약 작업을 기록해놓기때문에 cron으로 예약된 작업을 실행하지 못했을 경우에도 미처리 건에 대해 다시 작업을 수행할 수 있다.  

