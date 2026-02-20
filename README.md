# 📘 FacebookAPI (Unofficial)

> **Cookie-based Facebook Internal API wrapper** — Thực hiện các tương tác Facebook thông qua cookie, không cần Access Token chính thức.

🌐 **Language / Ngôn ngữ:** [🇬🇧 English](README.en.md) | 🇻🇳 Tiếng Việt

**Author:** Đạt Thành - pillrock  
**Language:** Python 3.8+  
**Last updated:** 20/02/2026

---

## ✅ Tính năng đã kiểm tra

| # | Hàm | Mô tả | Trạng thái |
|---|-----|--------|-----------|
| 1 | `info()` | Lấy tên & UID tài khoản từ cookie | ✅ Hoạt động |
| 2 | `reaction()` | Bày tỏ cảm xúc bài viết (LIKE, LOVE, HAHA...) | ✅ Hoạt động |
| 3 | `reaction_comment()` | Bày tỏ cảm xúc trên comment | ✅ Hoạt động |
| 4 | `share()` | Chia sẻ bài viết lên tường | ✅ Hoạt động |
| 5 | `comment()` | Đăng bình luận vào bài viết | ✅ Hoạt động |
| 6 | `follow()` | Follow một người dùng | ✅ Hoạt động |
| 7 | `join_group()` | Tham gia vào một nhóm | ✅ Hoạt động |
| 8 | `like_page()` | Like / Follow một trang | ✅ Hoạt động |

---

## 🚀 Cài đặt

### Yêu cầu

Cài đặt các thư viện cần thiết bằng một trong hai cách:

**Cách 1 — Dùng `requirements.txt` (khuyến nghị):**
```bash
pip install -r requirements.txt
```

**Cách 2 — Cài thủ công:**
```bash
pip install requests
```

### Cấu trúc thư mục

```
FacebookAPI/
├── FacebookAPI.py    ← File chính (import từ đây)
├── requirements.txt  ← Danh sách thư viện cần thiết
├── README.md         ← Tài liệu tiếng Việt
└── README.en.md      ← Tài liệu tiếng Anh
```

---

## 🔑 Lấy Cookie Facebook

> Cookie là "chìa khóa" để đăng nhập Facebook. Phải lấy đúng mới dùng được.

**Cách lấy:**

1. Mở **Chrome / Edge** → đăng nhập Facebook
2. Nhấn **F12** → chọn tab **Network**
3. Reload trang (`F5`) → click vào bất kỳ request nào tới `facebook.com`
4. Chọn tab **Headers** → tìm dòng `cookie:` trong **Request Headers**
5. Copy toàn bộ giá trị (chuỗi dài bắt đầu bằng `datr=...`)

```
datr=xxx; sb=xxx; c_user=61572991975647; xs=xxx; fr=xxx; ...
```

> ⚠️ **Lưu ý bảo mật:** Cookie = quyền truy cập tài khoản. KHÔNG chia sẻ cho người khác!

---

## 📖 Hướng dẫn sử dụng

### Khởi tạo

```python
from FacebookAPI import FacebookAPI

COOKIE = "datr=xxx; c_user=YOUR_UID; xs=xxx; ..."

fb = FacebookAPI(COOKIE)
```

---

### 1. `info()` — Kiểm tra tài khoản

```python
result = fb.info()
# {'success': 200, 'id': '61572991975647', 'name': 'Nguyễn Văn A'}

if 'success' in result:
    print(f"Tài khoản: {result['name']} | UID: {result['id']}")
else:
    print("Cookie die hoặc bị checkpoint!")
```

---

### 2. `reaction()` — Bày tỏ cảm xúc bài viết

```python
# Lấy post_id từ URL bài viết: facebook.com/photo?fbid=122173519772766399
# → post_id = "122173519772766399"

result = fb.reaction("122173519772766399", "LOVE")
print(result)  # True/False
```

**Loại cảm xúc hỗ trợ:**

| Tham số | Cảm xúc |
|---------|---------|
| `"LIKE"` | 👍 Thích |
| `"LOVE"` | ❤️ Yêu thích |
| `"CARE"` | 🤗 Thương thương |
| `"HAHA"` | 😆 Haha |
| `"WOW"` | 😮 Wow |
| `"SAD"` | 😢 Buồn |
| `"ANGRY"` | 😡 Phẫn nộ |

---

### 3. `reaction_comment()` — React comment

```python
# Lấy comment_id: chuột phải vào thời gian comment → Copy link
# Link dạng: facebook.com/...?comment_id=1258842489460361
# → comment_id = "1258842489460361"  (chỉ dùng số thuần, KHÔNG base64)

result = fb.reaction_comment("1258842489460361", "HAHA")
print(result)  # True/False
```

---

### 4. `share()` — Chia sẻ bài viết

```python
result = fb.share("122173519772766399")
print(result)  # True/False
```

---

### 5. `comment()` — Đăng bình luận

```python
result = fb.comment("122173519772766399", "Hay quá! 🔥")
print(result)  # True/False
```

---

### 6. `follow()` — Follow người dùng

```python
# Lấy user_id từ URL: facebook.com/profile.php?id=100054958380559
# → user_id = "100054958380559"

result = fb.follow("100054958380559")
print(result)  # True/False
```

---

### 7. `join_group()` — Tham gia nhóm

```python
# Lấy group_id từ URL: facebook.com/groups/1432596974946895
# → group_id = "1432596974946895"

result = fb.join_group("1432596974946895")
print(result)  # True/False
# True = tham gia thành công (public) hoặc đã gửi yêu cầu (private)
```

---

### 8. `like_page()` — Like / Follow trang

```python
# Lấy page_id từ URL trang: facebook.com/profile.php?id=180375029024062
# Hoặc: facebook.com/pages/.../323713887631229
# → page_id = "180375029024062"

result = fb.like_page("180375029024062")
print(result)  # True/False
```

---

## 🔍 Cách lấy ID cho từng hàm

### 📌 POST ID (dùng cho reaction, share, comment)

```
facebook.com/photo?fbid=122173519772766399
                         ↑ đây là post_id

facebook.com/permalink.php?story_fbid=122173519772766399&id=61572991975647
                                       ↑ đây là post_id

facebook.com/61572991975647/posts/122173519772766399
                                   ↑ đây là post_id
```

> ⚠️ Nếu ID dạng `"user_post"` (ví dụ `"61572991975647_122173519772766399"`), class tự động tách phần sau `_`.

---

### 📌 COMMENT ID (dùng cho reaction_comment)

1. Vào bài viết trên Facebook
2. Chuột phải vào **dòng thời gian** của comment (ví dụ "2 giờ")
3. Chọn **"Copy link"**
4. Link dạng:
   ```
   facebook.com/...?comment_id=1258842489460361
                                ↑ đây là comment_id
   ```

> ⚠️ **KHÔNG dùng** chuỗi base64 dài như `Y29tbWVudDox...`. Phải lấy **số thuần** từ link.

---

### 📌 PAGE ID (dùng cho like_page)

**Cách 1** — Xem URL khi vào trang About:
```
facebook.com/pages/TenTrang/323713887631229
                             ↑ đây là page_id
```

**Cách 2** — F12 Console, gõ:
```javascript
document.body.innerHTML.match(/"page_id":"(\d+)"/)[1]
```

---

### 📌 GROUP ID (dùng cho join_group)

```
facebook.com/groups/1432596974946895
                    ↑ đây là group_id
```

---

### 📌 USER ID (dùng cho follow)

**Cách 1** — Xem URL profile:
```
facebook.com/profile.php?id=100054958380559
                            ↑ đây là user_id
```

**Cách 2** — F12 Console trên trang profile, gõ:
```javascript
document.querySelector('[data-userid]')?.dataset.userid
```

---

## 🛡️ Proxy (tuỳ chọn)

```python
# Dạng: "host:port:username:password"
proxy = "123.456.789.0:8080:user:pass"

fb = FacebookAPI(COOKIE, proxy=proxy)
```

---

## ⚙️ Cơ chế hoạt động

```
Cookie → _fetch_tokens() → lấy fb_dtsg, lsd, jazoest từ HTML
                                        ↓
                              Gọi POST /api/graphql/
                              với fb_dtsg + lsd + variables
                                        ↓
                              Facebook xử lý → response JSON
```

Tất cả request đều gọi endpoint nội bộ:
```
POST https://www.facebook.com/api/graphql/
```

---

## ⚠️ Lưu ý quan trọng

1. **Cookie hết hạn**: Cookie Facebook thường sống **vài tuần đến vài tháng**. Nếu token lấy được rỗng → cookie đã hết hạn, cần lấy lại.

2. **Rate limit**: Facebook có giới hạn tương tác. Nếu dùng quá nhiều → tài khoản có thể bị **checkpoint** hoặc **block tạm thời**.

4. **Trách nhiệm pháp lý**: Tool này chỉ dùng cho mục đích học tập và nghiên cứu. Việc tự động hóa tài khoản vi phạm **Terms of Service của Facebook**.

---

## 🐛 Troubleshooting

| Lỗi | Nguyên nhân | Cách fix |
|-----|-------------|---------|
| `fb_dtsg` rỗng | Cookie hết hạn / sai | Lấy cookie mới từ trình duyệt |
| `ValueError: Cookie không hợp lệ` | Thiếu `c_user=` trong cookie | Copy lại cookie đầy đủ |
| Tất cả hàm trả về `False` | `doc_id` outdated | Bắt request thật và update `doc_id` |
| `ModuleNotFoundError: requests` | Chưa cài thư viện | `pip install requests` |
| Reaction thành công nhưng không thấy trên FB | Bị spam filter | Đổi loại reaction / chờ vài giây |

---

## 📄 License

Dự án này được phát hành dưới giấy phép **[MIT License](LICENSE)**.

Bạn được phép sử dụng, sao chép, chỉnh sửa và phân phối tự do, miễn là giữ nguyên thông tin bản quyền gốc.

> ⚠️ Dù có giấy phép MIT, công cụ này chỉ nên dùng cho **mục đích học tập và nghiên cứu**. Việc tự động hóa tài khoản vi phạm **Terms of Service của Facebook**.
