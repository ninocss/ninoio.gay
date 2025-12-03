# ninoio.gay

Static website for **ninoio** hosted via GitHub Pages (custom domain: `ninoio.gay`).  
The site has a small “humanity verification” flow made of three mini‑tests, gated via cookies.

## Structure

- [index.html](index.html)  
  Main homepage (projects, about, particles background).  
  Redirects to the test login if the cookie `final=true` is not set.

- [static/css/](static/css)
  - `home.css` – styling for the homepage.
  - `index.css` – styling for the login / entry page.
  - `aim.css` – aim test (Test 1) styling.
  - `iq.css` – IQ test (Test 2) styling.
  - `general.css` – general knowledge quiz (Test 3) styling.
  - `imp.css` – shared base styles (imported by other CSS files).

- [static/test/login/index.html](static/test/login/index.html)  
  “Verify humanity” entry page.  
  - Starts the test flow with **Test 1**.
  - Allows skipping and “remembering” the decision via cookies:
    - `final=true|false`
    - `skip=true|false`

- [static/test/first/index.html](static/test/first/index.html) – **Test 1: Aim Test**
  - Simple aim game with moving containers.
  - On completion (or skip) sets `taskCompletedTest1=true` and redirects to Test 2.

- [static/test/second/index.html](static/test/second/index.html) – **Test 2: IQ Test**
  - Multiple‑choice IQ quiz.
  - Uses Firebase Firestore collection `questions`.
  - On completion (or skip) sets `taskCompletedTest2=true` and redirects to Test 3.

- [static/test/third/index.html](static/test/third/index.html) – **Test 3: General Knowledge**
  - Multiple‑choice quiz with progress bar and feedback.
  - Uses Firebase Firestore collection `questions_test3`.
  - On completion sets `final=true` and redirects back to `/`.

- [.well-known/discord](.well-known/discord)  
  Discord verification file.

- [CNAME](CNAME)  
  Custom domain configuration (`ninoio.gay`).

## Test Flow & Cookies

1. **Entry**: `/static/test/login/`
2. **Test 1 (Aim)**: `/static/test/first/`  
   - Sets `taskCompletedTest1=true`.
3. **Test 2 (IQ)**: `/static/test/second/`  
   - Requires `taskCompletedTest1=true`.  
   - Sets `taskCompletedTest2=true`.
4. **Test 3 (General Knowledge)**: `/static/test/third/`  
   - Requires `taskCompletedTest2=true`.  
   - Sets `final=true` and returns to `/`.

The homepage `/` checks `final=true` and otherwise redirects back to `/static/test/login/`.

## Development

This is a plain static site (HTML/CSS/JS only). You can run it locally with any static file server, for example:

```sh
# from the project root
python -m http.server 8000
# then open http://localhost:8000/
```

Ensure you configure Firebase Firestore with the collections:

- `questions` (for Test 2)
- `questions_test3` (for Test 3)

and that documents follow the shape:

```js
{
  question: "Question text",
  answers: ["A", "B", "C", "D"],
  correct: 0 // index into answers
}
```

## License

No explicit license specified. All rights reserved by ninoio.