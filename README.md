# 정산 보기 링크 사이트

`https://gongjaksoft26.github.io/dutchpay/s/#<코드>` 링크가 가리키는 정적 페이지와 안드로이드 앱 링크 검증 파일이다.
서버·DB가 없다. 정산 내용은 링크의 `#` 뒤에만 있고 브라우저가 서버로 보내지 않는다.

## 동작

| 받는 사람 | 일어나는 일 |
|---|---|
| 앱이 있다 | 링크를 누르면 **앱이 바로 열려** 정산 내역을 보여 준다 (앱 링크 검증이 되어 있을 때) |
| 앱이 없다 | 이 페이지가 열려 정산 내역을 보여 주고, "앱 설치" 버튼이 Play로 보낸다. 링크의 `referrer`로 정산이 따라가서, 설치 후 첫 실행에서 앱이 그 정산을 보여 준다 |

## 배포 (사용자가 한다)

1. GitHub에 **`gongjaksoft26.github.io`** 이름의 **공개** 저장소를 만든다 (사용자 사이트. 앱 링크 검증은 도메인 루트의 `/.well-known/`이 필요해서 `learnup-legal` 같은 프로젝트 페이지로는 안 된다).
2. 이 `site/` 폴더의 **내용**(`.nojekyll`, `.well-known/`, `dutchpay/`)을 저장소 루트에 올린다. `.nojekyll`이 없으면 GitHub Pages가 `.well-known`을 무시한다.
3. Settings → Pages에서 main 브랜치 루트를 배포한다.
4. `.well-known/assetlinks.json`의 지문을 채운다 — Play Console → 앱 무결성(App integrity) → 앱 서명 → **앱 서명 키 인증서 SHA-256**. (업로드 키 지문이 아니다. Play가 서명해 사용자에게 가는 키다.)
5. 확인: `https://gongjaksoft26.github.io/.well-known/assetlinks.json`이 리다이렉트 없이 200으로 JSON을 준다.
6. 이후 `adb shell pm get-app-links com.gongjaksoft.dutchpaycalculator`에서 `gongjaksoft26.github.io: verified`가 나오는지 본다.

## 바꿀 때

호스트·경로를 바꾸면 `apps/app/lib/core/app_info.dart`(inviteHost/invitePath)와
`AndroidManifest.xml`의 intent-filter를 함께 바꾼다. 페이지의 검증 한도(`LIMITS`)는
`apps/app/lib/domain/settlement_link.dart`의 `SharedSettlement`와 같아야 한다.
