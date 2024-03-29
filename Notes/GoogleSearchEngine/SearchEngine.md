# 구글 검색엔진

> author: @rud1676

## 구글 Essential

> Google 검색에서 표시되고 좋은 실적을 내도록 하는 핵심요소

- 기술 요구사항 : 검색 결과에 웹 페이지를 표시하기 위해 웹페이지에 요구하는 최소한의 기본 요건. (대부분은 신경 쓰지 않고도 기술 요구사항을 갖추게 됨.)
- 스팸 정책: 사이트 전체에서 순위가 낮아지거나 최악의 경우 완전히 사라지는 결과를 초래하는 수법, 동작
- 주요 권장사항 - 검색엔진 최적화.

### 기술 요구사항 - 상세히

1. 구글봇이 차단되지 않도록 합니다.(다시 말하면 페이지 검색 및 액세스가 가능해야합니다.)

- 페이지가 비공개라면(로그인해야 볼 수 있는 경우) 크롤링 하지 않습니다.
- 구글봇이 페이지를 찾고 액세스할 수 있는지 먼저 확인 URL검사 도구를 사용해 분석받기!

2. 페이지가 200(status) 상태 코드로 게재되는 페이지에만 색인을 생성함. 이것도 URL검사도구에서 확인가능

3. 색인이 가능한 콘텐츠가 있어야됨. - 색인 가능 콘텐츠는 구글 검색에서 지원하는 파일형식, 스팸정책 위반하지 않은 콘텐츠.

### 스팸정책

자기 사이트가 스팸정책을 위반한다고 생각한다면 "검색 품질 사용자 보고서"를 제출해 Google에게 알려주기!

1. **클로킹**

검색 순위를 조작하고 사용자를 구분해 검색엔진과 사람에게 다른 콘텐츠를 표시하는 행위.

만약 사이트가 해킹되서 해커는 아래와 같은 스크립트를 심을 수 있다.

```php
<?php
$userAgent = $_SERVER['HTTP_USER_AGENT'];

if (preg_match('/Googlebot/', $userAgent)) {
    // 검색 엔진 크롤러에게 보여줄 콘텐츠
    echo "여기는 여행지 정보 페이지입니다.";
} else {
    // 일반 사용자에게 보여줄 콘텐츠
    echo "할인된 의약품을 구매할 수 있는 페이지입니다.";
}
?>
```

해킹된 사이트의 예시
![그림](https://web.dev/static/articles/hacked/image/different-errors-might-i-735533d78f615.gif?hl=ko)

또한
**페이월 또는 콘텐츠 제한 메커니즘**(ex 사용자가 구독해야 보이는 컨텐츠 등)이 적용되어 자료에 액세스 정책이 저렇다면, 개발자는 "유연한 샘플링 일반 가이드"를 통해 적용한다면 이건 클로킹으로 간주되지 않음. (이건 클로킹이 아니라 사용자와 검색엔진이 똑같은 자료를 볼 수 있게 해둔 것이기 때문.)

2. **도어웨이**

> 도어웨이 페이지는 검색엔진 순위를 조작하기 위해 최적화된 페이지. 보통 이런페이지에선 가치가 있는 콘텐츠를 제공하지 않고 검색엔진에서 높은 순위를 차지하기 위해 최적화된 페이지임.

구체적인 예시.

2-1. **유사한 웹사이트 운영**

하나의 음식점이 내용이 거의 똑같은 웹 페이지에 대해 각 도시마다 별도의 웹사이트를 만들어 운영하는 경우.(도시별로 다른 URL을 사용하지만 해당 지역에 특화된 몇 가지 키워드를 추가해 각기 다른 도시 이름으로 검색했을 때 상위 에 노출되도록 함.)

2-2. **지역 타켓팅 도메인 이름이나 페이지**

"도시-부동산.com"의 패턴의 여러 도메인을 구매하고 각지역에 대해 검색어 최적화를 진행한다. 그러면 검색어를 입력한 사용자는 이런 페이지들이 검색 결과에 나타나고, 결국 모든 페이지("도시-부동산.com")의 부동산 목록 페이지로 유도된다! => 링크모아놓은 사이트로 유도되는 것과 비슷한 느낌인듯.

2-3. **검색결과마다 비슷한 페이지 생성**

하나의 음식점에서 맛잇는 음식, 저렴한 음식, 핵존맛 음식 등 유사한 키워드에 대해서 동일한 여러 페이지를 만든다. 그러나 사용자는 실제로 클릭하면 일반적인 음식에 관한 목록페이지로 리다이렉션된다.

3.  **해킹된 콘텐츠**

- 코드삽입(악성 자바스크립트 주입 또는 iframe불러오기)
- 새로운 페이지가 삽입됨
- 콘텐츠가 부적절하게 삽입됨
- 유해한 페이지로 리다이렉션

4. **숨겨진 텍스트 및 링크**

검색엔진 조작하고 방문한 사람에게는 안드러난 행위

예시

- 흰생 배경에 흰색 텍스트
- 이미지 뒤에 텍스트 숨김
- CSS이용해 포지션을 화면 바깥으로

의도적인 동작들은 스팸정책으로 간주

그러나 아래의 케이스는 위반되지 않는다. - 요즘은 동적인 방식으로 콘텐츠 숨기기를 활용하기 때문.

- 추가 콘텐츠 숨기기와 표시 간에 전환하는 탭 또는 아코디언 콘텐츠(호버하면 나오는 메뉴 등)
- 이미지 텍스트 단락을 순환하는 슬라이드쇼(배너)
- 상호작용할 때 추가 콘텐츠를 표시하는 도움말(나무위키 각주)
- 스크린 리더에만 액세스 할 수 있는 컨텐츠

5. **유인 키워드 반복**

말 그대로 순위를 조작하기 위해 넣는 키워드

- 가치가 없는 전화번호
- 도시와 지역을 나열하는 텍스트 블록
- 부자연스럽게 느껴지는 텍스트

```
무제한 앱 스토어 크레딧. 앱 스토어 크레딧을 무료로 제공한다고 주장하는 사이트는 많지만 모두 허위 사이트이며 무제한 앱 스토어 크레딧을 찾는 사용자를 항상 악용합니다. 이 웹사이트에서 무제한 앱 스토어 크레딧을 바로 받을 수 있습니다. 지금 바로 무제한 앱 스토어 크레딧 페이지를 방문하여 크레딧을 받으세요.
```

![alt text](../IMG/serach.png)

6. **링크 스팸**

검색엔진은 웹페이지의 관련성 판단할 때 링크를 중요한 요소로 사용한다.

이때 이 링크가 순위를 조작하기 위한 링크라면 스팸정책 위반.

예시

- 순위 조작 목적으로 링크 구매, 판매하는 것 => 제품에 대한 그을 쓰고 링크를 무조건 포함시키는 대가.
- 과도환 링크 교환 상호링크만을 목적으로 하는 파트너 페이지.
- 사이트에 대한 링크를 자동으로 생성하는 프로그램 사용
- 너무많은 링크가 포함된 기사
- 품질이 낮은 사이트 링크

7. **머신 생성 트래픽**

자통 트래픽이라 하는데 "자동화된 검색어를 Google에 전송"하는 행위와 Google검색에 대한 자동화된 액세스 =>한마디로 봇!

8. **혼돈을 주는 기능**

- 앱 스토어 크레딧을 제고한다고 주장하지만 실제로 크래딧을 제공하지 않는 가짜 생성기가 포함된 사이트.
- 사기성 광고로 유동하는 사이트

9. **스크랩한 콘텐츠**

- 법적 삭제 요청이 접수되는 경우에는 사이트 순위가 엄청 내려간다.
- 스크랩한 콘텐츠의 악성행위는 자동화 기법으로 수집(조금 수정해서 복사한 사이트가 예시)

10. **부적절한 리다이렉션**

- 상당히 다른 콘텐츠로 리디렉션
- 데스크톱 사용자는 정상, 모바일 사용자는 완전 다른 스팸성 도메인.

아래는 합법적인 리디렉션

- 새 주소로 사이트 이전
- 여러 페이지를 하나로 통합한 과정
- 로그인 하면 내부 페이지로 리디렉션

11. **자동 생성 스팸 콘텐츠**

자동으로 생성된 콘텐츠를 사용자에게 도움이 되는 것이 아닌 검색 순위를 조작하는 것을 주목적으로 생성된 컨텐츠

- 스크랩하여 생성된 텍스트
- 의미없는 텍스트
- 품질이 저하된 컨텐츠
- 자동화 된 도구로 번역이 됫지만 게시전 사람의 검토를 선별하지 않은 텍스트

12. **법적인 제제를 받은 콘텐츠**

- 법적으로 삭제된 요청이 다수가 접수된 사이트
- 개인적보 삭제가 요청이 다수 접소된 사이트
- 검색 콘텐츠 정책을 우회하려는 행위(주요 뉴스 ,디스커버) 이런 것들에게 사용자의 자격을 삭제, 제한하면 적절한 조취가 취해질 수 있음
- 사기 사이트

## 검색엔진 기초 가이드

용어 설명

- 색인: 구글에서 알고있는 모든 웹페이지를 색인에 저장합니다. 즉, 색인은 구글에게 우리 사이트의 위치와 콘텐츠를 등록시키는 저장공간 정도로 생각하면 된다.
- 크롤링: 업데이트, 신규 사이트를 찾는 프로세스
- 크롤러: 크롤링하는 소프트웨어
- 구글봇: 구글 크롤러의 일반적인 이름

**내 사이트가 구글에 표시되는지 확인**

site:를 붙여 검색으로 확인합니다. 그 사이트에 대한 정보를 색인에서 가져옵니다.

**GoogleSearchConsole**

콘텐츠를 구글에 제출하고 검색 실적으로 모니터링하는데 도움이 되는 도구.

내 사이트에서 중요한 문제를 발견하면 searchConsole이 알림을 내게 보내도록 설정할 수 있다.

아래의 질문도 체크해보자

- 내 사이트가 Google에 표시되나요?
- 사용자에게 고품질 콘텐츠를 게재하나요?
- 소유 중인 지역 비즈니스가 Google에 표시되나요?
- 모든 기기에서 내 콘텐츠에 쉽고 빠르게 액세스할 수 있나요?
- 내 웹사이트는 안전한가요?

### 구글이 내 콘텐츠를 발견하게 해주기

- 사이트맵을 제출합니다!
- 다른 페이지에 있는 링크를 통해 페이지를 찾습니다. 사이트를 홍보해서 사용자가 내 사이트를 발견하도록 유도해라!

### 구글에게 크롤링 원하지 않는 사이트를 알리기

robots.txt파일에 명시해야됨!

```
# brandonsbaseballcards.com/robots.txt
# Tell Google not to crawl any URLs in the shopping cart or images in the icons folder,
# because they won't be useful in Google Search results.
User-agent: googlebot
Disallow: /checkout/
Disallow: /icons/
```

Google에 사이트의 정보를 보여주는걸 원치 않지만, 링크를 알게된 사용자가 페이지를 액세스 하는 것에 상관없다면 noindex태그 사용하기!

### title을 고유하고 정확한 페이지 제목으로 만들어라!

페이지가 검색결과 페이지에 표시될 때 title요소의 내용이 검색 결과의 제목 링크로 표시될 수 있다.

### meta에 description을 사용해라!!

```html
<html>
  <head>
    <title>
      Brandon's Baseball Cards - Buy Cards, Baseball News, Card Prices
    </title>
    <meta
      name="description"
      content="Brandon's Baseball Cards provides a large selection of vintage and modern baseball cards for sale. We also offer daily baseball news and events."
    />
  </head>
  <body>
    ...
  </body>
</html>
```

메타 설명태그는 검색 결과의 페이지에서 스니펫으로 사용할 수 있기 때문에 중요하다.(무조건은 아님)

아래는 피해야될 사항입니다.

- 일반적인 설명을 적는 경우(ex: 이건 야구 사이트입니다)
- 키워드로만 채운경우
- 문서 전체의 내용을 복사한 경우

또한 각 페이지마다 고유한 설명을 사용해야합니다.

### 표제 태그를 사용해 중요한 텍스트를 강조해야함.

중요한부분과 덜 중요한 부분을 생각해 표제 태그를 어디에 사용할지 결정해야함.

아래는 피해야될 사항입니다.

- em, strong이 적절한 경우에도 표제 태그 사용
- 무질서하게 섞는 경우
- 도움이 되지 않는 택스트를 배치할 경우

### 구조화된 마크업 추가하기

사이트 페이지에 추가할 수 있는 코드로, 검색엔진에게 콘텐츠를 설명해준다. 그래서 검색엔진은 이 페이지에 어떤 내용이 있는지 더 잘 이해할 수 있다.

![alt text](../IMG/searchi.png)

예를 들면 사진과 같이 온라인 상점에 개별 제품 페이지를 마크업 하면 해당 페이지의 자전거에 가격, 고객 리뷰 등을 포함되어 있다는 것을 이해하게 된다.

그래서 비즈니스와 관련된 여러 가지 항목을 마크업 할 수 있습니다.

- 판매중인 제품
- 업체 위치
- 제품, 비즈니스 관련 동영상
- 영업시간
- 이벤트 목록
- 레시피
- 로고 및 기타

```json
<script type="application/ld+json">
{
  "@context": "http://schema.org/",
  "@type": "Product",
  "name": "Samsung Galaxy S20",
  "image": [
    "https://example.com/photos/1x1/photo.jpg",
    "https://example.com/photos/4x3/photo.jpg",
    "https://example.com/photos/16x9/photo.jpg"
   ],
  "description": "Samsung Galaxy S20는 최신 스마트폰으로, 우수한 카메라와 빠른 성능을 제공합니다.",
  "sku": "0446310786",
  "mpn": "925872",
  "brand": {
    "@type": "Brand",
    "name": "Samsung"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.4",
    "reviewCount": "89"
  },
  "offers": {
    "@type": "Offer",
    "url": "https://example.com/product",
    "priceCurrency": "USD",
    "price": "999.99",
    "priceValidUntil": "2020-11-05",
    "itemCondition": "http://schema.org/NewCondition",
    "availability": "http://schema.org/InStock",
    "seller": {
      "@type": "Organization",
      "name": "Example.com"
    }
  }
}
</script>
```

```json
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@type": "Event",
  "name": "Open Air Concert",
  "startDate": "2024-08-14T19:00",
  "location": {
    "@type": "Place",
    "name": "Central Park",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "5th Ave & 72nd St",
      "addressLocality": "New York",
      "postalCode": "10021",
      "addressRegion": "NY",
      "addressCountry": "US"
    }
  },
  "image": [
     "https://example.com/photos/event1.jpg",
     "https://example.com/photos/event2.jpg"
  ],
  "description": "Join us for an evening of live music at Central Park with top artists from around the world.",
  "offers

```

다양한 가이드는 아래의 주소를 한번 살펴보자

[링크](https://developers.google.com/search/docs/appearance/structured-data/article?hl=ko)

**마크업을 확인하고 테스트할려면...**

[Google 리치 결과 테스트 도구](https://search.google.com/test/rich-results?hl=ko)를 활용하세요!!

아래는 피해야될 사항입니다.

- 잘못된 마크업
- 구현에 확신이 없음에도 사이트의 소스코드를 변경하는 경우

  > 사이트의 소스 코드를 변경하지 않고 구조화된 마크업을 사용해 보려는 경우 콘텐츠 유형 하위 집합을 지원하며 Search Console에 통합되어 있는 도구인 데이터 하이라이터를 사용하세요. 마크업 코드를 복사하여 페이지에 붙여넣을 수 있게 만들려면 마크업 도우미를 사용해 보세요.

- 가짜 리뷰 작성과 관련없는 마크업 작성
- 보이지 않는 마크업 데이터를 추가한 경우

### 사이트 계층구조 구성하기

URL은 아래와 같은 구조를 가진다.

protocol://hostname/path/filename?querystring#fragment

프로토콜은 무조건 https로 하고 탐색 경로 목록을 사용해야합니다.

> 탐색경로란 페이지 상단 또는 하단에 한 행으로 위치한 내부 링크로 방문자는 이를 사용하여 이전 섹션이나 루트 페이지로 빠르게 돌아갈 수 있습니다.

피해야될 사항

- 링크가 서로 복잡하게 얽혀있는 경우
- 너무 콘텐츠를 잘게 나눠 특정 콘텐츠까지 20번이나 이동하고 그래야되는 경우
- 이미지나 애니메이션으로 탐색 기능을 사용하는 경우.
- 스크립트 기반으로한 이벤트 처리를 요구하는 경우

```html

```

1. 탐색용 택스트를 사용해야합니다.(이벤트를 기반으로 처리하면 안됨)

```html
<a href="/contact.html">Contact Us</a>

<!-- 틀린 예시 -->
<a href="javascript:void(0);" onclick="loadPage('contact.html');">Contact Us</a>
```

2. 검색엔진용 사이트맵을 만들어주세요

html로 아래와 같은 페이지를 만들 수 있다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>사이트 맵</title>
  </head>
  <body>
    <h1>사이트 탐색</h1>
    <ul>
      <li><a href="/section1/">섹션 1</a></li>
      <li><a href="/section2/">섹션 2</a></li>
      <li><a href="/section3/">섹션 3</a></li>
      <!-- 더 많은 섹션과 페이지 링크 추가 -->
    </ul>
    <p><a href="/sitemap.xml">XML 사이트맵</a></p>
  </body>
</html>
```

xml로도 만들 수 있음!

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>https://example.com/</loc>
        <lastmod>2024-01-01</lastmod>
    </url>
    <url>
        <loc>https://example.com/section1/</loc>
        <lastmod>2024-01-10</lastmod>
    </url>
    <url>
        <loc>https://example.com/section2/</loc>
        <lastmod>2024-01-20</lastmod>
    </url>
    <!-- 더 많은 URL 추가 -->
</urlset>
```

피해야될 사항

- 탐색 페이지가 최신이 아니고 깨진 링크가 포함될 때
- 주제별 등으로 페이지를 구성하지 않고 단순히 페이지를 나열하기만 한 탐색페이지를 만드는 경우

3. 유용한 404 not found페이지를 만들자!

루트로 리다이렉션하는 링크, 또는 인기있는 콘텐츠로 연결되는 링크를 제공해주자.

4. URL에 적절한단어 사용하기

콘텐츠 및 구조와 관련된 단어가 사이트의 URL에 포함되어 있다면 방문자에게 더욱 친숙한 느낌을준다.

피하기

- page.html등 일반적인 이름의 페이지 선택한 경우
- 불필요한 매개변수 , session id등 긴 url을 사용하는 경우
- 과도한 키워드를 사용한 경우

### 소비자의 니즈를 파악

Google Ads에서 키워드 플래너, Google Search Console에서 실적 보고서를 분석해 어떤 키워드가 좋을지 생각할 거리를 제공해줍니다.

쉽고, 주제를 명확하고,신선하고 독창적인 콘텐츠를 제공해주기

### 링클르 현명하게 사용해주세요

> . 어떤 경우든 간에 앵커 텍스트가 좋을수록 사용자가 탐색하기 쉬우며 링크를 통해 연결되는 페이지를 Google이 더 잘 이해할 수 있습니다.

![이미지](https://lh3.googleusercontent.com/kJ9Hr6MZ3i8pG7U0v40uFT4NV1Ys7TEcG4fkLOhq6E1sUJY9UoUyWHoMj0uRGaiwxzl1=w557)

고려해야할 사항

**설명을 제공하는 텍스트여야됨**

- 일반적인 텍스트를 사용하면 안됨(페이지, 여기 등)
- 주제와 관련없으면 안됨

**텍스트를 간결하게**

링크를 일반 텍스트 처럼 보이게 CSS를 수정하면 안됨!!

> 링크 대상에 주의해주세요! 링크 대상에게 내 사이트의 평판을 넘겨주길 원하지 않을 경우에는 nofollow옵션을 사용해 주세요.
>
> NOFOLLOW는 언제 유용하게 사용될까요? 사이트에 공개 댓글을 사용할 수 있는 블로그가 있는 경우 댓글 내에 링크가 있으면 내가 불편하게 느끼는 페이지에 내 사이트의 평판이 전달될 수 있습니다. 블로그의 댓글 영역은 댓글 스팸에 매우 취약합니다. 사용자가 추가한 링크를 NOFOLLOW하면 내 페이지에서 어렵게 얻은 평판을 스팸 사이트에 넘겨주지 않도록 보장됩니다.

```html
<a href="https://www.example.com" rel="nofollow">Anchor text here</a>
```

### 이미지에 Alt속성을 적절히 사용해주세요

이미지 저장시 피해야될 사항

- 일반적인이름 imgae.jpg같은 건 피해주세요
- 매우 긴 파일이름

### 사이트를 모바일 친화적으로 만들어주세요

LightHout와 같은 모바일 친화성 도구를 사용해 친화정도를 측정할 수 있음!!

색인이 정확하게 생성될 수 있도록 모바일 사이트를 구성하는법

1. 모바일 사이트에 맞게 페이지 형식을 지정했다면 구글에게 알려주세요!
2. 반응형 웹디자인을 사용할 경우. meta name=viewport태그를 사용해 콘텐츠 조정 방법을 알려주세요.
