
# 1. react-map-gl로 데이터 시각화하기

이번 주는 본격적으로 react-map-gl을 통해 지도를 구성하고 geojson 데이터를 통해 서울시 행정구역을 시각화하는 작업을 수행했습니다.
이번 작업동안 설정한 파일 디렉토리 구조는 다음과 같습니다.

```text

src
 |
 |ㅡㅡㅡ components
 |        |ㅡㅡㅡㅡㅡ MapBox.js
 |
 |ㅡㅡㅡ containers
 |        |ㅡㅡㅡㅡㅡ MapBoxContainer.js
 |
 |ㅡㅡㅡ modules
 |        |ㅡㅡㅡㅡㅡ index.js
 |        |ㅡㅡㅡㅡㅡ mapbox.js
 |
 |ㅡㅡㅡ utils
 |        |ㅡㅡㅡㅡㅡ mapbox/mapStyle.js
 |
 |ㅡㅡㅡ webpack.config.js

```

## 1-1. mapbox로 지도 구성하기

react-map-gl을 통해 지도를 구성하기 위해서 [mapbox](https://www.mapbox.com/)의 accessToken이 필요합니다.
이후 viewport 초기 설정을 마치고 ReatMapGL 컴포넌트에 관련 속성값을 전달합니다.

&lt;components/Mapbox.js&gt;

```JavaScript
import dotenv from "dotenv";
import React, { useState } from "react";
import ReactMapGL from "react-map-gl";

dotenv.config();
// accessToken
const MAPBOX_TOKEN = process.env.REACT_APP_MAPBOX;

function MapBox() {
  // 초기 설정
  const [viewport, setViewport] = useState({
    width: 800,
    height: 800,
    latitude: 37.5642135,
    longitude: 127.0016985,
    zoom: 10,
  });

  return (
    <ReactMapGL
      {...viewport}
      mapStyle="mapbox://styles/mapbox/streets-v9"
      onViewportChange={(nextViewport) => setViewport(nextViewport)}
      mapboxApiAccessToken={MAPBOX_TOKEN}
    ></ReactMapGL>
  );
}
export default MapBox;

```

## 1-2. Redux로 상태관리하기

프로젝트의 전체적인 확장성을 고려하여 Redux로 상태관리를 수행하기로 결정하였습니다.
서울시 행정구역을 표현하기 위해 관련 데이터를 axios를 통해 비동기 처리로 가져오면서 상태관리를 해야하기 때문에
Redux 미들웨어로 redux-thunk를 사용하였습니다.

### 1) 액션 함수와 리듀서 구성

&lt;modules/mapbox.js&gt;

```JavaScript

import axios from "axios";

const DATA_LOADING = "mapbox/DATA_LOADING";
const DATA_SUCCESS = "mapbox/DATA_SUCCESS";
const DATA_FAILURE = "mapbox/DATA_FAILURE";

// 액션 함수 (redux-thunk를 사용해 비동기 처리)
export const getData = () => async (dispatch) => {
  dispatch({ type: DATA_LOADING });
  try {
    const fetchData = await axios.get(
      "https://raw.githubusercontent.com/vuski/admdongkor/master/ver20210101/HangJeongDong_ver20210101.geojson"
    );
    dispatch({ type: DATA_SUCCESS, payload: fetchData.data });
  } catch (e) {
    dispatch({ type: DATA_FAILURE, payload: e });
  }
};

const initialState = {
  loading: false,
  data: null,
  error: null,
};

// 리듀서
export default function mapboxReducer(state = initialState, action) {
  switch (action.type) {
    case DATA_LOADING:
      return { ...state, loading: true, data: null, error: null };
    case DATA_SUCCESS:
      return { ...state, loading: false, data: action.payload, error: null };
    case DATA_FAILURE:
      return { ...state, loading: false, data: null, error: action.payload };
    default:
      return state;
  }
}

```

### 2) 리듀서 함수를 결합

차후 확장성을 고려해 combineReducers를 통해 리듀서들을 결합하는 코드를 미리 구성하였습니다.

&lt;modules/index.js&gt;

```JavaScript

import { combineReducers } from "redux";
import mapboxReducer from "./mapbox";

const rootReducers = combineReducers({ mapboxReducer });

export default rootReducers;

```

### 3) 결합된 리듀서를 스토어에 전달

결합된 리듀서를 통해 스토어를 만들고 Provider를 통해 스토어를 전달하였습니다.

&lt;index.js&gt;

```JavaScript

import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import { createStore, applyMiddleware } from "redux";
import rootReducers from "./modules";
import ReduxThunk from "redux-thunk";
import App from "./App";
import { BrowserRouter } from "react-router-dom";

// 스토어 생성
const store = createStore(rootReducers, applyMiddleware(ReduxThunk));

// 스토어 전달
ReactDOM.render(
  <BrowserRouter>
    <Provider store={store}>
      <App />
    </Provider>
  </BrowserRouter>,

  document.getElementById("root")
);

```

## 1-3. 스토어 구독(subscribe)

스토어를 구독하기 위해서는 useSelector 훅을 사용하고 구성된 액션함수를 사용하기 위해 useDispatch 훅을 사용했습니다.
이후 state에 저장된 data값으로 행정구역을 구성하기 위해 Mapbox 컴포넌트에 props로 전달합니다.

&lt;containers/MapBoxContainer.js&gt;

```JavaScript

import React, { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import MapBox from "../components/MapBox";
import { getData } from "../modules/mapbox";

function MapBoxContainer() {
  let { loading, data, error } = useSelector((state) => state.mapboxReducer);
  const dispatch = useDispatch();

  // 컴포넌트가 마운트 되었을 때만 getData 함수 실행
  useEffect(() => {
    dispatch(getData());
  }, [dispatch]);

  if (loading) return <div>로딩중 ...</div>;
  if (error) return <div>에러 발생</div>;
  if (!data) return <div>행정 구역 데이터 불러오기 실패</div>;

  // 서울특별시만 해당하는 데이터를 필터링
  data.features = data.features.filter(
    (element) => element.properties.sidonm === "서울특별시"
  );
  return <MapBox data={data} />;
}
export default MapBoxContainer;

```

## 1-4. 행정구역 표현

행정구역을 표현하기 위해 전달받은 props 값을 통해 Source에 geojson데이터를 전달합니다.
이후 경계선을 표현하기 위해 layerStyle을 설정한 후 Layer 컴포넌트에 설정한 값을 전달합니다.

[layerStyle관련 문서](https://docs.mapbox.com/mapbox-gl-js/style-spec/layers)

&lt;components/Mapbox.js&gt;

```JavaScript

import ReactMapGL, { Layer, Source } from "react-map-gl";

const layerStyle = {
  id: "data",
  type: "fill",
  paint: {
    "fill-outline-color": "#ff0000",
    "fill-color": "#04e40f",
    "fill-opacity": 0.8,
  },
};

<Source type="geojson" data={data}>
    <Layer {...layerStyle} />
</Source>

```

## 1-5. 구현간 발생한 문제

- 새로고침 간 일시적인 404에러 발생 → SPA 특징에 기반한 문제
  - SPA의 라우팅은 색인 파일인 index.html을 HTML의 history 객체를 거쳐 수행됨
  - 하지만 새로고침 시 index.html을 거치지 않게 되어 라우팅이 정상적으로 이루어지지 않음 → 404 에러 발생
  - 서버 측에서 404 오류 발생 시 /index.html 로 이동하는 코드 작성으로 해결
  - [관련 자료](https://darrengwon.tistory.com/245)

- 지도가 로드된 후 다른 메뉴로 벗어났다가 다시 모니터링 메뉴로 돌아오면 에러 발생 → cleaning 함수의 부재에 의해 발생
  - props로 전달받은 data가 Mapbox 컴포넌트가 unmount 될 때 return 되지 않음
  - 다른 메뉴로 벗어났다가 다시 모니터링 메뉴로 돌아오게 되면 data를 다시 로드해 전달하므로 반환되지 않은
  data(unmount component 요소)의 상태 관리를 수행한다고 판단해 이와 같은 에러 발생
  - 컴포넌트 unmount 시 useEffect의 return으로 해결

&lt;components/Mapbox.js&gt;

```JavaScript

useEffect(() => {
    return { data };
  }, [data]);

```

# 2. Webpack 환경 구성하기

mapbox를 빌드하기 위해선 Webpack 환경 구성이 필요합니다.
다음 자료를 참고하면서 webpack.config.js를 설정하였습니다.

[관련 자료](https://github.com/visgl/react-map-gl/tree/6.0-release/examples/get-started/hooks)

## 2-1. package 버전

Webpack 설정 시 버전 충돌이 발생하여 구글링을 통해 서로 호환되는 버전을 검색하여 사용하였습니다.
사용한 패키지들의 버전은 다음과 같습니다.

&lt;package.json&gt;

```JSON
  "dependencies":{
    "node-sass": "4.14.1",
  },
  "devDependencies": {
    "@babel/core": "^7.14.0",
    "@babel/preset-react": "^7.13.13",
    "babel-loader": "^8.2.2",
    "css-loader": "^5.2.4",
    "sass-loader": "10.0.5",
    "style-loader": "^2.0.0",
    "webpack": "4.44.2",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.2"
  },

```

## 2-2. Webpack 설정하기

&lt;webpack.config.js&gt;

```JavaScript

const path = require("path");
const webpack = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const dotenv = require("dotenv");

dotenv.config();

module.exports = {
  mode: "development",
  entry: {
    app: "./src/index.js",
  },
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "index_bundler.js",
  },
  devtool: "source-map",
  // fs 에러 해결
  node: { fs: "empty" },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: [/node_modules/],
        use: [
          {
            loader: "babel-loader",
            options: {
              presets: ["@babel/preset-react"],
            },
          },
        ],
      },
      {
        test: /\.s[ac]ss$/i,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
  // Webpack dev-server fail loading 에러 해결
  devServer: {
    historyApiFallback: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
    new webpack.DefinePlugin({
      "process.env": {
        REACT_APP_MAPBOX: JSON.stringify(process.env.REACT_APP_MAPBOX),
        REACT_APP_CHILDCARE_KEY: JSON.stringify(
          process.env.REACT_APP_CHILDCARE_KEY
        ),
      },
    }),
  ],
};

```

각 옵션을 살펴보면 다음과 같습니다.

- mode : 모듈 번들러의 옵션을 설정합니다. (development / production)
- entry : 시작 파일을 설정합니다.
- output : 모듈 번들러 수행 결과 파일을 저장할 위치를 지정합니다.
- devtool : 디버깅 파일을 생성합니다.
- module : 모듈 규칙을 지정합니다. 여기선 JS와 Sass(SCSS) 모듈 규칙을 지정하였습니다.
- plugins : 추가적으로 사용할 플러그인입니다.
  - HtmlWebpackPlugin : 모듈 번들러 결과에 index.html을 생성하는 플러그인입니다.
  - webpack.DefinePlugin : 환경 변수(.env)를 접근하기 위해 사용한 플러그인입니다.

# 3. hover 스타일 구성하기

마우스를 행정구역에 hover시 지역구 단위로 스타일을 적용하는 기능을 추가하였습니다. 또한 툴팁 메세지로 지역구명을 출력하였습니다.
이 기능을 구현하기 위해서 onHover 이벤트 핸들러를 통해 지역구명과 좌표를 가져왔고 동일한 지역구명을 가진 geojson 데이터를
필터링하는 배열을 작성하였습니다. 작성한 배열을 Layer 컴포넌트에 전달하면 자동으로 필터링 스타일이 적용됩니다.

&lt;components/Mapbox.js&gt;

```JavaScript

import React, { useCallback, useEffect, useMemo, useState } from "react";
import ReactMapGL, { Popup, Layer, Source } from "react-map-gl";
import { areaLayer, highlightLayer } from "../utils/mapbox/mapStyle";

const MapboxAccessToken = process.env.REACT_APP_MAPBOX;

function MapBox({ data }) {
  const [viewport, setViewport] = useState({
    width: 1200,
    height: 800,
    latitude: 37.5642135,
    longitude: 127.0016985,
    zoom: 10.5,
  });
  useEffect(() => {
    return { data };
  }, [data]);

  const [hoverInfo, setHoverInfo] = useState(null);

  // hover 이벤트 핸들러
  const onHover = useCallback((e) => {
    const hoverArea = e.features && e.features[0];
    setHoverInfo({
      longitude: e.lngLat[0],
      latitude: e.lngLat[1],
      areaName: hoverArea && hoverArea.properties.sggnm,
    });
  }, []);
  
  // 지역 선택과 필터링
  const selectedArea = (hoverInfo && hoverInfo.areaName) || "";
  const filter = useMemo(() => ["in", "sggnm", selectedArea], [selectedArea]);

  return (
    <ReactMapGL
      {...viewport}
      mapStyle="mapbox://styles/mapbox/streets-v11"
      onViewportChange={(nextViewport) => setViewport(nextViewport)}
      onHover={onHover}
      mapboxApiAccessToken={MapboxAccessToken}
    >
      <Source type="geojson" data={data}>
        <Layer {...areaLayer} /> // 전체에 기본 스타일 적용
        <Layer {...highlightLayer} filter={filter} /> // hover 스타일 적용(필터링)
      </Source>
      {selectedArea && (
        <Popup // 툴팁 메세지
          longitude={hoverInfo.longitude}
          latitude={hoverInfo.latitude}
          closeButton={false}
          className="areaInfo"
        >
          {selectedArea}
        </Popup>
      )}
    </ReactMapGL>
  );
}
export default MapBox;

```

&lt;utils/mapbox/mapStyle.js&gt;

```JavaScript

// 기본 스타일
export const areaLayer = {
  id: "area",
  type: "fill",
  paint: {
    "fill-outline-color": "#d0ff00",
    "fill-color": "#04e40f",
    "fill-opacity": 0.8,
  },
};

// hover 스타일
export const highlightLayer = {
  id: "area-highlighted",
  type: "fill",
  paint: {
    "fill-outline-color": "#d0ff00",
    "fill-color": "#ff0000",
    "fill-opacity": 0.8,
  },
};

```

<참고 자료>

- [react-map-gl 시작하기](https://visgl.github.io/react-map-gl/docs/get-started/get-started)
- [lineStyle 구성하기](https://docs.mapbox.com/mapbox-gl-js/style-spec/layers/)
- [SPA에서 뒤로가기, 새로고침 시 404 Not Found 오류 해결](https://darrengwon.tistory.com/245)
- [react-map-gl Webpack 환경 구성](https://github.com/visgl/react-map-gl/tree/6.0-release/examples/get-started/hooks)
- [react-map-gl hover 스타일 적용](https://github.com/visgl/react-map-gl/tree/6.1-release/examples/filter)
