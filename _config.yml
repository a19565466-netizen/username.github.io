<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>ورود / ثبت‌نام</title>
<style>
  :root{--bg:#0e1414;--panel:#161d1d;--panel2:#1c2424;--line:#2a3333;--text:#e9efee;--muted:#93a3a1;--accent:#5ee6c5;--err:#ff6b6b;}
  *{box-sizing:border-box;}
  body{margin:0;min-height:100vh;display:flex;align-items:center;justify-content:center;background:var(--bg);color:var(--text);font-family:Tahoma,sans-serif;}
  .box{width:100%;max-width:360px;background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:26px 24px;margin:16px;}
  h2{margin:0 0 4px;font-size:1.2rem;}
  p.sub{margin:0 0 18px;color:var(--muted);font-size:.85rem;}
  label{display:block;font-size:.82rem;color:var(--muted);margin:12px 0 6px;}
  input{width:100%;padding:10px 12px;border-radius:8px;border:1px solid var(--line);background:var(--panel2);color:var(--text);font-size:.92rem;outline:none;}
  input:focus{border-color:var(--accent);}
  button{width:100%;margin-top:18px;padding:10px 14px;border-radius:9px;border:none;background:var(--accent);color:#06231d;font-weight:700;font-size:.92rem;cursor:pointer;}
  button:disabled{opacity:.6;cursor:default;}
  .switch{margin-top:14px;text-align:center;font-size:.82rem;color:var(--muted);}
  .switch a{color:var(--accent);cursor:pointer;text-decoration:none;}
  .err{color:var(--err);font-size:.82rem;min-height:1.2em;margin-top:10px;}
</style>
</head>
<body>

<div class="box">
  <h2 id="title">ورود</h2>
  <p class="sub" id="sub">وارد حسابت شو</p>
  <label>ایمیل</label>
  <input id="email" type="email" autocomplete="email">
  <label>رمز عبور</label>
  <input id="password" type="password" autocomplete="current-password">
  <button id="submitBtn">ورود</button>
  <div class="err" id="err"></div>
  <div class="switch"><a id="switchLink">حساب نداری؟ ثبت‌نام کن</a></div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
  import {
    getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword,
    onAuthStateChanged
  } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";
  import { firebaseConfig } from "./firebase-config.js";

  const app = initializeApp(firebaseConfig);
  const auth = getAuth(app);

  let mode = "login";
  const title = document.getElementById("title");
  const sub = document.getElementById("sub");
  const submitBtn = document.getElementById("submitBtn");
  const switchLink = document.getElementById("switchLink");
  const err = document.getElementById("err");
  const emailInput = document.getElementById("email");
  const passInput = document.getElementById("password");

  function setMode(m){
    mode = m;
    err.textContent = "";
    if(mode === "login"){
      title.textContent = "ورود";
      sub.textContent = "وارد حسابت شو";
      submitBtn.textContent = "ورود";
      switchLink.textContent = "حساب نداری؟ ثبت‌نام کن";
    } else {
      title.textContent = "ثبت‌نام";
      sub.textContent = "یه حساب تازه بساز";
      submitBtn.textContent = "ثبت‌نام";
      switchLink.textContent = "حساب داری؟ وارد شو";
    }
  }
  switchLink.addEventListener("click", () => setMode(mode === "login" ? "signup" : "login"));

  submitBtn.addEventListener("click", async () => {
    err.textContent = "";
    submitBtn.disabled = true;
    const email = emailInput.value.trim();
    const password = passInput.value;
    try{
      if(mode === "login"){
        await signInWithEmailAndPassword(auth, email, password);
      } else {
        await createUserWithEmailAndPassword(auth, email, password);
      }
      location.href = "dashboard.html";
    }catch(e){
      err.textContent = translateError(e.code);
    }finally{
      submitBtn.disabled = false;
    }
  });

  function translateError(code){
    const map = {
      "auth/invalid-email": "ایمیل معتبر نیست",
      "auth/user-not-found": "حسابی با این ایمیل پیدا نشد",
      "auth/wrong-password": "رمز عبور اشتباه است",
      "auth/email-already-in-use": "این ایمیل قبلاً ثبت‌نام کرده",
      "auth/weak-password": "رمز عبور باید حداقل ۶ کاراکتر باشد"
    };
    return map[code] || "خطایی رخ داد، دوباره امتحان کن";
  }

  onAuthStateChanged(auth, (user) => {
    if(user) location.href = "dashboard.html";
  });

  setMode("login");
</script>
</body>
</html>
