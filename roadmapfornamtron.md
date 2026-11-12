Đây là roadmap để cho nam tròn học và làm được những thứ mình muốn với nghành công nghệ thông tin
# 🗺️ Roadmap Full-stack: Java Spring Boot + React/Next.js

> **Mục tiêu cuối cùng:** Tự xây dựng hoàn chỉnh, từ con số 0 đến lúc chạy thật trên Internet, một website thương mại như:
> - 🛒 **Shop bán điện thoại / đồ công nghệ** (kiểu CellphoneS, Thế Giới Di Động)
> - 🎓 **Nền tảng bán khóa học online** (kiểu Udemy)
> - …và sau đó là bất kỳ web nào khác (đặt phòng, đặt đồ ăn, mạng xã hội nhỏ, quản lý nội bộ…)
>
> **Stack chính:** Java · Spring Boot · PostgreSQL/MySQL · Spring Security · TypeScript · React · Next.js · Docker · Git
>
> **Thời gian dự kiến:** 9–12 tháng nếu học 15–20 giờ/tuần (người mới hoàn toàn). Có nền lập trình rồi thì có thể rút còn 5–7 tháng.

---

## 📑 Mục lục

0. [Bức tranh tổng thể](#muc-0)
1. [Phương pháp học chuẩn trong thời đại AI](#muc-1)
2. [Giai đoạn 0 – Nền móng & công cụ](#muc-2)
3. [Giai đoạn 1 – HTML, CSS, JavaScript](#muc-3)
4. [Giai đoạn 2 – Java Core](#muc-4)
5. [Giai đoạn 3 – Database & SQL](#muc-5)
6. [Giai đoạn 4 – Spring Boot](#muc-6)
7. [Giai đoạn 5 – Security](#muc-7)
8. [Giai đoạn 6 – TypeScript](#muc-8)
9. [Giai đoạn 7 – React](#muc-9)
10. [Giai đoạn 8 – Next.js](#muc-10)
11. [Giai đoạn 9 – Tính năng nâng cao của web thật](#muc-11)
12. [Giai đoạn 10 – DevOps & Deploy](#muc-12)
13. [Dự án lớn 1 – Shop điện thoại](#muc-13)
14. [Dự án lớn 2 – Nền tảng khóa học](#muc-14)
15. [Sau roadmap: học gì tiếp](#muc-15)
16. [Lịch học mẫu theo tuần](#muc-16)
17. [Checklist tự đánh giá & đi xin việc](#muc-17)
18. [Tài nguyên học](#muc-18)

---

<a id="muc-0"></a>

## 0. Bức tranh tổng thể

### 0.1. Một website thương mại gồm những gì?

```
┌──────────────────────────┐        HTTPS / JSON        ┌───────────────────────────────┐
│        FRONTEND          │  ───────────────────────▶  │            BACKEND            │
│  Next.js (React + TS)    │                            │   Spring Boot (Java) – REST   │
│  - Trang chủ, danh mục   │  ◀───────────────────────  │   - Controller (API)          │
│  - Chi tiết sản phẩm     │                            │   - Service (nghiệp vụ)       │
│  - Giỏ hàng, thanh toán  │                            │   - Repository (truy cập DB)  │
│  - Trang admin           │                            │   - Security (JWT, phân quyền)│
└──────────────────────────┘                            └──────────────┬────────────────┘
                                                                        │
              ┌───────────────────────┬────────────────────┬────────────┼──────────────┐
              ▼                       ▼                    ▼            ▼              ▼
        PostgreSQL/MySQL           Redis              Lưu file      Email/SMS     Cổng thanh toán
        (dữ liệu chính)          (cache, session)    (ảnh, video)   (gửi OTP,     (VNPay, MoMo,
                                                     S3/Cloudinary   hóa đơn)       Stripe)
```

### 0.2. "Nguyên khối" (Monolith) nghĩa là gì trong roadmap này?

- **Backend là một khối duy nhất** (một project Spring Boot), chia **module theo nghiệp vụ** bên trong: `user`, `product`, `order`, `payment`, `course`… → gọi là **Modular Monolith**.
- **Frontend tách riêng** (Next.js) và gọi backend qua REST API.
- Đây là cách **đúng đắn nhất cho người mới và cho 90% công ty nhỏ/vừa**. Đừng vội học Microservices — chỉ học khi đã làm chắc monolith (xem mục 15).

### 0.3. Bản đồ kỹ năng (đi theo thứ tự)

```
Nền móng (Git, terminal, HTTP)
   └─▶ HTML/CSS/JS
          └─▶ Java Core ──▶ SQL/Database ──▶ Spring Boot ──▶ Spring Security
                                                                  │
          TypeScript ──▶ React ──▶ Next.js ◀──────────────────────┘ (ghép FE + BE)
                                       │
                                       ▼
                     Tính năng nâng cao (thanh toán, upload, cache, search…)
                                       │
                                       ▼
                         Docker ─▶ CI/CD ─▶ Deploy lên Internet
                                       │
                                       ▼
                  🛒 Dự án Shop điện thoại  +  🎓 Dự án Nền tảng khóa học
```

> 💡 **Mẹo:** Có thể học **song song** 2 nhánh: buổi sáng Java/Spring, buổi tối JS/React — miễn là không học quá 2 thứ mới cùng lúc.

---

<a id="muc-1"></a>

## 1. Phương pháp học chuẩn trong thời đại AI

AI (ChatGPT, Claude, Copilot, Cursor, Claude Code…) có thể viết code nhanh hơn bạn. Vì vậy **giá trị của bạn không còn là "gõ được code"**, mà là:

1. **Hiểu hệ thống** – biết code chạy thế nào, vì sao chạy, khi nào sẽ hỏng.
2. **Thiết kế** – chia module, thiết kế database, API, luồng nghiệp vụ.
3. **Đánh giá & sửa** – đọc code (của người hoặc của AI), phát hiện bug, lỗ hổng bảo mật, chỗ chậm.
4. **Ra quyết định** – chọn công nghệ, đánh đổi giữa nhanh/đúng/rẻ.
5. **Giao tiếp yêu cầu** – mô tả chính xác điều cần làm (cho người hoặc cho AI).

### 1.1. Quy tắc vàng khi học với AI

| Quy tắc | Giải thích |
|---|---|
| ✋ **Tự làm trước, hỏi AI sau** | Mỗi bài tập: tự nghĩ & tự code ít nhất 20–30 phút. Bí rồi mới hỏi. |
| 🧠 **Hỏi để hiểu, không hỏi để chép** | Hỏi "tại sao", "khác gì", "khi nào dùng" thay vì "viết cho tôi". |
| 🚫 **Không dán code mình không giải thích được** | Mỗi dòng code AI viết, bạn phải giải thích lại được bằng lời. |
| 🔍 **Luôn kiểm chứng** | AI có thể sai, bịa API, dùng phiên bản cũ. Đối chiếu với **tài liệu chính thức**. |
| 🧪 **Code phải có test** | Dùng AI để sinh test case, nhưng bạn tự quyết định test gì. |
| 📝 **Viết lại bằng lời của mình** | Sau mỗi chủ đề, viết 5–10 dòng ghi chú như đang dạy lại người khác. |

### 1.2. Ba chế độ dùng AI theo giai đoạn

| Giai đoạn | Chế độ | Cho phép AI làm gì |
|---|---|---|
| **Học nền tảng** (Giai đoạn 0–3, 6) | 🧑‍🏫 **AI = Gia sư** | Giải thích khái niệm, ra bài tập, chấm bài, giải thích lỗi. **Không** cho AI viết lời giải. |
| **Học framework** (Giai đoạn 4, 5, 7, 8) | 🤝 **AI = Bạn code cùng** | Gợi ý, review code bạn viết, so sánh cách làm. Code chính vẫn do bạn viết. |
| **Làm dự án lớn** (Giai đoạn 9+) | 👷 **AI = Đồng nghiệp junior** | Cho AI sinh code lặp lại (DTO, test, boilerplate, CRUD), bạn **thiết kế, review và chịu trách nhiệm**. |

### 1.3. Bộ prompt mẫu dùng khi học

**Hiểu khái niệm:**
```
Giải thích [Dependency Injection trong Spring] cho người mới.
1) Định nghĩa 2 câu. 2) Ví dụ đời thường. 3) Ví dụ code nhỏ nhất.
4) Nếu không có nó thì code sẽ ra sao? 5) 3 câu hỏi để tôi tự kiểm tra hiểu chưa.
```

**Ra bài tập (không cho đáp án):**
```
Cho tôi 5 bài tập về [Java Stream API] từ dễ đến khó, bối cảnh là danh sách sản phẩm điện thoại.
CHƯA đưa lời giải. Tôi sẽ gửi bài làm để bạn chấm.
```

**Review code:**
```
Đây là code của tôi: [dán code].
Hãy review như senior: lỗi logic, lỗi bảo mật, hiệu năng, đặt tên, cấu trúc.
Đừng viết lại toàn bộ — chỉ ra vấn đề và gợi ý hướng sửa để tôi tự sửa.
```

**Debug:**
```
Tôi gặp lỗi: [dán stacktrace]. Code liên quan: [dán].
Tôi đoán nguyên nhân là [giả thuyết của bạn]. Tôi đúng hay sai? Hãy dẫn tôi tìm ra nguyên nhân bằng câu hỏi gợi mở.
```

**Thiết kế:**
```
Tôi đang thiết kế database cho chức năng [giỏ hàng + đơn hàng] của web bán điện thoại.
Đây là ERD của tôi: [mô tả]. Chỉ ra điểm yếu, trường hợp chưa xử lý (ví dụ: giá thay đổi sau khi đặt, hết hàng…).
```

**Phỏng vấn thử:**
```
Đóng vai người phỏng vấn Java Spring Boot Junior. Hỏi tôi từng câu một về [JPA & Transaction],
chờ tôi trả lời rồi chấm điểm và giải thích.
```

### 1.4. Vòng lặp học cho MỌI chủ đề (Learn → Build → Explain → Review)

```
1. LEARN   (30%)  Đọc tài liệu chính thức / xem 1 video ngắn. Ghi chú ý chính.
2. BUILD   (50%)  Tự code một bài nhỏ áp dụng ngay. Không copy.
3. EXPLAIN (10%)  Viết lại bằng lời mình (note, blog, hoặc giải thích cho AI và nhờ AI bắt lỗi).
4. REVIEW  (10%)  Nhờ AI review + tự ôn lại sau 1 ngày, 1 tuần, 1 tháng (spaced repetition).
```

### 1.5. Thói quen bắt buộc

- 📒 **Nhật ký học (learning log):** mỗi ngày 3 dòng: hôm nay học gì / vướng gì / mai làm gì. Lưu trong repo GitHub.
- 🟩 **Commit mỗi ngày lên GitHub** — vừa luyện Git, vừa làm portfolio.
- 🃏 **Flashcard (Anki):** các khái niệm hay quên (HTTP status code, annotation Spring, hook React…).
- 🧩 **Mỗi giai đoạn kết thúc bằng 1 mini project** — không có project = chưa xong giai đoạn.
- 📖 **Đọc code người khác:** mỗi tuần đọc 1 repo mẫu trên GitHub (ví dụ Spring PetClinic).
- 🛑 **Quy tắc 2 lần:** cùng một lỗi gặp lần 2 → ghi vào file `bugs-i-met.md` kèm cách sửa.

### 1.6. Công cụ AI nên dùng

| Công cụ | Dùng để |
|---|---|
| Claude / ChatGPT (chat) | Gia sư, giải thích, review, thiết kế, phỏng vấn thử |
| GitHub Copilot / Cursor / Claude Code | Gợi ý code trong IDE, sinh boilerplate, refactor, viết test |
| NotebookLM hoặc tương tự | Tóm tắt tài liệu dài, hỏi đáp trên tài liệu chính thức |
| AI review PR (Copilot review, CodeRabbit…) | Tập thói quen review code như trong công ty |

> ⚠️ **Giai đoạn 0–3: tắt gợi ý code tự động trong IDE.** Bật lại từ giai đoạn 4. Lý do: tay và não phải quen với cú pháp trước.

---

<a id="muc-2"></a>

## 2. Giai đoạn 0 – Nền móng & công cụ (2–3 tuần)

### 2.1. Máy tính & hệ điều hành
- [ ] Cấu trúc thư mục, đường dẫn tuyệt đối / tương đối
- [ ] Biến môi trường (`PATH`, `JAVA_HOME`)
- [ ] Cài đặt phần mềm, quản lý phiên bản (SDKMAN cho Java, nvm/fnm cho Node)

### 2.2. Terminal / Command line
- [ ] Lệnh cơ bản: `cd`, `ls`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `grep`, `find`
- [ ] Pipe `|`, redirect `>`, `>>`
- [ ] Biết dùng ít nhất 1 shell: Bash (Git Bash trên Windows) hoặc PowerShell
- [ ] (Khuyên dùng) Cài **WSL2** trên Windows để quen môi trường Linux — server thật chạy Linux

### 2.3. Git & GitHub (cực kỳ quan trọng)
- [ ] `init`, `clone`, `add`, `commit`, `status`, `log`, `diff`
- [ ] Branch: `branch`, `switch`, `merge`, `rebase` (hiểu khác nhau)
- [ ] Remote: `push`, `pull`, `fetch`
- [ ] Xử lý **merge conflict**
- [ ] `.gitignore`, không bao giờ commit file `.env`, mật khẩu
- [ ] Viết commit message chuẩn (Conventional Commits: `feat:`, `fix:`, `refactor:`…)
- [ ] GitHub: repo, Pull Request, Issue, README, GitHub Projects (Kanban)
- [ ] Quy trình **Git Flow đơn giản**: `main` + `feature/*` + PR

### 2.4. Web hoạt động thế nào
- [ ] Client – Server, Request – Response
- [ ] URL gồm những phần nào (scheme, domain, port, path, query, fragment)
- [ ] DNS, IP, Domain, Hosting
- [ ] **HTTP**: method (GET, POST, PUT, PATCH, DELETE), header, body, status code (2xx, 3xx, 4xx, 5xx)
- [ ] HTTPS, TLS là gì (mức khái niệm)
- [ ] Cookie, Session, Local Storage
- [ ] JSON là gì
- [ ] REST API là gì
- [ ] Dùng **Postman / Bruno / Insomnia** để gọi thử API công khai

### 2.5. Công cụ làm việc
- [ ] **IntelliJ IDEA** (Community hoặc Ultimate – sinh viên được miễn phí) cho Java
- [ ] **VS Code** cho frontend
- [ ] Phím tắt cơ bản của IDE, debug bằng breakpoint
- [ ] DevTools của trình duyệt: Elements, Console, Network, Application

### 2.6. Tư duy lập trình
- [ ] Biến, kiểu dữ liệu, điều kiện, vòng lặp, hàm (học trong ngôn ngữ đầu tiên)
- [ ] Chia nhỏ vấn đề, viết pseudo-code trước khi code
- [ ] Giải 20–30 bài dễ trên LeetCode/HackerRank/Codeforces (mức Easy)

✅ **Mini project:** Tạo repo `learning-journey` trên GitHub, viết README giới thiệu bản thân + kế hoạch học, commit nhật ký học mỗi ngày.

---

<a id="muc-3"></a>

## 3. Giai đoạn 1 – HTML, CSS, JavaScript (5–6 tuần)

### 3.1. HTML (1 tuần)
- [ ] Cấu trúc tài liệu, `head`, `body`, meta tags
- [ ] Thẻ ngữ nghĩa: `header`, `nav`, `main`, `section`, `article`, `footer`
- [ ] Text, link, ảnh, list, table
- [ ] **Form**: `input` các loại, `select`, `textarea`, `label`, validation có sẵn
- [ ] Accessibility cơ bản: `alt`, `label`, thứ tự heading
- [ ] SEO cơ bản: `title`, `meta description`, Open Graph

### 3.2. CSS (2 tuần)
- [ ] Selector, specificity, cascade, kế thừa
- [ ] Box model, `margin`, `padding`, `border`, `box-sizing`
- [ ] Đơn vị: `px`, `rem`, `em`, `%`, `vh/vw`
- [ ] **Flexbox** (bắt buộc thành thạo)
- [ ] **Grid** (bắt buộc thành thạo)
- [ ] Position: `relative`, `absolute`, `fixed`, `sticky`
- [ ] **Responsive**: media query, mobile-first
- [ ] Biến CSS (custom properties), dark mode
- [ ] Transition, animation
- [ ] Sau khi vững CSS thuần → học **Tailwind CSS** (sẽ dùng chính trong dự án)

### 3.3. JavaScript (3 tuần)
**Cơ bản**
- [ ] `let`, `const`, kiểu dữ liệu, toán tử, so sánh `==` vs `===`
- [ ] Hàm thường, arrow function, tham số mặc định, rest/spread
- [ ] Object, Array và các method: `map`, `filter`, `reduce`, `find`, `some`, `every`, `sort`
- [ ] Destructuring, template string, optional chaining `?.`, nullish `??`
- [ ] Scope, hoisting, **closure**, `this`

**DOM & sự kiện**
- [ ] Chọn phần tử, thay đổi nội dung/style/class
- [ ] Event listener, event bubbling, event delegation
- [ ] Form handling, validation

**Bất đồng bộ (rất quan trọng)**
- [ ] Event loop, call stack, callback queue (mức khái niệm)
- [ ] Callback → Promise → `async/await`
- [ ] `fetch` API, xử lý lỗi với `try/catch`
- [ ] Gọi API thật (ví dụ: dummyjson.com, fakestoreapi.com)

**Hiện đại**
- [ ] ES Modules: `import` / `export`
- [ ] `npm`, `package.json`, `node_modules`, script
- [ ] Công cụ build: **Vite**
- [ ] `localStorage`, `sessionStorage`

✅ **Mini project:** Trang **danh sách điện thoại** dùng HTML/CSS/JS thuần:
- Lấy dữ liệu từ API giả (dummyjson) → hiển thị dạng lưới card
- Tìm kiếm, lọc theo hãng, sắp xếp theo giá
- Giỏ hàng lưu trong `localStorage`
- Responsive cho điện thoại

---

<a id="muc-4"></a>

## 4. Giai đoạn 2 – Java Core (6–8 tuần)

> Dùng **Java LTS mới nhất** (Java 21 hoặc 25). Tránh tài liệu dạy Java 8 thuần.

### 4.1. Cú pháp & nền tảng (1–2 tuần)
- [ ] JDK, JRE, JVM; biên dịch & chạy chương trình
- [ ] Kiểu nguyên thủy vs kiểu tham chiếu, wrapper class, autoboxing
- [ ] `String`, `StringBuilder`, tính bất biến (immutability)
- [ ] Điều kiện, vòng lặp, `switch` mới (switch expression)
- [ ] Mảng, phương thức, overloading
- [ ] `var`, text block, record, pattern matching (`instanceof`, `switch`)

### 4.2. Lập trình hướng đối tượng – OOP (2 tuần) ⭐
- [ ] Class, object, constructor, `this`, `static`
- [ ] **4 tính chất**: Đóng gói, Kế thừa, Đa hình, Trừu tượng
- [ ] `interface` vs `abstract class`
- [ ] `equals()` & `hashCode()`, `toString()`
- [ ] Access modifier: `private`, `protected`, `public`, package-private
- [ ] `final`, immutable object
- [ ] Composition vs Inheritance (ưu tiên composition)
- [ ] `enum` (rất hay dùng: `OrderStatus`, `Role`…)
- [ ] **Nguyên lý SOLID** (mức hiểu + ví dụ)

### 4.3. Collections & Generics (1 tuần)
- [ ] `List` (`ArrayList`, `LinkedList`), `Set` (`HashSet`, `TreeSet`), `Map` (`HashMap`, `TreeMap`, `LinkedHashMap`)
- [ ] Khi nào dùng loại nào, độ phức tạp cơ bản (Big-O)
- [ ] `Comparable` vs `Comparator`
- [ ] Generics: `<T>`, bounded type `<T extends …>`, wildcard `?`
- [ ] `Optional`

### 4.4. Functional & Stream (1 tuần)
- [ ] Lambda, method reference
- [ ] Functional interface: `Function`, `Predicate`, `Consumer`, `Supplier`
- [ ] **Stream API**: `filter`, `map`, `flatMap`, `sorted`, `collect`, `groupingBy`, `reduce`
- [ ] Bài tập: thống kê doanh thu theo hãng, top 5 sản phẩm bán chạy…

### 4.5. Exception, I/O, Date (0.5 tuần)
- [ ] Checked vs unchecked exception, `try/catch/finally`, try-with-resources
- [ ] Tạo custom exception (ví dụ `ProductNotFoundException`)
- [ ] Đọc/ghi file cơ bản
- [ ] `java.time`: `LocalDate`, `LocalDateTime`, `Instant`, `ZonedDateTime` (quan trọng cho đơn hàng)
- [ ] `BigDecimal` cho **tiền** (KHÔNG dùng `double` cho tiền!)

### 4.6. Đa luồng – Concurrency (0.5–1 tuần, mức cơ bản)
- [ ] Thread, Runnable, ExecutorService
- [ ] Race condition, `synchronized`, `volatile` (khái niệm)
- [ ] `CompletableFuture`
- [ ] **Virtual threads** (Java 21+)
- [ ] Hiểu vì sao "2 người cùng mua chiếc điện thoại cuối cùng" là bài toán đồng thời

### 4.7. Công cụ build & test (0.5 tuần)
- [ ] **Maven** (hoặc Gradle): `pom.xml`, dependency, lifecycle (`compile`, `test`, `package`)
- [ ] **JUnit 5**: `@Test`, assertion, `@BeforeEach`, parameterized test
- [ ] **AssertJ** cho assertion dễ đọc
- [ ] Clean code: đặt tên, hàm ngắn, không lặp code (DRY)

✅ **Mini project:** **Ứng dụng console quản lý cửa hàng điện thoại**
- Quản lý sản phẩm (CRUD), khách hàng, đơn hàng trong bộ nhớ (`List`, `Map`)
- Tính tổng tiền bằng `BigDecimal`, áp mã giảm giá
- Thống kê bằng Stream (doanh thu theo tháng, theo hãng)
- Lưu/đọc dữ liệu ra file JSON (dùng thư viện Jackson)
- Có ít nhất 15 unit test

---

<a id="muc-5"></a>

## 5. Giai đoạn 3 – Database & SQL (3–4 tuần)

> Chọn **PostgreSQL** (khuyên dùng) hoặc MySQL. Kiến thức dùng chung 90%.

### 5.1. SQL cơ bản (1 tuần)
- [ ] Cài PostgreSQL bằng Docker (hoặc cài trực tiếp) + công cụ GUI: DBeaver / pgAdmin / DataGrip
- [ ] `CREATE TABLE`, kiểu dữ liệu (`INT`, `BIGINT`, `VARCHAR`, `TEXT`, `NUMERIC`, `BOOLEAN`, `TIMESTAMP`, `JSONB`)
- [ ] `INSERT`, `SELECT`, `UPDATE`, `DELETE`
- [ ] `WHERE`, `ORDER BY`, `LIMIT/OFFSET`, `LIKE`, `IN`, `BETWEEN`
- [ ] Hàm gộp: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, `HAVING`

### 5.2. SQL nâng cao (1 tuần)
- [ ] **JOIN**: `INNER`, `LEFT`, `RIGHT`, `FULL` — vẽ sơ đồ để hiểu
- [ ] Subquery, `EXISTS`
- [ ] CTE (`WITH`), Window function (`ROW_NUMBER`, `RANK`, `SUM OVER`) – dùng cho báo cáo
- [ ] View
- [ ] `UPSERT` (`ON CONFLICT`)

### 5.3. Thiết kế database (1 tuần) ⭐
- [ ] Khóa chính (PK), khóa ngoại (FK), `UNIQUE`, `NOT NULL`, `CHECK`
- [ ] Quan hệ 1-1, 1-n, n-n (bảng trung gian)
- [ ] **Chuẩn hóa** 1NF, 2NF, 3NF — và khi nào cố tình phi chuẩn hóa
- [ ] Vẽ **ERD** (dbdiagram.io, drawio)
- [ ] Soft delete (`deleted_at`) vs hard delete
- [ ] Cột audit: `created_at`, `updated_at`, `created_by`
- [ ] Lưu **snapshot giá** trong `order_items` (giá lúc mua ≠ giá hiện tại)

### 5.4. Hiệu năng & toàn vẹn (1 tuần)
- [ ] **Index**: B-tree, composite index, khi nào index giúp/khi nào hại
- [ ] `EXPLAIN` / `EXPLAIN ANALYZE` để đọc query plan
- [ ] **Transaction & ACID**
- [ ] Isolation level: Read Committed, Repeatable Read, Serializable; dirty read, phantom read
- [ ] Locking: `SELECT … FOR UPDATE`, optimistic locking (cột `version`)
- [ ] Bài toán **trừ tồn kho** khi nhiều người mua cùng lúc
- [ ] Backup & restore (`pg_dump`)

### 5.5. Biết thêm (đọc lướt)
- [ ] NoSQL là gì: MongoDB (document), Redis (key-value) — khi nào dùng
- [ ] Full-text search trong PostgreSQL

✅ **Mini project:** Thiết kế ERD + viết script SQL cho **shop điện thoại**:
- Bảng: `users`, `roles`, `categories`, `brands`, `products`, `product_variants` (màu, dung lượng), `product_images`, `inventories`, `carts`, `cart_items`, `orders`, `order_items`, `payments`, `reviews`, `coupons`
- Viết 20 câu query nghiệp vụ: top sản phẩm bán chạy, doanh thu theo ngày, khách hàng mua nhiều nhất, sản phẩm sắp hết hàng…

---

<a id="muc-6"></a>

## 6. Giai đoạn 4 – Spring Boot (8–10 tuần)

> Dùng **Spring Boot bản ổn định mới nhất** (3.x hoặc 4.x) + Java 21+. Tạo project tại **start.spring.io**.

### 6.1. Nền tảng Spring (1 tuần) ⭐
- [ ] Spring vs Spring Boot khác gì; auto-configuration, starter
- [ ] **IoC Container & Dependency Injection** (constructor injection)
- [ ] Bean, `@Component`, `@Service`, `@Repository`, `@Controller`, `@Configuration`, `@Bean`
- [ ] Bean scope, lifecycle
- [ ] `application.yml` / `application.properties`, `@Value`, `@ConfigurationProperties`
- [ ] **Profiles**: `dev`, `test`, `prod`
- [ ] Logging với SLF4J + Logback; log level
- [ ] Spring Boot DevTools, Lombok (biết dùng nhưng hiểu nó sinh ra gì)

### 6.2. Xây REST API (2 tuần) ⭐
- [ ] Kiến trúc phân lớp: **Controller → Service → Repository**
- [ ] `@RestController`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@PatchMapping`, `@DeleteMapping`
- [ ] `@PathVariable`, `@RequestParam`, `@RequestBody`, `ResponseEntity`
- [ ] **Thiết kế REST chuẩn**: đặt tên resource số nhiều, status code đúng, versioning `/api/v1`
- [ ] **DTO** (request/response) — không trả Entity trực tiếp ra ngoài
- [ ] Mapping DTO ↔ Entity: thủ công hoặc **MapStruct**
- [ ] **Validation**: `@Valid`, `@NotBlank`, `@Size`, `@Email`, `@Min`, custom validator
- [ ] **Xử lý lỗi tập trung**: `@RestControllerAdvice`, `@ExceptionHandler`, format lỗi thống nhất (ProblemDetail – RFC 9457)
- [ ] Phân trang, sắp xếp, lọc: `Pageable`, `Page<T>`
- [ ] Tài liệu API tự động: **springdoc-openapi (Swagger UI)**

### 6.3. Spring Data JPA & Hibernate (2–3 tuần) ⭐⭐
- [ ] ORM là gì, JPA vs Hibernate vs Spring Data JPA
- [ ] `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@Column`
- [ ] Quan hệ: `@OneToMany`, `@ManyToOne`, `@ManyToMany`, `@OneToOne`; owning side, `mappedBy`
- [ ] `cascade`, `orphanRemoval`
- [ ] **Fetch type**: LAZY vs EAGER (mặc định nên LAZY)
- [ ] ⚠️ **Vấn đề N+1** và cách sửa: `JOIN FETCH`, `@EntityGraph`, DTO projection
- [ ] `JpaRepository`, query method (`findByBrandAndPriceLessThan`), `@Query` (JPQL & native)
- [ ] Specification / Querydsl / Criteria cho **bộ lọc động** (lọc điện thoại theo hãng, giá, RAM…)
- [ ] Entity lifecycle: transient, managed, detached; persistence context; dirty checking
- [ ] **`@Transactional`**: propagation, rollback, readOnly, vì sao gọi nội bộ trong cùng class không ăn transaction
- [ ] Optimistic locking `@Version`, pessimistic lock `@Lock`
- [ ] Auditing: `@CreatedDate`, `@LastModifiedDate`
- [ ] **Migration database**: **Flyway** (hoặc Liquibase) — KHÔNG dùng `ddl-auto=update` ở production
- [ ] (Biết thêm) JdbcTemplate / jOOQ cho query phức tạp, báo cáo

### 6.4. Kiểm thử backend (1 tuần) ⭐
- [ ] Kim tự tháp test: unit → integration → e2e
- [ ] Unit test Service với **Mockito** (`@Mock`, `@InjectMocks`, `when/thenReturn`, `verify`)
- [ ] Test Controller: `@WebMvcTest` + `MockMvc`
- [ ] Test Repository: `@DataJpaTest`
- [ ] Integration test thật với **Testcontainers** (PostgreSQL thật trong Docker)
- [ ] Đo độ phủ test: JaCoCo (mục tiêu ≥ 70% cho service)

### 6.5. Những thứ đi kèm (1–2 tuần)
- [ ] Upload file: `MultipartFile`, giới hạn kích thước, kiểm tra loại file
- [ ] Gửi email: Spring Mail + template (Thymeleaf), test bằng Mailpit/MailHog
- [ ] Tác vụ định kỳ: `@Scheduled` (hủy đơn chưa thanh toán sau 15 phút)
- [ ] Xử lý bất đồng bộ: `@Async`, `@EventListener`, `ApplicationEventPublisher`
- [ ] Cache: `@Cacheable` (sau đó gắn Redis ở giai đoạn 9)
- [ ] **Spring Boot Actuator**: health check, metrics
- [ ] Gọi API bên ngoài: `RestClient` / `WebClient`
- [ ] CORS config để frontend gọi được

### 6.6. Kiến trúc code (song song)
- [ ] Cấu trúc package **theo tính năng** (package-by-feature):
```
com.namtron.shop
├── common/            (exception, response, util, config)
├── auth/              (controller, service, dto, jwt)
├── user/
├── catalog/           (category, brand, product, variant)
├── cart/
├── order/
├── payment/
├── review/
└── admin/
```
- [ ] Mỗi module chỉ gọi module khác qua **Service public**, không đụng thẳng Repository của module khác → giữ monolith gọn gàng (Modular Monolith). Có thể dùng **Spring Modulith** để kiểm tra.
- [ ] Design pattern hay gặp: Builder, Factory, Strategy (tính phí ship, phương thức thanh toán), Template Method

✅ **Mini project:** **REST API cho shop điện thoại (bản 1, chưa có bảo mật)**
- CRUD danh mục, hãng, sản phẩm, biến thể
- API danh sách sản phẩm có phân trang + lọc động (hãng, khoảng giá, RAM, bộ nhớ) + sắp xếp
- Giỏ hàng, đặt hàng, trừ tồn kho trong transaction, xử lý hết hàng
- Flyway migration, Swagger UI, xử lý lỗi thống nhất
- Unit test + integration test với Testcontainers

---

<a id="muc-7"></a>

## 7. Giai đoạn 5 – Security (3–4 tuần)

### 7.1. Khái niệm nền (0.5 tuần)
- [ ] **Authentication** (bạn là ai) vs **Authorization** (bạn được làm gì)
- [ ] Hash mật khẩu: **BCrypt / Argon2** — không bao giờ lưu mật khẩu thô, không dùng MD5/SHA1
- [ ] Session-based vs Token-based (JWT): ưu nhược điểm
- [ ] Cookie flags: `HttpOnly`, `Secure`, `SameSite`
- [ ] HTTPS bắt buộc ở production

### 7.2. Spring Security (2 tuần) ⭐⭐
- [ ] Kiến trúc: **Security Filter Chain**, `SecurityFilterChain` bean
- [ ] `UserDetailsService`, `PasswordEncoder`, `AuthenticationManager`
- [ ] Đăng ký, đăng nhập, đăng xuất
- [ ] **JWT**: access token (ngắn hạn) + **refresh token** (dài hạn, lưu DB, có thể thu hồi), rotation
- [ ] Lưu token ở đâu phía frontend: cookie `HttpOnly` (khuyên dùng) vs localStorage (rủi ro XSS)
- [ ] Phân quyền: Role (`USER`, `ADMIN`, `INSTRUCTOR`…), `@PreAuthorize`, `hasRole`, kiểm tra quyền sở hữu tài nguyên (user chỉ xem đơn hàng của mình)
- [ ] **OAuth2 Login**: đăng nhập bằng Google / Facebook
- [ ] Xác thực email, quên mật khẩu (token 1 lần, hết hạn)
- [ ] (Nâng cao) 2FA bằng OTP; Spring Authorization Server hoặc Keycloak

### 7.3. Bảo mật ứng dụng web – OWASP Top 10 (1 tuần) ⭐
- [ ] **Broken Access Control** (IDOR: sửa `/orders/123` thành `/orders/124`)
- [ ] **Injection** (SQL injection) – dùng parameterized query, không nối chuỗi SQL
- [ ] **XSS** – escape output, Content Security Policy
- [ ] **CSRF** – khi nào cần bật (dùng cookie), khi nào có thể tắt
- [ ] **CORS** – chỉ cho phép domain frontend của bạn
- [ ] Security misconfiguration – tắt endpoint debug, ẩn stacktrace ở prod
- [ ] Vulnerable dependencies – dùng Dependabot, OWASP Dependency-Check
- [ ] **Rate limiting** chống brute-force đăng nhập (Bucket4j)
- [ ] Upload file an toàn: kiểm tra MIME, đổi tên file, không lưu trong thư mục chạy code
- [ ] Quản lý **secret**: biến môi trường, `.env` không commit, Vault/secret manager ở prod
- [ ] **Xử lý thanh toán an toàn**: luôn xác minh chữ ký callback từ cổng thanh toán, không tin giá frontend gửi lên
- [ ] Security header: HSTS, X-Frame-Options, X-Content-Type-Options

✅ **Mini project:** Gắn bảo mật cho API shop:
- Đăng ký/đăng nhập JWT + refresh token trong cookie HttpOnly
- Đăng nhập Google
- Admin mới được thêm/sửa sản phẩm; user chỉ xem được đơn của mình
- Rate limit đăng nhập; test bảo mật (thử tự tấn công IDOR, SQL injection vào API của mình)

---

<a id="muc-8"></a>

## 8. Giai đoạn 6 – TypeScript (2 tuần)

- [ ] Vì sao TypeScript: bắt lỗi lúc viết code, gợi ý tốt hơn, làm việc nhóm dễ hơn
- [ ] Kiểu cơ bản: `string`, `number`, `boolean`, `array`, `tuple`, `any` (tránh), `unknown`, `never`
- [ ] `type` vs `interface`
- [ ] Union `|`, intersection `&`, literal type (`'PENDING' | 'PAID'`)
- [ ] Type narrowing (`typeof`, `in`, discriminated union)
- [ ] Generics (`ApiResponse<T>`, `Page<T>`)
- [ ] Utility types: `Partial`, `Pick`, `Omit`, `Record`, `Readonly`, `ReturnType`
- [ ] `tsconfig.json`, `strict: true`
- [ ] Gõ kiểu cho dữ liệu API trả về; **validate lúc chạy bằng Zod**
- [ ] (Nâng cao) Sinh type TypeScript tự động từ OpenAPI của Spring Boot (`openapi-typescript`, Orval) → frontend và backend luôn khớp nhau

✅ **Mini project:** Viết lại mini project JS ở giai đoạn 1 bằng TypeScript với `strict: true`.

---

<a id="muc-9"></a>

## 9. Giai đoạn 7 – React (6–8 tuần)

### 9.1. React cốt lõi (2 tuần) ⭐
- [ ] Tạo project bằng **Vite + React + TypeScript**
- [ ] JSX, component, props, `children`
- [ ] Render có điều kiện, render danh sách, `key`
- [ ] **State**: `useState`; state bất biến (immutable update)
- [ ] Sự kiện, form có kiểm soát (controlled)
- [ ] **`useEffect`**: khi nào dùng & khi nào KHÔNG cần dùng
- [ ] `useRef`, `useMemo`, `useCallback` (hiểu trước khi tối ưu)
- [ ] **Custom hook** (`useDebounce`, `useCart`)
- [ ] Context API (theme, auth)
- [ ] Lifting state up, composition
- [ ] React mới: Suspense, `use`, Actions, `useOptimistic` (theo bản React hiện tại)

### 9.2. Hệ sinh thái bắt buộc (2–3 tuần) ⭐
- [ ] **Routing**: React Router (nếu dùng Vite) — route lồng nhau, route bảo vệ
- [ ] **Gọi API & cache dữ liệu server**: **TanStack Query** (`useQuery`, `useMutation`, invalidate, phân trang, infinite scroll)
- [ ] HTTP client: `fetch` hoặc Axios (interceptor gắn token, tự refresh token)
- [ ] **State toàn cục phía client**: **Zustand** (đơn giản) hoặc Redux Toolkit (dự án lớn, nhiều công ty dùng)
- [ ] **Form**: **React Hook Form + Zod**
- [ ] **Styling**: **Tailwind CSS** + **shadcn/ui** (component đẹp, tùy biến được)
- [ ] Icon: lucide-react; toast thông báo: sonner
- [ ] Bảng dữ liệu cho admin: TanStack Table
- [ ] Biểu đồ cho dashboard: Recharts
- [ ] Xử lý lỗi: Error Boundary, trạng thái loading/empty/error cho mọi màn hình

### 9.3. Chất lượng code frontend (1 tuần)
- [ ] ESLint + Prettier
- [ ] Cấu trúc thư mục theo tính năng (`features/cart`, `features/product`…)
- [ ] Tối ưu: lazy loading route, code splitting, memo đúng chỗ, React DevTools Profiler
- [ ] Accessibility: dùng phím được, contrast, aria
- [ ] **Test**: Vitest + React Testing Library (unit/component), **Playwright** (e2e: đăng nhập → thêm giỏ → đặt hàng)

✅ **Mini project:** Frontend **React (Vite)** cho API shop ở giai đoạn 4–5:
- Trang danh sách có bộ lọc (state đồng bộ lên URL query), chi tiết sản phẩm chọn biến thể
- Giỏ hàng, đặt hàng, lịch sử đơn
- Đăng nhập/đăng ký, route bảo vệ, tự refresh token
- Trang admin CRUD sản phẩm có upload ảnh

---

<a id="muc-10"></a>

## 10. Giai đoạn 8 – Next.js (3–4 tuần)

### 10.1. Vì sao cần Next.js?
- Web bán hàng / bán khóa học **sống nhờ Google (SEO)**. React thuần (SPA) render ở trình duyệt → SEO yếu, tải lần đầu chậm.
- Next.js render ở server (SSR/SSG) → Google đọc được nội dung, trang hiện nhanh.
- 👉 **Dùng Next.js cho trang khách hàng**. Trang admin có thể dùng chung Next.js hoặc React (Vite) đều được.
- 👉 Trong roadmap này: **Next.js chỉ làm frontend (+ BFF nhẹ)**, mọi nghiệp vụ, dữ liệu nằm ở **Spring Boot**.

### 10.2. Kiến thức Next.js (App Router) ⭐
- [ ] Cấu trúc `app/`, file-based routing, `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, `not-found.tsx`
- [ ] Route động `[slug]`, route group `(shop)`, `(admin)`
- [ ] **Server Components vs Client Components** (`"use client"`) — hiểu ranh giới này là quan trọng nhất
- [ ] Các kiểu render: **SSR, SSG, ISR** (revalidate), streaming
- [ ] Fetch dữ liệu từ Spring Boot trong Server Component; cache & revalidate
- [ ] **Server Actions** (gửi form) — gọi tiếp sang Spring Boot
- [ ] Route Handlers (`app/api/...`) — dùng làm BFF/proxy khi cần
- [ ] **Middleware**: chặn trang cần đăng nhập, chuyển hướng
- [ ] Xử lý auth với cookie HttpOnly từ Spring Boot (hoặc Auth.js nếu cần)
- [ ] **SEO**: `metadata`, `generateMetadata`, sitemap, robots, Open Graph, structured data (JSON-LD Product, Course)
- [ ] Tối ưu: `next/image`, `next/font`, lazy load
- [ ] Biến môi trường: `NEXT_PUBLIC_` vs biến bí mật phía server
- [ ] Đa ngôn ngữ (i18n) — nếu cần

✅ **Mini project:** Chuyển frontend khách hàng của shop sang Next.js:
- Trang chủ, danh mục, chi tiết sản phẩm render server, có SEO đầy đủ, URL đẹp (`/dien-thoai/iphone-16-pro-max`)
- Giỏ hàng & checkout là Client Component
- Kiểm tra điểm **Lighthouse ≥ 90** cho Performance và SEO

---

<a id="muc-11"></a>

## 11. Giai đoạn 9 – Tính năng nâng cao của web thật (4–6 tuần)

Học **khi dự án cần đến**, không học dàn trải. Mỗi mục là một kỹ năng mà web thật bắt buộc có.

### 11.1. Lưu trữ file & media
- [ ] Lưu ảnh/video lên **object storage**: AWS S3 / Cloudflare R2 / **MinIO** (chạy local) / Cloudinary
- [ ] **Presigned URL**: frontend upload thẳng lên storage, không đi qua server
- [ ] Resize ảnh, nhiều kích thước (thumbnail), định dạng WebP/AVIF
- [ ] CDN để tải ảnh nhanh

### 11.2. Thanh toán 💳
- [ ] Luồng chuẩn: tạo đơn `PENDING` → chuyển sang cổng thanh toán → **IPN/Webhook** báo kết quả → cập nhật `PAID`
- [ ] Tích hợp **VNPay / MoMo / ZaloPay** (sandbox) cho Việt Nam; **Stripe** cho quốc tế
- [ ] Xác minh chữ ký (checksum), **idempotency** (webhook gửi 2 lần không được cộng tiền 2 lần)
- [ ] Hoàn tiền, đối soát, hết hạn thanh toán
- [ ] COD (thanh toán khi nhận hàng)

### 11.3. Cache & hiệu năng
- [ ] **Redis**: cache sản phẩm, danh mục, trang chủ; TTL, invalidate khi cập nhật
- [ ] Redis cho: lưu giỏ hàng khách vãng lai, rate limit, OTP, distributed lock
- [ ] HTTP caching: `Cache-Control`, `ETag`
- [ ] Đo hiệu năng bằng **k6** / JMeter

### 11.4. Tìm kiếm
- [ ] Bản 1: PostgreSQL full-text search + `unaccent` (tìm "iphone" ra "iPhone", tìm không dấu)
- [ ] Bản 2: **Meilisearch** hoặc **Elasticsearch/OpenSearch**: gợi ý khi gõ, sửa lỗi chính tả, faceted search (lọc theo hãng/giá kèm số lượng)
- [ ] Đồng bộ dữ liệu DB → search engine

### 11.5. Thông báo & thời gian thực
- [ ] Email giao dịch: xác nhận đơn, hóa đơn (dịch vụ: Resend, SendGrid, Amazon SES)
- [ ] **WebSocket / STOMP** hoặc **Server-Sent Events**: thông báo đơn hàng mới cho admin, chat hỗ trợ
- [ ] (Tùy chọn) Push notification trình duyệt

### 11.6. Xử lý nền & sự kiện
- [ ] Hàng đợi: **RabbitMQ** (hoặc Redis Stream) cho gửi email, xử lý video, tạo hóa đơn PDF
- [ ] **Outbox pattern** (gửi sự kiện tin cậy khi đã commit DB) – mức hiểu
- [ ] Job định kỳ: hủy đơn quá hạn, gửi email nhắc giỏ hàng bị bỏ quên, báo cáo doanh thu hằng ngày
- [ ] (Nâng cao) Kafka – chỉ khi đã vững RabbitMQ

### 11.7. Video cho nền tảng khóa học 🎬
- [ ] Không phát file MP4 lớn trực tiếp → chuyển sang **HLS** (`.m3u8`) bằng **FFmpeg**, nhiều độ phân giải
- [ ] Bảo vệ video: signed URL có hạn, chỉ người đã mua mới xem được
- [ ] Hoặc dùng dịch vụ: Mux, Cloudflare Stream, Bunny Stream
- [ ] Trình phát: hls.js / Video.js; lưu tiến độ xem

### 11.8. Các thứ khác hay gặp
- [ ] Xuất **Excel/PDF** (Apache POI, OpenPDF/JasperReports) cho báo cáo, hóa đơn
- [ ] Tích hợp **API vận chuyển** (GHN, GHTK) để tính phí ship
- [ ] Đa ngôn ngữ, đa tiền tệ
- [ ] **Tích hợp AI vào sản phẩm**: chatbot tư vấn chọn điện thoại, tóm tắt review, gợi ý khóa học (gọi API LLM từ Spring Boot bằng **Spring AI**; RAG trên dữ liệu sản phẩm)
- [ ] Analytics: Google Analytics / PostHog; theo dõi funnel mua hàng

---

<a id="muc-12"></a>

## 12. Giai đoạn 10 – DevOps & Deploy (3–4 tuần)

### 12.1. Linux server cơ bản
- [ ] SSH, quản lý user, quyền file (`chmod`, `chown`)
- [ ] Quản lý process, xem log (`journalctl`, `tail -f`)
- [ ] Firewall cơ bản (`ufw`)

### 12.2. Docker ⭐
- [ ] Image, container, volume, network
- [ ] Viết **Dockerfile** cho Spring Boot (multi-stage build, image nhỏ) và cho Next.js (`output: standalone`)
- [ ] **Docker Compose**: chạy cả hệ thống (backend + frontend + PostgreSQL + Redis + MinIO + Mailpit) bằng 1 lệnh
- [ ] `.dockerignore`, biến môi trường, healthcheck

### 12.3. Reverse proxy & domain
- [ ] **Nginx** (hoặc Caddy): reverse proxy, gzip, phục vụ static
- [ ] Mua domain, trỏ DNS
- [ ] **HTTPS miễn phí** với Let's Encrypt (Certbot) hoặc Caddy tự động
- [ ] Cloudflare: CDN, chống DDoS cơ bản

### 12.4. CI/CD
- [ ] **GitHub Actions**: mỗi PR tự chạy build + test + lint
- [ ] Tự build Docker image, đẩy lên registry (GHCR / Docker Hub)
- [ ] Tự deploy khi merge vào `main`
- [ ] Quét bảo mật dependency & secret trong pipeline

### 12.5. Nơi deploy (chọn 1 cách)
| Cách | Phù hợp |
|---|---|
| **VPS** (DigitalOcean, Vultr, Hetzner, VPS Việt Nam) + Docker Compose | Học được nhiều nhất, rẻ, khuyên dùng |
| Render / Railway / Fly.io cho backend + **Vercel** cho Next.js | Nhanh, ít cấu hình |
| **AWS** (EC2, RDS, S3, CloudFront) | Học thêm để đi làm, phức tạp hơn |

### 12.6. Vận hành (Observability)
- [ ] Log có cấu trúc (JSON), tập trung log (Loki / ELK)
- [ ] Metrics: Actuator + **Prometheus + Grafana**
- [ ] Theo dõi lỗi: **Sentry** (cả frontend và backend)
- [ ] Uptime monitor (UptimeRobot, Better Stack)
- [ ] **Backup database tự động** hằng ngày + thử restore
- [ ] (Biết thêm) Kubernetes — học sau khi đã đi làm hoặc khi thật sự cần

✅ **Mini project:** Deploy shop lên Internet với domain riêng, HTTPS, CI/CD tự động, có backup và monitor.

---

<a id="muc-13"></a>

## 13. Dự án lớn 1 – Shop điện thoại (kiểu CellphoneS)

> Thời gian: 6–8 tuần (làm xuyên suốt, cộng dồn từ các mini project). Đây là **dự án portfolio chính**.

### 13.1. Phân tích yêu cầu (làm trước khi code!)
- [ ] Viết **User Story** cho 3 vai trò: Khách vãng lai, Khách hàng, Admin/Nhân viên
- [ ] Vẽ **wireframe** các màn hình chính (Figma / Excalidraw)
- [ ] Vẽ **ERD**, **sơ đồ kiến trúc**, **luồng đặt hàng & thanh toán** (sequence diagram)
- [ ] Liệt kê API (OpenAPI) trước khi code (API-first)
- [ ] Chia công việc thành Issue trên GitHub Projects, ước lượng thời gian

### 13.2. Tính năng phía khách hàng
**Duyệt sản phẩm**
- [ ] Trang chủ: banner, flash sale có đồng hồ đếm ngược, sản phẩm nổi bật theo danh mục
- [ ] Menu danh mục nhiều cấp (Điện thoại → Apple → iPhone 16 series)
- [ ] Danh sách có **bộ lọc đa tiêu chí** (hãng, giá, RAM, ROM, màn hình, pin, nhu cầu), sắp xếp, phân trang
- [ ] Chi tiết sản phẩm: gallery ảnh, chọn **biến thể** (màu + dung lượng → giá & tồn kho khác nhau), thông số kỹ thuật, bài viết đánh giá
- [ ] **So sánh sản phẩm** (2–3 máy)
- [ ] Tìm kiếm có gợi ý, không dấu
- [ ] Sản phẩm đã xem gần đây, sản phẩm tương tự

**Mua hàng**
- [ ] Giỏ hàng (cho cả khách chưa đăng nhập, gộp giỏ khi đăng nhập)
- [ ] Mã giảm giá (theo %, theo số tiền, đơn tối thiểu, giới hạn lượt dùng, hạn sử dụng)
- [ ] Chọn địa chỉ (tỉnh/thành → phường/xã), tính phí ship
- [ ] Thanh toán: COD, VNPay/MoMo
- [ ] Trả góp (mô phỏng: chọn kỳ hạn, tính tiền mỗi tháng)
- [ ] Email xác nhận đơn

**Tài khoản**
- [ ] Đăng ký/đăng nhập (email, Google), quên mật khẩu
- [ ] Sổ địa chỉ, lịch sử đơn, theo dõi trạng thái đơn, hủy đơn
- [ ] Đánh giá & bình luận (chỉ người đã mua mới được đánh giá sao), hỏi đáp
- [ ] Danh sách yêu thích
- [ ] Tích điểm thành viên / hạng thành viên

### 13.3. Tính năng phía Admin
- [ ] Dashboard: doanh thu theo ngày/tháng, đơn mới, top sản phẩm, biểu đồ
- [ ] Quản lý danh mục, hãng, sản phẩm, biến thể, thông số, ảnh
- [ ] Quản lý tồn kho (nhập kho, lịch sử xuất nhập, cảnh báo sắp hết)
- [ ] Quản lý đơn hàng theo **state machine**: `PENDING → CONFIRMED → SHIPPING → DELIVERED` / `CANCELLED` / `RETURNED`
- [ ] Quản lý khuyến mãi, flash sale, banner
- [ ] Quản lý người dùng, phân quyền nhân viên (Admin, Nhân viên kho, Nhân viên bán hàng)
- [ ] Duyệt đánh giá, trả lời hỏi đáp
- [ ] Xuất báo cáo Excel
- [ ] Nhật ký thao tác (audit log)

### 13.4. Bài toán kỹ thuật cần giải được (điểm cộng khi phỏng vấn)
- [ ] **Flash sale**: 1.000 người tranh 100 máy → không bán vượt tồn kho (lock DB / Redis atomic decrement / hàng đợi)
- [ ] Giữ hàng tạm khi đang thanh toán, nhả lại nếu quá 15 phút
- [ ] Giá thay đổi khi khách đang để trong giỏ
- [ ] Webhook thanh toán đến 2 lần, hoặc không đến
- [ ] Trang danh sách vẫn nhanh khi có 100.000 sản phẩm (index, cache, search engine)
- [ ] SEO: URL thân thiện, sitemap, dữ liệu có cấu trúc

### 13.5. Thứ tự làm (milestone)
| Tuần | Mục tiêu |
|---|---|
| 1 | Phân tích, ERD, wireframe, setup repo, Docker Compose, CI |
| 2 | Auth + quản lý user + catalog (danh mục, hãng, sản phẩm, biến thể) |
| 3 | Danh sách, lọc, tìm kiếm, chi tiết sản phẩm (FE + BE) |
| 4 | Giỏ hàng, mã giảm giá, đặt hàng, trừ tồn kho |
| 5 | Thanh toán VNPay/MoMo + COD, email, trạng thái đơn |
| 6 | Trang admin + dashboard + báo cáo |
| 7 | Review, wishlist, so sánh, flash sale, cache Redis |
| 8 | Test e2e, tối ưu hiệu năng & SEO, deploy, viết README + demo video |

---

<a id="muc-14"></a>

## 14. Dự án lớn 2 – Nền tảng khóa học (kiểu Udemy)

> Thời gian: 5–7 tuần. Tái sử dụng rất nhiều thứ từ dự án 1 (auth, thanh toán, upload, admin) — chính là lúc thấy giá trị của việc viết code có tổ chức.

### 14.1. Vai trò
- **Học viên** – tìm, mua, học khóa học
- **Giảng viên** – tạo và bán khóa học, xem doanh thu
- **Admin** – duyệt khóa học, quản lý người dùng, chia doanh thu

### 14.2. Tính năng học viên
- [ ] Duyệt/tìm kiếm khóa học theo chủ đề, trình độ, giá, đánh giá, ngôn ngữ
- [ ] Trang chi tiết: mô tả, nội dung (section → lecture), video học thử miễn phí, giảng viên, đánh giá
- [ ] Giỏ hàng, mã giảm giá của giảng viên, thanh toán
- [ ] **Trang học**: trình phát video HLS, danh sách bài, đánh dấu hoàn thành, **lưu tiến độ** và vị trí đang xem
- [ ] Ghi chú theo mốc thời gian video, hỏi đáp trong từng bài
- [ ] Quiz / bài tập, tài liệu đính kèm
- [ ] **Chứng chỉ** hoàn thành (sinh PDF, có mã xác thực)
- [ ] Đánh giá khóa học (sau khi học ≥ x%)
- [ ] Danh sách mong muốn, khóa học của tôi

### 14.3. Tính năng giảng viên
- [ ] Đăng ký làm giảng viên
- [ ] Trình tạo khóa học: thông tin, giáo trình (kéo thả sắp xếp section/lecture), upload video (upload theo chunk, chạy nền chuyển sang HLS)
- [ ] Đặt giá, tạo coupon
- [ ] Gửi khóa học để admin duyệt (state machine: `DRAFT → PENDING_REVIEW → PUBLISHED / REJECTED`)
- [ ] Dashboard: số học viên, doanh thu, đánh giá, tỉ lệ hoàn thành
- [ ] Trả lời hỏi đáp, gửi thông báo cho học viên

### 14.4. Tính năng admin
- [ ] Duyệt khóa học, quản lý danh mục chủ đề
- [ ] Quản lý người dùng, giảng viên
- [ ] **Chia doanh thu** (ví dụ giảng viên 70% / nền tảng 30%), lịch thanh toán cho giảng viên
- [ ] Hoàn tiền trong 30 ngày
- [ ] Báo cáo doanh thu toàn hệ thống

### 14.5. Bài toán kỹ thuật đặc thù
- [ ] Upload video dung lượng lớn (resumable upload / multipart upload lên S3)
- [ ] Xử lý video nền (hàng đợi + FFmpeg), cập nhật trạng thái "đang xử lý"
- [ ] Chặn xem trộm video (signed URL hết hạn, kiểm tra quyền sở hữu khóa học)
- [ ] Ghi tiến độ học thường xuyên mà không làm nghẽn DB (gom nhóm / debounce)
- [ ] Gợi ý khóa học (đơn giản: cùng chủ đề, người mua A cũng mua B; nâng cao: dùng embedding AI)

### 14.6. Ý tưởng dự án tiếp theo (để luyện thêm)
- 🏨 Đặt phòng khách sạn/homestay (lịch trống, đặt cọc, chống đặt trùng)
- 🍔 Đặt đồ ăn (nhiều cửa hàng, đơn realtime, bản đồ)
- 🎫 Bán vé sự kiện/xem phim (chọn ghế realtime, giữ ghế)
- 💬 Mạng xã hội mini (bài viết, follow, feed, chat realtime)
- 🏢 Hệ thống quản lý nội bộ (HR, chấm công, nghỉ phép, phân quyền phức tạp)

---

<a id="muc-15"></a>

## 15. Sau roadmap: học gì tiếp

Chỉ học khi đã hoàn thành 2 dự án lớn:

| Chủ đề | Học khi nào |
|---|---|
| **Microservices** (Spring Cloud, API Gateway, service discovery, Saga, distributed tracing) | Khi đã hiểu rõ giới hạn của monolith |
| **Kafka**, event-driven architecture | Khi cần xử lý lượng sự kiện lớn |
| **Kubernetes** | Khi đi làm ở công ty dùng nó |
| **System Design** (load balancer, sharding, replication, CAP) | Để lên mid/senior, phỏng vấn công ty lớn |
| **DDD** (Domain-Driven Design), Clean/Hexagonal Architecture | Khi nghiệp vụ phức tạp |
| **GraphQL**, gRPC | Khi dự án yêu cầu |
| **React Native** | Muốn làm app mobile dùng lại kiến thức React |
| **Cấu trúc dữ liệu & giải thuật** sâu hơn | Song song, để phỏng vấn |
| **Xây sản phẩm có AI**: Spring AI, RAG, vector DB (pgvector), AI agent | Xu hướng mạnh — nên học sớm sau roadmap |

---

<a id="muc-16"></a>

## 16. Lịch học mẫu theo tuần

### 16.1. Một ngày (người đi học/đi làm, ~2.5–3 giờ)
| Thời gian | Hoạt động |
|---|---|
| 15 phút | Ôn flashcard + đọc lại ghi chú hôm qua |
| 45 phút | Học lý thuyết mới (tài liệu chính thức / video) |
| 90 phút | **Code** — bài tập / mini project (AI ở chế độ phù hợp giai đoạn) |
| 15 phút | Viết nhật ký học + commit lên GitHub |

### 16.2. Một tuần
| Ngày | Việc |
|---|---|
| T2 – T6 | Vòng lặp Learn → Build mỗi ngày |
| T7 | Làm phần lớn của mini project, nhờ AI review code |
| CN | Ôn tập cả tuần, viết 1 bài tổng kết (blog/Notion), nghỉ ngơi |

### 16.3. Tổng lộ trình (≈ 44–52 tuần)
| Tháng | Nội dung |
|---|---|
| 1 | Giai đoạn 0 + bắt đầu HTML/CSS |
| 2 | JavaScript + bắt đầu Java Core |
| 3 | Java Core (OOP, Collections, Stream) |
| 4 | Database & SQL + bắt đầu Spring Boot |
| 5 | Spring Boot (REST, JPA) |
| 6 | Spring Boot (test, đi kèm) + Security |
| 7 | TypeScript + React |
| 8 | React (hệ sinh thái) + Next.js |
| 9 | Tính năng nâng cao + Docker + Deploy |
| 10–11 | 🛒 Dự án Shop điện thoại |
| 12 | 🎓 Dự án Khóa học + chuẩn bị CV, phỏng vấn |

> 📌 Chậm hơn kế hoạch là **bình thường**. Quan trọng là đều đặn mỗi ngày, không bỏ quá 2 ngày liên tiếp.

---

<a id="muc-17"></a>

## 17. Checklist tự đánh giá & đi xin việc

### 17.1. Bạn đã "xong" roadmap khi tự làm được (không cần AI viết hộ) những việc sau:
- [ ] Giải thích được một request đi từ trình duyệt → Next.js → Spring Boot → DB và quay về
- [ ] Thiết kế ERD cho một nghiệp vụ mới trong 1 giờ
- [ ] Viết CRUD API có validation, phân trang, xử lý lỗi, test trong 1 buổi
- [ ] Giải thích & sửa được lỗi N+1, lỗi `LazyInitializationException`
- [ ] Giải thích JWT + refresh token và các rủi ro bảo mật
- [ ] Giải thích Server Component vs Client Component
- [ ] Chạy cả hệ thống bằng `docker compose up`
- [ ] Deploy lên VPS có domain + HTTPS + CI/CD
- [ ] Đọc hiểu và review được code do AI sinh ra, chỉ ra được ít nhất 1 vấn đề

### 17.2. Portfolio
- [ ] 2 dự án lớn đã deploy, có link demo, tài khoản demo (user & admin)
- [ ] README mỗi dự án gồm: giới thiệu, ảnh/GIF demo, sơ đồ kiến trúc, ERD, công nghệ, cách chạy local, các bài toán kỹ thuật đã giải
- [ ] Video demo 3–5 phút
- [ ] GitHub có lịch sử commit đều đặn, PR có mô tả rõ ràng
- [ ] (Điểm cộng) Blog kỹ thuật 5–10 bài về những gì đã học/giải quyết

### 17.3. CV & phỏng vấn
- [ ] CV 1 trang, ghi rõ kết quả: "Xử lý flash sale 1.000 request đồng thời không bán vượt tồn kho"
- [ ] Ôn câu hỏi phỏng vấn: Java Core, OOP, Collections, Spring (IoC, AOP, Transaction), JPA, SQL, REST, Security, React, Git
- [ ] Luyện 50–100 bài LeetCode Easy/Medium
- [ ] Phỏng vấn thử với AI (prompt ở mục 1.3) và với bạn bè
- [ ] Chuẩn bị kể lại 3 câu chuyện: một bug khó, một quyết định thiết kế, một lần học công nghệ mới nhanh

---

<a id="muc-18"></a>

## 18. Tài nguyên học

> Luôn ưu tiên **tài liệu chính thức** — nó đúng phiên bản và chính xác nhất. Dùng AI để giải thích tài liệu, không để thay thế tài liệu.

### Nền tảng
- **roadmap.sh** – các roadmap Backend, Java, Spring Boot, React, Frontend
- **MDN Web Docs** (developer.mozilla.org) – HTML, CSS, JS, HTTP
- **javascript.info** – JavaScript từ cơ bản đến nâng cao
- **Pro Git** (git-scm.com/book) – sách Git miễn phí
- **The Odin Project** – lộ trình web miễn phí, nhiều bài thực hành

### Java & Spring
- **dev.java** – tài liệu Java chính thức
- Sách: *Head First Java*, *Effective Java* (Joshua Bloch), *Clean Code*
- **spring.io/guides** và **docs.spring.io** – hướng dẫn chính thức
- **Baeldung** – bài viết Spring cực kỳ chi tiết
- Sách: *Spring in Action*, *Spring Security in Action*
- Repo mẫu: **spring-petclinic**
- **Vlad Mihalcea** (blog) – JPA/Hibernate hiệu năng cao

### Database
- **PostgreSQL Documentation**, **pgexercises.com**, **SQLBolt**
- Sách: *Designing Data-Intensive Applications* (đọc khi đã vững cơ bản)

### Frontend
- **typescriptlang.org/docs** (Handbook)
- **react.dev** – tài liệu React chính thức (đọc phần Learn từ đầu đến cuối)
- **nextjs.org/learn** – khóa học Next.js miễn phí chính thức
- **TanStack Query docs**, **tailwindcss.com/docs**, **ui.shadcn.com**

### Security
- **OWASP Top 10**, **OWASP Cheat Sheet Series**
- **PortSwigger Web Security Academy** – học bảo mật web có lab thực hành miễn phí

### DevOps
- **docs.docker.com** (Get Started)
- **GitHub Actions docs**
- **DigitalOcean Community Tutorials** – hướng dẫn cấu hình server, Nginx, SSL rất dễ hiểu

### Luyện tập
- **LeetCode**, **HackerRank**, **Exercism** (Java, TypeScript)
- **Frontend Mentor** – luyện dựng giao diện từ thiết kế có sẵn

---

## 🏁 Lời nhắn cuối

> **"AI có thể viết code thay bạn, nhưng không thể hiểu thay bạn."**
>
> Người dùng AI giỏi nhất là người **hiểu sâu nền tảng** — họ biết hỏi đúng, biết AI sai ở đâu, và ghép mọi thứ thành một sản phẩm hoàn chỉnh.
> Hãy dùng AI để **học nhanh gấp 3**, không phải để **bỏ qua việc học**.
>
> Mỗi ngày một ít. Commit mỗi ngày. Xây thứ gì đó thật. 🚀
