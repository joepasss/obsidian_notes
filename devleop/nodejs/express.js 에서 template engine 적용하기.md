###  PUSH method를 이용해서 data 추가하는 기능 만들기

[[Express.js]] 에서 만든 앱에서,

``` typescript
// src/routes/admin.ts
import express from "express";
import path from "path";
import { viewsDir } from "../utils/path";

const adminRouter = express.Router();

// hard coded database
interface ProductInterface {
  title: string;
}

// 임시로 데이터를 저장할 변수 생성
const products: ProductInterface[] = [];

adminRouter.get("/add-product", (_req, res, _next) => {
  res.sendFile(path.join(viewsDir, "add-product.html"));
});

adminRouter.post("/add-product", (req, res, _next) => {
  // post request 를 보낼 때, 데이터를 추가
  products.push({ title: req.body.title });
  res.redirect("/");
});

export default adminRouter;
export { products, ProductInterface };
```

``` typescript
// src/routes/shop.ts
import express from "express";
import path from "path";
import { viewsDir } from "../utils/path";
import { products } from "./admin";

const shopRouter = express.Router();

shopRouter.get("/", (_req, res, _next) => {
  // 데이터가 push 되었는지 확인하는 용도 (임시)
  console.log(products);
  res.sendFile(path.join(viewsDir, "shop.html"));
});

export default shopRouter;
```

### template engine 적용하기

`npm install --save ejs pug express-handlebars` 로 패키지 설치 (`ejs`, `pug`, `handlebars` 세 개 다 템플릿 엔진의 일종, 세 가지 모두 설치함)

`index.ts` 파일에서 `pug` 템플릿 엔진 관련 환경설정 해줌
``` typescript
// src/index.ts

import express from "express";
import bodyParser from "body-parser";
import path from "path";

import adminRouter from "./routes/admin";
import shopRouter from "./routes/shop";
import { rootDir, viewsDir } from "./utils/path";

const app = express();

// view engine 설정
app.set("view engine", "pug");
// views (.pug 파일이 들어있는 폴더) 폴더 지정 (default값이 views)
app.set("views", "views");

app.use(bodyParser.urlencoded());
app.use(express.static(path.join(rootDir, "public")));

// routes
app.use("/admin", adminRouter);
app.use(shopRouter);

// 404 error
app.use((_req, res, _next) => {
  res.status(404).sendFile(path.resolve(viewsDir, "not-found.html"));
});

app.listen(3000);
```

shop.html 을 shop.pug로 변환
``` pug
// views/shop.pug
html(lang="en")
  head 
    meta(charset="UTF-8")
    meta(name="viewport", content="width=device-width, initial-scale=1.0")
    title SHOP
    link(rel="stylesheet" href="/css/main.css")
  body
    header 
      nav 
        ul 
          li 
            a(href="/") Shop
          li 
            a(href="/admin/add-product") Add Products
    main 
      h1 My Products 
```

html 페이지를 서빙하고 있던 `shop.ts` 파일에서 `pug` 파일을 서빙하도록 변경
``` typescript
import express from "express";
import { products } from "./admin";

const shopRouter = express.Router();

shopRouter.get("/", (_req, res, _next) => {
  console.log(products);
  res.render("shop");
});

export default shopRouter;
```

동적 컨텐츠 출력하기
``` typescript
// src/routes/shop.ts
import express from "express";
import { products } from "./admin";

const shopRouter = express.Router();

shopRouter.get("/", (_req, res, _next) => {
  // res.render 함수를 이용해서 products 를 전달
  res.render("shop", { prods: products });
});

export default shopRouter;
```

``` pug
// views/shop.pug
html(lang="en")
  head 
    meta(charset="UTF-8")
    meta(name="viewport", content="width=device-width, initial-scale=1.0")
    title SHOP
    link(rel="stylesheet" href="/css/main.css")
  body
    header 
      nav 
        ul 
          li 
            a(href="/") Shop
          li 
            a(href="/admin/add-product") Add Products
    main 
      h1 My Products 
      // each 함수로 products 를 순회하면서, title 값을 출력
      each product in prods 
        p #{product.title}
```

`pug`에서 `if statement` 사용하기
``` pug
html(lang="en")
  head 
    meta(charset="UTF-8")
    meta(name="viewport", content="width=device-width, initial-scale=1.0")
    title SHOP
    link(rel="stylesheet" href="/css/main.css")
  body
    header 
      nav 
        ul 
          li 
            a(href="/") Shop
          li 
            a(href="/admin/add-product") Add Products
    main 
      if prods.length > 0
        h1 My Products
        each product in prods 
          p #{product.title}
      else
        h1 No Products ...
```

