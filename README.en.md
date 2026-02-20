# 📘 FacebookAPI (Unofficial)

> **Cookie-based Facebook Internal API wrapper** — Interact with Facebook using cookies, no official Access Token required.

🌐 **Language / Ngôn ngữ:** 🇬🇧 English | [🇻🇳 Tiếng Việt](README.md)

**Author:** Đạt Thành - pillrock  
**Language:** Python 3.8+  
**Last updated:** 20/02/2026

---

## ✅ Tested Features

| # | Method | Description | Status |
|---|--------|-------------|--------|
| 1 | `info()` | Get account name & UID from cookie | ✅ Working |
| 2 | `reaction()` | React to a post (LIKE, LOVE, HAHA…) | ✅ Working |
| 3 | `reaction_comment()` | React to a comment | ✅ Working |
| 4 | `share()` | Share a post to your timeline | ✅ Working |
| 5 | `comment()` | Post a comment on a post | ✅ Working |
| 6 | `follow()` | Follow a user | ✅ Working |
| 7 | `join_group()` | Join a group | ✅ Working |
| 8 | `like_page()` | Like / Follow a page | ✅ Working |

---

## 🚀 Installation

### Requirements

Install all required libraries using one of the following methods:

**Method 1 — Using `requirements.txt` (recommended):**
```bash
pip install -r requirements.txt
```

**Method 2 — Manual install:**
```bash
pip install requests
```

### Project Structure

```
FacebookAPI/
├── FacebookAPI.py    ← Main file (import from here)
├── requirements.txt  ← Required dependencies
├── README.md         ← Vietnamese documentation
└── README.en.md      ← English documentation (this file)
```

---

## 🔑 Getting Your Facebook Cookie

> A cookie is your "login key" for Facebook. You need a valid cookie to use this library.

**Steps:**

1. Open **Chrome / Edge** → log in to Facebook
2. Press **F12** → go to the **Network** tab
3. Reload the page (`F5`) → click on any request to `facebook.com`
4. Go to the **Headers** tab → find the `cookie:` line under **Request Headers**
5. Copy the entire value (a long string starting with `datr=...`)

```
datr=xxx; sb=xxx; c_user=61572991975647; xs=xxx; fr=xxx; ...
```

> ⚠️ **Security warning:** Your cookie = access to your account. **NEVER share it with anyone!**

---

## 📖 Usage Guide

### Initialization

```python
from FacebookAPI import FacebookAPI

COOKIE = "datr=xxx; c_user=YOUR_UID; xs=xxx; ..."

fb = FacebookAPI(COOKIE)
```

---

### 1. `info()` — Check account

```python
result = fb.info()
# {'success': 200, 'id': '61572991975647', 'name': 'John Doe'}

if 'success' in result:
    print(f"Account: {result['name']} | UID: {result['id']}")
else:
    print("Cookie is expired or account is checkpointed!")
```

---

### 2. `reaction()` — React to a post

```python
# Get post_id from the post URL: facebook.com/photo?fbid=122173519772766399
# → post_id = "122173519772766399"

result = fb.reaction("122173519772766399", "LOVE")
print(result)  # True / False
```

**Supported reaction types:**

| Parameter | Reaction |
|-----------|---------|
| `"LIKE"` | 👍 Like |
| `"LOVE"` | ❤️ Love |
| `"CARE"` | 🤗 Care |
| `"HAHA"` | 😆 Haha |
| `"WOW"` | 😮 Wow |
| `"SAD"` | 😢 Sad |
| `"ANGRY"` | 😡 Angry |

---

### 3. `reaction_comment()` — React to a comment

```python
# Get comment_id: right-click on the comment timestamp → Copy link
# Link format: facebook.com/...?comment_id=1258842489460361
# → comment_id = "1258842489460361"  (plain number ONLY, NOT base64)

result = fb.reaction_comment("1258842489460361", "HAHA")
print(result)  # True / False
```

---

### 4. `share()` — Share a post

```python
result = fb.share("122173519772766399")
print(result)  # True / False
```

---

### 5. `comment()` — Post a comment

```python
result = fb.comment("122173519772766399", "Great post! 🔥")
print(result)  # True / False
```

---

### 6. `follow()` — Follow a user

```python
# Get user_id from URL: facebook.com/profile.php?id=100054958380559
# → user_id = "100054958380559"

result = fb.follow("100054958380559")
print(result)  # True / False
```

---

### 7. `join_group()` — Join a group

```python
# Get group_id from URL: facebook.com/groups/1432596974946895
# → group_id = "1432596974946895"

result = fb.join_group("1432596974946895")
print(result)  # True / False
# True = joined successfully (public) or request sent (private group)
```

---

### 8. `like_page()` — Like / Follow a page

```python
# Get page_id from the page URL: facebook.com/profile.php?id=180375029024062
# Or: facebook.com/pages/.../323713887631229
# → page_id = "180375029024062"

result = fb.like_page("180375029024062")
print(result)  # True / False
```

---

## 🔍 How to Find IDs

### 📌 POST ID (for reaction, share, comment)

```
facebook.com/photo?fbid=122173519772766399
                         ↑ this is the post_id

facebook.com/permalink.php?story_fbid=122173519772766399&id=61572991975647
                                       ↑ this is the post_id

facebook.com/61572991975647/posts/122173519772766399
                                   ↑ this is the post_id
```

> ⚠️ If the ID is in `"user_post"` format (e.g. `"61572991975647_122173519772766399"`), the class automatically extracts the part after `_`.

---

### 📌 COMMENT ID (for reaction_comment)

1. Go to the post on Facebook
2. Right-click on the **timestamp** of the comment (e.g. "2 hours ago")
3. Select **"Copy link"**
4. The link will look like:
   ```
   facebook.com/...?comment_id=1258842489460361
                                ↑ this is the comment_id
   ```

> ⚠️ **Do NOT use** the long base64 string like `Y29tbWVudDox...`. Always use the **plain number** from the link.

---

### 📌 PAGE ID (for like_page)

**Method 1** — Check the URL on the About page:
```
facebook.com/pages/PageName/323713887631229
                             ↑ this is the page_id
```

**Method 2** — F12 Console, run:
```javascript
document.body.innerHTML.match(/"page_id":"(\d+)"/)[1]
```

---

### 📌 GROUP ID (for join_group)

```
facebook.com/groups/1432596974946895
                    ↑ this is the group_id
```

---

### 📌 USER ID (for follow)

**Method 1** — Check the profile URL:
```
facebook.com/profile.php?id=100054958380559
                             ↑ this is the user_id
```

**Method 2** — F12 Console on the profile page, run:
```javascript
document.querySelector('[data-userid]')?.dataset.userid
```

---

## 🛡️ Proxy (Optional)

```python
# Format: "host:port:username:password"
proxy = "123.456.789.0:8080:user:pass"

fb = FacebookAPI(COOKIE, proxy=proxy)
```

---

## ⚙️ How It Works

```
Cookie → _fetch_tokens() → extract fb_dtsg, lsd, jazoest from HTML
                                      ↓
                            POST /api/graphql/
                            with fb_dtsg + lsd + variables
                                      ↓
                            Facebook processes → JSON response
```

All requests target the internal endpoint:
```
POST https://www.facebook.com/api/graphql/
```

---

## ⚠️ Important Notes

1. **Cookie expiration**: Facebook cookies typically last **a few weeks to a few months**. If the fetched token is empty → the cookie has expired. Grab a new one from your browser.

2. **Rate limiting**: Facebook limits interaction frequency. Excessive use may result in **checkpoint** or **temporary block**.

4. **Legal responsibility**: This tool is for **educational and research purposes only**. Automating Facebook accounts violates **Facebook's Terms of Service**.

---

## 🐛 Troubleshooting

| Error | Cause | Fix |
|-------|-------|-----|
| `fb_dtsg` is empty | Cookie expired / invalid | Get a fresh cookie from your browser |
| `ValueError: Cookie không hợp lệ` | Missing `c_user=` in cookie | Copy the complete cookie string |
| All methods return `False` | `doc_id` is outdated | Capture a real request and update `doc_id` |
| `ModuleNotFoundError: requests` | Library not installed | Run `pip install -r requirements.txt` |
| Reaction succeeds but not visible on FB | Spam filter triggered | Try a different reaction type / wait a few seconds |

---

## 📄 License

This project is released under the **[MIT License](LICENSE)**.

You are free to use, copy, modify, and distribute this software, as long as the original copyright notice is retained.

> ⚠️ Despite the MIT license, this tool should only be used for **educational and research purposes**. Automating Facebook accounts violates **Facebook’s Terms of Service**.
