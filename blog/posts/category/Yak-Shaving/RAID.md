# RAID(Redundant Array of Independent Disk)

## 1. RAID란?
물리적으로 독립된(개별적인) 여러개의 디스크를 묶어 한개의 논리적인 디스크로 만드는 기술이다.  
'여러개' 라는 표현에서 알 수 있듯이, RAID구성은 최소한  2개의 물리적인 디스크가 있어야 가능하다.

## 2. RAID의 구현 방법
RAID를 구현하는 방법에는 하드웨어적인 방법과 소프트웨어적인 방법이 있다.  
하드웨어적인 방법은 운영체제가 한개의 디스크로 인식하게 한다.  
소프트웨어적인 방법은 운영체제에서 사용자가 한개의 디스크로 인식하게끔 처리한다.  

## 3. RAID의 종류
RAID를 구성하는 방법에는 RAID 0 ~ RAID 6 등 여러가지가 존재한다.  
이러한 구성 방법을 '레벨'이라고 일컫는다.