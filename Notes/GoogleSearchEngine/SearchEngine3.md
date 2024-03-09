# 구글 검색엔진

> author: @rud1676

## 목차

- robots란 무엇일까

## 대망의 robots

> robots.txt 파일은 웹사이트 루트 디렉토리에 위치하는 파일, 검색엔진 크롤러가 사이트 내 어떤 페이지를 크롤링할 수 있는지 지정하는 역활. 이 파일은 사이트의 크롤링을 관리하며, 특정 URL을 검색엔진 결과에서 제외하고자 할 때 사용된다.

주로 크롤링 트래픽 관리(서버 과부화 방지), 비중요 페이지 숨기기(비중요,불필요페이지 크롤링 방지) 에 사용한다.

### 사용 시 주의사항

- 검색결과 제외: robots.txt는 페이지를 Google 검색결과에서 완전히 숨기는 것이 아님. 완전히 숨기기 위해서는 "noindex 태그 사용이나 페이지 비밀번호 설정"이 필요.
- CMS프로그램 사용 시: Wix, Blogger,jekyll 같은 CMS를 사용하는 경우, CMS의 검색 설정을 통해 크롤링을 관리.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>비공개 페이지</title>
    <meta name="robots" content="noindex, follow" />
  </head>
  <body>
    <h1>이 페이지는 검색 결과에 나타나지 않습니다</h1>
  </body>
</html>
```

비밀번호 설정법: 아파치나 서버 설정에 .htpasswd 파일을 만들어 설정할 수 있음.

```txt
AuthType Basic
AuthName "Restricted Content"
AuthUserFile /full/path/to/.htpasswd
Require valid-user
```

### 파일형식마다 사용법.

- 웹페이지: 크롤링 트래픽 관리 및 비중요 페이지 숨기기에 사용
- 미디어 파일: 크롤링 트래픽 관리 및 Google 검색결과에서의 미디어 파일 숨기기 가능
- 리소스 파일: 페이지 로딩에 중요하지 않은 리소스 파일의 크롤링 차단 가능. 단, 페이지 이해에 중요한 리소스는 차단하지 않아야 함

### 한계

- 크롤러 준수 여부: 모든 크롤러가 robots.txt 지침을 준수하는것이 아님. 보안을 위해서는 추가적인 방법을 고려해야한다!
- 크롤러별 구문 해석 차이: 크롤러마다 robots.txt 파일의 규칙을 다르게 해석할 수 있다.
- 외부 링크를 통한 색인 생성: robots.txt로 차단된 페이지라도 외부에서 링크되어 있으면 색인이 생성될 수 있다.

## robots.txt 기본구조

- User-agent: 크롤러를 지정. \*는 모든 크롤러를 의미.
- Disallow: 크롤링을 금지할 페이지나 디렉토리를 지정.
- Allow: 크롤링을 허용할 페이지나 디렉토리를 지정. (선택적)
- Sitemap: 사이트맵 파일의 위치를 지정. (선택적)

기본적인 파일은 아래와 같다(by 챗지피티)

```robots.txt
User-agent: Googlebot
Disallow: /nogooglebot/

User-agent: *
Allow: /

Sitemap: https://www.example.com/sitemap.xml
```

1. 이름이 Googlebot인 사용자 에이전트는 https://example.com/nogooglebot/으로 시작하는 URL을 크롤링할 수 없습니다.
2. 그 외 모든 사용자 에이전트는 전체 사이트를 크롤링할 수 있습니다. 이 부분을 생략해도 결과는 동일합니다. 사용자 에이전트가 전체 사이트를 크롤링할 수 있도록 허용하는 것이 기본 동작입니다.
3. 사이트의 사이트맵 파일은 https://www.example.com/sitemap.xml에 있습니다.

### 생성하기

생성할때 아래와 같은 흐름으로 작업합니다.

1. robots.txt만들고
2. 그 파일에 규칙 추가하고
3. 사이트 루트 디렉토리에 업로드하고
4. 규칙을 테스트!

파일 인코딩은 UTF-8로 지정합니다!

### 자잘한 규칙들

- 기본적인 가정은 사용자 에이전트에서 disallow 규칙으로 차단되지 않은 페이지나 디렉터리를 크롤링할 수는 있음.
- 규칙은 대소문자를 구분한다. 예를 들어, disallow: /file.asp는 https://www.example.com/file.asp에 적용되지만 https://www.example.com/FILE.asp에는 적용되지 않는다.
- \# 문자는 주석의 시작입니다
- 유저 그룹 하나당 규칙을 지정할 수 있음

```
# Example 1: Block only Googlebot
User-agent: Googlebot
Disallow: /

# Example 2: Block Googlebot and Adsbot
User-agent: Googlebot
User-agent: AdsBot-Google
Disallow: /

# Example 3: Block all crawlers except AdsBot (AdsBot crawlers must be named explicitly)
User-agent: *
Disallow: /
```

### 테스트

- 배포된 사이트에서 SearchConsole에 robots.txt테스터 도구로 테스팅 가능
- 구글 오픈소스에 robots.txt 라이브러리에서 빌드해서 직접 로컬에서 테스트할 수 있음.

### 구글에게 알리기

보통은 robots를 바로 탐색을 하지만, 업데이트를 즉시 해야되는 경우(캐시된것을 바로 업데이트해야할 필요에) 구글에 직접 등록한다

### 예시

```
User-agent: *
Disallow: /
Allow: /public/
```

```
User-agent: Googlebot-Image
Disallow: /images/dogs.jpg
```

```
User-agent: *
Disallow: /

User-agent: Mediapartners-Google
Allow: /
```

## 표준화

- 표준화는 중복 콘텐츠 문제를 해결하기 위한 Google의 알고리즘. 이 프로세스를 통해 Google은 대표 페이지, 즉 "표준 URL"로 선택하여 검색 결과에 표시함. 표준화의 주요 목적과 방법을 요약.

### 목적

- 중복 콘텐츠 관리: 같은 내용을 담고 있는 여러 페이지가 있을 경우, 이를 하나로 통합.
- 검색 경험 개선: 사용자에게 중복된 콘텐츠를 여러 번 보여주는 것을 방지하고, 가장 관련성 높은 콘텐츠 제공.
- 크롤링 효율성 증가: Google 크롤러가 중복 콘텐츠를 반복해서 크롤링하는 것을 줄여, 사이트의 서버 부담을 경감.

### 원인

- 지역별 변형: 같은 언어로 제공되는 다른 지역을 대상으로 한 콘텐츠.
- 기기 변형: 모바일 버전과 데스크톱 버전 등 기기에 따라 제공되는 콘텐츠.
- 프로토콜 변형: HTTP 버전과 HTTPS 버전.
- 사이트 기능: 카테고리 페이지의 정렬 및 필터링 결과.
- 실수로 인한 변형: 실수로 공개된 사이트의 데모 버전 등.

### 표준화 과정에서 고려되는 요소

- 프로토콜 및 리디렉션: HTTP 및 HTTPS, 리디렉션 사용 여부.
- 사이트맵: 사이트맵에 포함된 URL.
- rel="canonical": 페이지의 표준 버전을 명시하는 태그.

### 우리가 해야할 일(권고 사항)

- 표준 URL 명시: 선호하는 표준 URL을 명시하기 위해 rel="canonical" 태그 사용.
- 언어 및 지역 설정: 다양한 언어와 지역을 대상으로 하는 사이트의 경우, 적절한 현지화 설정 적용.

### 그러나..

- Google은 여러 신호를 기반으로 가장 유용하고 완전한 페이지를 표준 페이지로 선택.
- 사이트 소유자는 선호하는 URL을 명시할 수 있지만, Google이 반드시 이를 따르는 것은 아님.

### 중복콘텐츠 예시(by Chat GPT)

- 지역별 변형: 같은 언어로 제공되는 콘텐츠가 여러 지역을 대상으로 하여 여러 URL을 통해 접근 가능한 경우입니다. 예를 들어, 미국 영어로 작성된 웹페이지가 example.com/en-us와 example.com/en-gb를 통해 접근 가능할 수 있습니다. 페이지 내용은 거의 동일하지만, 지역별 차이(예: 날짜 형식, 철자)를 반영할 수 있습니다.
- 기기 변형: 같은 페이지가 데스크톱 사용자와 모바일 사용자를 위해 다른 URL에 제공되는 경우입니다. 예를 들어, example.com/product와 m.example.com/product가 이에 해당합니다.
- 프로토콜 변형: 웹사이트가 HTTP와 HTTPS 버전 모두를 제공하는 경우, 콘텐츠의 내용은 동일하지만 프로토콜만 다를 수 있습니다. http://example.com과 https://example.com이 예시입니다.
- 사이트 기능: 콘텐츠는 동일하지만, URL에 정렬이나 필터링 파라미터가 추가된 경우입니다. 예를 들어, example.com/products?sort=price와 example.com/products?sort=name에서 페이지는 동일한 상품 목록을 보여주지만 정렬 순서만 다릅니다.
- 실수로 인한 변형: 개발 과정에서 생성된 테스트 버전의 페이지가 실수로 검색 엔진에 의해 발견되고 접근 가능한 상태가 되는 경우입니다.

### 표준 URL에 관하여(우리가 고려해야할 것)

**표준 URL로 설정하기**

- 리디렉션 (가장 강력한 신호):리디렉션 대상 URL을 표준 URL로 지정. 주로 301 리디렉션을 사용합니다.
- rel="canonical" 링크 주석:

```html
<head>
  <link rel="canonical" href="https://example.com/official-page" />
</head>
```

해당 페이지가 중복 콘텐츠 중 표준 페이지임을 나타냄

- 사이트맵 포함:사이트맵에 포함된 URL은 표준 URL로 간주되는 약한 신호. 사이트맵에 선호하는 URL을 포함

**왜지정을해야하는가?**

- 검색 결과 정확성: 선호하는 URL이 검색 결과에 표시.
- 신호 통합: 유사하거나 중복된 페이지에 대한 검색 엔진의 신호를 선호하는 URL로 통합.
- 측정 용이성: 다양한 URL에 걸친 콘텐츠의 효과 측정을 단순화.
- 크롤링 효율성: 중복 콘텐츠의 크롤링에 리소스를 낭비하지 않고, 새로운 또는 업데이트된 페이지의 크롤링에 집중.

**표준화과정에서 반드시 알아야될 사항**

- 표준화를 위해 robots.txt 파일이나 URL 삭제 도구로 표준화 하지 않기! 이들은 페이지를 검색 결과에서 완전히 숨기거나 반응하지 않도록 하기위해 사용하는 것.
- 서로 다른 표준화 기술을 혼합하여 사용하지 않습니다. 예를 들어, 사이트맵과 rel="canonical"을 동시에 다르게 설정x.
- noindex 태그는 표준 페이지 선택을 방해할 수 있으므로, 주의해서 사용하기.

**canonical예시(GPT)**

```html
<html>
  <head>
    <title>Explore the world of dresses</title>
    <link rel="canonical" href="https://example.com/dresses/green-dresses" />
    <!-- other elements -->
  </head>
  <!-- rest of the HTML -->
</html>
```

rel="canonical" 속성이 있는 link 요소를 중복 페이지의 head 섹션에 추가하여 표준 페이지로 연결되도록 합니다. 예를 들면 다음과 같습니다.

```html
<html>
  <head>
    <title>Explore the world of dresses</title>
    <link
      rel="alternate"
      media="only screen and (max-width: 640px)"
      href="https://m.example.com/dresses/green-dresses"
    />
    <link rel="canonical" href="https://example.com/dresses/green-dresses" />
    <!-- other elements -->
  </head>
  <!-- rest of the HTML -->
</html>
```

표준 페이지에 별도의 URL에 있는 모바일 변형이 있는 경우 모바일 버전의 페이지를 가리키는 rel="alternate" link 요소를 해당 페이지에 추가합니다.

### 표준화에서 생기는 문제 해결하기

표준화 과정에서 검사하는 요소는 아래와 같다.

- URL 검사 도구 사용: Google에서 어떤 페이지를 표준으로 하는지 확인할 수 있음.
- 표준 지정과 Google 선택: 표준 페이지를 지정해도 Google이 다른 페이지를 표준으로 선택할 가능성이 있다. 이는 콘텐츠 품질 등 다양한 이유로 발생.
- Google 선택의 효율성 평가: Google이 선택한 표준 URL이 사용자에게 더 유용한지 평가.

**예시(GPT)**
예시:
웹사이트에 https://example.com/product와 https://example.com/product?color=blue 같은 중복 페이지가 있다고 가정해봅시다. 여기서 https://example.com/product 페이지를 표준으로 지정하고 싶습니다. URL 검사 도구를 사용하여 https://example.com/product?color=blue 페이지가 Google에 의해 어떻게 처리되고 있는지 확인할 수 있습니다. 도구가 https://example.com/product를 표준 URL로 보여주면, 설정이 올바르게 적용된 것입니다.

예시:
https://example.com/product를 표준으로 지정했지만, Google이 https://example.com/product?color=blue를 표준으로 선택했다면, 이는 후자의 URL이 사용자에게 더 관련성이 높거나 콘텐츠 품질이 더 높다고 판단했기 때문일 수 있습니다. 이 경우, 두 URL의 콘텐츠와 사용자 경험을 다시 평가해보아야 합니다.

예시:
Google이 https://example.com/product?color=blue를 표준으로 선택했고, 이 페이지가 사용자에게 더 많은 클릭과 전환을 가져다준다면, Google의 선택이 실제로 비즈니스에 더 이익이 될 수 있습니다. 이 경우, Google의 선택을 수용하고, 다른 중복 페이지들을 이 페이지로 리디렉션하거나 콘텐츠를 개선하여 Google의 선택을 뒷받침하는 방향으로 전략을 조정할 수 있습니다.

**일반적으로 경험하는 것들**

- 현지화된 주석 없는 언어 변형: 여러 언어 또는 지역별 사이트가 동일 콘텐츠를 제공할 경우, hreflang 주석을 사용하여 현지화된 사이트를 명시하세요.
- 잘못 사용된 표준 요소: 일부 CMS 또는 플러그인이 표준화 기술을 잘못 사용하여 외부 URL을 표준으로 지정할 수 있습니다. 개발자 도구를 통해 HTML을 검사하세요.
- 잘못 구성된 서버: 서버 구성 오류로 인해 교차 도메인 URL 선택이 발생할 수 있습니다. 예기치 않은 교차 도메인 리디렉션 또는 soft 404 페이지 반환 등이 이에 해당합니다.
- 악의적인 해킹: 해킹으로 인해 페이지에 교차 도메인 rel="canonical" 링크 또는 리디렉션이 삽입되어 악성 또는 스팸 URL을 표준으로 지정할 수 있습니다.
- 신디케이션 콘텐츠: 신디케이션 파트너가 콘텐츠의 중복을 방지하기 위해 표준 링크 요소를 잘못 사용할 수 있습니다. 신디케이션 콘텐츠 차단을 고려하세요.
- 모방 웹사이트: 다른 사이트가 사용자의 콘텐츠를 무단으로 사용하여 Google이 그 사이트의 URL을 표준으로 선택하는 경우가 있습니다. 저작권 침해 신고를 통해 해결할 수 있습니다.

**예시(GPT)**

1. 현지화된 주석 없는 언어 변형
   상황: 당신은 영어로 된 콘텐츠를 미국(example.com/en-us), 영국(example.com/en-gb), 오스트레일리아(example.com/en-au) 대상으로 제공합니다. 이 콘텐츠는 기본적으로 동일하지만, 각 지역에 맞게 약간의 차이(예: 철자, 통화)를 두고 있습니다.

문제: Google이 이 세 페이지 중 하나를 무작위로 표준으로 선택하게 됩니다. 이로 인해 특정 지역의 사용자에게 가장 적합한 콘텐츠가 제공되지 않을 수 있습니다.

해결 방법: 각 페이지의 <head>에 hreflang 주석을 추가하여 Google에 각 지역별 콘텐츠의 관계를 명시합니다. 예를 들어, 미국 페이지에는 다음과 같은 태그를 추가합니다:

```html
<link rel="alternate" hreflang="en-us" href="https://example.com/en-us" />
<link rel="alternate" hreflang="en-gb" href="https://example.com/en-gb" />
<link rel="alternate" hreflang="en-au" href="https://example.com/en-au" />
```

2. 잘못 구성된 서버
   상황: 당신의 웹사이트 example.com에서, 특정 리소스 요청에 대해 예기치 않게 anotherdomain.com의 콘텐츠를 반환하게 됩니다.

문제: 이는 서버 구성 오류 또는 잘못된 DNS 설정으로 인해 발생할 수 있으며, Google이 anotherdomain.com의 URL을 표준으로 선택하게 만들 수 있습니다.

해결 방법: 서버 로그를 확인하고, 리디렉션 규칙이나 DNS 설정을 검토하여 오류를 수정합니다.

3. 신디케이션 콘텐츠
   상황: 당신은 파트너 웹사이트와 콘텐츠를 공유합니다. 파트너 사이트가 해당 콘텐츠를 자신의 웹사이트에 게재하면서 rel="canonical"을 잘못 설정해, 당신의 원본 콘텐츠를 가리키지 않게 됩니다.

문제: 이는 Google이 신디케이션 파트너의 페이지를 표준으로 잘못 인식하게 만들 수 있습니다.

해결 방법: 파트너와 협력하여 신디케이션된 콘텐츠에 당신의 웹사이트를 가리키는 올바른 rel="canonical" 링크를 설정하도록 합니다.

## 모바일 사이트에 대한 색인 권장사항

### 모바일 사이트 만들기

- 반응형 웹 디자인을 구현하여 모든 기기에서 동일한 콘텐츠를 제공하며, 화면 크기에 맞게 콘텐츠를 조정.
- 동적 게재를 사용하여 **같은 URL에서 다른 기기 유형에 맞는 HTML을 제공**.
- 별도의 URL 구성에서는 모바일과 데스크톱 버전을 위한 다른 URL을 사용하되, 각각에 적합한 콘텐츠를 제공.

### Google이 콘텐츠에 액세스하고 렌더링할 수 있도록 하기

- 모바일과 데스크톱 버전 모두에서 동일한 robots meta 태그를 사용합니다.

```html
<meta name="robots" content="index, follow" />
```

- 사용자 상호작용이 필요 없는 콘텐츠 로딩 방식을 사용하여 Google이 콘텐츠를 쉽게 크롤링할 수 있도록 합니다.

> lazy loading 기법을 사용할 경우, IntersectionObserver API 등을 활용하여 크롤러가 콘텐츠를 인식할 수 있도록 설정하기.

- 리소스 URL 차단을 피하고, Google이 리소스를 크롤링할 수 있도록 robots.txt 설정을 확인합니다.

> robots.txt 파일을 검토하고, 웹사이트의 중요 리소스에 대한 Disallow 규칙이 없는지 확인하세요.
> 예를 들어, User-agent: Googlebot과 Disallow: /js/ 또는 Disallow: /css/ 같은 규칙이 없어야 합니다.
> Google의 무료 도구인 Search Console의 'robots.txt 테스터'를 사용하여 robots.txt 파일이 Google 크롤러에 올바르게 설정되어 있는지 테스트할 수 있습니다.

### 데스크톱과 모바일에서 콘텐츠의 일관성 유지

- 모바일 사이트에 데스크톱 사이트와 동일한 콘텐츠와 메타데이터(title, 메타 설명)를 포함시킵니다.
- 구조화된 데이터는 데스크톱과 모바일 버전에서 동일하게 유지하며, 모바일 URL을 포함한 적절한 URL을 사용합니다.

### 별도 URL을 사용하는 경우 추가 권장사항

- hreflang 태그를 올바르게 설정하여 다국어 또는 다지역 사이트의 각 버전을 올바르게 가리키도록 합니다.
- 데스크톱과 모바일 사이트 모두에 대해 Google Search Console에서 확인하여, 두 버전 모두에 대한 데이터와 메시지에 접근할 수 있도록 합니다.
- robots.txt와 rel=canonical 및 rel=alternate 태그가 올바르게 구성되어 있는지 확인합니다.

### 모바일 버전의 별도 URL에 관한 추가 권장사항

- 오류 페이지의 일관성 유지

예를 들어, 데스크톱 사이트에서는 콘텐츠를 정상적으로 제공하는 반면, 모바일 사이트에서는 오류 페이지를 반환하는 경우, Google은 해당 모바일 페이지를 색인에서 제외할 수 있음.

- 프래그먼트 URL 사용 피하기

모바일 버전에서는 URL의 프래그먼트(예: #section) 부분을 사용하지 않아야 한다. 프래그먼트 URL은 색인생성 못함.

- 콘텐츠 일관성 유지

여러 콘텐츠를 제공하는 데스크톱 사이트의 각 페이지에 해당하는 모바일 버전이 있어야 한다. 리다이랙션으로 처리를 하면 누락 될 확률이 크다.

- Google Search Console 사용

사이트의 데스크톱 및 모바일 버전 모두를 Google Search Console에서 확인하여, 두 버전 모두에 관한 데이터와 메시지에 접근할 수 있도록 한다. Google이 사이트의 색인 생성을 모바일 중심으로 전환할 때, 데이터 변동이 발생할 수 있음.

- 별도 URL의 hreflang 링크 확인

다국어 사이트의 경우, 모바일과 데스크톱 URL을 별도로 설정하는 rel="alternate" hreflang 링크를 사용해야 합니다. 이는 Google에게 각 언어 또는 지역별 페이지의 모바일과 데스크톱 버전의 위치를 정확히 알려줍니다.

```html
<link rel="canonical" href="https://example.com/" />
<link rel="alternate" hreflang="es" href="https://m.example.com/es/" />
<link rel="alternate" hreflang="fr" href="https://m.example.com/fr/" />
<link rel="alternate" hreflang="de" href="https://m.example.com/de/" />
<link rel="alternate" hreflang="th" href="https://m.example.com/th/" />

/*PC */
<link rel="canonical" href="https://example.com/" />
<link
  rel="alternate"
  media="only screen and (max-width: 640px)"
  href="https://m.example.com/"
/>
<link rel="alternate" hreflang="es" href="https://example.com/es/" />
<link rel="alternate" hreflang="fr" href="https://example.com/fr/" />
<link rel="alternate" hreflang="de" href="https://example.com/de/" />
<link rel="alternate" hreflang="th" href="https://example.com/th/" />
```

- robots.txt 및 링크 요소 설정
  robots.txt 규칙이 사이트의 데스크톱 및 모바일 버전에서 모두 정상적으로 작동하는지 확인합니다. 또한, 데스크톱 URL을 표준(rel="canonical")으로 지정하고, 모바일 URL을 해당 데스크톱 URL의 대체(rel="alternate")로 지정해야 합니다. 이는 Google에게 어떤 버전이 원본 콘텐츠를 포함하고 있는지 명확하게 알려주는 역할을 합니다.

```html
<link rel="canonical" href="https://example.com/" />

<link rel="canonical" href="https://example.com/" />
<link
  rel="alternate"
  media="only screen and (max-width: 640px)"
  href="https://m.example.com/"
/>
```

## 문제

```txt
robots.txt 파일의 주된 용도는 무엇인가요?
A) 사이트의 디자인 변경
B) 사용자 인증 관리
C) 검색 엔진 크롤러의 사이트 크롤링 관리
D) 웹사이트 속도 향상
E) 데이터베이스 백업

robots.txt 파일에서 "Disallow: /" 지시어의 목적은 무엇인가요?
A) 모든 사용자에게 사이트 접근 허용
B) 모든 크롤러의 사이트 접근 차단
C) 사이트의 특정 부분을 강조
D) 사이트의 속도를 제한
E) 사이트에 광고를 추가

rel="canonical" 태그의 목적은 무엇인가요?
A) 웹 페이지의 색상 스킴을 설정
B) 다른 사이트로의 리디렉션 지정
C) 중복 콘텐츠 문제 해결을 위해 선호하는 URL 지정
D) 웹사이트의 메인 페이지를 설정
E) 사이트맵 생성

다음 중 모바일 사이트를 구축할 때 권장되는 구성 방식은?
A) 별도의 모바일 앱만 제공
B) 오직 데스크톱 버전만 제공
C) 반응형 웹 디자인
D) 오직 텍스트 기반 콘텐츠 제공
E) 모든 페이지를 PDF로 제공

robots.txt 파일에서 "Allow: /" 지시어의 역할은 무엇인가요?
A) 모든 크롤러의 사이트 접근을 차단
B) 사이트의 특정 부분에 대한 접근을 허용
C) 사이트의 모든 부분에 대한 접근을 허용
D) 사이트 접속을 사용자 인증에 제한
E) 사이트의 콘텐츠를 암호화

robots.txt 파일에서 특정 사용자 에이전트에 대한 지시어를 설정하지 않으면 어떻게 되나요?
A) 사이트가 인터넷에서 삭제됨
B) 모든 사용자 에이전트에 대해 사이트 접근이 허용됨
C) 사이트 접속이 완전히 차단됨
D) 사이트가 자동으로 데스크톱 모드로 전환됨
E) 사이트에 바이러스가 생성됨

"hreflang" 태그의 주된 목적은 무엇인가요?
A) 사이트의 기본 언어 설정
B) 다국어 사이트에서 언어별 페이지 버전을 지정
C) HTML 버전을 지정
D) 사이트의 프로그래밍 언어 변경
E) 사이트의 폰트 스타일 지정

robots.txt 파일의 변경사항이 Google에 바로 반영되지 않는 이유는 무엇인가요?
A) Google이 변경사항을 무시하기 때문
B) 인터넷 연결 문제
C) 변경사항이 유효하지 않기 때문
D) Google이 사이트를 다시 크롤링하고 인덱싱하는 데 시간이 걸리기 때문
E) robots.txt 파일이 서버에 제대로 업로드되지 않았기 때문

*robots.txt 파일에서 "User-agent: " 지시어의 의미는 무엇인가요?
A) 특정 사용자 에이전트를 지정
B) 모든 사용자 에이전트를 차단
C) 특정 사용자 에이전트에 대한 접근을 허용
D) 모든 사용자 에이전트에 대한 지침을 적용
E) 사용자 에이전트의 이름을 변경

robots.txt 파일과 관련 없는 것은?
A) 크롤링 제어
B) 검색 엔진 최적화
C) 사용자 인증 관리
D) 검색 엔진 결과에서 페이지 제외
E) 사이트맵 지정

```

<details>
  <summary>첫번째토글</summary>
  1. 정답: C
2. 정답: B
3. 정답: C
4. 정답: C
5. 정답: C
6. 정답: B
7. 정답: B
8. 정답: D
9. 정답: D
10. 정답: C
</details>
