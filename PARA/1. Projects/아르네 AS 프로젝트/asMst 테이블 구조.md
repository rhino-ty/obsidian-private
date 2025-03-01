매장 설비나 시설에 대한 A/S 요청을 접수하고, 처리 과정을 관리하는 웹 애플리케이션.

`asMst` 테이블은 A/S 서비스 관리의 핵심 테이블입니다. 이 테이블은 A/S 요청 접수부터 완료까지의 모든 정보를 저장하고 있습니다.

## asMst 테이블 구조 (추정)

아래는 코드에서 추출한 `asMst` 테이블 구조입니다.

|필드명|데이터 타입|설명|
|---|---|---|
|asNo|VARCHAR|A/S 접수번호 (기본키)|
|asDate|DATETIME|접수일시|
|shopCD|VARCHAR|매장코드 (외래키: shopM 테이블)|
|shopName|VARCHAR|매장명|
|shopTel|VARCHAR|매장 전화번호|
|shopUser|VARCHAR|매장 담당자|
|asStat|CHAR(1)|상태코드 (1:신청, 2:확정, 3:전화완료, 4:방문완료, 5:입점, 6:재방문, 7:지정점이관, 8:통화완료)|
|asToCall|VARCHAR|접수경로 코드|
|asLine|VARCHAR|접수라인 코드|
|ctUserID|VARCHAR|콜센터 담당자 ID (외래키: exms_user 테이블)|
|centerCkDT|DATETIME|센터 확인 시간|
|centerMemo|TEXT|센터메모|
|mtUserID|VARCHAR|유지보수 담당자 ID (외래키: exms_user 테이블)|
|workuserId|VARCHAR|작업자 ID|
|compCD1|VARCHAR|담당회사 코드|
|compName1|VARCHAR|담당회사명|
|compCD2|VARCHAR|보조 회사 코드|
|compName2|VARCHAR|보조 회사명|
|mtJsDT|DATETIME|유지보수 접수 시간|
|mtTelDT|DATETIME|유지보수 전화 시간|
|dueDate|DATETIME|방문예정일시|
|inShopDT|DATETIME|입점 시간|
|visitDT|DATETIME|방문 시간|
|sTime|TIME|작업 시작 시간|
|eTime|TIME|작업 종료 시간|
|finishDT|DATETIME|완료일시|
|finishYN|CHAR(1)|보고서 작성 여부 (Y/N)|
|asType1|VARCHAR|A/S 유형1 코드 (대분류)|
|asType2|VARCHAR|A/S 유형2 코드 (세부 카테고리/집기시설)|
|asType3|VARCHAR|A/S 유형3 코드 (증상)|
|asType21|VARCHAR|추가 A/S 유형2 첫번째|
|asType31|VARCHAR|추가 A/S 유형3 첫번째|
|procCD1|VARCHAR|처리 코드 첫번째|
|asType22|VARCHAR|추가 A/S 유형2 두번째|
|asType32|VARCHAR|추가 A/S 유형3 두번째|
|procCD2|VARCHAR|처리 코드 두번째|
|asType23|VARCHAR|추가 A/S 유형2 세번째|
|asType33|VARCHAR|추가 A/S 유형3 세번째|
|procCD3|VARCHAR|처리 코드 세번째|
|asType24|VARCHAR|추가 A/S 유형2 네번째|
|asType34|VARCHAR|추가 A/S 유형3 네번째|
|procCD4|VARCHAR|처리 코드 네번째|
|claimList|TEXT|접수내역|
|procList|TEXT|처리내역|
|procExpen|DECIMAL|처리 비용|
|itemCD|VARCHAR|주 아이템 코드|
|itemCD1|VARCHAR|아이템 코드1|
|itemCD2|VARCHAR|아이템 코드2|
|itemCD3|VARCHAR|아이템 코드3|
|itemCD4|VARCHAR|아이템 코드4|
|partsYN|CHAR(1)|부품 사용 여부|
|partsTotalAmt|DECIMAL|부품 총액|
|chulAmt|DECIMAL|출장 비용|
|shopAMT|DECIMAL|매장 부담 금액|
|BonbuAmt|DECIMAL|본부 부담 금액|
|groupAsYN|CHAR(1)|그룹 A/S 여부|
|groupAsNo|VARCHAR|그룹 A/S 번호|
|dongh1|VARCHAR|동행자1 ID|
|dongh1DT|DATETIME|동행자1 시간|
|dongh2|VARCHAR|동행자2 ID|
|dongh2DT|DATETIME|동행자2 시간|
|dongh3|VARCHAR|동행자3 ID|
|dongh3DT|DATETIME|동행자3 시간|
|jegoUnitYN|CHAR(1)|재고 단위 확인 여부|
|jegoUnitYN1|CHAR(1)|재고 단위1 확인 여부|
|jegoUnitYN2|CHAR(1)|재고 단위2 확인 여부|
|jegoUnitYN3|CHAR(1)|재고 단위3 확인 여부|
|jegoUnitYN4|CHAR(1)|재고 단위4 확인 여부|
|asPoint|INT|A/S 포인트|
|asPointList|TEXT|포인트 상세 정보|
|asPointTotal|INT|포인트 총합|
|imageSaveCnt|INT|저장된 이미지 수|
|reserved1|VARCHAR|예약 필드1|
|reserved2|VARCHAR|예약 필드2|
|insertID|VARCHAR|등록자 ID|
|insertIP|VARCHAR|등록 IP|
|insertDT|DATETIME|등록일시|
|updateID|VARCHAR|수정자 ID|
|updateIP|VARCHAR|수정 IP|
|updateDT|DATETIME|수정일시|
|REFL_YN|CHAR(1)|반영 여부|
|REFL_DATE|DATETIME|반영 일자|

이 확장된 테이블 구조는 A/S 서비스의 전체 생명주기를 포괄적으로 관리하기 위한 다양한 필드를 포함하고 있습니다. 매장 정보부터 담당자, 작업 내용, 비용, 동행자, 포인트까지 A/S 서비스와 관련된 모든 정보를 추적할 수 있도록 설계되어 있습니다.
## 관련 테이블 (추정)

`asMst` 테이블과 관련된 주요 테이블들

1. **`shopM`** - 매장 정보 테이블
    
    - `shopCD` (기본키)
    - `shopName` (매장명)
    - `shopTel` (매장 전화번호)
    - 기타 매장 정보
2. **`exms_user`** - 사용자 정보 테이블
    
    - `ID` (기본키)
    - `name` (사용자 이름)
    - `class` (사용자 권한: '`mt`', '`mtt`' 등)
    - `compCD` (소속 회사 코드)
    - 기타 사용자 정보
3. **`exms_commonCode`** - 코드 정보 테이블
    
    - `codeType` (코드 유형: '`asStat`', '`asType1`', '`asType2`', '`asType3`' 등)
    - `codeValue` (코드 값)
    - `codeName` (코드 이름)

## ERD (Entity Relationship Diagram)


```
+-------------------+       +-----------------+       +-------------------+
|     shopM         |       |     asMst       |       |    exms_user      |
+-------------------+       +-----------------+       +-------------------+
| PK: shopCD        |<----->| FK: shopCD      |       | PK: ID            |
|    shopName       |       | PK: asNo        |<----->| FK: mtUserID      |
|    shopTel        |       |    asDate       |       |    name           |
|    ...            |       |    shopName     |       |    class          |
+-------------------+       |    asToCall     |       |    compCD         |
                            |    asLine       |       |    ...            |
      +---------------+     |    ctUserID     |       +-------------------+
      |exms_commonCode|     |    compCD1      |
      +---------------+     |    compName1    |
      | codeType      |---->|    mtUserID     |
      | codeValue     |     |    asType1      |
      | codeName      |     |    asType2      |
      +---------------+     |    asType3      |
                            |    claimList    |
                            |    procList     |
                            |    asStat       |
                            |    dueDate      |
                            |    finishDT     |
                            |    finishYN     |
                            |    centerMemo   |
                            +-----------------+
```

## 주요 기능 흐름



1. A/S 접수: 매장에서 A/S 요청이 들어오면 `asMst` 테이블에 새 레코드 생성
2. 담당자 배정: 해당 지역/센터의 담당자(`mtUserID`)에게 A/S 작업 배정
3. 상태 관리: A/S 처리 과정에 따라 상태(asStat) 변경
    - 신청(1) → 확정(2) → 전화완료(3)/방문완료(4)/입점(5)/재방문(6)/지정점이관(7)/통화완료(8)
4. 보고서 처리: 완료 후 보고서 작성 여부(`finishYN`) 표시

이 시스템은 매장 설비나 시설에 문제가 생겼을 때 A/S 요청을 접수하고, 담당자가 방문하거나 전화로 처리하는 과정을 관리하는 서비스 시스템으로 보입니다. 특히 여러 지역 센터(서울, 부산, 대구, 대전, 광주, 강원, 제주 등)를 관리하는 구조로 되어 있습니다.

1. 매장에서 A/S 요청 → 접수(신청)
2. 담당자 배정 → 확정
3. 처리 방식에 따라:
    - 유선처리 → 전화완료
    - 방문처리 → 방문완료
    - 입점처리 → 입점
    - 추가 방문 필요 → 재방문
    - 다른 담당점 처리 필요 → 지정점이관
    - 통화만 완료 → 통화완료
4. 처리 완료 후 보고서 작성 → finishYN = 'Y'

## 센터 코드 정보

시스템에서 사용하는 센터 코드는 다음과 같습니다.

| 센터코드 | 센터명  |
| ---- | ---- |
| A001 | 서울센터 |
| A002 | 부산센터 |
| A003 | 대구센터 |
| A004 | 대전센터 |
| A005 | 광주센터 |
| A006 | 유지보수 |
| A010 | 강원센터 |
| A014 | 제주센터 |

## 주요 상태 코드 (asStat)

A/S 처리 과정의 상태를 나타내는 코드입니다.

1. **신청**: 최초 접수 상태
2. **확정**: 담당자 배정 완료
3. **전화완료**: 유선으로 처리 완료
4. **방문완료**: 현장 방문하여 처리 완료
5. **입점**: 입점 처리 (이전에는 '시설팀이관'으로 사용)
6. **재방문**: 추가 방문 필요
7. **지정점이관**: 다른 담당점으로 이관 (이전에는 '타업체이관'으로 사용)
8. **통화완료**: 유선 통화 완료

## 주요 코드 유형 (codeType)

`exms_commonCode` 테이블에서 관리되는 코드 유형들.

- `asStat`: A/S 처리 상태 코드
- `asType1`: A/S 대분류 (예: 매장시설, 집기 등)
- `asType2`: A/S 집기/시설 분류
- `asType3`: A/S 증상 분류
- `asToCall`: 접수 경로 코드
- `asLine`: 접수 라인 코드
- `bu`: 지역(Business Unit) 코드

