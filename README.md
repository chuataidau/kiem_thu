# API Testing Report - JSONPlaceholder & Weather API

## Project Information

| Field | Information |
|---|---|
| Project Name | REST API Testing Practice |
| Tester | [Your Name] |
| Testing Tool | Postman |
| API Category | REST API |
| Testing Method | Manual API Testing |
| Testing Date | [Testing Date] |

---

# 1. Objective

Mục tiêu của bài thực hành kiểm thử API:

- Làm quen với quy trình kiểm thử REST API bằng Postman
- Thực hiện kiểm tra các HTTP methods:
  - GET
  - POST
  - PUT
  - DELETE
- Đánh giá khả năng xử lý lỗi của API với dữ liệu không hợp lệ
- Thực hành các tính năng quan trọng trong Postman:
  - Collection
  - Environment
  - Variables
  - Test Scripts
  - Collection Runner

---

# 2. APIs Used

## 2.1 JSONPlaceholder API

### Base URL

```http
https://jsonplaceholder.typicode.com
```

API này được sử dụng để thực hiện kiểm thử CRUD operations.

---

## 2.2 Weather API

### Base URL

```http
http://api.weatherapi.com/v1
```

API được dùng để thực hành gửi request với query parameters.

---

# 3. Environment Configuration

## Environment Variable

| Variable | Assigned Value |
|---|---|
| `baseUrl` | `https://jsonplaceholder.typicode.com` |

---

# 4. Setup Process

## 4.1 Create Collection

Tên collection được tạo trong Postman:

```text
Lab7
```

### Screenshot

![Create Collection](img/taocol.png)

---

## 4.2 Create Environment

Tên Environment được cấu hình:

```text
JSONPlaceholder
```

### Screenshot

![Create Environment](img/taoEnviroment.png)

---

## 4.3 Add Environment Variable

| Variable Name | Value |
|---|---|
| baseUrl | https://jsonplaceholder.typicode.com |

### Screenshot

![Add Variable](img/taoEnviroment.png)

---

## 4.4 Attach Environment

Environment được liên kết với workspace để sử dụng biến động `{{baseUrl}}`.

### Screenshot

![Attach Environment](img/taoreqthemev.png)

---

# 5. Test Cases

| TC ID | Scenario | Method | Endpoint | Expected Outcome | Actual Outcome | Status |
|---|---|---|---|---|---|---|
| TC01 | Retrieve Post | GET | `/posts/1` | 200 OK | 200 OK | PASS |
| TC02 | Add New Post | POST | `/posts` | 201 Created | 201 Created | PASS |
| TC03 | Modify Existing Post | PUT | `/posts/1` | 200 OK | 200 OK | PASS |
| TC04 | Remove Post | DELETE | `/posts/1` | 200 OK | 200 OK | PASS |
| TC05 | Test Huge ID | GET | `/posts/99999999` | API handles invalid input | 404 Not Found | PASS |
| TC06 | Test Negative ID | GET | `/posts/-999` | API handles invalid input | 404 Not Found | PASS |

---

# 6. API Requests

---

# 6.1 GET Request

## Endpoint

```http
GET {{baseUrl}}/posts/1
```

## Test Script

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

## Result

- Returned Status: 200 OK
- Testing Status: PASS

### Screenshot

![GET Test](img/testGET.png)

---

# 6.2 POST Request

## Endpoint

```http
POST {{baseUrl}}/posts
```

## Request Body

```json
{
  "title": "Test Postman",
  "body": "Learning API Testing",
  "userId": 1
}
```

## Test Script

```javascript
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});
```

## Result

- Returned Status: 201 Created
- Testing Status: PASS

### Screenshot

![POST Test](img/TestPOST.png)

---

# 6.3 PUT Request

## Endpoint

```http
PUT {{baseUrl}}/posts/1
```

## Request Body

```json
{
  "id": 1,
  "title": "Updated Title",
  "body": "Updated Content",
  "userId": 1
}
```

## Test Script

```javascript
pm.test("PUT success", function () {
    pm.response.to.have.status(200);
});
```

## Result

- Returned Status: 200 OK
- Testing Status: PASS

### Screenshot

![PUT Test](img/testPUT.png)

---

# 6.4 DELETE Request

## Endpoint

```http
DELETE {{baseUrl}}/posts/1
```

## Test Script

```javascript
pm.test("DELETE success", function () {
    pm.response.to.have.status(200);
});
```

## Result

- Returned Status: 200 OK
- Testing Status: PASS

### Screenshot

![DELETE Test](img/testDELETE.png)

---

# 6.5 GET Huge ID Testing

## Endpoint

```http
GET {{baseUrl}}/posts/99999999
```

## Purpose

Mục đích của bài kiểm thử:

- Đảm bảo API không bị crash
- Kiểm tra khả năng xử lý ID cực lớn
- Đánh giá độ ổn định của response time

## Test Script

```javascript
pm.test("Status code valid", function () {
    pm.expect(
        [200, 404].includes(pm.response.code)
    ).to.be.true;
});

pm.test("Response time under 3s", function () {
    pm.expect(pm.response.responseTime).to.be.below(3000);
});

pm.test("API should not crash", function () {
    pm.expect(pm.response.text()).to.not.include("Internal Server Error");
});
```

## Result

- Returned Status: 404 Not Found
- Testing Status: PASS

### Screenshot

![GET Huge ID](img/testGetIDlon.png)

---

# 6.6 GET Negative ID Testing

## Endpoint

```http
GET {{baseUrl}}/posts/-999
```

## Purpose

Thực hiện kiểm thử với giá trị ID âm để xác minh API xử lý lỗi đúng cách.

## Result

- Returned Status: 404 Not Found
- Testing Status: PASS

### Screenshot

![GET Negative ID](img/testGETIDAM.png)

---

# 7. Weather API Testing

## Endpoint

```http
GET http://api.weatherapi.com/v1/current.json
```

## Query Parameters

| Parameter | Value |
|---|---|
| key | YOUR_API_KEY |
| q | Hanoi |

## Example URL

```http
http://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q=Hanoi
```

## Expected Result

- API trả về dữ liệu thời tiết hiện tại
- Status code trả về là 200 OK

### Screenshot

![Weather API](img/testAPIthoitiet.png)

---

# 8. Full Collection Run

Toàn bộ API requests được thực thi bằng Collection Runner trong Postman.

## Result Summary

| Metric | Value |
|---|---|
| Total Requests | 6 |
| Passed Tests | 6 |
| Failed Tests | 0 |
| Errors | 0 |
| Success Rate | 100% |

---

# 9. Collection Runner Result

### Screenshot

![Collection Runner](img/RunFULLTEST.png)

---

# 10. Performance Summary

| Request Type | Response Time |
|---|---|
| GET | 154 ms |
| POST | 265 ms |
| PUT | 268 ms |
| DELETE | 268 ms |
| Huge ID Test | 50 ms |
| Negative ID Test | 55 ms |

---

# 11. Findings

## Positive Findings

- CRUD APIs hoạt động chính xác
- Response times ổn định trong toàn bộ quá trình test
- Không xuất hiện lỗi Internal Server Error
- Environment variables hoạt động đúng
- Collection Runner chạy thành công
- API xử lý tốt các trường hợp negative testing

---

# 12. Conclusion

Kết quả kiểm thử cho thấy:

- REST APIs hoạt động ổn định với các CRUD operations
- API xử lý tốt dữ liệu đầu vào không hợp lệ
- Không phát hiện lỗi nghiêm trọng hoặc crash hệ thống
- Postman hỗ trợ hiệu quả cho cả manual testing và automation testing
