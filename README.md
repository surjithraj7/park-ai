<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SmartPark AI — Intelligent Parking Management</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root {
  --cyan: #00f5d4; --cyan-dim: #00c4aa; --electric: #7c3aed; --electric-glow: #a78bfa;
  --gold: #f59e0b; --bg: #050b14; --bg2: #0a1628; --bg3: #0f1f3a;
  --surface: #112240; --surface2: #1a2f50; --text: #e2e8f0; --text-muted: #8898aa;
  --text-dim: #4a5568; --border: rgba(0,245,212,0.12); --border-hover: rgba(0,245,212,0.3);
  --red: #ef4444; --green: #22c55e; --orange: #f97316;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
html{scroll-behavior:smooth;}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);overflow-x:hidden;line-height:1.6;}
::-webkit-scrollbar{width:4px;}::-webkit-scrollbar-track{background:var(--bg);}::-webkit-scrollbar-thumb{background:var(--cyan-dim);border-radius:2px;}

/* ── NOISE ── */
body::before{content:'';position:fixed;inset:0;background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");pointer-events:none;z-index:0;opacity:0.4;}
.grid-bg{position:absolute;inset:0;background-image:linear-gradient(rgba(0,245,212,0.03) 1px,transparent 1px),linear-gradient(90deg,rgba(0,245,212,0.03) 1px,transparent 1px);background-size:60px 60px;pointer-events:none;}

/* ── OVERLAY PAGES ── */
.page-overlay{display:none;position:fixed;inset:0;z-index:200;background:var(--bg);overflow-y:auto;}
.page-overlay.active{display:block;}

/* ── NAV ── */
nav{position:fixed;top:0;left:0;right:0;z-index:100;display:flex;align-items:center;justify-content:space-between;padding:1rem 3rem;background:rgba(5,11,20,0.9);backdrop-filter:blur(20px);border-bottom:1px solid var(--border);}
.logo{font-family:'Syne',sans-serif;font-weight:800;font-size:1.4rem;color:var(--cyan);letter-spacing:-0.02em;display:flex;align-items:center;gap:0.5rem;cursor:pointer;}
.logo-icon{width:32px;height:32px;background:linear-gradient(135deg,var(--cyan),var(--electric));border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:16px;}
.nav-links{display:flex;gap:2rem;align-items:center;}
.nav-links a{font-size:0.75rem;color:var(--text-muted);text-decoration:none;font-weight:500;letter-spacing:0.02em;transition:color 0.2s;text-transform:uppercase;cursor:pointer;}
.nav-links a:hover{color:var(--cyan);}
.nav-cta{padding:0.5rem 1.25rem;background:var(--cyan);color:var(--bg)!important;border-radius:6px;font-weight:600!important;}
.nav-cta:hover{background:var(--cyan-dim)!important;}
.nav-user{display:flex;align-items:center;gap:0.75rem;}
.nav-avatar{width:32px;height:32px;border-radius:50%;background:linear-gradient(135deg,var(--electric),var(--cyan-dim));display:flex;align-items:center;justify-content:center;font-size:0.75rem;font-weight:700;color:white;cursor:pointer;}
.nav-username{font-size:0.8rem;color:var(--text-muted);}

/* ── HERO ── */
.hero{position:relative;min-height:100vh;display:flex;align-items:center;justify-content:center;text-align:center;padding:8rem 2rem 4rem;overflow:hidden;}
.hero-orb{position:absolute;border-radius:50%;filter:blur(80px);pointer-events:none;}
.orb1{width:500px;height:500px;background:rgba(0,245,212,0.08);top:-100px;left:-100px;}
.orb2{width:400px;height:400px;background:rgba(124,58,237,0.1);bottom:0;right:-50px;}
.orb3{width:300px;height:300px;background:rgba(245,158,11,0.06);top:50%;left:50%;transform:translate(-50%,-50%);}
.hero-badge{display:inline-flex;align-items:center;gap:0.5rem;background:rgba(0,245,212,0.08);border:1px solid rgba(0,245,212,0.2);border-radius:100px;padding:0.35rem 1rem;font-size:0.75rem;color:var(--cyan);font-family:'JetBrains Mono',monospace;letter-spacing:0.05em;text-transform:uppercase;margin-bottom:2rem;animation:fadeInDown 0.8s ease;}
.badge-dot{width:6px;height:6px;background:var(--cyan);border-radius:50%;animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:0.5;transform:scale(1.5)}}
.hero h1{font-family:'Syne',sans-serif;font-size:clamp(2.5rem,7vw,5.5rem);font-weight:800;line-height:1.0;letter-spacing:-0.03em;margin-bottom:1.5rem;animation:fadeInUp 0.8s ease 0.1s both;}
.gradient-text{background:linear-gradient(135deg,var(--cyan) 0%,var(--electric-glow) 50%,var(--gold) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.hero p{max-width:560px;margin:0 auto 2.5rem;font-size:1.125rem;color:var(--text-muted);line-height:1.7;animation:fadeInUp 0.8s ease 0.2s both;}
.hero-buttons{display:flex;gap:1rem;justify-content:center;flex-wrap:wrap;animation:fadeInUp 0.8s ease 0.3s both;}
@keyframes fadeInDown{from{opacity:0;transform:translateY(-20px)}to{opacity:1;transform:translateY(0)}}
@keyframes fadeInUp{from{opacity:0;transform:translateY(30px)}to{opacity:1;transform:translateY(0)}}

/* ── CAR SVG STRIP ── */
.car-strip{position:absolute;bottom:0;left:0;right:0;height:140px;overflow:hidden;pointer-events:none;}
.car-track{display:flex;gap:0;width:200%;animation:carScroll 18s linear infinite;}
.car-track-rev{display:flex;gap:0;width:200%;animation:carScrollRev 22s linear infinite;margin-top:8px;}
@keyframes carScroll{from{transform:translateX(0)}to{transform:translateX(-50%)}}
@keyframes carScrollRev{from{transform:translateX(-50%)}to{transform:translateX(0)}}

/* ── STATS ── */
.stats-bar{display:flex;gap:0;justify-content:center;border:1px solid var(--border);border-radius:12px;background:var(--bg2);max-width:750px;margin:4rem auto 0;overflow:hidden;animation:fadeInUp 0.8s ease 0.4s both;}
.stat-item{flex:1;padding:1.5rem;border-right:1px solid var(--border);text-align:center;}
.stat-item:last-child{border-right:none;}
.stat-num{font-family:'Syne',sans-serif;font-size:1.75rem;font-weight:800;color:var(--cyan);display:block;}
.stat-label{font-size:0.75rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;}

/* ── BUTTONS ── */
.btn-primary{padding:0.875rem 2rem;background:linear-gradient(135deg,var(--cyan),var(--cyan-dim));color:var(--bg);border:none;border-radius:8px;font-weight:700;font-size:0.9rem;cursor:pointer;text-decoration:none;font-family:'DM Sans',sans-serif;transition:transform 0.2s,box-shadow 0.2s;letter-spacing:0.02em;display:inline-block;}
.btn-primary:hover{transform:translateY(-2px);box-shadow:0 12px 40px rgba(0,245,212,0.3);}
.btn-outline{padding:0.875rem 2rem;background:transparent;color:var(--text);border:1px solid var(--border-hover);border-radius:8px;font-weight:500;font-size:0.9rem;cursor:pointer;text-decoration:none;font-family:'DM Sans',sans-serif;transition:all 0.2s;display:inline-block;}
.btn-outline:hover{border-color:var(--cyan);color:var(--cyan);background:rgba(0,245,212,0.05);}
.btn-sm{padding:0.5rem 1rem;font-size:0.8rem;}

/* ── SECTIONS ── */
section{padding:6rem 2rem;position:relative;}
.container{max-width:1200px;margin:0 auto;}
.section-label{font-family:'JetBrains Mono',monospace;font-size:0.7rem;letter-spacing:0.15em;text-transform:uppercase;color:var(--cyan);margin-bottom:0.75rem;}
.section-title{font-family:'Syne',sans-serif;font-size:clamp(1.8rem,4vw,3rem);font-weight:800;letter-spacing:-0.02em;line-height:1.15;margin-bottom:1rem;}
.section-sub{color:var(--text-muted);max-width:520px;line-height:1.7;}

/* ── DASHBOARD ── */
.dashboard-section{background:var(--bg2);}
.dashboard-grid{display:grid;grid-template-columns:1fr 1.4fr;gap:2rem;margin-top:3rem;align-items:start;}
.lot-map{background:var(--bg3);border:1px solid var(--border);border-radius:16px;padding:1.5rem;position:relative;}
.lot-map-title{font-family:'JetBrains Mono',monospace;font-size:0.7rem;color:var(--cyan);letter-spacing:0.1em;text-transform:uppercase;margin-bottom:1.25rem;display:flex;align-items:center;gap:0.5rem;}
.live-dot{width:6px;height:6px;background:var(--green);border-radius:50%;animation:pulse 1.5s infinite;}
.parking-grid{display:grid;grid-template-columns:repeat(8,1fr);gap:5px;}
.pspot{aspect-ratio:1;border-radius:4px;cursor:pointer;transition:transform 0.15s;position:relative;}
.pspot:hover{transform:scale(1.1);z-index:2;}
.pspot.free{background:rgba(34,197,94,0.2);border:1px solid rgba(34,197,94,0.4);}
.pspot.occupied{background:rgba(239,68,68,0.25);border:1px solid rgba(239,68,68,0.5);}
.pspot.reserved{background:rgba(245,158,11,0.2);border:1px solid rgba(245,158,11,0.4);}
.pspot.ev{background:rgba(0,245,212,0.15);border:1px solid rgba(0,245,212,0.35);}
.pspot.disabled{background:rgba(139,92,246,0.2);border:1px solid rgba(139,92,246,0.4);}
.lot-legend{display:flex;gap:1rem;margin-top:1rem;flex-wrap:wrap;}
.legend-item{display:flex;align-items:center;gap:0.35rem;font-size:0.7rem;color:var(--text-muted);}
.legend-dot{width:8px;height:8px;border-radius:2px;}
.dashboard-right{display:flex;flex-direction:column;gap:1.25rem;}
.metric-card{background:var(--bg3);border:1px solid var(--border);border-radius:12px;padding:1.25rem 1.5rem;display:flex;align-items:center;justify-content:space-between;transition:border-color 0.3s;}
.metric-card:hover{border-color:var(--border-hover);}
.metric-info{display:flex;flex-direction:column;gap:0.2rem;}
.metric-label{font-size:0.75rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;}
.metric-value{font-family:'Syne',sans-serif;font-size:1.75rem;font-weight:800;color:var(--text);}
.metric-change{font-size:0.75rem;}
.metric-change.up{color:var(--green);}.metric-change.down{color:var(--red);}
.metric-icon{width:48px;height:48px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:1.25rem;}
.icon-green{background:rgba(34,197,94,0.12);}.icon-red{background:rgba(239,68,68,0.12);}.icon-cyan{background:rgba(0,245,212,0.1);}.icon-gold{background:rgba(245,158,11,0.12);}
.occ-bar-wrap{margin-top:0.5rem;}
.occ-bar-track{height:6px;background:var(--surface2);border-radius:3px;overflow:hidden;}
.occ-bar-fill{height:100%;border-radius:3px;background:linear-gradient(90deg,var(--cyan),var(--electric-glow));transition:width 1s ease;}

/* ── FEATURES ── */
.features-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem;margin-top:3rem;}
.feat-card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:2rem;transition:all 0.3s;position:relative;overflow:hidden;}
.feat-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,transparent,var(--cyan),transparent);opacity:0;transition:opacity 0.3s;}
.feat-card:hover{border-color:var(--border-hover);transform:translateY(-4px);box-shadow:0 20px 60px rgba(0,0,0,0.3);}
.feat-card:hover::before{opacity:1;}
.feat-icon{width:52px;height:52px;border-radius:12px;background:rgba(0,245,212,0.08);border:1px solid rgba(0,245,212,0.15);display:flex;align-items:center;justify-content:center;font-size:1.5rem;margin-bottom:1.25rem;}
.feat-title{font-family:'Syne',sans-serif;font-size:1.1rem;font-weight:700;margin-bottom:0.6rem;}
.feat-desc{font-size:0.875rem;color:var(--text-muted);line-height:1.65;}
.feat-tag{display:inline-block;margin-top:1rem;padding:0.2rem 0.65rem;background:rgba(124,58,237,0.15);border:1px solid rgba(124,58,237,0.3);border-radius:4px;font-family:'JetBrains Mono',monospace;font-size:0.65rem;color:var(--electric-glow);letter-spacing:0.05em;}

/* ── TERMINAL ── */
.terminal{background:#020810;border:1px solid rgba(0,245,212,0.2);border-radius:12px;overflow:hidden;font-family:'JetBrains Mono',monospace;}
.terminal-bar{background:var(--surface);padding:0.75rem 1rem;display:flex;align-items:center;gap:0.5rem;border-bottom:1px solid rgba(0,245,212,0.1);}
.t-dot{width:10px;height:10px;border-radius:50%;}
.t-red{background:#ff5f57;}.t-yellow{background:#ffbd2e;}.t-green{background:#28ca40;}
.terminal-title{font-size:0.7rem;color:var(--text-muted);margin-left:0.5rem;letter-spacing:0.05em;}
.terminal-body{padding:1.5rem;min-height:280px;font-size:0.78rem;line-height:1.8;}
.t-line{display:block;}.t-prompt{color:var(--cyan);}.t-cmd{color:var(--text);}.t-out{color:#64748b;}.t-val{color:var(--gold);}.t-good{color:var(--green);}.t-warn{color:var(--orange);}
.t-cursor{display:inline-block;width:8px;height:14px;background:var(--cyan);animation:blink 1s infinite;vertical-align:middle;border-radius:1px;}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0}}

/* ── HOW IT WORKS ── */
.hiw-steps{display:grid;grid-template-columns:repeat(4,1fr);gap:0;margin-top:4rem;position:relative;}
.hiw-steps::before{content:'';position:absolute;top:32px;left:12%;right:12%;height:1px;background:linear-gradient(90deg,transparent,var(--border-hover),transparent);}
.step-card{text-align:center;padding:1.5rem 1rem;}
.step-num{width:64px;height:64px;border-radius:50%;background:var(--bg2);border:1px solid var(--border-hover);display:flex;align-items:center;justify-content:center;margin:0 auto 1.5rem;font-family:'Syne',sans-serif;font-size:1.4rem;font-weight:800;color:var(--cyan);position:relative;z-index:1;}
.step-title{font-family:'Syne',sans-serif;font-weight:700;font-size:1rem;margin-bottom:0.5rem;}
.step-desc{font-size:0.825rem;color:var(--text-muted);line-height:1.6;}

/* ── PRICING ── */
.pricing-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem;margin-top:3rem;}
.price-card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:2rem;transition:all 0.3s;position:relative;}
.price-card.featured{border-color:var(--cyan);background:linear-gradient(135deg,rgba(0,245,212,0.04),var(--bg2));}
.price-card:hover{transform:translateY(-4px);box-shadow:0 20px 60px rgba(0,0,0,0.3);}
.popular-badge{position:absolute;top:-12px;left:50%;transform:translateX(-50%);background:linear-gradient(135deg,var(--cyan),var(--electric-glow));color:var(--bg);font-size:0.65rem;font-weight:700;padding:0.25rem 0.75rem;border-radius:100px;letter-spacing:0.06em;text-transform:uppercase;white-space:nowrap;}
.price-tier{font-size:0.75rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.75rem;}
.price-amount{font-family:'Syne',sans-serif;font-size:2.5rem;font-weight:800;}
.price-period{font-size:0.8rem;color:var(--text-muted);margin-left:0.25rem;}
.price-features{list-style:none;margin:1.5rem 0;}
.price-features li{padding:0.5rem 0;font-size:0.875rem;color:var(--text-muted);display:flex;gap:0.6rem;align-items:center;border-bottom:1px solid rgba(255,255,255,0.04);}
.price-features li:last-child{border-bottom:none;}
.check{color:var(--green);font-weight:700;}

/* ── TESTIMONIALS ── */
.testimonials-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem;margin-top:3rem;}
.testi-card{background:var(--bg3);border:1px solid var(--border);border-radius:16px;padding:1.75rem;transition:border-color 0.3s;}
.testi-card:hover{border-color:var(--border-hover);}
.stars{color:var(--gold);font-size:0.875rem;margin-bottom:1rem;letter-spacing:0.1em;}
.testi-text{font-size:0.875rem;color:var(--text-muted);line-height:1.7;margin-bottom:1.25rem;font-style:italic;}
.testi-author{display:flex;align-items:center;gap:0.75rem;}
.author-avatar{width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:0.85rem;background:linear-gradient(135deg,var(--electric),var(--cyan-dim));color:white;flex-shrink:0;}
.author-name{font-weight:600;font-size:0.875rem;}
.author-role{font-size:0.75rem;color:var(--text-muted);}

/* ── CTA ── */
.cta-section{text-align:center;padding:8rem 2rem;position:relative;overflow:hidden;}
.cta-section::before{content:'';position:absolute;width:600px;height:600px;background:radial-gradient(circle,rgba(0,245,212,0.06) 0%,transparent 70%);top:50%;left:50%;transform:translate(-50%,-50%);border-radius:50%;}
.cta-section h2{font-family:'Syne',sans-serif;font-size:clamp(2rem,5vw,3.5rem);font-weight:800;margin-bottom:1rem;letter-spacing:-0.02em;}

/* ── FOOTER ── */
footer{background:var(--bg);border-top:1px solid var(--border);padding:3rem 2rem;}
.footer-inner{max-width:1200px;margin:0 auto;display:flex;justify-content:space-between;align-items:center;}
.footer-links{display:flex;gap:2rem;}
.footer-links a{font-size:0.8rem;color:var(--text-muted);text-decoration:none;transition:color 0.2s;}
.footer-links a:hover{color:var(--cyan);}
.footer-copy{font-size:0.75rem;color:var(--text-dim);}

/* ── DETECTION ── */
.detect-section{background:var(--bg3);}
.detect-layout{display:grid;grid-template-columns:1fr 1fr;gap:3rem;align-items:start;margin-top:3rem;}
.camera-feed{background:#010509;border:1px solid var(--border);border-radius:12px;overflow:hidden;position:relative;aspect-ratio:16/10;}
.camera-overlay{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;flex-direction:column;gap:0.75rem;}
.scan-frame{width:160px;height:120px;border:2px solid var(--cyan);border-radius:8px;position:relative;animation:scanPulse 2s ease infinite;}
@keyframes scanPulse{0%,100%{border-color:var(--cyan);box-shadow:0 0 0 0 rgba(0,245,212,0.3)}50%{border-color:var(--cyan-dim);box-shadow:0 0 20px rgba(0,245,212,0.2)}}
.scan-line{position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,transparent,var(--cyan),transparent);animation:scanMove 1.5s linear infinite;}
@keyframes scanMove{from{top:0}to{top:100%}}
.scan-frame::before{content:'';position:absolute;top:-2px;left:-2px;width:12px;height:12px;border:3px solid var(--gold);border-right:none;border-bottom:none;}
.scan-frame::after{content:'';position:absolute;top:-2px;right:-2px;width:12px;height:12px;border:3px solid var(--gold);border-left:none;border-bottom:none;}
.camera-status{font-family:'JetBrains Mono',monospace;font-size:0.65rem;color:var(--cyan);letter-spacing:0.1em;}
.cam-label{position:absolute;bottom:12px;left:12px;font-family:'JetBrains Mono',monospace;font-size:0.6rem;color:var(--cyan);opacity:0.6;}
.cam-rec{position:absolute;top:12px;right:12px;display:flex;align-items:center;gap:4px;font-family:'JetBrains Mono',monospace;font-size:0.6rem;color:var(--red);}
.rec-dot{width:5px;height:5px;background:var(--red);border-radius:50%;animation:pulse 1s infinite;}
.detections-log{display:flex;flex-direction:column;gap:0.75rem;}
.det-entry{background:var(--bg2);border:1px solid var(--border);border-radius:8px;padding:1rem 1.25rem;display:flex;align-items:center;justify-content:space-between;font-family:'JetBrains Mono',monospace;font-size:0.72rem;}
.det-plate{color:var(--text);font-weight:500;letter-spacing:0.08em;}
.det-time{color:var(--text-muted);}
.det-status{padding:0.2rem 0.6rem;border-radius:4px;font-size:0.65rem;font-weight:600;}
.det-in{background:rgba(34,197,94,0.15);color:var(--green);}
.det-out{background:rgba(239,68,68,0.15);color:var(--red);}
.det-vip{background:rgba(245,158,11,0.15);color:var(--gold);}

/* ── AI PANEL ── */
.ai-panel-section{background:var(--bg2);}
.ai-panel-layout{display:grid;grid-template-columns:1.2fr 1fr;gap:3rem;align-items:center;margin-top:3rem;}
.ai-features-list{list-style:none;}
.ai-feat-item{display:flex;gap:1rem;align-items:flex-start;padding:1.25rem 0;border-bottom:1px solid var(--border);}
.ai-feat-item:last-child{border-bottom:none;}
.ai-feat-num{font-family:'JetBrains Mono',monospace;font-size:0.7rem;color:var(--cyan);min-width:28px;margin-top:0.2rem;}
.ai-feat-title{font-weight:600;font-size:0.95rem;margin-bottom:0.25rem;}
.ai-feat-desc{font-size:0.825rem;color:var(--text-muted);}

/* ── REVEAL ── */
.reveal{opacity:0;transform:translateY(30px);transition:opacity 0.6s ease,transform 0.6s ease;}
.reveal.visible{opacity:1;transform:translateY(0);}

/* ── RESPONSIVE ── */
@media(max-width:900px){
  .dashboard-grid,.ai-panel-layout,.detect-layout{grid-template-columns:1fr;}
  .features-grid,.hiw-steps,.pricing-grid,.testimonials-grid{grid-template-columns:1fr;}
  nav{padding:1rem 1.25rem;}.nav-links{display:none;}
}

/* ══════════════════════════════════════
   LOGIN / REGISTER PAGE
══════════════════════════════════════ */
.auth-page{min-height:100vh;display:grid;grid-template-columns:1fr 1fr;position:relative;}
@media(max-width:768px){.auth-page{grid-template-columns:1fr;}}

.auth-visual{position:relative;background:#020810;overflow:hidden;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:3rem;}
.auth-visual-bg{position:absolute;inset:0;background:radial-gradient(ellipse at center,rgba(0,245,212,0.08) 0%,transparent 70%);}
.auth-visual-grid{position:absolute;inset:0;background-image:linear-gradient(rgba(0,245,212,0.04) 1px,transparent 1px),linear-gradient(90deg,rgba(0,245,212,0.04) 1px,transparent 1px);background-size:50px 50px;}
.auth-car-showcase{position:relative;z-index:1;width:100%;max-width:460px;}
.auth-brand{font-family:'Syne',sans-serif;font-size:2rem;font-weight:800;color:var(--cyan);margin-bottom:0.5rem;display:flex;align-items:center;gap:0.75rem;}
.auth-tagline{color:var(--text-muted);font-size:0.95rem;margin-bottom:3rem;}
.auth-stats{display:grid;grid-template-columns:repeat(2,1fr);gap:1rem;margin-top:2rem;}
.auth-stat{background:rgba(0,245,212,0.05);border:1px solid rgba(0,245,212,0.12);border-radius:10px;padding:1rem;}
.auth-stat-num{font-family:'Syne',sans-serif;font-size:1.5rem;font-weight:800;color:var(--cyan);}
.auth-stat-label{font-size:0.7rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;}

.auth-form-panel{background:var(--bg);display:flex;align-items:center;justify-content:center;padding:3rem 2rem;}
.auth-form-box{width:100%;max-width:420px;}
.auth-title{font-family:'Syne',sans-serif;font-size:2rem;font-weight:800;margin-bottom:0.4rem;}
.auth-sub{color:var(--text-muted);font-size:0.9rem;margin-bottom:2rem;}
.auth-tabs{display:flex;background:var(--bg2);border-radius:8px;padding:4px;margin-bottom:2rem;border:1px solid var(--border);}
.auth-tab{flex:1;padding:0.6rem;border-radius:6px;text-align:center;font-size:0.85rem;font-weight:600;cursor:pointer;transition:all 0.2s;color:var(--text-muted);}
.auth-tab.active{background:var(--cyan);color:var(--bg);}
.form-group{margin-bottom:1.25rem;}
.form-label{font-size:0.78rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:0.4rem;display:block;}
.form-input{width:100%;padding:0.75rem 1rem;background:var(--bg2);border:1px solid var(--border);border-radius:8px;color:var(--text);font-size:0.9rem;font-family:'DM Sans',sans-serif;transition:border-color 0.2s;outline:none;}
.form-input:focus{border-color:var(--cyan);}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:1rem;}
.form-error{font-size:0.75rem;color:var(--red);margin-top:0.35rem;display:none;}
.form-error.show{display:block;}
.auth-btn{width:100%;padding:0.875rem;background:linear-gradient(135deg,var(--cyan),var(--cyan-dim));color:var(--bg);border:none;border-radius:8px;font-weight:700;font-size:0.95rem;cursor:pointer;font-family:'Syne',sans-serif;transition:all 0.2s;margin-top:0.5rem;}
.auth-btn:hover{transform:translateY(-2px);box-shadow:0 8px 30px rgba(0,245,212,0.3);}
.auth-divider{text-align:center;color:var(--text-dim);font-size:0.8rem;margin:1.25rem 0;position:relative;}
.auth-divider::before,.auth-divider::after{content:'';position:absolute;top:50%;width:40%;height:1px;background:var(--border);}
.auth-divider::before{left:0;}.auth-divider::after{right:0;}
.social-btns{display:grid;grid-template-columns:1fr 1fr;gap:0.75rem;}
.social-btn{padding:0.65rem;border:1px solid var(--border);border-radius:8px;background:var(--bg2);color:var(--text-muted);font-size:0.85rem;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:0.5rem;transition:all 0.2s;font-family:'DM Sans',sans-serif;}
.social-btn:hover{border-color:var(--border-hover);color:var(--text);}
.back-link{display:inline-flex;align-items:center;gap:0.5rem;color:var(--cyan);font-size:0.85rem;cursor:pointer;margin-bottom:2rem;text-decoration:none;transition:opacity 0.2s;}
.back-link:hover{opacity:0.8;}

/* ══════════════════════════════════════
   BOOKING PAGE
══════════════════════════════════════ */
.booking-page{min-height:100vh;padding:7rem 2rem 4rem;}
.booking-grid{display:grid;grid-template-columns:1.5fr 1fr;gap:2.5rem;max-width:1100px;margin:2rem auto 0;}
@media(max-width:900px){.booking-grid{grid-template-columns:1fr;}}
.booking-map-section{}
.booking-card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:2rem;}
.booking-slot-grid{display:grid;grid-template-columns:repeat(8,1fr);gap:6px;margin:1.5rem 0;}
.b-spot{aspect-ratio:1;border-radius:5px;cursor:pointer;transition:all 0.15s;position:relative;}
.b-spot.free{background:rgba(34,197,94,0.15);border:1px solid rgba(34,197,94,0.4);}
.b-spot.free:hover{background:rgba(34,197,94,0.35);transform:scale(1.08);}
.b-spot.occupied{background:rgba(239,68,68,0.2);border:1px solid rgba(239,68,68,0.4);cursor:not-allowed;}
.b-spot.selected{background:rgba(0,245,212,0.3);border:2px solid var(--cyan);transform:scale(1.08);}
.b-spot.ev{background:rgba(0,245,212,0.1);border:1px solid rgba(0,245,212,0.3);}
.b-spot.disabled{background:rgba(139,92,246,0.15);border:1px solid rgba(139,92,246,0.3);}
.booking-summary{position:sticky;top:7rem;}
.summary-row{display:flex;justify-content:space-between;padding:0.6rem 0;border-bottom:1px solid var(--border);font-size:0.875rem;}
.summary-row:last-child{border-bottom:none;}
.summary-row.total{font-weight:700;font-size:1rem;color:var(--cyan);}
.spot-label{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;font-size:0.45rem;color:rgba(255,255,255,0.4);font-family:'JetBrains Mono',monospace;}

/* ══════════════════════════════════════
   PAYMENT PAGE
══════════════════════════════════════ */
.payment-page{min-height:100vh;padding:7rem 2rem 4rem;}
.payment-container{max-width:860px;margin:0 auto;}
.payment-grid{display:grid;grid-template-columns:1fr 380px;gap:2rem;margin-top:2rem;}
@media(max-width:800px){.payment-grid{grid-template-columns:1fr;}}
.payment-methods{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:2rem;}
.method-tabs{display:flex;gap:0.75rem;margin-bottom:2rem;flex-wrap:wrap;}
.method-tab{padding:0.6rem 1.25rem;border:1px solid var(--border);border-radius:8px;cursor:pointer;font-size:0.85rem;font-weight:500;transition:all 0.2s;background:transparent;color:var(--text-muted);}
.method-tab.active{border-color:var(--cyan);background:rgba(0,245,212,0.08);color:var(--cyan);}
.method-content{display:none;}
.method-content.active{display:block;}
.upi-apps{display:grid;grid-template-columns:repeat(4,1fr);gap:0.75rem;margin-bottom:1.5rem;}
.upi-app{border:1px solid var(--border);border-radius:10px;padding:0.75rem;text-align:center;cursor:pointer;transition:all 0.2s;}
.upi-app:hover,.upi-app.selected{border-color:var(--cyan);background:rgba(0,245,212,0.06);}
.upi-app-icon{font-size:1.5rem;margin-bottom:0.3rem;}
.upi-app-name{font-size:0.7rem;color:var(--text-muted);}
.card-form{display:flex;flex-direction:column;gap:1rem;}
.card-preview{background:linear-gradient(135deg,#0f2b4a,#1a3d5c);border-radius:14px;padding:1.5rem;margin-bottom:1.5rem;position:relative;overflow:hidden;}
.card-preview::before{content:'';position:absolute;right:-20px;top:-20px;width:120px;height:120px;border-radius:50%;background:rgba(0,245,212,0.05);border:30px solid rgba(0,245,212,0.05);}
.card-chip{width:32px;height:24px;background:linear-gradient(135deg,#d4a754,#f0c870);border-radius:4px;margin-bottom:1rem;}
.card-number-preview{font-family:'JetBrains Mono',monospace;font-size:1.1rem;letter-spacing:0.2em;color:rgba(255,255,255,0.9);margin-bottom:1rem;}
.card-meta{display:flex;justify-content:space-between;font-size:0.7rem;color:rgba(255,255,255,0.6);}
.wallet-options{display:grid;grid-template-columns:1fr 1fr;gap:1rem;}
.wallet-opt{border:1px solid var(--border);border-radius:10px;padding:1rem;cursor:pointer;transition:all 0.2s;display:flex;align-items:center;gap:0.75rem;}
.wallet-opt:hover,.wallet-opt.selected{border-color:var(--cyan);background:rgba(0,245,212,0.06);}
.wallet-icon{font-size:1.5rem;}
.wallet-name{font-size:0.875rem;font-weight:600;}
.wallet-bal{font-size:0.72rem;color:var(--green);}

.payment-summary-box{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:2rem;height:fit-content;}
.pay-summary-title{font-family:'Syne',sans-serif;font-size:1.1rem;font-weight:700;margin-bottom:1.5rem;}
.pay-row{display:flex;justify-content:space-between;padding:0.65rem 0;border-bottom:1px solid var(--border);font-size:0.875rem;}
.pay-row:last-child{border-bottom:none;}
.pay-total{font-weight:700;font-size:1.05rem;color:var(--cyan);}
.pay-now-btn{width:100%;padding:1rem;background:linear-gradient(135deg,var(--cyan),var(--cyan-dim));color:var(--bg);border:none;border-radius:10px;font-weight:700;font-size:1rem;cursor:pointer;font-family:'Syne',sans-serif;margin-top:1.5rem;transition:all 0.2s;}
.pay-now-btn:hover{transform:translateY(-2px);box-shadow:0 10px 35px rgba(0,245,212,0.3);}
.security-note{display:flex;align-items:center;gap:0.5rem;font-size:0.72rem;color:var(--text-muted);margin-top:0.75rem;justify-content:center;}

/* ── SUCCESS MODAL ── */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(5,11,20,0.85);backdrop-filter:blur(8px);z-index:300;align-items:center;justify-content:center;}
.modal-overlay.active{display:flex;}
.modal-box{background:var(--bg2);border:1px solid rgba(0,245,212,0.3);border-radius:20px;padding:3rem;max-width:440px;width:90%;text-align:center;animation:modalIn 0.4s ease;}
@keyframes modalIn{from{opacity:0;transform:scale(0.9)}to{opacity:1;transform:scale(1)}}
.modal-icon{font-size:4rem;margin-bottom:1rem;}
.modal-title{font-family:'Syne',sans-serif;font-size:1.6rem;font-weight:800;margin-bottom:0.5rem;}
.modal-sub{color:var(--text-muted);font-size:0.9rem;margin-bottom:2rem;}
.modal-ticket{background:var(--bg3);border:1px solid var(--border);border-radius:12px;padding:1.25rem;margin-bottom:1.5rem;font-family:'JetBrains Mono',monospace;font-size:0.78rem;text-align:left;}
.ticket-row{display:flex;justify-content:space-between;padding:0.35rem 0;color:var(--text-muted);}
.ticket-val{color:var(--cyan);}

/* ── DASHBOARD USER ── */
.user-dashboard{padding:7rem 2rem 4rem;}
.dashboard-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:2rem;}
.dash-welcome{font-family:'Syne',sans-serif;font-size:1.6rem;font-weight:800;}
.bookings-table{width:100%;border-collapse:collapse;margin-top:1.5rem;}
.bookings-table th{text-align:left;padding:0.75rem 1rem;font-size:0.72rem;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-muted);border-bottom:1px solid var(--border);}
.bookings-table td{padding:0.875rem 1rem;font-size:0.875rem;border-bottom:1px solid rgba(255,255,255,0.04);}
.bookings-table tr:last-child td{border-bottom:none;}
.status-badge{padding:0.2rem 0.6rem;border-radius:4px;font-size:0.7rem;font-weight:600;}
.status-active{background:rgba(34,197,94,0.15);color:var(--green);}
.status-completed{background:rgba(100,116,139,0.2);color:var(--text-muted);}
.status-upcoming{background:rgba(245,158,11,0.15);color:var(--gold);}

/* ── CAR SVG STYLES ── */
.car-display{width:100%;filter:drop-shadow(0 20px 40px rgba(0,245,212,0.2));}

/* ── NOTIFICATION TOAST ── */
.toast{position:fixed;bottom:2rem;right:2rem;background:var(--bg2);border:1px solid rgba(0,245,212,0.3);border-radius:10px;padding:1rem 1.5rem;font-size:0.85rem;z-index:500;transform:translateY(100px);opacity:0;transition:all 0.3s;max-width:320px;}
.toast.show{transform:translateY(0);opacity:1;}
.toast-success{border-color:rgba(34,197,94,0.4);}
.toast-error{border-color:rgba(239,68,68,0.4);}
</style>
</head>
<body>

<!-- ── TOAST ── -->
<div class="toast" id="toast"></div>

<!-- ══════════════════════════════════════
     SUCCESS MODAL
══════════════════════════════════════ -->
<div class="modal-overlay" id="successModal">
  <div class="modal-box">
    <div class="modal-icon">✅</div>
    <div class="modal-title">Payment Successful!</div>
    <div class="modal-sub">Your parking slot has been booked. Check your email for confirmation.</div>
    <div class="modal-ticket" id="ticketDetails">
      <div class="ticket-row"><span>Booking ID</span><span class="ticket-val" id="t-id"></span></div>
      <div class="ticket-row"><span>Slot</span><span class="ticket-val" id="t-slot"></span></div>
      <div class="ticket-row"><span>Duration</span><span class="ticket-val" id="t-dur"></span></div>
      <div class="ticket-row"><span>Amount Paid</span><span class="ticket-val" id="t-amt"></span></div>
      <div class="ticket-row"><span>Status</span><span class="ticket-val" style="color:var(--green)">CONFIRMED</span></div>
    </div>
    <button class="auth-btn" onclick="closeModal()">View My Bookings</button>
  </div>
</div>

<!-- ══════════════════════════════════════
     NAV
══════════════════════════════════════ -->
<nav id="mainNav">
  <div class="logo" onclick="showPage('main')">
    <div class="logo-icon">🅿</div>
    SmartPark<span style="color:var(--text-muted);font-weight:400"> AI</span>
  </div>
  <div class="nav-links" id="navLinks">
    <a onclick="showPage('main');scrollTo(0,0)">Home</a>
    <a onclick="document.getElementById('dashboard').scrollIntoView({behavior:'smooth'})">Live Map</a>
    <a onclick="document.getElementById('features').scrollIntoView({behavior:'smooth'})">Features</a>
    <a onclick="document.getElementById('pricing').scrollIntoView({behavior:'smooth'})">Pricing</a>
    <a onclick="requireAuth('booking')" class="nav-cta">Book Slot</a>
  </div>
  <div id="navUser" style="display:none" class="nav-user">
    <div class="nav-avatar" id="navAvatar" onclick="showPage('userdash')">U</div>
    <span class="nav-username" id="navUsername"></span>
    <a onclick="logout()" style="font-size:0.75rem;color:var(--text-muted);cursor:pointer;margin-left:0.5rem;">Logout</a>
  </div>
</nav>

<!-- ══════════════════════════════════════
     MAIN WEBSITE
══════════════════════════════════════ -->
<div id="mainPage">

<!-- HERO -->
<section class="hero" id="home">
  <div class="grid-bg"></div>
  <div class="hero-orb orb1"></div>
  <div class="hero-orb orb2"></div>
  <div class="hero-orb orb3"></div>

  <div style="position:relative;z-index:1;max-width:900px;margin:0 auto;">
    <div class="hero-badge"><span class="badge-dot"></span>AI-Powered · Real-Time · Computer Vision Enabled</div>
    <h1>The Future of<br><span class="gradient-text">Smart Parking</span><br>Is Here</h1>
    <p>Transform any parking facility into an intelligent ecosystem. Real-time slot detection, AI license plate recognition, predictive analytics, and seamless payment — all in one platform.</p>
    <div class="hero-buttons">
      <button class="btn-primary" onclick="requireAuth('booking')">Book a Slot →</button>
      <button class="btn-outline" onclick="document.getElementById('features').scrollIntoView({behavior:'smooth'})">Explore Features</button>
    </div>
    <div class="stats-bar">
      <div class="stat-item"><span class="stat-num" id="cnt1">0</span><span class="stat-label">Slots Managed</span></div>
      <div class="stat-item"><span class="stat-num" id="cnt2">0</span><span class="stat-label">Cities Deployed</span></div>
      <div class="stat-item"><span class="stat-num" id="cnt3">0</span><span class="stat-label">Vehicles / Day</span></div>
      <div class="stat-item"><span class="stat-num" id="cnt4">0%</span><span class="stat-label">Uptime SLA</span></div>
    </div>
  </div>

  <!-- CAR SVG STRIP -->
  <div class="car-strip">
    <div style="height:70px;overflow:hidden;opacity:0.25;">
      <div class="car-track" id="carTrack1"></div>
    </div>
    <div style="height:60px;overflow:hidden;opacity:0.15;margin-top:4px;">
      <div class="car-track-rev" id="carTrack2"></div>
    </div>
  </div>
</section>

<!-- CAR SHOWCASE SECTION -->
<section style="background:var(--bg2);padding:5rem 2rem;">
  <div class="container">
    <div class="section-label">// ai vehicle recognition</div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:4rem;align-items:center;">
      <div>
        <h2 class="section-title">Detect Any Vehicle,<br><span class="gradient-text">Instantly</span></h2>
        <p class="section-sub">Our computer vision engine identifies sedans, SUVs, trucks, motorcycles, and EVs in under 200ms — day or night, rain or shine.</p>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:1rem;margin-top:2rem;">
          <div style="background:var(--bg3);border:1px solid var(--border);border-radius:10px;padding:1rem;text-align:center;">
            <div style="font-family:'Syne',sans-serif;font-size:1.4rem;font-weight:800;color:var(--cyan);">99.7%</div>
            <div style="font-size:0.72rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;">Detection Accuracy</div>
          </div>
          <div style="background:var(--bg3);border:1px solid var(--border);border-radius:10px;padding:1rem;text-align:center;">
            <div style="font-family:'Syne',sans-serif;font-size:1.4rem;font-weight:800;color:var(--gold);">200ms</div>
            <div style="font-size:0.72rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;">Response Time</div>
          </div>
          <div style="background:var(--bg3);border:1px solid var(--border);border-radius:10px;padding:1rem;text-align:center;">
            <div style="font-family:'Syne',sans-serif;font-size:1.4rem;font-weight:800;color:var(--green);">8+</div>
            <div style="font-size:0.72rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;">Vehicle Types</div>
          </div>
          <div style="background:var(--bg3);border:1px solid var(--border);border-radius:10px;padding:1rem;text-align:center;">
            <div style="font-family:'Syne',sans-serif;font-size:1.4rem;font-weight:800;color:var(--electric-glow);">24/7</div>
            <div style="font-size:0.72rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;">All Conditions</div>
          </div>
        </div>
      </div>
      <div style="position:relative;">
        <!-- SVG Car Showcase -->
        <svg viewBox="0 0 500 300" class="car-display" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <linearGradient id="bodyGrad" x1="0%" y1="0%" x2="100%" y2="100%">
              <stop offset="0%" style="stop-color:#1a3a5c"/>
              <stop offset="100%" style="stop-color:#0d2137"/>
            </linearGradient>
            <linearGradient id="roofGrad" x1="0%" y1="0%" x2="0%" y2="100%">
              <stop offset="0%" style="stop-color:#2a5080"/>
              <stop offset="100%" style="stop-color:#1a3a5c"/>
            </linearGradient>
            <filter id="glow"><feGaussianBlur stdDeviation="3" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
            <filter id="softglow"><feGaussianBlur stdDeviation="6" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
          </defs>
          <!-- Road -->
          <rect x="0" y="245" width="500" height="55" fill="#060e1a" rx="0"/>
          <rect x="0" y="243" width="500" height="4" fill="#0d1f35"/>
          <!-- Road markings -->
          <rect x="40" y="266" width="60" height="4" fill="rgba(0,245,212,0.15)" rx="2"/>
          <rect x="140" y="266" width="60" height="4" fill="rgba(0,245,212,0.15)" rx="2"/>
          <rect x="240" y="266" width="60" height="4" fill="rgba(0,245,212,0.15)" rx="2"/>
          <rect x="340" y="266" width="60" height="4" fill="rgba(0,245,212,0.15)" rx="2"/>
          <rect x="440" y="266" width="60" height="4" fill="rgba(0,245,212,0.15)" rx="2"/>
          <!-- Shadow -->
          <ellipse cx="250" cy="248" rx="160" ry="12" fill="rgba(0,0,0,0.5)"/>
          <!-- Car Body -->
          <rect x="60" y="180" width="380" height="65" rx="8" fill="url(#bodyGrad)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
          <!-- Roof / Cabin -->
          <path d="M155 180 Q175 130 205 125 L295 125 Q330 130 345 180Z" fill="url(#roofGrad)" stroke="rgba(0,245,212,0.15)" stroke-width="1"/>
          <!-- Windows -->
          <path d="M165 178 Q178 145 205 138 L245 138 L245 178Z" fill="rgba(0,245,212,0.08)" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <path d="M250 178 L250 138 L295 138 Q322 145 332 178Z" fill="rgba(0,245,212,0.08)" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <!-- Window divider -->
          <line x1="249" y1="138" x2="249" y2="178" stroke="rgba(0,245,212,0.3)" stroke-width="2"/>
          <!-- Front bumper detail -->
          <rect x="60" y="220" width="380" height="10" rx="4" fill="rgba(0,245,212,0.08)" stroke="rgba(0,245,212,0.15)" stroke-width="1"/>
          <!-- Headlights -->
          <ellipse cx="95" cy="205" rx="22" ry="12" fill="#0a1f35" stroke="rgba(0,245,212,0.4)" stroke-width="1.5" filter="url(#glow)"/>
          <ellipse cx="95" cy="205" rx="14" ry="7" fill="rgba(0,245,212,0.15)"/>
          <ellipse cx="95" cy="205" rx="6" ry="3" fill="rgba(0,245,212,0.6)" filter="url(#glow)"/>
          <!-- Headlight beam -->
          <path d="M72 200 L10 185 L10 215 L72 208Z" fill="rgba(0,245,212,0.04)"/>
          <!-- Tail lights -->
          <ellipse cx="405" cy="205" rx="22" ry="12" fill="#0a1f35" stroke="rgba(239,68,68,0.5)" stroke-width="1.5"/>
          <ellipse cx="405" cy="205" rx="14" ry="7" fill="rgba(239,68,68,0.15)"/>
          <ellipse cx="405" cy="205" rx="6" ry="3" fill="rgba(239,68,68,0.7)"/>
          <!-- Grille -->
          <rect x="68" y="198" width="18" height="18" rx="2" fill="rgba(0,245,212,0.06)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
          <line x1="68" y1="204" x2="86" y2="204" stroke="rgba(0,245,212,0.3)" stroke-width="1"/>
          <line x1="68" y1="210" x2="86" y2="210" stroke="rgba(0,245,212,0.3)" stroke-width="1"/>
          <line x1="75" y1="198" x2="75" y2="216" stroke="rgba(0,245,212,0.3)" stroke-width="1"/>
          <line x1="80" y1="198" x2="80" y2="216" stroke="rgba(0,245,212,0.3)" stroke-width="1"/>
          <!-- Door lines -->
          <line x1="185" y1="182" x2="185" y2="242" stroke="rgba(255,255,255,0.08)" stroke-width="1.5"/>
          <line x1="315" y1="182" x2="315" y2="242" stroke="rgba(255,255,255,0.08)" stroke-width="1.5"/>
          <!-- Door handles -->
          <rect x="225" y="208" width="20" height="4" rx="2" fill="rgba(0,245,212,0.3)"/>
          <rect x="270" y="208" width="20" height="4" rx="2" fill="rgba(0,245,212,0.3)"/>
          <!-- Wheels -->
          <circle cx="145" cy="242" r="28" fill="#080f1a" stroke="rgba(0,245,212,0.3)" stroke-width="2"/>
          <circle cx="145" cy="242" r="20" fill="#0d1a2e"/>
          <circle cx="145" cy="242" r="12" fill="#0a1525" stroke="rgba(0,245,212,0.4)" stroke-width="1"/>
          <circle cx="145" cy="242" r="4" fill="rgba(0,245,212,0.6)"/>
          <!-- Wheel spokes -->
          <line x1="145" y1="222" x2="145" y2="262" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="125" y1="242" x2="165" y2="242" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="131" y1="228" x2="159" y2="256" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>
          <line x1="131" y1="256" x2="159" y2="228" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>

          <circle cx="355" cy="242" r="28" fill="#080f1a" stroke="rgba(0,245,212,0.3)" stroke-width="2"/>
          <circle cx="355" cy="242" r="20" fill="#0d1a2e"/>
          <circle cx="355" cy="242" r="12" fill="#0a1525" stroke="rgba(0,245,212,0.4)" stroke-width="1"/>
          <circle cx="355" cy="242" r="4" fill="rgba(0,245,212,0.6)"/>
          <line x1="355" y1="222" x2="355" y2="262" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="335" y1="242" x2="375" y2="242" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="341" y1="228" x2="369" y2="256" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>
          <line x1="341" y1="256" x2="369" y2="228" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>

          <!-- Scan overlay -->
          <rect x="60" y="125" width="380" height="125" rx="4" fill="none" stroke="rgba(0,245,212,0.5)" stroke-width="1.5" stroke-dasharray="8,4"/>
          <!-- Corner markers -->
          <path d="M60 145 L60 125 L85 125" stroke="var(--cyan)" stroke-width="2.5" fill="none"/>
          <path d="M415 125 L440 125 L440 145" stroke="var(--cyan)" stroke-width="2.5" fill="none"/>
          <path d="M60 230 L60 250 L85 250" stroke="var(--cyan)" stroke-width="2.5" fill="none"/>
          <path d="M415 250 L440 250 L440 230" stroke="var(--cyan)" stroke-width="2.5" fill="none"/>
          <!-- Plate -->
          <rect x="195" y="227" width="110" height="22" rx="3" fill="#e8e8e8"/>
          <text x="250" y="243" text-anchor="middle" font-family="monospace" font-size="11" font-weight="bold" fill="#1a1a2e">TN09 AB 1234</text>
          <!-- Scan label -->
          <text x="250" y="115" text-anchor="middle" font-family="monospace" font-size="9" fill="rgba(0,245,212,0.7)" letter-spacing="2">VEHICLE DETECTED · PLATE READING</text>
          <!-- Confidence -->
          <text x="440" y="175" text-anchor="end" font-family="monospace" font-size="8" fill="rgba(0,245,212,0.6)">CONF: 99.7%</text>
          <!-- EV badge -->
          <rect x="200" y="148" width="36" height="16" rx="3" fill="rgba(0,245,212,0.2)" stroke="rgba(0,245,212,0.5)" stroke-width="1"/>
          <text x="218" y="160" text-anchor="middle" font-family="monospace" font-size="8" fill="var(--cyan)" font-weight="bold">⚡EV</text>
        </svg>
        <!-- Floating badges -->
        <div style="position:absolute;top:10px;right:0;background:rgba(34,197,94,0.15);border:1px solid rgba(34,197,94,0.4);border-radius:8px;padding:0.4rem 0.8rem;font-family:'JetBrains Mono',monospace;font-size:0.65rem;color:var(--green);">✓ SLOT ASSIGNED: B2-14</div>
        <div style="position:absolute;bottom:60px;left:-10px;background:rgba(245,158,11,0.1);border:1px solid rgba(245,158,11,0.3);border-radius:8px;padding:0.4rem 0.8rem;font-family:'JetBrains Mono',monospace;font-size:0.65rem;color:var(--gold);">⚡ EV CHARGING AVAILABLE</div>
      </div>
    </div>
  </div>
</section>

<!-- LIVE DASHBOARD -->
<section class="dashboard-section" id="dashboard">
  <div class="container">
    <div class="section-label">// live overview</div>
    <h2 class="section-title">Real-Time Parking Dashboard</h2>
    <p class="section-sub">Monitor every slot in your facility, live. AI detects occupancy changes within milliseconds.</p>
    <div class="dashboard-grid">
      <div class="lot-map reveal">
        <div class="lot-map-title"><span class="live-dot"></span>LEVEL B2 — MAIN FACILITY</div>
        <div class="parking-grid" id="parkingGrid"></div>
        <div class="lot-legend">
          <div class="legend-item"><div class="legend-dot" style="background:rgba(34,197,94,0.6)"></div>Free</div>
          <div class="legend-item"><div class="legend-dot" style="background:rgba(239,68,68,0.6)"></div>Occupied</div>
          <div class="legend-item"><div class="legend-dot" style="background:rgba(245,158,11,0.6)"></div>Reserved</div>
          <div class="legend-item"><div class="legend-dot" style="background:rgba(0,245,212,0.6)"></div>EV Charging</div>
          <div class="legend-item"><div class="legend-dot" style="background:rgba(139,92,246,0.6)"></div>Disabled</div>
        </div>
        <div style="margin-top:1rem;font-size:0.72rem;color:var(--text-muted);">⟳ Updates every 500ms via AI camera feed</div>
      </div>
      <div class="dashboard-right reveal">
        <div class="metric-card">
          <div class="metric-info">
            <span class="metric-label">Available Slots</span>
            <span class="metric-value" id="freeCount">28</span>
            <span class="metric-change up">↑ 3 in last 5 min</span>
            <div class="occ-bar-wrap"><div class="occ-bar-track"><div class="occ-bar-fill" id="freeBar" style="width:45%"></div></div></div>
          </div>
          <div class="metric-icon icon-green">🟢</div>
        </div>
        <div class="metric-card">
          <div class="metric-info">
            <span class="metric-label">Occupied</span>
            <span class="metric-value" id="occCount">36</span>
            <span class="metric-change down">↓ 2 in last 5 min</span>
          </div>
          <div class="metric-icon icon-red">🔴</div>
        </div>
        <div class="metric-card">
          <div class="metric-info">
            <span class="metric-label">Avg Dwell Time</span>
            <span class="metric-value">42<span style="font-size:1rem;color:var(--text-muted)"> min</span></span>
            <span class="metric-change up">AI predicted: 38 min</span>
          </div>
          <div class="metric-icon icon-cyan">⏱</div>
        </div>
        <div class="metric-card">
          <div class="metric-info">
            <span class="metric-label">Revenue Today</span>
            <span class="metric-value">₹<span id="revenue">12,480</span></span>
            <span class="metric-change up">↑ 18% vs yesterday</span>
          </div>
          <div class="metric-icon icon-gold">💰</div>
        </div>
        <div style="background:var(--bg3);border:1px solid var(--border);border-radius:12px;padding:1.25rem;">
          <div style="font-size:0.7rem;color:var(--cyan);letter-spacing:0.1em;text-transform:uppercase;margin-bottom:1rem;font-family:'JetBrains Mono',monospace;">AI Prediction — Next 2h</div>
          <div id="predChart" style="display:flex;align-items:flex-end;gap:6px;height:70px;"></div>
          <div style="display:flex;justify-content:space-between;margin-top:0.5rem;">
            <span style="font-size:0.65rem;color:var(--text-dim)">9AM</span><span style="font-size:0.65rem;color:var(--text-dim)">10AM</span><span style="font-size:0.65rem;color:var(--text-dim)">11AM</span><span style="font-size:0.65rem;color:var(--text-dim)">12PM</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- FEATURES -->
<section id="features">
  <div class="container">
    <div class="section-label">// capabilities</div>
    <h2 class="section-title">Powered by Advanced AI</h2>
    <p class="section-sub">Every component built on cutting-edge machine learning and computer vision.</p>
    <div class="features-grid">
      <div class="feat-card reveal"><div class="feat-icon">👁️</div><div class="feat-title">Computer Vision Detection</div><div class="feat-desc">YOLO-based overhead cameras detect vehicle presence with 99.7% accuracy, updating slot status in under 200ms.</div><span class="feat-tag">CV · YOLO · 200ms</span></div>
      <div class="feat-card reveal"><div class="feat-icon">🔢</div><div class="feat-title">License Plate Recognition</div><div class="feat-desc">OCR-powered ALPR reads number plates in all lighting conditions. Auto-match for VIP, monthly, and blacklisted vehicles.</div><span class="feat-tag">OCR · ALPR · 97% Accuracy</span></div>
      <div class="feat-card reveal"><div class="feat-icon">📊</div><div class="feat-title">Predictive Occupancy</div><div class="feat-desc">LSTM neural networks forecast peak hours and slot demand up to 72 hours in advance.</div><span class="feat-tag">LSTM · 72h Forecast</span></div>
      <div class="feat-card reveal"><div class="feat-icon">🗺️</div><div class="feat-title">Dynamic Navigation</div><div class="feat-desc">Real-time indoor maps guide drivers to the nearest available slot, reducing search time by 60%.</div><span class="feat-tag">Indoor Maps · BLE Beacons</span></div>
      <div class="feat-card reveal"><div class="feat-icon">💳</div><div class="feat-title">Seamless Payments</div><div class="feat-desc">Automatic billing via UPI, FASTag, card, or QR. Payments triggered on exit — no manual action needed.</div><span class="feat-tag">UPI · FASTag · QR</span></div>
      <div class="feat-card reveal"><div class="feat-icon">🔔</div><div class="feat-title">Smart Alerts</div><div class="feat-desc">Instant SMS/app notifications for over-stay, unauthorized parking, and EV charging completion.</div><span class="feat-tag">SMS · Push · Webhooks</span></div>
    </div>
  </div>
</section>

<!-- VEHICLE DETECTION — REAL CAMERA + OCR -->
<section class="detect-section" id="detection">
  <div class="container">
    <div class="section-label">// ai detection engine</div>
    <h2 class="section-title">Live Number Plate Scanner</h2>
    <p class="section-sub">Point your camera at a vehicle number plate. Our OCR engine reads it instantly and logs the entry.</p>
    <div class="detect-layout">

      <!-- REAL CAMERA FEED -->
      <div style="display:flex;flex-direction:column;gap:1rem;" class="reveal">
        <div class="camera-feed" style="position:relative;background:#010509;border:1px solid var(--border);border-radius:12px;overflow:hidden;aspect-ratio:16/10;">
          <!-- Video element (real camera) -->
          <video id="realCamVideo" autoplay playsinline muted style="width:100%;height:100%;object-fit:cover;display:none;"></video>
          <!-- Canvas for capture -->
          <canvas id="snapCanvas" style="display:none;"></canvas>
          <!-- Captured image preview -->
          <img id="snapPreview" style="width:100%;height:100%;object-fit:contain;display:none;position:absolute;inset:0;background:#010509;" alt="Captured plate"/>

          <!-- Overlay when camera not started -->
          <div class="camera-overlay" id="camPlaceholder">
            <div class="scan-frame"><div class="scan-line"></div></div>
            <div class="camera-status" id="camStatus">CLICK START CAMERA</div>
          </div>

          <!-- Scan frame overlay on video -->
          <div id="liveScanOverlay" style="display:none;position:absolute;inset:0;pointer-events:none;">
            <div style="position:absolute;top:50%;left:50%;transform:translate(-50%,-55%);width:60%;max-width:260px;height:80px;border:2px solid var(--cyan);border-radius:6px;animation:scanPulse 2s ease infinite;">
              <div class="scan-line"></div>
              <span style="position:absolute;top:-18px;left:50%;transform:translateX(-50%);font-family:'JetBrains Mono',monospace;font-size:0.58rem;color:var(--cyan);white-space:nowrap;letter-spacing:0.08em;">ALIGN PLATE HERE</span>
            </div>
          </div>

          <div class="cam-label" style="position:absolute;bottom:12px;left:12px;font-family:'JetBrains Mono',monospace;font-size:0.6rem;color:var(--cyan);opacity:0.7;" id="camLabel">CAM-B2-04 · ANPR MODE</div>
          <div class="cam-rec" style="position:absolute;top:12px;right:12px;display:flex;align-items:center;gap:4px;font-family:'JetBrains Mono',monospace;font-size:0.6rem;color:var(--red);" id="camRecIndicator" style="display:none!important;"><span class="rec-dot"></span> REC</div>
        </div>

        <!-- OCR Result -->
        <div id="ocrResultBox" style="display:none;background:rgba(0,245,212,0.06);border:1px solid rgba(0,245,212,0.3);border-radius:10px;padding:1rem 1.25rem;">
          <div style="font-family:'JetBrains Mono',monospace;font-size:0.65rem;color:var(--cyan);letter-spacing:0.1em;margin-bottom:0.5rem;">OCR RESULT</div>
          <div style="display:flex;align-items:center;gap:1rem;flex-wrap:wrap;">
            <span style="font-family:'JetBrains Mono',monospace;font-size:1.4rem;font-weight:700;color:var(--text);letter-spacing:0.1em;" id="ocrPlateText">—</span>
            <span style="font-size:0.72rem;color:var(--green);font-family:'JetBrains Mono',monospace;" id="ocrConf"></span>
          </div>
          <div style="margin-top:0.75rem;display:flex;gap:0.5rem;flex-wrap:wrap;">
            <button class="btn-primary btn-sm" onclick="useScannedPlate()">✓ Use This Plate &amp; Book</button>
            <button class="btn-outline btn-sm" onclick="retakeScan()">↺ Retake</button>
          </div>
        </div>

        <!-- OCR Progress -->
        <div id="ocrProgress" style="display:none;background:var(--bg3);border:1px solid var(--border);border-radius:10px;padding:1rem 1.25rem;font-family:'JetBrains Mono',monospace;font-size:0.72rem;color:var(--cyan);" id="ocrProgress">
          <span id="ocrProgressText">Reading plate...</span>
          <div style="height:3px;background:var(--surface2);border-radius:2px;margin-top:0.5rem;overflow:hidden;"><div id="ocrBar" style="height:100%;background:var(--cyan);width:0%;transition:width 0.3s;border-radius:2px;"></div></div>
        </div>

        <!-- Camera Buttons -->
        <div style="display:flex;gap:0.75rem;flex-wrap:wrap;">
          <button class="btn-primary btn-sm" id="startCamBtn" onclick="startCamera()">📷 Start Camera</button>
          <button class="btn-outline btn-sm" id="snapBtn" onclick="captureAndScan()" style="display:none;">🔍 Scan Plate</button>
          <button class="btn-outline btn-sm" id="stopCamBtn" onclick="stopCamera()" style="display:none;">⏹ Stop</button>
          <label class="btn-outline btn-sm" style="cursor:pointer;display:inline-block;">
            📁 Upload Image
            <input type="file" accept="image/*" style="display:none;" onchange="handleImageUpload(event)"/>
          </label>
        </div>
      </div>

      <!-- Detection Log -->
      <div class="detections-log reveal">
        <div style="font-family:'JetBrains Mono',monospace;font-size:0.7rem;color:var(--cyan);letter-spacing:0.1em;text-transform:uppercase;margin-bottom:1rem;">Detection Log</div>
        <div id="detectionLog"></div>
      </div>
    </div>
  </div>
</section>

<!-- AI PANEL -->
<section class="ai-panel-section" id="ai">
  <div class="container">
    <div class="section-label">// intelligence layer</div>
    <h2 class="section-title">AI Brain — Always Learning</h2>
    <div class="ai-panel-layout">
      <div class="terminal reveal">
        <div class="terminal-bar">
          <span class="t-dot t-red"></span><span class="t-dot t-yellow"></span><span class="t-dot t-green"></span>
          <span class="terminal-title">smartpark-ai · inference-engine · v3.2.1</span>
        </div>
        <div class="terminal-body">
          <span class="t-line"><span class="t-prompt">$ </span><span class="t-cmd">smartpark analyze --lot=B2</span></span>
          <span class="t-line t-out">Initializing AI inference pipeline...</span>
          <span class="t-line t-out">Loading YOLOv8 model — weights: parking_v4.pt</span>
          <span class="t-line">&nbsp;</span>
          <span class="t-line"><span class="t-prompt">→ </span><span class="t-out">Slots scanned: </span><span class="t-val">64/64</span></span>
          <span class="t-line"><span class="t-prompt">→ </span><span class="t-out">Confidence avg: </span><span class="t-val">99.3%</span></span>
          <span class="t-line"><span class="t-prompt">→ </span><span class="t-out">Vehicles detected: </span><span class="t-val">36</span></span>
          <span class="t-line"><span class="t-prompt">→ </span><span class="t-out">ALPR processed: </span><span class="t-val">34</span></span>
          <span class="t-line">&nbsp;</span>
          <span class="t-line"><span class="t-prompt">⚠ </span><span class="t-warn">ALERT: TN09AB1234 — over-stay (3h 42m)</span></span>
          <span class="t-line"><span class="t-prompt">⚠ </span><span class="t-warn">SLOT B2-07: Unauthorized (no registration)</span></span>
          <span class="t-line">&nbsp;</span>
          <span class="t-line"><span class="t-prompt">✓ </span><span class="t-good">Peak prediction: 1–3 PM (92% occupancy)</span></span>
          <span class="t-line"><span class="t-prompt">✓ </span><span class="t-good">Revenue forecast today: ₹18,200</span></span>
          <span class="t-line">&nbsp;</span>
          <span class="t-line"><span class="t-prompt">$ </span><span class="t-cursor"></span></span>
        </div>
      </div>
      <ul class="ai-features-list reveal">
        <li class="ai-feat-item"><span class="ai-feat-num">01</span><div><div class="ai-feat-title">Continuous Learning</div><div class="ai-feat-desc">The model retrains weekly on new data from your facility, improving detection accuracy over time.</div></div></li>
        <li class="ai-feat-item"><span class="ai-feat-num">02</span><div><div class="ai-feat-title">Anomaly Detection</div><div class="ai-feat-desc">Flags unusual patterns — abandoned vehicles, over-stays, unauthorized occupancy.</div></div></li>
        <li class="ai-feat-item"><span class="ai-feat-num">03</span><div><div class="ai-feat-title">Revenue Intelligence</div><div class="ai-feat-desc">Dynamic pricing suggestions based on demand, time-of-day, and historical patterns.</div></div></li>
        <li class="ai-feat-item"><span class="ai-feat-num">04</span><div><div class="ai-feat-title">Edge Inference</div><div class="ai-feat-desc">AI runs on-device on edge hardware — no cloud dependency. Works even offline.</div></div></li>
      </ul>
    </div>
  </div>
</section>

<!-- HOW IT WORKS -->
<section id="how-it-works">
  <div class="container" style="text-align:center;">
    <div class="section-label">// process</div>
    <h2 class="section-title">How SmartPark Works</h2>
    <p class="section-sub" style="margin:0 auto;">From vehicle entry to payment — completely automated.</p>
    <div class="hiw-steps">
      <div class="step-card reveal"><div class="step-num">01</div><div class="step-title">Vehicle Arrives</div><div class="step-desc">Camera reads the number plate in real time. System checks registration and available slots.</div></div>
      <div class="step-card reveal"><div class="step-num">02</div><div class="step-title">Slot Assigned</div><div class="step-desc">AI assigns the optimal slot based on proximity, EV preference, and pre-booking.</div></div>
      <div class="step-card reveal"><div class="step-num">03</div><div class="step-title">Monitoring Active</div><div class="step-desc">Overhead sensors continuously track occupancy. Alerts fire for over-stay or unauthorized use.</div></div>
      <div class="step-card reveal"><div class="step-num">04</div><div class="step-title">Auto Checkout</div><div class="step-desc">On exit, billing calculated and payment processed via FASTag/UPI automatically.</div></div>
    </div>
  </div>
</section>

<!-- PRICING -->
<section id="pricing" style="background:var(--bg2)">
  <div class="container">
    <div class="section-label">// plans</div>
    <h2 class="section-title">Simple, Scalable Pricing</h2>
    <p class="section-sub">All plans include AI, mobile app, and real-time dashboard.</p>
    <div class="pricing-grid">
      <div class="price-card reveal">
        <div class="price-tier">Starter</div>
        <div><span class="price-amount">₹4,999</span><span class="price-period">/month</span></div>
        <p style="font-size:0.8rem;color:var(--text-muted);margin-top:0.75rem;">Up to 100 slots. Perfect for malls and offices.</p>
        <ul class="price-features">
          <li><span class="check">✓</span> Up to 100 parking slots</li>
          <li><span class="check">✓</span> Real-time occupancy map</li>
          <li><span class="check">✓</span> Basic ALPR</li>
          <li><span class="check">✓</span> UPI + QR payments</li>
          <li><span class="check">✓</span> Mobile app (iOS + Android)</li>
          <li style="opacity:0.4"><span>✗</span> Predictive analytics</li>
          <li style="opacity:0.4"><span>✗</span> Edge AI hardware</li>
        </ul>
        <button class="btn-outline" style="display:block;width:100%;text-align:center;margin-top:1.5rem;" onclick="requireAuth('booking')">Get Started</button>
      </div>
      <div class="price-card featured reveal">
        <div class="popular-badge">Most Popular</div>
        <div class="price-tier">Professional</div>
        <div><span class="price-amount">₹12,999</span><span class="price-period">/month</span></div>
        <p style="font-size:0.8rem;color:var(--text-muted);margin-top:0.75rem;">Up to 500 slots. Full AI suite included.</p>
        <ul class="price-features">
          <li><span class="check">✓</span> Up to 500 parking slots</li>
          <li><span class="check">✓</span> Full computer vision suite</li>
          <li><span class="check">✓</span> Advanced ALPR + VIP</li>
          <li><span class="check">✓</span> All payment methods</li>
          <li><span class="check">✓</span> Predictive analytics</li>
          <li><span class="check">✓</span> Dynamic pricing AI</li>
          <li style="opacity:0.4"><span>✗</span> Edge AI hardware</li>
        </ul>
        <button class="btn-primary" style="display:block;width:100%;text-align:center;margin-top:1.5rem;" onclick="requireAuth('booking')">Get Started</button>
      </div>
      <div class="price-card reveal">
        <div class="price-tier">Enterprise</div>
        <div><span class="price-amount">Custom</span></div>
        <p style="font-size:0.8rem;color:var(--text-muted);margin-top:0.75rem;">Unlimited slots. Full deployment + hardware.</p>
        <ul class="price-features">
          <li><span class="check">✓</span> Unlimited slots</li>
          <li><span class="check">✓</span> Edge AI hardware included</li>
          <li><span class="check">✓</span> Multi-facility management</li>
          <li><span class="check">✓</span> Custom integrations (ERP/CMS)</li>
          <li><span class="check">✓</span> 24/7 dedicated support</li>
          <li><span class="check">✓</span> White-label mobile app</li>
          <li><span class="check">✓</span> SLA 99.99% uptime</li>
        </ul>
        <button class="btn-outline" style="display:block;width:100%;text-align:center;margin-top:1.5rem;" onclick="requireAuth('booking')">Contact Sales</button>
      </div>
    </div>
  </div>
</section>

<!-- TESTIMONIALS -->
<section style="background:var(--bg3);">
  <div class="container">
    <div class="section-label">// reviews</div>
    <h2 class="section-title">Trusted by Facility Managers</h2>
    <div class="testimonials-grid">
      <div class="testi-card reveal"><div class="stars">★★★★★</div><p class="testi-text">"SmartPark reduced our parking disputes by 85%. The ALPR is incredibly accurate and the dashboard is intuitive."</p><div class="testi-author"><div class="author-avatar">RK</div><div><div class="author-name">Rajesh Kumar</div><div class="author-role">Facility Manager, Phoenix Mall Chennai</div></div></div></div>
      <div class="testi-card reveal"><div class="stars">★★★★★</div><p class="testi-text">"Revenue went up 22% in the first month after deployment. The dynamic pricing feature alone paid for the system."</p><div class="testi-author"><div class="author-avatar">AP</div><div><div class="author-name">Anjali Priya</div><div class="author-role">Operations Head, IT Park Hyderabad</div></div></div></div>
      <div class="testi-card reveal"><div class="stars">★★★★★</div><p class="testi-text">"The edge AI works even without internet. That was a non-negotiable for us. SmartPark delivered on every promise."</p><div class="testi-author"><div class="author-avatar">SM</div><div><div class="author-name">Sarvesh Menon</div><div class="author-role">CTO, Smart City Corp Bengaluru</div></div></div></div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section" id="contact">
  <div style="position:relative;z-index:1;">
    <div class="section-label">// get started</div>
    <h2>Ready to Transform Your<br><span class="gradient-text">Parking Facility?</span></h2>
    <p style="color:var(--text-muted);margin-bottom:2.5rem;font-size:1.1rem;">Join 200+ facilities already using SmartPark AI. Setup takes under 48 hours.</p>
    <div style="display:flex;gap:1rem;justify-content:center;flex-wrap:wrap;">
      <button class="btn-primary" onclick="requireAuth('booking')">Book a Slot →</button>
      <button class="btn-outline" onclick="showPage('auth')">Create Account</button>
    </div>
    <p style="margin-top:2rem;font-size:0.75rem;color:var(--text-dim);font-family:'JetBrains Mono',monospace;">Built by Surjith Raj S · Smart Parking Management System · B.Tech CSE Final Year Project</p>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <div class="logo" style="font-size:1.1rem;"><div class="logo-icon" style="width:24px;height:24px;font-size:12px;">🅿</div>SmartPark AI</div>
    <div class="footer-links">
      <a onclick="document.getElementById('features').scrollIntoView({behavior:'smooth'})">Features</a>
      <a onclick="document.getElementById('pricing').scrollIntoView({behavior:'smooth'})">Pricing</a>
      <a onclick="document.getElementById('how-it-works').scrollIntoView({behavior:'smooth'})">How It Works</a>
      <a onclick="showPage('auth')">Login</a>
    </div>
    <div class="footer-copy">© 2025 SmartPark AI. All rights reserved.</div>
  </div>
</footer>
</div><!-- /mainPage -->


<!-- ══════════════════════════════════════
     AUTH PAGE (LOGIN / REGISTER)
══════════════════════════════════════ -->
<div class="page-overlay" id="authPage">
  <div class="auth-page">
    <!-- Visual Side -->
    <div class="auth-visual">
      <div class="auth-visual-bg"></div>
      <div class="auth-visual-grid"></div>
      <div class="auth-car-showcase" style="position:relative;z-index:1;">
        <div class="auth-brand">
          <div class="logo-icon" style="width:36px;height:36px;font-size:18px;">🅿</div>
          SmartPark AI
        </div>
        <div class="auth-tagline">The intelligent parking platform for modern cities.</div>

        <!-- Large SVG Car -->
        <svg viewBox="0 0 400 220" xmlns="http://www.w3.org/2000/svg" style="width:100%;filter:drop-shadow(0 15px 30px rgba(0,245,212,0.15));margin-bottom:1.5rem;">
          <defs>
            <linearGradient id="bg2" x1="0%" y1="0%" x2="100%" y2="100%"><stop offset="0%" style="stop-color:#1a4a7c"/><stop offset="100%" style="stop-color:#0d2b4e"/></linearGradient>
            <linearGradient id="roof2" x1="0%" y1="0%" x2="0%" y2="100%"><stop offset="0%" style="stop-color:#2060a0"/><stop offset="100%" style="stop-color:#1a4a7c"/></linearGradient>
          </defs>
          <ellipse cx="200" cy="200" rx="145" ry="10" fill="rgba(0,0,0,0.4)"/>
          <!-- Body -->
          <rect x="30" y="140" width="340" height="58" rx="10" fill="url(#bg2)" stroke="rgba(0,245,212,0.25)" stroke-width="1"/>
          <!-- Roof -->
          <path d="M110 140 Q125 100 155 95 L245 95 Q275 100 290 140Z" fill="url(#roof2)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
          <!-- Windows -->
          <path d="M120 138 Q130 110 155 103 L192 103 L192 138Z" fill="rgba(0,245,212,0.07)" stroke="rgba(0,245,212,0.3)" stroke-width="1.5"/>
          <path d="M197 138 L197 103 L245 103 Q272 110 278 138Z" fill="rgba(0,245,212,0.07)" stroke="rgba(0,245,212,0.3)" stroke-width="1.5"/>
          <line x1="196" y1="103" x2="196" y2="138" stroke="rgba(0,245,212,0.35)" stroke-width="2"/>
          <!-- Bumper -->
          <rect x="30" y="178" width="340" height="8" rx="4" fill="rgba(0,245,212,0.07)"/>
          <!-- Lights front -->
          <ellipse cx="62" cy="162" rx="20" ry="10" fill="#07111f" stroke="rgba(0,245,212,0.5)" stroke-width="1.5"/>
          <ellipse cx="62" cy="162" rx="12" ry="6" fill="rgba(0,245,212,0.12)"/>
          <ellipse cx="62" cy="162" rx="5" ry="2.5" fill="rgba(0,245,212,0.7)"/>
          <!-- Beam -->
          <path d="M42 158 L5 148 L5 170 L42 165Z" fill="rgba(0,245,212,0.04)"/>
          <!-- Lights rear -->
          <ellipse cx="338" cy="162" rx="20" ry="10" fill="#07111f" stroke="rgba(239,68,68,0.5)" stroke-width="1.5"/>
          <ellipse cx="338" cy="162" rx="12" ry="6" fill="rgba(239,68,68,0.12)"/>
          <ellipse cx="338" cy="162" rx="5" ry="2.5" fill="rgba(239,68,68,0.7)"/>
          <!-- Door lines -->
          <line x1="148" y1="142" x2="148" y2="195" stroke="rgba(255,255,255,0.07)" stroke-width="1.5"/>
          <line x1="252" y1="142" x2="252" y2="195" stroke="rgba(255,255,255,0.07)" stroke-width="1.5"/>
          <!-- Handles -->
          <rect x="178" y="165" width="18" height="4" rx="2" fill="rgba(0,245,212,0.35)"/>
          <rect x="215" y="165" width="18" height="4" rx="2" fill="rgba(0,245,212,0.35)"/>
          <!-- Wheels -->
          <circle cx="110" cy="197" r="24" fill="#06101a" stroke="rgba(0,245,212,0.35)" stroke-width="2"/>
          <circle cx="110" cy="197" r="17" fill="#0b1827"/>
          <circle cx="110" cy="197" r="10" fill="#07121f" stroke="rgba(0,245,212,0.4)" stroke-width="1"/>
          <circle cx="110" cy="197" r="3.5" fill="rgba(0,245,212,0.6)"/>
          <line x1="110" y1="180" x2="110" y2="214" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="93" y1="197" x2="127" y2="197" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="98" y1="185" x2="122" y2="209" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>
          <line x1="98" y1="209" x2="122" y2="185" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>

          <circle cx="290" cy="197" r="24" fill="#06101a" stroke="rgba(0,245,212,0.35)" stroke-width="2"/>
          <circle cx="290" cy="197" r="17" fill="#0b1827"/>
          <circle cx="290" cy="197" r="10" fill="#07121f" stroke="rgba(0,245,212,0.4)" stroke-width="1"/>
          <circle cx="290" cy="197" r="3.5" fill="rgba(0,245,212,0.6)"/>
          <line x1="290" y1="180" x2="290" y2="214" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="273" y1="197" x2="307" y2="197" stroke="rgba(0,245,212,0.25)" stroke-width="1.5"/>
          <line x1="278" y1="185" x2="302" y2="209" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>
          <line x1="278" y1="209" x2="302" y2="185" stroke="rgba(0,245,212,0.15)" stroke-width="1.5"/>
          <!-- Plate -->
          <rect x="160" y="182" width="80" height="16" rx="2" fill="#e8e8e8"/>
          <text x="200" y="194" text-anchor="middle" font-family="monospace" font-size="8.5" font-weight="bold" fill="#1a1a2e">TN09 AB 1234</text>
          <!-- Roof antenna -->
          <line x1="200" y1="95" x2="200" y2="78" stroke="rgba(0,245,212,0.4)" stroke-width="1.5"/>
          <circle cx="200" cy="76" r="3" fill="rgba(0,245,212,0.5)"/>
        </svg>

        <!-- Floating electric SUV -->
        <svg viewBox="0 0 400 130" xmlns="http://www.w3.org/2000/svg" style="width:85%;display:block;margin:0 auto;filter:drop-shadow(0 10px 20px rgba(124,58,237,0.2));">
          <defs>
            <linearGradient id="suv1" x1="0%" y1="0%" x2="100%" y2="100%"><stop offset="0%" style="stop-color:#2d1b69"/><stop offset="100%" style="stop-color:#1a0e3d"/></linearGradient>
          </defs>
          <ellipse cx="200" cy="120" rx="130" ry="8" fill="rgba(0,0,0,0.3)"/>
          <!-- SUV Body (taller) -->
          <rect x="35" y="65" width="330" height="55" rx="6" fill="url(#suv1)" stroke="rgba(124,58,237,0.3)" stroke-width="1"/>
          <!-- SUV Roof (boxy) -->
          <rect x="75" y="30" width="250" height="38" rx="6" fill="#241260" stroke="rgba(124,58,237,0.2)" stroke-width="1"/>
          <!-- Windows -->
          <rect x="83" y="34" width="70" height="30" rx="3" fill="rgba(124,58,237,0.08)" stroke="rgba(124,58,237,0.3)" stroke-width="1.2"/>
          <rect x="162" y="34" width="80" height="30" rx="3" fill="rgba(124,58,237,0.08)" stroke="rgba(124,58,237,0.3)" stroke-width="1.2"/>
          <rect x="251" y="34" width="66" height="30" rx="3" fill="rgba(124,58,237,0.08)" stroke="rgba(124,58,237,0.3)" stroke-width="1.2"/>
          <!-- Lights -->
          <rect x="38" y="75" width="25" height="10" rx="2" fill="#0a0519" stroke="rgba(124,58,237,0.5)" stroke-width="1"/>
          <rect x="40" y="77" width="20" height="6" rx="1" fill="rgba(124,58,237,0.4)"/>
          <rect x="337" y="75" width="25" height="10" rx="2" fill="#0a0519" stroke="rgba(239,68,68,0.5)" stroke-width="1"/>
          <rect x="338" y="77" width="20" height="6" rx="1" fill="rgba(239,68,68,0.5)"/>
          <!-- Wheels -->
          <circle cx="105" cy="118" r="20" fill="#070e1a" stroke="rgba(124,58,237,0.4)" stroke-width="2"/>
          <circle cx="105" cy="118" r="13" fill="#0c1428"/>
          <circle cx="105" cy="118" r="5" fill="rgba(124,58,237,0.6)"/>
          <circle cx="295" cy="118" r="20" fill="#070e1a" stroke="rgba(124,58,237,0.4)" stroke-width="2"/>
          <circle cx="295" cy="118" r="13" fill="#0c1428"/>
          <circle cx="295" cy="118" r="5" fill="rgba(124,58,237,0.6)"/>
          <!-- EV badge -->
          <rect x="172" y="72" width="56" height="18" rx="3" fill="rgba(124,58,237,0.2)" stroke="rgba(124,58,237,0.5)" stroke-width="1"/>
          <text x="200" y="84" text-anchor="middle" font-family="monospace" font-size="9" fill="#a78bfa" font-weight="bold">⚡ ELECTRIC</text>
        </svg>

        <div class="auth-stats">
          <div class="auth-stat"><div class="auth-stat-num">50K+</div><div class="auth-stat-label">Slots Managed</div></div>
          <div class="auth-stat"><div class="auth-stat-num">42</div><div class="auth-stat-label">Cities</div></div>
          <div class="auth-stat"><div class="auth-stat-num">99.9%</div><div class="auth-stat-label">Uptime</div></div>
          <div class="auth-stat"><div class="auth-stat-num">₹18K+</div><div class="auth-stat-label">Daily Revenue</div></div>
        </div>
      </div>
    </div>

    <!-- Form Side -->
    <div class="auth-form-panel">
      <div class="auth-form-box">
        <span class="back-link" onclick="showPage('main')">← Back to Home</span>
        <div class="auth-title">Welcome Back</div>
        <div class="auth-sub">Sign in to manage your parking slots.</div>

        <div class="auth-tabs">
          <div class="auth-tab active" id="loginTab" onclick="switchTab('login')">Sign In</div>
          <div class="auth-tab" id="registerTab" onclick="switchTab('register')">Register</div>
        </div>

        <!-- LOGIN FORM -->
        <div id="loginForm">
          <div class="form-group">
            <label class="form-label">Email Address</label>
            <input type="email" class="form-input" id="loginEmail" placeholder="you@example.com" autocomplete="email"/>
            <div class="form-error" id="loginEmailErr">Please enter a valid email.</div>
          </div>
          <div class="form-group">
            <label class="form-label">Password</label>
            <input type="password" class="form-input" id="loginPass" placeholder="••••••••" autocomplete="current-password"/>
            <div class="form-error" id="loginPassErr">Invalid credentials.</div>
          </div>
          <div style="display:flex;justify-content:flex-end;margin-bottom:1rem;"><span style="font-size:0.8rem;color:var(--cyan);cursor:pointer;">Forgot password?</span></div>
          <button class="auth-btn" onclick="handleLogin()">Sign In →</button>
          <div class="auth-divider">or continue with</div>
          <div class="social-btns">
            <button class="social-btn" onclick="socialLogin('Google')">🌐 Google</button>
            <button class="social-btn" onclick="socialLogin('GitHub')">🐙 GitHub</button>
          </div>
          <p style="text-align:center;margin-top:1.5rem;font-size:0.82rem;color:var(--text-muted);">No account? <span style="color:var(--cyan);cursor:pointer;" onclick="switchTab('register')">Register free →</span></p>
        </div>

        <!-- REGISTER FORM -->
        <div id="registerForm" style="display:none;">
          <div class="form-row">
            <div class="form-group">
              <label class="form-label">First Name</label>
              <input type="text" class="form-input" id="regFirst" placeholder="Surjith"/>
            </div>
            <div class="form-group">
              <label class="form-label">Last Name</label>
              <input type="text" class="form-input" id="regLast" placeholder="Raj"/>
            </div>
          </div>
          <div class="form-group">
            <label class="form-label">Email Address</label>
            <input type="email" class="form-input" id="regEmail" placeholder="you@example.com"/>
            <div class="form-error" id="regEmailErr">Email already registered.</div>
          </div>
          <div class="form-group">
            <label class="form-label">Phone Number</label>
            <input type="tel" class="form-input" id="regPhone" placeholder="+91 98765 43210"/>
          </div>
          <div class="form-group">
            <label class="form-label">Vehicle Number</label>
            <input type="text" class="form-input" id="regVehicle" placeholder="TN09 AB 1234" style="text-transform:uppercase;"/>
          </div>
          <div class="form-group">
            <label class="form-label">Password</label>
            <input type="password" class="form-input" id="regPass" placeholder="Min 8 characters"/>
            <div class="form-error" id="regPassErr">Password must be at least 8 characters.</div>
          </div>
          <button class="auth-btn" onclick="handleRegister()">Create Account →</button>
          <p style="text-align:center;margin-top:1.5rem;font-size:0.82rem;color:var(--text-muted);">Already have an account? <span style="color:var(--cyan);cursor:pointer;" onclick="switchTab('login')">Sign in →</span></p>
        </div>
      </div>
    </div>
  </div>
</div>


<!-- ══════════════════════════════════════
     BOOKING PAGE
══════════════════════════════════════ -->
<div class="page-overlay" id="bookingPage">
  <div class="booking-page">
    <div style="max-width:1100px;margin:0 auto;">
      <span class="back-link" onclick="showPage('main')">← Back to Home</span>
      <div class="section-label">// reserve a slot</div>
      <h1 class="section-title">Book Your Parking Slot</h1>
      <p style="color:var(--text-muted);">Select a free slot below, choose your duration, and proceed to payment.</p>

      <div class="booking-grid">
        <!-- Map -->
        <div>
          <div class="booking-card">
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:0.5rem;">
              <div>
                <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:1.1rem;">Level B2 — Main Facility</div>
                <div style="font-size:0.75rem;color:var(--text-muted);font-family:'JetBrains Mono',monospace;">Click a GREEN slot to select</div>
              </div>
              <div style="display:flex;gap:0.5rem;align-items:center;font-size:0.72rem;color:var(--green);">
                <span class="live-dot"></span> LIVE
              </div>
            </div>
            <div class="booking-slot-grid" id="bookingGrid"></div>
            <div class="lot-legend" style="margin-top:1rem;">
              <div class="legend-item"><div class="legend-dot" style="background:rgba(34,197,94,0.6)"></div>Free (Click to select)</div>
              <div class="legend-item"><div class="legend-dot" style="background:rgba(239,68,68,0.6)"></div>Occupied</div>
              <div class="legend-item"><div class="legend-dot" style="background:rgba(0,245,212,0.6)"></div>EV Charging</div>
              <div class="legend-item"><div class="legend-dot" style="background:rgba(139,92,246,0.6)"></div>Disabled</div>
            </div>
          </div>

          <!-- Duration -->
          <div class="booking-card" style="margin-top:1.5rem;">
            <div style="font-family:'Syne',sans-serif;font-weight:700;margin-bottom:1rem;">Select Duration</div>
            <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:0.75rem;" id="durationPicker">
              <div class="upi-app" onclick="selectDuration(1,this)" data-hours="1"><div class="upi-app-icon">1h</div><div class="upi-app-name">₹50</div></div>
              <div class="upi-app" onclick="selectDuration(2,this)" data-hours="2"><div class="upi-app-icon">2h</div><div class="upi-app-name">₹95</div></div>
              <div class="upi-app selected" onclick="selectDuration(3,this)" data-hours="3"><div class="upi-app-icon">3h</div><div class="upi-app-name">₹135</div></div>
              <div class="upi-app" onclick="selectDuration(6,this)" data-hours="6"><div class="upi-app-icon">6h</div><div class="upi-app-name">₹240</div></div>
            </div>
            <div style="margin-top:1rem;">
              <label class="form-label">Vehicle Number</label>
              <input type="text" class="form-input" id="bookVehicle" placeholder="TN09 AB 1234" style="text-transform:uppercase;margin-top:0.4rem;"/>
            </div>
          </div>
        </div>

        <!-- Summary -->
        <div class="booking-summary">
          <div class="booking-card">
            <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:1.1rem;margin-bottom:1.5rem;">Booking Summary</div>

            <!-- Mini car SVG -->
            <div style="text-align:center;margin-bottom:1.5rem;">
              <svg viewBox="0 0 200 90" xmlns="http://www.w3.org/2000/svg" style="width:180px;">
                <ellipse cx="100" cy="82" rx="70" ry="5" fill="rgba(0,0,0,0.3)"/>
                <rect x="20" y="48" width="160" height="35" rx="6" fill="#0d2137" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
                <path d="M55 48 Q63 22 78 18 L122 18 Q137 22 145 48Z" fill="#112d4a" stroke="rgba(0,245,212,0.15)" stroke-width="1"/>
                <path d="M62 47 Q68 28 80 23 L98 23 L98 47Z" fill="rgba(0,245,212,0.06)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
                <path d="M102 47 L102 23 L120 23 Q132 28 138 47Z" fill="rgba(0,245,212,0.06)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
                <ellipse cx="42" cy="62" rx="12" ry="6" fill="#07111f" stroke="rgba(0,245,212,0.4)" stroke-width="1"/>
                <ellipse cx="42" cy="62" rx="5" ry="2.5" fill="rgba(0,245,212,0.5)"/>
                <circle cx="57" cy="81" r="13" fill="#06101a" stroke="rgba(0,245,212,0.4)" stroke-width="1.5"/>
                <circle cx="57" cy="81" r="8" fill="#0b1827"/>
                <circle cx="57" cy="81" r="3" fill="rgba(0,245,212,0.5)"/>
                <circle cx="143" cy="81" r="13" fill="#06101a" stroke="rgba(0,245,212,0.4)" stroke-width="1.5"/>
                <circle cx="143" cy="81" r="8" fill="#0b1827"/>
                <circle cx="143" cy="81" r="3" fill="rgba(0,245,212,0.5)"/>
                <rect x="80" y="64" width="40" height="11" rx="2" fill="#e8e8e8"/>
                <text x="100" y="73" text-anchor="middle" font-family="monospace" font-size="6" font-weight="bold" fill="#1a1a2e" id="bookPlatePreview">TN09 AB 1234</text>
              </svg>
            </div>

            <div>
              <div class="summary-row"><span style="color:var(--text-muted);">Selected Slot</span><span id="sumSlot" style="color:var(--cyan);font-family:'JetBrains Mono',monospace;">— None —</span></div>
              <div class="summary-row"><span style="color:var(--text-muted);">Duration</span><span id="sumDur">3 Hours</span></div>
              <div class="summary-row"><span style="color:var(--text-muted);">Parking Fee</span><span id="sumBase">₹135</span></div>
              <div class="summary-row"><span style="color:var(--text-muted);">GST (18%)</span><span id="sumGst">₹24</span></div>
              <div class="summary-row total"><span>Total Amount</span><span id="sumTotal">₹159</span></div>
            </div>
            <button class="btn-primary" style="width:100%;margin-top:1.5rem;text-align:center;" onclick="proceedToPayment()">Proceed to Payment →</button>
            <p style="font-size:0.72rem;color:var(--text-muted);text-align:center;margin-top:0.75rem;">🔒 Secured by SmartPark Payment Gateway</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>


<!-- ══════════════════════════════════════
     PAYMENT PAGE
══════════════════════════════════════ -->
<div class="page-overlay" id="paymentPage">
  <div class="payment-page">
    <div class="payment-container">
      <span class="back-link" onclick="showPage('booking')">← Back to Booking</span>
      <div class="section-label">// secure checkout</div>
      <h1 class="section-title">Complete Payment</h1>

      <div class="payment-grid">
        <!-- Methods -->
        <div class="payment-methods">
          <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:1.1rem;margin-bottom:1.5rem;">Choose Payment Method</div>
          <div class="method-tabs">
            <button class="method-tab active" onclick="switchMethod('upi',this)">📱 UPI</button>
            <button class="method-tab" onclick="switchMethod('card',this)">💳 Card</button>
            <button class="method-tab" onclick="switchMethod('wallet',this)">👛 Wallet</button>
            <button class="method-tab" onclick="switchMethod('netbanking',this)">🏦 Net Banking</button>
            <button class="method-tab" onclick="switchMethod('fastag',this)">🚗 FASTag</button>
          </div>

          <!-- UPI — real deep links -->
          <div class="method-content active" id="method-upi">
            <div style="font-size:0.85rem;color:var(--text-muted);margin-bottom:1rem;">Select your UPI app to open it directly</div>
            <div class="upi-apps">
              <div class="upi-app selected" onclick="selectUpiApp(this)" data-app="gpay" data-scheme="tez://upi/pay"><div class="upi-app-icon" style="font-size:1.6rem;">
                <svg viewBox="0 0 40 40" width="36" height="36"><rect width="40" height="40" rx="8" fill="#fff"/><text y="28" x="4" font-size="22">💚</text></svg>
              </div><div class="upi-app-name">GPay</div></div>
              <div class="upi-app" onclick="selectUpiApp(this)" data-app="phonepe" data-scheme="phonepe://pay"><div class="upi-app-icon" style="font-size:1.6rem;">
                <svg viewBox="0 0 40 40" width="36" height="36"><rect width="40" height="40" rx="8" fill="#5f259f"/><text y="28" x="6" font-size="22">💜</text></svg>
              </div><div class="upi-app-name">PhonePe</div></div>
              <div class="upi-app" onclick="selectUpiApp(this)" data-app="paytm" data-scheme="paytmmp://pay"><div class="upi-app-icon" style="font-size:1.6rem;">
                <svg viewBox="0 0 40 40" width="36" height="36"><rect width="40" height="40" rx="8" fill="#002970"/><text y="28" x="6" font-size="22">🔵</text></svg>
              </div><div class="upi-app-name">Paytm</div></div>
              <div class="upi-app" onclick="selectUpiApp(this)" data-app="bhim" data-scheme="upi://pay"><div class="upi-app-icon" style="font-size:1.6rem;">
                <svg viewBox="0 0 40 40" width="36" height="36"><rect width="40" height="40" rx="8" fill="#f26522"/><text y="28" x="6" font-size="22">🟠</text></svg>
              </div><div class="upi-app-name">BHIM</div></div>
            </div>
            <div class="form-group" style="margin-top:1rem;">
              <label class="form-label">Or enter UPI ID manually</label>
              <input type="text" class="form-input" id="upiId" placeholder="yourname@okaxis"/>
            </div>
            <!-- Open UPI App button -->
            <button class="btn-primary" style="width:100%;margin-top:1rem;" id="openUpiAppBtn" onclick="openUpiApp()">Open GPay App to Pay →</button>
            <div style="background:var(--bg3);border:1px solid var(--border);border-radius:10px;padding:1rem;text-align:center;margin-top:1rem;">
              <canvas id="upiQrCanvas" width="1" height="1" style="display:none;"></canvas>
              <div style="position:relative;display:inline-block;margin:0 auto 0.5rem;">
                <img src="data:image/jpeg;base64,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AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOMMMMMMMMJAABJKONAAAAAEMMAEIAEMMMAAAMMMMMMMMMAAAAAAAAAAAAAAAAAAAAAAAAAAAKEDDDDDDGFAAFFLDHAAAADDDGBBHDAABDAAAADDDDDDDKAAAAAAAAAAAAAAAAAAAAAAAAAAAKFADDDDCAFABGBBJCAABGAAAFAAADCAFAAAAAAADDDDAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKAAAAAFAAAFKKFAAAAAAAFAAAAAAFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKAAAAAFAAFIAKFAAEMMAAIMAEMIAEMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKAAAAAFAAFADIEADDDDDDAABDACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAJDDDCAFAAFBDIAABDDDDHAABDADIEDCAAAAADDDCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAAAAAAAFAAABCKBAAAAAAAFIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKEMMMMMMIFAABEECBMAEMAEJFAAEMIMNAOIAAMMMMMMMAAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAAAAAAAFAAFAAIEACAAAAFEAAAAACFAKAAAAAAAAAAACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFAAAAAAAAAAFAAAAAAAFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACFACAAAAABAIBFKABAAIAAAAAAIAAAAAAIAEAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKBACGIAAEIAAAMACBAAAAAAAAAKBMIAAAAAAAGMMMCAACKAAAAAAAAAAAAAAAAAAAAAAAAAAKBAAKFAAAAAKAAAKAAAABAAAAAAAACCBAABAACBAAKAAACAAAAAAAAAAAAAAAAAAAAAAAAAAKFAAAFAAAFAAAAAAAAAAAAAAAAAAAAAFAAAAAAAAAKAAKAAAAAAAAAAAAAAAAAAAAAAAAAAAKFACKFAAAAAAAAICFAAFBAIABAAAAMAFAKAEAIAAIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKKFAAAAAAAACIEAAGEAAAEAAAAAAFAKABAABBAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFDLDAAABDDAAEJAFAABDADBDDABDCDHDDCADDDDDABDDAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAKFAAAAAAAAAAKFAAAEIMIEKAAAAKFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMMMIMMAAEJAAFIAEIAEGAAAAAABAAAAFMKAEMEMMEAAAGAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACDDDCFDAFACAAABIAAGYggAAAAAFAIBCAAAAABDAKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFAACBBAKEEKAFAKAByZwXdAEAAAFAAFAAABAAIBAKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFAAMMAAKAIIKFAKAKdGFvbAEAMAFAKAAAEMEIEMAKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMAAAEAAABEAAEAKAJBhsduABAAAFAIBEAAAAAAAAAIAAAAAAAAAAAAAAAAAAAAAAAAAACAAACAAAAAAAEAAAAAJAFPbI0wwDAAAFAAFBACBAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAAKAAAAAAAFAAKFABCAEMoQABIAAKAAKAAAAAAAAAAKAAAAAAAAAAAAAAAAAAAAAAAAAAAKBACKFMIEEACFNKKEMANCAAAAAIEAAEJAKEAMAAEAKAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAFOAPIFAPKAAIBPIFPPPPMICBCAAAAAAFAKBAPABBKHLAHAAAAAAAAAAAAAAAAAAAAAAAAAAABDDCBGAABDDCEDDABDCBCDDDDDDCDBDGDJDCDBDDCAAAJCAAAAAAAAAAAAAAAAAAAAAAAAAAKAAKFAAAAAAAAAAKFAAAFAAFAAAAAAAFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKBMIMMMAAAAIBIIIAAIJFAEIAAAAAAAEMIAAAAAAAIEAGIAAAAAAAAAAAAAAAAAAAAAAAAAAKFAADAAAAAAAFAACAAAGFACABCCBAAAAAAABAAAAAAAAIAAAAAAAAAAAAAAAAAAAAAAAAAAAKFACHBAAABAKAACLDDABBACFFCABACKAAKAAAAABAABACAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAGCBMAAIAKBMIMMMAAAAAAAAAAAIMMMAAEMMMMIAEAIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAEJAACFAEBAAMJBEMMAEKBAAEAAAAEIAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAADDDDDDDDDDADHCAKFABGBDCEGBDDADKFAAAADCAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAAAAAAAFAAFAAKFAAAFAAAAAAAAAAFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFMMMMMMIFAENMMOJMEMMMIBMAMMAEMIAAAAMAAAAGMMMIAAAAAAAAAAAAAAAAAAAAAAAAAAKFAJDDDAAFAAFCCEIAEEAAEMDDMIAAABMCFDDDHAAMMMMIAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKAAAAAFAAAELCBDAABDDDDDBDDCAFDKAAACAAADDDDAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKAAAAAFAAAAAKFAAAAAAFAAAAAAAFAAAAAAAAAKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKFAKAAAAAFAABAAAFAANFAIAFAIAAIKFAABAAAEEIAAAAIAAAAAAAAAAAAAAAAAAAAAAAAAAKFAIAAAAAFAAEAAAFAAGFAAAFAAAACKEAAFAAAABACBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKEAAAAAAAFAAFDAKAAAAFAAAFBCBADCEAAABAAAAABCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAAAAAAAFAAFAAKFAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAAAAAAAAAAAAAAAAAAAAAAAAAAEMMMMMMMMIAEIAMIAMMMMMAEIAAEMIEMMMMMAAEMMIAMIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAACCBCCAAAAAAAABAABAADAAAABAAABCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKAOVaQebICPFBBMeJCHEDCLAPDAGKPQGJJAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMDLAIBIEAMNUUHAMCKOAKJAGHIMFJASDLCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABwAAAAAAAAAAAAAAAAAAAACwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABwAAAAAAAAAAAAAAAAAAAAAAMUccccccccccccccccccccccccccccccccccccccccccccccQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAPZRQCDAASQCDDCCCADTZyDACTBNRRdSQDBABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIa3VSw4JfSadz2QZf92SSca36QXXRwKTZ819QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIAEMIEAEMANEEVYAAEAIIEIMNYEAIAAEMMUUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/EAAL/2gAMAwEAAgADAAAAEP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AO+93qt//wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wCuseIhr/8Asew//wD13P8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/ACefpVY/f+tjNOANFP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/ADCsmab9v4zDlqAFFv8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AMY86h1dv+hvEriFFv8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/APQUlWV//wD/AP8A/wD+DT//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AMOdd/8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wDrTzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzb/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A9fPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPOd//wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/APV88888888888888888888888888888888888888888888888888t//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888888888888888888888888888888888888888888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88EMMMMMMMMc88oY0kY88888kMcUEI8sMMMc8oMMMMMMMMME888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo888888s888ooUcs88848U84cUs8c8o8U8o8U8888888U888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo888888o880o4wU888448U8oUU8w088888o8U888848UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888Uo8888oUUo88A88U8oUU88888888o8U8888o8UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888Uo8888ccUo88MMMc4MEcsMc8sMc8o8U8888o8UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888Uo88884wYk84wwwwk0cw0w84w888o8U8888o8UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8QwwwUo88848wU484wwwww48Qk84cow88o8U4wwwk8UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo888888o8888IUkI888888soEU88888888o8U888888UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Us888888s8888M8kI8cg4808oUU8800s8coo88888888cU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88c88888888888888Uo80Ao8U8sUc88YU880oo888888888c888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888888888888888888Ao8U88888888888888888888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88kY8kMMMMMM8c4Y08oMMMo8U8MMU88UU8Mc84MMMMMMY888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U8M0EI884MM88ccMc888o8U888kIMEc88kYo8kMMMco8cI888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U4wwU8888ww8884c8888swU888wgww84wwko8Yw888owYU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo888888888888888888888888U88UU8888o88o88888U8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8Ek8888c8888s0oM8Mc8EMY0MIMcc88MMMMUo8M84Mc8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8UU8www888880Yg8ww88U8sQwg88888ww08Uo4wwk888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Us8Yw888sww08sU0o88M08Q4888480ww88848Ywk884880888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U88U88888888888Uo888wM8s8cU888Uo88oo8U888U888U888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88c88cc888oc8888cMc8s0888888c888cs8cos8EMYcc88Uc888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA8888884888s4808w8888w888mg508U888880o48U8o8088U8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA8888888884w488okU0o8c8pFlFQp848w0888488Ywo8408U8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888888kYM888cs0o808hhxPuf8s8kU88sc88scsMkc8U8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888s88c888coccsM808xKj2H08s8cc4M8ssMUs88888Mk888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA884www088wwwwwg88848Q8fF98G+8sww0s8ww48U8wwww80c888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U888U88U8888888Uo8s08vdf888c88U8888888888888U8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U4M0UsMcsM8sYoUUsMc4w88888k8M8Mc88oYM08os8oME0888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888s8UU8888k8Yw88c888sYMwkwQww888888ok8w0ow0ow80888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA880888088Us8804880888Io88o888o808480848U848U88c0888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U88U888U8888888Uo888888888888U8o888o8U888U888U888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo888880888css8888cs880488U88U88888o8U888cs8Uc888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo88488U84884880888888Us40U88U88888o8U888888U0888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo88088U8488448Qww8Ms8c8sc8480U88888888408880U888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8k0sM88c88oMcsMMc88888888880kMMc88MMMMc848UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88c88ccs88888MMM8048s888kMoEMI8M0o88o888s4c8s8UU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88wwwwwwwwww840w8Uo8w84wU4oYwk88Uo88o4w08o8888Qc888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U88888888888o88Uo88888888U88888o88oo8U8o888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88UoMMMMMMI88sMMMEYMsMIMUscckI8MMM88o8M88o8MMMMU888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U8888o888840sM8swo8sM88Mc8884M08888oo8MMMM8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888Uo8888oc8488Ao88swwc880888888888o888880888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888Uo888888Uo88A888888U88U8o88888U8o88888U888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888Uo888sMM8o888M8Ec4UEc880o88s8884MckMMMc888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo8U888co8804ww0o88088U8oUU480Qk884888o00Yw888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88Uo888888s888s88U888848U4oUU488048884888880s888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88U88888888888888Uo88888Uo8U888UU8888888888U88U8888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88c88888888s8c8888s888888888888888888888s88c8888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888888888888888888888888888888888888888888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888888888888888888888888888888888888888888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA8888888808w804008888484w808808w80884888488888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88888888UopVVpFQUswkkkdsc00swgUskcMkoxAAI4888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888884s48csYk0wF5YQcUAkU8Us4MsMM40RAsUk888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA88888888888888888888s88888888888M888888888888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wA888888888888888888888888888888888888888888888888888//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A9Xzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzyn/8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD73fPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPNf/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8Au8888888888888888888888888888888888888888888888e/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD6PMP9+f8ATzfPf3P/AP8A6NOt+88N9/KPHFO+9e+//wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/APk2EICiXcE84628UYaGwUUEWKkU0g+dUEqKCc//AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A9/8A/wD3/wD+/wDfp/sIfvv/AH//AN8++h+++/8A/wD/AOzx/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP8A/wD/AP/EADYRAAICAQIEAwUHAgcAAAAAAAABAhEDBDEQIUFRBRJQBjJCYHETFCAjQIGQIqAHFTBhgKHR/9oACAECAQE/AP47ZT7DbExTaE7+Q5MZNtEckW+UkxITri3RchO+LEK/WXw1jmsfL3epp03OKT5vv24x24Pk7LRXMTdsexfIWxF361qNXHE/KlbJamUvN526aaRgeSEvPypIx6yPLz8mXaI7cfKhN3QveZLY+EjsR9Z1GSOKLk/2H582V1ze74cy32NHqVJLHLfoJUuDTu0W+wkNNOx2z4SOxFes58Mc2Nwl+z7CwQ0+CaS5+V2+5zvYlBytpcjQ44zx5YSVptGn0MMORztvt/t/oOxKl620mmmrRPQ4JbXH6H+W4G7k5S+piw48UahGv+BlotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotFotfqHL1FS/TSfqcX/cLajU/ZvyxVyFq897oxauE+Uv6X8ieHaKWs1UMXw7zfaKPuOjeL7J6eHkqqo9o/Z6OgitVgm3ilOnGW8W+GDxPTatzWDKn5XTWzNJrMmOcYylcH36fIfgehWm0inJfmZak/p0Qke2XiinKGgxv3Gp5fr0R7S+JfddJ9hCX5mZV9I9T2dhll4lCUfdjGXn+jRixvJkjBbt/IennHJgxTjtKCaPFfEsPhmhyajJKKfu403703sjV6uvt9TqMnec5M8Q1mTXavJnl8TqK7LojwbQfc9IlJfmT/qn/AOHhun8sHlkuctvp8h+FeNZNEliyRc8V/vH6H+K+Va72f0Oo0s/Pixaq8veNxpNmXVarLBQyZ8k4rZSk2j2S8FWv1cs+WL+xw/8AcxeFwU7eRuPahJJJLZfImXHDLiyYskVKE4uMovZpj9ifBXl8/wCclfuefkabTafS4YYcGOMMcdor+/GkvU4r9M4+oqP6ikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUikUv4p92Ky0WWWuCZa4Wiy1+G9i+YnwsstceqGdTqLhZaLRa9F5M7o5UdTufDwW7FsR2PiOrHsPlTEPubHY6sXUjsdTufCdmIe6H0PiKF1Huhe8LY6Ie69FpcKRSKXGilwrnwpD7fhrjSKXB9uNL8NISGtil/Cn/8QAMxEAAgICAAIIBAQHAQAAAAAAAREAAgMEBTEGEhMhIkFRYBBAUGEUQnGgFSMyUoCQkaH/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" alt="UPI QR Code - sujir9311-2@okaxis" style="width:190px;height:190px;object-fit:contain;border-radius:12px;display:block;border:1px solid rgba(0,245,212,0.2);"/>
                <div style="position:absolute;bottom:-10px;left:50%;transform:translateX(-50%);background:rgba(0,245,212,0.15);border:1px solid rgba(0,245,212,0.4);border-radius:20px;padding:3px 12px;white-space:nowrap;font-family:'JetBrains Mono',monospace;font-size:0.6rem;color:#00f5d4;letter-spacing:0.05em;">✓ VERIFIED UPI</div>
              </div>
              <div style="font-size:0.78rem;font-weight:700;color:#e2e8f0;font-family:'JetBrains Mono',monospace;margin-top:1rem;">sujir9311-2@okaxis</div>
              <div style="font-size:0.65rem;color:#8898aa;margin-top:0.2rem;">Scan with GPay · PhonePe · Paytm · BHIM</div>
              <div style="font-size:0.72rem;color:#00f5d4;margin-top:0.4rem;font-family:'JetBrains Mono',monospace;" id="upiAmountLabel">Amount: ₹159</div>
            </div>
          </div>

          <!-- CARD -->
          <div class="method-content" id="method-card">
            <div class="card-preview">
              <div class="card-chip"></div>
              <div class="card-number-preview" id="cardNumPreview">•••• •••• •••• ••••</div>
              <div class="card-meta">
                <div><div style="font-size:0.6rem;opacity:0.6;margin-bottom:2px;">CARD HOLDER</div><div id="cardNamePreview">YOUR NAME</div></div>
                <div><div style="font-size:0.6rem;opacity:0.6;margin-bottom:2px;">EXPIRES</div><div id="cardExpPreview">MM/YY</div></div>
                <div style="font-size:1.5rem;">💳</div>
              </div>
            </div>
            <div class="card-form">
              <div class="form-group">
                <label class="form-label">Card Number</label>
                <input type="text" class="form-input" id="cardNum" placeholder="1234 5678 9012 3456" maxlength="19" oninput="formatCard(this)"/>
              </div>
              <div class="form-group">
                <label class="form-label">Cardholder Name</label>
                <input type="text" class="form-input" id="cardName" placeholder="SURJITH RAJ S" style="text-transform:uppercase;" oninput="document.getElementById('cardNamePreview').textContent=this.value.toUpperCase()||'YOUR NAME'"/>
              </div>
              <div class="form-row">
                <div class="form-group">
                  <label class="form-label">Expiry (MM/YY)</label>
                  <input type="text" class="form-input" id="cardExp" placeholder="08/26" maxlength="5" oninput="formatExp(this)"/>
                </div>
                <div class="form-group">
                  <label class="form-label">CVV</label>
                  <input type="password" class="form-input" id="cardCvv" placeholder="•••" maxlength="3"/>
                </div>
              </div>
            </div>
          </div>

          <!-- WALLET — real balance tracking -->
          <div class="method-content" id="method-wallet">
            <div style="font-size:0.85rem;color:var(--text-muted);margin-bottom:1rem;">Select your wallet</div>
            <div class="wallet-options" id="walletOptions">
              <div class="wallet-opt selected" onclick="selectWallet(this)" data-wallet="paytm" data-balance="1250"><div class="wallet-icon">🟢</div><div><div class="wallet-name">Paytm Wallet</div><div class="wallet-bal" id="balPaytm">Balance: ₹1,250</div></div></div>
              <div class="wallet-opt" onclick="selectWallet(this)" data-wallet="phonepe" data-balance="890"><div class="wallet-icon">🔵</div><div><div class="wallet-name">PhonePe Wallet</div><div class="wallet-bal" id="balPhonepe">Balance: ₹890</div></div></div>
              <div class="wallet-opt" onclick="selectWallet(this)" data-wallet="amazon" data-balance="500"><div class="wallet-icon">🟠</div><div><div class="wallet-name">Amazon Pay</div><div class="wallet-bal" id="balAmazon">Balance: ₹500</div></div></div>
              <div class="wallet-opt" onclick="selectWallet(this)" data-wallet="mobikwik" data-balance="320"><div class="wallet-icon">🟣</div><div><div class="wallet-name">Mobikwik</div><div class="wallet-bal" id="balMobikwik">Balance: ₹320</div></div></div>
            </div>
            <div id="walletWarning" style="display:none;margin-top:0.75rem;padding:0.6rem 1rem;background:rgba(239,68,68,0.08);border:1px solid rgba(239,68,68,0.3);border-radius:8px;font-size:0.8rem;color:var(--red);">⚠ Insufficient balance in selected wallet. Please choose another or add money.</div>
          </div>

          <!-- NET BANKING -->
          <div class="method-content" id="method-netbanking">
            <div style="font-size:0.85rem;color:var(--text-muted);margin-bottom:1rem;">Select your bank</div>
            <div style="display:grid;grid-template-columns:repeat(2,1fr);gap:0.75rem;" id="bankList">
              <div class="wallet-opt selected" onclick="selectBank(this)"><div class="wallet-icon">🏦</div><div><div class="wallet-name">SBI</div></div></div>
              <div class="wallet-opt" onclick="selectBank(this)"><div class="wallet-icon">🏦</div><div><div class="wallet-name">HDFC Bank</div></div></div>
              <div class="wallet-opt" onclick="selectBank(this)"><div class="wallet-icon">🏦</div><div><div class="wallet-name">ICICI Bank</div></div></div>
              <div class="wallet-opt" onclick="selectBank(this)"><div class="wallet-icon">🏦</div><div><div class="wallet-name">Axis Bank</div></div></div>
              <div class="wallet-opt" onclick="selectBank(this)"><div class="wallet-icon">🏦</div><div><div class="wallet-name">Kotak Bank</div></div></div>
              <div class="wallet-opt" onclick="selectBank(this)"><div class="wallet-icon">🏦</div><div><div class="wallet-name">Bank of Baroda</div></div></div>
            </div>
          </div>

          <!-- FASTAG -->
          <div class="method-content" id="method-fastag">
            <div style="background:var(--bg3);border:1px solid rgba(0,245,212,0.2);border-radius:12px;padding:2rem;text-align:center;">
              <div style="font-size:3rem;margin-bottom:1rem;">🚗</div>
              <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:1.1rem;margin-bottom:0.5rem;">FASTag Auto Payment</div>
              <p style="font-size:0.85rem;color:var(--text-muted);">Payment will be automatically deducted from your linked FASTag account when you exit the parking facility.</p>
              <div style="margin-top:1.5rem;background:rgba(0,245,212,0.06);border:1px solid rgba(0,245,212,0.2);border-radius:8px;padding:1rem;">
                <div style="font-family:'JetBrains Mono',monospace;font-size:0.75rem;color:var(--cyan);">LINKED FASTAG</div>
                <div style="font-size:0.9rem;margin-top:0.25rem;" id="fastagVehicle">TN09 AB 1234</div>
                <div style="font-size:0.8rem;color:var(--green);margin-top:0.25rem;">Balance: ₹2,400 ✓</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Summary -->
        <div class="payment-summary-box">
          <div class="pay-summary-title">Order Summary</div>

          <!-- Tiny car -->
          <div style="text-align:center;margin-bottom:1.25rem;padding:1rem;background:var(--bg3);border-radius:10px;border:1px solid var(--border);">
            <svg viewBox="0 0 160 70" xmlns="http://www.w3.org/2000/svg" style="width:140px;">
              <ellipse cx="80" cy="64" rx="56" ry="4" fill="rgba(0,0,0,0.3)"/>
              <rect x="16" y="38" width="128" height="27" rx="5" fill="#0d2137" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
              <path d="M44 38 Q50 18 62 15 L98 15 Q110 18 116 38Z" fill="#112d4a" stroke="rgba(0,245,212,0.15)" stroke-width="1"/>
              <path d="M50 37 Q54 22 63 18 L77 18 L77 37Z" fill="rgba(0,245,212,0.06)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
              <path d="M81 37 L81 18 L97 18 Q106 22 110 37Z" fill="rgba(0,245,212,0.06)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
              <ellipse cx="32" cy="48" rx="9" ry="5" fill="#07111f" stroke="rgba(0,245,212,0.4)" stroke-width="1"/>
              <ellipse cx="32" cy="48" rx="4" ry="2" fill="rgba(0,245,212,0.5)"/>
              <circle cx="44" cy="63" r="10" fill="#06101a" stroke="rgba(0,245,212,0.4)" stroke-width="1.5"/>
              <circle cx="44" cy="63" r="6" fill="#0b1827"/>
              <circle cx="44" cy="63" r="2.5" fill="rgba(0,245,212,0.5)"/>
              <circle cx="116" cy="63" r="10" fill="#06101a" stroke="rgba(0,245,212,0.4)" stroke-width="1.5"/>
              <circle cx="116" cy="63" r="6" fill="#0b1827"/>
              <circle cx="116" cy="63" r="2.5" fill="rgba(0,245,212,0.5)"/>
              <rect x="63" y="50" width="34" height="9" rx="2" fill="#e8e8e8"/>
              <text x="80" y="58" text-anchor="middle" font-family="monospace" font-size="5" font-weight="bold" fill="#1a1a2e" id="payPlatePreview">TN09 AB 1234</text>
            </svg>
            <div style="font-size:0.72rem;color:var(--text-muted);font-family:'JetBrains Mono',monospace;margin-top:0.5rem;" id="paySlotPreview">Slot: B2-14</div>
          </div>

          <div class="pay-row"><span style="color:var(--text-muted);">Parking Slot</span><span id="paySlot" style="font-family:'JetBrains Mono',monospace;color:var(--cyan);">B2-14</span></div>
          <div class="pay-row"><span style="color:var(--text-muted);">Duration</span><span id="payDur">3 Hours</span></div>
          <div class="pay-row"><span style="color:var(--text-muted);">Vehicle</span><span id="payVehicle" style="font-family:'JetBrains Mono',monospace;">TN09 AB 1234</span></div>
          <div class="pay-row"><span style="color:var(--text-muted);">Parking Fee</span><span id="payBase">₹135</span></div>
          <div class="pay-row"><span style="color:var(--text-muted);">GST (18%)</span><span id="payGst">₹24</span></div>
          <div class="pay-row pay-total"><span>Total</span><span id="payTotal">₹159</span></div>

          <button class="pay-now-btn" id="payNowBtn" onclick="processPayment()">Pay Now →</button>
          <div class="security-note">🔒 256-bit SSL Encrypted · PCI DSS Compliant</div>
        </div>
      </div>
    </div>
  </div>
</div>


<!-- ══════════════════════════════════════
     USER DASHBOARD
══════════════════════════════════════ -->
<div class="page-overlay" id="userDashPage">
  <div class="user-dashboard">
    <div class="container">
      <span class="back-link" onclick="showPage('main')">← Back to Home</span>
      <div class="dashboard-header">
        <div>
          <div style="font-size:0.75rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.1em;font-family:'JetBrains Mono',monospace;margin-bottom:0.25rem;">// my account</div>
          <div class="dash-welcome" id="dashWelcome">Welcome, Surjith 👋</div>
        </div>
        <button class="btn-primary btn-sm" onclick="showPage('booking')">+ Book New Slot</button>
      </div>

      <!-- Summary Cards -->
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:1.25rem;margin-bottom:2rem;">
        <div class="metric-card"><div class="metric-info"><span class="metric-label">Total Bookings</span><span class="metric-value" id="dashTotalBookings">0</span></div><div class="metric-icon icon-cyan">📋</div></div>
        <div class="metric-card"><div class="metric-info"><span class="metric-label">Active Now</span><span class="metric-value" id="dashActive">0</span></div><div class="metric-icon icon-green">🟢</div></div>
        <div class="metric-card"><div class="metric-info"><span class="metric-label">Total Spent</span><span class="metric-value" id="dashSpent">₹0</span></div><div class="metric-icon icon-gold">💰</div></div>
        <div class="metric-card"><div class="metric-info"><span class="metric-label">Saved vs Avg</span><span class="metric-value" style="color:var(--green);">12%</span></div><div class="metric-icon icon-green">📈</div></div>
      </div>

      <!-- Account Info -->
      <div style="display:grid;grid-template-columns:1fr 2fr;gap:1.5rem;margin-bottom:2rem;">
        <div class="booking-card" style="background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:1.75rem;">
          <div style="font-family:'Syne',sans-serif;font-weight:700;margin-bottom:1.25rem;">Account Details</div>
          <div style="text-align:center;margin-bottom:1.25rem;">
            <div style="width:64px;height:64px;border-radius:50%;background:linear-gradient(135deg,var(--electric),var(--cyan-dim));display:flex;align-items:center;justify-content:center;font-size:1.5rem;font-weight:700;color:white;margin:0 auto 0.75rem;" id="dashAvatar">S</div>
            <div style="font-weight:600;" id="dashName">Surjith Raj S</div>
            <div style="font-size:0.8rem;color:var(--text-muted);" id="dashEmail">surjith@example.com</div>
          </div>
          <div style="font-size:0.8rem;">
            <div style="display:flex;justify-content:space-between;padding:0.5rem 0;border-bottom:1px solid var(--border);"><span style="color:var(--text-muted);">Phone</span><span id="dashPhone">+91 98765 43210</span></div>
            <div style="display:flex;justify-content:space-between;padding:0.5rem 0;border-bottom:1px solid var(--border);"><span style="color:var(--text-muted);">Vehicle</span><span style="font-family:'JetBrains Mono',monospace;color:var(--cyan);" id="dashVehicle">TN09 AB 1234</span></div>
            <div style="display:flex;justify-content:space-between;padding:0.5rem 0;"><span style="color:var(--text-muted);">Member Since</span><span id="dashSince">Jan 2025</span></div>
          </div>
        </div>

        <!-- Bookings Table -->
        <div style="background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:1.75rem;">
          <div style="font-family:'Syne',sans-serif;font-weight:700;margin-bottom:1.25rem;">Recent Bookings</div>
          <div style="overflow-x:auto;">
            <table class="bookings-table" id="bookingsTable">
              <thead><tr><th>Booking ID</th><th>Slot</th><th>Vehicle</th><th>Duration</th><th>Amount</th><th>Status</th></tr></thead>
              <tbody id="bookingsTbody">
                <tr><td colspan="6" style="text-align:center;color:var(--text-dim);padding:2rem;">No bookings yet. <span style="color:var(--cyan);cursor:pointer;" onclick="showPage('booking')">Book your first slot →</span></td></tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>


<script>
// ════════════════════════════════════════
// DATABASE (localStorage)
// ════════════════════════════════════════
const DB = {
  get: (k) => JSON.parse(localStorage.getItem('sp_'+k) || 'null'),
  set: (k,v) => localStorage.setItem('sp_'+k, JSON.stringify(v)),
  getUsers: () => DB.get('users') || [],
  getBookings: () => DB.get('bookings') || [],
  getCurrentUser: () => DB.get('currentUser'),
  saveUsers: (u) => DB.set('users', u),
  saveBookings: (b) => DB.set('bookings', b),
  setCurrentUser: (u) => DB.set('currentUser', u),
  clearCurrentUser: () => localStorage.removeItem('sp_currentUser'),
  addUser: (user) => {
    const users = DB.getUsers();
    users.push(user);
    DB.saveUsers(users);
  },
  addBooking: (booking) => {
    const bks = DB.getBookings();
    bks.push(booking);
    DB.saveBookings(bks);
  },
  findUser: (email) => DB.getUsers().find(u => u.email === email),
  getUserBookings: (userId) => DB.getBookings().filter(b => b.userId === userId),
};

// ════════════════════════════════════════
// PAGE ROUTING
// ════════════════════════════════════════
function showPage(page) {
  document.getElementById('mainPage').style.display = 'none';
  document.getElementById('authPage').classList.remove('active');
  document.getElementById('bookingPage').classList.remove('active');
  document.getElementById('paymentPage').classList.remove('active');
  document.getElementById('userDashPage').classList.remove('active');

  if (page === 'main') {
    document.getElementById('mainPage').style.display = 'block';
    window.scrollTo(0,0);
  } else if (page === 'auth') {
    document.getElementById('authPage').classList.add('active');
    authPage.scrollTo(0,0);
  } else if (page === 'booking') {
    document.getElementById('bookingPage').classList.add('active');
    initBookingGrid();
    bookingPage.scrollTo(0,0);
  } else if (page === 'payment') {
    document.getElementById('paymentPage').classList.add('active');
    syncPaymentSummary();
    paymentPage.scrollTo(0,0);
  } else if (page === 'userdash') {
    document.getElementById('userDashPage').classList.add('active');
    loadUserDashboard();
    userDashPage.scrollTo(0,0);
  }
}

function requireAuth(dest) {
  if (DB.getCurrentUser()) {
    showPage(dest);
  } else {
    pendingDest = dest;
    showPage('auth');
  }
}
let pendingDest = 'booking';

// ════════════════════════════════════════
// AUTH
// ════════════════════════════════════════
function switchTab(tab) {
  document.getElementById('loginTab').classList.toggle('active', tab==='login');
  document.getElementById('registerTab').classList.toggle('active', tab==='register');
  document.getElementById('loginForm').style.display = tab==='login' ? 'block' : 'none';
  document.getElementById('registerForm').style.display = tab==='register' ? 'block' : 'none';
}

function handleLogin() {
  const email = document.getElementById('loginEmail').value.trim();
  const pass = document.getElementById('loginPass').value;
  const emailErr = document.getElementById('loginEmailErr');
  const passErr = document.getElementById('loginPassErr');
  emailErr.classList.remove('show');
  passErr.classList.remove('show');
  if (!email || !email.includes('@')) { emailErr.classList.add('show'); return; }
  const user = DB.findUser(email);
  if (!user || user.password !== pass) { passErr.textContent = 'Incorrect email or password.'; passErr.classList.add('show'); return; }
  DB.setCurrentUser(user);
  updateNavUser(user);
  showToast('✅ Welcome back, ' + user.firstName + '!', 'success');
  showPage(pendingDest || 'main');
}

function handleRegister() {
  const first = document.getElementById('regFirst').value.trim();
  const last = document.getElementById('regLast').value.trim();
  const email = document.getElementById('regEmail').value.trim();
  const phone = document.getElementById('regPhone').value.trim();
  const vehicle = document.getElementById('regVehicle').value.trim().toUpperCase();
  const pass = document.getElementById('regPass').value;
  const emailErr = document.getElementById('regEmailErr');
  const passErr = document.getElementById('regPassErr');
  emailErr.classList.remove('show');
  passErr.classList.remove('show');
  if (!email || !email.includes('@')) { emailErr.textContent='Please enter a valid email.'; emailErr.classList.add('show'); return; }
  if (DB.findUser(email)) { emailErr.textContent='Email already registered.'; emailErr.classList.add('show'); return; }
  if (pass.length < 8) { passErr.classList.add('show'); return; }
  const user = { id: 'u_'+Date.now(), firstName: first||'User', lastName: last, email, phone: phone||'+91 98765 43210', vehicle: vehicle||'TN09 AB 1234', password: pass, joinDate: new Date().toLocaleDateString('en-IN', {month:'short',year:'numeric'}) };
  DB.addUser(user);
  DB.setCurrentUser(user);
  updateNavUser(user);
  showToast('🎉 Account created! Welcome, ' + user.firstName + '!', 'success');
  if (document.getElementById('bookVehicle')) document.getElementById('bookVehicle').value = user.vehicle;
  showPage(pendingDest || 'main');
}

function socialLogin(provider) {
  const user = { id: 'u_'+Date.now(), firstName: 'Demo', lastName: 'User', email: 'demo@smartpark.ai', phone: '+91 98765 43210', vehicle: 'TN09 AB 1234', password: '', joinDate: new Date().toLocaleDateString('en-IN',{month:'short',year:'numeric'}) };
  if (!DB.findUser(user.email)) DB.addUser(user);
  DB.setCurrentUser(user);
  updateNavUser(user);
  showToast('✅ Signed in with '+provider, 'success');
  showPage(pendingDest || 'main');
}

function logout() {
  DB.clearCurrentUser();
  updateNavUser(null);
  showPage('main');
  showToast('👋 Logged out successfully', 'success');
}

function updateNavUser(user) {
  const navUser = document.getElementById('navUser');
  const navLinks = document.getElementById('navLinks');
  if (user) {
    document.getElementById('navAvatar').textContent = user.firstName[0].toUpperCase();
    document.getElementById('navUsername').textContent = user.firstName;
    navUser.style.display = 'flex';
    // hide login link in nav
  } else {
    navUser.style.display = 'none';
  }
}

// ════════════════════════════════════════
// BOOKING
// ════════════════════════════════════════
let selectedSlot = null;
let selectedDuration = 3;
let selectedPrice = 135;
const slotTypes = ['free','free','free','free','occupied','occupied','reserved','ev','disabled'];

function initBookingGrid() {
  const grid = document.getElementById('bookingGrid');
  grid.innerHTML = '';
  for (let i = 0; i < 64; i++) {
    let t = slotTypes[Math.floor(Math.random() * slotTypes.length)];
    if (i < 10 && i % 3 !== 0) t = 'free'; // ensure some free spots
    const d = document.createElement('div');
    d.className = 'b-spot ' + t;
    const lbl = document.createElement('div');
    lbl.className = 'spot-label';
    lbl.textContent = 'B'+(i+1);
    d.appendChild(lbl);
    const slotId = 'B2-' + String(i+1).padStart(2,'0');
    d.title = slotId;
    if (t === 'free' || t === 'ev') {
      d.onclick = () => selectSlot(d, slotId);
    }
    grid.appendChild(d);
  }
  // Pre-fill vehicle
  const user = DB.getCurrentUser();
  if (user && document.getElementById('bookVehicle')) {
    document.getElementById('bookVehicle').value = user.vehicle;
    updatePlatePreview(user.vehicle);
  }
  document.getElementById('bookVehicle').addEventListener('input', (e) => updatePlatePreview(e.target.value));
}

function updatePlatePreview(val) {
  const el = document.getElementById('bookPlatePreview');
  if (el) el.textContent = val.toUpperCase() || 'TN09 AB 1234';
}

function selectSlot(el, slotId) {
  document.querySelectorAll('.b-spot.selected').forEach(s => { s.classList.remove('selected'); s.classList.add('free'); });
  el.classList.remove('free','ev');
  el.classList.add('selected');
  selectedSlot = slotId;
  document.getElementById('sumSlot').textContent = slotId;
  updateSummary();
}

function selectDuration(h, el) {
  document.querySelectorAll('#durationPicker .upi-app').forEach(e => e.classList.remove('selected'));
  el.classList.add('selected');
  selectedDuration = h;
  const prices = {1:50, 2:95, 3:135, 6:240};
  selectedPrice = prices[h];
  updateSummary();
}

function updateSummary() {
  const gst = Math.round(selectedPrice * 0.18);
  const total = selectedPrice + gst;
  document.getElementById('sumDur').textContent = selectedDuration + ' Hour' + (selectedDuration>1?'s':'');
  document.getElementById('sumBase').textContent = '₹'+selectedPrice;
  document.getElementById('sumGst').textContent = '₹'+gst;
  document.getElementById('sumTotal').textContent = '₹'+total;
}

function proceedToPayment() {
  if (!selectedSlot) { showToast('⚠ Please select a parking slot', 'error'); return; }
  const vehicle = document.getElementById('bookVehicle').value.trim().toUpperCase();
  if (!vehicle) { showToast('⚠ Please enter your vehicle number', 'error'); return; }
  bookingData = { slot: selectedSlot, duration: selectedDuration, price: selectedPrice, vehicle, gst: Math.round(selectedPrice*0.18), total: selectedPrice + Math.round(selectedPrice*0.18) };
  showPage('payment');
}

let bookingData = {};

function syncPaymentSummary() {
  if (!bookingData.slot) return;
  document.getElementById('paySlot').textContent = bookingData.slot;
  document.getElementById('payDur').textContent = bookingData.duration + ' Hour' + (bookingData.duration>1?'s':'');
  document.getElementById('payVehicle').textContent = bookingData.vehicle;
  document.getElementById('payBase').textContent = '₹'+bookingData.price;
  document.getElementById('payGst').textContent = '₹'+bookingData.gst;
  document.getElementById('payTotal').textContent = '₹'+bookingData.total;
  document.getElementById('paySlotPreview').textContent = 'Slot: '+bookingData.slot;
  document.getElementById('payPlatePreview').textContent = bookingData.vehicle;
  document.getElementById('fastagVehicle').textContent = bookingData.vehicle;
}

// ════════════════════════════════════════
// PAYMENT
// ════════════════════════════════════════
function switchMethod(m, el) {
  document.querySelectorAll('.method-tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.method-content').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  document.getElementById('method-'+m).classList.add('active');
}

function selectUpiApp(el) { document.querySelectorAll('.upi-apps .upi-app').forEach(e => e.classList.remove('selected')); el.classList.add('selected'); }
function selectWallet(el) { document.querySelectorAll('.wallet-opt').forEach(e => e.classList.remove('selected')); el.classList.add('selected'); }
function selectBank(el) { document.querySelectorAll('#bankList .wallet-opt').forEach(e => e.classList.remove('selected')); el.classList.add('selected'); }

function formatCard(inp) {
  let v = inp.value.replace(/\D/g,'').substring(0,16);
  inp.value = v.replace(/(.{4})/g,'$1 ').trim();
  const preview = v.padEnd(16,'•').replace(/(.{4})/g,'$1 ').trim();
  document.getElementById('cardNumPreview').textContent = preview || '•••• •••• •••• ••••';
}
function formatExp(inp) {
  let v = inp.value.replace(/\D/g,'');
  if (v.length >= 2) v = v.substring(0,2)+'/'+v.substring(2,4);
  inp.value = v;
  document.getElementById('cardExpPreview').textContent = inp.value || 'MM/YY';
}

function processPayment() {
  const btn = document.getElementById('payNowBtn');
  btn.textContent = '⏳ Processing...';
  btn.disabled = true;
  setTimeout(() => {
    btn.textContent = '✅ Payment Successful!';
    saveBooking();
    setTimeout(showSuccessModal, 400);
  }, 2200);
}

function saveBooking() {
  const user = DB.getCurrentUser();
  const bookingId = 'BK'+Date.now().toString().slice(-6);
  const booking = {
    id: bookingId,
    userId: user ? user.id : 'guest',
    slot: bookingData.slot,
    vehicle: bookingData.vehicle,
    duration: bookingData.duration,
    price: bookingData.price,
    gst: bookingData.gst,
    total: bookingData.total,
    status: 'Active',
    time: new Date().toLocaleTimeString('en-IN', {hour:'2-digit',minute:'2-digit'}),
    date: new Date().toLocaleDateString('en-IN')
  };
  DB.addBooking(booking);
  // Show in modal
  document.getElementById('t-id').textContent = bookingId;
  document.getElementById('t-slot').textContent = booking.slot;
  document.getElementById('t-dur').textContent = booking.duration + 'h';
  document.getElementById('t-amt').textContent = '₹'+booking.total;
}

function showSuccessModal() {
  document.getElementById('successModal').classList.add('active');
}

function closeModal() {
  document.getElementById('successModal').classList.remove('active');
  document.getElementById('payNowBtn').textContent = 'Pay Now →';
  document.getElementById('payNowBtn').disabled = false;
  showPage('userdash');
}

// ════════════════════════════════════════
// USER DASHBOARD
// ════════════════════════════════════════
function loadUserDashboard() {
  const user = DB.getCurrentUser();
  if (!user) { showPage('auth'); return; }
  document.getElementById('dashWelcome').textContent = 'Welcome, ' + user.firstName + ' 👋';
  document.getElementById('dashAvatar').textContent = user.firstName[0];
  document.getElementById('dashName').textContent = user.firstName + ' ' + (user.lastName||'');
  document.getElementById('dashEmail').textContent = user.email;
  document.getElementById('dashPhone').textContent = user.phone;
  document.getElementById('dashVehicle').textContent = user.vehicle;
  document.getElementById('dashSince').textContent = user.joinDate;

  const bookings = DB.getUserBookings(user.id);
  const totalSpent = bookings.reduce((s,b) => s+b.total, 0);
  const active = bookings.filter(b => b.status==='Active').length;

  document.getElementById('dashTotalBookings').textContent = bookings.length;
  document.getElementById('dashActive').textContent = active;
  document.getElementById('dashSpent').textContent = '₹'+totalSpent;

  const tbody = document.getElementById('bookingsTbody');
  if (bookings.length === 0) {
    tbody.innerHTML = '<tr><td colspan="6" style="text-align:center;color:var(--text-dim);padding:2rem;">No bookings yet. <span style="color:var(--cyan);cursor:pointer;" onclick="showPage(\'booking\')">Book your first slot →</span></td></tr>';
  } else {
    tbody.innerHTML = bookings.slice().reverse().map(b => `
      <tr>
        <td style="font-family:'JetBrains Mono',monospace;font-size:0.78rem;color:var(--cyan)">${b.id}</td>
        <td style="font-family:'JetBrains Mono',monospace">${b.slot}</td>
        <td style="font-family:'JetBrains Mono',monospace">${b.vehicle}</td>
        <td>${b.duration}h</td>
        <td style="color:var(--green)">₹${b.total}</td>
        <td><span class="status-badge status-${b.status.toLowerCase()}">${b.status}</span></td>
      </tr>
    `).join('');
  }
}

// ════════════════════════════════════════
// TOAST
// ════════════════════════════════════════
let toastTimer;
function showToast(msg, type='success') {
  const toast = document.getElementById('toast');
  toast.textContent = msg;
  toast.className = 'toast toast-'+type+' show';
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => toast.classList.remove('show'), 3000);
}

// ════════════════════════════════════════
// MAIN PAGE INIT
// ════════════════════════════════════════
function animateCounter(el, target, suffix='', duration=1500) {
  let start = 0;
  const step = target / (duration / 16);
  const timer = setInterval(() => {
    start = Math.min(start + step, target);
    el.textContent = Math.round(start).toLocaleString() + suffix;
    if (start >= target) clearInterval(timer);
  }, 16);
}

setTimeout(() => {
  animateCounter(document.getElementById('cnt1'), 50000, '+');
  animateCounter(document.getElementById('cnt2'), 42);
  animateCounter(document.getElementById('cnt3'), 12000, '+');
  let n = 0;
  const ival = setInterval(() => { n++; document.getElementById('cnt4').textContent = n + '%'; if(n>=99) clearInterval(ival); }, 15);
}, 800);

// Parking grid
const types = ['free','free','free','occupied','occupied','reserved','ev','disabled'];
const grid = document.getElementById('parkingGrid');
const spots = [];
for (let i = 0; i < 64; i++) {
  const t = types[Math.floor(Math.random() * types.length)];
  const d = document.createElement('div');
  d.className = 'pspot ' + t;
  d.title = 'Slot B2-'+String(i+1).padStart(2,'0')+' · '+t;
  grid.appendChild(d);
  spots.push(d);
}
setInterval(() => {
  const idx = Math.floor(Math.random() * 64);
  const t = types[Math.floor(Math.random() * types.length)];
  spots[idx].className = 'pspot ' + t;
  const free = spots.filter(s => s.className.includes('free')).length;
  const occ = spots.filter(s => s.className.includes('occupied')).length;
  document.getElementById('freeCount').textContent = free;
  document.getElementById('occCount').textContent = occ;
  document.getElementById('freeBar').style.width = (free/64*100)+'%';
}, 1200);

// Prediction chart
const predData = [65,72,80,88,92,85,78,70,60,55,50,48];
const predChart = document.getElementById('predChart');
predData.forEach((v,i) => {
  const bar = document.createElement('div');
  bar.style.cssText = `flex:1;background:${i<6?'rgba(0,245,212,0.5)':'rgba(124,58,237,0.4)'};border-radius:3px 3px 0 0;height:${v}%;`;
  predChart.appendChild(bar);
});

// Detection log
const plates = ['TN09AB1234','TN07CD5678','TN10EF9012','TN01GH3456','TN22IJ7890','KA03KL1122','AP16MN3344','MH12OP5566'];
const statusTypes = [{cls:'det-in',label:'ENTRY'},{cls:'det-out',label:'EXIT'},{cls:'det-vip',label:'VIP'}];
const logEl = document.getElementById('detectionLog');
const camStatus = document.getElementById('camStatus');
const camMessages = ['SCANNING...','VEHICLE DETECTED','READING PLATE...','PLATE MATCHED','SLOT ASSIGNED','GATE OPENED'];
let msgIdx = 0;
function addDetection() {
  const plate = plates[Math.floor(Math.random()*plates.length)];
  const st = statusTypes[Math.floor(Math.random()*statusTypes.length)];
  const time = new Date().toTimeString().slice(0,8);
  const entry = document.createElement('div');
  entry.className = 'det-entry';
  entry.style.opacity = '0';
  entry.innerHTML = `<span class="det-plate">${plate}</span><span class="det-time">${time}</span><span class="det-status ${st.cls}">${st.label}</span>`;
  logEl.insertBefore(entry, logEl.firstChild);
  setTimeout(() => { entry.style.transition='all 0.3s'; entry.style.opacity='1'; }, 10);
  if (logEl.children.length > 6) logEl.removeChild(logEl.lastChild);
}
for (let i = 0; i < 5; i++) setTimeout(addDetection, i*200);
setInterval(addDetection, 3000);
setInterval(() => { camStatus.textContent = camMessages[msgIdx%camMessages.length]; msgIdx++; }, 1000);

// Car strip
function makeCar(scale=1, colorH=200) {
  return `<svg viewBox="0 0 200 70" xmlns="http://www.w3.org/2000/svg" style="width:${120*scale}px;flex-shrink:0;margin-right:30px;">
    <ellipse cx="100" cy="62" rx="65" ry="5" fill="rgba(0,0,0,0.3)"/>
    <rect x="15" y="35" width="170" height="27" rx="5" fill="hsl(${colorH},60%,12%)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
    <path d="M48 35 Q55 15 68 12 L132 12 Q145 15 152 35Z" fill="hsl(${colorH},60%,14%)" stroke="rgba(0,245,212,0.15)" stroke-width="1"/>
    <path d="M55 34 Q60 18 68 14 L97 14 L97 34Z" fill="rgba(0,245,212,0.05)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
    <path d="M101 34 L101 14 L132 14 Q140 18 145 34Z" fill="rgba(0,245,212,0.05)" stroke="rgba(0,245,212,0.2)" stroke-width="1"/>
    <ellipse cx="30" cy="48" rx="10" ry="5" fill="rgba(0,18,36,0.9)" stroke="rgba(0,245,212,0.5)" stroke-width="1"/>
    <ellipse cx="30" cy="48" rx="4" ry="2" fill="rgba(0,245,212,0.5)"/>
    <circle cx="47" cy="61" r="11" fill="#060e1a" stroke="rgba(0,245,212,0.35)" stroke-width="1.5"/>
    <circle cx="47" cy="61" r="7" fill="#0b1827"/>
    <circle cx="47" cy="61" r="3" fill="rgba(0,245,212,0.5)"/>
    <circle cx="153" cy="61" r="11" fill="#060e1a" stroke="rgba(0,245,212,0.35)" stroke-width="1.5"/>
    <circle cx="153" cy="61" r="7" fill="#0b1827"/>
    <circle cx="153" cy="61" r="3" fill="rgba(0,245,212,0.5)"/>
  </svg>`;
}
const t1 = document.getElementById('carTrack1');
const t2 = document.getElementById('carTrack2');
const carColors = [200, 220, 180, 260, 190, 210];
for (let i = 0; i < 14; i++) {
  t1.innerHTML += makeCar(1, carColors[i%carColors.length]);
  t2.innerHTML += makeCar(0.8, carColors[(i+2)%carColors.length]);
}

// Scroll reveal
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => { if(e.isIntersecting){ e.target.classList.add('visible'); observer.unobserve(e.target); } });
}, { threshold: 0.1 });
reveals.forEach(el => observer.observe(el));

// Init nav state
const u = DB.getCurrentUser();
if (u) updateNavUser(u);


// ════════════════════════════════════════
// CAMERA + TESSERACT OCR — NUMBER PLATE
// ════════════════════════════════════════
let cameraStream = null;
let selectedUpiApp = { app: 'gpay', scheme: 'tez://upi/pay', name: 'GPay' };

async function startCamera() {
  try {
    const constraints = { video: { facingMode: { ideal: 'environment' }, width: { ideal: 1280 }, height: { ideal: 720 } } };
    cameraStream = await navigator.mediaDevices.getUserMedia(constraints);
    const video = document.getElementById('realCamVideo');
    video.srcObject = cameraStream;
    video.style.display = 'block';
    document.getElementById('camPlaceholder').style.display = 'none';
    document.getElementById('liveScanOverlay').style.display = 'block';
    document.getElementById('camRecIndicator').style.display = 'flex';
    document.getElementById('startCamBtn').style.display = 'none';
    document.getElementById('snapBtn').style.display = 'inline-block';
    document.getElementById('stopCamBtn').style.display = 'inline-block';
    document.getElementById('ocrResultBox').style.display = 'none';
    showToast('📷 Camera started. Align plate in the frame.', 'success');
  } catch(e) {
    showToast('⚠ Camera access denied. Please allow camera or use "Upload Image".', 'error');
  }
}

function stopCamera() {
  if (cameraStream) { cameraStream.getTracks().forEach(t => t.stop()); cameraStream = null; }
  const video = document.getElementById('realCamVideo');
  video.style.display = 'none';
  video.srcObject = null;
  document.getElementById('camPlaceholder').style.display = 'flex';
  document.getElementById('liveScanOverlay').style.display = 'none';
  document.getElementById('camRecIndicator').style.display = 'none';
  document.getElementById('startCamBtn').style.display = 'inline-block';
  document.getElementById('snapBtn').style.display = 'none';
  document.getElementById('stopCamBtn').style.display = 'none';
  document.getElementById('camStatus').textContent = 'CLICK START CAMERA';
}

function captureAndScan() {
  const video = document.getElementById('realCamVideo');
  const canvas = document.getElementById('snapCanvas');
  canvas.width = video.videoWidth;
  canvas.height = video.videoHeight;
  canvas.getContext('2d').drawImage(video, 0, 0);
  const dataUrl = canvas.toDataURL('image/png');
  const preview = document.getElementById('snapPreview');
  preview.src = dataUrl;
  preview.style.display = 'block';
  stopCamera();
  runOCR(canvas);
}

function handleImageUpload(evt) {
  const file = evt.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(e) {
    const img = new Image();
    img.onload = function() {
      const canvas = document.getElementById('snapCanvas');
      canvas.width = img.width;
      canvas.height = img.height;
      canvas.getContext('2d').drawImage(img, 0, 0);
      const preview = document.getElementById('snapPreview');
      preview.src = e.target.result;
      preview.style.display = 'block';
      document.getElementById('camPlaceholder').style.display = 'none';
      runOCR(canvas);
    };
    img.src = e.target.result;
  };
  reader.readAsDataURL(file);
}

function runOCR(canvas) {
  const prog = document.getElementById('ocrProgress');
  const bar = document.getElementById('ocrBar');
  const progText = document.getElementById('ocrProgressText');
  prog.style.display = 'block';
  document.getElementById('ocrResultBox').style.display = 'none';
  let pct = 0;
  const interval = setInterval(() => { pct = Math.min(pct + Math.random()*15, 90); bar.style.width = pct+'%'; }, 200);

  // Load Tesseract.js from CDN
  if (!window.Tesseract) {
    const s = document.createElement('script');
    s.src = 'https://cdn.jsdelivr.net/npm/tesseract.js@5/dist/tesseract.min.js';
    document.head.appendChild(s);
    s.onload = () => doRecognize(canvas, interval, bar, prog, progText);
  } else {
    doRecognize(canvas, interval, bar, prog, progText);
  }
}

function doRecognize(canvas, interval, bar, prog, progText) {
  progText.textContent = 'OCR engine initializing...';
  Tesseract.recognize(canvas, 'eng', {
    logger: m => {
      if (m.status === 'recognizing text') {
        progText.textContent = 'Reading plate: ' + Math.round((m.progress||0)*100) + '%';
        bar.style.width = Math.round((m.progress||0)*100) + '%';
      }
    }
  }).then(({ data: { text, confidence } }) => {
    clearInterval(interval);
    bar.style.width = '100%';
    setTimeout(() => { prog.style.display = 'none'; }, 500);

    // Clean OCR text — keep only letters, digits, spaces
    let raw = text.replace(/[^A-Z0-9 ]/gi, '').trim().toUpperCase();
    // Try to extract Indian plate format (e.g. TN09AB1234)
    const match = raw.match(/([A-Z]{2}[\s]?[0-9]{1,2}[\s]?[A-Z]{1,3}[\s]?[0-9]{3,4})/);
    const plate = match ? match[0].replace(/\s+/g,' ').trim() : (raw.slice(0,15) || 'UNREADABLE');
    const conf = Math.round(confidence);

    document.getElementById('ocrPlateText').textContent = plate;
    document.getElementById('ocrConf').textContent = conf > 60 ? '✓ Confidence: ' + conf + '%' : '⚠ Low confidence: ' + conf + '% — verify manually';
    document.getElementById('ocrResultBox').style.display = 'block';

    // Log to detection panel
    const entry = document.createElement('div');
    entry.className = 'det-entry';
    entry.style.opacity = '0';
    entry.innerHTML = `<span class="det-plate">${plate}</span><span class="det-time">${new Date().toTimeString().slice(0,8)}</span><span class="det-status det-in">OCR SCAN</span>`;
    const log = document.getElementById('detectionLog');
    log.insertBefore(entry, log.firstChild);
    setTimeout(() => { entry.style.transition='all 0.3s'; entry.style.opacity='1'; }, 10);
    if (log.children.length > 6) log.removeChild(log.lastChild);

    showToast('🔍 Plate read: ' + plate, 'success');
  }).catch(() => {
    clearInterval(interval);
    prog.style.display = 'none';
    showToast('⚠ OCR failed. Try a clearer image.', 'error');
  });
}

function useScannedPlate() {
  const plate = document.getElementById('ocrPlateText').textContent;
  if (!plate || plate === '—') return;
  requireAuth('booking');
  setTimeout(() => {
    const vehicleInput = document.getElementById('bookVehicle');
    if (vehicleInput) vehicleInput.value = plate;
    showToast('✅ Plate ' + plate + ' auto-filled in booking!', 'success');
  }, 400);
}

function retakeScan() {
  document.getElementById('snapPreview').style.display = 'none';
  document.getElementById('ocrResultBox').style.display = 'none';
  document.getElementById('camPlaceholder').style.display = 'flex';
  document.getElementById('camStatus').textContent = 'CLICK START CAMERA';
  document.getElementById('startCamBtn').style.display = 'inline-block';
}

// ════════════════════════════════════════
// UPI DEEP LINKS — REAL APP OPENING
// ════════════════════════════════════════
function selectUpiApp(el) {
  document.querySelectorAll('.upi-app').forEach(a => a.classList.remove('selected'));
  el.classList.add('selected');
  selectedUpiApp = { app: el.dataset.app, scheme: el.dataset.scheme, name: el.querySelector('.upi-app-name').textContent };
  const btn = document.getElementById('openUpiAppBtn');
  if (btn) btn.textContent = 'Open ' + selectedUpiApp.name + ' App to Pay →';
  drawUpiQr();
}

function openUpiApp() {
  const total = document.getElementById('payTotal') ? document.getElementById('payTotal').textContent.replace('₹','').replace(',','').trim() : '159';
  const vehicle = document.getElementById('payVehicle') ? document.getElementById('payVehicle').textContent : 'Vehicle';
  const upiVpa = 'sujir9311-2@okaxis';
  const name = encodeURIComponent('Suji SmartPark');
  const note = encodeURIComponent('Parking: ' + vehicle);
  const amt = encodeURIComponent(total);
  // Standard UPI Intent URL — works on Android with UPI apps installed
  const upiUrl = `upi://pay?pa=${upiVpa}&pn=${name}&tn=${note}&am=${amt}&cu=INR`;
  // App-specific deep links
  const links = {
    gpay: `tez://upi/pay?pa=${upiVpa}&pn=${name}&tn=${note}&am=${amt}&cu=INR`,
    phonepe: `phonepe://pay?pa=${upiVpa}&pn=${name}&tn=${note}&am=${amt}&cu=INR`,
    paytm: `paytmmp://pay?pa=${upiVpa}&pn=${name}&tn=${note}&am=${amt}&cu=INR`,
    bhim: upiUrl,
  };
  const deepLink = links[selectedUpiApp.app] || upiUrl;
  // Try app deep link, fallback to standard UPI
  window.location.href = deepLink;
  // Fallback: after 1.5s if still here, try the UPI universal link
  setTimeout(() => {
    showToast('📱 Opening ' + selectedUpiApp.name + '... If nothing opened, use UPI ID: ' + upiVpa, 'info');
  }, 1500);
}

function drawUpiQr() { return; // Real QR image is embedded — no canvas draw needed
  //
  // Draw a simple visual QR-like pattern on canvas (not a real scannable QR — for UI)
  // For a real scannable QR, Tesseract can be replaced with qrcode.js
  const canvas = document.getElementById('upiQrCanvas');
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const size = 140;
  ctx.fillStyle = '#ffffff';
  ctx.fillRect(0, 0, size, size);
  ctx.fillStyle = '#1a1a2e';
  // Corner squares
  [[8,8],[96,8],[8,96]].forEach(([x,y]) => {
    ctx.fillRect(x, y, 36, 36);
    ctx.fillStyle = '#fff';
    ctx.fillRect(x+4, y+4, 28, 28);
    ctx.fillStyle = '#1a1a2e';
    ctx.fillRect(x+9, y+9, 18, 18);
    ctx.fillStyle = '#fff';
  });
  ctx.fillStyle = '#1a1a2e';
  // Data cells (pseudorandom but stable)
  const cells = [[56,8],[68,8],[56,20],[68,20],[56,56],[68,56],[80,56],[8,56],[20,56],[32,56],[56,68],[80,68],[92,68],[56,80],[68,80],[92,80],[68,92],[92,92],[104,92],[104,56],[104,44]];
  cells.forEach(([x,y]) => ctx.fillRect(x, y, 10, 10));
  // SmartPark center label
  ctx.fillStyle = '#00f5d4';
  ctx.font = 'bold 7px monospace';
  ctx.textAlign = 'center';
  ctx.fillText('🅿', 70, 48);
}

// Update QR and label when payment amount changes
const origSyncPayment = typeof syncPaymentSummary === 'function' ? syncPaymentSummary : null;
function syncPaymentSummary() {
  if (origSyncPayment) origSyncPayment();
  setTimeout(() => {
    const total = document.getElementById('payTotal');
    if (total && document.getElementById('upiAmountLabel')) {
      document.getElementById('upiAmountLabel').textContent = 'Amount: ' + total.textContent;
    }
    drawUpiQr();
  }, 100);
}

// ════════════════════════════════════════
// WALLET — REAL BALANCE DEDUCTION
// ════════════════════════════════════════
const walletBalances = { paytm: 1250, phonepe: 890, amazon: 500, mobikwik: 320 };
let selectedWalletKey = 'paytm';

function selectWallet(el) {
  document.querySelectorAll('.wallet-opt').forEach(w => w.classList.remove('selected'));
  el.classList.add('selected');
  selectedWalletKey = el.dataset.wallet;
  checkWalletBalance();
}

function checkWalletBalance() {
  const total = getPayableAmount();
  const bal = walletBalances[selectedWalletKey] || 0;
  const warn = document.getElementById('walletWarning');
  if (warn) warn.style.display = bal < total ? 'block' : 'none';
}

function getPayableAmount() {
  const el = document.getElementById('payTotal');
  if (!el) return 159;
  return parseInt(el.textContent.replace(/[^0-9]/g,'')) || 159;
}

function deductWalletBalance(amount) {
  walletBalances[selectedWalletKey] = Math.max(0, (walletBalances[selectedWalletKey] || 0) - amount);
  const balMap = { paytm: 'balPaytm', phonepe: 'balPhonepe', amazon: 'balAmazon', mobikwik: 'balMobikwik' };
  const el = document.getElementById(balMap[selectedWalletKey]);
  if (el) el.textContent = 'Balance: ₹' + walletBalances[selectedWalletKey].toLocaleString('en-IN');
  // Update selected opt text too
  document.querySelectorAll('.wallet-opt').forEach(w => {
    if (w.dataset.wallet === selectedWalletKey) {
      const bd = w.querySelector('.wallet-bal');
      if (bd) bd.textContent = 'Balance: ₹' + walletBalances[selectedWalletKey].toLocaleString('en-IN');
    }
  });
}

// Hook into existing processPayment — intercept wallet
const _origProcessPayment = typeof processPayment === 'function' ? processPayment : null;
window.processPayment = function() {
  const activeMethod = document.querySelector('.method-tab.active');
  const method = activeMethod ? activeMethod.textContent.toLowerCase() : '';
  if (method.includes('wallet')) {
    const total = getPayableAmount();
    const bal = walletBalances[selectedWalletKey] || 0;
    if (bal < total) {
      showToast('⚠ Insufficient wallet balance! Please recharge or choose another method.', 'error');
      checkWalletBalance();
      return;
    }
    deductWalletBalance(total);
  }
  if (_origProcessPayment) _origProcessPayment();
};

// Draw QR on payment page open
const _origShowPage = showPage;
window.showPage = function(page) {
  _origShowPage(page);
  if (page === 'payment') setTimeout(() => { drawUpiQr(); checkWalletBalance(); }, 200);
};

// Draw initial QR when loaded
setTimeout(drawUpiQr, 500);


showPage('main');
</script>
</body>
</html>
