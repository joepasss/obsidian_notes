### core modules

`http`: Launch a server, send request
`https`: Launch a SSL server

### Launch a server using http core module

``` typescript
import http from "http";

const server = http.createServer((req, res) => {
  console.log(req);
});

server.listen(3000);
```
[[req object]]
### send

`res.setHeader(name: string, value: any): http.ServerResponse`: Header 설정 가능
`res.write()`: response 내용 작성
`res.end()`:  write 가 끝났다는걸 알려줌

``` typescript
import http from "http";

const server = http.createServer((req, res) => {
  console.log(req.url);
  console.log(req.method);
  console.log(req.headers.host);

  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>MY DOCUMENT</title></head>");
  res.write("<body><h1>Hello</h1></body>");
  res.write("</html>");
  res.end();
});

server.listen(3000);
```

### 간단한 routing

`req.url` 에 클라이언트에서 원하는 라우트가 담겨오므로, if문을 이용해서 간단하게 라우팅이 가능하다

```typescript
import http from "http";

const server = http.createServer((req, res) => {
  console.log(req.url);
  console.log(req.method);
  console.log(req.headers.host);

  if (req.url === "/") {
    res.setHeader("Content-Type", "text/html");
    res.write("<html>");
    res.write("<head><title>MY DOCUMENT</title></head>");
    res.write("<body><h1>Hello</h1></body>");
    res.write("</html>");
    return res.end();
  }

  if (req.url?.startsWith("/api")) {
    res.setHeader("Content-Type", "Application/json");
    res.write('{"name": "joe"}');
    return res.end();
  }
});

server.listen(3000);
```
