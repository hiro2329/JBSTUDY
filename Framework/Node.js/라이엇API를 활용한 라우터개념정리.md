# Node.js Express 라우터와 Riot API 연동 예제 정리

## 1. 라우터(Router)란?

라우터는 사용자의 요청 URL(경로)에 따라
서버가 어떤 동작을 할지 정해주는 역할을 합니다.

예시:

- / : 메인 페이지
- /search : 검색 결과 페이지

Express에서는 아래와 같이 라우터를 정의합니다.

```javascript
app.get("/", (req, res) => {
  res.render("index");
});

app.get("/search", async (req, res) => {
  // 검색 처리
});
```

---

## 2. 코드 예시

```javascript
const axios = require("axios");
const express = require("express");
const app = express();
const path = require("path");
const API = ""; // Riot Games API 키 입력

app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");

// Bootstrap CSS 제공
app.use(
  "/bootstrap",
  express.static(path.join(__dirname, "node_modules/bootstrap/dist"))
);

// 메인 페이지 라우터
app.get("/", (req, res) => {
  res.render("index");
});

// 검색 라우터
app.get("/search", async (req, res) => {
  console.log("search 라우트 진입");
  console.log(req.query); // { gameName: 'hideonbush', tagLine: 'KR1' }
  const { gameName, tagLine } = req.query;
  try {
    // 1. 소환사 계정 정보(puuid) 조회
    const accountRes = await axios.get(
      `https://asia.api.riotgames.com/riot/account/v1/accounts/by-riot-id/${encodeURIComponent(
        gameName
      )}/${encodeURIComponent(tagLine)}`,
      { headers: { "X-Riot-Token": API } }
    );
    const puuid = accountRes.data.puuid;
    if (!puuid) {
      return res.send("소환사 정보를 찾을 수 없습니다.");
    }

    // 2. puuid로 소환사 상세 정보 조회
    const summonerRes = await axios.get(
      `https://kr.api.riotgames.com/lol/summoner/v4/summoners/by-puuid/${puuid}`,
      { headers: { "X-Riot-Token": API } }
    );
    const data = summonerRes.data;

    // 3. 결과를 userinfo.ejs에 전달
    res.render("userinfo", { user: data, gameName, tagLine });
  } catch (error) {
    if (error.response) {
      res.send(
        `에러 상태: ${error.response.status}<br>${JSON.stringify(
          error.response.data
        )}`
      );
    } else {
      res.send(`에러: ${error.message}`);
    }
  }
});

app.listen(3000, () => {
  console.log("서버 실행 중");
});
```

(2) index.ejs (검색 폼)

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>롤 소환사 검색</title>
    <link rel="stylesheet" href="/bootstrap/css/bootstrap.min.css" />
  </head>
  <body>
    <div class="container mt-5">
      <form action="/search" method="GET" class="d-flex" role="search">
        <input
          class="form-control me-2"
          type="search"
          name="gameName"
          placeholder="소환사명"
          required
        />
        <input
          class="form-control me-2"
          type="search"
          name="tagLine"
          placeholder="태그 (예: KR1)"
          required
        />
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </body>
</html>
```

(3) userinfo.ejs (검색 결과)

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>소환사 정보</title>
    <link rel="stylesheet" href="/bootstrap/css/bootstrap.min.css" />
  </head>
  <body>
    <div class="container mt-5">
      <h2><%= gameName %>#<%= tagLine %></h2>
      <p>소환사 레벨: <%= user.summonerLevel %></p>
      <p>프로필 아이콘 번호: <%= user.profileIconId %></p>
      <img
        src="https://ddragon.leagueoflegends.com/cdn/15.11.1/img/profileicon/<%= user.profileIconId %>.png"
        alt="프로필 아이콘"
      />
      <br />
      <a href="/" class="btn btn-primary mt-3">다시 검색</a>
    </div>
  </body>
</html>
```

## 3. 전체 흐름

1. 사용자가 `/search` 경로로 요청을 보냅니다.
2. 서버는
   `gameName`과 `tagLine`을 쿼리 파라미터로 받아옵니다.
3. Riot API를 호출하여 해당
   소환사의 `puuid`를 조회합니다.
4. `puuid`를 사용하여 소환사의 상세 정보를
   조회합니다.
5. 조회된 정보를 `userinfo.ejs` 템플릿에 전달하여 렌더링합니다.

---

## 4. 정리

- 라우터는 URL 경로별로 서버 동작을 분리하는 핵심 개념
- Express에서는 app.get("/경로", 콜백) 형태로 사용
- EJS와 함께 쓰면, 서버에서 받은 데이터를 템플릿에 쉽게 전달 가능
