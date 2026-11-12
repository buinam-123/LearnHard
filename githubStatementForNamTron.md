# 🐙 Git & GitHub từ A đến Z cho Nam Tròn

> **Mục tiêu:** Hiểu và dùng thành thạo Git + GitHub để quản lý mã nguồn, từ lệnh đầu tiên `git init` đến các kỹ thuật nâng cao như `rebase`, `cherry-pick`, `bisect`, làm việc nhóm bằng Pull Request và tự động hoá bằng GitHub Actions.
>
> **Cách đọc file này:**
> - Mỗi lệnh đều có: **📌 Tác dụng** → **⌨️ Cú pháp** → **🔍 Giải thích chi tiết** → **🤔 Tại sao lại ở vị trí này trong quy trình**.
> - Các phần được sắp xếp **đúng thứ tự bạn sẽ dùng ngoài thực tế**. Đọc từ trên xuống, đừng nhảy cóc.
> - Cuối mỗi phần lớn và cuối file có **bài tập có hướng dẫn từng bước**. Hãy *gõ tay* từng lệnh, đừng copy-paste — tay nhớ lâu hơn mắt.
>
> **Môi trường:** Windows → dùng **Git Bash** (cài cùng Git) hoặc terminal trong VS Code. Mọi lệnh dưới đây chạy được trên Windows, macOS, Linux.

---

## 📑 Mục lục

0. [Bức tranh tổng thể: Git là gì, GitHub là gì](#muc-0)
1. [Cài đặt & cấu hình lần đầu](#muc-1)
2. [Cấp độ 1 – Các lệnh cơ bản trên máy của bạn](#muc-2)
3. [Cấp độ 2 – Đưa code lên GitHub (remote)](#muc-3)
4. [Cấp độ 3 – Nhánh (branch) và gộp nhánh (merge)](#muc-4)
5. [Cấp độ 4 – Xử lý xung đột (conflict)](#muc-5)
6. [Cấp độ 5 – Quy trình làm việc trên GitHub: Fork, Pull Request, Issue](#muc-6)
7. [Cấp độ 6 – Quay lui, sửa sai, cứu dữ liệu](#muc-7)
8. [Cấp độ 7 – Kỹ thuật nâng cao](#muc-8)
9. [Cấp độ 8 – GitHub CLI (`gh`) & GitHub Actions](#muc-9)
10. [Quy ước làm việc nhóm chuyên nghiệp](#muc-10)
11. [🏋️ Bộ bài tập tổng hợp có hướng dẫn](#muc-11)
12. [🚑 Lỗi thường gặp & cách chữa](#muc-12)
13. [📋 Cheat sheet – bảng tra nhanh](#muc-13)

---

<a id="muc-0"></a>

## 0. Bức tranh tổng thể

### 0.1. Git khác GitHub thế nào?

| | **Git** | **GitHub** |
|---|---|---|
| Là gì | Phần mềm **quản lý phiên bản** chạy trên máy bạn | **Website/dịch vụ** lưu trữ repo Git trên Internet |
| Cần mạng? | Không | Có |
| Ví dụ đời thường | Cuốn sổ nhật ký ghi lại mọi lần bạn sửa bài | Thư viện công cộng nơi bạn gửi bản sao cuốn sổ để người khác đọc/cùng viết |
| Ai làm ra | Linus Torvalds (2005) | Công ty GitHub (Microsoft mua lại 2018) |

👉 **Git** là công cụ, **GitHub** là nơi chứa. Bạn có thể dùng Git mà không cần GitHub, nhưng không thể dùng GitHub hiệu quả nếu không biết Git. (GitLab, Bitbucket là các "thư viện" khác tương tự GitHub.)

### 0.2. Tại sao phải dùng Git?

- **Không bao giờ mất code:** mỗi lần `commit` là một "điểm lưu game". Hỏng thì quay về.
- **Biết ai sửa gì, khi nào, tại sao:** lịch sử ghi rõ từng dòng.
- **Làm việc nhóm không giẫm chân nhau:** mỗi người một nhánh, xong thì gộp.
- **Thử nghiệm thoải mái:** tạo nhánh thử ý tưởng, hỏng thì xoá nhánh, nhánh chính không ảnh hưởng.
- **Bắt buộc khi đi làm:** 100% công ty phần mềm dùng Git. Nhà tuyển dụng sẽ xem GitHub của bạn.

### 0.3. Ba "khu vực" của Git — PHẢI HIỂU cái này trước mọi lệnh

```
   (1) WORKING DIRECTORY          (2) STAGING AREA             (3) LOCAL REPOSITORY          (4) REMOTE (GitHub)
   Thư mục làm việc               Khu vực chờ (index)          Kho lịch sử trên máy          Kho trên Internet
 ┌──────────────────────┐      ┌──────────────────────┐      ┌──────────────────────┐      ┌──────────────────────┐
 │ File bạn đang sửa    │      │ Những thay đổi bạn   │      │ Các commit (điểm     │      │ Bản sao repo để chia │
 │ trong VS Code        │      │ ĐÃ CHỌN để chụp ảnh  │      │ lưu) đã chụp xong    │      │ sẻ / backup / làm    │
 │                      │      │                      │      │                      │      │ việc nhóm            │
 └──────────┬───────────┘      └──────────┬───────────┘      └──────────┬───────────┘      └──────────┬───────────┘
            │      git add  ──────────▶   │     git commit ─────────▶   │      git push  ──────────▶  │
            │                             │                             │                             │
            │  ◀──────── git restore      │  ◀──── git restore --staged │   ◀────────── git fetch/pull│
```

**Ví dụ đời thường — chụp ảnh tập thể:**
1. **Working directory** = cả lớp đang đứng lộn xộn trong sân.
2. **`git add`** = bạn gọi những người *muốn có mặt trong ảnh* lên bục. (Có thể gọi từng người, không bắt buộc cả lớp.)
3. **`git commit`** = bấm máy chụp. Ảnh được lưu vĩnh viễn vào album kèm ghi chú.
4. **`git push`** = đăng album lên Facebook (GitHub) để mọi người cùng xem.

🤔 **Tại sao lại cần bước "staging" ở giữa, sao không commit thẳng?**
Vì bạn thường sửa nhiều thứ cùng lúc (vừa sửa bug đăng nhập, vừa đổi màu nút). Staging cho phép bạn tách thành **2 commit riêng biệt, mỗi commit một mục đích** → lịch sử sạch, dễ đọc, dễ quay lui đúng chỗ.

### 0.4. Các thuật ngữ sẽ gặp

| Thuật ngữ | Nghĩa |
|---|---|
| **Repository (repo)** | Một dự án được Git quản lý (thư mục có thư mục ẩn `.git` bên trong) |
| **Commit** | Một "ảnh chụp" trạng thái code tại 1 thời điểm, có mã định danh (hash) như `f375861` |
| **Branch** | Một dòng phát triển độc lập. Thực chất chỉ là một "con trỏ" trỏ vào 1 commit |
| **HEAD** | Con trỏ cho biết "bạn đang đứng ở đâu" (thường là nhánh hiện tại) |
| **main / master** | Tên nhánh chính mặc định (GitHub giờ dùng `main`) |
| **Remote** | Bản repo nằm ở nơi khác (GitHub). Tên mặc định là `origin` |
| **origin/main** | Bản sao *trên máy bạn* của nhánh `main` trên remote `origin`, cập nhật khi `fetch` |
| **Clone** | Tải toàn bộ repo từ GitHub về máy |
| **Fork** | Sao chép repo của người khác thành repo của bạn *trên GitHub* |
| **Pull Request (PR)** | Lời đề nghị "hãy gộp nhánh của tôi vào nhánh của bạn", kèm review |
| **Merge** | Gộp lịch sử 2 nhánh lại |
| **Conflict** | Hai bên cùng sửa 1 chỗ, Git không tự quyết được, cần người chọn |

---

<a id="muc-1"></a>

## 1. Cài đặt & cấu hình lần đầu

> Phần này chỉ làm **1 lần duy nhất trên mỗi máy tính**.

### 1.1. `git --version` — kiểm tra đã cài Git chưa

- **📌 Tác dụng:** In ra phiên bản Git. Nếu báo lỗi "command not found" → chưa cài.
- **⌨️ Cú pháp:**
  ```bash
  git --version
  # Kết quả mẫu: git version 2.47.0.windows.1
  ```
- **🔍 Chưa cài?** Tải tại https://git-scm.com → Next liên tục (giữ mặc định). Nó cài kèm **Git Bash**.
- **🤔 Tại sao đầu tiên?** Không có Git thì mọi lệnh sau đều vô nghĩa.

### 1.2. `git config` — khai báo bạn là ai

- **📌 Tác dụng:** Gắn tên và email vào **mọi commit** bạn tạo ra. GitHub dùng email này để biết commit là của tài khoản nào (hiện avatar của bạn).
- **⌨️ Cú pháp:**
  ```bash
  git config --global user.name "Bui Thanh Nam"
  git config --global user.email "email-dang-ky-github@example.com"
  ```
- **🔍 Giải thích:**
  - `--global` = áp dụng cho **mọi repo** trên máy (lưu trong file `~/.gitconfig`).
  - Bỏ `--global` = chỉ áp dụng cho repo hiện tại (dùng khi máy công ty cần email công ty cho 1 repo riêng).
  - Email nên trùng email tài khoản GitHub, nếu không commit sẽ không hiện avatar và không tính "ô xanh" contribution.
- **🤔 Tại sao trước `commit`?** Git **từ chối commit** nếu không biết bạn là ai.

Một số cấu hình hữu ích nên làm luôn:

```bash
git config --global init.defaultBranch main     # repo mới tạo sẽ có nhánh chính tên "main" thay vì "master"
git config --global core.editor "code --wait"   # dùng VS Code làm trình soạn thảo khi Git cần bạn gõ message
git config --global core.autocrlf true          # (Windows) tự chuyển ký tự xuống dòng CRLF <-> LF, tránh diff "sửa cả file"
git config --global pull.rebase false           # khi git pull: dùng merge (mặc định dễ hiểu cho người mới)
git config --global color.ui auto               # tô màu output

git config --list                               # xem toàn bộ cấu hình hiện tại
git config user.name                            # xem 1 giá trị cụ thể
```

### 1.3. Kết nối máy tính với GitHub bằng SSH key

Khi bạn `push` code, GitHub cần biết **"bạn có đúng là chủ tài khoản không?"**. Có 2 cách: HTTPS (đăng nhập bằng trình duyệt/token) và **SSH** (dùng cặp khoá). SSH được khuyên dùng vì cài 1 lần, dùng mãi không cần nhập mật khẩu.

**Ví dụ đời thường:** SSH key giống cặp **ổ khoá – chìa khoá**. Bạn đưa *ổ khoá* (public key) cho GitHub gắn vào cửa, giữ *chìa* (private key) trong máy. Chỉ máy có đúng chìa mới mở được.

**Bước 1 – Tạo cặp khoá:**
```bash
ssh-keygen -t ed25519 -C "email-dang-ky-github@example.com"
# Nhấn Enter 3 lần (giữ đường dẫn mặc định, không đặt passphrase cũng được)
```
- `-t ed25519`: thuật toán mã hoá hiện đại, ngắn và an toàn.
- `-C`: ghi chú (comment) để bạn nhận ra khoá này của ai.
- Kết quả tạo ra 2 file trong `~/.ssh/`: `id_ed25519` (**chìa – TUYỆT ĐỐI không đưa ai**) và `id_ed25519.pub` (ổ khoá – được phép chia sẻ).

**Bước 2 – Copy public key:**
```bash
cat ~/.ssh/id_ed25519.pub
# Bôi đen toàn bộ dòng bắt đầu bằng "ssh-ed25519 ..." và copy
```

**Bước 3 – Dán lên GitHub:** GitHub → ảnh đại diện → **Settings** → **SSH and GPG keys** → **New SSH key** → dán vào → **Add SSH key**.

**Bước 4 – Kiểm tra:**
```bash
ssh -T git@github.com
# Lần đầu hỏi "Are you sure you want to continue connecting?" → gõ yes
# Thành công: "Hi <username>! You've successfully authenticated..."
```

🤔 **Tại sao làm bước này trước khi học `push`?** Vì nếu không, đến lúc `push` bạn sẽ bị lỗi `Permission denied (publickey)` và bị ngắt mạch học.

### ✅ Bài tập 1 – Chuẩn bị môi trường

1. Chạy `git --version`, chụp lại kết quả.
2. Cấu hình `user.name`, `user.email`, `init.defaultBranch main`.
3. Chạy `git config --list` và tìm 3 dòng bạn vừa đặt.
4. Tạo SSH key, thêm vào GitHub, chạy `ssh -T git@github.com` thấy dòng "Hi ...".

---

<a id="muc-2"></a>

## 2. Cấp độ 1 – Các lệnh cơ bản trên máy của bạn

> Đây là **vòng lặp hằng ngày** bạn sẽ dùng hàng trăm lần:
> ```
> sửa code → git status → git diff → git add → git commit → (lặp lại)
> ```

### 2.1. `git init` — biến một thư mục thường thành repo Git

- **📌 Tác dụng:** Tạo thư mục ẩn `.git` — nơi Git lưu toàn bộ lịch sử. Từ giờ Git bắt đầu "theo dõi" thư mục này.
- **⌨️ Cú pháp:**
  ```bash
  mkdir hoc-git        # tạo thư mục mới
  cd hoc-git           # đi vào thư mục
  git init             # khởi tạo repo
  # Initialized empty Git repository in .../hoc-git/.git/
  ```
- **🔍 Giải thích:**
  - Chỉ chạy **1 lần** cho mỗi dự án.
  - Xoá thư mục `.git` = xoá toàn bộ lịch sử (code hiện tại vẫn còn). Đừng động vào nó.
  - **Không** `git init` bên trong một repo khác (repo lồng repo gây rối).
- **🤔 Tại sao đầu tiên?** Mọi lệnh Git khác đều cần có `.git` mới chạy được. Chạy `git status` ở thư mục chưa init sẽ báo `fatal: not a git repository`.
- 💡 Nếu dự án **đã có trên GitHub** thì không dùng `init` mà dùng `git clone` (xem [mục 3.1](#muc-3)).

### 2.2. `git status` — "bây giờ tình hình thế nào?"

- **📌 Tác dụng:** Cho biết bạn đang ở nhánh nào, file nào đã sửa, file nào đã `add`, file nào Git chưa theo dõi.
- **⌨️ Cú pháp:**
  ```bash
  git status
  git status -s        # bản rút gọn, mỗi file 1 dòng
  ```
- **🔍 Đọc kết quả:**
  ```
  On branch main
  Changes to be committed:          ← (Staging) đã add, sẽ vào commit tới   — màu XANH
          modified:   index.html
  Changes not staged for commit:    ← đã sửa nhưng CHƯA add                  — màu ĐỎ
          modified:   style.css
  Untracked files:                  ← file mới, Git chưa từng theo dõi       — màu ĐỎ
          app.js
  ```
  Bản `-s`: `M ` (xanh, đã stage), ` M` (đỏ, chưa stage), `??` (untracked), `A ` (file mới đã add).
- **🤔 Tại sao chạy liên tục?** Đây là "la bàn". **Trước và sau mỗi lệnh quan trọng hãy chạy `git status`** để chắc chắn mọi thứ như bạn nghĩ. Thói quen này cứu bạn khỏi 90% lỗi.

### 2.3. `git diff` — xem chính xác đã sửa những dòng nào

- **📌 Tác dụng:** Hiển thị sự khác nhau từng dòng.
- **⌨️ Cú pháp:**
  ```bash
  git diff                 # Working dir  vs  Staging  → những gì CHƯA add
  git diff --staged        # Staging      vs  Commit cuối → những gì SẼ được commit
  git diff main feature    # so sánh 2 nhánh
  git diff abc123 def456   # so sánh 2 commit
  ```
- **🔍 Đọc kết quả:** dòng bắt đầu `-` (đỏ) = bị xoá, `+` (xanh) = được thêm. Nhấn `q` để thoát nếu dài.
- **🤔 Tại sao trước `add`?** Để **tự review code của mình** trước khi chốt: có còn `console.log` debug không? có lỡ sửa nhầm file không? Sau `add`, dùng `--staged` kiểm tra lần cuối trước `commit`.

### 2.4. `git add` — đưa thay đổi vào khu vực chờ (staging)

- **📌 Tác dụng:** Chọn những thay đổi sẽ được đưa vào commit tiếp theo.
- **⌨️ Cú pháp:**
  ```bash
  git add index.html          # add 1 file
  git add src/                # add cả thư mục
  git add *.css               # add theo mẫu
  git add .                   # add MỌI thay đổi trong thư mục hiện tại (kể cả file mới, file xoá)
  git add -p                  # add TỪNG ĐOẠN, Git hỏi y/n cho mỗi đoạn — rất mạnh (nâng cao)
  ```
- **🔍 Giải thích:**
  - `git add` **không** gửi gì lên mạng, **không** tạo điểm lưu — chỉ "đánh dấu".
  - Sau khi add mà sửa tiếp file đó, phần sửa mới **chưa** được add → phải add lại.
  - `git add .` tiện nhưng nguy hiểm nếu chưa có `.gitignore` (có thể add cả file mật khẩu, `node_modules`). → **Luôn `git status` trước `git add .`**
- **🤔 Tại sao trước `commit`?** Commit chỉ chụp những gì **nằm trong staging**. Không add = commit rỗng (`nothing to commit`).

### 2.5. `git commit` — tạo điểm lưu

- **📌 Tác dụng:** Chụp ảnh toàn bộ staging, lưu vĩnh viễn vào lịch sử với 1 mã hash + thông điệp mô tả.
- **⌨️ Cú pháp:**
  ```bash
  git commit -m "Thêm trang giới thiệu"     # commit với message ngắn
  git commit                                # mở editor để viết message dài (dòng 1 = tiêu đề, dòng trống, rồi mô tả)
  git commit -am "Sửa lỗi chính tả"         # -a: tự add mọi file ĐÃ ĐƯỢC THEO DÕI rồi commit (không add file mới!)
  ```
- **🔍 Viết message thế nào cho chuẩn?**
  - Mô tả **làm gì & tại sao**, không mô tả "đã sửa file nào" (Git đã biết điều đó).
  - ❌ `update`, `fix`, `asdf`, `sửa`
  - ✅ `Sửa lỗi giỏ hàng không cập nhật tổng tiền khi xoá sản phẩm`
  - Xem chuẩn **Conventional Commits** ở [mục 10](#muc-10).
- **🔍 Nguyên tắc vàng:** **Một commit = một thay đổi có ý nghĩa**. Đừng dồn cả ngày làm việc vào 1 commit; cũng đừng commit từng dấu chấm phẩy.
- **🤔 Tại sao sau `add` và trước `push`?** `add` chọn → `commit` chốt → `push` chia sẻ. Bạn chỉ có thể push những gì đã thành commit.

### 2.6. `git log` — xem lịch sử

- **📌 Tác dụng:** Liệt kê các commit từ mới đến cũ.
- **⌨️ Cú pháp:**
  ```bash
  git log                                   # đầy đủ: hash, tác giả, ngày, message (nhấn q để thoát)
  git log --oneline                         # mỗi commit 1 dòng — dùng nhiều nhất
  git log --oneline --graph --all           # vẽ cây nhánh bằng ký tự — RẤT nên nhớ
  git log -5                                # 5 commit gần nhất
  git log --author="Nam"                    # lọc theo tác giả
  git log -- index.html                     # chỉ commit có động đến file này
  git log -p                                # kèm luôn nội dung diff của từng commit
  git log --since="2 weeks ago"             # theo thời gian
  ```
- **🔍 Mẹo:** Tạo lệnh tắt (alias) để khỏi gõ dài:
  ```bash
  git config --global alias.lg "log --oneline --graph --all --decorate"
  git lg     # từ giờ chỉ cần gõ thế này
  ```
- **🤔 Khi nào dùng?** Sau mỗi commit để kiểm tra; khi cần tìm "commit nào làm hỏng"; khi cần lấy mã hash để dùng cho các lệnh khác (`revert`, `reset`, `cherry-pick`…).

### 2.7. `git show` — xem chi tiết 1 commit

```bash
git show              # commit mới nhất (HEAD)
git show f375861      # commit cụ thể
git show HEAD~2       # commit cách HEAD 2 bước về trước
```
- `HEAD~1` = cha của HEAD, `HEAD~2` = ông của HEAD... Ký hiệu này dùng ở rất nhiều lệnh khác.

### 2.8. `.gitignore` — bảo Git "đừng quan tâm những file này"

- **📌 Tác dụng:** Liệt kê file/thư mục mà Git **bỏ qua**: không hiện trong `status`, không bị `add .`.
- **Cách dùng:** Tạo file tên đúng là `.gitignore` ở thư mục gốc repo:
  ```gitignore
  # Thư viện tải về (tải lại được bằng npm install / mvn install)
  node_modules/
  target/

  # File build
  dist/
  build/
  *.class

  # File bí mật — KHÔNG BAO GIỜ được đưa lên GitHub
  .env
  *.pem
  application-secret.yml

  # Rác hệ điều hành / IDE
  .DS_Store
  Thumbs.db
  .idea/
  .vscode/

  # Log
  *.log
  ```
- **🔍 Lưu ý quan trọng:**
  - `.gitignore` chỉ có tác dụng với file **chưa từng được theo dõi**. File đã commit rồi thì phải gỡ khỏi Git: `git rm --cached ten-file` rồi commit.
  - Mẫu `.gitignore` cho từng ngôn ngữ: https://github.com/github/gitignore
- **🤔 Tại sao phải tạo NGAY SAU `git init`, TRƯỚC commit đầu tiên?** Vì nếu lỡ commit `node_modules` (hàng trăm MB) hay `.env` (mật khẩu database), chúng nằm vĩnh viễn trong lịch sử — xoá file ở commit sau **không** xoá nó khỏi lịch sử. Mật khẩu lộ lên GitHub public = bị bot quét và lợi dụng trong vài phút.

### 2.9. `git rm` và `git mv` — xoá / đổi tên file theo cách Git hiểu

```bash
git rm file.txt            # xoá file khỏi ổ đĩa VÀ ghi nhận việc xoá vào staging
git rm --cached file.txt   # chỉ ngừng theo dõi (file vẫn còn trên máy) — dùng khi lỡ commit file cần ignore
git mv cu.txt moi.txt      # đổi tên + ghi nhận vào staging
```
- Thực ra bạn có thể xoá/đổi tên bằng Explorer rồi `git add .` — Git cũng hiểu. `git rm`/`git mv` chỉ gộp 2 bước làm 1.

### 🔁 Tóm tắt thứ tự Cấp độ 1 và lý do

```
git init          ← tạo kho (1 lần)
tạo .gitignore    ← chặn rác/bí mật TRƯỚC khi lỡ add
   ┌───────────────── vòng lặp hằng ngày ─────────────────┐
   │ sửa code                                              │
   │ git status      ← xem đang có gì thay đổi            │
   │ git diff        ← tự review từng dòng                 │
   │ git add <file>  ← chọn thứ sẽ vào ảnh                 │
   │ git diff --staged ← (tuỳ chọn) kiểm tra lần cuối      │
   │ git commit -m   ← bấm máy chụp                        │
   │ git log --oneline ← xác nhận commit đã có             │
   └──────────────────────────────────────────────────────┘
```

### ✅ Bài tập 2 – Repo đầu tiên

**Mục tiêu:** Tạo repo, tạo 3 commit có ý nghĩa, hiểu rõ 3 khu vực.

```bash
# 1. Tạo repo
mkdir hoc-git && cd hoc-git
git init
git status                         # Quan sát: "No commits yet", không có gì

# 2. Tạo .gitignore trước tiên
echo "secret.txt" > .gitignore
echo "mat khau la 123" > secret.txt
git status                         # Quan sát: chỉ thấy .gitignore, KHÔNG thấy secret.txt → ignore hoạt động

# 3. Commit đầu tiên
git add .gitignore
git commit -m "Thêm .gitignore"

# 4. Tạo file và quan sát từng khu vực
echo "<h1>Xin chao</h1>" > index.html
git status                         # index.html: Untracked (đỏ)
git add index.html
git status                         # index.html: Changes to be committed (xanh)
git commit -m "Tạo trang chủ"

# 5. Sửa file, so sánh diff 2 kiểu
echo "<p>Toi la Nam Tron</p>" >> index.html
git diff                           # thấy dòng +<p>... 
git diff --staged                  # TRỐNG — vì chưa add
git add index.html
git diff                           # TRỐNG — vì đã add hết
git diff --staged                  # thấy dòng +<p>...
git commit -m "Thêm lời giới thiệu vào trang chủ"

# 6. Xem lịch sử
git log --oneline                  # thấy 3 commit
git show HEAD~1                    # xem commit "Tạo trang chủ"
```

**Câu hỏi tự kiểm tra:**
1. Ở bước 5, tại sao `git diff` và `git diff --staged` cho kết quả ngược nhau trước và sau `add`?
2. Nếu bạn `add index.html`, sau đó sửa tiếp `index.html` rồi `commit`, phần sửa sau có vào commit không? (Thử luôn để kiểm chứng!)
3. `secret.txt` có nằm trong commit nào không? Kiểm tra bằng `git show --stat HEAD~2`.

---

<a id="muc-3"></a>

## 3. Cấp độ 2 – Đưa code lên GitHub (remote)

> Tới giờ mọi thứ chỉ nằm trên máy. Máy hỏng = mất hết. Giờ ta đưa lên GitHub.

Có **2 kịch bản** — chọn đúng kịch bản quyết định thứ tự lệnh:

| Kịch bản | Bắt đầu từ | Lệnh chính |
|---|---|---|
| **A.** Dự án đã có trên GitHub (của bạn hoặc người khác) | GitHub | `git clone` |
| **B.** Dự án đang có trên máy, chưa có trên GitHub | Máy bạn | tạo repo rỗng trên GitHub → `git remote add` → `git push -u` |

### 3.1. `git clone` — tải repo từ GitHub về (Kịch bản A)

- **📌 Tác dụng:** Tải **toàn bộ** repo (code + toàn bộ lịch sử + các nhánh) về máy, tự động `init` và tự động gắn remote tên `origin`.
- **⌨️ Cú pháp:**
  ```bash
  git clone git@github.com:username/ten-repo.git            # dùng SSH (khuyên dùng)
  git clone https://github.com/username/ten-repo.git        # dùng HTTPS
  git clone git@github.com:username/ten-repo.git ten-khac   # đổi tên thư mục khi tải về
  git clone --depth 1 <url>                                 # chỉ tải commit mới nhất (repo khổng lồ, chỉ cần đọc code)
  ```
- **🔍 Lấy URL ở đâu?** Trang repo trên GitHub → nút xanh **<> Code** → tab **SSH** → copy.
- **🤔 Tại sao không cần `git init`?** `clone` = `init` + `remote add origin` + `fetch` + `checkout` gộp lại. Chạy `init` rồi mới `clone` là thừa (và tạo repo lồng repo).

### 3.2. `git remote` — quản lý "địa chỉ" kho trên mạng (Kịch bản B)

- **📌 Tác dụng:** Cho repo trên máy biết "kho trên GitHub nằm ở đâu".
- **⌨️ Cú pháp:**
  ```bash
  git remote -v                                         # liệt kê các remote (fetch/push URL)
  git remote add origin git@github.com:user/repo.git    # thêm remote tên "origin"
  git remote set-url origin <url-moi>                   # đổi URL (vd: đổi từ HTTPS sang SSH, hoặc repo đổi tên)
  git remote rename origin old                          # đổi tên remote
  git remote remove old                                 # xoá remote
  ```
- **🔍 Giải thích:**
  - `origin` chỉ là **tên quy ước** cho remote chính. Bạn có thể đặt tên khác nhưng đừng — mọi người đều dùng `origin`.
  - 1 repo có thể có nhiều remote (vd: `origin` = fork của bạn, `upstream` = repo gốc — xem [mục 6](#muc-6)).
- **🤔 Tại sao trước `push`?** `push` cần biết đẩy **đi đâu**. Chưa có remote → `fatal: 'origin' does not appear to be a git repository`.

### 3.3. `git push` — đẩy commit lên GitHub

- **📌 Tác dụng:** Gửi các commit **mà GitHub chưa có** từ nhánh trên máy lên nhánh tương ứng trên remote.
- **⌨️ Cú pháp:**
  ```bash
  git push -u origin main     # LẦN ĐẦU của mỗi nhánh: đẩy + "ghi nhớ" liên kết nhánh local main ↔ origin/main
  git push                    # các lần sau: Git đã nhớ đích, chỉ cần gõ thế này
  git push origin feature-x   # đẩy nhánh cụ thể
  git push origin --delete feature-x   # xoá nhánh trên GitHub
  git push --tags             # đẩy các tag (xem mục 8)
  ```
- **🔍 Giải thích `-u` (`--set-upstream`):** Thiết lập "nhánh theo dõi" (upstream). Sau đó `git push`, `git pull`, `git status` (dòng "Your branch is ahead of 'origin/main' by 2 commits") đều tự biết so với nhánh nào. **Chỉ cần `-u` một lần cho mỗi nhánh mới.**
- **🔍 Khi push bị từ chối:**
  ```
  ! [rejected]        main -> main (fetch first)
  error: failed to push some refs ...
  hint: Updates were rejected because the remote contains work that you do not have locally.
  ```
  → Trên GitHub có commit mà máy bạn chưa có (đồng đội vừa push, hoặc bạn sửa trên web). **Giải pháp: `git pull` trước, rồi `push` lại.** Đừng dùng `--force` (xem [mục 12](#muc-12)).
- **🤔 Tại sao sau `commit`?** Push gửi *commit*, không gửi file đang sửa. File chưa commit sẽ không bao giờ lên GitHub.

### 3.4. `git fetch` — tải về thông tin mới, CHƯA gộp

- **📌 Tác dụng:** Tải các commit/nhánh mới từ GitHub về và cập nhật `origin/main`, `origin/...`. **Không động vào code bạn đang làm.**
- **⌨️ Cú pháp:**
  ```bash
  git fetch                 # fetch từ origin
  git fetch --all --prune   # fetch mọi remote, đồng thời xoá các origin/xxx mà trên GitHub đã bị xoá
  git log --oneline main..origin/main    # sau fetch: xem GitHub có những commit nào mà mình chưa có
  git diff main origin/main              # xem chi tiết khác nhau
  ```
- **🤔 Tại sao có fetch khi đã có pull?** Fetch là "**nhìn trước khi nhảy**". An toàn tuyệt đối — bạn xem đồng đội đã làm gì rồi mới quyết định gộp.

### 3.5. `git pull` — tải về VÀ gộp vào nhánh hiện tại

- **📌 Tác dụng:** `git pull` = `git fetch` + `git merge origin/<nhánh-hiện-tại>`.
- **⌨️ Cú pháp:**
  ```bash
  git pull                 # fetch + merge
  git pull --rebase        # fetch + rebase: đặt commit của bạn lên trên commit mới của người khác → lịch sử thẳng (xem mục 8)
  git pull origin main     # kéo nhánh main của origin vào nhánh hiện tại
  ```
- **🔍 Lưu ý:** Nên **commit hoặc stash** thay đổi dở dang trước khi pull, nếu không Git có thể từ chối: `Your local changes would be overwritten by merge`.
- **🤔 Tại sao `pull` TRƯỚC khi bắt đầu làm việc và TRƯỚC `push`?**
  - **Đầu ngày:** pull để làm việc trên code mới nhất → giảm conflict.
  - **Trước push:** nếu GitHub có commit mới, push sẽ bị từ chối; pull để hợp nhất trước.

### 🔁 Quy trình chuẩn mỗi ngày (làm 1 mình)

```bash
git pull                     # 1. Lấy code mới nhất (có thể bạn sửa ở máy khác/trên web)
# ... code ...
git status                   # 2. Xem thay đổi
git add .                    # 3. Chọn
git commit -m "..."          # 4. Chốt (có thể nhiều commit trong ngày)
git pull                     # 5. Đồng bộ lần nữa trước khi đẩy
git push                     # 6. Đẩy lên
```

### ✅ Bài tập 3 – Đưa repo `hoc-git` lên GitHub (Kịch bản B)

1. Trên GitHub: nút **+** góc phải → **New repository** → tên `hoc-git` → **KHÔNG** tick "Add a README" (vì repo trên máy đã có lịch sử, tạo README trên web sẽ gây lệch) → **Create repository**.
2. Copy URL SSH mà GitHub hiển thị.
3. Trong thư mục `hoc-git` trên máy:
   ```bash
   git remote add origin git@github.com:<username>/hoc-git.git
   git remote -v                 # kiểm tra
   git branch -M main            # đảm bảo nhánh tên main (-M = đổi tên ép buộc)
   git push -u origin main
   ```
4. F5 trang GitHub → thấy file và 3 commit. Kiểm tra `secret.txt` **không** có trên GitHub.
5. **Thử "pull":** trên web GitHub, bấm vào `index.html` → biểu tượng ✏️ → thêm 1 dòng → **Commit changes**. Về máy:
   ```bash
   git status            # vẫn báo "up to date" — vì máy CHƯA hỏi GitHub
   git fetch
   git status            # giờ báo "behind 'origin/main' by 1 commit"
   git log --oneline main..origin/main   # xem commit đó
   git pull              # gộp vào
   cat index.html        # thấy dòng vừa sửa trên web
   ```
   👉 Bài học: `status` chỉ so với **bản sao `origin/main` trên máy**, phải `fetch` mới cập nhật.
6. **Thử Kịch bản A:** sang thư mục khác, `git clone` chính repo đó với tên `hoc-git-may-2`, sửa 1 file, commit, push. Quay lại thư mục `hoc-git` gốc, `git pull` để nhận thay đổi. Bạn vừa mô phỏng làm việc trên 2 máy!

---

<a id="muc-4"></a>

## 4. Cấp độ 3 – Nhánh (branch) và gộp nhánh (merge)

### 4.1. Nhánh là gì, tại sao cần?

```
                    C4 ── C5      ← nhánh "feature-login" (bạn đang làm tính năng đăng nhập)
                   /
 C1 ── C2 ── C3 ──────── C6       ← nhánh "main" (luôn chạy được, đồng đội fix bug khẩn ở đây)
```

- `main` phải **luôn ổn định, chạy được** (vì nó được deploy lên web thật).
- Mỗi tính năng/bug làm trên **nhánh riêng** → làm hỏng thoải mái, không ảnh hưởng ai.
- Xong và test ổn → **merge** về `main`.
- Về kỹ thuật, nhánh chỉ là **một file nhỏ chứa mã hash của 1 commit** → tạo nhánh tốn 0 giây, 0 dung lượng. Đừng ngại tạo nhánh.

### 4.2. `git branch` — liệt kê, tạo, xoá nhánh

```bash
git branch                    # liệt kê nhánh local, dấu * là nhánh hiện tại
git branch -a                 # cả nhánh remote (remotes/origin/...)
git branch -v                 # kèm commit cuối của mỗi nhánh
git branch feature-login      # TẠO nhánh mới (nhưng VẪN ĐỨNG ở nhánh cũ!)
git branch -d feature-login   # xoá nhánh (chỉ cho xoá nếu đã merge — an toàn)
git branch -D feature-login   # xoá ÉP BUỘC (kể cả chưa merge — mất commit nếu không ở đâu khác)
git branch -m ten-cu ten-moi  # đổi tên nhánh
git branch --merged           # nhánh nào đã gộp vào nhánh hiện tại (xoá được an toàn)
```

### 4.3. `git switch` (và `git checkout`) — chuyển nhánh

- **📌 Tác dụng:** Di chuyển HEAD sang nhánh khác; các file trong thư mục làm việc thay đổi theo nhánh đó.
- **⌨️ Cú pháp:**
  ```bash
  git switch main                 # chuyển sang main
  git switch -c feature-login     # TẠO + CHUYỂN luôn (dùng nhiều nhất)
  git switch -                    # quay về nhánh vừa ở trước đó

  # Cách cũ (vẫn gặp rất nhiều trong tài liệu/StackOverflow):
  git checkout main
  git checkout -b feature-login
  ```
- **🔍 Tại sao có 2 lệnh?** `checkout` cũ làm quá nhiều việc (chuyển nhánh + khôi phục file + ...) dễ gây nhầm. Từ Git 2.23, tách thành `switch` (chuyển nhánh) và `restore` (khôi phục file). Dùng `switch` cho rõ nghĩa, nhưng phải **đọc hiểu được** `checkout`.
- **⚠️ Lưu ý:** Nếu có thay đổi chưa commit mà xung đột với nhánh đích, Git chặn lại. Hãy `commit` hoặc `stash` trước.
- **🤔 Tại sao `switch -c` TRƯỚC khi code tính năng mới?** Nếu bạn quên, code sẽ nằm trên `main`. (Vẫn cứu được: chưa commit thì cứ `git switch -c ten-nhanh` — thay đổi đi theo bạn sang nhánh mới.)

### 4.4. `git merge` — gộp nhánh

- **📌 Tác dụng:** Đưa các commit của nhánh khác **vào nhánh hiện tại**.
- **⌨️ Cú pháp:**
  ```bash
  git switch main              # 1. ĐỨNG Ở NHÁNH NHẬN (đích)
  git pull                     # 2. cập nhật main mới nhất
  git merge feature-login      # 3. kéo feature-login VÀO main
  git branch -d feature-login  # 4. dọn dẹp
  git push                     # 5. đẩy main lên
  ```
- **🔍 Quy tắc ghi nhớ:** "**Đứng ở đâu, gộp vào đó.**" `git merge X` = "lấy X đổ vào chỗ tôi đang đứng".
- **🔍 Hai kiểu merge:**

  **a) Fast-forward** — `main` không có commit mới kể từ lúc tách nhánh → Git chỉ việc "kéo con trỏ" `main` lên, không tạo commit mới:
  ```
  Trước:  C1 ── C2 (main)            Sau:   C1 ── C2 ── C3 ── C4 (main, feature)
                  \
                   C3 ── C4 (feature)
  ```
  **b) Three-way merge** — cả 2 nhánh đều có commit mới → Git tạo **merge commit** (có 2 cha):
  ```
  Trước:  C1 ── C2 ── C5 (main)      Sau:   C1 ── C2 ── C5 ────── M (main)
                  \                                \              /
                   C3 ── C4 (feature)               C3 ── C4 ────
  ```
  ```bash
  git merge --no-ff feature    # ép tạo merge commit kể cả khi fast-forward được (giữ dấu vết "đã có 1 nhánh tính năng")
  git merge --squash feature   # nén mọi commit của feature thành 1 thay đổi trong staging, rồi bạn tự commit
  git merge --abort            # huỷ merge đang dở (khi gặp conflict và muốn quay lại như cũ)
  ```
- **🤔 Tại sao phải `switch main` + `pull` TRƯỚC `merge`?** Merge gộp vào nhánh **đang đứng** → phải đứng đúng chỗ. Pull trước để gộp vào bản mới nhất, tránh push xong lại bị từ chối.

### 4.5. Đẩy nhánh lên GitHub

```bash
git switch -c feature-login
# ... commit ...
git push -u origin feature-login     # lần đầu của nhánh này cần -u
```
Thực tế đi làm, bạn **không tự merge vào main trên máy**, mà push nhánh lên rồi tạo **Pull Request** trên GitHub để đồng đội review → xem [mục 6](#muc-6).

### ✅ Bài tập 4 – Làm 2 tính năng song song

```bash
cd hoc-git
git switch -c feature-about
echo "<h2>Ve toi</h2>" > about.html
git add about.html && git commit -m "Tạo trang Về tôi"

git switch main
ls                                  # QUAN SÁT: about.html BIẾN MẤT! (vì nó chỉ có ở nhánh feature-about)

git switch -c feature-contact
echo "<h2>Lien he</h2>" > contact.html
git add contact.html && git commit -m "Tạo trang Liên hệ"

git lg                              # alias đã tạo ở mục 2.6 — thấy 2 nhánh tách ra từ main

git switch main
git merge feature-about             # Fast-forward
git merge feature-contact           # Tạo merge commit (mở editor → lưu & đóng để chấp nhận message mặc định)
git lg                              # thấy hình chữ "Y" hội tụ lại

git branch -d feature-about feature-contact
git push
```
**Câu hỏi:** Tại sao merge thứ nhất là fast-forward nhưng merge thứ hai thì không?

---

<a id="muc-5"></a>

## 5. Cấp độ 4 – Xử lý xung đột (conflict)

### 5.1. Conflict xảy ra khi nào?

Khi 2 nhánh **cùng sửa cùng một dòng** (hoặc 1 bên sửa, 1 bên xoá file) và bạn merge/pull/rebase. Git không biết nên giữ bên nào → dừng lại hỏi bạn.

**Conflict KHÔNG phải lỗi**, nó là chuyện bình thường hằng ngày khi làm nhóm. Đừng hoảng.

### 5.2. Nhận diện conflict

```
$ git merge feature-x
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```
```bash
git status       # "both modified: index.html" → đây là file cần xử lý
```
Mở file, bạn sẽ thấy:
```html
<<<<<<< HEAD
<h1>Chào mừng đến shop Nam Tròn</h1>
=======
<h1>Welcome to Nam Tron Shop</h1>
>>>>>>> feature-x
```
- Từ `<<<<<<< HEAD` đến `=======`: phiên bản của **nhánh bạn đang đứng**.
- Từ `=======` đến `>>>>>>> feature-x`: phiên bản của **nhánh đang được gộp vào**.

### 5.3. Các bước giải quyết — làm đúng thứ tự

```bash
# Bước 1: Xem những file nào bị conflict
git status

# Bước 2: Mở từng file, quyết định nội dung cuối cùng:
#   - giữ bên trên, hoặc bên dưới, hoặc kết hợp cả hai, hoặc viết lại
#   - XOÁ HẾT 3 dòng đánh dấu <<<<<<<  =======  >>>>>>>
#   (VS Code hiện nút "Accept Current / Accept Incoming / Accept Both" rất tiện)

# Bước 3: Đánh dấu "đã giải quyết" bằng cách add
git add index.html

# Bước 4: Kiểm tra không còn file nào "both modified"
git status

# Bước 5: Hoàn tất merge
git commit            # Git đã soạn sẵn message "Merge branch ..." → lưu & đóng

# Muốn bỏ cuộc, quay về như trước khi merge:
git merge --abort
```
- **🤔 Tại sao phải `add` sau khi sửa?** Với Git, `add` file đang conflict nghĩa là "*tôi đã xử lý xong file này*". Chưa add → Git coi như chưa giải quyết và không cho commit.
- **🤔 Tại sao phải chạy app/test lại trước khi commit?** Code hết dấu `<<<<` chưa chắc đã chạy đúng — hai thay đổi có thể không tương thích về logic.

### 5.4. Mẹo giảm conflict
- `git pull` thường xuyên (mỗi sáng, trước khi tạo nhánh mới).
- Nhánh sống ngắn: vài ngày là merge, đừng để 1 tháng.
- Chia nhỏ việc, mỗi người phụ trách file/khu vực khác nhau.
- Thống nhất formatter (Prettier, ...) để không bị conflict vì khoảng trắng.

### ✅ Bài tập 5 – Tự tạo và tự giải quyết conflict

```bash
cd hoc-git
git switch main
echo "Tieu de: Shop Nam Tron" > title.txt
git add title.txt && git commit -m "Thêm tiêu đề"

git switch -c tieng-anh
echo "Tieu de: Nam Tron Shop" > title.txt
git commit -am "Đổi tiêu đề sang tiếng Anh"

git switch main
echo "Tieu de: Cua hang Nam Tron" > title.txt
git commit -am "Đổi tiêu đề đầy đủ hơn"

git merge tieng-anh          # 💥 CONFLICT
git status
# Mở title.txt, sửa thành: "Tieu de: Cua hang Nam Tron (Nam Tron Shop)" và xoá các dấu đánh dấu
git add title.txt
git commit
git lg
```
**Làm thêm:** Lặp lại bài tập nhưng lần này dùng `git merge --abort` khi gặp conflict, rồi kiểm tra `git status` & `cat title.txt` xem mọi thứ đã về như cũ chưa.

---

<a id="muc-6"></a>

## 6. Cấp độ 5 – Quy trình làm việc trên GitHub: Fork, Pull Request, Issue

### 6.1. Issue — "phiếu công việc"

- Tab **Issues** trên repo: nơi ghi lỗi (bug), đề xuất tính năng, câu hỏi.
- Mỗi issue có số, ví dụ `#12`. Commit hoặc PR ghi `Fixes #12` → khi PR được merge vào nhánh mặc định, issue #12 **tự đóng**.
- Dùng **Labels** (bug, enhancement…), **Assignees** (ai làm), **Milestones** (thuộc đợt phát hành nào).
- **🤔 Tại sao issue đứng đầu quy trình?** Trong nhóm, ai cũng phải biết "việc này có người làm chưa, làm vì sao". Issue là nơi thảo luận *trước* khi viết code.

### 6.2. Pull Request (PR) — "xin phép gộp code"

PR là trái tim của làm việc nhóm trên GitHub: bạn push nhánh lên, tạo PR, đồng đội **xem diff, bình luận từng dòng, yêu cầu sửa, approve**, rồi mới merge.

**Quy trình đầy đủ (repo của nhóm, bạn có quyền push):**
```bash
# 1. Bắt đầu từ main mới nhất
git switch main
git pull

# 2. Tạo nhánh theo issue
git switch -c feature/12-trang-dang-nhap

# 3. Code + commit (nhiều lần)
git add .
git commit -m "feat: thêm form đăng nhập"

# 4. Trước khi push, cập nhật main mới để giảm conflict (tuỳ chọn nhưng nên làm)
git fetch
git merge origin/main          # hoặc: git rebase origin/main (xem mục 8)

# 5. Đẩy nhánh lên
git push -u origin feature/12-trang-dang-nhap
```
6. Lên GitHub → hiện banner vàng **"Compare & pull request"** → bấm.
7. Điền:
   - **base:** `main` ← **compare:** `feature/12-trang-dang-nhap`
   - **Title:** ngắn gọn, rõ ràng.
   - **Description:** làm gì, tại sao, test thế nào, ảnh chụp màn hình nếu là UI, và `Closes #12`.
   - **Reviewers:** chọn đồng đội.
8. Reviewer góp ý → bạn sửa trên **cùng nhánh** ở máy → `commit` → `push`. PR **tự cập nhật**, không cần tạo PR mới.
9. Được approve → bấm **Merge**. GitHub có 3 lựa chọn:

| Kiểu merge | Kết quả | Khi nào dùng |
|---|---|---|
| **Create a merge commit** | Giữ nguyên mọi commit + 1 merge commit | Muốn giữ chi tiết lịch sử |
| **Squash and merge** | Nén mọi commit của PR thành **1 commit** trên main | Phổ biến nhất — main sạch, mỗi PR = 1 commit |
| **Rebase and merge** | Đặt lại từng commit lên đầu main, không merge commit | Lịch sử thẳng, các commit đều đã sạch đẹp |

10. Dọn dẹp:
```bash
git switch main
git pull                                          # nhận code vừa merge
git branch -d feature/12-trang-dang-nhap          # xoá nhánh local (nếu squash merge, cần -D vì Git không nhận ra đã merge)
git fetch --prune                                 # xoá tham chiếu origin/... của nhánh đã bị xoá trên GitHub
```

**🤔 Tại sao không merge thẳng vào main mà phải qua PR?**
- Có người thứ 2 đọc code → bắt lỗi sớm, chia sẻ kiến thức.
- CI (GitHub Actions) tự chạy test trên PR → không merge code hỏng.
- Lưu lại *lý do* thay đổi gắn với code mãi mãi.

### 6.3. Fork — đóng góp vào repo KHÔNG phải của bạn (open source)

Bạn không có quyền push vào repo của người lạ. Cách làm: **fork** (sao chép thành repo của bạn) → sửa trên fork → tạo PR từ fork về repo gốc.

```
 Repo gốc (upstream)                Fork của bạn (origin)               Máy bạn
 github.com/tacgia/project  ──fork──▶ github.com/namtron/project ──clone──▶ ~/project
          ▲                                     ▲                            │
          │          Pull Request               │          git push          │
          └─────────────────────────────────────┴────────────────────────────┘
```

```bash
# 1. Trên GitHub repo gốc: bấm "Fork" (góc phải trên)

# 2. Clone FORK CỦA BẠN (không phải repo gốc)
git clone git@github.com:namtron/project.git
cd project

# 3. Thêm repo gốc làm remote thứ 2, đặt tên "upstream"
git remote add upstream https://github.com/tacgia/project.git
git remote -v      # origin = fork của bạn, upstream = repo gốc

# 4. Tạo nhánh, sửa, commit
git switch -c fix-typo-readme
# ... sửa ...
git commit -am "docs: sửa lỗi chính tả trong README"

# 5. Push lên FORK (origin)
git push -u origin fix-typo-readme

# 6. Lên GitHub → "Compare & pull request" → base repository: tacgia/project, base: main
```

**Giữ fork luôn cập nhật với repo gốc:**
```bash
git switch main
git fetch upstream
git merge upstream/main      # hoặc rebase
git push origin main         # cập nhật fork trên GitHub
```
(Hoặc bấm nút **"Sync fork"** trên trang fork của bạn.)

**🤔 Tại sao cần remote `upstream`?** Repo gốc liên tục có commit mới từ người khác. Không có `upstream`, fork của bạn sẽ ngày càng cũ và PR sẽ conflict.

### 6.4. Các tính năng GitHub khác nên biết

| Tính năng | Dùng để |
|---|---|
| **README.md** | "Mặt tiền" của repo: giới thiệu, cách cài, cách chạy, ảnh demo |
| **Branch protection rules** (Settings → Branches) | Cấm push thẳng vào main, bắt buộc PR + 1 approve + CI xanh |
| **Releases** | Phát hành phiên bản (gắn với tag), kèm changelog, file đính kèm |
| **GitHub Pages** | Host web tĩnh miễn phí (HTML/CSS/JS) tại `username.github.io/repo` |
| **Projects** | Bảng Kanban quản lý issue/PR |
| **Actions** | Tự động hoá: chạy test, build, deploy (xem [mục 9](#muc-9)) |
| **Secrets** (Settings → Secrets) | Lưu mật khẩu/API key cho Actions, không lộ trong code |
| **Profile README** | Repo trùng tên username → README hiện ở trang cá nhân — "CV" của dev |

### ✅ Bài tập 6 – Mô phỏng làm nhóm bằng PR

1. Trên GitHub repo `hoc-git`: tạo **Issue** "Thêm trang FAQ" → ghi nhớ số issue (vd `#1`).
2. Trên máy:
   ```bash
   git switch main && git pull
   git switch -c feature/1-faq
   echo "<h2>Cau hoi thuong gap</h2>" > faq.html
   git add faq.html && git commit -m "feat: thêm trang FAQ"
   git push -u origin feature/1-faq
   ```
3. Tạo PR, description ghi `Closes #1`.
4. Trong tab **Files changed** của PR, bấm dấu **+** cạnh 1 dòng để tự comment góp ý (giả làm reviewer).
5. "Sửa theo góp ý": thêm 1 dòng vào `faq.html`, commit, push → quan sát PR tự cập nhật.
6. **Squash and merge** → kiểm tra issue #1 đã tự đóng.
7. Dọn dẹp ở máy (`switch main`, `pull`, `branch -D`, `fetch --prune`).
8. **Nâng cao:** vào Settings → Branches → thêm rule cho `main`: "Require a pull request before merging". Thử `git push` thẳng lên main → bị từ chối.
9. **Open source thật:** fork https://github.com/firstcontributions/first-contributions và làm theo README của nó (repo được tạo ra chính để người mới tập tạo PR đầu tiên).

---

<a id="muc-7"></a>

## 7. Cấp độ 6 – Quay lui, sửa sai, cứu dữ liệu

> Phần đáng sợ nhất với người mới, nhưng thật ra Git **rất khó làm mất dữ liệu thật sự** nếu bạn đã commit. Hãy học kỹ.

### 7.0. Bảng chọn lệnh — "Tôi muốn huỷ cái gì?"

| Tình huống | Lệnh | Mức nguy hiểm |
|---|---|---|
| Sửa file lung tung, muốn về như commit cuối (chưa add) | `git restore <file>` | ⚠️ mất phần sửa đó |
| Lỡ `add` nhầm file, muốn bỏ ra khỏi staging (giữ nội dung) | `git restore --staged <file>` | ✅ an toàn |
| Vừa commit, quên add 1 file / gõ sai message | `git commit --amend` | ⚠️ chưa push thì ok |
| Muốn "bỏ" vài commit cuối (CHƯA push), giữ code để làm lại | `git reset --soft/--mixed HEAD~n` | ✅ an toàn |
| Muốn xoá sạch vài commit cuối (CHƯA push) và mọi thay đổi | `git reset --hard HEAD~n` | 🔥 nguy hiểm |
| Commit ĐÃ PUSH bị lỗi, muốn huỷ tác dụng | `git revert <hash>` | ✅ an toàn, dùng cho nhánh chung |
| Lỡ reset --hard / xoá nhánh, muốn cứu | `git reflog` | 🚑 cứu hộ |
| Xoá file rác chưa được theo dõi | `git clean -fd` | 🔥 không cứu được |

### 7.1. `git restore` — khôi phục file

```bash
git restore index.html              # bỏ mọi sửa đổi CHƯA add của file, trở về bản trong staging/commit cuối
git restore .                       # cho mọi file (cẩn thận!)
git restore --staged index.html     # bỏ file khỏi staging, NỘI DUNG SỬA VẪN GIỮ (ngược lại của git add)
git restore --source=HEAD~3 app.js  # lấy phiên bản app.js của 3 commit trước
```
- Cách cũ tương đương: `git checkout -- index.html` và `git reset HEAD index.html`.
- ⚠️ `git restore <file>` xoá phần sửa **chưa từng commit** → không có cách cứu (Git chưa bao giờ lưu nó).

### 7.2. `git commit --amend` — sửa commit vừa tạo

```bash
git commit --amend -m "Message mới đúng chính tả"    # sửa message
git add file-quen.txt
git commit --amend --no-edit                         # nhét thêm file vào commit cuối, giữ message
```
- **🔍 Thực chất:** amend **không sửa** commit cũ mà tạo **commit mới thay thế** (hash khác).
- **⚠️ Quy tắc:** Chỉ amend commit **CHƯA push**. Nếu đã push, amend xong push sẽ bị từ chối vì lịch sử khác GitHub → phải force push → làm khổ đồng đội.

### 7.3. `git reset` — di chuyển nhánh lùi về commit cũ

Giả sử lịch sử: `A ── B ── C (HEAD, main)`, chạy `git reset <mode> B`:

| Mode | Nhánh trỏ về B | Staging | Working dir | Ý nghĩa thực tế |
|---|---|---|---|---|
| `--soft` | ✅ | **giữ** thay đổi của C (đã add sẵn) | giữ | "Huỷ commit, nhưng giữ nguyên để commit lại" — gộp nhiều commit thành 1 |
| `--mixed` (mặc định) | ✅ | bị xoá (chuyển về chưa add) | **giữ** | "Huỷ commit và add, code vẫn còn" |
| `--hard` | ✅ | xoá | **xoá** | "Coi như C chưa từng tồn tại" 🔥 |

```bash
git reset --soft HEAD~1     # bỏ commit cuối, thay đổi vẫn ở staging
git reset HEAD~2            # bỏ 2 commit cuối, thay đổi về working dir
git reset --hard HEAD~1     # bỏ commit cuối VÀ mọi thay đổi của nó
git reset --hard origin/main  # vứt hết mọi thứ local, làm cho nhánh giống hệt GitHub
```
- **🤔 Ví dụ dùng `--soft`:** bạn có 5 commit "wip", "wip2", "fix nhỏ"... muốn gộp thành 1 commit đẹp trước khi push: `git reset --soft HEAD~5` rồi `git commit -m "feat: ..."`.
- **⚠️ Quy tắc vàng:** **KHÔNG reset commit đã push lên nhánh dùng chung.** Lịch sử của bạn sẽ khác của mọi người. Dùng `revert`.

### 7.4. `git revert` — huỷ một commit bằng cách tạo commit "ngược lại"

```bash
git revert a1b2c3d          # tạo commit mới đảo ngược mọi thay đổi của a1b2c3d
git revert HEAD             # đảo ngược commit cuối
git revert --no-edit HEAD   # không mở editor
```
```
Trước: A ── B ── C(lỗi) ── D
Sau:   A ── B ── C(lỗi) ── D ── C'(huỷ C)     ← lịch sử chỉ TIẾN, không bị viết lại
```
- **🤔 Tại sao revert an toàn hơn reset cho code đã push?** Revert **thêm** commit mới, không xoá gì → đồng đội chỉ cần `pull` bình thường. Reset **viết lại** lịch sử → đồng đội bị lệch.
- Trên GitHub, PR đã merge có nút **Revert** tạo sẵn PR đảo ngược.

### 7.5. `git reflog` — hộp đen máy bay 🚑

- **📌 Tác dụng:** Ghi lại **mọi vị trí HEAD từng đi qua** trên máy bạn (commit, reset, switch, merge, rebase…), giữ khoảng 90 ngày. Kể cả commit "đã mất" vì `reset --hard` hay xoá nhánh vẫn còn ở đây.
```bash
git reflog
# a1b2c3d HEAD@{0}: reset: moving to HEAD~3
# f9e8d7c HEAD@{1}: commit: Thêm thanh toán        ← commit bạn tưởng đã mất
# ...

git reset --hard f9e8d7c           # quay lại đó
# hoặc an toàn hơn: tạo nhánh mới từ commit đó
git branch cuu-ho f9e8d7c
```
- **🔍 Lưu ý:** reflog chỉ có ở máy local và chỉ cứu được thứ **đã từng được commit**.

### 7.6. `git clean` — xoá file chưa được theo dõi

```bash
git clean -n      # CHẠY THỬ: liệt kê những gì SẼ bị xoá (luôn chạy cái này trước!)
git clean -fd     # xoá thật file (-f) và thư mục (-d) untracked
git clean -fdx    # xoá luôn cả file trong .gitignore (vd: node_modules, build) 
```
- **🤔 Tại sao luôn `-n` trước `-f`?** File untracked chưa từng vào Git → xoá là mất vĩnh viễn, reflog cũng không cứu được.

### ✅ Bài tập 7 – Phá rồi cứu

```bash
cd hoc-git
# a) restore
echo "rac rac" >> index.html
git restore index.html && git status        # sạch

# b) restore --staged
echo "test" > tam.txt && git add tam.txt
git restore --staged tam.txt && git status  # tam.txt quay về Untracked

# c) amend
git add tam.txt && git commit -m "thme file tam"     # cố tình gõ sai
git commit --amend -m "Thêm file tạm" && git log --oneline -1

# d) reset --soft gộp commit
echo 1 > a.txt && git add a.txt && git commit -m "wip 1"
echo 2 > b.txt && git add b.txt && git commit -m "wip 2"
git reset --soft HEAD~2 && git status       # a.txt, b.txt vẫn đang staged
git commit -m "Thêm a.txt và b.txt"

# e) reset --hard rồi cứu bằng reflog
git reset --hard HEAD~1
ls                                          # a.txt, b.txt đã biến mất 😱
git reflog                                  # tìm dòng "commit: Thêm a.txt và b.txt"
git reset --hard <hash-tìm-được>
ls                                          # đã trở lại 🎉

# f) revert
git revert --no-edit HEAD
git log --oneline -3                        # có commit "Revert ..."; a.txt, b.txt bị xoá bằng commit mới
```

---

<a id="muc-8"></a>

## 8. Cấp độ 7 – Kỹ thuật nâng cao

### 8.1. `git stash` — cất tạm việc đang dở

**Tình huống:** Đang code dở tính năng, sếp gọi "fix bug gấp trên main!". Code dở chưa muốn commit, nhưng phải chuyển nhánh.

```bash
git stash                         # cất mọi thay đổi (đã/chưa add) vào "ngăn kéo", thư mục trở nên sạch
git stash push -m "dang lam form login"   # cất kèm tên cho dễ nhớ
git stash -u                      # cất cả file untracked (mặc định stash bỏ qua file mới)
git stash list                    # stash@{0}: On feature: dang lam form login
git switch main                   # ... đi fix bug, commit, push ...
git switch feature-login
git stash pop                     # lấy ra lại và XOÁ khỏi ngăn kéo
git stash apply stash@{1}         # lấy ra nhưng VẪN GIỮ trong ngăn kéo
git stash drop stash@{0}          # xoá 1 stash
git stash show -p stash@{0}       # xem nội dung stash
```
- **🤔 Tại sao stash TRƯỚC switch/pull?** Git từ chối chuyển nhánh/pull nếu thay đổi dở dang bị ghi đè. Stash dọn đường.
- **Thay thế:** Nhiều người thích commit tạm `git commit -m "WIP"` rồi sau đó `reset --soft HEAD~1` — cũng hợp lệ.

### 8.2. `git rebase` — "trồng lại" nhánh lên gốc mới

**Vấn đề:** Bạn tách nhánh `feature` từ `main` lúc C2. Trong lúc đó main có thêm C5, C6. Merge sẽ tạo merge commit, lịch sử nhiều "hình thoi" rối.

```
Trước rebase:                         Sau  git switch feature && git rebase main:
        C3 ── C4 (feature)                                    C3' ── C4' (feature)
       /                                                     /
C1 ── C2 ── C5 ── C6 (main)           C1 ── C2 ── C5 ── C6 (main)
```
- **📌 Tác dụng:** Lấy từng commit của nhánh hiện tại, "chơi lại" chúng lên đầu nhánh đích. Kết quả: lịch sử **thẳng tắp** như thể bạn bắt đầu làm sau C6.
```bash
git switch feature
git rebase main              # đặt feature lên đầu main
# Nếu conflict: sửa file → git add <file> → git rebase --continue
# Bỏ qua commit đang lỗi: git rebase --skip
# Huỷ toàn bộ: git rebase --abort

git pull --rebase            # pull kiểu rebase: commit local của bạn được đặt lên trên commit mới từ GitHub
```
- **🔍 Lưu ý:** C3', C4' là commit **mới** (hash khác) → rebase **viết lại lịch sử**.
- **⚠️ QUY TẮC VÀNG CỦA REBASE:** *Không bao giờ rebase những commit đã push lên nhánh mà người khác đang dùng.* Rebase nhánh feature **của riêng bạn** thì ok (sau đó cần `git push --force-with-lease`).

**Merge hay Rebase?**

| | Merge | Rebase |
|---|---|---|
| Lịch sử | Giữ nguyên sự thật, có merge commit | Thẳng, gọn |
| An toàn | Rất an toàn | Viết lại lịch sử, cần cẩn thận |
| Conflict | Giải quyết 1 lần | Có thể phải giải quyết ở từng commit |
| Người mới | ✅ Dùng cái này trước | Học sau khi đã vững merge |

### 8.3. `git rebase -i` — dọn dẹp lịch sử trước khi tạo PR

```bash
git rebase -i HEAD~4         # mở editor liệt kê 4 commit cuối (cũ ở trên, mới ở dưới)
```
```
pick a1 feat: thêm form đăng nhập
pick b2 wip
pick c3 sửa typo
pick d4 feat: validate email
```
Đổi chữ đầu mỗi dòng:
| Lệnh | Tác dụng |
|---|---|
| `pick` | giữ nguyên |
| `reword` (r) | giữ nhưng sửa message |
| `squash` (s) | gộp vào commit phía trên, gộp luôn message |
| `fixup` (f) | gộp vào commit phía trên, bỏ message của nó |
| `drop` (d) | xoá commit |
| đổi thứ tự dòng | đổi thứ tự commit |

Ví dụ đổi `b2` và `c3` thành `fixup` → còn 2 commit đẹp. Lưu & đóng editor.
- **🤔 Khi nào dùng?** Trước khi push/tạo PR, để reviewer đọc lịch sử dễ hiểu. Chỉ với commit **chưa push** hoặc trên nhánh riêng.
- ⚠️ Lưu ý: Một số terminal tích hợp không hỗ trợ chế độ tương tác tốt; dùng Git Bash với `core.editor` đã cấu hình VS Code.

### 8.4. `git push --force-with-lease` — đẩy ép một cách "lịch sự"

Sau khi rebase/amend nhánh **riêng** đã push, `git push` bị từ chối vì lịch sử khác. Khi đó:
```bash
git push --force-with-lease     # chỉ ép nếu trên GitHub không có ai push thêm gì mà bạn chưa thấy
git push --force                # ép bất chấp — có thể XOÁ commit của đồng đội 🔥 tránh dùng
```
- **Không bao giờ force push lên `main`/nhánh chung.**

### 8.5. `git cherry-pick` — nhặt đúng 1 commit từ nhánh khác

**Tình huống:** Trên nhánh `develop` có commit `e5f6` fix bug bảo mật, cần đưa ngay lên `main` mà không mang theo các tính năng dở dang khác của `develop`.
```bash
git switch main
git cherry-pick e5f6               # copy commit e5f6 thành 1 commit mới trên main
git cherry-pick a1..d4             # nhiều commit (từ sau a1 đến d4)
git cherry-pick --no-commit e5f6   # chỉ áp thay đổi, chưa commit
# Conflict: sửa → git add → git cherry-pick --continue  (hoặc --abort)
```

### 8.6. `git tag` — đánh dấu phiên bản phát hành

```bash
git tag                                    # liệt kê tag
git tag -a v1.0.0 -m "Phát hành bản đầu"   # tag có chú thích (khuyên dùng cho release)
git tag v1.0.1 a1b2c3d                     # tag nhẹ vào commit cũ
git push origin v1.0.0                     # tag KHÔNG tự push theo commit — phải push riêng
git push --tags                            # đẩy mọi tag
git tag -d v1.0.1                          # xoá tag local
git push origin --delete v1.0.1            # xoá tag trên GitHub
git switch --detach v1.0.0                 # xem code tại phiên bản đó
```
- Quy ước **Semantic Versioning**: `vMAJOR.MINOR.PATCH` — tăng MAJOR khi thay đổi phá vỡ tương thích, MINOR khi thêm tính năng, PATCH khi sửa lỗi.
- Trên GitHub: tab **Releases** → **Draft a new release** → chọn tag → viết changelog.

### 8.7. `git blame` — dòng này ai viết, khi nào?

```bash
git blame index.html                # mỗi dòng kèm hash, tác giả, ngày
git blame -L 10,20 index.html       # chỉ dòng 10–20
```
- Mục đích không phải "đổ lỗi" mà để tìm **commit và lý do** dòng đó tồn tại (`git show <hash>`). VS Code + extension **GitLens** hiển thị blame ngay trong editor.

### 8.8. `git bisect` — tìm commit gây lỗi bằng tìm kiếm nhị phân

**Tình huống:** Hôm nay web lỗi, tuần trước (v1.0.0) vẫn chạy. Ở giữa có 100 commit. Bisect giúp tìm ra commit thủ phạm trong ~7 bước (log₂100).
```bash
git bisect start
git bisect bad                  # commit hiện tại bị lỗi
git bisect good v1.0.0          # commit này còn tốt
# Git tự checkout commit ở giữa → bạn chạy thử app
git bisect good                 # nếu commit này ổn
git bisect bad                  # nếu commit này lỗi
# ... lặp lại đến khi Git báo: "abc123 is the first bad commit"
git bisect reset                # quay về nhánh ban đầu

# Tự động hoá nếu có script test (exit 0 = good, khác 0 = bad):
git bisect run npm test
```

### 8.9. `git worktree` — làm 2 nhánh cùng lúc ở 2 thư mục

```bash
git worktree add ../hotfix-folder main     # mở nhánh main ở thư mục khác, KHÔNG cần stash việc đang dở
git worktree list
git worktree remove ../hotfix-folder
```
- Thay thế cho stash khi việc dở dang quá lớn, hoặc khi cần chạy song song 2 phiên bản để so sánh.

### 8.10. Một số lệnh khác

```bash
git shortlog -sn                 # thống kê số commit theo tác giả
git log -S "tenHam"              # tìm commit thêm/xoá chuỗi "tenHam" (cực hữu ích khi tìm ai xoá hàm)
git grep "TODO"                  # tìm chuỗi trong các file được Git theo dõi
git switch --detach a1b2c3d      # xem code ở commit cũ (trạng thái "detached HEAD" — đừng commit ở đây, hoặc tạo nhánh trước)
git submodule add <url> libs/x   # nhúng repo khác vào repo này (ít dùng, phức tạp, biết để đọc hiểu)
git archive -o code.zip HEAD     # xuất code thành file zip, không kèm .git
```

### ✅ Bài tập 8 – Nâng cao

1. **Stash:** sửa `index.html` (đừng commit) → `git stash push -m "dang sua"` → `git switch -c hotfix` → sửa `about.html`, commit → `git switch main` → `git merge hotfix` → quay lại và `git stash pop`.
2. **Rebase:** tạo nhánh `feature-rebase` từ main, commit 2 lần. Quay lại main commit 1 lần. Về `feature-rebase` chạy `git rebase main`. So sánh `git lg` trước/sau (chụp lại).
3. **Interactive rebase:** trên 1 nhánh mới, tạo 4 commit "wip". Dùng `git rebase -i HEAD~4` gộp thành 1 commit message "feat: ...".
4. **Cherry-pick:** trên nhánh `thu-nghiem` tạo 3 commit; chỉ cherry-pick commit thứ 2 vào main.
5. **Tag & Release:** tạo tag `v1.0.0` cho main, push tag, tạo Release trên GitHub.
6. **Bisect:** chạy script tạo 20 commit, trong đó commit thứ 13 chèn chữ "BUG" vào file:
   ```bash
   git switch -c bisect-lab
   for i in $(seq 1 20); do
     if [ $i -eq 13 ]; then echo "BUG" >> data.txt; else echo "dong $i" >> data.txt; fi
     git add data.txt && git commit -q -m "commit $i"
   done
   git bisect start
   git bisect bad
   git bisect good HEAD~19
   git bisect run sh -c '! grep -q BUG data.txt'   # tự động: có chữ BUG = bad
   git bisect reset
   ```
   Kiểm tra Git có chỉ đúng "commit 13" không.

---

<a id="muc-9"></a>

## 9. Cấp độ 8 – GitHub CLI (`gh`) & GitHub Actions

### 9.1. GitHub CLI — làm việc với GitHub ngay trong terminal

Cài tại https://cli.github.com, sau đó:

```bash
gh auth login                         # đăng nhập 1 lần (chọn GitHub.com → SSH → login with browser)

# Repo
gh repo create hoc-gh --public --source=. --push   # tạo repo trên GitHub từ thư mục hiện tại + push luôn (thay cho bước tạo trên web + remote add + push)
gh repo clone username/repo
gh repo view --web                    # mở repo trên trình duyệt
gh repo fork tacgia/project --clone   # fork + clone + tự thêm upstream

# Issue
gh issue create --title "Lỗi giỏ hàng" --body "Mô tả..."
gh issue list
gh issue view 12

# Pull Request
gh pr create --fill                   # tạo PR từ nhánh hiện tại, lấy title/body từ commit
gh pr list
gh pr checkout 34                     # kéo PR #34 của đồng đội về máy để test
gh pr review 34 --approve
gh pr merge 34 --squash --delete-branch
gh pr status

# Actions
gh run list
gh run watch                          # xem workflow đang chạy theo thời gian thực
```
- **🤔 Tại sao học `gh` sau cùng?** Nó chỉ là "lối tắt" cho những gì bạn đã làm trên web. Phải hiểu bản chất (PR, fork, remote) trước thì mới dùng lối tắt mà không bị lạc.

### 9.2. GitHub Actions — tự động hoá (CI/CD)

**CI (Continuous Integration):** mỗi lần push/PR, GitHub tự chạy test/build. PR có dấu ✅ hoặc ❌.
**CD (Continuous Deployment):** merge vào main → tự deploy lên server.

Tạo file `.github/workflows/ci.yml`:
```yaml
name: CI                                  # tên hiển thị ở tab Actions

on:                                       # KHI NÀO chạy
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest                # máy ảo GitHub cung cấp miễn phí
    steps:
      - uses: actions/checkout@v4         # bước 1: tải code của repo vào máy ảo
      - uses: actions/setup-node@v4       # bước 2: cài Node.js
        with:
          node-version: 20
      - run: npm ci                       # bước 3: cài thư viện
      - run: npm test                     # bước 4: chạy test — thất bại thì PR hiện ❌
```
Ví dụ cho Java/Spring Boot (khớp roadmap của bạn):
```yaml
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 21
      - run: ./mvnw -B test
```
- Commit + push file này → tab **Actions** thấy workflow chạy.
- Kết hợp **Branch protection** "Require status checks to pass" → không ai merge được code làm hỏng test.
- Mật khẩu deploy đặt ở **Settings → Secrets and variables → Actions**, dùng trong YAML bằng `${{ secrets.TEN_SECRET }}`.

### ✅ Bài tập 9

1. Cài `gh`, `gh auth login`. Tạo 1 repo mới hoàn toàn bằng `gh repo create`.
2. Tạo issue và PR bằng `gh`, merge bằng `gh pr merge --squash --delete-branch`.
3. Thêm workflow đơn giản chỉ có bước `run: echo "Xin chao tu GitHub Actions"` và `run: ls -la`, push và xem log trong tab Actions.
4. Cố tình thêm bước `run: exit 1` trên 1 PR → thấy ❌. Sửa lại → ✅.
5. Deploy `index.html` của repo `hoc-git` bằng **GitHub Pages** (Settings → Pages → Deploy from branch `main`).

---

<a id="muc-10"></a>

## 10. Quy ước làm việc nhóm chuyên nghiệp

### 10.1. Đặt tên nhánh

```
feature/12-dang-nhap-google     ← tính năng mới, kèm số issue
fix/45-loi-tinh-tong-gio-hang   ← sửa lỗi
hotfix/thanh-toan-500           ← lỗi khẩn cấp trên production
docs/cap-nhat-readme            ← tài liệu
refactor/tach-service-order     ← cải tổ code, không đổi chức năng
chore/nang-cap-spring-boot      ← việc vặt: cấu hình, thư viện
```
Chữ thường, không dấu, nối bằng gạch ngang.

### 10.2. Conventional Commits — chuẩn viết commit message

```
<type>(<phạm vi>): <mô tả ngắn ở thể mệnh lệnh>

<thân: giải thích TẠI SAO, không phải CÁI GÌ (tuỳ chọn)>

<footer: Closes #12, BREAKING CHANGE: ... (tuỳ chọn)>
```
| type | Dùng khi |
|---|---|
| `feat` | thêm tính năng |
| `fix` | sửa lỗi |
| `docs` | tài liệu |
| `style` | format, dấu chấm phẩy (không đổi logic) |
| `refactor` | cải tổ code |
| `test` | thêm/sửa test |
| `chore` | build, thư viện, cấu hình |
| `perf` | tối ưu hiệu năng |

Ví dụ:
```
feat(cart): cho phép cập nhật số lượng sản phẩm trong giỏ
fix(auth): sửa lỗi token hết hạn không tự refresh

Người dùng bị đăng xuất sau 15 phút dù vẫn đang thao tác.
Thêm interceptor gọi /refresh khi nhận 401.

Closes #45
```

### 10.3. Mô hình làm việc (Workflow)

**GitHub Flow** (đơn giản — khuyên dùng cho nhóm nhỏ và dự án của bạn):
```
main (luôn deploy được)
  ├── feature/a ──PR──▶ main
  ├── fix/b     ──PR──▶ main
```
1. `main` luôn chạy được. 2. Mọi việc đều tạo nhánh từ main. 3. Push sớm, mở PR sớm. 4. Review + CI xanh → merge. 5. Merge xong → deploy.

**Git Flow** (phức tạp hơn, dùng cho sản phẩm phát hành theo phiên bản): có thêm `develop`, `release/*`, `hotfix/*`. Biết để đọc hiểu khi công ty dùng.

### 10.4. Checklist trước khi tạo PR
- [ ] Đã `pull`/`rebase` main mới nhất, không conflict.
- [ ] App chạy được, test pass trên máy.
- [ ] `git diff main...HEAD` tự review: không còn `console.log`, code comment thừa, file không liên quan.
- [ ] Không có file bí mật (`.env`, key).
- [ ] Commit message rõ ràng (đã dọn bằng `rebase -i` nếu cần).
- [ ] PR description: làm gì, tại sao, test thế nào, ảnh chụp UI, `Closes #...`.
- [ ] PR nhỏ (< 400 dòng thay đổi lý tưởng) — PR to không ai muốn review.

---

<a id="muc-11"></a>

## 11. 🏋️ Bộ bài tập tổng hợp có hướng dẫn

> Các bài trong từng mục ở trên là bài tập "luyện lệnh". Phần này là **dự án mini** mô phỏng tình huống thật. Làm lần lượt.

### 🟢 Dự án 1 — "Nhật ký học tập" (1 người, cơ bản)

**Mục tiêu:** Thuần thục vòng lặp `status → add → commit → push`, viết commit message chuẩn.

1. Dùng `gh repo create` hoặc web tạo repo `nhat-ky-hoc-tap` (có README).
2. `git clone` về máy.
3. Tạo `.gitignore` (bỏ qua `*.log`, `.env`), commit: `chore: thêm .gitignore`.
4. **Mỗi ngày trong 7 ngày**, tạo file `ngay-XX.md` ghi lại những gì học được, commit theo chuẩn `docs: nhật ký ngày XX`, push.
5. Ngày thứ 3, cố tình commit với message sai → sửa bằng `--amend` (trước khi push).
6. Ngày thứ 5, lỡ add file `test.log` → dùng `restore --staged`, và thêm `.gitignore` cho đúng.
7. Kết thúc: xem lịch sử `git log --oneline`, trang GitHub có ô xanh contribution 7 ngày.

**Tiêu chí hoàn thành:** 7+ commit có ý nghĩa, không có file rác trên GitHub.

### 🟡 Dự án 2 — "Landing page shop" (1 người, nhánh + PR)

**Mục tiêu:** Mọi thay đổi đều qua nhánh + PR, tự xử lý conflict.

1. Repo `shop-landing`, bật **Branch protection** cho main (bắt buộc PR).
2. Tạo 4 issue: `#1 Header`, `#2 Danh sách sản phẩm`, `#3 Footer`, `#4 Đổi màu chủ đạo`.
3. Với mỗi issue: `switch -c feature/<số>-<tên>` → code HTML/CSS → commit → push → PR có `Closes #<số>` → squash merge.
4. **Tạo conflict có chủ đích:** mở 2 nhánh cùng lúc từ main: `feature/4-mau-xanh` và `feature/5-mau-do`, cả 2 sửa cùng dòng màu nền trong `style.css`. Merge PR thứ nhất trên GitHub. Với nhánh thứ hai:
   ```bash
   git switch feature/5-mau-do
   git fetch
   git merge origin/main         # 💥 conflict
   # giải quyết → add → commit → push → PR tự cập nhật, hết conflict
   ```
5. Gắn tag `v1.0.0`, tạo Release, bật **GitHub Pages** để có link web thật.

**Tiêu chí:** Lịch sử main mỗi commit = 1 PR; tất cả issue đóng tự động; web chạy online.

### 🟠 Dự án 3 — "Làm nhóm 2 người" (cần 1 người bạn, hoặc tự đóng 2 vai bằng 2 thư mục clone)

**Mục tiêu:** Trải nghiệm thật việc pull/push của nhiều người, review code của nhau.

1. Bạn tạo repo, vào **Settings → Collaborators** mời bạn của bạn (hoặc bạn clone vào 2 thư mục `may-A`, `may-B`).
2. Hai người cùng làm trên 2 nhánh khác nhau, sửa **chung 1 file** `README.md` (mỗi người thêm 1 mục).
3. A merge PR trước. B gặp conflict khi cập nhật → B tự giải quyết.
4. Mỗi người **review PR của người kia**: để ít nhất 2 comment, dùng chức năng **Suggest changes**, "Request changes" rồi "Approve".
5. B thử `git push` khi A vừa push lên main → bị từ chối → `git pull --rebase` → push lại.
6. Ghi lại trong `RETRO.md`: gặp lỗi gì, giải quyết thế nào.

### 🔴 Dự án 4 — "Đóng góp open source" (nâng cao)

1. Làm repo https://github.com/firstcontributions/first-contributions (fork → clone → nhánh → sửa → push → PR).
2. Tìm repo thật có label `good first issue` (tìm trên GitHub: `label:"good first issue" language:Java`).
3. Đọc `CONTRIBUTING.md` của repo đó **trước tiên** (mỗi repo có luật riêng).
4. Comment xin nhận issue → fork → thêm `upstream` → làm → giữ fork đồng bộ bằng `fetch upstream` + `rebase upstream/main` → PR.
5. Phản hồi review lịch sự, sửa theo yêu cầu.

### 🟣 Dự án 5 — "CI/CD cho dự án Spring Boot/Next.js của roadmap"

1. Khi bắt đầu dự án lớn trong [roadmap](roadmapfornamtron.md), tạo repo ngay từ ngày đầu.
2. Thêm workflow CI chạy `./mvnw test` (backend) và `npm run build` (frontend) trên mọi PR.
3. Bật branch protection: PR + CI xanh mới được merge.
4. Dùng Conventional Commits suốt dự án; mỗi mốc lớn gắn tag `v0.1.0`, `v0.2.0`…
5. Viết README chất lượng (mô tả, ảnh chụp, công nghệ, cách chạy local, link demo) — đây chính là **portfolio xin việc**.

---

<a id="muc-12"></a>

## 12. 🚑 Lỗi thường gặp & cách chữa

| Thông báo lỗi | Nguyên nhân | Cách chữa |
|---|---|---|
| `fatal: not a git repository` | Đang đứng ngoài thư mục repo | `cd` vào đúng thư mục, hoặc `git init` nếu thật sự là dự án mới |
| `Author identity unknown` | Chưa config tên/email | Làm [mục 1.2](#muc-1) |
| `Permission denied (publickey)` | Chưa cài SSH key / key chưa thêm lên GitHub | Làm [mục 1.3](#muc-1), kiểm tra `ssh -T git@github.com` |
| `! [rejected] ... (fetch first)` | GitHub có commit mới mà máy chưa có | `git pull` (giải quyết conflict nếu có) rồi `git push` |
| `! [rejected] ... (non-fast-forward)` sau khi amend/rebase | Bạn đã viết lại lịch sử đã push | Nhánh riêng: `git push --force-with-lease`. Nhánh chung: đừng làm vậy, dùng `revert` |
| `fatal: The current branch X has no upstream branch` | Push nhánh mới lần đầu | `git push -u origin X` |
| `error: Your local changes would be overwritten by checkout/merge` | Có thay đổi chưa commit | `git stash` hoặc `git commit`, rồi làm lại |
| `CONFLICT (content): Merge conflict in ...` | 2 bên sửa cùng chỗ | Làm theo [mục 5.3](#muc-5) |
| `You are in 'detached HEAD' state` | Đã checkout 1 commit/tag, không phải nhánh | Chỉ xem thì ok. Muốn làm tiếp: `git switch -c ten-nhanh-moi`. Muốn quay về: `git switch main` |
| `refusing to merge unrelated histories` | Tạo README trên GitHub + `init` riêng ở máy → 2 lịch sử không chung gốc | `git pull origin main --allow-unrelated-histories` rồi giải quyết conflict |
| Lỡ push `.env` / mật khẩu lên GitHub | — | **1) Đổi mật khẩu/thu hồi key NGAY** (coi như đã lộ). 2) Thêm vào `.gitignore`, `git rm --cached .env`, commit. 3) Muốn xoá khỏi lịch sử: dùng `git filter-repo` hoặc BFG (nâng cao) |
| Commit nhầm lên `main` thay vì nhánh mới (chưa push) | — | `git branch feature-x` (giữ commit ở nhánh mới) → `git reset --hard origin/main` (đưa main về như GitHub) → `git switch feature-x` |
| Mắc kẹt trong màn hình lạ khi `git log`/`git commit` | Đang trong trình xem `less` hoặc editor `vim` | `less`: nhấn `q`. `vim`: nhấn `Esc`, gõ `:wq` Enter (lưu & thoát) hoặc `:q!` (thoát không lưu). Cấu hình VS Code làm editor để tránh |
| File bị diff toàn bộ dù chỉ sửa 1 dòng (Windows) | Khác ký tự xuống dòng CRLF/LF | `git config --global core.autocrlf true`, thêm file `.gitattributes` với `* text=auto` |

### ⚠️ 5 điều KHÔNG BAO GIỜ làm
1. `git push --force` lên `main` hoặc nhánh người khác đang dùng.
2. Commit file bí mật (`.env`, private key, mật khẩu).
3. `rebase`/`reset`/`amend` commit đã push lên nhánh chung.
4. `git reset --hard` hoặc `git clean -f` mà không xem trước (`git status`, `git clean -n`).
5. Xoá thư mục `.git` để "sửa lỗi" — gần như luôn có cách đúng hơn (hỏi `reflog` trước).

---

<a id="muc-13"></a>

## 13. 📋 Cheat sheet – bảng tra nhanh

### Thiết lập
```bash
git config --global user.name "Tên"          # khai báo tên
git config --global user.email "email"       # khai báo email
git init                                     # tạo repo mới
git clone <url>                              # tải repo về
```

### Vòng lặp hằng ngày
```bash
git status                                   # xem tình hình
git diff / git diff --staged                 # xem thay đổi chưa/đã add
git add <file> / git add .                   # đưa vào staging
git commit -m "type: mô tả"                  # tạo điểm lưu
git log --oneline --graph --all              # xem lịch sử dạng cây
```

### Đồng bộ với GitHub
```bash
git remote add origin <url>                  # gắn địa chỉ GitHub
git push -u origin <nhánh>                   # đẩy lần đầu
git push                                     # đẩy
git fetch                                    # tải thông tin mới, chưa gộp
git pull / git pull --rebase                 # tải + gộp
```

### Nhánh
```bash
git branch                                   # liệt kê
git switch -c <nhánh>                        # tạo + chuyển
git switch <nhánh>                           # chuyển
git merge <nhánh>                            # gộp <nhánh> vào nhánh hiện tại
git branch -d <nhánh>                        # xoá nhánh đã merge
git push origin --delete <nhánh>             # xoá nhánh trên GitHub
```

### Sửa sai
```bash
git restore <file>                           # bỏ sửa đổi chưa add
git restore --staged <file>                  # bỏ khỏi staging
git commit --amend                           # sửa commit cuối (chưa push)
git reset --soft HEAD~1                      # huỷ commit, giữ code ở staging
git reset --hard HEAD~1                      # huỷ commit + code 🔥
git revert <hash>                            # huỷ commit đã push (an toàn)
git reflog                                   # cứu hộ
```

### Nâng cao
```bash
git stash / git stash pop                    # cất / lấy lại việc dở
git rebase <nhánh>                           # đặt lại gốc nhánh
git rebase -i HEAD~n                         # dọn dẹp n commit cuối
git cherry-pick <hash>                       # nhặt 1 commit
git tag -a v1.0.0 -m "..." && git push --tags  # đánh dấu phiên bản
git blame <file>                             # ai sửa dòng nào
git bisect start/good/bad/reset              # tìm commit gây lỗi
git push --force-with-lease                  # đẩy ép an toàn (nhánh riêng)
```

### GitHub CLI
```bash
gh repo create <tên> --public --source=. --push
gh issue create / gh issue list
gh pr create --fill / gh pr checkout <số> / gh pr merge <số> --squash
gh run watch
```

---

## 📚 Tài nguyên học thêm

- **Pro Git** (sách miễn phí, có bản tiếng Việt một phần): https://git-scm.com/book
- **Learn Git Branching** (game trực quan, CỰC KỲ nên chơi để hiểu nhánh/rebase): https://learngitbranching.js.org
- **GitHub Skills** (khoá học tương tác chính chủ): https://skills.github.com
- **Oh Shit, Git!?!** (cứu hộ tình huống khó): https://ohshitgit.com
- **Conventional Commits**: https://www.conventionalcommits.org
- **Mẫu .gitignore**: https://github.com/github/gitignore

---

> 💪 **Lời khuyên cuối:** Git chỉ thật sự "ngấm" khi bạn **dùng nó mỗi ngày cho dự án thật**. Từ hôm nay, mọi thứ bạn code trong [roadmap](roadmapfornamtron.md) — dù chỉ là bài tập HTML nhỏ — đều phải nằm trong 1 repo Git và được push lên GitHub. Sau 3 tháng, những lệnh trong file này sẽ trở thành phản xạ.
