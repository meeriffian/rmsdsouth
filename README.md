<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DSC Karachi South — Complaint Portal</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<style>
:root{
  --navy:#0b1f3a;--navy-mid:#12305c;--navy-light:#1a4580;
  --gold:#c9a44a;--gold-lt:#e8c96a;--gold-pale:#faf3dc;
  --white:#fff;--bg:#f5f2eb;--gray-1:#ede9e0;--gray-2:#d5d0c6;--gray-4:#9a9590;--gray-6:#5c5852;
  --ok:#1a6e3f;--ok-bg:#eaf4ee;--warn:#8a5800;--warn-bg:#fff3d6;
  --err:#8a1c1c;--err-bg:#fdf0f0;--info:#1a4f8a;--info-bg:#e8f0fb;
  --fd:'Playfair Display',Georgia,serif;--fb:'DM Sans','Segoe UI',sans-serif;
  --sh:0 2px 10px rgba(11,31,58,.09);--sm:0 4px 22px rgba(11,31,58,.14);
  --r:8px;--rl:14px;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{min-height:100%;font-family:var(--fb);background:var(--bg);color:var(--navy)}
#toastBox{position:fixed;top:18px;right:18px;z-index:9999;display:flex;flex-direction:column;gap:10px;pointer-events:none}
.toast{padding:12px 20px;border-radius:var(--r);font-size:.86rem;font-weight:500;color:#fff;box-shadow:var(--sm);animation:tIn .3s ease;max-width:320px}
.toast.s{background:#1a6e3f}.toast.e{background:#8a1c1c}.toast.i{background:var(--navy)}
@keyframes tIn{from{opacity:0;transform:translateX(30px)}to{opacity:1;transform:none}}
#loader{position:fixed;inset:0;background:var(--navy);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9998;transition:opacity .5s}
#loader.hide{opacity:0;pointer-events:none}
.loader-emblem{font-size:56px;animation:pulse 1.4s infinite}
.loader-title{font-family:var(--fd);color:var(--gold-lt);font-size:1.1rem;margin-top:16px;text-align:center;padding:0 24px}
.loader-bar{width:180px;height:3px;background:rgba(255,255,255,.15);border-radius:2px;margin-top:20px;overflow:hidden}
.loader-fill{height:100%;background:var(--gold);animation:lFill 1.4s ease forwards}
@keyframes lFill{from{width:0}to{width:100%}}
@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.08)}}
.hdr{background:linear-gradient(135deg,var(--navy) 0%,var(--navy-light) 100%);border-bottom:3px solid var(--gold);box-shadow:var(--sm);position:sticky;top:0;z-index:900}
.hdr-top{display:flex;align-items:center;justify-content:space-between;padding:12px 24px;gap:12px;flex-wrap:wrap}
.hdr-logo{display:flex;align-items:center;gap:12px}
.emblem{width:50px;height:50px;border-radius:50%;background:var(--gold);display:flex;align-items:center;justify-content:center;font-size:22px;color:var(--navy);flex-shrink:0;box-shadow:0 0 0 3px rgba(201,164,74,.3)}
.court-name h1{font-family:var(--fd);color:var(--white);font-size:1rem;font-weight:700;line-height:1.25}
.court-name p{font-size:.74rem;color:var(--gold-lt);margin-top:2px}
.hdr-nav{display:flex;gap:6px;flex-wrap:wrap;align-items:center}
.nb{background:rgba(255,255,255,.08);border:1px solid rgba(201,164,74,.3);color:var(--gold-lt);font-family:var(--fb);font-size:.8rem;padding:7px 13px;border-radius:6px;cursor:pointer;transition:all .2s;font-weight:500}
.nb:hover,.nb.on{background:var(--gold);color:var(--navy);border-color:var(--gold)}
.hdr-bar{background:rgba(0,0,0,.22);padding:5px 24px;display:flex;align-items:center;gap:18px;font-size:.73rem;color:var(--gold-pale);border-top:1px solid rgba(201,164,74,.12);flex-wrap:wrap}
.view{display:none}.view.on{display:block}
/* LOGIN */
.login-wrap{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px;background:linear-gradient(160deg,var(--navy) 0%,var(--navy-mid) 60%,var(--navy-light) 100%)}
.login-card{background:var(--white);border-radius:var(--rl);padding:40px 34px;max-width:420px;width:100%;box-shadow:var(--sm)}
.login-tabs{display:flex;gap:0;margin-bottom:24px;border:2px solid var(--gray-2);border-radius:var(--r);overflow:hidden}
.ltab{flex:1;padding:10px;text-align:center;cursor:pointer;font-size:.84rem;font-weight:600;font-family:var(--fb);background:var(--white);border:none;color:var(--gray-6);transition:all .2s}
.ltab.on{background:var(--navy);color:var(--white)}
/* HERO */
.hero{background:linear-gradient(160deg,var(--navy) 0%,var(--navy-light) 60%,#1e4d8c 100%);padding:44px 24px 36px;text-align:center;position:relative;overflow:hidden}
.hero::before{content:'⚖';position:absolute;font-size:200px;opacity:.04;top:-20px;right:-10px;line-height:1}
.hero h2{font-family:var(--fd);color:var(--white);font-size:1.8rem;margin-bottom:8px}
.hero p{color:var(--gold-lt);font-size:.93rem;max-width:480px;margin:0 auto}
/* CARDS */
.cp{background:var(--white);border-radius:var(--rl);border:1px solid var(--gray-2);box-shadow:var(--sh);overflow:hidden}
.ch{background:var(--navy);color:var(--white);padding:12px 18px;display:flex;align-items:center;gap:10px;border-bottom:2px solid var(--gold)}
.ch .ico{color:var(--gold);font-size:.95rem}
.ch h3{font-family:var(--fd);font-size:.95rem;font-weight:600}
.cb{padding:18px}
/* FORM */
.fl{font-weight:500;font-size:.85rem;color:var(--navy);margin-bottom:5px;display:block}
.fl .r{color:#c0392b;margin-left:3px}
.fc,.fs{border:1.5px solid var(--gray-2);border-radius:var(--r);font-family:var(--fb);font-size:.88rem;color:var(--navy);padding:9px 12px;transition:border-color .2s,box-shadow .2s;background:var(--white);width:100%}
.fc:focus,.fs:focus{border-color:var(--gold);box-shadow:0 0 0 3px rgba(201,164,74,.18);outline:none}
.fc.err{border-color:#c0392b}
.frow{margin-bottom:13px}
/* BUTTONS */
.bg{background:linear-gradient(135deg,var(--gold) 0%,var(--gold-lt) 100%);color:var(--navy);border:none;font-weight:600;font-family:var(--fb);padding:10px 24px;border-radius:var(--r);cursor:pointer;transition:all .2s;font-size:.92rem;box-shadow:0 2px 8px rgba(201,164,74,.32)}
.bg:hover{transform:translateY(-1px);box-shadow:0 4px 14px rgba(201,164,74,.44)}
.bg:disabled{opacity:.5;cursor:not-allowed;transform:none}
.bn{background:var(--navy);color:var(--white);border:none;font-weight:500;font-family:var(--fb);padding:9px 16px;border-radius:var(--r);cursor:pointer;transition:all .2s;font-size:.85rem}
.bn:hover{background:var(--navy-light)}
.bog{background:transparent;color:var(--gold);border:1.5px solid var(--gold);font-family:var(--fb);padding:7px 14px;border-radius:var(--r);cursor:pointer;transition:all .2s;font-size:.84rem;font-weight:500}
.bog:hover{background:var(--gold);color:var(--navy)}
/* CATEGORY */
.cat-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:9px}
.cat-btn{border:1.5px solid var(--gray-2);background:var(--white);border-radius:var(--r);padding:12px 6px;cursor:pointer;transition:all .2s;text-align:center;font-family:var(--fb)}
.cat-btn:hover{border-color:var(--gold);background:var(--gold-pale)}
.cat-btn.sel{background:var(--navy);border-color:var(--navy);color:var(--white)}
.cat-btn .ci{font-size:1.5rem;display:block;margin-bottom:4px}
.cat-btn .cn{font-size:.79rem;font-weight:600}
.sub-row{display:none;gap:10px;margin-top:9px}
.sub-row.vis{display:flex}
.chk-btn{flex:1;display:flex;align-items:center;justify-content:center;gap:7px;padding:10px 6px;border:2px solid var(--gray-2);border-radius:var(--r);background:var(--white);cursor:pointer;font-family:var(--fb);font-size:.86rem;font-weight:600;color:var(--navy);transition:all .2s}
.chk-btn:hover{border-color:var(--gold);background:var(--gold-pale)}
.chk-btn.chk{background:var(--navy);color:var(--white);border-color:var(--navy)}
.cat-sel{display:none;margin-top:9px;padding:9px 13px;background:var(--ok-bg);border:1.5px solid var(--ok);border-radius:var(--r);font-size:.85rem;color:var(--ok);font-weight:600;align-items:center;justify-content:space-between}
.cat-sel.vis{display:flex}
/* STATS */
.sg{display:grid;grid-template-columns:repeat(2,1fr);gap:11px;margin-bottom:16px}
.sc{background:var(--white);border-radius:var(--rl);border:1px solid var(--gray-2);padding:15px;text-align:center;box-shadow:var(--sh);transition:transform .2s}
.sc:hover{transform:translateY(-2px)}
.sn{font-size:1.8rem;font-weight:700;font-family:var(--fd);line-height:1}
.sl{font-size:.76rem;color:var(--gray-6);margin-top:4px;font-weight:500}
.sc.t .sn{color:var(--info)}.sc.p .sn{color:var(--warn)}.sc.i .sn{color:#1a5ea8}.sc.r .sn{color:var(--ok)}
/* BADGES */
.bs{display:inline-flex;align-items:center;gap:4px;padding:3px 9px;border-radius:20px;font-size:.71rem;font-weight:600}
.bp{background:var(--warn-bg);color:var(--warn)}.bi{background:var(--info-bg);color:var(--info)}.br{background:var(--ok-bg);color:var(--ok)}
/* COMPLAINT CARDS */
.ci-card{background:var(--white);border-radius:var(--rl);border:1px solid var(--gray-2);padding:14px;margin-bottom:9px;box-shadow:var(--sh);transition:all .2s}
.ci-card:hover{border-color:var(--gold);box-shadow:var(--sm)}
.ci-top{display:flex;justify-content:space-between;align-items:flex-start;gap:8px;margin-bottom:7px;flex-wrap:wrap}
.ci-id{font-family:var(--fd);font-size:.93rem;font-weight:700;color:var(--navy)}
.ci-meta{font-size:.78rem;color:var(--gray-6);margin-bottom:5px}
.ci-actions{display:flex;gap:7px;flex-wrap:wrap;margin-top:9px}
.ab{padding:6px 11px;border-radius:6px;font-size:.76rem;font-weight:600;cursor:pointer;border:none;font-family:var(--fb);transition:all .2s}
.ab:hover{opacity:.82}
.ab-view{background:var(--info-bg);color:var(--info)}
.ab-prog{background:var(--warn-bg);color:var(--warn)}
.ab-del{background:var(--err-bg);color:var(--err)}
.ab-dj{background:#f0e8f8;color:#5b2d8e}
.ab-dj-done{background:#e8f4e8;color:var(--ok);cursor:default}
.ab-cert{background:var(--gold-pale);color:var(--warn)}
.ab-print{background:var(--info-bg);color:var(--info)}
/* ADMIN TABS */
.atabs{display:flex;gap:4px;background:var(--navy);padding:5px;border-radius:var(--r);margin-bottom:16px;flex-wrap:wrap}
.at{background:transparent;border:none;color:rgba(255,255,255,.6);font-family:var(--fb);font-size:.78rem;padding:7px 12px;border-radius:6px;cursor:pointer;font-weight:500;transition:all .2s;white-space:nowrap}
.at.on{background:var(--gold);color:var(--navy);font-weight:700}
/* FILTER */
.fbar{display:flex;gap:9px;margin-bottom:14px;flex-wrap:wrap}
.fbar input,.fbar select{flex:1;min-width:140px}
/* MODAL */
.mo{position:fixed;inset:0;background:rgba(11,31,58,.55);backdrop-filter:blur(3px);z-index:800;display:none;align-items:center;justify-content:center;padding:16px}
.mo.on{display:flex}
.mb{background:var(--white);border-radius:var(--rl);width:100%;max-width:540px;max-height:92vh;overflow-y:auto;box-shadow:var(--sm)}
.mh{background:var(--navy);color:var(--white);padding:13px 18px;display:flex;justify-content:space-between;align-items:center;border-bottom:2px solid var(--gold);position:sticky;top:0;z-index:1}
.mh h4{font-family:var(--fd);font-size:.95rem}
.mbody{padding:16px 18px}
.mf{padding:12px 18px;display:flex;gap:9px;justify-content:flex-end;border-top:1px solid var(--gray-1)}
.mx{background:none;border:none;color:var(--gold-lt);font-size:1rem;cursor:pointer}
.mx:hover{color:var(--white)}
.info-row{display:flex;align-items:flex-start;padding:9px 0;border-bottom:1px solid var(--gray-1)}
.info-row:last-child{border-bottom:none}
.ik{min-width:120px;font-size:.76rem;font-weight:600;color:var(--gray-6);text-transform:uppercase;letter-spacing:.4px;padding-top:2px}
.iv{font-size:.88rem;color:var(--navy);font-weight:500;word-break:break-word;flex:1}
/* ACCOUNTS */
.acc-tabs{display:flex;gap:9px;margin-bottom:16px}
.act{flex:1;padding:11px;border-radius:var(--r);border:2px solid var(--gray-2);background:var(--white);cursor:pointer;text-align:center;font-family:var(--fb);font-size:.84rem;font-weight:600;transition:all .2s}
.act.inc.on{background:var(--ok-bg);border-color:var(--ok);color:var(--ok)}
.act.exp.on{background:var(--err-bg);border-color:var(--err);color:var(--err)}
.bal-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:11px;margin-bottom:16px}
.bal-card{background:var(--white);border-radius:var(--r);padding:13px;text-align:center;border:1px solid var(--gray-2)}
.bal-num{font-size:1.2rem;font-weight:700;font-family:var(--fd)}
.bal-lbl{font-size:.72rem;color:var(--gray-6);font-weight:500;margin-top:3px}
.ledger-item{background:var(--white);border-radius:var(--r);border:1px solid var(--gray-2);padding:13px;margin-bottom:7px;display:flex;justify-content:space-between;align-items:flex-start;gap:9px;flex-wrap:wrap}
.li-inc{border-left:4px solid var(--ok)}.li-exp{border-left:4px solid var(--err)}
/* TRACK */
.tick-badge{display:inline-flex;align-items:center;gap:8px;background:var(--gold-pale);color:var(--navy);border:1.5px solid var(--gold);padding:9px 20px;border-radius:30px;font-weight:700;font-size:1.05rem;letter-spacing:1px;margin:14px 0}
/* COURT PORTAL */
.court-banner{background:linear-gradient(135deg,var(--navy-mid),var(--navy-light));border-bottom:2px solid var(--gold);padding:10px 24px;display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.court-badge{background:var(--gold);color:var(--navy);padding:6px 16px;border-radius:20px;font-size:.82rem;font-weight:700;font-family:var(--fd)}
.sw{max-width:700px;margin:0 auto;padding:22px 15px}
.chart-row{display:flex;align-items:center;gap:9px;margin-bottom:7px;font-size:.8rem}
.chart-lbl{min-width:76px;color:var(--gray-6);font-weight:500}
.chart-bar{height:20px;border-radius:4px;transition:width .6s ease;min-width:4px;display:flex;align-items:center;padding-left:7px;font-size:.73rem;font-weight:600;color:white}
.bk{display:none;align-items:center;gap:5px;background:rgba(255,255,255,.1);border:1px solid rgba(201,164,74,.35);color:var(--gold-lt);font-family:var(--fb);font-size:.78rem;padding:6px 11px;border-radius:6px;cursor:pointer;transition:all .2s}
.bk:hover{background:var(--gold);color:var(--navy)}
.bk.vis{display:flex}
.notif-btn{position:relative;background:rgba(255,255,255,.1);border:1px solid rgba(201,164,74,.3);color:var(--gold-lt);padding:7px 10px;border-radius:6px;cursor:pointer;font-size:.88rem}
.ndot{position:absolute;top:-3px;right:-3px;width:9px;height:9px;background:#e74c3c;border-radius:50%;display:none;border:2px solid var(--navy)}
.ndot.vis{display:block}
#nDrop{position:fixed;top:68px;right:14px;width:290px;background:var(--white);border-radius:var(--rl);box-shadow:var(--sm);z-index:850;display:none;border:1px solid var(--gray-2);max-height:320px;overflow-y:auto}
#nDrop.on{display:block}
.gd{border:none;border-top:1px solid var(--gray-2);margin:14px 0}
.ft{background:var(--navy);color:var(--gold-pale);text-align:center;padding:14px;font-size:.76rem;border-top:2px solid var(--gold);margin-top:28px}
.ft strong{color:var(--gold-lt)}
/* COURT USERS TABLE */
.users-table{width:100%;border-collapse:collapse;font-size:.82rem}
.users-table th{background:var(--navy);color:white;padding:8px 10px;text-align:left}
.users-table td{padding:8px 10px;border-bottom:1px solid var(--gray-1)}
.users-table tr:hover td{background:var(--gold-pale)}
@media(max-width:600px){
  .hdr-top{padding:9px 12px}.court-name h1{font-size:.85rem}.hdr-bar{display:none}
  .sw{padding:14px 11px}.sg{grid-template-columns:repeat(2,1fr)}.cat-grid{grid-template-columns:repeat(2,1fr)}
  .bal-grid{grid-template-columns:repeat(3,1fr)}.at{font-size:.7rem;padding:6px 7px}.hero h2{font-size:1.4rem}
  .login-card{padding:26px 18px}.mb{max-width:100%;margin:8px}
}
@media(min-width:900px){
  .cat-grid{grid-template-columns:repeat(5,1fr)}.sg{grid-template-columns:repeat(4,1fr)}.sw{max-width:760px;padding:26px 18px}
}
</style>
</head>
<body>

<div id="loader">
  <div class="loader-emblem">⚖️</div>
  <div class="loader-title">District &amp; Sessions Court Karachi South<br>Complaint Management System</div>
  <div class="loader-bar"><div class="loader-fill"></div></div>
</div>
<div id="toastBox"></div>

<!-- ═══ LOGIN VIEW ═══ -->
<div id="vLogin" class="view">
  <div class="login-wrap">
    <div class="login-card">
      <span style="font-size:46px;text-align:center;display:block;margin-bottom:14px">⚖️</span>
      <h2 style="font-family:var(--fd);text-align:center;font-size:1.3rem;color:var(--navy);margin-bottom:3px">DSC Karachi South</h2>
      <p style="text-align:center;font-size:.8rem;color:var(--gray-6);margin-bottom:20px">Complaint Management System</p>
      <div class="login-tabs">
        <button class="ltab on" id="ltab-court" onclick="setLoginTab('court')"><i class="fas fa-gavel" style="margin-right:5px"></i>Court / Branch</button>
        <button class="ltab" id="ltab-admin" onclick="setLoginTab('admin')"><i class="fas fa-shield-halved" style="margin-right:5px"></i>Admin</button>
      </div>
      <div id="loginForm">
        <div class="frow">
          <label class="fl" id="loginUserLbl">Court Username <span class="r">*</span></label>
          <input type="text" id="lUser" class="fc" placeholder="Enter username" onkeydown="if(event.key==='Enter')doLogin()">
        </div>
        <div class="frow">
          <label class="fl">Password <span class="r">*</span></label>
          <div style="position:relative">
            <input type="password" id="lPass" class="fc" placeholder="Enter password" onkeydown="if(event.key==='Enter')doLogin()">
            <button onclick="togglePass()" style="position:absolute;right:10px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;color:var(--gray-4)"><i id="eyeIco" class="fas fa-eye"></i></button>
          </div>
        </div>
        <div id="lockMsg" style="display:none;background:var(--err-bg);color:var(--err);font-size:.8rem;padding:8px 12px;border-radius:var(--r);margin-bottom:12px;text-align:center"></div>
        <button class="bg" id="loginBtn" onclick="doLogin()" style="width:100%;padding:12px">
          <i class="fas fa-right-to-bracket" style="margin-right:7px"></i>Login
        </button>
      </div>
    </div>
  </div>
</div>

<!-- ═══ HEADER ═══ -->
<div id="mainHdr" style="display:none">
  <header class="hdr">
    <div class="hdr-top">
      <div class="hdr-logo">
        <div class="emblem">⚖️</div>
        <div class="court-name">
          <h1>DISTRICT &amp; SESSIONS COURT KARACHI SOUTH</h1>
          <p>Repair &amp; Maintenance Complaint Portal</p>
        </div>
      </div>
      <nav class="hdr-nav">
        <button class="bk" id="backBtn" onclick="goBack()"><i class="fas fa-arrow-left"></i> Back</button>
        <button class="nb on" id="nCourt" onclick="switchTo('court')" style="display:none"><i class="fas fa-file-pen" style="margin-right:5px"></i>My Complaints</button>
        <button class="nb" id="nTrack" onclick="switchTo('track')"><i class="fas fa-search" style="margin-right:5px"></i>Track</button>
        <button class="nb" id="nAdmin" onclick="switchTo('admin')" style="display:none"><i class="fas fa-gauge" style="margin-right:5px"></i>Admin</button>
        <button class="nb" id="nLogout" onclick="doLogout()" style="display:none"><i class="fas fa-right-from-bracket" style="margin-right:5px"></i>Logout</button>
        <button class="notif-btn" id="nBell" onclick="toggleNotif()" style="display:none"><i class="fas fa-bell"></i><span class="ndot" id="nDot"></span></button>
      </nav>
    </div>
    <div class="hdr-bar">
      <span><i class="fas fa-map-marker-alt"></i> Karachi South, Sindh</span>
      <span><i class="fas fa-clock"></i> <span id="hdrTime"></span></span>
      <span id="syncStatus" style="color:var(--gold-lt)"></span>
      <span id="loggedInAs" style="color:var(--gold);font-weight:600"></span>
    </div>
  </header>
  <div id="nDrop">
    <div style="padding:11px 15px;border-bottom:1px solid var(--gray-1);font-weight:600;font-size:.83rem;color:var(--navy);display:flex;justify-content:space-between">
      Notifications <button onclick="clearNotifs()" style="background:none;border:none;font-size:.73rem;color:var(--gray-4);cursor:pointer">Clear all</button>
    </div>
    <div id="nList"></div>
  </div>
</div>

<!-- ═══ COURT VIEW ═══ -->
<div id="vCourt" class="view">
  <div id="courtBanner" class="court-banner">
    <i class="fas fa-gavel" style="color:var(--gold);font-size:1.1rem"></i>
    <span style="color:var(--gold-lt);font-size:.82rem;font-weight:500">Logged in as:</span>
    <span class="court-badge" id="courtBadge">Court Name</span>
    <span style="margin-left:auto;color:var(--gold-lt);font-size:.76rem" id="courtComplaintCount"></span>
  </div>

  <div id="courtSuccess" style="display:none">
    <div class="sw">
      <div class="cp" style="text-align:center;padding:34px 22px">
        <div style="font-size:50px;margin-bottom:12px">✅</div>
        <h3 style="font-family:var(--fd);color:var(--navy);font-size:1.35rem;margin-bottom:7px">Complaint Submitted!</h3>
        <p style="color:var(--gray-6);font-size:.88rem;margin-bottom:16px">Please save your complaint number.</p>
        <div style="font-size:.78rem;font-weight:600;color:var(--gray-6);text-transform:uppercase;letter-spacing:1px;margin-bottom:4px"><i class="fas fa-hashtag" style="margin-right:5px;color:var(--gold)"></i>Complaint Number</div>
        <div class="tick-badge"><span id="genTicket">DSC-000001</span></div>
        <div style="margin:10px 0 22px">
          <button onclick="copyId(document.getElementById('genTicket').textContent)" class="bog" style="font-size:.8rem"><i class="fas fa-copy" style="margin-right:5px"></i>Copy Number</button>
        </div>
        <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
          <button class="bn" onclick="switchTo('track')"><i class="fas fa-search" style="margin-right:5px"></i>Track Status</button>
          <button class="bg" onclick="newComplaint()"><i class="fas fa-plus" style="margin-right:5px"></i>New Complaint</button>
        </div>
      </div>
    </div>
  </div>

  <div id="courtForm">
    <div class="hero" style="padding:30px 24px 24px">
      <h2 style="font-size:1.5rem">File a Complaint</h2>
      <p id="courtHeroSub">Submit your repair &amp; maintenance request</p>
    </div>
    <div class="sw">
      <div class="cp">
        <div class="ch"><i class="fas fa-file-pen ico"></i><h3>Complaint Registration Form</h3></div>
        <div class="cb">
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:13px">
            <div class="frow" style="margin-bottom:0">
              <label class="fl">Full Name <span class="r">*</span></label>
              <input type="text" id="cName" class="fc" placeholder="e.g. Irfan Ahmed Memon" oninput="validateName(this)">
              <span id="cNameErr" style="font-size:.73rem;color:#c0392b;display:none;margin-top:2px"></span>
            </div>
            <div class="frow" style="margin-bottom:0">
              <label class="fl">Designation <span class="r">*</span></label>
              <input type="text" id="cDesig" class="fc" placeholder="e.g. Clerk, Naib Qasid">
            </div>
          </div>
          <!-- Department shown but pre-filled from court login -->
          <div class="frow" style="margin-top:13px">
            <label class="fl">Department / Court</label>
            <input type="text" id="cDept" class="fc" readonly style="background:var(--gray-1);color:var(--gray-6);font-weight:600">
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:13px">
            <div class="frow" style="margin-bottom:0">
              <label class="fl">Block / Floor</label>
              <input type="text" id="cBlock" class="fc" placeholder="e.g. Block A, 2nd Floor">
            </div>
            <div class="frow" style="margin-bottom:0">
              <label class="fl">Contact Number</label>
              <input type="tel" id="cPhone" class="fc" placeholder="03XX-XXXXXXX" oninput="validatePhone(this)">
              <span id="cPhoneErr" style="font-size:.73rem;color:#c0392b;display:none;margin-top:2px"></span>
            </div>
          </div>
          <div class="frow" style="margin-top:13px">
            <label class="fl">Complaint Category <span class="r">*</span></label>
            <input type="hidden" id="cCategory">
            <div class="cat-grid" id="catGrid">
              <button type="button" class="cat-btn" data-cat="Electrical" onclick="selectCat(this)"><span class="ci">⚡</span><span class="cn">Electrical</span></button>
              <button type="button" class="cat-btn" data-cat="Plumbing" onclick="selectCat(this)"><span class="ci">🔧</span><span class="cn">Plumbing</span></button>
              <button type="button" class="cat-btn" data-cat="Furniture" onclick="selectCat(this)"><span class="ci">🪑</span><span class="cn">Furniture</span></button>
              <button type="button" class="cat-btn" data-cat="Civil" onclick="selectCat(this)"><span class="ci">🏗️</span><span class="cn">Civil</span></button>
              <button type="button" class="cat-btn" data-cat="Others" onclick="selectCat(this,'noSub')"><span class="ci">📋</span><span class="cn">Others</span></button>
            </div>
            <div class="sub-row" id="subRow">
              <button type="button" class="chk-btn" id="subR1" onclick="selectSub('Repair',this)"><i class="fas fa-wrench"></i> Repair</button>
              <button type="button" class="chk-btn" id="subR2" onclick="selectSub('Replace',this)"><i class="fas fa-arrows-rotate"></i> Replace</button>
            </div>
            <div class="cat-sel" id="catSelBox">
              <span><i class="fas fa-circle-check" style="margin-right:6px"></i><span id="catSelTxt"></span></span>
              <button type="button" onclick="clearCat()" style="background:none;border:none;color:var(--ok);cursor:pointer;font-size:.76rem;text-decoration:underline">Change</button>
            </div>
          </div>
          <div class="frow" style="margin-top:13px">
            <label class="fl">Complaint Description <span class="r">*</span></label>
            <div style="border:1.5px solid var(--gray-2);border-radius:var(--r);overflow:hidden">
              <textarea id="cDesc" class="fc" style="border:none;outline:none;border-radius:0;resize:vertical;min-height:100px" placeholder="Describe the issue clearly..." oninput="document.getElementById('charCnt').textContent=this.value.length+' chars'"></textarea>
              <div style="background:var(--gray-1);padding:4px 12px;display:flex;justify-content:space-between;font-size:.7rem;color:var(--gray-4)">
                <span><i class="fas fa-info-circle" style="margin-right:3px"></i>Be specific for faster resolution</span>
                <span id="charCnt">0 chars</span>
              </div>
            </div>
          </div>
          <div class="frow">
            <label class="fl">Photo / Attachment <span style="font-size:.72rem;color:var(--gray-4)">(Optional — Max 2MB)</span></label>
            <input type="file" id="cImg" class="fc" accept="image/*" onchange="handleImg(this)">
            <div id="imgPrev" style="display:none;margin-top:7px">
              <img id="imgSrc" style="max-width:100%;max-height:180px;border-radius:var(--r);border:1px solid var(--gray-2)" src="" alt="">
              <button type="button" onclick="clearImg()" style="margin-top:5px;display:block;background:none;border:none;color:var(--err);font-size:.78rem;cursor:pointer"><i class="fas fa-times" style="margin-right:3px"></i>Remove</button>
            </div>
          </div>
          <button class="bg" id="submitBtn" onclick="submitComplaint()" style="width:100%;padding:12px;font-size:.94rem;margin-top:4px">
            <i class="fas fa-paper-plane" style="margin-right:7px"></i>Submit Complaint
          </button>
        </div>
      </div>

      <!-- Court's own complaints list -->
      <div style="margin-top:18px">
        <div class="cp">
          <div class="ch"><i class="fas fa-list ico"></i><h3>My Court's Complaints</h3></div>
          <div class="cb">
            <div id="courtList"><p style="font-size:.84rem;color:var(--gray-4)">Loading...</p></div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <footer class="ft">Designed &amp; Developed by <strong>Irfan Ahmed Memon</strong> — Jr. Clerk &nbsp;|&nbsp; 0335-2345183 &nbsp;|&nbsp; District &amp; Sessions Court Karachi South</footer>
</div>

<!-- ═══ TRACK VIEW ═══ -->
<div id="vTrack" class="view">
  <div class="hero" style="padding:34px 22px 28px">
    <h2 style="font-size:1.6rem">Track Your Complaint</h2>
    <p>Enter your Complaint Number to check current status</p>
  </div>
  <div class="sw">
    <div class="cp">
      <div class="ch"><i class="fas fa-magnifying-glass ico"></i><h3>Complaint Status Tracker</h3></div>
      <div class="cb">
        <div style="display:flex;gap:9px;margin-bottom:16px">
          <input type="text" id="trackIn" class="fc" placeholder="Enter Complaint Number e.g. DSC-000001" style="flex:1;text-transform:uppercase" onkeydown="if(event.key==='Enter')trackComplaint()">
          <button class="bn" onclick="trackComplaint()"><i class="fas fa-search" style="margin-right:4px"></i>Track</button>
        </div>
        <div id="trackResult"></div>
      </div>
    </div>
  </div>
  <footer class="ft">Designed &amp; Developed by <strong>Irfan Ahmed Memon</strong> — District &amp; Sessions Court Karachi South</footer>
</div>

<!-- ═══ ADMIN VIEW ═══ -->
<div id="vAdmin" class="view">
  <div class="sw" style="max-width:820px">
    <div class="atabs">
      <button class="at on" id="at-dash" onclick="aTab('dash')"><i class="fas fa-gauge" style="margin-right:4px"></i>Dashboard</button>
      <button class="at" id="at-list" onclick="aTab('list')"><i class="fas fa-list" style="margin-right:4px"></i>All Complaints</button>
      <button class="at" id="at-accounts" onclick="aTab('accounts')"><i class="fas fa-coins" style="margin-right:4px"></i>Accounts</button>
      <button class="at" id="at-report" onclick="aTab('report')"><i class="fas fa-file-chart-column" style="margin-right:4px"></i>Reports</button>
      <button class="at" id="at-users" onclick="aTab('users')"><i class="fas fa-users" style="margin-right:4px"></i>Court Users</button>
      <button class="at" id="at-settings" onclick="aTab('settings')"><i class="fas fa-gear" style="margin-right:4px"></i>Settings</button>
    </div>

    <!-- DASHBOARD -->
    <div id="tab-dash">
      <div class="sg">
        <div class="sc t"><div class="sn" id="stTotal">0</div><div class="sl"><i class="fas fa-inbox" style="margin-right:3px"></i>Total</div></div>
        <div class="sc p"><div class="sn" id="stPend">0</div><div class="sl"><i class="fas fa-clock" style="margin-right:3px"></i>Pending</div></div>
        <div class="sc i"><div class="sn" id="stProg">0</div><div class="sl"><i class="fas fa-gear" style="margin-right:3px"></i>In Progress</div></div>
        <div class="sc r"><div class="sn" id="stRes">0</div><div class="sl"><i class="fas fa-check" style="margin-right:3px"></i>Resolved</div></div>
      </div>
      <div class="cp" style="margin-bottom:16px">
        <div class="ch"><i class="fas fa-chart-bar ico"></i><h3>Complaints by Category</h3></div>
        <div class="cb" id="chartArea"></div>
      </div>
    </div>

    <!-- ALL COMPLAINTS -->
    <div id="tab-list" style="display:none">
      <div class="fbar">
        <input type="text" class="fc" id="searchIn" placeholder="🔍 Search name, ID, court..." oninput="renderList()">
        <select class="fs" id="filterSt" onchange="renderList()">
          <option value="">All Status</option>
          <option>Pending</option><option>In Progress</option><option>Resolved</option>
        </select>
        <select class="fs" id="filterCat" onchange="renderList()">
          <option value="">All Categories</option>
          <option>Electrical</option><option>Plumbing</option><option>Furniture</option><option>Civil</option><option>Others</option>
        </select>
      </div>
      <div id="complaintList"></div>
      <div id="listEmpty" style="display:none;text-align:center;padding:30px;color:var(--gray-4)"><i class="fas fa-inbox" style="font-size:1.8rem;margin-bottom:9px;display:block"></i>No complaints found</div>
    </div>

    <!-- ACCOUNTS -->
    <div id="tab-accounts" style="display:none">
      <div class="bal-grid">
        <div class="bal-card"><div class="bal-num" id="bInc" style="color:var(--ok)">Rs.0</div><div class="bal-lbl">Total Income</div></div>
        <div class="bal-card"><div class="bal-num" id="bExp" style="color:var(--err)">Rs.0</div><div class="bal-lbl">Total Expense</div></div>
        <div class="bal-card"><div class="bal-num" id="bBal">Rs.0</div><div class="bal-lbl">Balance</div></div>
      </div>
      <div class="cp" style="margin-bottom:16px">
        <div class="ch"><i class="fas fa-plus ico"></i><h3>Add Entry</h3></div>
        <div class="cb">
          <div class="acc-tabs">
            <button class="act inc on" id="accTypeInc" onclick="setAccType('income')"><i class="fas fa-arrow-down" style="margin-right:5px"></i>Income</button>
            <button class="act exp" id="accTypeExp" onclick="setAccType('expense')"><i class="fas fa-arrow-up" style="margin-right:5px"></i>Expense</button>
          </div>
          <input type="hidden" id="accType" value="income">
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:11px">
            <div class="frow" style="margin-bottom:0"><label class="fl"><span id="partyLbl">Payer — Received From</span> <span class="r">*</span></label><input type="text" id="accParty" class="fc" placeholder="Name / Department"></div>
            <div class="frow" style="margin-bottom:0"><label class="fl">Amount (Rs.) <span class="r">*</span></label><input type="number" id="accAmt" class="fc" placeholder="0" min="0"></div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:11px;margin-top:11px">
            <div class="frow" style="margin-bottom:0"><label class="fl">Date <span class="r">*</span></label><input type="date" id="accDate" class="fc"></div>
            <div class="frow" style="margin-bottom:0"><label class="fl">Note</label><input type="text" id="accNote" class="fc" placeholder="e.g. Office supplies"></div>
          </div>
          <button class="bg" onclick="addAccount()" style="margin-top:13px;width:100%"><i class="fas fa-plus" style="margin-right:5px"></i>Add Entry</button>
        </div>
      </div>
      <div class="cp">
        <div class="ch" style="justify-content:space-between">
          <div style="display:flex;align-items:center;gap:9px"><i class="fas fa-book ico"></i><h3>Ledger</h3></div>
          <div style="display:flex;gap:7px">
            <select class="fs" id="ledgerFilter" onchange="renderAccounts()" style="font-size:.76rem;padding:5px 8px"><option value="">All</option><option value="income">Income</option><option value="expense">Expense</option></select>
            <button onclick="printAccounts()" class="bog" style="font-size:.74rem;padding:4px 11px"><i class="fas fa-print" style="margin-right:3px"></i>Print</button>
          </div>
        </div>
        <div class="cb">
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:9px;margin-bottom:13px">
            <div><label class="fl" style="font-size:.76rem">Date From</label><input type="date" id="accFrom" class="fc" style="font-size:.82rem" onchange="renderAccounts()"></div>
            <div><label class="fl" style="font-size:.76rem">Date To</label><input type="date" id="accTo" class="fc" style="font-size:.82rem" onchange="renderAccounts()"></div>
          </div>
          <div id="ledgerList"></div>
          <div id="ledgerEmpty" style="display:none;text-align:center;padding:22px;color:var(--gray-4);font-size:.84rem"><i class="fas fa-coins" style="font-size:1.7rem;margin-bottom:7px;display:block"></i>No entries yet</div>
        </div>
      </div>
    </div>

    <!-- REPORTS -->
    <div id="tab-report" style="display:none">
      <div class="cp" style="margin-bottom:14px">
        <div class="ch"><i class="fas fa-file-chart-column ico"></i><h3>Generate Report</h3></div>
        <div class="cb">
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:11px;margin-bottom:13px">
            <div><label class="fl">From Date</label><input type="date" id="rFrom" class="fc"></div>
            <div><label class="fl">To Date</label><input type="date" id="rTo" class="fc"></div>
          </div>
          <div style="display:flex;gap:9px;flex-wrap:wrap">
            <button class="bg" onclick="printReport()"><i class="fas fa-print" style="margin-right:5px"></i>Print Report</button>
            <button class="bg" onclick="generateReport()" style="background:linear-gradient(135deg,#1a4580,#1a6e3f)"><i class="fas fa-file-lines" style="margin-right:5px"></i>Generate Report</button>
            <button class="bog" onclick="exportCSV()"><i class="fas fa-file-csv" style="margin-right:5px"></i>Export CSV</button>
          </div>
        </div>
      </div>
      <div class="cp"><div class="ch"><i class="fas fa-eye ico"></i><h3>Report Preview</h3></div><div class="cb" id="reportPrev" style="font-size:.82rem"></div></div>
    </div>

    <!-- COURT USERS -->
    <div id="tab-users" style="display:none">
      <div class="cp" style="margin-bottom:14px">
        <div class="ch"><i class="fas fa-users ico"></i><h3>Court User Accounts</h3></div>
        <div class="cb">
          <p style="font-size:.83rem;color:var(--gray-6);margin-bottom:14px">All court login credentials. Share with respective courts.</p>
          <div id="usersList"><p style="color:var(--gray-4)">Loading...</p></div>
        </div>
      </div>
    </div>

    <!-- SETTINGS -->
    <div id="tab-settings" style="display:none">
      <div class="cp" style="margin-bottom:14px">
        <div class="ch"><i class="fas fa-key ico"></i><h3>Change Admin Password</h3></div>
        <div class="cb">
          <div class="frow"><label class="fl">Current Password <span class="r">*</span></label><input type="password" id="pwCurr" class="fc"></div>
          <div class="frow"><label class="fl">New Password <span class="r">*</span></label><input type="password" id="pwNew" class="fc" placeholder="Minimum 6 characters"></div>
          <div class="frow"><label class="fl">Confirm Password <span class="r">*</span></label><input type="password" id="pwConf" class="fc"></div>
          <button class="bg" onclick="changePassword()"><i class="fas fa-save" style="margin-right:5px"></i>Update Password</button>
          <div id="pwMsg" style="display:none;background:var(--ok-bg);color:var(--ok);padding:8px 13px;border-radius:var(--r);font-size:.82rem;font-weight:600;margin-top:10px">✅ Password changed!</div>
        </div>
      </div>
      <div class="cp">
        <div class="ch"><i class="fas fa-database ico"></i><h3>Data Management</h3></div>
        <div class="cb">
          <button class="bn" onclick="loadAll().then(()=>{updateStats();renderList();renderAccounts();renderChart();toast('Refreshed!','s')})"><i class="fas fa-rotate" style="margin-right:5px"></i>Refresh All Data</button>
        </div>
      </div>
    </div>
  </div>
  <footer class="ft">Designed &amp; Developed by <strong>Irfan Ahmed Memon</strong> — Jr. Clerk &nbsp;|&nbsp; 0335-2345183 &nbsp;|&nbsp; District &amp; Sessions Court Karachi South</footer>
</div>

<!-- MODALS -->
<div class="mo" id="mView"><div class="mb">
  <div class="mh"><h4><i class="fas fa-file-lines" style="margin-right:7px;color:var(--gold)"></i>Complaint Details</h4><button class="mx" onclick="closeModal('mView')">✕</button></div>
  <div class="mbody" id="mViewBody"></div>
  <div class="mf"><button class="bog" onclick="closeModal('mView')">Close</button></div>
</div></div>

<div class="mo" id="mEdit"><div class="mb">
  <div class="mh"><h4><i class="fas fa-pen-to-square" style="margin-right:7px;color:var(--gold)"></i>Edit Complaint</h4><button class="mx" onclick="closeModal('mEdit')">✕</button></div>
  <div class="mbody">
    <input type="hidden" id="editId">
    <div class="frow"><label class="fl">Name <span class="r">*</span></label><input type="text" id="editName" class="fc"></div>
    <div class="frow"><label class="fl">Designation</label><input type="text" id="editDesig" class="fc"></div>
    <div class="frow"><label class="fl">Status</label><select id="editStatus" class="fs"><option>Pending</option><option>In Progress</option><option>Resolved</option></select></div>
    <div class="frow"><label class="fl">Technician</label><input type="text" id="editTech" class="fc" placeholder="Assigned technician"></div>
    <div class="frow"><label class="fl">Remarks</label><textarea id="editRemarks" class="fc" rows="3"></textarea></div>
  </div>
  <div class="mf"><button class="bog" onclick="closeModal('mEdit')">Cancel</button><button class="bg" onclick="saveEdit()"><i class="fas fa-save" style="margin-right:5px"></i>Save</button></div>
</div></div>

<div class="mo" id="mAcc"><div class="mb">
  <div class="mh"><h4><i class="fas fa-edit" style="margin-right:7px;color:var(--gold)"></i>Edit Account Entry</h4><button class="mx" onclick="closeModal('mAcc')">✕</button></div>
  <div class="mbody">
    <input type="hidden" id="eaId">
    <div class="frow"><label class="fl">Party <span class="r">*</span></label><input type="text" id="eaParty" class="fc"></div>
    <div class="frow"><label class="fl">Amount (Rs.) <span class="r">*</span></label><input type="number" id="eaAmt" class="fc" min="0"></div>
    <div class="frow"><label class="fl">Note</label><input type="text" id="eaNote" class="fc"></div>
    <div class="frow"><label class="fl">Date</label><input type="date" id="eaDate" class="fc"></div>
  </div>
  <div class="mf"><button class="bog" onclick="closeModal('mAcc')">Cancel</button><button class="bg" onclick="saveAccEdit()"><i class="fas fa-save" style="margin-right:5px"></i>Save</button></div>
</div></div>

<div class="mo" id="mImg" onclick="closeModal('mImg')">
  <img id="mImgSrc" src="" style="max-width:94vw;max-height:90vh;border-radius:var(--r);box-shadow:var(--sm)" alt="">
</div>

<script>
// ═══════════════════════════════════════════════════
//  SUPABASE
// ═══════════════════════════════════════════════════
const SB_URL = 'https://vwlnfmvdhwyfeigkluxf.supabase.co';
const SB_KEY = 'sb_publishable_Tys3RGm29ZwkfsuTFHl19w_xIjoWAxx';

async function sb(method, table, body, params='') {
  const headers = {'apikey':SB_KEY,'Authorization':'Bearer '+SB_KEY,'Content-Type':'application/json'};
  if (method==='POST'||method==='PATCH') headers['Prefer']='return=representation';
  const res = await fetch(`${SB_URL}/rest/v1/${table}${params}`,{method,headers,body:body?JSON.stringify(body):undefined});
  if (!res.ok) { const e=await res.text(); throw new Error(`${res.status}: ${e}`); }
  const t=await res.text(); return t?JSON.parse(t):null;
}

// ═══════════════════════════════════════════════════
//  STATE
// ═══════════════════════════════════════════════════
let complaints=[], accounts=[], courtUsers=[];
let isAdmin=false, currentUser='', currentCourt='', loginTabMode='court';
let navStack=['login'], lastTrack='', selCatBase='';
let notifications=JSON.parse(localStorage.getItem('dsc_notifs')||'[]');
let adminPasswords={'dsc.admin':localStorage.getItem('pw_dsc.admin')||'DSC@2025!','irfan':localStorage.getItem('pw_irfan')||'Irfan@123'};
let loginAttempts=0, lockUntil=0;
let imgData='';

// ═══════════════════════════════════════════════════
//  INIT
// ═══════════════════════════════════════════════════
window.addEventListener('load', async () => {
  setInterval(updateClock,1000); updateClock();
  document.getElementById('accDate').valueAsDate=new Date();
  await loadAll();
  document.getElementById('loader').classList.add('hide');
  showView('vLogin');
});

async function loadAll() {
  setSyncStatus('⏳ Loading...');
  try {
    const [cmps,accs,users] = await Promise.all([
      sb('GET','complaints','','?order=date.desc'),
      sb('GET','accounts','','?order=created_at.desc'),
      sb('GET','court_users','','?order=court_name.asc')
    ]);
    complaints=cmps||[]; accounts=accs||[]; courtUsers=users||[];
    setSyncStatus('✅ Live');
    setTimeout(()=>setSyncStatus(''),3000);
  } catch(e) {
    setSyncStatus('⚠️ Offline');
    toast('Database connection failed.','e');
  }
}

// ═══════════════════════════════════════════════════
//  GENERATE ID
// ═══════════════════════════════════════════════════
async function generateId() {
  try {
    const latest=await sb('GET','complaints','','?id=like.DSC-______&order=id.desc&limit=1');
    if (latest&&latest.length) { const n=parseInt(latest[0].id.replace('DSC-',''))||0; return 'DSC-'+String(n+1).padStart(6,'0'); }
    const existing=complaints.filter(c=>c.id&&/^DSC-\d{6}$/.test(c.id)).map(c=>parseInt(c.id.replace('DSC-',''))||0);
    const max=existing.length?Math.max(...existing):0;
    return 'DSC-'+String(max+1).padStart(6,'0');
  } catch(e) {
    const existing=complaints.filter(c=>c.id&&/^DSC-\d{6}$/.test(c.id)).map(c=>parseInt(c.id.replace('DSC-',''))||0);
    const max=existing.length?Math.max(...existing):0;
    return 'DSC-'+String(max+1).padStart(6,'0');
  }
}

// ═══════════════════════════════════════════════════
//  NAVIGATION
// ═══════════════════════════════════════════════════
function showView(id) {
  document.querySelectorAll('.view').forEach(v=>v.classList.remove('on'));
  const el=document.getElementById(id); if(el){el.classList.add('on');window.scrollTo(0,0);}
}

function switchTo(tab,isBack) {
  ['nCourt','nTrack','nAdmin'].forEach(id=>{const el=document.getElementById(id);if(el)el.classList.remove('on');});
  const bk=document.getElementById('backBtn');
  if (tab==='court'&&currentCourt) {
    document.getElementById('nCourt').classList.add('on');
    showView('vCourt'); if(!isBack)navStack.push('court'); bk.classList.add('vis');
    loadCourtComplaints();
  } else if (tab==='track') {
    document.getElementById('nTrack').classList.add('on');
    showView('vTrack'); if(!isBack)navStack.push('track'); bk.classList.add('vis');
    const ti=document.getElementById('trackIn');
    if(lastTrack&&ti&&!ti.value){ti.value=lastTrack;trackComplaint();}
  } else if (tab==='admin'&&isAdmin) {
    document.getElementById('nAdmin').classList.add('on');
    showView('vAdmin'); if(!isBack)navStack.push('admin'); bk.classList.add('vis');
    loadAll().then(()=>{updateStats();renderList();renderAccounts();renderChart();renderReport();renderUsers();});
  }
}

function goBack() {
  if(navStack.length>1){navStack.pop();switchTo(navStack[navStack.length-1],true);}
}

// ═══════════════════════════════════════════════════
//  LOGIN
// ═══════════════════════════════════════════════════
function setLoginTab(mode) {
  loginTabMode=mode;
  document.getElementById('ltab-court').classList.toggle('on',mode==='court');
  document.getElementById('ltab-admin').classList.toggle('on',mode==='admin');
  document.getElementById('loginUserLbl').textContent=mode==='court'?'Court Username *':'Admin Username *';
  document.getElementById('lUser').placeholder=mode==='court'?'e.g. civil1, nazarat':'Admin username';
}

async function doLogin() {
  if(Date.now()<lockUntil){const s=Math.ceil((lockUntil-Date.now())/1000);showLockMsg('Locked. Try in '+s+'s');return;}
  const u=document.getElementById('lUser').value.trim();
  const p=document.getElementById('lPass').value;
  if(!u||!p){toast('Enter username and password.','e');return;}

  if (loginTabMode==='admin') {
    // Admin login
    if(adminPasswords[u]&&adminPasswords[u]===p) {
      isAdmin=true; currentUser=u; currentCourt=''; loginAttempts=0;
      setupAfterLogin(false);
      toast('Welcome, Admin!','s');
    } else {
      handleFailedLogin();
    }
  } else {
    // Court login — check DB
    try {
      const res=await sb('GET','court_users','',`?username=eq.${encodeURIComponent(u)}&password=eq.${encodeURIComponent(p)}`);
      if(res&&res.length) {
        isAdmin=false; currentUser=u; currentCourt=res[0].court_name; loginAttempts=0;
        setupAfterLogin(true);
        toast('Welcome! '+currentCourt,'s');
      } else {
        handleFailedLogin();
      }
    } catch(e) { toast('Login failed. Check connection.','e'); }
  }
  document.getElementById('lPass').value='';
}

function setupAfterLogin(isCourt) {
  document.getElementById('lockMsg').style.display='none';
  document.getElementById('lUser').value='';
  document.getElementById('mainHdr').style.display='block';
  document.getElementById('nLogout').style.display='inline-flex';

  if(isCourt) {
    document.getElementById('nCourt').style.display='inline-flex';
    document.getElementById('nCourt').classList.add('on');
    document.getElementById('courtBadge').textContent=currentCourt;
    document.getElementById('loggedInAs').textContent='🏛️ '+currentCourt;
    showView('vCourt');
    navStack=['court'];
    loadCourtComplaints();
  } else {
    document.getElementById('nAdmin').style.display='inline-flex';
    document.getElementById('nBell').style.display='inline-flex';
    document.getElementById('loggedInAs').textContent='🔐 Admin: '+currentUser;
    showView('vAdmin');
    navStack=['admin'];
    loadAll().then(()=>{updateStats();renderList();renderAccounts();renderChart();renderReport();renderUsers();});
  }
}

function handleFailedLogin() {
  loginAttempts++;
  if(loginAttempts>=5){lockUntil=Date.now()+600000;loginAttempts=0;showLockMsg('Too many attempts. Locked 10 mins.');}
  else{toast('Invalid credentials. '+(5-loginAttempts)+' attempt(s) left.','e');}
}

function showLockMsg(msg){const el=document.getElementById('lockMsg');el.style.display='block';el.textContent=msg;}

function doLogout() {
  isAdmin=false; currentUser=''; currentCourt='';
  document.getElementById('nAdmin').style.display='none';
  document.getElementById('nCourt').style.display='none';
  document.getElementById('nLogout').style.display='none';
  document.getElementById('nBell').style.display='none';
  document.getElementById('mainHdr').style.display='none';
  document.getElementById('loggedInAs').textContent='';
  navStack=['login'];
  showView('vLogin');
  toast('Logged out.','i');
}

function togglePass(){const i=document.getElementById('lPass');const ico=document.getElementById('eyeIco');i.type=i.type==='password'?'text':'password';ico.className=i.type==='password'?'fas fa-eye':'fas fa-eye-slash';}

// ═══════════════════════════════════════════════════
//  COURT COMPLAINTS
// ═══════════════════════════════════════════════════
async function loadCourtComplaints() {
  const el=document.getElementById('courtList');
  el.innerHTML='<p style="font-size:.84rem;color:var(--gray-4)"><i class="fas fa-spinner fa-spin" style="margin-right:5px"></i>Loading...</p>';
  try {
    const list=await sb('GET','complaints','',`?department=eq.${encodeURIComponent(currentCourt)}&order=date.desc`);
    const myComplaints=list||[];
    // Update local cache
    myComplaints.forEach(c=>{const idx=complaints.findIndex(x=>x.id===c.id);if(idx>=0)complaints[idx]=c;else complaints.push(c);});
    document.getElementById('courtComplaintCount').textContent=myComplaints.length+' complaint(s) filed';
    if(!myComplaints.length){el.innerHTML='<p style="font-size:.84rem;color:var(--gray-4);text-align:center;padding:16px"><i class="fas fa-inbox" style="display:block;font-size:1.8rem;margin-bottom:8px"></i>No complaints filed yet</p>';return;}
    el.innerHTML=myComplaints.map(c=>{
      const sc=c.status==='Pending'?'bs bp':c.status==='In Progress'?'bs bi':'bs br';
      const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'});
      return `<div class="ci-card">
        <div class="ci-top">
          <div><div class="ci-id">${esc(c.id)}</div><div class="ci-meta">${esc(c.name)} · ${dt}</div></div>
          <span class="${sc}">${esc(c.status)}</span>
        </div>
        <div style="font-size:.81rem;color:var(--gray-6)">${esc(c.category||'—')}</div>
        <div style="font-size:.82rem;color:var(--navy);margin-top:3px">${esc((c.description||'').substring(0,80))}${(c.description||'').length>80?'...':''}</div>
        <div class="ci-actions">
          <button class="ab ab-view" onclick="viewComplaintCourt('${esc(c.id)}')"><i class="fas fa-eye" style="margin-right:3px"></i>View</button>
          <button onclick="copyId('${esc(c.id)}')" class="ab" style="background:var(--gold-pale);color:var(--warn)"><i class="fas fa-copy" style="margin-right:3px"></i>Copy No.</button>
          ${c.status!=='Resolved'?`<button class="ab ab-del" onclick="courtResolve('${esc(c.id)}')"><i class="fas fa-check" style="margin-right:3px"></i>Mark Resolved</button>`:''}
        </div>
      </div>`;
    }).join('');
  } catch(e) { el.innerHTML='<p style="color:var(--err)">Failed to load complaints.</p>'; }
}

function viewComplaintCourt(id) {
  const c=complaints.find(x=>x.id===id); if(!c) return;
  const sc=c.status==='Pending'?'bs bp':c.status==='In Progress'?'bs bi':'bs br';
  const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'long',year:'numeric'});
  document.getElementById('mViewBody').innerHTML=`
    <div style="padding:12px 16px;background:var(--navy);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:7px">
      <span style="font-family:var(--fd);color:var(--gold-lt);font-size:.98rem;font-weight:700">${esc(c.id)}</span>
      <span class="${sc}">${esc(c.status)}</span>
    </div>
    <div class="mbody">
      ${infoRow('Name',c.name)}${infoRow('Designation',c.designation||'—')}
      ${infoRow('Department',c.department||'—')}${infoRow('Category',c.category||'—')}
      ${infoRow('Filed On',dt)}${infoRow('Description',c.description||'—')}
      ${c.tech?infoRow('Technician',c.tech):''}${c.remarks?infoRow('Remarks',c.remarks):''}
      ${c.resolved_date?infoRow('Resolved On',c.resolved_date):''}
      ${c.approved_by?infoRow('Approved By',c.approved_by):''}
      ${c.image_url?`<div class="info-row"><div class="ik">Photo</div><div class="iv"><img src="${c.image_url}" style="max-width:100%;max-height:200px;border-radius:var(--r);cursor:pointer" onclick="openImgModal('${c.image_url}')"></div></div>`:''}
    </div>`;
  openModal('mView');
}

async function courtResolve(id) {
  if(!confirm('Mark this complaint as resolved?')) return;
  const today=new Date().toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'});
  try {
    await sb('PATCH','complaints',{status:'Resolved',resolved_by:'Complainant - '+currentCourt,resolved_date:today},`?id=eq.${encodeURIComponent(id)}`);
    const c=complaints.find(x=>x.id===id);
    if(c){c.status='Resolved';c.resolved_by='Complainant - '+currentCourt;c.resolved_date=today;}
    toast('Marked as resolved!','s');
    loadCourtComplaints();
  } catch(e){toast('Update failed.','e');}
}

// ═══════════════════════════════════════════════════
//  VALIDATION
// ═══════════════════════════════════════════════════
function validateName(el){const err=document.getElementById('cNameErr');if(/\d/.test(el.value)){el.classList.add('err');err.textContent='Name cannot contain numbers';err.style.display='block';}else{el.classList.remove('err');err.style.display='none';}}
function validatePhone(el){const err=document.getElementById('cPhoneErr');if(el.value&&!/^[0-9\-\+\s]{7,15}$/.test(el.value)){el.classList.add('err');err.textContent='Invalid phone number';err.style.display='block';}else{el.classList.remove('err');err.style.display='none';}}

// ═══════════════════════════════════════════════════
//  CATEGORY
// ═══════════════════════════════════════════════════
function selectCat(btn,mode){
  document.querySelectorAll('.cat-btn').forEach(b=>b.classList.remove('sel'));
  btn.classList.add('sel');selCatBase=btn.dataset.cat;
  document.getElementById('cCategory').value='';document.getElementById('catSelBox').classList.remove('vis');
  document.getElementById('subR1').classList.remove('chk');document.getElementById('subR2').classList.remove('chk');
  if(mode==='noSub'){document.getElementById('subRow').classList.remove('vis');document.getElementById('cCategory').value=selCatBase;document.getElementById('catSelTxt').textContent=selCatBase;document.getElementById('catSelBox').classList.add('vis');document.querySelectorAll('.cat-btn').forEach(b=>b.style.display='none');}
  else{document.getElementById('subRow').classList.add('vis');}
}
function selectSub(val,btn){
  document.getElementById('subR1').classList.remove('chk');document.getElementById('subR2').classList.remove('chk');btn.classList.add('chk');
  const full=`${selCatBase} — ${val}`;document.getElementById('cCategory').value=full;document.getElementById('catSelTxt').textContent=full;
  document.getElementById('catSelBox').classList.add('vis');document.querySelectorAll('.cat-btn').forEach(b=>b.style.display='none');document.getElementById('subRow').classList.remove('vis');
}
function clearCat(){document.querySelectorAll('.cat-btn').forEach(b=>{b.classList.remove('sel');b.style.display='';});document.getElementById('subRow').classList.remove('vis');document.getElementById('catSelBox').classList.remove('vis');document.getElementById('cCategory').value='';document.getElementById('subR1').classList.remove('chk');document.getElementById('subR2').classList.remove('chk');selCatBase='';}

// ═══════════════════════════════════════════════════
//  IMAGE
// ═══════════════════════════════════════════════════
function handleImg(el){const f=el.files[0];if(!f)return;if(f.size>2*1024*1024){toast('Max 2MB.','e');el.value='';return;}const r=new FileReader();r.onload=e=>{imgData=e.target.result;document.getElementById('imgSrc').src=imgData;document.getElementById('imgPrev').style.display='block';};r.readAsDataURL(f);}
function clearImg(){imgData='';document.getElementById('cImg').value='';document.getElementById('imgPrev').style.display='none';}

// ═══════════════════════════════════════════════════
//  SUBMIT COMPLAINT
// ═══════════════════════════════════════════════════
async function submitComplaint() {
  const name=document.getElementById('cName').value.trim();
  const desig=document.getElementById('cDesig').value.trim();
  const dept=currentCourt; // Always from court login
  const cat=document.getElementById('cCategory').value;
  const desc=document.getElementById('cDesc').value.trim();
  const phone=document.getElementById('cPhone').value.trim();
  const block=document.getElementById('cBlock').value.trim();

  if(!name){toast('Full name is required.','e');return;}
  if(/\d/.test(name)){toast('Name cannot contain numbers.','e');return;}
  if(!desig){toast('Designation is required.','e');return;}
  if(!cat){toast('Please select a category.','e');return;}
  if(!desc){toast('Please describe the issue.','e');return;}
  if(phone&&!/^[0-9\-\+\s]{7,15}$/.test(phone)){toast('Invalid phone number.','e');return;}

  const btn=document.getElementById('submitBtn');
  btn.disabled=true;btn.innerHTML='<i class="fas fa-spinner fa-spin" style="margin-right:7px"></i>Submitting...';

  try {
    const id=await generateId();
    const complaint={id,name,designation:desig,department:dept,block:block||'',category:cat,description:desc,phone:phone||'',status:'Pending',tech:'',remarks:'',resolved_by:'',resolved_date:'',image_url:imgData||'',approved_by:''};
    await sb('POST','complaints',complaint);
    complaints.unshift(complaint);
    addNotif({id,name,department:dept,category:cat});
    document.getElementById('genTicket').textContent=id;
    document.getElementById('courtForm').style.display='none';
    document.getElementById('courtSuccess').style.display='block';
    setSyncStatus('✅ Saved');
  } catch(e){toast('Submit failed: '+e.message,'e');console.error(e);}
  btn.disabled=false;
  btn.innerHTML='<i class="fas fa-paper-plane" style="margin-right:7px"></i>Submit Complaint';
}

function newComplaint(){
  document.getElementById('courtForm').style.display='block';
  document.getElementById('courtSuccess').style.display='none';
  ['cName','cDesig','cBlock','cDesc','cPhone'].forEach(id=>{const el=document.getElementById(id);if(el)el.value='';});
  document.getElementById('charCnt').textContent='0 chars';
  clearCat();clearImg();window.scrollTo(0,0);
  loadCourtComplaints();
}

// ═══════════════════════════════════════════════════
//  TRACK
// ═══════════════════════════════════════════════════
async function trackComplaint() {
  const raw=document.getElementById('trackIn').value.trim().toUpperCase();
  if(!raw){toast('Enter a complaint number.','e');return;}
  lastTrack=raw;
  document.getElementById('trackResult').innerHTML='<div style="text-align:center;padding:20px;color:var(--gray-4)"><i class="fas fa-spinner fa-spin" style="font-size:1.4rem"></i><br><br>Searching...</div>';
  let c=complaints.find(x=>x.id===raw);
  if(!c){try{const res=await sb('GET','complaints','',`?id=eq.${encodeURIComponent(raw)}`);c=res&&res[0];}catch(e){}}
  const el=document.getElementById('trackResult');
  if(!c){el.innerHTML=`<div style="text-align:center;padding:22px;color:var(--err);background:var(--err-bg);border-radius:var(--r)"><i class="fas fa-circle-xmark" style="font-size:1.8rem;margin-bottom:9px;display:block"></i><strong>Complaint not found</strong></div>`;return;}
  const sc=c.status==='Pending'?'bs bp':c.status==='In Progress'?'bs bi':'bs br';
  const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'long',year:'numeric'});
  el.innerHTML=`<div class="cp">
    <div style="padding:14px 16px;border-bottom:1px solid var(--gray-1);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:8px">
      <div style="display:flex;align-items:center;gap:9px;flex-wrap:wrap">
        <span style="font-family:var(--fd);font-size:1rem;font-weight:700;color:var(--navy)">${esc(c.id)}</span>
        <button onclick="copyId('${esc(c.id)}')" style="background:var(--gold-pale);border:1.5px solid var(--gold);color:var(--navy);border-radius:6px;padding:3px 9px;font-size:.73rem;cursor:pointer;font-weight:600"><i class="fas fa-copy" style="margin-right:3px"></i>Copy</button>
      </div>
      <span class="${sc}">${esc(c.status)}</span>
    </div>
    <div class="mbody">
      ${infoRow('Name',c.name)}${infoRow('Designation',c.designation||'—')}
      ${infoRow('Department',c.department||'—')}${infoRow('Category',c.category||'—')}
      ${infoRow('Filed On',dt)}${infoRow('Description',c.description||'—')}
      ${c.tech?infoRow('Technician',c.tech):''}${c.remarks?infoRow('Remarks',c.remarks):''}
      ${c.resolved_date?infoRow('Resolved On',c.resolved_date):''}${c.approved_by?infoRow('Approved By',c.approved_by):''}
      ${c.image_url?`<div class="info-row"><div class="ik">Photo</div><div class="iv"><img src="${c.image_url}" style="max-width:100%;max-height:170px;border-radius:var(--r);border:1px solid var(--gray-2)"></div></div>`:''}
    </div>
    ${c.status!=='Resolved'?`<div style="padding:0 16px 16px"><button class="bg" onclick="complainantResolve('${esc(c.id)}')" style="width:100%;padding:10px"><i class="fas fa-check-circle" style="margin-right:5px"></i>Mark as Resolved</button></div>`:`<div style="padding:0 16px 16px;text-align:center;color:var(--ok);font-weight:600;font-size:.86rem"><i class="fas fa-check-double" style="margin-right:5px"></i>Complaint resolved</div>`}
  </div>`;
}

async function complainantResolve(id) {
  if(!confirm('Mark as resolved?')) return;
  const today=new Date().toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'});
  try {
    await sb('PATCH','complaints',{status:'Resolved',resolved_by:'Complainant',resolved_date:today},`?id=eq.${encodeURIComponent(id)}`);
    const c=complaints.find(x=>x.id===id);
    if(c){c.status='Resolved';c.resolved_by='Complainant';c.resolved_date=today;}
    addNotif({id,name:c?.name||'',department:c?.department||'',category:c?.category||'',type:'resolved'});
    toast('Marked as resolved!','s');
    trackComplaint();
  } catch(e){toast('Update failed.','e');}
}

// ═══════════════════════════════════════════════════
//  ADMIN STATS + CHART
// ═══════════════════════════════════════════════════
function updateStats(){
  document.getElementById('stTotal').textContent=complaints.length;
  document.getElementById('stPend').textContent=complaints.filter(c=>c.status==='Pending').length;
  document.getElementById('stProg').textContent=complaints.filter(c=>c.status==='In Progress').length;
  document.getElementById('stRes').textContent=complaints.filter(c=>c.status==='Resolved').length;
}
function renderChart(){
  const cats=['Electrical','Plumbing','Furniture','Civil','Others'];
  const colors=['#c9a44a','#1a4580','#1a6e3f','#8a1c1c','#5c5852'];
  const counts=cats.map(c=>complaints.filter(x=>(x.category||'').includes(c)).length);
  const max=Math.max(...counts,1);
  document.getElementById('chartArea').innerHTML=cats.map((c,i)=>`
    <div class="chart-row">
      <div class="chart-lbl">${c}</div>
      <div style="flex:1;background:var(--gray-1);border-radius:4px;overflow:hidden">
        <div class="chart-bar" style="background:${colors[i]};width:${Math.round(counts[i]/max*100)}%">${counts[i]||''}</div>
      </div>
      <div style="min-width:26px;text-align:right;font-size:.76rem;font-weight:600;color:var(--gray-6)">${counts[i]}</div>
    </div>`).join('');
}

// ═══════════════════════════════════════════════════
//  ADMIN COMPLAINT LIST
// ═══════════════════════════════════════════════════
function renderList(){
  const q=(document.getElementById('searchIn')?.value||'').toLowerCase();
  const st=document.getElementById('filterSt')?.value||'';
  const cat=document.getElementById('filterCat')?.value||'';
  let list=complaints.filter(c=>{
    const mQ=!q||c.id?.toLowerCase().includes(q)||c.name?.toLowerCase().includes(q)||c.department?.toLowerCase().includes(q)||c.category?.toLowerCase().includes(q);
    return mQ&&(!st||c.status===st)&&(!cat||(c.category||'').includes(cat));
  });
  const el=document.getElementById('complaintList');
  const em=document.getElementById('listEmpty');
  if(!list.length){el.innerHTML='';em.style.display='block';return;}
  em.style.display='none';
  el.innerHTML=list.map(c=>{
    const sc=c.status==='Pending'?'bs bp':c.status==='In Progress'?'bs bi':'bs br';
    const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'});
    const djBtn=c.approved_by?`<button class="ab ab-dj-done" disabled><i class="fas fa-stamp" style="margin-right:3px"></i>DJ Approved</button>`:`<button class="ab ab-dj" onclick="approveDJ('${esc(c.id)}')"><i class="fas fa-stamp" style="margin-right:3px"></i>Approve DJ</button>`;
    const certBtn=c.status==='Resolved'?`<button class="ab ab-cert" onclick="printCertificate('${esc(c.id)}')"><i class="fas fa-certificate" style="margin-right:3px"></i>Certificate</button>`:'';
    return `<div class="ci-card">
      <div class="ci-top">
        <div><div class="ci-id">${esc(c.id)}</div><div class="ci-meta">${esc(c.name)} &nbsp;·&nbsp; ${esc(c.department||'—')} &nbsp;·&nbsp; ${dt}</div></div>
        <span class="${sc}">${esc(c.status)}</span>
      </div>
      <div style="font-size:.81rem;color:var(--gray-6);margin-bottom:3px"><i class="fas fa-tag" style="margin-right:4px;color:var(--gold)"></i>${esc(c.category||'—')}</div>
      <div style="font-size:.82rem;color:var(--navy)">${esc((c.description||'').substring(0,90))}${(c.description||'').length>90?'...':''}</div>
      <div class="ci-actions">
        <button class="ab ab-view" onclick="viewComplaint('${esc(c.id)}')"><i class="fas fa-eye" style="margin-right:3px"></i>View</button>
        <button class="ab ab-prog" onclick="openEdit('${esc(c.id)}')"><i class="fas fa-pen" style="margin-right:3px"></i>Edit</button>
        <button class="ab ab-print" onclick="printComplaint('${esc(c.id)}')"><i class="fas fa-print" style="margin-right:3px"></i>Print</button>
        ${djBtn}${certBtn}
        <button class="ab ab-del" onclick="delComplaint('${esc(c.id)}')"><i class="fas fa-trash" style="margin-right:3px"></i>Delete</button>
      </div>
    </div>`;
  }).join('');
}

function viewComplaint(id){
  const c=complaints.find(x=>x.id===id);if(!c)return;
  const sc=c.status==='Pending'?'bs bp':c.status==='In Progress'?'bs bi':'bs br';
  const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'long',year:'numeric'});
  document.getElementById('mViewBody').innerHTML=`
    <div style="padding:12px 16px;background:var(--navy);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:7px">
      <span style="font-family:var(--fd);color:var(--gold-lt);font-size:.98rem;font-weight:700">${esc(c.id)}</span>
      <span class="${sc}">${esc(c.status)}</span>
    </div>
    <div class="mbody">
      ${infoRow('Name',c.name)}${infoRow('Designation',c.designation||'—')}${infoRow('Department',c.department||'—')}
      ${infoRow('Block',c.block||'—')}${infoRow('Category',c.category||'—')}${infoRow('Phone',c.phone||'—')}
      ${infoRow('Filed On',dt)}${infoRow('Description',c.description||'—')}
      ${c.tech?infoRow('Technician',c.tech):''}${c.remarks?infoRow('Remarks',c.remarks):''}
      ${c.resolved_date?infoRow('Resolved On',c.resolved_date):''}${c.approved_by?infoRow('Approved By',c.approved_by):''}
      ${c.image_url?`<div class="info-row"><div class="ik">Photo</div><div class="iv"><img src="${c.image_url}" style="max-width:100%;max-height:200px;border-radius:var(--r);cursor:pointer;border:1px solid var(--gray-2)" onclick="openImgModal('${c.image_url}')"></div></div>`:''}
    </div>`;
  openModal('mView');
}

function openEdit(id){
  const c=complaints.find(x=>x.id===id);if(!c)return;
  document.getElementById('editId').value=id;
  document.getElementById('editName').value=c.name||'';
  document.getElementById('editDesig').value=c.designation||'';
  document.getElementById('editStatus').value=c.status||'Pending';
  document.getElementById('editTech').value=c.tech||'';
  document.getElementById('editRemarks').value=c.remarks||'';
  openModal('mEdit');
}

async function saveEdit(){
  const id=document.getElementById('editId').value;
  const name=document.getElementById('editName').value.trim();
  const status=document.getElementById('editStatus').value;
  if(!name){toast('Name required.','e');return;}
  const today=new Date().toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'});
  const patch={name,designation:document.getElementById('editDesig').value.trim(),status,tech:document.getElementById('editTech').value.trim(),remarks:document.getElementById('editRemarks').value.trim()};
  if(status==='Resolved'){patch.resolved_date=patch.resolved_date||today;patch.resolved_by=patch.resolved_by||'Admin';}
  try{await sb('PATCH','complaints',patch,`?id=eq.${encodeURIComponent(id)}`);const c=complaints.find(x=>x.id===id);if(c)Object.assign(c,patch);closeModal('mEdit');renderList();updateStats();toast('Updated.','s');}
  catch(e){toast('Update failed: '+e.message,'e');}
}

async function delComplaint(id){
  if(!confirm(`Delete ${id}?`))return;
  try{await sb('DELETE','complaints','',`?id=eq.${encodeURIComponent(id)}`);complaints=complaints.filter(x=>x.id!==id);renderList();updateStats();renderChart();toast('Deleted.','s');}
  catch(e){toast('Delete failed.','e');}
}

async function approveDJ(id){
  if(!confirm('Approve by DJ South?'))return;
  const approver='District & Sessions Judge Karachi South';
  try{await sb('PATCH','complaints',{approved_by:approver},`?id=eq.${encodeURIComponent(id)}`);const c=complaints.find(x=>x.id===id);if(c)c.approved_by=approver;renderList();toast('Approved by DJ South!','s');}
  catch(e){toast('Failed: '+e.message,'e');}
}

// ═══════════════════════════════════════════════════
//  PRINT COMPLAINT
// ═══════════════════════════════════════════════════
function printComplaint(id){
  const c=complaints.find(x=>x.id===id);if(!c)return;
  const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'long',year:'numeric'});
  const w=window.open('','_blank');
  w.document.write(`<!DOCTYPE html><html><head><title>Complaint ${c.id}</title><style>body{font-family:'Times New Roman',serif;padding:30px;max-width:700px;margin:0 auto;color:#0b1f3a}h1{font-size:1.1rem;font-weight:bold;text-align:center;border-bottom:2px solid #c9a44a;padding-bottom:8px;margin-bottom:16px}table{width:100%;border-collapse:collapse;font-size:.9rem}td{padding:7px 10px;border:1px solid #d5d0c6}td:first-child{font-weight:bold;background:#faf3dc;width:32%}.footer{text-align:center;font-size:.75rem;color:#9a9590;margin-top:20px;border-top:1px solid #d5d0c6;padding-top:10px}@media print{.nb{display:none}}</style></head><body>
  <div style="text-align:center;margin-bottom:10px;font-size:.85rem;color:#5c5852">District & Sessions Court Karachi South</div>
  <h1>COMPLAINT — ${c.id}</h1>
  <table><tr><td>Complaint No.</td><td>${c.id}</td></tr><tr><td>Name</td><td>${c.name}</td></tr><tr><td>Designation</td><td>${c.designation||'—'}</td></tr><tr><td>Department</td><td>${c.department||'—'}</td></tr><tr><td>Category</td><td>${c.category||'—'}</td></tr><tr><td>Status</td><td>${c.status}</td></tr><tr><td>Date Filed</td><td>${dt}</td></tr><tr><td>Description</td><td>${c.description||'—'}</td></tr>${c.tech?`<tr><td>Technician</td><td>${c.tech}</td></tr>`:''}${c.remarks?`<tr><td>Remarks</td><td>${c.remarks}</td></tr>`:''}${c.resolved_date?`<tr><td>Resolved On</td><td>${c.resolved_date}</td></tr>`:''}${c.approved_by?`<tr><td>Approved By</td><td>${c.approved_by}</td></tr>`:''}</table>
  <div style="display:flex;justify-content:space-between;margin-top:40px"><div style="text-align:center;width:45%"><div style="border-top:1px solid #0b1f3a;padding-top:5px;font-size:.8rem">${c.name}</div></div><div style="text-align:center;width:45%"><div style="border-top:1px solid #0b1f3a;padding-top:5px;font-size:.8rem">Irfan Ahmed Memon — Jr. Clerk</div></div></div>
  <div class="footer">Generated: ${new Date().toLocaleString()} | DSC Karachi South</div>
  <div style="text-align:center;margin-top:14px"><button onclick="window.print()" style="background:#c9a44a;color:#0b1f3a;border:none;padding:8px 24px;border-radius:6px;cursor:pointer">🖨️ Print</button></div>
  </body></html>`);
  w.document.close();
}

// ═══════════════════════════════════════════════════
//  CERTIFICATE
// ═══════════════════════════════════════════════════
function printCertificate(id){
  const c=complaints.find(x=>x.id===id);if(!c)return;
  const dt=new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'long',year:'numeric'});
  const w=window.open('','_blank');
  w.document.write(`<!DOCTYPE html><html><head><title>Certificate — ${c.id}</title>
  <style>body{font-family:'Times New Roman',serif;padding:36px;max-width:740px;margin:0 auto;color:#0b1f3a}.outer{border:7px double #c9a44a;padding:28px}.inner{border:2px solid #c9a44a;padding:22px}.hdr{text-align:center;margin-bottom:20px}.hdr h1{font-size:1.15rem;font-weight:bold;letter-spacing:1px;margin:4px 0}.hdr h2{font-size:.9rem;color:#5c5852;margin:2px 0;font-weight:normal}hr{border:none;border-top:2px solid #c9a44a;margin:14px 0}.title{text-align:center;font-size:1.3rem;font-weight:bold;letter-spacing:2px;margin:14px 0;text-decoration:underline}.body{font-size:.95rem;line-height:1.9;text-align:justify;margin-bottom:14px}table{width:100%;border-collapse:collapse;font-size:.88rem;margin:14px 0}td{padding:6px 10px;border:1px solid #d5d0c6}td:first-child{font-weight:bold;background:#faf3dc;width:32%}.sigs{display:flex;justify-content:space-between;margin-top:44px}.sig{text-align:center;width:44%}.sigline{border-top:2px solid #0b1f3a;margin-bottom:6px}.stamp{width:72px;height:72px;border:2px dashed #c9a44a;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:.58rem;font-weight:bold;color:#c9a44a;margin:7px auto;text-align:center;line-height:1.3}.footer{text-align:center;margin-top:20px;font-size:.72rem;color:#9a9590}@media print{.noprint{display:none}}</style></head><body>
  <div class="outer"><div class="inner">
    <div class="hdr"><div style="font-size:46px;margin-bottom:7px">⚖️</div><h1>DISTRICT & SESSIONS COURT KARACHI SOUTH</h1><h2>Repair & Maintenance Complaint Management System</h2><hr></div>
    <div class="title">RESOLUTION CERTIFICATE</div>
    <div class="body">This is to certify that the complaint bearing reference number <strong>${c.id}</strong> filed by <strong>${c.name}</strong>, ${c.designation||''} of ${c.department||''}, has been successfully resolved by the Maintenance Department of District & Sessions Court Karachi South.</div>
    <table>
      <tr><td>Complaint No.</td><td>${c.id}</td></tr><tr><td>Complainant</td><td>${c.name}</td></tr>
      <tr><td>Designation</td><td>${c.designation||'—'}</td></tr><tr><td>Department</td><td>${c.department||'—'}</td></tr>
      <tr><td>Category</td><td>${c.category||'—'}</td></tr><tr><td>Description</td><td>${c.description||'—'}</td></tr>
      <tr><td>Date Filed</td><td>${dt}</td></tr>
      <tr><td>Resolved On</td><td>${c.resolved_date||new Date().toLocaleDateString('en-PK',{day:'2-digit',month:'long',year:'numeric'})}</td></tr>
      <tr><td>Resolved By</td><td>${c.resolved_by||'Admin'}</td></tr>
      ${c.tech?`<tr><td>Technician</td><td>${c.tech}</td></tr>`:''}
      ${c.approved_by?`<tr><td>Approved By</td><td>${c.approved_by}</td></tr>`:''}
      ${c.remarks?`<tr><td>Remarks</td><td>${c.remarks}</td></tr>`:''}
    </table>
    <div class="sigs">
      <div class="sig"><div class="stamp">OFFICE<br>STAMP</div><div class="sigline"></div><div style="font-size:.82rem;font-weight:bold">Presiding Officer</div><div style="font-size:.74rem;color:#5c5852">${c.department||'District & Sessions Court Karachi South'}</div></div>
      <div class="sig"><div class="stamp">COURT<br>SEAL</div><div class="sigline"></div><div style="font-size:.82rem;font-weight:bold">Maintenance Incharge</div><div style="font-size:.74rem;color:#5c5852">District & Sessions Court Karachi South</div></div>
    </div>
    <hr>
    <div class="footer">Generated: ${new Date().toLocaleString('en-PK')} &nbsp;|&nbsp; District & Sessions Court Karachi South</div>
  </div></div>
  <div class="noprint" style="text-align:center;margin-top:16px"><button onclick="window.print()" style="background:#c9a44a;color:#0b1f3a;border:none;padding:9px 26px;border-radius:7px;font-size:.92rem;font-weight:bold;cursor:pointer">🖨️ Print Certificate</button></div>
  </body></html>`);
  w.document.close();
}

// ═══════════════════════════════════════════════════
//  ADMIN TABS
// ═══════════════════════════════════════════════════
function aTab(tab){
  ['dash','list','accounts','report','users','settings'].forEach(t=>{
    document.getElementById(`tab-${t}`).style.display=t===tab?'block':'none';
    document.getElementById(`at-${t}`).classList.toggle('on',t===tab);
  });
  if(tab==='list')renderList();
  if(tab==='accounts')renderAccounts();
  if(tab==='report')renderReport();
  if(tab==='dash'){renderChart();}
  if(tab==='users')renderUsers();
}

// ═══════════════════════════════════════════════════
//  COURT USERS LIST
// ═══════════════════════════════════════════════════
function renderUsers(){
  const el=document.getElementById('usersList');
  if(!courtUsers.length){el.innerHTML='<p style="color:var(--gray-4)">No users found.</p>';return;}
  el.innerHTML=`<table class="users-table">
    <thead><tr><th>#</th><th>Court / Branch</th><th>Username</th><th>Password</th></tr></thead>
    <tbody>${courtUsers.map((u,i)=>`<tr><td>${i+1}</td><td>${esc(u.court_name)}</td><td><code>${esc(u.username)}</code></td><td><code>${esc(u.password)}</code></td></tr>`).join('')}</tbody>
  </table>`;
}

// ═══════════════════════════════════════════════════
//  ACCOUNTS
// ═══════════════════════════════════════════════════
function setAccType(type){document.getElementById('accType').value=type;document.getElementById('accTypeInc').classList.toggle('on',type==='income');document.getElementById('accTypeExp').classList.toggle('on',type==='expense');document.getElementById('partyLbl').textContent=type==='income'?'Payer — Received From':'Payee — Paid To';}

async function addAccount(){
  const type=document.getElementById('accType').value;
  const party=document.getElementById('accParty').value.trim();
  const amt=parseFloat(document.getElementById('accAmt').value);
  const date=document.getElementById('accDate').value;
  const note=document.getElementById('accNote').value.trim();
  if(!party){toast('Party required.','e');return;}if(!amt||amt<=0){toast('Valid amount required.','e');return;}if(!date){toast('Date required.','e');return;}
  try{const res=await sb('POST','accounts',{type,party,amount:amt,note,date});accounts.unshift(res&&res[0]?res[0]:{type,party,amount:amt,note,date});renderAccounts();document.getElementById('accParty').value='';document.getElementById('accAmt').value='';document.getElementById('accNote').value='';toast('Entry added.','s');}
  catch(e){toast('Failed: '+e.message,'e');}
}

function renderAccounts(){
  const filter=document.getElementById('ledgerFilter')?.value||'';
  const from=document.getElementById('accFrom')?.value||'';
  const to=document.getElementById('accTo')?.value||'';
  let list=accounts;
  if(filter)list=list.filter(a=>a.type===filter);
  if(from)list=list.filter(a=>a.date>=from);
  if(to)list=list.filter(a=>a.date<=to);
  const totalInc=accounts.filter(a=>a.type==='income').reduce((s,a)=>s+Number(a.amount),0);
  const totalExp=accounts.filter(a=>a.type==='expense').reduce((s,a)=>s+Number(a.amount),0);
  const bal=totalInc-totalExp;
  document.getElementById('bInc').textContent='Rs.'+totalInc.toLocaleString();
  document.getElementById('bExp').textContent='Rs.'+totalExp.toLocaleString();
  const bEl=document.getElementById('bBal');bEl.textContent='Rs.'+Math.abs(bal).toLocaleString()+(bal<0?' (Deficit)':'');bEl.style.color=bal>=0?'var(--ok)':'var(--err)';
  const el=document.getElementById('ledgerList');const em=document.getElementById('ledgerEmpty');
  if(!list.length){el.innerHTML='';em.style.display='block';return;}em.style.display='none';
  el.innerHTML=list.map(a=>{const isInc=a.type==='income';return `<div class="ledger-item ${isInc?'li-inc':'li-exp'}">
    <div><div style="font-size:.76rem;font-weight:600;color:${isInc?'var(--ok)':'var(--err)'};text-transform:uppercase;margin-bottom:2px">${isInc?'Income':'Expense'}</div><div style="font-weight:600;font-size:.88rem">${esc(a.party)}</div><div style="font-size:.76rem;color:var(--gray-4)">${a.note||'—'} · ${a.date}</div></div>
    <div style="text-align:right"><div style="font-weight:700;font-size:.98rem;color:${isInc?'var(--ok)':'var(--err)'}">${isInc?'+':'-'}Rs.${Number(a.amount).toLocaleString()}</div>
    <div style="display:flex;gap:5px;margin-top:5px;justify-content:flex-end"><button class="ab ab-view" onclick="editAcc('${a.id}')"><i class="fas fa-edit"></i></button><button class="ab ab-del" onclick="delAcc('${a.id}')"><i class="fas fa-trash"></i></button></div></div>
  </div>`;}).join('');
}

function editAcc(id){const a=accounts.find(x=>x.id===id);if(!a)return;document.getElementById('eaId').value=id;document.getElementById('eaParty').value=a.party;document.getElementById('eaAmt').value=a.amount;document.getElementById('eaNote').value=a.note||'';document.getElementById('eaDate').value=a.date;openModal('mAcc');}
async function saveAccEdit(){const id=document.getElementById('eaId').value;const party=document.getElementById('eaParty').value.trim();const amt=parseFloat(document.getElementById('eaAmt').value);const note=document.getElementById('eaNote').value.trim();const date=document.getElementById('eaDate').value;if(!party||!amt){toast('Required.','e');return;}try{await sb('PATCH','accounts',{party,amount:amt,note,date},`?id=eq.${id}`);const a=accounts.find(x=>x.id===id);if(a)Object.assign(a,{party,amount:amt,note,date});closeModal('mAcc');renderAccounts();toast('Updated.','s');}catch(e){toast('Failed.','e');}}
async function delAcc(id){if(!confirm('Delete?'))return;try{await sb('DELETE','accounts','',`?id=eq.${id}`);accounts=accounts.filter(x=>x.id!==id);renderAccounts();toast('Deleted.','s');}catch(e){toast('Failed.','e');}}
function printAccounts(){const totalInc=accounts.filter(a=>a.type==='income').reduce((s,a)=>s+Number(a.amount),0);const totalExp=accounts.filter(a=>a.type==='expense').reduce((s,a)=>s+Number(a.amount),0);const w=window.open('','_blank');w.document.write(`<!DOCTYPE html><html><head><title>Accounts</title><style>body{font-family:Arial,sans-serif;padding:20px}table{width:100%;border-collapse:collapse}th,td{border:1px solid #ddd;padding:7px;font-size:12px}th{background:#0b1f3a;color:white}.inc{color:green}.exp{color:red}h2{color:#0b1f3a}</style></head><body><h2>District & Sessions Court Karachi South — Accounts Ledger</h2><p style="font-size:12px;color:#888">Generated: ${new Date().toLocaleString()}</p><p><strong>Income:</strong> Rs.${totalInc.toLocaleString()} | <strong>Expense:</strong> Rs.${totalExp.toLocaleString()} | <strong>Balance:</strong> Rs.${(totalInc-totalExp).toLocaleString()}</p><table><thead><tr><th>Type</th><th>Party</th><th>Amount</th><th>Note</th><th>Date</th></tr></thead><tbody>${accounts.map(a=>`<tr><td class="${a.type==='income'?'inc':'exp'}">${a.type==='income'?'Income':'Expense'}</td><td>${a.party}</td><td class="${a.type==='income'?'inc':'exp'}">${a.type==='income'?'+':'-'}Rs.${Number(a.amount).toLocaleString()}</td><td>${a.note||'—'}</td><td>${a.date}</td></tr>`).join('')}</tbody></table></body></html>`);w.document.close();w.print();}

// ═══════════════════════════════════════════════════
//  REPORTS
// ═══════════════════════════════════════════════════
function renderReport(){
  const from=document.getElementById('rFrom')?.value;const to=document.getElementById('rTo')?.value;
  let list=complaints;if(from)list=list.filter(c=>c.date>=from);if(to)list=list.filter(c=>c.date<=to+'T23:59:59');
  document.getElementById('reportPrev').innerHTML=`<div style="margin-bottom:10px;font-size:.82rem;color:var(--gray-6)">Showing <strong>${list.length}</strong> complaints</div>
  ${list.length?`<table style="width:100%;border-collapse:collapse;font-size:.8rem"><thead><tr style="background:var(--navy);color:white"><th style="padding:7px;text-align:left">ID</th><th style="padding:7px;text-align:left">Name</th><th style="padding:7px;text-align:left">Category</th><th style="padding:7px;text-align:left">Status</th><th style="padding:7px;text-align:left">Date</th></tr></thead><tbody>${list.map((c,i)=>`<tr style="background:${i%2?'var(--gray-1)':'white'}"><td style="padding:6px 7px;font-weight:600">${esc(c.id)}</td><td style="padding:6px 7px">${esc(c.name)}</td><td style="padding:6px 7px">${esc(c.category||'—')}</td><td style="padding:6px 7px">${esc(c.status)}</td><td style="padding:6px 7px">${new Date(c.date).toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'})}</td></tr>`).join('')}</tbody></table>`:'<p style="color:var(--gray-4)">No complaints.</p>'}`;
}

function printReport(){
  const from=document.getElementById('rFrom')?.value;const to=document.getElementById('rTo')?.value;
  let list=complaints;if(from)list=list.filter(c=>c.date>=from);if(to)list=list.filter(c=>c.date<=to+'T23:59:59');
  const w=window.open('','_blank');
  w.document.write(`<!DOCTYPE html><html><head><title>Report</title><style>body{font-family:Arial,sans-serif;padding:20px}table{width:100%;border-collapse:collapse}th,td{border:1px solid #ddd;padding:7px;font-size:12px}th{background:#0b1f3a;color:white}h2{color:#0b1f3a}tr:nth-child(even){background:#f5f5f5}</style></head><body><h2>District & Sessions Court Karachi South — Complaint Report</h2><p style="font-size:12px;color:#888">Generated: ${new Date().toLocaleString()} | Total: ${list.length}</p><table><thead><tr><th>ID</th><th>Name</th><th>Department</th><th>Category</th><th>Status</th><th>Date</th></tr></thead><tbody>${list.map(c=>`<tr><td>${c.id}</td><td>${c.name}</td><td>${c.department||'—'}</td><td>${c.category||'—'}</td><td>${c.status}</td><td>${new Date(c.date).toLocaleDateString()}</td></tr>`).join('')}</tbody></table></body></html>`);
  w.document.close();w.print();
}

function generateReport(){
  const from=document.getElementById('rFrom')?.value;const to=document.getElementById('rTo')?.value;
  let list=complaints;if(from)list=list.filter(c=>c.date>=from);if(to)list=list.filter(c=>c.date<=to+'T23:59:59');
  const totalC=list.length;const pending=list.filter(c=>c.status==='Pending').length;const inprog=list.filter(c=>c.status==='In Progress').length;const resolved=list.filter(c=>c.status==='Resolved').length;
  const cats=['Electrical','Plumbing','Furniture','Civil','Others'];
  const catCounts=cats.map(cat=>({name:cat,count:list.filter(c=>(c.category||'').includes(cat)).length}));
  const w=window.open('','_blank');
  w.document.write(`<!DOCTYPE html><html><head><title>DSC Report</title><style>body{font-family:'Times New Roman',serif;padding:36px;max-width:800px;margin:0 auto;color:#0b1f3a}.outer{border:6px double #c9a44a;padding:26px}.hdr{text-align:center;margin-bottom:20px;border-bottom:2px solid #c9a44a;padding-bottom:14px}h1{font-size:1.15rem;font-weight:bold;margin:4px 0}h2{font-size:.9rem;color:#5c5852;font-weight:normal;margin:2px 0}.rep-title{font-size:1.2rem;font-weight:bold;text-align:center;letter-spacing:2px;margin:14px 0;text-decoration:underline}.summary{display:flex;gap:14px;margin:16px 0;flex-wrap:wrap}.sum-box{flex:1;min-width:120px;border:1px solid #d5d0c6;padding:12px;text-align:center;border-radius:6px}.sum-num{font-size:1.6rem;font-weight:bold}.sum-lbl{font-size:.75rem;color:#5c5852;margin-top:2px}table{width:100%;border-collapse:collapse;font-size:.82rem;margin-top:14px}th{background:#0b1f3a;color:white;padding:7px 8px;text-align:left}td{padding:6px 8px;border:1px solid #d5d0c6}tr:nth-child(even){background:#f9f7f2}.footer{text-align:center;font-size:.72rem;color:#9a9590;margin-top:20px;border-top:1px solid #d5d0c6;padding-top:10px}@media print{.noprint{display:none}}</style></head><body>
  <div class="outer"><div class="hdr"><div style="font-size:40px;margin-bottom:6px">⚖️</div><h1>DISTRICT & SESSIONS COURT KARACHI SOUTH</h1><h2>Repair & Maintenance Complaint Management System</h2></div>
  <div class="rep-title">COMPLAINT REPORT</div>
  <div style="font-size:.82rem;color:#5c5852;text-align:center;margin-bottom:14px">Period: ${from||'All time'} ${to?'to '+to:''} | Generated: ${new Date().toLocaleString('en-PK')}</div>
  <div class="summary"><div class="sum-box"><div class="sum-num" style="color:#1a4f8a">${totalC}</div><div class="sum-lbl">Total</div></div><div class="sum-box"><div class="sum-num" style="color:#8a5800">${pending}</div><div class="sum-lbl">Pending</div></div><div class="sum-box"><div class="sum-num" style="color:#1a5ea8">${inprog}</div><div class="sum-lbl">In Progress</div></div><div class="sum-box"><div class="sum-num" style="color:#1a6e3f">${resolved}</div><div class="sum-lbl">Resolved</div></div></div>
  <div style="font-weight:bold;font-size:.88rem;margin:14px 0 6px">Category Summary: ${catCounts.map(x=>`${x.name}: ${x.count}`).join(' | ')}</div>
  <table><thead><tr><th>#</th><th>Complaint No.</th><th>Name</th><th>Department</th><th>Category</th><th>Status</th><th>Date</th></tr></thead>
  <tbody>${list.map((comp,i)=>`<tr><td>${i+1}</td><td style="font-weight:bold">${comp.id}</td><td>${comp.name}</td><td style="font-size:.76rem">${comp.department||'—'}</td><td>${comp.category||'—'}</td><td>${comp.status}</td><td>${new Date(comp.date).toLocaleDateString('en-PK',{day:'2-digit',month:'short',year:'numeric'})}</td></tr>`).join('')}</tbody></table>
  <div style="display:flex;justify-content:space-between;margin-top:44px"><div style="text-align:center;width:44%"><div style="border-top:1px solid #0b1f3a;padding-top:5px;font-size:.8rem;font-weight:bold">Irfan Ahmed Memon</div><div style="font-size:.74rem;color:#5c5852">Jr. Clerk — DSC Karachi South</div></div><div style="text-align:center;width:44%"><div style="border-top:1px solid #0b1f3a;padding-top:5px;font-size:.8rem;font-weight:bold">District & Sessions Judge</div><div style="font-size:.74rem;color:#5c5852">Karachi South</div></div></div>
  <div class="footer">District & Sessions Court Karachi South | ${new Date().toLocaleString('en-PK')}</div></div>
  <div class="noprint" style="text-align:center;margin-top:16px"><button onclick="window.print()" style="background:#c9a44a;color:#0b1f3a;border:none;padding:9px 26px;border-radius:7px;font-size:.92rem;font-weight:bold;cursor:pointer">🖨️ Print</button></div>
  </body></html>`);
  w.document.close();
}

function exportCSV(){
  const from=document.getElementById('rFrom')?.value;const to=document.getElementById('rTo')?.value;
  let list=complaints;if(from)list=list.filter(c=>c.date>=from);if(to)list=list.filter(c=>c.date<=to+'T23:59:59');
  const rows=[['ID','Name','Designation','Department','Block','Category','Status','Phone','Date','Technician','Remarks','Approved By','Description']];
  list.forEach(c=>rows.push([c.id,c.name,c.designation,c.department,c.block,c.category,c.status,c.phone,new Date(c.date).toLocaleDateString(),c.tech,c.remarks,c.approved_by,c.description].map(v=>`"${(v||'').replace(/"/g,'""')}"`)));
  const csv=rows.map(r=>r.join(',')).join('\n');
  const a=document.createElement('a');a.href='data:text/csv;charset=utf-8,'+encodeURIComponent(csv);a.download=`DSC_Report_${new Date().toISOString().slice(0,10)}.csv`;a.click();
}

// ═══════════════════════════════════════════════════
//  CHANGE PASSWORD
// ═══════════════════════════════════════════════════
function changePassword(){
  const curr=document.getElementById('pwCurr').value;const nw=document.getElementById('pwNew').value;const conf=document.getElementById('pwConf').value;
  if(!curr||!nw||!conf){toast('All fields required.','e');return;}
  if(adminPasswords[currentUser]!==curr){toast('Current password incorrect.','e');return;}
  if(nw.length<6){toast('Min 6 characters.','e');return;}
  if(nw!==conf){toast('Passwords do not match.','e');return;}
  adminPasswords[currentUser]=nw;localStorage.setItem('pw_'+currentUser,nw);
  document.getElementById('pwCurr').value='';document.getElementById('pwNew').value='';document.getElementById('pwConf').value='';
  const msg=document.getElementById('pwMsg');msg.style.display='block';setTimeout(()=>msg.style.display='none',4000);
  toast('Password changed!','s');
}

// ═══════════════════════════════════════════════════
//  NOTIFICATIONS
// ═══════════════════════════════════════════════════
function addNotif(c){notifications.unshift({id:c.id,name:c.name,dept:c.department,cat:c.category,type:c.type||'new',time:new Date().toISOString(),read:false});if(notifications.length>30)notifications=notifications.slice(0,30);try{localStorage.setItem('dsc_notifs',JSON.stringify(notifications));}catch(e){}updateBell();}
function updateBell(){const u=notifications.filter(n=>!n.read).length;document.getElementById('nDot').classList.toggle('vis',u>0);}
function toggleNotif(){document.getElementById('nDrop').classList.toggle('on');renderNotifs();}
function renderNotifs(){
  const el=document.getElementById('nList');
  if(!notifications.length){el.innerHTML='<div style="padding:14px;font-size:.81rem;color:var(--gray-4);text-align:center">No notifications</div>';return;}
  el.innerHTML=notifications.map(n=>{n.read=true;const ico=n.type==='resolved'?'✅':'🔔';const label=n.type==='resolved'?'Resolved':'New complaint';return `<div style="padding:10px 14px;border-bottom:1px solid var(--gray-1);font-size:.8rem"><div style="font-weight:600;color:var(--navy)">${ico} ${esc(n.id)} — ${esc(n.name)}</div><div style="font-size:.74rem;color:var(--gray-4)">${label} · ${esc(n.cat||'—')} · ${new Date(n.time).toLocaleTimeString()}</div></div>`;}).join('');
  try{localStorage.setItem('dsc_notifs',JSON.stringify(notifications));}catch(e){}updateBell();
}
function clearNotifs(){notifications=[];try{localStorage.setItem('dsc_notifs','[]');}catch(e){}document.getElementById('nDrop').classList.remove('on');updateBell();}
document.addEventListener('click',e=>{if(!e.target.closest('#nDrop')&&!e.target.closest('#nBell'))document.getElementById('nDrop').classList.remove('on');});

// ═══════════════════════════════════════════════════
//  MODALS + UTILS
// ═══════════════════════════════════════════════════
function openModal(id){document.getElementById(id).classList.add('on');}
function closeModal(id){document.getElementById(id).classList.remove('on');}
function openImgModal(src){document.getElementById('mImgSrc').src=src;openModal('mImg');}
document.querySelectorAll('.mo').forEach(m=>m.addEventListener('click',function(e){if(e.target===this&&this.id!=='mImg')closeModal(this.id);}));
document.addEventListener('keydown',e=>{if(e.key==='Escape')document.querySelectorAll('.mo.on').forEach(m=>m.classList.remove('on'));});
function esc(s){return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function infoRow(k,v){return `<div class="info-row"><div class="ik">${k}</div><div class="iv">${esc(v)}</div></div>`;}
function toast(msg,type='i'){const el=document.createElement('div');el.className=`toast ${type}`;el.textContent=msg;document.getElementById('toastBox').appendChild(el);setTimeout(()=>el.remove(),4000);}
function copyId(id){if(navigator.clipboard&&window.isSecureContext)navigator.clipboard.writeText(id).then(()=>toast('Copied: '+id,'s'));else{const t=document.createElement('textarea');t.value=id;document.body.appendChild(t);t.select();document.execCommand('copy');document.body.removeChild(t);toast('Copied: '+id,'s');}}
function setSyncStatus(msg){const el=document.getElementById('syncStatus');if(el)el.textContent=msg;}
function updateClock(){const t=new Date().toLocaleString('en-PK',{weekday:'short',day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit',hour12:true});const el=document.getElementById('hdrTime');if(el)el.textContent=t;}
</script>
</body>
</html>
