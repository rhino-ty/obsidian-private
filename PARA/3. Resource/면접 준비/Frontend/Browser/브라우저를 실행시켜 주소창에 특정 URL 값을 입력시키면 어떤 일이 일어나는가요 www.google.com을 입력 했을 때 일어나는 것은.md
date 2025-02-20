---
sticker: emoji//2b50
---
[What happens when you types Google?](https://devjin-blog.com/what-happen-browser-search/) 여기 보기

1. 브라우저는 입력된 주소를 분석하여 해당 도메인의 IP 주소를 알아내기 위해 **DNS(Domain Name System) 서버에 요청**합니다
2. DNS 서버는 해당 도메인의 **IP 주소를 반환**합니다.
3. 브라우저는 해당 IP 주소로 **HTTP 요청**을 보냅니다.
4. 구글 서버는 브라우저의 요청을 받고, 요청에 맞는 **HTML, CSS, JS 파일 등을 포함한 응답**을 반환합니다.
5. 브라우저는 서버에서 받은 응답을 파싱하여 **렌더링 엔진**에 전달합니다.
6. 렌더링 엔진은 HTML 문서를 파싱하고, **DOM(Document Object Model) 트리**를 생성합니다.
7. CSS 파일도 함께 파싱되며, **CSSOM(CSS Object Model) 트리**를 생성합니다.
8. DOM 트리와 CSSOM 트리가 결합되어 **렌더 트리(Render Tree)**가 생성됩니다.
9. 브라우저는 렌더 트리를 기반으로 화면에 컨텐츠를 그리고, JS로 사용자가 인터랙션할 수 있는 기능을 제공합니다.

![[Pasted image 20250220221735.png]]
