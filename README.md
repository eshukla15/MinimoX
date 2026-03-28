# 🐦 TweetBar

**A lightweight Twitter-style posting app — built from scratch with Django to actually *understand* how web frameworks work.**

---

## Why This Project Exists

Ever read a Django tutorial and felt like you understood it... until you tried building something yourself?

That's exactly where this project came from. I wanted to go beyond "hello world" and build something with **real features** — user accounts, media uploads, CRUD operations, protected routes — the kind of stuff you actually deal with when building real web apps.

TweetBar isn't trying to be the next Twitter. It's a focused, hands-on project to **learn Django by doing**, not just by reading docs. Every file in this repo represents a concept I wrestled with and eventually understood.

---

## What It Does

TweetBar is a social micro-posting platform where users can:

- 📝 **Post tweets** — short text posts (up to 240 characters), optionally with a photo
- ✏️ **Edit their own tweets** — because everyone deserves a chance to fix typos
- 🗑️ **Delete their own tweets** — no more regretting what you posted
- 🔐 **Register & log in** — full authentication with email, username, and password
- 🖼️ **Upload images** — attach photos to tweets, displayed in a clean card layout
- 👀 **Browse all tweets** — see what everyone's posting, newest first

The key point: you can only edit or delete **your own** tweets. Other people's posts are read-only. Just like how it should work.

---

## How It Works (High-Level)

Think of it like a restaurant:

1. **The front door (URLs)** — When you visit a page, Django's URL router figures out what you're asking for and sends you to the right place.
2. **The kitchen (Views)** — The views are where the actual logic lives. They decide what data to fetch, what forms to show, and what to do with submitted data.
3. **The menu (Templates)** — HTML templates that display everything to the user. They extend a shared layout (navbar, styling) so the app feels consistent.
4. **The pantry (Models)** — The Tweet model defines *what* a tweet is — text, photo, who posted it, and when. Django turns this into a database table automatically.
5. **The bouncer (Auth)** — Django's built-in auth system handles login, logout, and registration. The `@login_required` decorator protects routes that need a logged-in user.

```
User Request → URL Router → View (logic) → Template (display)
                                ↕
                          Model (database)
```

---

## Tech Stack

| Technology | Purpose |
|---|---|
| **Python 3** | The language everything runs on |
| **Django 5.2** | Web framework — handles routing, ORM, auth, templating, and more |
| **SQLite** | Database — lightweight, zero-config, perfect for development |
| **Bootstrap 5** | CSS framework for responsive, dark-themed UI |
| **Pillow** | Image processing library — needed for Django's `ImageField` |
| **Django Templates (Jinja-like)** | Server-side HTML rendering with template inheritance |

---

## Getting Started

### Prerequisites

- Python 3.10+ installed on your machine
- `pip` (comes with Python)
- Git (optional, for cloning)

### Setup

**1. Clone the repository**

```bash
git clone https://github.com/eshukla15/learningDjango-Twitter.git
cd learningDjango-Twitter
```

**2. Create and activate a virtual environment**

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

**3. Install dependencies**

```bash
pip install -r requirements.txt
```

**4. Run database migrations**

```bash
cd learningdjango
python manage.py migrate
```

**5. Create a superuser (for admin access)**

```bash
python manage.py createsuperuser
```

**6. Start the development server**

```bash
python manage.py runserver
```

**7. Open your browser**

Head to [http://127.0.0.1:8000](http://127.0.0.1:8000) and you're in! 🎉

---

## Usage

### As a new user:
1. Click **Register** in the navbar
2. Create an account with username, email, and password
3. You're automatically logged in and redirected to the tweet feed

### Posting a tweet:
1. Click **Create a tweet** on the home page
2. Write your text (up to 240 characters)
3. Optionally attach an image
4. Hit **Submit** — your tweet shows up in the feed

### Managing your tweets:
- **Edit** and **Delete** buttons appear only on *your* tweets
- Deleting asks for confirmation first (no accidental deletions)

### Admin panel:
- Visit `/admin/` and log in with your superuser credentials
- Full control over all users and tweets

---

## Project Structure

```
learningDjango/
├── learningdjango/              # Django project root
│   ├── learningdjango/          # Project settings & config
│   │   ├── settings.py          # All the Django configuration
│   │   ├── urls.py              # Root URL routing
│   │   └── wsgi.py / asgi.py    # Server entry points
│   ├── tweet/                   # The main app
│   │   ├── models.py            # Tweet data model
│   │   ├── views.py             # All the view logic (CRUD + auth)
│   │   ├── forms.py             # Tweet form & registration form
│   │   ├── urls.py              # App-level URL routing
│   │   ├── admin.py             # Admin panel registration
│   │   └── templates/           # App-specific templates
│   ├── templates/               # Shared templates (layout, auth pages)
│   │   ├── layout.html          # Base template with navbar
│   │   └── registration/        # Login, register, logout pages
│   ├── media/                   # User-uploaded images
│   └── db.sqlite3               # SQLite database file
├── requirements.txt
└── notes.txt                    # My learning notes along the way
```

---

## Challenges & Learnings

**Understanding Django's "magic"** — Django does a *lot* for you behind the scenes. The ORM, form validation, CSRF protection, auth middleware — it took a while to understand what's automatic and what I need to wire up myself.

**Template inheritance clicked late** — I initially duplicated HTML across every page. Then I discovered `{% extends "layout.html" %}` and `{% block content %}` and suddenly everything made sense. One base layout, everything else extends it.

**Image uploads aren't trivial** — You'd think accepting a file upload is simple. Turns out you need `enctype="multipart/form-data"` on the form, Pillow installed, `MEDIA_URL` and `MEDIA_ROOT` configured in settings, and a URL pattern to serve media files during development. Lots of moving parts.

**The `@login_required` decorator is a lifesaver** — Instead of writing `if request.user.is_authenticated` checks everywhere, one decorator handles route protection cleanly. Small thing, but it made the code so much cleaner.

**Ownership-based permissions** — Making sure users can only edit/delete *their own* tweets required passing `user=request.user` to the query. Simple fix, but the kind of thing you don't think about until someone deletes another person's post.

---

## Future Improvements

- 🔍 **Search & hashtags** — Let users discover tweets by topic, not just scroll through everything
- 💬 **Comments & replies** — Turn it into an actual conversation platform
- ❤️ **Likes & bookmarks** — Because engagement features are what make social apps sticky
- 👤 **User profiles** — Profile pictures, bios, and a "your tweets" page
- 📱 **Mobile-first redesign** — The Bootstrap layout works, but a custom responsive design would feel much better
- 🔔 **Real-time updates** — Use Django Channels or WebSockets so the feed updates without refreshing
- 🧪 **Tests** — Add proper unit tests and integration tests (the `tests.py` is still empty and judging me)

---

## Contributing

This is a learning project, but if you want to contribute — awesome! Here's how:

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/cool-thing`)
3. Make your changes
4. Commit with a clear message (`git commit -m "Add cool thing"`)
5. Push and open a Pull Request

No contribution is too small. Fix a typo, improve a template, add a test — it all counts. 🙌

---

## Closing Note

I built TweetBar because I wanted to *actually understand Django*, not just follow along with a tutorial and forget everything the next day.

Every feature in this app taught me something — from how Django's ORM maps Python classes to database tables, to how template inheritance keeps your HTML DRY, to how middleware and decorators can protect routes without cluttering your views.

It's not perfect. The code has rough edges. But that's the point — it's an honest snapshot of learning by building.

If you're learning Django too, I hope this repo gives you a useful reference. Clone it, break it, rebuild it. That's the best way to learn.

Happy coding! ✌️
