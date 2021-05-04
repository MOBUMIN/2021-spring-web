
# 1. 사이드바 메뉴 만들기

이번 주의 시작은 프로젝트 팀에서 본격적으로 React 개발을 시작하기 위해 사이드바 메뉴를 구성하는 작업을 수행했습니다.
사이드바 메뉴를 구성할 때 핵심적으로 사용한 것은 `node-sass`를 통한 스타일 구성과 `react-router-dom`을 통한
라우팅이었습니다. 지금까지 학습한 내용을 응용한다는 점에서 매우 신선하고 재미있는 경험이었습니다.
이번 작업동안 설정한 파일 디렉토리 구조는 다음과 같습니다.

```text

src
 |
 |ㅡㅡㅡ components // React 컴포넌트
 |          |ㅡㅡㅡㅡㅡSideBar.js
 |          |ㅡㅡㅡㅡㅡSideMenu.js
 |
 |ㅡㅡㅡ pages // 라우팅할 페이지
 |        |ㅡㅡㅡㅡㅡ Home.js
 |        |ㅡㅡㅡㅡㅡ Monitoring.js
 |        |ㅡㅡㅡㅡㅡ Settings.js
 |
 |ㅡㅡㅡ styles // 스타일 구성
          |ㅡㅡㅡㅡㅡ components
          |               |ㅡㅡㅡㅡㅡ_sideBar.scss
          |
          |ㅡㅡㅡㅡㅡ config
          |             |ㅡㅡㅡㅡㅡ_variable.scss
          |             |ㅡㅡㅡㅡㅡreset.scss
          |
          |ㅡㅡㅡㅡㅡ pages
          |            |ㅡㅡㅡㅡㅡ_pages.scss
          |
          |ㅡㅡㅡㅡㅡ main.scss

```

## 1-1. 라우팅 구성하기

가장 먼저 페이지 라우팅을 구현하기 위해 `react-router-dom` 패키지를 설치했습니다.
그 다음 index.js에 `BrowserRouter`를 설정하였습니다.

```JavaScript

import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
 document.getElementById("root")
);

```

이후 App.js 에서 라우팅 기능을 추가했습니다.
라우팅은 `Route` 컴포넌트를 통해 경로와 렌더링할 페이지(컴포넌트)를 설정함으로써 수행할 수 있습니다.

```JavaScript

import { Route } from "react-router-dom";
import Home from "./pages/Home";
import Monitoring from "./pages/Monitoring";
import Settings from "./pages/Settings";

function App() {
  return (
    <>
      <Route path="/" component={Home} exact />
      <Route path="/monitoring" component={Monitoring} />
      <Route path="/settings" component={Settings} />
    </>
  );
}
export default App;

```

## 1-2. 컴포넌트 구성하기

컴포넌트는 사이드 바 컴포넌트와 나중에 메뉴의 원활한 추가를 위해 `SideMenu` 컴포넌트로 따로 구성하였습니다.
메뉴 컴포넌트에서 사용할 react-icons의 아이콘은 children props로 전달해주었습니다.

```JavaScript

import React from "react";
import { FaHome } from "react-icons/fa";
import { FiMonitor, FiSettings } from "react-icons/fi";
import SideMenu from "./SideMenu";

function SideBar() {
  return (
    <nav className="sidebar">
      <header>Menu</header>

      <SideMenu menu="Home" route="/">
        <FaHome className="menu-icon" />
      </SideMenu>
      <SideMenu menu="Monitoring" route="/monitoring">
        <FiMonitor className="menu-icon" />
      </SideMenu>
      <SideMenu menu="Settings" route="/settings">
        <FiSettings className="menu-icon" />
      </SideMenu>
    </nav>
  );
}
export default SideBar;

```

사이드바 메뉴에는 클릭 시 페이지 이동을 통한 링크 기능을 추가해주어야합니다. 하지만 &lt;a&gt; 태그를 사용하면 페이지 전체가
새로고침되는 문제가 발생하기 때문에 `react-router-dom`의 `Link` 컴포넌트를 사용했습니다.

```JavaScript

import { Link } from "react-router-dom";

function SideMenu({ menu, route, children }) {
  return (
    <ul>
      <li className="sidemenu">
        <Link to={route}>
          {children} {menu}
        </Link>
      </li>
    </ul>
  );
}
export default SideMenu;

```

## 1-3. 스타일 구성하기

스타일은 scss를 통해 스타일을 구성하기 위해 `node-sass` 패키지를 설치해주었습니다.
css 스타일의 초기화를 위해 reset.scss를 추가해주었고 사용할 색상은 변수로 선언해
_variable.scss에서 관리하였습니다.

```scss

.sidebar {
  position: absolute;
  width: 15rem;
  height: 100%;
  background-color: $sidebar;

  header {
    background-color: $sidebar-header;
    padding: 1rem 0.5rem;
    font-size: 1.25rem;
    font-weight: 700;
  }
  .sidemenu {
    &:hover {
      background-color: $sidebar-hover;
      transition: background-color 0.2s linear;
    }
    a {
      display: flex;
      color: $white;
      padding: 1rem 0.5rem;

      .menu-icon {
        margin-right: 0.5rem;
      }
    }
  }
}

```

# 2. 정적 웹 호스팅 (with AWS S3)

Github에서는 gh-pages 브랜치를 통한 정적 웹 호스팅 기능을 지원하지만 private 저장소를 gh-pages를 통해
호스팅하기 위해서는 Github pro 계정이 필요합니다. 전 pro 계정을 보유하고 있지 않아 이 참에 다른 외부
사이트를 통해 작성한 React코드를 정적 웹 호스팅하는 방법을 알아보기 위해 관련 자료를 조사했습니다.

저는 다음의 [자료](https://react-etc.vlpt.us/08.deploy-s3.html)를 참고해 AWS S3를 이용하여
위에서 구현한 사이드 바에 대해 정적 웹 호스팅을 수행하였습니다. AWS S3로 정적 웹 호스팅하는 방법은
다음과 같은 순서로 수행할 수 있습니다.

1. AWS IAM과 AWS CLI를 통한 사용자 등록
2. AWS S3 버킷 생성
3. AWS S3와 빌드 폴더 연동
4. 배포하기

위 과정을 통해 작성한 코드가 정상적으로 배포된 것을 확인할 수 있었습니다.

[배포 확인하기](http://kids-keeper.s3-website.ap-northeast-2.amazonaws.com/)

&lt;참고자료&gt;

- [리액트 앱 AWS S3, CloudFront 에 배포하기](https://react-etc.vlpt.us/08.deploy-s3.html)
