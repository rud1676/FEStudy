# 구글 검색엔진

> author: @rud1676

## 목차

- 색인을 생성하는 파일 형식 알기
- url구조
- 사이트맵
- 크롤러 관리

## 색인을 생성할 수 있는 파일 목록

```
.pdf .ps .csv (구글어스) .kml .kmz

.gpx GPS정보
.hwp
html
xls
ppt

OpenOffice 제품들
odp
ods
odt

rtf
svg
text

일반적인 프로그래밍 언어로 된 소스코드 - cpp c java py pl등
xml

이미지, 동영상 형식도 색인 생성가능

그래서 나중에 파일 형식으로 검색 을 구글 검색에서 진행할 때 나올 수 있다.

ex: filetype:rtf galway
라고 검색하면 .rtf로 끝나는 rtf파일과 url을 검색한다!
```

## ULR구조 권장사항

**RFC3986**에 정의된 URL을 지원합니다! 또한 특수문자 중 표준에 따라 예약됨으로 정의된 문자는 퍼센트 인코딩이 되어야한다.

- URL에 간단하면서 구체적인 단어를 사용
- 현지화된 단어
- UTF-8 인코딩을 사용.

현지화에 대한 URL사용가이드는 아래에 나와있다.

### 다국어 다지역 사이트 관리

> 다국어 웹사이트: 두 개 이상의 언어로 콘텐츠를 제공하는 웹사이트이다.
> 다지역: 명시적으로 여러 국가의 사용자를 대상으로 하는 사이트

url구조 옵션엔 하위 도메인 없이 국가별 도메인으로 갈 수 있다.

**국가별 도메인으로 설정**

예시

> example.kr

이렇게 url을 두면 장점은..

- 명확한 지역 타겟팅,
- 서버 위치가 중요하지 않다(지금 kr이 한국을 타겟하지만 서버는 해외에 있어도됨 - ccTLD)
- 간편한 사이트 분리가 된다.(example.jp 도 가능)

그러나 단점은..

- 국가마다 도메인 을 두기 때문에 비용 발생
- 다수의 도메인이기 때문에 추가 인프라가 필요함.
- 요구사항이 엄격할 수 도 있어( 도메인 등록자가 해당 국가의 시민이거나 법인이어야 할 수 있습니다. - 법률이 빡센곳)
- 단일 국가만 타겟팅을 하기에..(글로벌한 서비스라면 충돌될 수 있음)

> ccTLD는 "Country Code Top-Level Domain"의 약자로, 각국의 국가 코드에 기반한 최상위 도메인을 의미합니다

**gTLD를 포함한 하위 도메인**

> gTLD : "Generic Top-Level Domain"의 약자로, 일반적인 최상위 도메인을 의미합니다. 이러한 도메인은 특정 국가에 속하지 않으며, 일반적으로 특정 분야나 목적에 따라 사용됩니다.

예시

- .com: 가장 널리 알려진 gTLD로, 주로 상업적인 웹사이트에 사용됩니다.
- .org: 비영리 조직이 주로 사용하는 도메인입니다.
- .net: 원래 네트워크 기술 관련 조직을 위해 설계되었지만, 현재는 다양한 목적으로 널리 사용됩니다.
- .info: 정보 제공 사이트에 사용됩니다.
- .blog: 블로그나 개인 출판을 목적으로 하는 사이트에 사용됩니다.

ex) de.example.com

장점으로는..

- 다양한 서버 위치 허용(하위에 두면됨)
- 간편한 사이트 분리
- 간편한 설정이 됨.(걍 도메인 설정하는 곳에서 하위 도메인에 이름을 추가하고 아이피 추가하면 됨.)

그러나 단점이.. 사용자가 URL만으로 지역 타겟팅을 인식하지 못할 수 있음(예: 'de'가 언어인지 국가인지 확실하지 않음)

**gTLD를 포함한 하위 디렉토리**

ex) example.com/de/

위에랑 비슷하지만 이것은 단일 서버이다. 그래서 사이트의 분리가 어렵다.

### 그렇다면 구글에서는 지역을 어떻게 판단할까?

ccTLD는 분명 특정 국가랑 연결되어있어서 상관 없다.

gTLD일땐...

- ip주소를 통한 서버 위치,
- 기타신호(현지 번호, 현지 언어 등으로 추적)

추론해 파악한다!

### 그외 URL의 권장사항

- 하이픈 사용(\_가 아닌 -로 사용해야됨!)
  > https://www.example.com/summer-clothing/filter?color-profile=dark-grey

### URL에 관한 일반적인 문제

여러개의 매개변수를 포함한 URL은 불필요한 URL이 많이 생길 수 있어 크롤러에 문제를 일으킨다. (예를 들면 URL의 매개변수 정보가 다르지만, 동일한 결과를 줄 수 있음 - 데이터 담는데 과부하 원인이 됨) 그래서 Googlebot이 모든 콘텐츠에 대한 색인을 완전히 생성 못할 확률이 매우 크다.

구체적인 예시는..

'특가'의 호텔 숙박 시설

> https://www.example.com/hotel-search-results.jsp?Ne=292&N=461

'특가'의 해변 호텔 수박 시설

'특가'의 해별 호텔 중 헬스장이 이쓴 호텔 숙박 시설

> https://www.example.com/hotel-search-results.jsp?Ne=292&N=461+4294967240+4294967270

'정렬매개변수' 동일한 정보를 매개변수로 정렬할 때. URL의 수 증가

> https://www.example.com/results?search_type=search_videos&search_query=tpb&search_sort=relevance&search_category=25

'캘린더' 매개변수. URL에 무한대로 생성되는 매개변수가 나올 수 있음.

이러한 문제는 아래와 같은 방법들로 해결한다.

- URL구조 단순화
- robots.txt로 문제가 되는 URL액세스 차단

## 서비스내에 링크 권장사항

- 일반적인 href가 있는 a태그를 사용하자.
- 엥커택스트를 잘 배치하자

```html
<a href="https://example.com/ghost-peppers">고스트 페퍼</a>
<a
  href="https://example.com/ghost-pepper-recipe"
  title="고스트 페퍼 피클 만드는 법"
></a>

// a요소가 비어있다면 위에 처럼 title붙여주기!

<a href="/add-to-cart.html"
  ><img src="enchiladas-in-shopping-cart.jpg" alt="장바구니에 엔칠라다 추가"
/></a>

// 이미지 링크라면 꼭 alt값!
```

- 일반적인 워딩 쓰지말고 구체적인 워딩 쓰기

ex) 도움말(x) 치즈를만드는방법(o)

- 지나치게 길게 두지 말기, 간결하게!

## sitemap 제작

사이트맵은 사이트에 있는 페이지, 동영상, 기타 파일과 각 관계에 관한 정보를 제공하는 파일!
검색엔진은 이 파일을 읽고 사이트를 더 효율적으로 크롤링함!

그러나 무조건 필요한건 아님! 구글에서 제시한 아래와 같은 상황일때 만들라고 했음!

- 사이트 크기가 크다!
- 외부 링크가 많지 않은 새로운 사이트
- 동영상,이미지가 많거나 구글 뉴스에 표시되는 사이트.

### xml로 사이트맵 제작

실제로 아래처럼 작성된다 - jekyll블로그

```xml

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9
                            http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">
    <url>
        <loc>https://www.example.com/</loc>
        <lastmod>2024-02-01</lastmod>
        <changefreq>daily</changefreq>
        <priority>1.0</priority>
    </url>
    <url>
        <loc>https://www.example.com/about</loc>
        <lastmod>2024-01-15</lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>https://www.example.com/products</loc>
        <lastmod>2024-01-20</lastmod>
        <changefreq>weekly</changefreq>
        <priority>0.9</priority>
    </url>
    <url>
        <loc>https://www.example.com/products/widget1</loc>
        <lastmod>2024-01-25</lastmod>
        <changefreq>weekly</changefreq>
        <priority>0.6</priority>
    </url>
    <!-- 더 많은 URL 추가 가능 -->
</urlset>
```

- loc: 페이지의 정확한 URL
- lastmod: 페이지의 마지막 수정 날짜입니다. 검색 엔진이 페이지의 콘텐츠가 얼마나 자주 업데이트 되는지 파악하는 데 도움이 됨.
- changefreq: 페이지의 변경 빈도를 나타냅니다.
- priority: 사이트 내에서 해당 페이지의 상대적 중요도를 0.0에서 1.0 사이의 값으로 나타낸다.

### RSS, mRSS, Atom 1.0

> GPT: RSS와 Atom을 활용한 사이트 업데이트 알림
> RSS나 Atom 피드를 사용하여 웹사이트의 최신 콘텐츠 업데이트를 알리는 것은, 검색 엔진이나 사용자가 사이트의 최신 정보를 쉽게 추적할 수 있게 해줍니다. 이 방법은 특히 블로그, 뉴스 사이트, 미디어 공유 사이트 등 **자주 콘텐츠가 업데이트되는 사이트**에 유용합니다.

### 텍스트 사이트맵

단순히 URL만 적으면 된다.

```txt
https://www.example.com/file1.html
https://www.example.com/file2.html
```

### 어떻게 사이트맵을 제작할까?

1. CMS에서 사이트맵 생성하도록 하기!

- Wix, WordPress 등 서비스를 사용하면 된다! 아마 자도응로 사이트맵을 생성햇을 것이다.

2. 수동으로 사이트맵 생성

- 위와 같은 형식으로 계속 작업을 해야한다. 그러나 장기적으로 지속적으로 관리하기가 힘들다.

3. 사이트맵을 생성해주는 도구를 사용하기 - SML-sitemaps.com과 같은 서비스 이용

4. google search console 을 사용해 구글에게 알려주기

- 사이트맵 보고서 기능 활용
- api사용해 자동화 방식으로 사이트맵 제출하기
- robots.txt에 사이트맵과 연결되는 경로를 지정하기

> 알아두기! 사이트맵의 속성도 있음! 잘알아둬서 반영하거나 확인하기
> image,news, video, 사이트맵 확장프로그램

## 크롤링

### Google에 URL 재크롤링 요청.

페이지가 추가되거나 변경되는 경우 색인을 다시 생성하도록 요청할 수 있음. URL검사도구와 색인상태 보고서로 진행상황 모니터링 하면됨!

Search COnsole을 통해 할 수 있다!

### 크롤링 요청횟수 조절하기

Google이 특정사이트 방문할 때 알아서 적합한 **크롤링 속도를** 결정하는 정교한 알고리즘을 사용한다.

그런데 경우에 따라서 그럼에도 심각한 부하가 발생하거나 서비스가 중단된 기간에 비용이 발생할 수 있다. 이 문제를 고려할 려면 GoogleBot이 생성하는 요청 횟수를 줄이도록 선택해야됨!

1. 서비스가 잠시 중단?됫을 때

200의 상태코드값 대신 500,503 429를 반환해라! 그러면 알아서 속도를 낮춘다.(대신 이 옵션은 길게 두면 안된다! 너무나 긴다면 구글이 URL을 삭제할 수 있음!)

2. 위에서 언급한 비 효율적 구조를 가지고 있는지 확인하기

### 구글봇과 다른 구글봇 확인하기

악의적 사용자가 GoogleBot을 가정해서 접근할 수 잇는데 그것을 구분하는 가이드.

구글 크롤러는 기본적으로 세가지 카테고리입니다.
|유형|설명|
|--|--|
|GoogleBot|기본 크롤러 robots.txtb에서 제시된 규칙을 준수|
|예외상황 크롤러|AdsBot등 특정 기능을 수행하는 크롤러|
|사용자 트리거 가져오기|사이트 인증 도구 등으로 사용자의 요청에 따라 작동하는 크롤러|

위에 제시된 3가지는 구글 문서에 가면 Ip범위를 확인할 수 있다.

**수동으로 확인하기**

명령줄 도구로 직접 확인하기!

구글문서에서 제시한 방법.

1. host 명령어를 사용해 로그의 액세스 IP 주소에 역방향 DNS 조회를 실행합니다.
2. 도메인 이름이 googlebot.com, google.com, 또는 googleusercontent.com인지 확인합니다.
3. 검색된 도메인 이름에서 host 명령어를 사용해 1단계에서 검색된 도메인 이름에 순방향 DNS 조회를 실행합니다.
4. 로그의 원래 액세스 IP 주소와 동일한지 확인합니다.

```zsh
$host 66.249.66.1
1.66.249.66.in-addr.arpa domain name pointer crawl-66-249-66-1.googlebot.com.

$host crawl-66-249-66-1.googlebot.com
crawl-66-249-66-1.googlebot.com has address 66.249.66.1
```

**자동 솔루션**

크롤러의 IP주소를 대조해 봇임을 식별하면 됨!

### 대규모사이트의 잦은 업데이트를 위한 가이드

> 웹 공간은 거의 무한하므로 사용할 수 있는 모든 URL을 탐색하고 색인을 생성하는 Google의 능력을 넘어섭니다. 따라서 Googlebot이 단일 사이트를 크롤링하는 데 쓸 수 있는 시간에는 제한이 있습니다. Google에서 사이트를 크롤링하는 데 쓰는 시간과 리소스의 양은 일반적으로 사이트의 크롤링 예산이라고 합니다. 사이트에서 크롤링된 모든 항목에 반드시 색인이 생성되는 것은 아닙니다. 각 페이지를 평가, 통합하여 크롤링 후 색인을 생성할지 판단해야 합니다.

- 크롤링 용량 한도: 크롤링 상태에 비례(쉽게 생각해 사이트의 속도가 느리면 안됨), 구글의 자체의 크롤링 한계고려.
- 크롤링이 필요한가?(크롤링 수요): 아래는 수요를 판단하는데 주요한 요소
  - 인식된 인벤토리: 안내가 없다면 모든 URL전부를 크롤링하려고한다.(시간낭비 요소) 이것을 제어하자
  - 인기도: 인기도가 높은URL은 더 자주 크롤링 됨.
  - 비활성: 변경사항을 충분히 포착하도록 문서를 다시 크롤링 하려고한다!

따라서! 인벤토리 관리가 어떻게보면 우리가 해야될 역활이다.

아래는 문서에서 제시한 방법들이다.

1. 중복되는 콘텐츠 통합
2. robots.txt를 사용해 URL크롤링 차단하기
3. 삭제된 페이지는 404를 무조건 반환하자! - 알고있는 URL의 기억을 지우라고 명령하는 것과 똑같
4. 사이트맵을 최신화!
5. 로딩 빠르게
6. 크롤링 모니터링해서 문제를 계속 체크하기!

**크롤링 모니터링**

1. 사이트를 개선했다고 해서 반드시 크롤링 예산이 증가하는 것이 아니다. **크롤링 통계 보고서**를 통해 정확하게 진단해보자.
2. 크롤링 되지 않아야 하는 사이트가 크롤링이 되는지 확인하기!
3. 페이지나 서비스가 추가됬는데 크롤링 되지 않는다면.. Google이 해당 페이지를 모르거나 콘텐츠가 차단되거나 크롤링 예산 이 없는것이다.
   1. 구글에 알려주고
   2. robots.txt를 검토하고
   3. 크롤링 예산이 없는지 확인 - 크롤링 통계 보고서로 확인
4. 과도한 크롤링 처리(긴급한사항)
   1. 일시적으로 503,429 응답으로 변형
   2. 상태가 해결된다면 다시 200으로 빨리돌려!(유지되면 구글봇이 색인에서 삭제해)

## HTTP상태코드에 따른 크롤러 반응

[문제풀어보기](https://developers.google.com/search/docs/crawling-indexing/large-site-managing-crawl-budget?hl=ko#availability_issues)

- 200 (success)
- 201 (created)
- 202 (accepted)

Google이 콘텐츠를 색인 생성 될 수 있음!

- 204 (no content)
  Googlebot이 컨텐츠가 없다고 알림. search consoledl soft 404오류를 표시함.

- 3xx(리다이렉션)
  마지막에 도착한 콘텐츠가 색인 생성의 대상으로 고려됨

- 4xx
  색인에서 삭제되는 URL

- 429(too many requeest) : 서버과부하라는 뜻이므로 색인에 영향 x
- 5xx : 서버오류이므로 크롤링 속도를 낮추라고 요청한다. 그러나 이런상태가 30일 이상 지속되면 URL은 삭제됩니다!
