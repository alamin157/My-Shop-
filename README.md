# style.css
@import url("https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;500;700&display=swap");
:root{--bg:#F3F5F2;--sf:#fff;--ink:#17202E;--mute:#5E6A7A;--line:#D9DED8;--ac:#3B3FD8;--on:#fff;--tile:#E4EAE1}
@media(prefers-color-scheme:dark){:root{--bg:#10151D;--sf:#18202B;--ink:#EEF1F5;--mute:#9AA6B6;--line:#2A3442;--ac:#8E91FF;--on:#10151D;--tile:#1F2A27}}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font:400 16px/1.6 "Hind Siliguri",system-ui,sans-serif}
button{font:inherit;color:inherit;cursor:pointer}
:focus-visible{outline:2px solid var(--ac);outline-offset:2px}
header{position:sticky;top:0;z-index:5;display:flex;align-items:center;justify-content:space-between;padding:10px 20px;background:var(--bg);border-bottom:1px solid var(--line)}
.brand{display:flex;gap:10px;align-items:center;font-weight:700;font-size:20px}
.brand svg{width:34px;height:34px}
.bag{background:var(--ink);color:var(--bg);border:0;border-radius:99px;padding:6px 16px}
.hero{max-width:1100px;margin:auto;padding:44px 20px 24px}
.hero h1{font-size:clamp(34px,8vw,68px);line-height:1.1;margin:0;max-width:14ch}
.hero p{color:var(--mute);max-width:48ch}
.filters{display:flex;gap:8px;flex-wrap:wrap;max-width:1100px;margin:auto;padding:0 20px 18px}
.chip{border:1px solid var(--line);background:none;border-radius:99px;padding:4px 14px}
.chip[aria-pressed=true]{background:var(--ink);color:var(--bg);border-color:var(--ink)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:16px;max-width:1100px;margin:auto;padding:0 20px 50px}
.tile{background:var(--tile);border-radius:6px;aspect-ratio:4/5;display:grid;place-items:center;overflow:hidden}
.tile img{width:100%;height:100%;object-fit:cover}
.tile svg{width:55%}
.card h3{font-size:17px;margin:8px 0 0}
.meta{display:flex;justify-content:space-between;color:var(--mute)}
.sizes{display:flex;gap:6px;margin-top:6px}
.sizes button{flex:1;border:1px solid var(--line);background:none;border-radius:4px;padding:2px 0}
.sizes button[aria-pressed=true]{border-color:var(--ac);color:var(--ac);font-weight:700}
.add{margin-top:8px;width:100%;border:1px solid var(--ink);background:none;border-radius:6px;padding:7px}
.add:disabled{opacity:.4}
footer{border-top:1px solid var(--line);padding:24px;text-align:center;color:var(--mute);font-size:14px}
.scrim{position:fixed;inset:0;background:#0007;display:none;z-index:9}
.scrim.on{display:block}
.panel{position:fixed;top:0;right:0;bottom:0;width:min(440px,100%);background:var(--sf);z-index:10;transform:translateX(100%);transition:transform .25s;display:flex;flex-direction:column;padding:16px 20px}
.panel.on{transform:none}
.top{display:flex;justify-content:space-between;align-items:center}
.top h2{margin:0}
.x{border:0;background:none;font-size:26px}
.lines{flex:1;overflow:auto}
.line{display:flex;gap:10px;align-items:center;padding:10px 0;border-bottom:1px solid var(--line)}
.th{width:50px;height:50px;background:var(--tile);border-radius:4px;display:grid;place-items:center;overflow:hidden}
.th img{width:100%;height:100%;object-fit:cover}
.th svg{width:60%}
.in{flex:1;font-size:15px}
.q button{width:26px;height:26px;border:1px solid var(--line);background:none;border-radius:4px}
.tot{display:flex;justify-content:space-between;padding:6px 0}
.tot.b{font-weight:700;font-size:18px}
.primary{background:var(--ac);color:var(--on);border:0;border-radius:6px;padding:12px;width:100%;font-weight:700}
.primary:disabled{opacity:.5}
label{display:block;font-size:14px;color:var(--mute);margin-top:10px}
input[type=text],input[type=tel]{width:100%;padding:9px;border:1px solid var(--line);border-radius:6px;background:var(--bg);color:var(--ink);font:inherit}
.opt{display:flex;gap:8px;align-items:center;color:var(--ink);font-size:16px;margin-top:6px}
.note{background:var(--tile);padding:10px;border-radius:6px;font-size:14px;margin-top:8px}
.err{color:#C0392B;min-height:22px;font-size:14px}
.empty{color:var(--mute);padding:20px 0}