# SQL

### DDL
스키마, 도메인, 테이블, 뷰, 인덱스를 정의하거나 변경 및 제거하는데 사용

정의된 내용은 DDL 컴파일러를 거쳐 meta-data가 되며 System Catalog에 저장된다.

##### CREAT
스키마, 도메인, 테이블, 뷰, 인덱스의 정의에 사용

##### ALTER
기존 테이블에 대해 새로운 열의 첨가, Default 값 변경, 열 삭제 등에 사용

##### DROP
스키마, 도메인, 테이블, 뷰, 인덱스 제거시 사용 (전체 삭제)

### DML
##### SELECT
```
DISTINCT
  중복 제거
COUNT
  개수
ORDER BY
  정렬 (ASC - 오름차순, DESC - 내림차순)
GROUP BY
  그룹
```
##### INSERT
##### UPDATE
##### DELETE


### DCL





```
CASCADE
  삭제할 요소가 참조 중이더라도 삭제
RESTRICTED
  삭제할 요소가 참조 중이면 삭제되지 않음



```
