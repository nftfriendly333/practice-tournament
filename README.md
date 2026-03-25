<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Trade Together</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700&family=Crimson+Text:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">
<style>
  :root {
    --gold:#f0c040;--gold-dark:#b8860b;--gold-light:#ffd966;
    --brown-dark:#3a2410;--red:#c0392b;--green:#27ae60;
    --bg:#1a0f05;--panel:#2d1a0a;--panel-light:#3d2510;
    --border:#6b4a22;--text:#f5e6c8;--text-dim:#c9a96e;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{background:var(--bg);color:var(--text);font-family:'Crimson Text',serif;min-height:100vh;
    background-image:radial-gradient(ellipse at 20% 0%,rgba(100,60,10,.3) 0%,transparent 60%),
    radial-gradient(ellipse at 80% 100%,rgba(80,40,5,.3) 0%,transparent 60%)}

  /* HEADER */
  .header{background:linear-gradient(180deg,#3d2208,#1e0f04);border-bottom:3px solid var(--gold-dark);
    padding:10px 16px;text-align:center;position:sticky;top:0;z-index:100;box-shadow:0 4px 20px rgba(0,0,0,.7)}
  .header-title{font-family:'Cinzel',serif;font-size:1.4rem;font-weight:700;color:var(--gold);letter-spacing:3px;
    text-shadow:0 0 20px rgba(240,192,64,.5),2px 2px 4px rgba(0,0,0,.8);line-height:1}
  .header-subtitle{font-size:.68rem;color:var(--text-dim);letter-spacing:2px;text-transform:uppercase;margin-top:2px}
  .wallet{display:inline-flex;align-items:center;gap:6px;background:linear-gradient(135deg,#3d2208,#2a1505);
    border:2px solid var(--gold-dark);border-radius:20px;padding:5px 14px;margin-top:7px;
    font-family:'Cinzel',serif;font-size:.88rem;color:var(--gold);
    box-shadow:inset 0 1px 0 rgba(255,215,0,.1),0 2px 8px rgba(0,0,0,.5)}
  .wallet-value{font-weight:700}

  /* TABS */
  .tabs{display:flex;background:var(--brown-dark);border-bottom:2px solid var(--gold-dark)}
  .tab{flex:1;padding:9px 2px;text-align:center;font-family:'Cinzel',serif;font-size:.7rem;letter-spacing:.5px;
    color:var(--text-dim);cursor:pointer;border-right:1px solid var(--border);transition:all .2s;
    text-transform:uppercase;user-select:none}
  .tab:last-child{border-right:none}
  .tab.active{background:linear-gradient(180deg,var(--panel-light),var(--panel));color:var(--gold);
    border-bottom:2px solid var(--gold);margin-bottom:-2px}
  .tab:hover:not(.active){background:rgba(255,200,60,.05);color:var(--text)}

  .main{padding:10px 12px;max-width:480px;margin:0 auto}
  .market-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px}
  .section-title{font-family:'Cinzel',serif;font-size:.92rem;color:var(--gold);letter-spacing:2px;text-transform:uppercase}
  .tick-indicator{display:flex;align-items:center;gap:5px;font-size:.76rem;color:var(--text-dim)}
  .tick-dot{width:7px;height:7px;border-radius:50%;background:var(--green);animation:pulse 1.5s ease-in-out infinite}
  @keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.4;transform:scale(.8)}}

  /* ITEM CARDS */
  .item-card{background:linear-gradient(135deg,var(--panel-light),var(--panel));border:2px solid var(--border);
    border-radius:10px;margin-bottom:12px;overflow:hidden;
    box-shadow:0 4px 16px rgba(0,0,0,.4),inset 0 1px 0 rgba(255,200,60,.08);transition:border-color .4s}
  .item-card.flashing-up{border-color:var(--green)!important}
  .item-card.flashing-down{border-color:var(--red)!important}
  .item-card-header{display:flex;align-items:center;gap:10px;padding:10px 12px 8px;border-bottom:1px solid rgba(107,74,34,.4)}
  .item-icon{width:40px;height:40px;background:linear-gradient(135deg,#4a2e10,#2a1808);border:2px solid var(--gold-dark);
    border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:1.35rem;flex-shrink:0}
  .item-info{flex:1;min-width:0}
  .item-name{font-family:'Cinzel',serif;font-size:.92rem;font-weight:600;color:var(--gold-light);
    white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .item-desc{font-size:.78rem;color:var(--text-dim);font-style:italic;margin-top:1px}
  .item-price-block{text-align:right;flex-shrink:0}
  .item-price{font-family:'Cinzel',serif;font-size:1.15rem;font-weight:700;color:var(--gold);line-height:1;transition:color .3s}
  .item-price.up{color:#5deb8f}.item-price.down{color:#ff6b6b}
  .item-change{font-size:.76rem;margin-top:2px;font-weight:600}
  .item-change.pos{color:var(--green)}.item-change.neg{color:var(--red)}
  .chart-wrap{padding:6px 12px 5px;border-bottom:1px solid rgba(107,74,34,.4);position:relative}
  canvas.price-chart{width:100%;height:180px;display:block;border-radius:4px}
  .chart-ohlc-bar{font-size:.62rem;color:var(--text-dim);padding:4px 2px 2px;font-family:monospace;
    display:flex;gap:8px;flex-wrap:wrap}
  .chart-ohlc-bar span{font-weight:700}

  .open-pos-bar{display:flex;align-items:center;flex-wrap:wrap;gap:4px;padding:5px 12px;
    border-bottom:1px solid rgba(107,74,34,.3);background:rgba(0,0,0,.15)}
  .pos-badge{font-family:'Cinzel',serif;font-size:.74rem;font-weight:700;padding:2px 8px;border-radius:10px;letter-spacing:.5px}
  .pos-badge.long{background:rgba(39,174,96,.2);border:1px solid rgba(39,174,96,.5);color:#7fffc4}
  .pos-badge.short{background:rgba(192,57,43,.2);border:1px solid rgba(192,57,43,.5);color:#ffb3b3}
  .pos-pnl{font-family:'Cinzel',serif;font-size:.78rem;font-weight:700;margin-left:auto}
  .pos-pnl.pos{color:var(--green)}.pos-pnl.neg{color:var(--red)}

  .btn-trade{display:block;width:calc(100% - 24px);margin:8px 12px;
    background:linear-gradient(135deg,#3a2408,#251505);border:1.5px solid var(--gold-dark);
    border-radius:7px;padding:9px;font-family:'Cinzel',serif;font-size:.82rem;font-weight:600;
    color:var(--gold);cursor:pointer;transition:all .15s;text-align:center;letter-spacing:1px;text-transform:uppercase}
  .btn-trade:hover{background:linear-gradient(135deg,#4a2e10,#2d1a08);box-shadow:0 0 10px rgba(240,192,64,.15)}
  .btn-trade:active{transform:scale(.97)}

  /* PORTFOLIO */
  .portfolio-summary{background:linear-gradient(135deg,#3d2208,#2a1505);border:2px solid var(--gold-dark);
    border-radius:10px;padding:14px;margin-bottom:12px;text-align:center;position:relative;overflow:hidden}
  .portfolio-summary::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;
    background:linear-gradient(90deg,transparent,var(--gold),transparent)}
  .portfolio-total-label{font-size:.76rem;color:var(--text-dim);letter-spacing:2px;text-transform:uppercase;margin-bottom:4px}
  .portfolio-total{font-family:'Cinzel',serif;font-size:1.8rem;font-weight:700;color:var(--gold);
    text-shadow:0 0 20px rgba(240,192,64,.3);line-height:1}
  .portfolio-pl{font-size:.9rem;margin-top:5px;font-weight:600}
  .portfolio-pl.pos{color:var(--green)}.portfolio-pl.neg{color:var(--red)}
  .portfolio-breakdown{display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-top:10px}
  .portfolio-stat{background:rgba(0,0,0,.2);border-radius:6px;padding:7px;text-align:center}
  .portfolio-stat-label{font-size:.7rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px}
  .portfolio-stat-value{font-family:'Cinzel',serif;font-size:.95rem;color:var(--gold-light);font-weight:600;margin-top:2px}

  /* POSITION CARDS */
  .position-card{background:linear-gradient(135deg,var(--panel-light),var(--panel));border:1.5px solid var(--border);
    border-radius:8px;padding:11px 12px;margin-bottom:8px}
  .position-card.long-card{border-left:3px solid var(--green)}
  .position-card.short-card{border-left:3px solid var(--red)}
  .pos-card-top{display:flex;align-items:center;gap:8px;margin-bottom:7px}
  .pos-icon{font-size:1.3rem;flex-shrink:0}
  .pos-card-name{font-family:'Cinzel',serif;font-size:.86rem;color:var(--gold-light);font-weight:600;flex:1}
  .pos-card-pnl{font-family:'Cinzel',serif;font-size:1rem;font-weight:700}
  .pos-card-pnl.pos{color:var(--green)}.pos-card-pnl.neg{color:var(--red)}
  .pos-card-details{display:grid;grid-template-columns:repeat(3,1fr);gap:4px;font-size:.76rem;color:var(--text-dim);margin-bottom:4px}
  .pos-card-detail span{color:var(--text);font-weight:600}
  .pos-card-orders{display:flex;gap:6px;margin-top:6px}
  .order-tag{padding:3px 8px;border-radius:4px;font-family:'Cinzel',serif;font-size:.72rem;letter-spacing:.5px}
  .order-tag.sl{background:rgba(192,57,43,.15);border:1px solid rgba(192,57,43,.4);color:#ff9999}
  .order-tag.tp{background:rgba(39,174,96,.15);border:1px solid rgba(39,174,96,.4);color:#99ffcc}
  .order-tag.none{background:rgba(107,74,34,.2);border:1px solid rgba(107,74,34,.4);color:var(--text-dim)}
  .btn-close-card{margin-top:8px;width:100%;background:rgba(0,0,0,.2);border:1px solid var(--border);
    border-radius:5px;padding:7px;font-family:'Cinzel',serif;font-size:.76rem;color:var(--text-dim);
    cursor:pointer;transition:all .15s;text-transform:uppercase;letter-spacing:1px}
  .btn-close-card:hover{border-color:var(--gold-dark);color:var(--gold)}
  .empty-holdings{text-align:center;padding:28px 20px;color:var(--text-dim);font-style:italic;font-size:.96rem}

  /* CHARTS PAGE */
  .chart-toggle-row{display:flex;align-items:center;gap:10px;margin-bottom:12px;flex-wrap:wrap}
  .chart-toggle-label{font-family:'Cinzel',serif;font-size:.7rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px}
  .chart-toggle-btns{display:flex;gap:5px}
  .chart-tog-btn{background:var(--panel-light);border:1.5px solid var(--border);border-radius:6px;
    padding:5px 12px;font-family:'Cinzel',serif;font-size:.7rem;color:var(--text-dim);cursor:pointer;
    transition:all .15s;letter-spacing:.5px}
  .chart-tog-btn.active{background:linear-gradient(135deg,#4a2e10,#2d1a08);border-color:var(--gold);color:var(--gold);
    box-shadow:0 0 8px rgba(240,192,64,.2)}
  .chart-tog-btn:hover:not(.active){border-color:var(--gold-dark);color:var(--text)}
  .ma-legend{display:flex;align-items:center;gap:5px;font-size:.68rem;color:var(--text-dim);margin-left:auto}
  .ma-dot{width:16px;height:2px;background:var(--gold);border-radius:1px;
    background:repeating-linear-gradient(90deg,var(--gold) 0,var(--gold) 4px,transparent 4px,transparent 7px)}
  .chart-full-card{background:linear-gradient(135deg,var(--panel-light),var(--panel));border:2px solid var(--border);
    border-radius:10px;margin-bottom:14px;overflow:hidden}
  .chart-full-header{display:flex;align-items:center;gap:10px;padding:10px 12px 7px;border-bottom:1px solid rgba(107,74,34,.4)}
  .chart-full-icon{font-size:1.2rem}
  .chart-full-name{font-family:'Cinzel',serif;font-size:.82rem;color:var(--gold-light);font-weight:600;flex:1}
  .chart-full-price{font-family:'Cinzel',serif;font-size:.95rem;color:var(--gold);font-weight:700;text-align:right}
  .chart-ohlc{font-size:.62rem;color:var(--text-dim);margin-top:2px;text-align:right;font-family:monospace}
  .chart-ohlc span{font-weight:700}
  .chart-full-canvas{width:100%;height:200px;display:block;padding:0}
  .chart-stats{display:grid;grid-template-columns:repeat(4,1fr);border-top:1px solid rgba(107,74,34,.4)}
  .chart-stat{padding:6px;text-align:center;border-right:1px solid rgba(107,74,34,.4)}
  .chart-stat:last-child{border-right:none}
  .chart-stat-label{font-size:.58rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:.8px}
  .chart-stat-val{font-family:'Cinzel',serif;font-size:.75rem;color:var(--text);font-weight:600;margin-top:2px}


  .btn-reset-funds{display:block;width:100%;background:linear-gradient(135deg,#3a1a05,#2a0f03);
    border:2px solid var(--gold-dark);border-radius:8px;padding:12px;
    font-family:'Cinzel',serif;font-size:.82rem;font-weight:700;color:var(--gold);
    cursor:pointer;transition:all .15s;text-align:center;letter-spacing:1px;text-transform:uppercase}
  .btn-reset-funds:hover{background:linear-gradient(135deg,#4a2e10,#2d1a08);box-shadow:0 0 12px rgba(240,192,64,.2)}
  .btn-reset-funds:active{transform:scale(.97)}

  /* HISTORY */
  .hist-stats-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:7px;margin-bottom:14px}
  .hist-stat-card{background:linear-gradient(135deg,var(--panel-light),var(--panel));border:1.5px solid var(--border);
    border-radius:9px;padding:10px 12px;text-align:center}
  .hist-stat-card.wide{grid-column:span 2}
  .hist-stat-label{font-size:.62rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1.2px;margin-bottom:3px}
  .hist-stat-value{font-family:'Cinzel',serif;font-size:1.1rem;font-weight:700;color:var(--gold);line-height:1}
  .hist-stat-value.pos{color:var(--green)}.hist-stat-value.neg{color:var(--red)}.hist-stat-value.neutral{color:var(--text)}
  .hist-stat-sub{font-size:.65rem;color:var(--text-dim);margin-top:3px}
  .win-rate-bar-wrap{margin-top:6px;height:6px;background:rgba(192,57,43,.3);border-radius:3px;overflow:hidden}
  .win-rate-bar-fill{height:100%;background:linear-gradient(90deg,var(--green),#5deb8f);border-radius:3px;transition:width .4s ease}
  .hist-filter-row{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;gap:8px}
  .hist-filters{display:flex;gap:6px}
  .hist-select{background:var(--panel-light);border:1.5px solid var(--border);border-radius:6px;padding:5px 22px 5px 8px;
    font-family:'Crimson Text',serif;font-size:.78rem;color:var(--text);outline:none;cursor:pointer;
    appearance:none;-webkit-appearance:none;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%23c9a96e'/%3E%3C/svg%3E");
    background-repeat:no-repeat;background-position:right 7px center}
  .hist-select:focus{border-color:var(--gold-dark)}
  .hist-entry{background:linear-gradient(135deg,var(--panel-light),var(--panel));border:1.5px solid var(--border);
    border-radius:8px;padding:10px 12px;margin-bottom:7px;display:flex;align-items:flex-start;gap:10px;
    animation:slideIn .25s ease}
  @keyframes slideIn{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}
  .hist-entry.win{border-left:3px solid var(--green)}
  .hist-entry.loss{border-left:3px solid var(--red)}
  .hist-entry.liq{border-left:3px solid #f60}
  .hist-icon{font-size:1.3rem;flex-shrink:0;margin-top:1px}
  .hist-body{flex:1;min-width:0}
  .hist-top{display:flex;align-items:center;gap:6px;margin-bottom:4px;flex-wrap:wrap}
  .hist-item-name{font-family:'Cinzel',serif;font-size:.84rem;color:var(--gold-light);font-weight:600;
    flex:1;min-width:0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .hist-dir-badge{font-family:'Cinzel',serif;font-size:.7rem;font-weight:700;padding:2px 7px;border-radius:8px;flex-shrink:0}
  .hist-dir-badge.long{background:rgba(39,174,96,.18);border:1px solid rgba(39,174,96,.45);color:#7fffc4}
  .hist-dir-badge.short{background:rgba(192,57,43,.18);border:1px solid rgba(192,57,43,.45);color:#ffb3b3}
  .hist-close-tag{font-size:.68rem;padding:2px 7px;border-radius:8px;flex-shrink:0;font-weight:600;letter-spacing:.5px}
  .hist-close-tag.manual{background:rgba(107,74,34,.3);border:1px solid rgba(107,74,34,.5);color:var(--text-dim)}
  .hist-close-tag.sl{background:rgba(192,57,43,.18);border:1px solid rgba(192,57,43,.4);color:#ff9999}
  .hist-close-tag.tp{background:rgba(39,174,96,.18);border:1px solid rgba(39,174,96,.4);color:#99ffcc}
  .hist-close-tag.liq{background:rgba(255,102,0,.18);border:1px solid rgba(255,102,0,.4);color:#ffcc88}
  .hist-details{display:flex;gap:10px;font-size:.76rem;color:var(--text-dim);flex-wrap:wrap}
  .hist-details span{white-space:nowrap}
  .hist-details .val{color:var(--text);font-weight:600}
  .hist-pnl{font-family:'Cinzel',serif;font-size:1rem;font-weight:700;flex-shrink:0;text-align:right;line-height:1;margin-top:2px}
  .hist-pnl.pos{color:var(--green)}.hist-pnl.neg{color:var(--red)}
  .hist-pnl-pct{font-size:.7rem;margin-top:3px;text-align:right}
  .hist-pnl-pct.pos{color:#5deb8f}.hist-pnl-pct.neg{color:#ff8888}
  .hist-time{font-size:.68rem;color:var(--text-dim);margin-top:4px;font-style:italic}
  .hist-empty{text-align:center;padding:30px 20px;color:var(--text-dim);font-style:italic;font-size:.96rem}
  .hist-empty-icon{font-size:2rem;margin-bottom:8px;opacity:.5}

  /* SL/TP PERCENTAGE PRESETS */
  .sltp-preset{background:var(--panel);border:1px solid var(--border);border-radius:5px;
    padding:3px 0;font-family:'Cinzel',serif;font-size:.65rem;color:var(--text-dim);
    cursor:pointer;transition:all .15s;flex:1;text-align:center}
  .sl-preset:hover,.sl-preset.active{background:rgba(192,57,43,.2);border-color:#c0392b;color:#ff9999}
  .tp-preset:hover,.tp-preset.active{background:rgba(39,174,96,.2);border-color:#27ae60;color:#99ffcc}
  .sltp-suffix{position:absolute;right:10px;top:50%;transform:translateY(-50%);
    font-family:'Cinzel',serif;font-size:.82rem;color:var(--text-dim);pointer-events:none}
  .sltp-price-hint{font-size:.68rem;color:var(--text-dim);margin-top:3px;min-height:14px;font-style:italic}
  .sltp-price-hint.sl-hint{color:#ff9999}
  .sltp-price-hint.tp-hint{color:#99ffcc}

  /* LOT SIZE */
  .modal-lot-row{display:flex;align-items:center;gap:8px;margin-bottom:14px}
  .lot-btn{width:36px;height:36px;background:linear-gradient(135deg,#4a2e10,#2d1a08);border:1.5px solid var(--gold-dark);
    border-radius:7px;color:var(--gold);font-size:1.2rem;font-weight:700;cursor:pointer;
    display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:all .15s;line-height:1}
  .lot-btn:hover{background:linear-gradient(135deg,#5a3a14,#3a2010);box-shadow:0 0 8px rgba(240,192,64,.2)}
  .lot-btn:active{transform:scale(.92)}
  .lot-input{flex:1;text-align:center;font-family:'Cinzel',serif;font-weight:600}
  .lot-label{font-family:'Cinzel',serif;font-size:.75rem;color:var(--text-dim);flex-shrink:0}

  /* WARNING POPUP */
  .warning-overlay{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:500;
    display:flex;align-items:center;justify-content:center;padding:20px;
    opacity:0;pointer-events:none;transition:opacity .2s}
  .warning-overlay.open{opacity:1;pointer-events:all}
  .warning-box{background:linear-gradient(135deg,#3d1208,#2a0a05);border:3px solid #c0392b;
    border-radius:14px;padding:28px 24px;max-width:320px;width:100%;text-align:center;
    box-shadow:0 0 40px rgba(192,57,43,.4),0 8px 32px rgba(0,0,0,.8);
    transform:scale(.85);transition:transform .25s cubic-bezier(.34,1.4,.64,1)}
  .warning-overlay.open .warning-box{transform:scale(1)}
  .warning-emoji{font-size:3rem;margin-bottom:10px;animation:wobble .5s ease .1s}
  @keyframes wobble{0%,100%{transform:rotate(0)}25%{transform:rotate(-10deg)}75%{transform:rotate(10deg)}}
  .warning-title{font-family:'Cinzel',serif;font-size:1.15rem;font-weight:700;color:#ff6b6b;
    letter-spacing:1px;margin-bottom:8px;text-shadow:0 0 12px rgba(255,107,107,.4)}
  .warning-body{font-size:.88rem;color:var(--text-dim);margin-bottom:18px;line-height:1.5}
  .warning-pct{font-family:'Cinzel',serif;font-size:1.4rem;font-weight:700;color:#ff4444;margin-bottom:4px}
  .warning-btns{display:flex;gap:8px}
  .warning-btn-cancel{flex:1;background:rgba(0,0,0,.3);border:1.5px solid var(--border);border-radius:8px;
    padding:10px;font-family:'Cinzel',serif;font-size:.75rem;color:var(--text-dim);cursor:pointer;
    transition:all .15s;text-transform:uppercase;letter-spacing:1px}
  .warning-btn-cancel:hover{border-color:var(--text-dim);color:var(--text)}
  .warning-btn-confirm{flex:1;background:linear-gradient(135deg,#6b1a1a,#4d0f0f);border:2px solid #c0392b;
    border-radius:8px;padding:10px;font-family:'Cinzel',serif;font-size:.75rem;font-weight:700;
    color:#ffb3b3;cursor:pointer;transition:all .15s;text-transform:uppercase;letter-spacing:1px}
  .warning-btn-confirm:hover{background:linear-gradient(135deg,#7d2020,#5c1212);box-shadow:0 0 10px rgba(192,57,43,.3)}

  /* BREAKING NEWS BANNER */
  .breaking-news{position:fixed;top:0;left:0;right:0;z-index:600;transform:translateY(-100%);
    transition:transform .4s cubic-bezier(.34,1.2,.64,1)}
  .breaking-news.show{transform:translateY(0)}
  .bn-bar{background:linear-gradient(135deg,#8b0000,#c0392b,#8b0000);
    border-bottom:3px solid #ff4444;padding:0;overflow:hidden;position:relative}
  .bn-bar::before{content:'';position:absolute;inset:0;
    background:repeating-linear-gradient(90deg,transparent,transparent 40px,rgba(255,255,255,.04) 40px,rgba(255,255,255,.04) 41px);
    pointer-events:none}
  .bn-inner{display:flex;align-items:stretch}
  .bn-label{background:#ff0000;color:#fff;font-family:'Cinzel',serif;font-size:.72rem;font-weight:700;
    letter-spacing:1px;padding:0 10px;text-transform:uppercase;flex-shrink:0;
    display:flex;align-items:center;writing-mode:initial;
    animation:bnLabelPulse 1s ease-in-out infinite;min-width:52px;justify-content:center}
  @keyframes bnLabelPulse{0%,100%{background:#ff0000}50%{background:#cc0000}}
  .bn-content{flex:1;padding:6px 10px;overflow:hidden;min-width:0}
  .bn-headline{font-family:'Cinzel',serif;font-size:.82rem;color:#fff;font-weight:700;
    letter-spacing:.3px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;line-height:1.2}
  .bn-meta{display:flex;align-items:center;gap:8px;margin-top:3px;flex-wrap:nowrap}
  .bn-sub{font-size:.66rem;color:rgba(255,220,220,.8);font-style:italic;
    white-space:nowrap;overflow:hidden;text-overflow:ellipsis;flex:1;min-width:0}
  .bn-countdown{font-family:'Cinzel',serif;font-size:.68rem;color:#fff;font-weight:700;
    white-space:nowrap;flex-shrink:0;background:rgba(0,0,0,.25);
    padding:2px 7px;border-radius:8px;letter-spacing:.5px}
  .bn-close{background:rgba(0,0,0,.3);border:none;color:rgba(255,255,255,.7);font-size:1rem;
    padding:0 10px;cursor:pointer;flex-shrink:0;transition:color .15s;min-width:32px}
  .bn-close:hover{color:#fff}

  /* CHART STROBE */
  .chart-wrap{position:relative}
  .strobe-overlay{position:absolute;inset:0;border-radius:4px;pointer-events:none;opacity:0;
    transition:opacity .08s;z-index:2}
  .strobe-overlay.flash-green{background:radial-gradient(ellipse at center,rgba(39,174,96,.35) 0%,transparent 70%)}
  .strobe-overlay.flash-red{background:radial-gradient(ellipse at center,rgba(192,57,43,.35) 0%,transparent 70%)}
  @keyframes strobeGlow{0%{box-shadow:0 0 0 0 transparent}
    50%{box-shadow:0 0 20px 4px rgba(240,192,64,.3)}
    100%{box-shadow:0 0 0 0 transparent}}
  .item-card.strobe-active{animation:strobeGlow .6s ease-out}

  /* CLOSE ALL BUTTON */
  .btn-close-all{display:block;width:100%;margin:12px 0 4px;
    background:linear-gradient(135deg,#5c1a1a,#3d0f0f);border:2px solid #c0392b;
    border-radius:8px;padding:11px;font-family:'Cinzel',serif;font-size:.8rem;font-weight:700;
    color:#ffb3b3;cursor:pointer;transition:all .15s;text-align:center;letter-spacing:1px;text-transform:uppercase}
  .btn-close-all:hover{background:linear-gradient(135deg,#6b2020,#4d1212);box-shadow:0 0 14px rgba(192,57,43,.35)}
  .btn-close-all:active{transform:scale(.97)}

  /* MINI CLOSE BUTTON ON MARKET CARD */
  .market-close-row{display:flex;gap:6px;padding:6px 12px 8px;align-items:center}
  .market-close-row .btn-trade{margin:0;flex:1;padding:8px 4px;font-size:.78rem}
  .btn-close-item{flex-shrink:0;background:rgba(192,57,43,.12);border:1px solid rgba(192,57,43,.4);
    border-radius:6px;padding:8px 16px;font-family:'Cinzel',serif;font-size:.74rem;font-weight:600;
    color:#ffb3b3;cursor:pointer;transition:all .15s;text-align:center;letter-spacing:.5px;white-space:nowrap}
  .btn-close-item:hover{background:rgba(192,57,43,.25);border-color:#c0392b}
  .btn-close-item:active{transform:scale(.96)}
  .btn-close-item:disabled{opacity:.25;cursor:not-allowed}

  /* TREND SHIFT INDICATOR on cards */
  .trend-badge{display:inline-flex;align-items:center;gap:3px;font-size:.7rem;
    padding:1px 6px;border-radius:8px;font-weight:700;letter-spacing:.5px}
  .trend-badge.bull{background:rgba(39,174,96,.15);border:1px solid rgba(39,174,96,.4);color:#7fffc4}
  .trend-badge.bear{background:rgba(192,57,43,.15);border:1px solid rgba(192,57,43,.4);color:#ffb3b3}
  .trend-badge.flat{background:rgba(107,74,34,.2);border:1px solid rgba(107,74,34,.4);color:var(--text-dim)}

  /* CHART LEGEND */
  .chart-legend-bar{background:rgba(0,0,0,.2);border:1px solid rgba(107,74,34,.3);border-radius:8px;
    padding:10px 12px;margin-bottom:12px;display:flex;flex-direction:column;gap:8px}
  .legend-item{display:flex;align-items:flex-start;gap:10px}
  .legend-ma-line{flex-shrink:0;width:28px;height:2px;margin-top:7px;
    background:repeating-linear-gradient(90deg,#f0c040 0,#f0c040 5px,transparent 5px,transparent 9px);
    border-radius:1px}
  .legend-session-line{flex-shrink:0;width:2px;height:28px;margin-top:2px;
    background:rgba(240,192,64,.6)}
  .legend-liq-line{flex-shrink:0;width:28px;height:2px;margin-top:7px;background:#ff4500}
  .legend-desc{font-size:.72rem;color:var(--text-dim);line-height:1.4;display:block}


  .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.72);z-index:300;
    display:flex;align-items:flex-start;justify-content:center;
    padding:0 12px;overflow-y:auto;
    opacity:0;pointer-events:none;transition:opacity .2s}
  .modal-overlay.open{opacity:1;pointer-events:all}
  .modal{background:linear-gradient(180deg,#3d2510,#2d1a0a);border:2px solid var(--gold-dark);
    border-radius:14px;width:100%;max-width:460px;padding:16px 16px 20px;
    transform:scale(.94) translateY(-8px);
    transition:transform .25s cubic-bezier(.34,1.2,.64,1),opacity .2s;
    box-shadow:0 8px 40px rgba(0,0,0,.7);margin-top:var(--modal-top,80px)}
  .modal-overlay.open .modal{transform:scale(1) translateY(0)}
  .modal-handle{display:none}
  .modal-title{font-family:'Cinzel',serif;font-size:1.1rem;color:var(--gold);text-align:center;margin-bottom:4px;letter-spacing:1px}
  .modal-price-display{text-align:center;font-size:.82rem;color:var(--text-dim);margin-bottom:12px}
  .modal-price-display strong{font-family:'Cinzel',serif;color:var(--gold);font-size:1.05rem}
  .modal-section-label{font-size:.74rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:6px}
  .modal-lev-row{display:flex;gap:5px;margin-bottom:12px}
  .modal-lev-btn{flex:1;background:var(--panel);border:1.5px solid var(--border);border-radius:7px;padding:8px 2px;
    font-family:'Cinzel',serif;font-size:.84rem;font-weight:600;color:var(--text-dim);cursor:pointer;text-align:center;transition:all .15s}
  .modal-lev-btn.active{background:linear-gradient(135deg,#4a2e10,#2d1a08);border-color:var(--gold);color:var(--gold);box-shadow:0 0 8px rgba(240,192,64,.2)}
  .modal-lev-btn:hover:not(.active){border-color:var(--gold-dark);color:var(--text)}
  .modal-sltp-row{display:flex;gap:8px;margin-bottom:12px}
  .modal-field{flex:1}
  .modal-field label{display:block;font-size:.74rem;text-transform:uppercase;letter-spacing:1px;margin-bottom:4px}
  .modal-field label.sl{color:#ff9999}.modal-field label.tp{color:#99ffcc}
  .modal-input{width:100%;background:rgba(0,0,0,.35);border:1.5px solid var(--border);border-radius:7px;
    padding:8px 10px;color:var(--text);font-family:'Crimson Text',serif;font-size:1rem;outline:none;transition:border-color .2s}
  .modal-input:focus{border-color:var(--gold-dark)}
  .modal-input.sl:focus{border-color:#e74c3c}.modal-input.tp:focus{border-color:#2ecc71}
  .modal-input::placeholder{color:rgba(201,169,110,.35);font-style:italic}
  .modal-cost-info{background:rgba(0,0,0,.25);border:1px solid rgba(107,74,34,.3);border-radius:7px;
    padding:8px 12px;font-size:.8rem;color:var(--text-dim);margin-bottom:12px}
  .modal-cost-row{display:flex;justify-content:space-between;margin-bottom:2px}
  .modal-cost-row:last-child{margin-bottom:0}
  .modal-cost-row strong{color:var(--text);font-family:'Cinzel',serif;font-size:.84rem}
  .modal-dir-btns{display:flex;gap:8px}
  .modal-btn-long{flex:1;background:linear-gradient(135deg,#1a5c32,#0f3d20);border:2px solid #27ae60;border-radius:8px;
    padding:12px;font-family:'Cinzel',serif;font-size:.88rem;font-weight:700;color:#7fffc4;cursor:pointer;
    transition:all .15s;text-align:center;letter-spacing:.5px}
  .modal-btn-long:hover{background:linear-gradient(135deg,#1e6b3a,#124826);box-shadow:0 0 12px rgba(39,174,96,.3)}
  .modal-btn-long:active{transform:scale(.97)}
  .modal-btn-long:disabled{opacity:.3;cursor:not-allowed;transform:none}
  .modal-btn-short{flex:1;background:linear-gradient(135deg,#5c1a1a,#3d0f0f);border:2px solid #c0392b;border-radius:8px;
    padding:12px;font-family:'Cinzel',serif;font-size:.88rem;font-weight:700;color:#ffb3b3;cursor:pointer;
    transition:all .15s;text-align:center;letter-spacing:.5px}
  .modal-btn-short:hover{background:linear-gradient(135deg,#6b2020,#4d1212);box-shadow:0 0 12px rgba(192,57,43,.3)}
  .modal-btn-short:active{transform:scale(.97)}
  .modal-btn-short:disabled{opacity:.3;cursor:not-allowed;transform:none}
  .modal-cancel{width:100%;margin-top:8px;background:transparent;border:1px solid var(--border);border-radius:7px;
    padding:8px;font-family:'Cinzel',serif;font-size:.76rem;color:var(--text-dim);cursor:pointer;
    transition:all .15s;text-transform:uppercase;letter-spacing:1px}
  .modal-cancel:hover{color:var(--text);border-color:var(--text-dim)}

  /* TOAST */
  .toast{position:fixed;bottom:80px;left:50%;transform:translateX(-50%) translateY(20px);
    background:var(--panel-light);border:2px solid var(--gold-dark);border-radius:30px;padding:9px 20px;
    font-family:'Cinzel',serif;font-size:.76rem;color:var(--gold);opacity:0;pointer-events:none;
    transition:all .3s;z-index:400;box-shadow:0 4px 20px rgba(0,0,0,.6);max-width:88vw;text-align:center}
  .toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
  .toast.buy{border-color:var(--green);color:#7fffc4}
  .toast.sell{border-color:var(--red);color:#ffb3b3}
  .toast.error{border-color:var(--red);color:#ffb3b3}
  .toast.sl-hit{border-color:#ff6b6b;color:#ffaaaa}
  .toast.tp-hit{border-color:#6bff9e;color:#aaffcc}

  .page{display:none}.page.active{display:block}
  .app-hidden{display:none!important}
  ::-webkit-scrollbar{width:4px}
  ::-webkit-scrollbar-track{background:var(--bg)}
  ::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}

  /* COUNTDOWN GATE */
  #gate{position:fixed;inset:0;z-index:9000;background:#1a0f05;background-color:#1a0f05;
    background-image:radial-gradient(ellipse at 20% 0%,rgba(100,60,10,.35) 0%,transparent 60%),
    radial-gradient(ellipse at 80% 100%,rgba(80,40,5,.35) 0%,transparent 60%);
    display:flex;align-items:center;justify-content:center;padding:20px}
  #gate.hidden{display:none!important}
  .gate-box{text-align:center;max-width:360px;width:100%}
  .gate-logo{font-size:3rem;margin-bottom:10px;filter:drop-shadow(0 0 18px rgba(240,192,64,.4))}
  .gate-title{font-family:'Cinzel',serif;font-size:1.6rem;font-weight:700;color:var(--gold);letter-spacing:4px;text-shadow:0 0 24px rgba(240,192,64,.5);margin-bottom:4px}
  .gate-subtitle{font-size:.72rem;color:var(--text-dim);letter-spacing:2px;text-transform:uppercase;margin-bottom:24px}
  .gate-cd-ring{display:flex;align-items:center;justify-content:center;gap:6px;margin-bottom:8px}
  .gate-cd-num{background:linear-gradient(180deg,#3a2208,#251505);border:2px solid var(--gold-dark);border-radius:10px;min-width:68px;padding:12px 8px;font-family:'Cinzel',serif;font-size:2.4rem;font-weight:700;color:var(--gold);text-align:center;line-height:1;box-shadow:0 0 20px rgba(240,192,64,.12),inset 0 1px 0 rgba(255,215,0,.1)}
  .gate-cd-sep{font-family:'Cinzel',serif;font-size:2rem;font-weight:700;color:var(--gold-dark);margin-top:-8px}
  .gate-cd-labels{display:flex;justify-content:center;gap:6px;font-size:.62rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:6px}
  .gate-cd-labels span{min-width:68px;text-align:center}
  .gate-cd-date{font-size:.7rem;color:var(--text-dim);margin-bottom:14px;letter-spacing:.5px}
  .gate-cd-msg{font-size:.82rem;color:#7fffc4;margin-bottom:14px;min-height:20px}
  .gate-divider{border:none;border-top:1px solid rgba(107,74,34,.4);margin:16px 0}
  .gate-reg-note{font-size:.74rem;color:var(--text-dim);margin-bottom:12px}
  .gate-btn-row{display:flex;gap:10px}
  .gate-btn{flex:1;border-radius:8px;padding:11px;font-family:'Cinzel',serif;font-size:.82rem;font-weight:700;cursor:pointer;transition:all .15s;letter-spacing:.8px;text-transform:uppercase}
  .gate-btn-primary{background:linear-gradient(135deg,#4a2e10,#2d1a08);border:2px solid var(--gold-dark);color:var(--gold)}
  .gate-btn-primary:hover{background:linear-gradient(135deg,#5a3a14,#3a2010);box-shadow:0 0 12px rgba(240,192,64,.2)}
  .gate-btn-secondary{background:transparent;border:1.5px solid var(--border);color:var(--text-dim)}
  .gate-btn-secondary:hover{border-color:var(--gold-dark);color:var(--text)}
  .auth-overlay{position:fixed;inset:0;z-index:9100;background:rgba(0,0,0,.75);display:flex;align-items:center;justify-content:center;padding:20px;opacity:0;pointer-events:none;transition:opacity .2s}
  .auth-overlay.open{opacity:1;pointer-events:all}
  .auth-modal-box{background:linear-gradient(180deg,#3d2510,#2d1a0a);border:2px solid var(--gold-dark);border-radius:14px;padding:24px 22px 20px;max-width:320px;width:100%;position:relative;box-shadow:0 8px 48px rgba(0,0,0,.9);max-height:90vh;overflow-y:auto;transform:scale(.94);transition:transform .22s cubic-bezier(.34,1.2,.64,1)}
  .auth-overlay.open .auth-modal-box{transform:scale(1)}
  .auth-modal-close{position:absolute;top:10px;right:12px;background:transparent;border:none;color:var(--text-dim);font-size:1.1rem;cursor:pointer;padding:2px 6px;border-radius:4px;transition:color .15s}
  .auth-modal-close:hover{color:var(--gold)}
  .auth-modal-icon{font-size:1.8rem;margin-bottom:6px}
  .auth-modal-title{font-family:'Cinzel',serif;font-size:1.05rem;font-weight:700;color:var(--gold);letter-spacing:1.5px;margin-bottom:4px}
  .auth-modal-sub{font-size:.78rem;color:var(--text-dim);margin-bottom:14px;line-height:1.5}
  .auth-field-lbl{text-align:left;font-size:.66rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1.2px;margin:10px 0 4px}
  .auth-inp{width:100%;background:rgba(0,0,0,.4);border:1.5px solid var(--border);border-radius:7px;padding:10px 12px;color:var(--text);font-family:'Crimson Text',serif;font-size:.98rem;outline:none;transition:border-color .2s;display:block}
  .auth-inp:focus{border-color:var(--gold-dark)}
  .auth-err{font-size:.72rem;color:#ff9999;min-height:16px;text-align:left;margin:5px 0 4px;padding-left:2px;font-style:italic}
  .auth-submit-btn{width:100%;background:linear-gradient(135deg,#4a2e10,#2d1a08);border:2px solid var(--gold-dark);border-radius:8px;padding:10px;font-family:'Cinzel',serif;font-size:.84rem;font-weight:700;color:var(--gold);cursor:pointer;transition:all .15s;letter-spacing:.8px;text-transform:uppercase;margin-top:8px}
  .auth-submit-btn:hover{background:linear-gradient(135deg,#5a3a14,#3a2010);box-shadow:0 0 10px rgba(240,192,64,.18)}
  .auth-submit-btn:disabled{opacity:.4;cursor:not-allowed}
  .auth-lnk{font-size:.76rem;color:var(--gold);cursor:pointer;text-decoration:underline}
  .auth-lnk:hover{color:var(--gold-light)}
  .auth-switch-row{margin-top:10px;text-align:center;font-size:.76rem;color:var(--text-dim)}
  .live-table-wrap{overflow-x:auto;margin-bottom:12px;border-radius:8px;border:1.5px solid var(--border)}
  .live-table{width:100%;border-collapse:collapse;font-size:.74rem}
  .live-table th{background:linear-gradient(180deg,#3d2510,#2d1a0a);color:var(--gold);font-family:'Cinzel',serif;font-size:.62rem;letter-spacing:.8px;text-transform:uppercase;padding:7px 8px;border-bottom:1.5px solid var(--border);white-space:nowrap;text-align:left}
  .live-table td{padding:7px 8px;border-bottom:1px solid rgba(107,74,34,.25);vertical-align:middle;color:var(--text);white-space:nowrap}
  .live-table tr:last-child td{border-bottom:none}
  .live-table tr:hover td{background:rgba(255,200,60,.04)}
  .live-table tr.me-row td{background:rgba(39,174,96,.05)}
  .lt-name{font-family:'Cinzel',serif;font-size:.72rem;color:var(--gold-light);font-weight:600}
  .lt-wallet{font-size:.6rem;color:var(--text-dim);font-family:monospace}
  .lt-badge{font-family:'Cinzel',serif;font-size:.64rem;font-weight:700;padding:2px 6px;border-radius:6px}
  .lt-badge.long{background:rgba(39,174,96,.2);border:1px solid rgba(39,174,96,.45);color:#7fffc4}
  .lt-badge.short{background:rgba(192,57,43,.2);border:1px solid rgba(192,57,43,.45);color:#ffb3b3}
  .lt-pnl-pos{color:var(--green);font-weight:700;font-family:'Cinzel',serif}
  .lt-pnl-neg{color:var(--red);font-weight:700;font-family:'Cinzel',serif}
  .lb-table-wrap{overflow-x:auto;border-radius:8px;border:1.5px solid var(--border)}
  .lb-table{width:100%;border-collapse:collapse;font-size:.74rem}
  .lb-table th{background:linear-gradient(180deg,#3d2510,#2d1a0a);color:var(--gold);font-family:'Cinzel',serif;font-size:.62rem;letter-spacing:.8px;text-transform:uppercase;padding:7px 8px;border-bottom:1.5px solid var(--border);text-align:left}
  .lb-table td{padding:8px 8px;border-bottom:1px solid rgba(107,74,34,.25);vertical-align:middle;color:var(--text)}
  .lb-table tr:last-child td{border-bottom:none}
  .lb-table tr.me-row td{background:rgba(39,174,96,.05)}
  .lb-rank-cell{font-family:'Cinzel',serif;font-size:1.1rem;text-align:center;width:36px}
  .lb-uname{font-family:'Cinzel',serif;font-size:.76rem;color:var(--gold-light);font-weight:600}
  .lb-wallet-cell{font-size:.6rem;color:var(--text-dim);font-family:monospace}
  .lb-pnl-cell{font-family:'Cinzel',serif;font-size:.88rem;font-weight:700;text-align:right}
  .lb-pnl-cell.pos{color:var(--green)}.lb-pnl-cell.neg{color:var(--red)}
  .lb-wealth-cell{font-size:.72rem;color:var(--text-dim);text-align:right}
  .lb-you-tag{font-size:.58rem;background:rgba(93,235,143,.15);border:1px solid rgba(93,235,143,.35);color:#7fffc4;border-radius:6px;padding:1px 5px;margin-left:4px;vertical-align:middle}
</style>
</head>
<body>
<script>
// Clear all localStorage on every load for fresh tournament start
(function(){
  try{
    Object.keys(localStorage).forEach(function(k){
      if(k.startsWith('tradeTogether_v1')||k.startsWith('tt_'))
        localStorage.removeItem(k);
    });
  }catch(e){}
})();
</script>

<!-- COUNTDOWN GATE -->
<div id="gate">
  <div class="gate-box">
    <div class="gate-logo">&#9876;&#65039;</div>
    <div class="gate-title">Trade Together</div>
    <div class="gate-subtitle">Leveraged Commodity Exchange</div>
    <div class="gate-cd-ring">
      <div class="gate-cd-num" id="cd-h">00</div><div class="gate-cd-sep">:</div>
      <div class="gate-cd-num" id="cd-m">00</div><div class="gate-cd-sep">:</div>
      <div class="gate-cd-num" id="cd-s">00</div>
    </div>
    <div class="gate-cd-labels"><span>Hours</span><span>Mins</span><span>Secs</span></div>
    <div class="gate-cd-date" id="cd-date"></div>
    <div class="gate-cd-msg" id="cd-msg"></div>
    <hr class="gate-divider">
    <div class="gate-reg-note">Register now &#8212; be ready when the tournament opens.</div>
    <div class="gate-btn-row">
      <button class="gate-btn gate-btn-primary" onclick="openAuthModal('register')">Create Account</button>
      <button class="gate-btn gate-btn-secondary" onclick="openAuthModal('login')">Sign In</button>
    </div>
  </div>
</div>
<div class="auth-overlay" id="auth-overlay">
  <div class="auth-modal-box">
    <button class="auth-modal-close" onclick="closeAuthModal()">&#x2715;</button>
    <div id="auth-panel-login">
      <div class="auth-modal-icon">&#9876;&#65039;</div>
      <div class="auth-modal-title">Sign In</div>
      <div class="auth-modal-sub">Enter your credentials to access the exchange.</div>
      <div class="auth-field-lbl">Username</div>
      <input class="auth-inp" id="login-user" type="text" maxlength="20" placeholder="Trader name" autocomplete="off">
      <div class="auth-field-lbl">Password</div>
      <input class="auth-inp" id="login-pass" type="password" maxlength="64" placeholder="Password">
      <div class="auth-err" id="login-err"></div>
      <button class="auth-submit-btn" id="login-btn" onclick="doLogin()">Sign In &rarr;</button>
      <div class="auth-switch-row">No account? <span class="auth-lnk" onclick="switchAuthPanel('register')">Create one &rarr;</span></div>
    </div>
    <div id="auth-panel-register" style="display:none">
      <div class="auth-modal-icon">&#9876;&#65039;</div>
      <div class="auth-modal-title">Create Account</div>
      <div class="auth-modal-sub">Register for the tournament. Wallet must be unique.</div>
      <div class="auth-field-lbl">Username</div>
      <input class="auth-inp" id="reg-user" type="text" maxlength="20" placeholder="e.g. DragonSlayer99" autocomplete="off">
      <div class="auth-field-lbl">Password</div>
      <input class="auth-inp" id="reg-pass" type="password" maxlength="64" placeholder="Min 6 characters">
      <div class="auth-field-lbl">Confirm Password</div>
      <input class="auth-inp" id="reg-pass2" type="password" maxlength="64" placeholder="Repeat password">
      <div class="auth-field-lbl">Wallet Address <span style="color:#ff9999;font-size:.63rem">*required &middot; unique</span></div>
      <input class="auth-inp" id="reg-wallet" type="text" maxlength="100" placeholder="0x&hellip; or your wallet address" style="font-size:.82rem;letter-spacing:.4px">
      <div class="auth-err" id="reg-err"></div>
      <button class="auth-submit-btn" id="reg-btn" onclick="doRegister()">Create Account &rarr;</button>
      <div class="auth-switch-row">Have an account? <span class="auth-lnk" onclick="switchAuthPanel('login')">Sign in &rarr;</span></div>
    </div>
  </div>
</div>

<!-- BREAKING NEWS BANNER -->
<div class="breaking-news app-hidden" id="breaking-news">
  <div class="bn-bar" id="bn-bar">
    <div class="bn-inner">
      <div class="bn-label" id="bn-label">📡</div>
      <div class="bn-content">
        <div class="bn-headline" id="bn-headline">Market conditions shifting...</div>
        <div class="bn-meta">
          <div class="bn-sub" id="bn-sub">Trend bias randomised across all assets</div>
          <div class="bn-countdown" id="bn-countdown"></div>
        </div>
      </div>
      <button class="bn-close" onclick="dismissNews()">✕</button>
    </div>
  </div>
</div>

<div id="app-shell" class="app-hidden">
<div class="header">
  <div class="header-title">⚔ Trade Together ⚔</div>
  <div class="header-subtitle">Leveraged Commodity Exchange</div>
  <div class="wallet">
    <span>🪙</span>
    <span class="wallet-value" id="wallet-display">10,000</span>
    <span style="font-size:.72rem;color:var(--text-dim)">PvE</span>
  </div>
  <div id="save-indicator" style="font-size:.58rem;color:rgba(201,169,110,.4);margin-top:3px;letter-spacing:1px">💾 Auto-saving</div>
  <div id="wallet-addr-display" style="font-size:.56rem;color:rgba(201,169,110,.35);margin-top:1px;font-family:monospace;letter-spacing:.5px"></div>
</div>

<div class="tabs">
  <div class="tab active" onclick="showPage('market')">&#127978; Market</div>
  <div class="tab" onclick="showPage('portfolio')">&#128230; Positions</div>
  <div class="tab" onclick="showPage('livetrades')">&#128308; Live</div>
  <div class="tab" onclick="showPage('leaderboard')">&#127942; Board</div>
  <div class="tab" onclick="showPage('history')">&#128220; History</div>
</div>


<div id="page-market" class="page active">
  <div class="main">
    <div class="market-header">
      <div class="section-title">Live Market</div>
      <div style="display:flex;align-items:center;gap:8px">
        <div class="chart-toggle-btns">
          <button class="chart-tog-btn active" id="tog-candle" onclick="setChartMode('candle')">🕯</button>
          <button class="chart-tog-btn" id="tog-line" onclick="setChartMode('line')">📈</button>
        </div>
        <div class="tick-indicator">
          <div class="tick-dot" id="tick-dot"></div>
          <span id="tick-label">Live</span>
        </div>
      </div>
    </div>
    <!-- Chart Legend -->
    <div class="chart-legend-bar">
      <div class="legend-item">
        <span class="legend-ma-line"></span>
        <div>
          <span class="legend-label">MA20</span>
          <span class="legend-desc">Moving Average — the average closing price of the last 20 candles. When price is above MA20 the market is trending up; below means trending down.</span>
        </div>
      </div>
      <div class="legend-item">
        <span class="legend-session-line"></span>
        <div>
          <span class="legend-label">Session End</span>
          <span class="legend-desc">Vertical dashed line marking where a 150-tick trading session ended and trend biases were reset.</span>
        </div>
      </div>
      <div class="legend-item">
        <span class="legend-liq-line"></span>
        <div>
          <span class="legend-label">Liquidation Price</span>
          <span class="legend-desc">Dashed orange-red line showing the price at which your open position would be automatically liquidated (95% margin loss). ▲ = Long, ▼ = Short.</span>
        </div>
      </div>
    </div>
    <div id="market-items"></div>
    <div id="market-close-all-wrap"></div>
  </div>
</div>

<div id="page-portfolio" class="page">
  <div class="main">
    <div class="portfolio-summary">
      <div class="portfolio-total-label">Total Wealth</div>
      <div class="portfolio-total" id="port-total">10,000 PvE</div>
      <div class="portfolio-pl" id="port-pl">+0 PvE (0.00%)</div>
      <div class="portfolio-breakdown">
        <div class="portfolio-stat">
          <div class="portfolio-stat-label">💰 Free Cash</div>
          <div class="portfolio-stat-value" id="port-cash">1,000 PvE</div>
        </div>
        <div class="portfolio-stat">
          <div class="portfolio-stat-label">📊 Open P&amp;L</div>
          <div class="portfolio-stat-value" id="port-openpl" style="color:var(--text)">0 PvE</div>
        </div>
      </div>
    </div>
    <div class="section-title" style="margin-bottom:10px;">Open Positions</div>
    <div id="positions-list"></div>
  </div>
</div>

<div id="page-history" class="page">
  <div class="main">
    <div class="hist-stats-grid" id="hist-stats"></div>
    <div class="hist-filter-row">
      <div class="section-title">📜 Trade Log</div>
      <div class="hist-filters">
        <select class="hist-select" id="hist-filter-item" onchange="renderHistory()">
          <option value="all">All Items</option>
          <option value="sword">Longsword</option>
          <option value="rune">Fire Rune</option>
          <option value="potion">Potion</option>
          <option value="gem">Onyx Gem</option>
          <option value="hide">Dragon Hide</option>
          <option value="arrow">Rune Arrow</option>
        </select>
        <select class="hist-select" id="hist-filter-outcome" onchange="renderHistory()">
          <option value="all">All Results</option>
          <option value="win">Wins</option>
          <option value="loss">Losses</option>
        </select>
      </div>
    </div>
    <div id="history-list"></div>
    <div style="margin-top:20px;padding-top:16px;border-top:1px solid rgba(107,74,34,.3)">
      <button class="btn-reset-funds" onclick="resetFundsAndHistory()">↺ Reset Funds &amp; History</button>
      <div style="font-size:.7rem;color:var(--text-dim);text-align:center;margin-top:6px;font-style:italic">Closes all positions and restarts with 10,000 PvE</div>
    </div>
  </div>
</div>

<!-- LIVE TRADES PAGE -->
<div id="page-livetrades" class="page">
  <div class="main">
    <div class="market-header" style="margin-bottom:12px">
      <div class="section-title">&#128308; Live Trades</div>
      <div class="tick-indicator"><div class="tick-dot"></div><span id="live-count" style="font-size:.7rem;color:var(--text-dim)">0 positions</span></div>
    </div>
    <div id="live-table-container"><div class="hist-empty"><div class="hist-empty-icon">&#128225;</div>No live trades yet.</div></div>
  </div>
</div>
<!-- LEADERBOARD PAGE -->
<div id="page-leaderboard" class="page">
  <div class="main">
    <div class="market-header" style="margin-bottom:12px">
      <div class="section-title">&#127942; Leaderboard</div>
      <div style="font-size:.68rem;color:var(--text-dim);letter-spacing:1px">P&amp;L Rankings</div>
    </div>
    <div id="lb-table-container"><div class="hist-empty"><div class="hist-empty-icon">&#127942;</div>No players yet.</div></div>
  </div>
</div>
<!-- WARNING POPUP — too much position size -->
<div class="warning-overlay" id="warning-overlay">
  <div class="warning-box">
    <div class="warning-emoji">🤡</div>
    <div class="warning-pct" id="warning-pct">--% of portfolio</div>
    <div class="warning-title">Too much leverage dumbo</div>
    <div class="warning-body">You're risking over half your total wealth on a single position. Are you absolutely sure about this?</div>
    <div class="warning-btns">
      <button class="warning-btn-cancel" onclick="closeWarning()">Play it safe</button>
      <button class="warning-btn-confirm" onclick="confirmTrade()">Yolo it 🎲</button>
    </div>
  </div>
</div>

<!-- WARNING POPUP — max leverage on high volatility -->
<div class="warning-overlay" id="lev-warning-overlay">
  <div class="warning-box" style="border-color:#ff8c00;box-shadow:0 0 40px rgba(255,140,0,.4),0 8px 32px rgba(0,0,0,.8)">
    <div class="warning-emoji">🔥</div>
    <div class="warning-pct" style="color:#ff8c00" id="lev-warning-vol">Volatility: --%</div>
    <div class="warning-title" style="color:#ff8c00;text-shadow:0 0 12px rgba(255,140,0,.4)" id="lev-warning-title">Max Leverage Warning</div>
    <div class="warning-body" id="lev-warning-body">This is a <strong style="color:#ffcc88">high volatility pair</strong>. Using 10x leverage means a small price move can wipe your entire margin instantly. Liquidation is very likely.</div>
    <div class="warning-btns">
      <button class="warning-btn-cancel" onclick="closeLevWarning()">Lower leverage</button>
      <button class="warning-btn-confirm" style="border-color:#ff8c00;background:linear-gradient(135deg,#5c3000,#3d1f00)" onclick="confirmLevTrade()">I understand the risk 🔥</button>
    </div>
  </div>
</div>

<!-- WARNING POPUP — 5x moderate risk -->
<div class="warning-overlay" id="lev5-warning-overlay">
  <div class="warning-box" style="border-color:#f0c040;box-shadow:0 0 30px rgba(240,192,64,.3),0 8px 32px rgba(0,0,0,.8)">
    <div class="warning-emoji">⚠️</div>
    <div class="warning-pct" style="color:#f0c040" id="lev5-warning-vol">Volatility: --%</div>
    <div class="warning-title" style="color:#f0c040;text-shadow:0 0 12px rgba(240,192,64,.3)">Elevated Risk — 5x Leverage</div>
    <div class="warning-body">5x leverage <strong style="color:#ffd966">amplifies both gains and losses by 5x</strong>. A 20% adverse move wipes your full margin. Manageable with a tight Stop Loss — but proceed with caution.</div>
    <div class="warning-btns">
      <button class="warning-btn-cancel" onclick="closeLev5Warning()">Lower leverage</button>
      <button class="warning-btn-confirm" style="border-color:#f0c040;background:linear-gradient(135deg,#4a3800,#2d2200);color:#ffd966" onclick="confirmLev5Trade()">Got it, proceed ⚠️</button>
    </div>
  </div>
</div>

<!-- MODAL -->
</div><!-- end app-shell -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal" id="modal">
    <div class="modal-handle"></div>
    <div class="modal-title" id="modal-title">Open Trade</div>
    <div class="modal-price-display" id="modal-price-display"></div>
    <div class="modal-section-label">Leverage</div>
    <div class="modal-lev-row">
      <div class="modal-lev-btn active" data-lev="1" onclick="setLev(1)">1x</div>
      <div class="modal-lev-btn" data-lev="3" onclick="setLev(3)">3x</div>
      <div class="modal-lev-btn" data-lev="5" onclick="setLev(5)">5x</div>
      <div class="modal-lev-btn" data-lev="10" onclick="setLev(10)">10x</div>
      <div class="modal-lev-btn" data-lev="25" onclick="setLev(25)">25x</div>
    </div>
    <div class="modal-section-label">Lot Size</div>
    <div class="modal-lot-row">
      <button class="lot-btn" onclick="adjustLot(-1)">−</button>
      <input class="modal-input lot-input" id="modal-lot" type="number" min="1" max="100" value="1" oninput="updateModalCost()">
      <button class="lot-btn" onclick="adjustLot(1)">+</button>
      <span class="lot-label">lots</span>
    </div>
    <div class="modal-section-label">Risk Management <span style="color:#ff9999;font-size:.65rem">(Stop Loss required · % of margin)</span></div>
    <div class="modal-sltp-row">
      <div class="modal-field">
        <label class="sl" for="modal-sl">🛑 Stop Loss %</label>
        <div style="display:flex;gap:4px;margin-bottom:4px">
          <button class="sltp-preset sl-preset" onclick="setSlPct(25)">25%</button>
          <button class="sltp-preset sl-preset" onclick="setSlPct(35)">35%</button>
          <button class="sltp-preset sl-preset" onclick="setSlPct(50)">50%</button>
        </div>
        <div style="position:relative">
          <input class="modal-input sl" id="modal-sl" type="number" min="0.1" max="99" step="0.1" placeholder="e.g. 5" oninput="updateModalCost()">
          <span class="sltp-suffix">%</span>
        </div>
        <div class="sltp-price-hint" id="sl-price-hint"></div>
      </div>
      <div class="modal-field">
        <label class="tp" for="modal-tp">🎯 Take Profit %</label>
        <div style="display:flex;gap:4px;margin-bottom:4px">
          <button class="sltp-preset tp-preset" onclick="setTpPct(50)">50%</button>
          <button class="sltp-preset tp-preset" onclick="setTpPct(75)">75%</button>
          <button class="sltp-preset tp-preset" onclick="setTpPct(100)">100%</button>
        </div>
        <div style="position:relative">
          <input class="modal-input tp" id="modal-tp" type="number" min="0.1" max="999" step="0.1" placeholder="Optional" oninput="updateModalCost()">
          <span class="sltp-suffix">%</span>
        </div>
        <div class="sltp-price-hint" id="tp-price-hint"></div>
      </div>
    </div>
    <div class="modal-cost-info" id="modal-cost-info">
      <div class="modal-cost-row"><span>Margin required</span><strong id="modal-margin">--</strong></div>
      <div class="modal-cost-row"><span>Notional exposure</span><strong id="modal-notional">--</strong></div>
      <div class="modal-cost-row"><span style="color:#ff9999">Opening fee</span><strong id="modal-fee" style="color:#ff9999">--</strong></div>
      <div class="modal-cost-row"><span>Total cost</span><strong id="modal-total-cost">--</strong></div>
      <div class="modal-cost-row"><span>Your free cash</span><strong id="modal-cash">--</strong></div>
      <div class="modal-cost-row" style="margin-top:4px;padding-top:4px;border-top:1px solid rgba(107,74,34,.3)">
        <span>Portfolio at risk</span><strong id="modal-pct-row" style="color:var(--green)">--%</strong>
      </div>
    </div>
    <div class="modal-dir-btns">
      <button class="modal-btn-long" id="modal-btn-long" onclick="openTrade('long')">▲ Long<br><span style="font-size:.65rem;opacity:.8">Profit if price rises</span></button>
      <button class="modal-btn-short" id="modal-btn-short" onclick="openTrade('short')">▼ Short<br><span style="font-size:.65rem;opacity:.8">Profit if price falls</span></button>
    </div>
    <button class="modal-cancel" onclick="closeModal()">Cancel</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
var ITEMS = [
  { id:'sword',  name:'Dragon Longsword',    desc:'A fearsome blade of dragonite',    icon:'⚔️', price:220, basePrice:220, volatility:0, trend:0, history:[], candles:[], candleTickBuf:[] },
  { id:'rune',   name:'Fire Rune Bundle',    desc:'Ancient runes pulsing with flame', icon:'🔥', price:85,  basePrice:85,  volatility:0, trend:0, history:[], candles:[], candleTickBuf:[] },
  { id:'potion', name:'Super Restore Potion',desc:'Restores stats drained in battle', icon:'🧪', price:150, basePrice:150, volatility:0, trend:0, history:[], candles:[], candleTickBuf:[] },
  { id:'gem',    name:'Onyx Gem',            desc:'Rare gemstone from deep mines',    icon:'💎', price:310, basePrice:310, volatility:0, trend:0, history:[], candles:[], candleTickBuf:[] },
  { id:'hide',   name:'Dragon Leather Hide', desc:'Tanned hide of a slain dragon',   icon:'🐉', price:175, basePrice:175, volatility:0, trend:0, history:[], candles:[], candleTickBuf:[] },
  { id:'arrow',  name:'Rune Arrow Bundle',   desc:'Magically tipped arrows of rune', icon:'🏹', price:55,  basePrice:55,  volatility:0, trend:0, history:[], candles:[], candleTickBuf:[] }
];

// Randomize volatility, trend, and starting price for all 6 items at startup
(function() {
  var allItems = [
    { id:'sword', priceMin:150, priceMax:350 },
    { id:'rune',  priceMin:50,  priceMax:150 },
    { id:'potion',priceMin:80,  priceMax:250 },
    { id:'gem',   priceMin:200, priceMax:450 },
    { id:'hide',  priceMin:80,  priceMax:280 },
    { id:'arrow', priceMin:20,  priceMax:120 }
  ];
  allItems.forEach(function(cfg) {
    var item = ITEMS.filter(function(i){ return i.id === cfg.id; })[0];
    item.price      = Math.round(cfg.priceMin + Math.random() * (cfg.priceMax - cfg.priceMin));
    item.basePrice  = item.price;
    item.volatility = Math.round((0.05 + Math.random() * 0.075) * 1.25 * 0.95 * 10000) / 10000;
    item.trend      = (Math.random() - 0.5) * 0.0012;
  });
}());

var MAX_HISTORY       = 60;
var TICK_MS           = 2000;
var CANDLE_TICKS      = 3;
var MA_PERIOD         = 20;
var STARTING_GP       = 10000;
var MARGIN_RATE       = 0.2;
var TREND_SHIFT_TICKS = 150;
var WARNING_TICKS     = 25;
var PRICE_CEILING     = 7000;   // above this, asset is suspended and reset
var CEILING_WARN_TICKS = 10;    // warn this many ticks before forced close

var wallet        = STARTING_GP;
var positions     = [];
var tradeHistory  = [];
var posIdCounter  = 0;
var lastPrices    = {};
var modalItemId   = null;
var modalLev      = 1;
var pendingTrade  = null;
var tickCount     = 0;
var chartMode     = 'candle';
var nextShiftAt   = TREND_SHIFT_TICKS;
var newsTimer     = null;
var strobeTimers  = {};
var sessionBoundaries = []; // tick counts where sessions ended, used to draw vertical lines
var ceilingWarnings   = {}; // { itemId: ticksUntilReset } — countdown per suspended asset

ITEMS.forEach(function(item) {
  lastPrices[item.id] = item.price;
  for (var i = 0; i < 20; i++) item.history.push(item.price);
  for (var j = 0; j < 15; j++) {
    item.candles.push({ o: item.price, h: item.price, l: item.price, c: item.price });
  }
});

// ── PRICE ENGINE ─────────────────────────────────────────────────────────────
function updatePrices() {
  // Price updates are now handled by hostTick (host) or clientSync (clients)
  // This function is kept as a no-op for compatibility
}

function calcPnl(pos, currentPrice) {
  var lots  = pos.lots || 1;
  var units = (pos.margin / pos.entryPrice) * pos.leverage;
  var diff  = currentPrice - pos.entryPrice;
  return pos.direction === 'long' ? diff * units : -diff * units;
}

function recordTrade(pos, exitPrice, pnl, closeReason) {
  var item    = getItem(pos.itemId);
  var pnlPct  = pct(pos.margin + pnl, pos.margin);
  tradeHistory.unshift({
    id: pos.id, itemId: pos.itemId, itemName: item.name, itemIcon: item.icon,
    direction: pos.direction, leverage: pos.leverage,
    entryPrice: pos.entryPrice, exitPrice: exitPrice,
    margin: pos.margin, pnl: Math.round(pnl), pnlPct: pnlPct,
    closeReason: closeReason, closedAt: Date.now(), sl: pos.sl, tp: pos.tp
  });
}

function checkSLTP() {
  var toClose = [];
  positions.forEach(function(pos) {
    var item = getItem(pos.itemId);
    var p    = item.price;
    var pnl  = calcPnl(pos, p);
    if (pos.sl != null) {
      if (pos.direction === 'long'  && p <= pos.sl) { toClose.push({ pos:pos, reason:'sl' }); return; }
      if (pos.direction === 'short' && p >= pos.sl) { toClose.push({ pos:pos, reason:'sl' }); return; }
    }
    if (pos.tp != null) {
      if (pos.direction === 'long'  && p >= pos.tp) { toClose.push({ pos:pos, reason:'tp' }); return; }
      if (pos.direction === 'short' && p <= pos.tp) { toClose.push({ pos:pos, reason:'tp' }); return; }
    }
    if (pnl <= -pos.margin * 0.95) { toClose.push({ pos:pos, reason:'liq' }); }
  });

  toClose.forEach(function(entry) {
    var pos  = entry.pos;
    var reason = entry.reason;
    var item = getItem(pos.itemId);
    var pnl  = calcPnl(pos, item.price);
    recordTrade(pos, item.price, pnl, reason);
    wallet += Math.max(0, pos.margin + pnl);
    positions = positions.filter(function(p) { return p.id !== pos.id; });
    var d = pos.direction === 'long' ? 'Long' : 'Short';
    if (reason === 'sl')       showToast('Stop Loss hit — ' + d + ' ' + item.name + ' | ' + fmtPnl(Math.round(pnl)) + ' PvE', 'sl-hit');
    else if (reason === 'tp')  showToast('Take Profit hit — ' + d + ' ' + item.name + ' | +' + fmt(Math.round(pnl)) + ' PvE', 'tp-hit');
    else                       showToast('Liquidated: ' + d + ' ' + item.name, 'error');
  });
}

// ── PRICE CEILING CHECK ───────────────────────────────────────────────────────
function checkPriceCeiling() {
  ITEMS.forEach(function(item) {
    // If already in countdown, tick it down
    if (ceilingWarnings[item.id] !== undefined) {
      ceilingWarnings[item.id]--;

      // Update banner countdown
      var el = document.getElementById('bn-countdown');
      if (el && ceilingWarnings[item.id] >= 0) {
        el.textContent = ceilingWarnings[item.id] + ' ticks';
      }

      if (ceilingWarnings[item.id] <= 0) {
        // Force close all positions for this item
        var toClose = positions.filter(function(p){ return p.itemId === item.id; });
        var totalPnl = 0;
        toClose.forEach(function(pos) {
          var pnl = calcPnl(pos, item.price);
          totalPnl += pnl;
          recordTrade(pos, item.price, pnl, 'ceiling');
          wallet += Math.max(0, pos.margin + pnl);
        });
        positions = positions.filter(function(p){ return p.itemId !== item.id; });

        // Reset item to base price
        item.price = item.basePrice;
        item.trend = randomTrend();
        item.history = [];
        item.candles = [];
        item.candleTickBuf = [];
        lastPrices[item.id] = item.price;
        for (var i = 0; i < 20; i++) item.history.push(item.price);
        for (var j = 0; j < 15; j++) {
          item.candles.push({ o: item.price, h: item.price, l: item.price, c: item.price });
        }

        delete ceilingWarnings[item.id];
        dismissNews();
        resetBannerStyle();

        // Remove this item's card so it rebuilds fresh
        var oldCard = document.getElementById('card-' + item.id);
        if (oldCard) oldCard.parentNode.removeChild(oldCard);

        showToast(item.icon + ' ' + item.name + ' suspended & reset to ' + fmt(item.basePrice) + ' PvE' + (toClose.length ? ' | Positions closed: ' + fmtPnl(Math.round(totalPnl)) + ' PvE' : ''), 'error');
        scheduleSave();
      }
      return;
    }

    // Check if price just crossed the ceiling
    if (item.price >= PRICE_CEILING) {
      ceilingWarnings[item.id] = CEILING_WARN_TICKS;

      // Show breaking news banner
      var bar   = document.getElementById('bn-bar');
      var label = document.getElementById('bn-label');
      if (bar)   { bar.style.background = 'linear-gradient(135deg,#4a2a00,#7a4a00,#4a2a00)'; bar.style.borderBottomColor = '#ff8c00'; }
      if (label) { label.style.background = '#8a4a00'; label.textContent = '⚠️ Suspended'; }
      document.getElementById('bn-headline').textContent = item.icon + '  ' + item.name + ' hit price ceiling of 7,000 PvE!';
      document.getElementById('bn-sub').textContent      = 'All positions will be closed automatically — asset resetting to base price';
      document.getElementById('bn-countdown').textContent = CEILING_WARN_TICKS + ' ticks';
      document.getElementById('breaking-news').classList.add('show');
      clearTimeout(newsTimer);
    }
  });
}

function randomTrend() {
  return (Math.random() - 0.5) * 0.0012; // -0.0006 to +0.0006
}

function shiftTrends() {
  sessionBoundaries.push(tickCount);
  if (sessionBoundaries.length > 60) sessionBoundaries.shift();
  ITEMS.forEach(function(item) { item.trend = randomTrend(); });
  nextShiftAt = tickCount + TREND_SHIFT_TICKS;
  dismissNews();
}

function showBreakingNews(ticksLeft) {
  resetBannerStyle();
  document.getElementById('bn-headline').textContent = 'New trading session coming soon!';
  document.getElementById('bn-sub').textContent = '150-tick session ending — trends will reset';
  document.getElementById('bn-countdown').textContent = ticksLeft + ' ticks';
  document.getElementById('breaking-news').classList.add('show');
}

function updateNewsCountdown(ticksLeft) {
  var el = document.getElementById('bn-countdown');
  if (el) el.textContent = ticksLeft + ' ticks';
}

function dismissNews() {
  document.getElementById('breaking-news').classList.remove('show');
  clearTimeout(newsTimer);
}

// ── PRICE SURGE / CRASH EVENTS ────────────────────────────────────────────────
// Only ONE event (surge OR crash) can be active at any time — never both.
// Each event: random asset, random duration 5-16 ticks, random magnitude 30-85%.
// After an event ends, a cooldown of 20-45 ticks before the next one fires.
// Events alternate: if last was surge, next is crash, and vice versa.

var activeEvent   = null; // { type:'surge'|'crash', itemId, ticksLeft, perTick, totalPct, ticks }
var lastEventType = null; // tracks alternation
var nextEventAt   = 15 + Math.floor(Math.random() * 25); // first event: tick 15-40

function pickRandomItem(excludeId) {
  var pool = ITEMS.filter(function(i){ return i.id !== excludeId; });
  return pool[Math.floor(Math.random() * pool.length)];
}

function startEvent(type) {
  var excludeId = null; // no exclusion needed since only one event at a time
  var item      = pickRandomItem(excludeId);
  var ticks     = 4 + Math.floor(Math.random() * 8);            // 4–11 ticks
  var totalPct, perTick;

  if (type === 'surge') {
    totalPct = 0.30 + Math.random() * 0.55;                     // 30–85% gain
    perTick  = Math.pow(1 + totalPct, 1 / ticks) - 1;
  } else {
    totalPct = 0.25 + Math.random() * 0.45;                     // 25–70% drop
    perTick  = 1 - Math.pow(1 - totalPct, 1 / ticks);
  }

  activeEvent   = { type: type, itemId: item.id, ticksLeft: ticks, perTick: perTick, totalPct: totalPct, ticks: ticks };
  lastEventType = type;
}

function showEventBanner(type, item, ticks, eventPct) {
  var pctStr = Math.round(eventPct * 100) + '%';
  var secStr = (ticks * TICK_MS / 1000).toFixed(0) + 's';
  var bar    = document.getElementById('bn-bar');
  var label  = document.getElementById('bn-label');

  if (type === 'surge') {
    document.getElementById('bn-headline').textContent = item.icon + ' ' + item.name + ' SURGING +' + pctStr;
    document.getElementById('bn-sub').textContent      = '~' + ticks + ' ticks (' + secStr + ') — act fast!';
    if (bar)   { bar.style.background = 'linear-gradient(135deg,#0a4a1a,#1a7a32,#0a4a1a)'; bar.style.borderBottomColor = '#2ecc71'; }
    if (label) { label.style.background = '#1a8a30'; label.textContent = '📈'; }
  } else {
    document.getElementById('bn-headline').textContent = item.icon + ' ' + item.name + ' CRASHING -' + pctStr;
    document.getElementById('bn-sub').textContent      = '~' + ticks + ' ticks (' + secStr + ') — watch out!';
    if (bar)   { bar.style.background = 'linear-gradient(135deg,#4a0a0a,#7a1a1a,#4a0a0a)'; bar.style.borderBottomColor = '#e74c3c'; }
    if (label) { label.style.background = '#8a1a1a'; label.textContent = '📉'; }
  }

  document.getElementById('bn-countdown').textContent = ticks + ' ticks';
  document.getElementById('breaking-news').classList.add('show');
  clearTimeout(newsTimer);
}

function updateEventBannerCountdown() {
  if (!activeEvent) return;
  var el = document.getElementById('bn-countdown');
  if (el) el.textContent = activeEvent.ticksLeft + ' ticks left';
}

function resetBannerStyle() {
  var bar   = document.getElementById('bn-bar');
  var label = document.getElementById('bn-label');
  if (bar)   { bar.style.background = ''; bar.style.borderBottomColor = ''; }
  if (label) { label.style.background = ''; label.textContent = '📡'; }
}

function applyEventPrices() {
  if (!activeEvent) return;

  var item = getItem(activeEvent.itemId);
  if (item) {
    if (activeEvent.type === 'surge') {
      item.price = Math.round(item.price * (1 + activeEvent.perTick) * 100) / 100;
    } else {
      item.price = Math.max(5, Math.round(item.price * (1 - activeEvent.perTick) * 100) / 100);
    }
  }

  activeEvent.ticksLeft--;

  if (activeEvent.ticksLeft <= 0) {
    var doneType = activeEvent.type;
    var doneName = item ? item.name : '';
    activeEvent = null;
    resetBannerStyle();
    dismissNews();

    // Schedule next event: 20-50 ticks cooldown, then alternate type
    nextEventAt = tickCount + 20 + Math.floor(Math.random() * 30);
  }
}

function maybeFireEvent() {
  // Never fire if an event is already active
  if (activeEvent) {
    updateEventBannerCountdown();
    return;
  }
  if (tickCount < nextEventAt) return;

  // Alternate: if last was surge fire crash, if last was crash fire surge,
  // if no history pick randomly
  var nextType;
  if (lastEventType === 'surge')       nextType = 'crash';
  else if (lastEventType === 'crash')  nextType = 'surge';
  else                                 nextType = Math.random() < 0.5 ? 'surge' : 'crash';

  startEvent(nextType);
}


function strobeChart(itemId, direction) {
  var wrap = document.querySelector('#card-' + itemId + ' .chart-wrap');
  if (!wrap) return;
  var ov = wrap.querySelector('.strobe-overlay');
  if (!ov) {
    ov = document.createElement('div');
    ov.className = 'strobe-overlay';
    wrap.appendChild(ov);
  }
  var cls = direction === 'up' ? 'flash-green' : 'flash-red';
  ov.className = 'strobe-overlay ' + cls;
  ov.style.opacity = '1';
  clearTimeout(strobeTimers[itemId]);
  strobeTimers[itemId] = setTimeout(function() { ov.style.opacity = '0'; }, 300);

  // Also pulse the card glow
  var card = document.getElementById('card-' + itemId);
  if (card) {
    card.classList.add('strobe-active');
    setTimeout(function() { card.classList.remove('strobe-active'); }, 600);
  }
}

// ── CLOSE ALL ─────────────────────────────────────────────────────────────────
function closeAllPositions() {
  var count = positions.length;
  if (count === 0) { showToast('No open positions to close', 'error'); return; }
  var totalPnl = 0;
  var toClose = positions.slice();
  toClose.forEach(function(pos) {
    var item = getItem(pos.itemId);
    var pnl  = calcPnl(pos, item.price);
    totalPnl += pnl;
    recordTrade(pos, item.price, pnl, 'manual');
    wallet += Math.max(0, pos.margin + pnl);
  });
  positions = [];
  showToast('Closed ' + count + ' position' + (count > 1 ? 's' : '') + ' | ' + fmtPnl(Math.round(totalPnl)) + ' PvE total', totalPnl >= 0 ? 'buy' : 'sell');
  scheduleSave();
  renderAll();
}

function closeItemPositions(itemId) {
  var item = getItem(itemId);
  var toClose = positions.filter(function(p){ return p.itemId === itemId; });
  if (toClose.length === 0) return;
  var totalPnl = 0;
  toClose.forEach(function(pos) {
    var pnl = calcPnl(pos, item.price);
    totalPnl += pnl;
    recordTrade(pos, item.price, pnl, 'manual');
    wallet += Math.max(0, pos.margin + pnl);
  });
  positions = positions.filter(function(p){ return p.itemId !== itemId; });
  showToast('Closed ' + item.name + ' positions | ' + fmtPnl(Math.round(totalPnl)) + ' PvE', totalPnl >= 0 ? 'buy' : 'sell');
  scheduleSave();
  renderAll();
}


function getItem(id) { return ITEMS.filter(function(i){ return i.id === id; })[0]; }
function fmt(n)      { return Math.round(n).toLocaleString(); }
function fmtPnl(v)   { return (v >= 0 ? '+' : '') + fmt(v); }
function pct(now, before) { return before ? (now - before) / before * 100 : 0; }
function fmtPct(v)   { return (v >= 0 ? '+' : '') + v.toFixed(2) + '%'; }

function timeAgo(ts) {
  var s = Math.floor((Date.now() - ts) / 1000);
  if (s < 60) return s + 's ago';
  var m = Math.floor(s / 60);
  if (m < 60) return m + 'm ago';
  return Math.floor(m / 60) + 'h ago';
}

// ── CHART ─────────────────────────────────────────────────────────────────────
function calcMA(candles, period) {
  var result = [];
  for (var i = 0; i < candles.length; i++) {
    if (i < period - 1) { result.push(null); continue; }
    var sum = 0;
    for (var j = i - period + 1; j <= i; j++) sum += candles[j].c;
    result.push(sum / period);
  }
  return result;
}

function drawCandleChart(el, candles, miniMode, item) {
  var dpr  = window.devicePixelRatio || 1;
  var w    = el.offsetWidth * dpr;
  var h    = el.offsetHeight * dpr;
  if (el.width !== w || el.height !== h) { el.width = w; el.height = h; }
  var ctx  = el.getContext('2d');
  ctx.clearRect(0, 0, w, h);

  if (!candles || candles.length < 2) return;

  // Only show last 40 candles for cleaner analysis
  candles = candles.slice(-40);

  var padT = (miniMode ? 6 : 18) * dpr;
  var padB = (miniMode ? 6 : 22) * dpr;
  var padL = (miniMode ? 4 : 8)  * dpr;
  var padR = (miniMode ? 4 : 8)  * dpr;
  var W    = w - padL - padR;
  var H    = h - padT - padB;

  // Gather open positions for this item to get liquidation prices
  var itemPositions = item ? positions.filter(function(p){ return p.itemId === item.id; }) : [];
  var liqPrices = itemPositions.map(function(p){ return calcLiqPrice(p); });

  // Price range — expand to include all liquidation prices
  var allH = candles.map(function(c){ return c.h; });
  var allL = candles.map(function(c){ return c.l; });
  var mn   = Math.min.apply(null, allL);
  var mx   = Math.max.apply(null, allH);
  liqPrices.forEach(function(lp) { mn = Math.min(mn, lp); mx = Math.max(mx, lp); });
  var pad  = (mx - mn) * 0.08 || 1;
  mn -= pad; mx += pad;
  var rng  = mx - mn;

  var py = function(v) { return padT + H - ((v - mn) / rng) * H; };
  var n  = candles.length;
  var cw = W / n;
  var bw = Math.max(1 * dpr, cw * 0.55);

  // Grid lines (full chart only)
  if (!miniMode) {
    ctx.strokeStyle = 'rgba(107,74,34,0.2)';
    ctx.lineWidth   = 1 * dpr;
    for (var g = 0; g <= 4; g++) {
      var gy = padT + (H / 4) * g;
      ctx.beginPath(); ctx.moveTo(padL, gy); ctx.lineTo(padL + W, gy); ctx.stroke();
      var gv = mx - (rng / 4) * g;
      ctx.fillStyle = 'rgba(201,169,110,0.5)';
      ctx.font = (9 * dpr) + 'px monospace';
      ctx.fillText(Math.round(gv), padL + 2 * dpr, gy - 2 * dpr);
    }
  }

  // Draw candles
  for (var i = 0; i < n; i++) {
    var c   = candles[i];
    var x   = padL + (i + 0.5) * cw;
    var isUp = c.c >= c.o;
    var col  = isUp ? '#27ae60' : '#c0392b';
    var bodyT = py(Math.max(c.o, c.c));
    var bodyB = py(Math.min(c.o, c.c));
    var bodyH = Math.max(1 * dpr, bodyB - bodyT);

    // Wick
    ctx.strokeStyle = col;
    ctx.lineWidth   = Math.max(1, 1 * dpr);
    ctx.beginPath();
    ctx.moveTo(x, py(c.h));
    ctx.lineTo(x, py(c.l));
    ctx.stroke();

    // Body
    ctx.fillStyle = col;
    ctx.fillRect(x - bw / 2, bodyT, bw, bodyH);

    // Body border for doji / flat candles
    if (bodyH <= 1 * dpr) {
      ctx.strokeStyle = col;
      ctx.lineWidth   = 1 * dpr;
      ctx.strokeRect(x - bw / 2, bodyT, bw, Math.max(1 * dpr, bodyH));
    }
  }

  // Moving average overlay
  var ma = calcMA(candles, MA_PERIOD);
  ctx.beginPath();
  var started = false;
  for (var k = 0; k < ma.length; k++) {
    if (ma[k] === null) continue;
    var mx2 = padL + (k + 0.5) * cw;
    var my  = py(ma[k]);
    if (!started) { ctx.moveTo(mx2, my); started = true; }
    else           ctx.lineTo(mx2, my);
  }
  ctx.strokeStyle = '#f0c040';
  ctx.lineWidth   = (miniMode ? 1.2 : 1.8) * dpr;
  ctx.lineJoin    = 'round';
  ctx.setLineDash([4 * dpr, 3 * dpr]);
  ctx.stroke();
  ctx.setLineDash([]);

  // Session boundary vertical lines
  // Each candle covers CANDLE_TICKS ticks. Most recent candle = tickCount.
  // Work backwards: candle[n-1] ended at tickCount, candle[n-2] at tickCount - CANDLE_TICKS, etc.
  var candleCount = candles.length;
  sessionBoundaries.forEach(function(boundaryTick) {
    // How many ticks ago did this boundary happen?
    var ticksAgo = tickCount - boundaryTick;
    if (ticksAgo < 0) return;
    // Which candle index does this correspond to?
    var candleAgo = ticksAgo / CANDLE_TICKS;
    var candleIdx = candleCount - 1 - candleAgo;
    if (candleIdx < 0 || candleIdx >= candleCount) return;
    var lineX = padL + (candleIdx + 0.5) * cw;
    ctx.save();
    ctx.strokeStyle = 'rgba(240,192,64,0.55)';
    ctx.lineWidth   = 1.5 * dpr;
    ctx.beginPath();
    ctx.moveTo(lineX, padT);
    ctx.lineTo(lineX, padT + H);
    ctx.stroke();
    ctx.restore();
  });

  // Current price line (full chart only)
  if (!miniMode && candles.length > 0) {
    var lastC  = candles[candles.length - 1];
    var lineY  = py(lastC.c);
    var lineCol = lastC.c >= lastC.o ? '#27ae60' : '#c0392b';
    ctx.strokeStyle = lineCol;
    ctx.lineWidth   = 1 * dpr;
    ctx.setLineDash([3 * dpr, 3 * dpr]);
    ctx.beginPath(); ctx.moveTo(padL, lineY); ctx.lineTo(padL + W, lineY); ctx.stroke();
    ctx.setLineDash([]);
    // Price label
    ctx.fillStyle = lineCol;
    ctx.font      = 'bold ' + (9 * dpr) + 'px monospace';
    ctx.fillText(Math.round(lastC.c), padL + W - 32 * dpr, lineY - 2 * dpr);
  }

  // Liquidation lines per open position
  itemPositions.forEach(function(pos) {
    var liqPrice = calcLiqPrice(pos);
    var liqY     = py(liqPrice);
    // Only draw if within visible range
    if (liqY < padT - 2 * dpr || liqY > padT + H + 2 * dpr) return;
    var isLong   = pos.direction === 'long';
    var liqColor = '#ff4500'; // bright orange-red for liquidation

    ctx.save();
    ctx.strokeStyle = liqColor;
    ctx.lineWidth   = 1.5 * dpr;
    ctx.beginPath();
    ctx.moveTo(padL, liqY);
    ctx.lineTo(padL + W, liqY);
    ctx.stroke();

    // Label on right edge
    ctx.fillStyle = liqColor;
    ctx.font = 'bold ' + (8 * dpr) + 'px monospace';
    ctx.textAlign = 'right';
    var label = 'LIQ ' + (isLong ? '▲' : '▼') + ' ' + Math.round(liqPrice);
    ctx.fillText(label, padL + W - 1 * dpr, liqY - 2 * dpr);
    ctx.textAlign = 'left';

    // Small filled triangle marker on the right edge
    var triSize = 5 * dpr;
    ctx.beginPath();
    ctx.moveTo(padL + W, liqY);
    ctx.lineTo(padL + W - triSize, liqY - triSize);
    ctx.lineTo(padL + W - triSize, liqY + triSize);
    ctx.closePath();
    ctx.fillStyle = liqColor;
    ctx.fill();
    ctx.restore();
  });
}

function drawLineChart(el, history, miniMode, item) {
  var dpr = window.devicePixelRatio || 1;
  var w   = el.offsetWidth * dpr;
  var h   = el.offsetHeight * dpr;
  if (el.width !== w || el.height !== h) { el.width = w; el.height = h; }
  var ctx  = el.getContext('2d');
  ctx.clearRect(0, 0, w, h);
  var data = history.slice(-60);
  if (data.length < 2) return;
  var pad  = 6 * dpr;
  var padT = (miniMode ? pad : 18 * dpr);
  var padB = (miniMode ? pad : 22 * dpr);
  var W    = w - pad * 2, H = h - padT - padB;

  // Gather open positions for this item
  var itemPositions = item ? positions.filter(function(p){ return p.itemId === item.id; }) : [];
  var liqPrices = itemPositions.map(function(p){ return calcLiqPrice(p); });

  // Expand price range to include liquidation prices
  var mn   = Math.min.apply(null, data) * 0.97;
  var mx   = Math.max.apply(null, data) * 1.03;
  liqPrices.forEach(function(lp) { mn = Math.min(mn, lp * 0.97); mx = Math.max(mx, lp * 1.03); });
  var rng  = mx - mn || 1;
  var px   = function(i){ return pad + (i / (data.length - 1)) * W; };
  var py   = function(v){ return padT + H - ((v - mn) / rng) * H; };
  var last = data[data.length - 1];
  var prev = data[data.length - 2];
  var color = last >= prev ? '#27ae60' : '#c0392b';

  var grad = ctx.createLinearGradient(0, padT, 0, padT + H);
  grad.addColorStop(0, color + '40'); grad.addColorStop(1, color + '00');
  ctx.beginPath(); ctx.moveTo(px(0), py(data[0]));
  for (var i = 1; i < data.length; i++) ctx.lineTo(px(i), py(data[i]));
  ctx.lineTo(px(data.length - 1), padT + H); ctx.lineTo(px(0), padT + H);
  ctx.closePath(); ctx.fillStyle = grad; ctx.fill();
  ctx.beginPath(); ctx.moveTo(px(0), py(data[0]));
  for (var j = 1; j < data.length; j++) ctx.lineTo(px(j), py(data[j]));
  ctx.strokeStyle = color; ctx.lineWidth = 2 * dpr; ctx.lineJoin = 'round'; ctx.stroke();

  // MA overlay on line chart (use fake candles from history)
  if (data.length >= MA_PERIOD) {
    var maData = [];
    for (var m = 0; m < data.length; m++) {
      if (m < MA_PERIOD - 1) { maData.push(null); continue; }
      var sum = 0;
      for (var n = m - MA_PERIOD + 1; n <= m; n++) sum += data[n];
      maData.push(sum / MA_PERIOD);
    }
    ctx.beginPath();
    var started = false;
    for (var k = 0; k < maData.length; k++) {
      if (maData[k] === null) continue;
      if (!started) { ctx.moveTo(px(k), py(maData[k])); started = true; }
      else           ctx.lineTo(px(k), py(maData[k]));
    }
    ctx.strokeStyle = '#f0c040';
    ctx.lineWidth   = (miniMode ? 1.2 : 1.8) * dpr;
    ctx.lineJoin    = 'round';
    ctx.setLineDash([4 * dpr, 3 * dpr]);
    ctx.stroke();
    ctx.setLineDash([]);
  }

  // Dot at end
  ctx.beginPath();
  ctx.arc(px(data.length - 1), py(data[data.length - 1]), 3 * dpr, 0, Math.PI * 2);
  ctx.fillStyle = color; ctx.fill();

  // Session boundary vertical lines on line chart
  // Each data point = 1 tick. Most recent data point = tickCount.
  var dataLen = data.length;
  sessionBoundaries.forEach(function(boundaryTick) {
    var ticksAgo = tickCount - boundaryTick;
    if (ticksAgo < 0 || ticksAgo >= dataLen) return;
    var dataIdx = dataLen - 1 - ticksAgo;
    var lineX = px(dataIdx);
    ctx.save();
    ctx.strokeStyle = 'rgba(240,192,64,0.55)';
    ctx.lineWidth   = 1.5 * dpr;
    ctx.beginPath();
    ctx.moveTo(lineX, padT);
    ctx.lineTo(lineX, padT + H);
    ctx.stroke();
    ctx.restore();
  });

  // Liquidation lines per open position
  itemPositions.forEach(function(pos) {
    var liqPrice = calcLiqPrice(pos);
    var liqY     = py(liqPrice);
    if (liqY < padT - 2 * dpr || liqY > padT + H + 2 * dpr) return;
    var isLong   = pos.direction === 'long';
    var liqColor = '#ff4500';

    ctx.save();
    ctx.strokeStyle = liqColor;
    ctx.lineWidth   = 1.5 * dpr;
    ctx.beginPath();
    ctx.moveTo(pad, liqY);
    ctx.lineTo(pad + W, liqY);
    ctx.stroke();

    if (!miniMode) {
      ctx.fillStyle = liqColor;
      ctx.font = 'bold ' + (8 * dpr) + 'px monospace';
      ctx.textAlign = 'right';
      var label = 'LIQ ' + (isLong ? '▲' : '▼') + ' ' + Math.round(liqPrice);
      ctx.fillText(label, pad + W - 1 * dpr, liqY - 2 * dpr);
      ctx.textAlign = 'left';
      var triSize = 5 * dpr;
      ctx.beginPath();
      ctx.moveTo(pad + W, liqY);
      ctx.lineTo(pad + W - triSize, liqY - triSize);
      ctx.lineTo(pad + W - triSize, liqY + triSize);
      ctx.closePath();
      ctx.fillStyle = liqColor;
      ctx.fill();
    }
    ctx.restore();
  });
}

function calcLiqPrice(pos) {
  var units = (pos.margin / pos.entryPrice) * pos.leverage;
  var liqLoss = pos.margin * 0.95;
  if (pos.direction === 'long') {
    return pos.entryPrice - liqLoss / units;
  } else {
    return pos.entryPrice + liqLoss / units;
  }
}

function drawChart(el, item, miniMode) {
  if (chartMode === 'candle') {
    drawCandleChart(el, item.candles, miniMode, item);
  } else {
    drawLineChart(el, item.history, miniMode, item);
  }
}

// ── MODAL ─────────────────────────────────────────────────────────────────────
function openModal(itemId) {
  modalItemId = itemId; modalLev = 1;
  var item = getItem(itemId);
  document.getElementById('modal-title').textContent = item.icon + '  ' + item.name;
  document.getElementById('modal-sl').value  = '';
  document.getElementById('modal-tp').value  = '';
  document.getElementById('modal-lot').value = '1';
  var slHint = document.getElementById('sl-price-hint');
  var tpHint = document.getElementById('tp-price-hint');
  if (slHint) { slHint.textContent = ''; slHint.className = 'sltp-price-hint sl-hint'; }
  if (tpHint) { tpHint.textContent = ''; tpHint.className = 'sltp-price-hint tp-hint'; }
  // clear preset active states
  document.querySelectorAll('.sltp-preset').forEach(function(b){ b.classList.remove('active'); });
  // Reset modalLev to 1 first so setLev(1) computes ratio 1/1 = no change
  modalLev = 1;
  setLev(1);

  // Position modal near the asset card
  var card = document.getElementById('card-' + itemId);
  var topOffset = 80; // default fallback
  if (card) {
    var rect = card.getBoundingClientRect();
    var scrollTop = window.pageYOffset || document.documentElement.scrollTop;
    topOffset = Math.max(70, rect.top + scrollTop - 10);
    // If card is near bottom, show above it
    var modalH = 520;
    if (rect.bottom + modalH > window.innerHeight + scrollTop) {
      topOffset = Math.max(70, rect.top + scrollTop - modalH - 10);
    }
  }
  document.getElementById('modal').style.marginTop = topOffset + 'px';
  document.getElementById('modal-overlay').classList.add('open');
  // Scroll overlay to show modal
  setTimeout(function() {
    var overlay = document.getElementById('modal-overlay');
    overlay.scrollTop = Math.max(0, topOffset - 60);
  }, 50);
}
function closeModal() { document.getElementById('modal-overlay').classList.remove('open'); }
document.getElementById('modal-overlay').addEventListener('click', function(e) {
  if (e.target.id === 'modal-overlay') closeModal();
});

function setLev(v) {
  var prevLev = modalLev;
  modalLev = v;
  document.querySelectorAll('.modal-lev-btn').forEach(function(b) {
    b.classList.toggle('active', +b.getAttribute('data-lev') === v);
  });
  // Auto-scale lots by leverage ratio
  var lotEl = document.getElementById('modal-lot');
  if (lotEl && prevLev > 0) {
    var currentLots = parseInt(lotEl.value) || 1;
    var newLots = Math.round(currentLots * (v / prevLev));
    lotEl.value = Math.max(1, Math.min(100, newLots));
  }
  updateModalCost();
}

function adjustLot(delta) {
  var el  = document.getElementById('modal-lot');
  var val = Math.max(1, Math.min(100, (parseInt(el.value) || 1) + delta));
  el.value = val;
  updateModalCost();
}

function getLots() {
  var v = parseInt(document.getElementById('modal-lot').value) || 1;
  return Math.max(1, Math.min(100, v));
}

function getTotalWealth() {
  var openPnl = 0;
  positions.forEach(function(pos) { openPnl += calcPnl(pos, getItem(pos.itemId).price); });
  var marginLocked = 0;
  positions.forEach(function(p) { marginLocked += p.margin; });
  return wallet + marginLocked + openPnl;
}

function setSlPct(pct) {
  document.getElementById('modal-sl').value = pct;
  document.querySelectorAll('.sl-preset').forEach(function(b){
    b.classList.toggle('active', parseFloat(b.textContent) === pct);
  });
  updateModalCost();
}

function setTpPct(pct) {
  document.getElementById('modal-tp').value = pct;
  document.querySelectorAll('.tp-preset').forEach(function(b){
    b.classList.toggle('active', parseFloat(b.textContent) === pct);
  });
  updateModalCost();
}

function pctToPrice(entryPrice, pct, dir, side, margin, leverage, lots) {
  // pct is % of margin (e.g. 25 = lose/gain 25% of margin)
  // units = how many "shares" the position controls
  var units = (margin / entryPrice) * leverage;
  var PvEMove = (margin * pct / 100) / units; // price move needed to hit that margin %

  if (side === 'sl') {
    return dir === 'long'
      ? entryPrice - PvEMove
      : entryPrice + PvEMove;
  } else {
    return dir === 'long'
      ? entryPrice + PvEMove
      : entryPrice - PvEMove;
  }
}

function updateModalCost() {
  var item    = getItem(modalItemId);
  if (!item) return;
  var lots     = getLots();
  var margin   = Math.round(item.price * MARGIN_RATE * lots);
  var notional = Math.round(item.price * modalLev * lots);
  var wealth   = getTotalWealth();
  var pctOfPort = wealth > 0 ? (margin / wealth * 100) : 0;
  var pctStr   = pctOfPort.toFixed(1) + '% of portfolio';
  var pctColor = pctOfPort >= 50 ? '#ff6b6b' : pctOfPort >= 25 ? '#f0c040' : 'var(--green)';

  document.getElementById('modal-price-display').innerHTML = 'Current Price: <strong>' + fmt(item.price) + ' PvE</strong>';
  document.getElementById('modal-margin').textContent   = fmt(margin) + ' PvE';
  document.getElementById('modal-notional').textContent = fmt(notional) + ' PvE (' + modalLev + 'x, ' + lots + ' lot' + (lots > 1 ? 's' : '') + ')';
  document.getElementById('modal-cash').textContent     = fmt(wallet) + ' PvE';
  var fee = Math.round(margin * 0.01);
  var feeEl = document.getElementById('modal-fee');
  if (feeEl) feeEl.textContent = fmt(fee) + ' PvE (1%)';
  var totalEl = document.getElementById('modal-total-cost');
  if (totalEl) totalEl.textContent = fmt(margin + fee) + ' PvE';

  var pctRow = document.getElementById('modal-pct-row');
  if (pctRow) { pctRow.textContent = pctStr; pctRow.style.color = pctColor; }

  // Update SL/TP price hints (show both long and short calculated price)
  var slVal = parseFloat(document.getElementById('modal-sl').value);
  var tpVal = parseFloat(document.getElementById('modal-tp').value);
  var slHint = document.getElementById('sl-price-hint');
  var tpHint = document.getElementById('tp-price-hint');

  if (slHint) {
    if (!isNaN(slVal) && slVal > 0) {
      var slLong  = Math.round(pctToPrice(item.price, slVal, 'long',  'sl', margin, modalLev, lots));
      var slShort = Math.round(pctToPrice(item.price, slVal, 'short', 'sl', margin, modalLev, lots));
      slHint.textContent = '▲ ' + fmt(slLong) + ' PvE  |  ▼ ' + fmt(slShort) + ' PvE';
    } else {
      slHint.textContent = '';
    }
  }

  if (tpHint) {
    if (!isNaN(tpVal) && tpVal > 0) {
      var tpLong  = Math.round(pctToPrice(item.price, tpVal, 'long',  'tp', margin, modalLev, lots));
      var tpShort = Math.round(pctToPrice(item.price, tpVal, 'short', 'tp', margin, modalLev, lots));
      tpHint.textContent = '▲ ' + fmt(tpLong) + ' PvE  |  ▼ ' + fmt(tpShort) + ' PvE';
    } else {
      tpHint.textContent = '';
    }
  }

  var ok = wallet >= (margin + Math.round(margin * 0.01));
  document.getElementById('modal-btn-long').disabled  = !ok;
  document.getElementById('modal-btn-short').disabled = !ok;
}

function openTrade(dir) {
  var item   = getItem(modalItemId);
  var lots   = getLots();
  var margin = Math.round(item.price * MARGIN_RATE * lots);
  var fee    = Math.round(margin * 0.01);
  if (wallet < margin + fee) { showToast('Not enough gold for margin + fee!', 'error'); return; }

  var slRaw = parseFloat(document.getElementById('modal-sl').value);
  var tpRaw = parseFloat(document.getElementById('modal-tp').value);

  // SL is required
  if (isNaN(slRaw) || slRaw <= 0) {
    showToast('Stop Loss is required — enter a % to protect your position', 'error');
    document.getElementById('modal-sl').focus();
    return;
  }
  if (slRaw >= 100) { showToast('Stop Loss cannot be 100% or more', 'error'); return; }

  // Convert % of margin to actual PvE prices
  var sl = Math.round(pctToPrice(item.price, slRaw, dir, 'sl', margin, modalLev, lots) * 100) / 100;
  var tp = (!isNaN(tpRaw) && tpRaw > 0)
    ? Math.round(pctToPrice(item.price, tpRaw, dir, 'tp', margin, modalLev, lots) * 100) / 100
    : null;

  // Sanity checks (shouldn't fail with pctToPrice but belt-and-braces)
  if (dir === 'long') {
    if (sl >= item.price) { showToast('Stop Loss % too large — would exceed current price', 'error'); return; }
    if (tp !== null && tp <= item.price) { showToast('Take Profit % too small for Long', 'error'); return; }
  } else {
    if (sl <= item.price) { showToast('Stop Loss % too large — would exceed current price', 'error'); return; }
    if (tp !== null && tp >= item.price) { showToast('Take Profit % too small for Short', 'error'); return; }
  }

  var wealth    = getTotalWealth();
  var pctOfPort = wealth > 0 ? (margin / wealth * 100) : 0;

  pendingTrade = { dir: dir, item: item, margin: margin, lots: lots, sl: sl, tp: tp };

  // 10x warning on high volatility pairs — most serious
  if (modalLev >= 10 && item.volatility >= 0.08) {
    document.getElementById('lev-warning-vol').textContent = 'Volatility: ' + (item.volatility * 100).toFixed(1) + '% per tick';
    document.getElementById('lev-warning-overlay').classList.add('open');
    return;
  }

  // 5x warning on all pairs — elevated but not critical
  if (modalLev === 5) {
    document.getElementById('lev5-warning-vol').textContent = 'Volatility: ' + (item.volatility * 100).toFixed(1) + '% per tick';
    document.getElementById('lev5-warning-overlay').classList.add('open');
    return;
  }

  if (pctOfPort >= 50) {
    document.getElementById('warning-pct').textContent = pctOfPort.toFixed(1) + '% of your portfolio';
    document.getElementById('warning-overlay').classList.add('open');
    return;
  }

  executeTrade(pendingTrade);
}

function executeTrade(trade) {
  var fee = Math.round(trade.margin * 0.01);
  wallet -= trade.margin + fee;
  positions.push({
    id: ++posIdCounter, itemId: modalItemId,
    direction: trade.dir, entryPrice: trade.item.price,
    leverage: modalLev, margin: trade.margin,
    lots: trade.lots, sl: trade.sl, tp: trade.tp, fee: fee
  });
  var d = trade.dir === 'long' ? 'Long' : 'Short';
  var slPct = trade.sl ? ' | SL @ ' + fmt(Math.round(trade.sl)) + ' PvE' : '';
  showToast(d + ' ' + modalLev + 'x x' + trade.lots + ' opened — ' + trade.item.name + ' @ ' + fmt(trade.item.price) + ' PvE (fee: ' + fmt(fee) + ' PvE)' + slPct, trade.dir === 'long' ? 'buy' : 'sell');
  closeModal();
  pendingTrade = null;
  scheduleSave();
  renderAll();
}

function confirmTrade() {
  closeWarning();
  if (pendingTrade) executeTrade(pendingTrade);
}

function closeWarning() {
  document.getElementById('warning-overlay').classList.remove('open');
}

function closeLevWarning() {
  document.getElementById('lev-warning-overlay').classList.remove('open');
  pendingTrade = null;
}

function closeLev5Warning() {
  document.getElementById('lev5-warning-overlay').classList.remove('open');
  pendingTrade = null;
}

function confirmLev5Trade() {
  document.getElementById('lev5-warning-overlay').classList.remove('open');
  if (!pendingTrade) return;
  // Still check position size warning after
  var wealth    = getTotalWealth();
  var pctOfPort = wealth > 0 ? (pendingTrade.margin / wealth * 100) : 0;
  if (pctOfPort >= 50) {
    document.getElementById('warning-pct').textContent = pctOfPort.toFixed(1) + '% of your portfolio';
    document.getElementById('warning-overlay').classList.add('open');
    return;
  }
  executeTrade(pendingTrade);
}

function confirmLevTrade() {
  document.getElementById('lev-warning-overlay').classList.remove('open');
  if (!pendingTrade) return;
  var wealth    = getTotalWealth();
  var pctOfPort = wealth > 0 ? (pendingTrade.margin / wealth * 100) : 0;
  if (pctOfPort >= 50) {
    document.getElementById('warning-pct').textContent = pctOfPort.toFixed(1) + '% of your portfolio';
    document.getElementById('warning-overlay').classList.add('open');
    return;
  }
  executeTrade(pendingTrade);
}

function closePosition(posId) {
  var pos = positions.filter(function(p){ return p.id === posId; })[0];
  if (!pos) return;
  var item = getItem(pos.itemId);
  var pnl  = calcPnl(pos, item.price);
  recordTrade(pos, item.price, pnl, 'manual');
  wallet  += Math.max(0, pos.margin + pnl);
  positions = positions.filter(function(p){ return p.id !== posId; });
  var d = pos.direction === 'long' ? 'Long' : 'Short';
  showToast('Closed ' + d + ' ' + item.name + ': ' + fmtPnl(Math.round(pnl)) + ' PvE', pnl >= 0 ? 'buy' : 'sell');
  scheduleSave();
  renderAll();
}

// ── TOAST ─────────────────────────────────────────────────────────────────────
var toastTimer;
function showToast(msg, type) {
  type = type || '';
  var t = document.getElementById('toast');
  t.textContent = msg;
  t.className = 'toast ' + type + ' show';
  clearTimeout(toastTimer);
  toastTimer = setTimeout(function(){ t.className = 'toast'; }, 2800);
}

// ── RENDER ────────────────────────────────────────────────────────────────────
function renderAll() {
  renderWallet(); renderMarket(); renderPortfolio(); renderHistory(); renderCloseAll();
}

function renderCloseAll() {
  var wrap = document.getElementById('market-close-all-wrap');
  if (!wrap) return;
  if (positions.length > 0) {
    wrap.innerHTML = '<button class="btn-close-all" onclick="closeAllPositions()">✕ Close All ' + positions.length + ' Position' + (positions.length > 1 ? 's' : '') + ' — Market Price</button>';
  } else {
    wrap.innerHTML = '';
  }
}

function renderWallet() {
  document.getElementById('wallet-display').textContent = fmt(wallet);
}

function renderMarket() {
  var container = document.getElementById('market-items');
  ITEMS.forEach(function(item) {
    var prev      = lastPrices[item.id] || item.price;
    var delta     = item.price - prev;
    var deltaPct  = pct(item.price, prev);
    var dir       = delta > 0 ? 'up' : (delta < 0 ? 'down' : '');
    var openPos   = positions.filter(function(p){ return p.itemId === item.id; });

    var card = document.getElementById('card-' + item.id);
    if (!card) {
      card = document.createElement('div');
      card.className = 'item-card';
      card.id = 'card-' + item.id;
      var h = '';
      h += '<div class="item-card-header">';
      h +=   '<div class="item-icon">' + item.icon + '</div>';
      h +=   '<div class="item-info">';
      h +=     '<div class="item-name">' + item.name + '</div>';
      h +=     '<div class="item-desc">' + item.desc + '</div>';
      h +=   '</div>';
      h +=   '<div class="item-price-block">';
      h +=     '<div class="item-price" id="price-' + item.id + '">' + fmt(item.price) + ' PvE</div>';
      h +=     '<div class="item-change" id="change-' + item.id + '"></div>';
      h +=   '</div>';
      h += '</div>';
      h += '<div class="chart-wrap"><canvas class="price-chart" id="mchart-' + item.id + '"></canvas><div class="chart-ohlc-bar" id="ohlc-' + item.id + '"></div></div>';
      h += '<div id="posbar-' + item.id + '"></div>';
      h += '<div class="market-close-row">';
      h +=   '<button class="btn-trade" style="margin:0;flex:1" onclick="openModal(\'' + item.id + '\')">Open Trade</button>';
      h +=   '<button class="btn-close-item" id="closeitem-' + item.id + '" onclick="closeItemPositions(\'' + item.id + '\')">Close</button>';
      h += '</div>';
      card.innerHTML = h;
      container.appendChild(card);
    }

    var priceEl = document.getElementById('price-' + item.id);
    priceEl.textContent = fmt(item.price) + ' PvE';
    priceEl.className = 'item-price ' + dir;
    setTimeout(function(){ priceEl.className = 'item-price'; }, 800);

    var changeEl = document.getElementById('change-' + item.id);
    if (delta !== 0) {
      var sign = delta > 0 ? '+' : '';
      changeEl.textContent = sign + fmt(Math.round(delta)) + ' (' + (deltaPct >= 0 ? '+' : '') + deltaPct.toFixed(1) + '%)';
      changeEl.className = 'item-change ' + (delta > 0 ? 'pos' : 'neg');
    }

    if (dir) {
      card.classList.add('flashing-' + dir);
      setTimeout(function(){ card.classList.remove('flashing-' + dir); }, 600);
    }

    var posbar = document.getElementById('posbar-' + item.id);
    if (openPos.length > 0) {
      var bars = '';
      openPos.forEach(function(pos) {
        var pnl      = calcPnl(pos, item.price);
        var dirLabel = pos.direction === 'long' ? 'LONG' : 'SHORT';
        var dirClass = pos.direction === 'long' ? 'long' : 'short';
        var pnlClass = pnl >= 0 ? 'pos' : 'neg';
        bars += '<div class="open-pos-bar">';
        bars += '<span class="pos-badge ' + dirClass + '">' + dirLabel + ' ' + pos.leverage + 'x</span>';
        bars += '<span style="font-size:.66rem;color:var(--text-dim)">@' + fmt(pos.entryPrice) + '</span>';
        if (pos.sl != null) bars += '<span style="font-size:.64rem;color:#ff9999">SL:' + fmt(pos.sl) + '</span>';
        if (pos.tp != null) bars += '<span style="font-size:.64rem;color:#99ffcc">TP:' + fmt(pos.tp) + '</span>';
        bars += '<span class="pos-pnl ' + pnlClass + '">' + fmtPnl(Math.round(pnl)) + ' PvE</span>';
        bars += '</div>';
      });
      posbar.innerHTML = bars;
    } else {
      posbar.innerHTML = '';
    }

    var canvas = document.getElementById('mchart-' + item.id);
    if (canvas && canvas.offsetWidth > 0) drawChart(canvas, item, false);

    // Update OHLC bar
    var ohlcBar = document.getElementById('ohlc-' + item.id);
    if (ohlcBar && item.candles.length) {
      var lc = item.candles[item.candles.length - 1];
      var cDir = lc.c >= lc.o ? 'var(--green)' : 'var(--red)';
      ohlcBar.innerHTML =
        'O:<span>' + fmt(lc.o) + '</span> ' +
        'H:<span style="color:#5deb8f">' + fmt(lc.h) + '</span> ' +
        'L:<span style="color:#ff6b6b">' + fmt(lc.l) + '</span> ' +
        'C:<span style="color:' + cDir + '">' + fmt(lc.c) + '</span>' +
        ' &nbsp; MA20:<span style="color:#f0c040" id="ma-' + item.id + '">--</span>';
      // Compute live MA20 for display
      var maVal = computeMA(item.candles, MA_PERIOD);
      var maEl = document.getElementById('ma-' + item.id);
      if (maEl) maEl.textContent = maVal !== null ? fmt(maVal) : '--';
    }

    // Update close-item button state
    var closeItemBtn = document.getElementById('closeitem-' + item.id);
    if (closeItemBtn) {
      var hasPos = positions.filter(function(p){ return p.itemId === item.id; }).length > 0;
      closeItemBtn.disabled = !hasPos;
    }
  });
}

function renderPortfolio() {
  var openPnl = 0;
  positions.forEach(function(pos) { openPnl += calcPnl(pos, getItem(pos.itemId).price); });
  var marginLocked = 0;
  positions.forEach(function(p) { marginLocked += p.margin; });
  var total  = wallet + marginLocked + openPnl;
  var pl     = total - STARTING_GP;
  var plPct  = pct(total, STARTING_GP);

  document.getElementById('port-total').textContent = fmt(total) + ' PvE';
  document.getElementById('port-cash').textContent  = fmt(wallet) + ' PvE';
  var openPlEl = document.getElementById('port-openpl');
  openPlEl.textContent  = fmtPnl(Math.round(openPnl)) + ' PvE';
  openPlEl.style.color  = openPnl >= 0 ? 'var(--green)' : 'var(--red)';
  var plEl = document.getElementById('port-pl');
  plEl.textContent = fmtPnl(Math.round(pl)) + ' PvE (' + fmtPct(plPct) + ')';
  plEl.className   = 'portfolio-pl ' + (pl >= 0 ? 'pos' : 'neg');

  var list = document.getElementById('positions-list');
  if (positions.length === 0) {
    list.innerHTML = '<div class="empty-holdings">No open positions, adventurer.<br>Visit the market to open a leveraged trade.</div>';
    return;
  }

  var html = '';
  positions.forEach(function(pos) {
    var item      = getItem(pos.itemId);
    var pnl       = calcPnl(pos, item.price);
    var pnlPct    = pct(pos.margin + pnl, pos.margin);
    var isLong    = pos.direction === 'long';
    var cardClass = isLong ? 'long-card' : 'short-card';
    var dirLabel  = isLong ? 'LONG' : 'SHORT';
    var pnlClass  = pnl >= 0 ? 'pos' : 'neg';
    var retColor  = pnlPct >= 0 ? 'var(--green)' : 'var(--red)';
    var slTag     = pos.sl != null
      ? '<span class="order-tag sl">SL: ' + fmt(pos.sl) + ' PvE</span>'
      : '<span class="order-tag none">No Stop Loss</span>';
    var tpTag     = pos.tp != null
      ? '<span class="order-tag tp">TP: ' + fmt(pos.tp) + ' PvE</span>'
      : '<span class="order-tag none">No Take Profit</span>';

    html += '<div class="position-card ' + cardClass + '">';
    html +=   '<div class="pos-card-top">';
    html +=     '<div class="pos-icon">' + item.icon + '</div>';
    html +=     '<div class="pos-card-name">' + item.name + '</div>';
    html +=     '<div class="pos-card-pnl ' + pnlClass + '">' + fmtPnl(Math.round(pnl)) + ' PvE</div>';
    html +=   '</div>';
    html +=   '<div class="pos-card-details">';
    html +=     '<div class="pos-card-detail">' + dirLabel + ' <span>' + pos.leverage + 'x</span></div>';
    html +=     '<div class="pos-card-detail">Lots <span>' + (pos.lots || 1) + '</span></div>';
    html +=     '<div class="pos-card-detail">Entry <span>' + fmt(pos.entryPrice) + '</span></div>';
    html +=   '</div>';
    html +=   '<div class="pos-card-details">';
    html +=     '<div class="pos-card-detail">Now <span>' + fmt(item.price) + '</span></div>';
    html +=     '<div class="pos-card-detail">Margin <span>' + fmt(pos.margin) + ' PvE</span></div>';
    html +=     '<div class="pos-card-detail">Fee paid <span style="color:#ff9999">' + fmt(pos.fee || 0) + ' PvE</span></div>';
    html +=     '<div class="pos-card-detail">Return <span style="color:' + retColor + '">' + fmtPct(pnlPct) + '</span></div>';
    html +=   '</div>';
    html +=   '<div class="pos-card-orders">' + slTag + tpTag + '</div>';
    html +=   '<button class="btn-close-card" onclick="closePosition(' + pos.id + ')">Close Position at Market Price</button>';
    html += '</div>';
  });
  list.innerHTML = html;
}

function computeMA(candles, period) {
  if (candles.length < period) return null;
  var sum = 0;
  for (var i = candles.length - period; i < candles.length; i++) sum += candles[i].c;
  return sum / period;
}

function setChartMode(mode) {
  chartMode = mode;
  var cBtn = document.getElementById('tog-candle');
  var lBtn = document.getElementById('tog-line');
  if (cBtn) cBtn.className = 'chart-tog-btn' + (mode === 'candle' ? ' active' : '');
  if (lBtn) lBtn.className = 'chart-tog-btn' + (mode === 'line'   ? ' active' : '');
  // Redraw all market canvases
  ITEMS.forEach(function(item) {
    var canvas = document.getElementById('mchart-' + item.id);
    if (canvas && canvas.offsetWidth > 0) drawChart(canvas, item, false);
  });
}

function renderHistory() {
  var statsEl = document.getElementById('hist-stats');
  var listEl  = document.getElementById('history-list');

  if (tradeHistory.length === 0) {
    statsEl.innerHTML =
      '<div class="hist-stat-card wide" style="padding:14px;">' +
        '<div class="hist-stat-label">No closed trades yet</div>' +
        '<div style="font-size:.8rem;color:var(--text-dim);margin-top:4px;font-style:italic;">Open and close positions to build your history</div>' +
      '</div>';
    listEl.innerHTML = '<div class="hist-empty"><div class="hist-empty-icon">📜</div>Your trade log is empty.</div>';
    return;
  }

  var wins  = tradeHistory.filter(function(t){ return t.pnl > 0; });
  var losses = tradeHistory.filter(function(t){ return t.pnl <= 0; });
  var totalPnl  = 0; tradeHistory.forEach(function(t){ totalPnl += t.pnl; });
  var winRate   = wins.length / tradeHistory.length * 100;
  var avgWin    = wins.length   ? wins.reduce(function(s,t){return s+t.pnl;},0)   / wins.length   : 0;
  var avgLoss   = losses.length ? losses.reduce(function(s,t){return s+t.pnl;},0) / losses.length : 0;
  var bestTrade  = Math.max.apply(null, tradeHistory.map(function(t){return t.pnl;}));
  var worstTrade = Math.min.apply(null, tradeHistory.map(function(t){return t.pnl;}));
  var tradeWord  = tradeHistory.length === 1 ? 'trade' : 'trades';
  var avgWinStr  = wins.length   ? ('+' + fmt(avgWin))  : '&mdash;';
  var avgLossStr = losses.length ? fmt(avgLoss)          : '&mdash;';
  var plClass    = totalPnl >= 0 ? 'pos' : 'neg';
  var wrClass    = winRate >= 50 ? 'pos' : 'neg';

  var stats = '';
  stats += '<div class="hist-stat-card wide">';
  stats +=   '<div class="hist-stat-label">Total Realised P&amp;L</div>';
  stats +=   '<div class="hist-stat-value ' + plClass + '">' + fmtPnl(totalPnl) + ' PvE</div>';
  stats +=   '<div class="hist-stat-sub">' + tradeHistory.length + ' ' + tradeWord + ' closed</div>';
  stats += '</div>';
  stats += '<div class="hist-stat-card">';
  stats +=   '<div class="hist-stat-label">Win Rate</div>';
  stats +=   '<div class="hist-stat-value ' + wrClass + '">' + winRate.toFixed(0) + '%</div>';
  stats +=   '<div class="win-rate-bar-wrap"><div class="win-rate-bar-fill" style="width:' + winRate + '%"></div></div>';
  stats +=   '<div class="hist-stat-sub">' + wins.length + 'W / ' + losses.length + 'L</div>';
  stats += '</div>';
  stats += '<div class="hist-stat-card">';
  stats +=   '<div class="hist-stat-label">Avg Win / Loss</div>';
  stats +=   '<div class="hist-stat-value neutral" style="font-size:.88rem;">';
  stats +=     '<span style="color:var(--green)">' + avgWinStr + '</span>';
  stats +=     '<span style="color:var(--text-dim);font-size:.75rem"> / </span>';
  stats +=     '<span style="color:var(--red)">' + avgLossStr + '</span>';
  stats +=   '</div>';
  stats +=   '<div class="hist-stat-sub">PvE per trade</div>';
  stats += '</div>';
  stats += '<div class="hist-stat-card">';
  stats +=   '<div class="hist-stat-label">Best Trade</div>';
  stats +=   '<div class="hist-stat-value pos">+' + fmt(bestTrade) + ' PvE</div>';
  stats += '</div>';
  stats += '<div class="hist-stat-card">';
  stats +=   '<div class="hist-stat-label">Worst Trade</div>';
  stats +=   '<div class="hist-stat-value neg">' + fmt(worstTrade) + ' PvE</div>';
  stats += '</div>';
  statsEl.innerHTML = stats;

  var filterItem    = document.getElementById('hist-filter-item').value;
  var filterOutcome = document.getElementById('hist-filter-outcome').value;
  var filtered = tradeHistory.filter(function(t) {
    if (filterItem !== 'all' && t.itemId !== filterItem) return false;
    if (filterOutcome === 'win'  && t.pnl <= 0) return false;
    if (filterOutcome === 'loss' && t.pnl > 0)  return false;
    return true;
  });

  if (filtered.length === 0) {
    listEl.innerHTML = '<div class="hist-empty"><div class="hist-empty-icon">📜</div>No trades match your filters.</div>';
    return;
  }

  var closeLabels  = { manual:'Manual', sl:'Stop Loss', tp:'Take Profit', liq:'Liquidated', ceiling:'Suspended' };
  var closeClasses = { manual:'manual', sl:'sl', tp:'tp', liq:'liq', ceiling:'liq' };

  var rows = '';
  filtered.forEach(function(t) {
    var isLong      = t.direction === 'long';
    var entryClass  = t.pnl > 0 ? 'win' : (t.closeReason === 'liq' ? 'liq' : 'loss');
    var dirClass    = isLong ? 'long' : 'short';
    var dirLabel    = isLong ? 'LONG' : 'SHORT';
    var pnlClass    = t.pnl >= 0 ? 'pos' : 'neg';
    var pctClass    = t.pnlPct >= 0 ? 'pos' : 'neg';
    var pnlPctStr   = (t.pnlPct >= 0 ? '+' : '') + t.pnlPct.toFixed(1) + '%';
    var priceDiff   = t.exitPrice - t.entryPrice;
    var diffStr     = (priceDiff >= 0 ? '+' : '') + fmt(Math.round(priceDiff));
    var diffColor   = priceDiff >= 0 ? 'var(--green)' : 'var(--red)';
    var rClass      = closeClasses[t.closeReason] || 'manual';
    var rLabel      = closeLabels[t.closeReason]  || 'Closed';

    rows += '<div class="hist-entry ' + entryClass + '">';
    rows +=   '<div class="hist-icon">' + t.itemIcon + '</div>';
    rows +=   '<div class="hist-body">';
    rows +=     '<div class="hist-top">';
    rows +=       '<div class="hist-item-name">' + t.itemName + '</div>';
    rows +=       '<span class="hist-dir-badge ' + dirClass + '">' + dirLabel + ' ' + t.leverage + 'x</span>';
    rows +=       '<span class="hist-close-tag ' + rClass + '">' + rLabel + '</span>';
    rows +=     '</div>';
    rows +=     '<div class="hist-details">';
    rows +=       '<span>Entry <span class="val">' + fmt(t.entryPrice) + '</span></span>';
    rows +=       '<span>Exit <span class="val">' + fmt(t.exitPrice) + '</span> <span style="color:' + diffColor + '">(' + diffStr + ')</span></span>';
    rows +=       '<span>Margin <span class="val">' + fmt(t.margin) + ' PvE</span></span>';
    rows +=     '</div>';
    rows +=     '<div class="hist-time">' + timeAgo(t.closedAt) + '</div>';
    rows +=   '</div>';
    rows +=   '<div>';
    rows +=     '<div class="hist-pnl ' + pnlClass + '">' + fmtPnl(t.pnl) + ' PvE</div>';
    rows +=     '<div class="hist-pnl-pct ' + pctClass + '">' + pnlPctStr + '</div>';
    rows +=   '</div>';
    rows += '</div>';
  });
  listEl.innerHTML = rows;
}

// ── SHARED MARKET STATE ──────────────────────────────────────────────────────
// One user is the "host" — they run the price engine and broadcast to Firebase.
// Everyone else reads from Firebase every tick instead of computing locally.
var isHost = false;
var hostCheckInterval = null;
var marketSyncInterval = null;
var MARKET_KEY = 'market_state';

async function electHost() {
  // Try to claim host role — first writer wins
  try {
    var existing = await fbGet(MARKET_KEY + '/host');
    var now = Date.now();
    // If no host, or host is stale (>8s old), claim it
    if (!existing || !existing.claimedAt || (now - existing.claimedAt) > 8000) {
      await fbSet(MARKET_KEY + '/host', { username: myUsername, claimedAt: now });
      // Verify we actually got it (race condition protection)
      var check = await fbGet(MARKET_KEY + '/host');
      if (check && check.username === myUsername) {
        isHost = true;
        console.log('Host role claimed by', myUsername);
        startHostEngine();
        return;
      }
    }
    isHost = false;
    startClientSync();
  } catch(e) {
    // If Firebase fails, run locally
    isHost = true;
    startHostEngine();
  }
}

function startHostEngine() {
  // Host renews claim every 5s and runs the price engine
  hostCheckInterval = setInterval(function() {
    fbSet(MARKET_KEY + '/host', { username: myUsername, claimedAt: Date.now() });
  }, 5000);
  // Host ticks at TICK_MS
  setInterval(hostTick, TICK_MS);
}

function startClientSync() {
  // Clients poll Firebase for market state every TICK_MS
  marketSyncInterval = setInterval(clientSync, TICK_MS);
  clientSync(); // immediate first sync
  // Clients still check if they should become host
  hostCheckInterval = setInterval(electHost, 6000);
}

async function hostTick() {
  tickCount++;
  // Session trend shift
  if (tickCount >= nextShiftAt) shiftTrends();
  // Price events
  maybeFireEvent();
  // Compute new prices locally
  applyEventPrices();
  var priceData = {};
  ITEMS.forEach(function(item) {
    lastPrices[item.id] = item.price;
    var chg = item.price * ((Math.random() - 0.48) * item.volatility + item.trend);
    item.price = Math.max(5, Math.round((item.price + chg) * 100) / 100);
    item.history.push(item.price);
    if (item.history.length > MAX_HISTORY) item.history.shift();
    item.candleTickBuf.push(item.price);
    if (item.candles.length === 0 || item.candleTickBuf.length === 1) {
      var prev = item.candles.length ? item.candles[item.candles.length - 1].c : item.price;
      item.candles.push({ o: prev, h: item.price, l: item.price, c: item.price });
    } else {
      var cur = item.candles[item.candles.length - 1];
      cur.h = Math.max(cur.h, item.price);
      cur.l = Math.min(cur.l, item.price);
      cur.c = item.price;
    }
    if (item.candleTickBuf.length >= CANDLE_TICKS) {
      item.candleTickBuf = [];
      if (item.candles.length > 60) item.candles.shift();
    }
    priceData[item.id] = {
      price: item.price,
      trend: item.trend,
      volatility: item.volatility,
      history: item.history.slice(-MAX_HISTORY),
      candles: item.candles.slice(-60)
    };
  });
  // Broadcast to Firebase
  try {
    await fbSet(MARKET_KEY + '/prices', {
      tickCount: tickCount,
      nextShiftAt: nextShiftAt,
      sessionBoundaries: sessionBoundaries.slice(-60),
      items: priceData,
      updatedAt: Date.now()
    });
  } catch(e) { /* broadcast failure — still run locally */ }

  // Update UI
  var dot = document.getElementById('tick-dot');
  if (dot) { dot.style.background = '#f0c040'; setTimeout(function(){ dot.style.background = 'var(--green)'; }, 300); }
  var lbl = document.getElementById('tick-label');
  if (lbl) lbl.textContent = 'Tick #' + tickCount;
  checkSLTP();
  checkPriceCeiling();
  renderAll();
}

async function clientSync() {
  try {
    var data = await fbGet(MARKET_KEY + '/prices');
    if (!data || !data.items) return;
    // Check data is fresh (< 6s old)
    if (Date.now() - data.updatedAt > 6000) return;

    // Sync tick count from host
    if (data.tickCount > tickCount) {
      tickCount = data.tickCount;
      nextShiftAt = data.nextShiftAt || nextShiftAt;
      if (data.sessionBoundaries) sessionBoundaries = data.sessionBoundaries;
    }

    // Sync prices from host
    ITEMS.forEach(function(item) {
      var d = data.items[item.id];
      if (!d) return;
      lastPrices[item.id] = item.price;
      item.price      = d.price;
      item.trend      = d.trend;
      item.volatility = d.volatility;
      if (d.history && d.history.length) item.history = d.history;
      if (d.candles && d.candles.length) item.candles = d.candles;
    });

    // Update UI
    var dot = document.getElementById('tick-dot');
    if (dot) { dot.style.background = '#f0c040'; setTimeout(function(){ dot.style.background = 'var(--green)'; }, 300); }
    var lbl = document.getElementById('tick-label');
    if (lbl) lbl.textContent = 'Tick #' + tickCount;
    checkSLTP();
    renderAll();
  } catch(e) { /* sync failure — keep last known state */ }
}

// ── TICK (legacy — only used as fallback) ──────────────────────────────────────────────────────────
function tick() {
  // This is now handled by hostTick / clientSync
  // Kept as no-op to avoid breaking any remaining references
}

// ── PAGE ──────────────────────────────────────────────────────────────────────
/* showPage defined in new JS */

function resetFundsAndHistory() {
  if (!confirm('Reset to 10,000 PvE and clear all trade history? Open positions will be closed. This cannot be undone.')) return;

  // Close all open positions at current market price
  positions.forEach(function(pos) {
    var item = getItem(pos.itemId);
    var pnl  = calcPnl(pos, item.price);
    wallet += Math.max(0, pos.margin + pnl);
  });
  positions = [];

  // Reset funds and history
  wallet       = STARTING_GP;
  tradeHistory = [];
  posIdCounter = 0;

  // Clear saved state
  saveState();
  scheduleSave();

  renderAll();
  showPage('market');
  showToast('Fresh start! 10,000 PvE loaded.', 'tp-hit');
}

// ═══ FIREBASE CONFIG ════════════════════════════════════════════════════════
// Free Firebase project — replace with your own if desired
const FB_URL = 'https://practicetournament-3887d-default-rtdb.firebaseio.com';

async function fbGet(path) {
  try {
    var r = await fetch(FB_URL + '/' + path + '.json');
    if (!r.ok) return null;
    return await r.json();
  } catch(e) { return null; }
}
async function fbSet(path, data) {
  try {
    var r = await fetch(FB_URL + '/' + path + '.json', {
      method: 'PUT',
      headers: {'Content-Type':'application/json'},
      body: JSON.stringify(data)
    });
    return r.ok;
  } catch(e) { return false; }
}
async function fbGetAll(path) {
  // Returns object of all children at path
  try {
    var r = await fetch(FB_URL + '/' + path + '.json');
    if (!r.ok) return {};
    var d = await r.json();
    return d || {};
  } catch(e) { return {}; }
}




// ── ONE-TIME DB WIPE (runs once on first load after deployment) ──────────────
async function wipeDatabaseOnce(){
  try{
    if(sessionStorage.getItem('tt_db_wiped_v2'))return;
    // Mark wiped FIRST so even if fetch fails we don't retry endlessly
    sessionStorage.setItem('tt_db_wiped_v2','1');
    var r = await fetch(FB_URL+'/.json',{method:'PUT',headers:{'Content-Type':'application/json'},body:'null'});
    if(r && r.ok){
      // Only clear localStorage keys from previous sessions (game state + old auth)
      // Do NOT clear tt_username/tt_wallet as those may have just been written
      Object.keys(localStorage).forEach(function(k){
        if(k.startsWith('tradeTogether_v1')) localStorage.removeItem(k);
      });
      console.log('Database wiped to clean slate.');
    }
  }catch(e){console.warn('DB wipe failed (network restricted):',e);}
}
// ═══ AUTH & GATE ════════════════════════════════════════════════════════════
var SAVE_KEY = 'tradeTogether_v1';
var saveTimer = null, myUsername = null, myWalletAddress = null;

// ── Tournament open time ─────────────────────────────────────────────────────
function etOff(){var n=new Date(),j=new Date(n.getFullYear(),0,1),l=new Date(n.getFullYear(),6,1);return n.getTimezoneOffset()<Math.max(j.getTimezoneOffset(),l.getTimezoneOffset())?4:5;}

// Tournament open: 6:12 PM ET on 3/25/2026 on 3/26/2025
function getTournamentOpen(){
  return new Date(Date.UTC(2026,2,25,18+etOff(),12,0));
}
function isTournamentOpen(){
  return new Date()>=new Date(Date.UTC(2026,2,25,18+etOff(),12,0));
}

// Warning banner: 6:13 PM ET on 3/25/2026
function getTournamentWarn(){
  return new Date(Date.UTC(2026,2,25,18+etOff(),13,0));
}
function isTournamentWarnTime(){
  return new Date()>=new Date(Date.UTC(2026,2,25,18+etOff(),13,0));
}

// Tournament close: 6:14 PM ET on 3/25/2026
function getTournamentClose(){
  return new Date(Date.UTC(2026,2,25,18+etOff(),14,0));
}
function isTournamentClosed(){
  return new Date()>=new Date(Date.UTC(2026,2,25,18+etOff(),14,0));
}

function pad2(n){return n<10?'0'+n:''+n;}

// Poll for warning + close events while game is running
var tournamentWarnFired = false;
var tournamentCloseFired = false;
function checkTournamentEvents(){
  if(!myUsername)return;
  var now = new Date();

  // 4:09 PM CT — show warning banner
  if(!tournamentWarnFired && isTournamentWarnTime() && !isTournamentClosed()){
    tournamentWarnFired = true;
    var diff = Math.max(0, getTournamentClose() - now);
    var secsLeft = Math.round(diff/1000);
    showTournamentWarning(secsLeft);
  }

  // 4:10 PM CT — force close all trades
  if(!tournamentCloseFired && isTournamentClosed()){
    tournamentCloseFired = true;
    forceTournamentClose();
  }
}

function showTournamentWarning(secsLeft){
  // Override the breaking news banner with the warning
  var bar   = document.getElementById('bn-bar');
  var label = document.getElementById('bn-label');
  if(bar){
    bar.style.background = 'linear-gradient(135deg,#7a4a00,#b8860b,#7a4a00)';
    bar.style.borderBottomColor = '#f0c040';
  }
  if(label){ label.style.background='#8a6a00'; label.textContent='\u26a0\ufe0f Warning'; }
  document.getElementById('bn-headline').textContent = '\u23f1 All trades close at 6:14 PM ET \u2014 1 minute remaining!';
  document.getElementById('bn-sub').textContent      = 'All open positions will be settled automatically. Check the \ud83c\udfc6 Leaderboard tab for standings.';
  document.getElementById('bn-countdown').textContent = secsLeft + 's';
  document.getElementById('breaking-news').classList.add('show');

  // Live countdown in the banner
  var warnInterval = setInterval(function(){
    var d = Math.max(0, getTournamentClose() - new Date());
    var s = Math.round(d/1000);
    var cEl = document.getElementById('bn-countdown');
    if(cEl) cEl.textContent = s + 's';
    if(d<=0) clearInterval(warnInterval);
  }, 1000);
}

function forceTournamentClose(){
  // Close all open positions at market price
  if(positions.length > 0){
    var totalPnl = 0;
    positions.slice().forEach(function(pos){
      var item = getItem(pos.itemId);
      var pnl  = calcPnl(pos, item.price);
      totalPnl += pnl;
      recordTrade(pos, item.price, pnl, 'manual');
      wallet += Math.max(0, pos.margin + pnl);
    });
    positions = [];
    saveState();
    renderAll();
    showToast('Tournament ended \u2014 all trades closed! Final P&L: ' + fmtPnl(Math.round(totalPnl)) + ' PvE', totalPnl >= 0 ? 'tp-hit' : 'sl-hit');
  }

  // Update banner to closed state
  var bar   = document.getElementById('bn-bar');
  var label = document.getElementById('bn-label');
  if(bar){
    bar.style.background = 'linear-gradient(135deg,#0a2a0a,#1a4a1a,#0a2a0a)';
    bar.style.borderBottomColor = '#27ae60';
  }
  if(label){ label.style.background='#1a5a1a'; label.textContent='\ud83c\udfc6 Final'; }
  document.getElementById('bn-headline').textContent = '\ud83c\udfc6 Tournament has ended! All trades have been settled.';
  document.getElementById('bn-sub').textContent      = 'View the \ud83c\udfc6 Leaderboard tab to see final standings and P&L rankings.';
  document.getElementById('bn-countdown').textContent = '';
  document.getElementById('breaking-news').classList.add('show');

  // Disable all trade buttons permanently
  document.querySelectorAll('.btn-trade, .btn-close-all, #modal-btn-long, #modal-btn-short').forEach(function(btn){
    btn.disabled = true;
    btn.style.opacity = '0.4';
    btn.style.cursor = 'not-allowed';
  });

  // Auto-navigate to leaderboard after 3 seconds
  setTimeout(function(){
    showPage('leaderboard');
  }, 3000);
}

var cdInt = null;
function startGateCountdown(){
  clearInterval(cdInt);
  function t(){
    if(isTournamentOpen()){clearInterval(cdInt);onTournamentOpen();return;}
    var d=Math.max(0,getTournamentOpen()-new Date());
    document.getElementById('cd-h').textContent=pad2(Math.floor(d/3600000));
    document.getElementById('cd-m').textContent=pad2(Math.floor((d%3600000)/60000));
    document.getElementById('cd-s').textContent=pad2(Math.floor((d%60000)/1000));
    var el=document.getElementById('cd-date');
    if(el){try{el.textContent=getTournamentOpen().toLocaleString('en-US',{weekday:'short',month:'short',day:'numeric',hour:'numeric',minute:'2-digit',timeZone:'America/New_York',timeZoneName:'short'});}catch(e){}}
  }
  t();cdInt=setInterval(t,1000);
}
function onTournamentOpen(){
  if(myUsername){enterGame();}
  else{
    ['cd-h','cd-m','cd-s'].forEach(function(id){document.getElementById(id).textContent='00';});
    var el=document.getElementById('cd-date');if(el)el.textContent='Tournament is now OPEN!';
    openAuthModal('login');
  }
}
function openAuthModal(panel){
  switchAuthPanel(panel);
  // Clear any previous errors
  document.getElementById('login-err').textContent='';
  document.getElementById('reg-err').textContent='';
  document.getElementById('auth-overlay').classList.add('open');
}
function closeAuthModal(){
  document.getElementById('auth-overlay').classList.remove('open');
  // Always force gate visible — never let user see a blank page
  var gate = document.getElementById('gate');
  if(gate){
    gate.classList.remove('hidden');
    gate.style.display = 'flex';
  }
  window.scrollTo(0,0);
}
function switchAuthPanel(panel){
  document.getElementById('auth-panel-login').style.display=panel==='login'?'':'none';
  document.getElementById('auth-panel-register').style.display=panel==='register'?'':'none';
}
document.addEventListener('DOMContentLoaded',function(){
  document.getElementById('auth-overlay').addEventListener('click',function(e){if(e.target===this)closeAuthModal();});
  ['login-user','login-pass'].forEach(function(id){var el=document.getElementById(id);if(el)el.addEventListener('keydown',function(e){if(e.key==='Enter')doLogin();});});
  ['reg-user','reg-pass','reg-pass2','reg-wallet'].forEach(function(id){var el=document.getElementById(id);if(el)el.addEventListener('keydown',function(e){if(e.key==='Enter')doRegister();});});
});

async function hashPw(pw){
  try{var b=await crypto.subtle.digest('SHA-256',new TextEncoder().encode(pw));return Array.from(new Uint8Array(b)).map(function(x){return x.toString(16).padStart(2,'0');}).join('');}
  catch(e){var h=0;for(var i=0;i<pw.length;i++)h=(Math.imul(31,h)+pw.charCodeAt(i))|0;return 'fb_'+Math.abs(h).toString(16);}
}
function fmtWallet(a){if(!a)return '';if(a.length<=12)return a;return a.slice(0,6)+'\u2026'+a.slice(-4);}
function safeKey(s){return s.replace(/[^a-zA-Z0-9_-]/g,'_');}

async function doRegister(){
  var btn=document.getElementById('reg-btn'),err=document.getElementById('reg-err');
  var user=document.getElementById('reg-user').value.trim();
  var pw=document.getElementById('reg-pass').value;
  var pw2=document.getElementById('reg-pass2').value;
  var wal=document.getElementById('reg-wallet').value.trim();
  err.textContent='';
  if(!user||user.length<2){err.textContent='Username must be at least 2 characters.';return;}
  if(!/^[a-zA-Z0-9_]+$/.test(user)){err.textContent='Letters, numbers and underscores only.';return;}
  if(!pw||pw.length<6){err.textContent='Password must be at least 6 characters.';return;}
  if(pw!==pw2){err.textContent='Passwords do not match.';return;}
  if(!wal||wal.trim().length<8){err.textContent='Wallet address is required (min 8 chars).';return;}
  btn.disabled=true;btn.textContent='Checking\u2026';
  try{
    var userKey = safeKey(user.toLowerCase());
    // Wallet key: strip all non-alphanumeric for safe storage key
    var walStripped = wal.toLowerCase().replace(/[^a-z0-9]/g,'');
    var walKey = 'w_'+walStripped;

    // Check username not already taken
    var existing = await fbGet('accounts/'+userKey);
    if(existing){
      err.textContent='Username already taken. Choose another.';
      btn.disabled=false;btn.textContent='Create Account \u2192';return;
    }

    // Check wallet not already used — check both the index key AND scan accounts for raw match
    var walOwner = await fbGet('wallets/'+walKey);
    if(walOwner){
      err.textContent='This wallet address is already linked to another account.';
      btn.disabled=false;btn.textContent='Create Account \u2192';return;
    }
    // Secondary check: scan all accounts for same wallet (case-insensitive raw match)
    var allAccounts = await fbGet('accounts');
    if(allAccounts){
      var walLower = wal.toLowerCase().trim();
      var dupe = Object.values(allAccounts).some(function(a){
        return a && a.wallet && a.wallet.toLowerCase().trim()===walLower;
      });
      if(dupe){
        err.textContent='This wallet address is already linked to another account.';
        btn.disabled=false;btn.textContent='Create Account \u2192';return;
      }
    }

    // Create account record
    var hash = await hashPw(pw);
    var now  = Date.now();
    await fbSet('accounts/'+userKey, {
      username:user, passwordHash:hash, wallet:wal,
      createdAt:now, startingBalance:STARTING_GP
    });
    // Register wallet index to prevent duplicates
    await fbSet('wallets/'+walKey, userKey);

    // Pre-save starting balance of 10,000 PvE to Firebase so it's ready at unlock
    await fbSet('balances/'+userKey, {
      username:user, wallet:wal,
      balance:STARTING_GP, createdAt:now, initialized:true
    });

    myUsername=user; myWalletAddress=wal;
    localStorage.setItem('tt_username', myUsername);
    localStorage.setItem('tt_wallet',   myWalletAddress);
    // Also persist starting wallet balance locally so it's instant on enterGame
    var initState={
      wallet:STARTING_GP, positions:[], tradeHistory:[], posIdCounter:0,
      tickCount:0, chartMode:'candle', nextShiftAt:TREND_SHIFT_TICKS,
      sessionBoundaries:[], items:[], savedAt:now,
      username:user, walletAddress:wal,
      totalWealth:STARTING_GP, totalPnl:0
    };
    localStorage.setItem(SAVE_KEY+':'+user, JSON.stringify(initState));

    closeAuthModal();
    if(isTournamentOpen()){enterGame();return;}
    var msg = document.getElementById('cd-msg');
    if(msg) msg.textContent='\u2713 Account created! 10,000 PvE loaded. Welcome, '+user+'.';
  }catch(e){
    err.textContent='Registration failed: '+(e&&e.message?e.message:'check console');
    console.error('Register error:',e);
  }
  btn.disabled=false;btn.textContent='Create Account \u2192';
}

async function doLogin(){
  var btn=document.getElementById('login-btn'),err=document.getElementById('login-err');
  var user=document.getElementById('login-user').value.trim();
  var pw=document.getElementById('login-pass').value;
  err.textContent='';
  if(!user){err.textContent='Enter your username.';return;}
  if(!pw){err.textContent='Enter your password.';return;}
  btn.disabled=true;btn.textContent='Signing in\u2026';
  try{
    var userKey = safeKey(user.toLowerCase());
    var record  = await fbGet('accounts/'+userKey);
    if(!record){err.textContent='Username not found.';btn.disabled=false;btn.textContent='Sign In \u2192';return;}
    if(await hashPw(pw)!==record.passwordHash){err.textContent='Incorrect password.';btn.disabled=false;btn.textContent='Sign In \u2192';return;}
    myUsername=record.username; myWalletAddress=record.wallet;
    localStorage.setItem('tt_username', myUsername);
    localStorage.setItem('tt_wallet',   myWalletAddress);
    closeAuthModal();
    if(isTournamentOpen()){enterGame();return;}
    document.getElementById('cd-msg').textContent='\u2713 Signed in as '+myUsername+'. Waiting for open\u2026';
  }catch(e){err.textContent='Login failed: '+(e&&e.message?e.message:'check console');console.error(e);}
  btn.disabled=false;btn.textContent='Sign In \u2192';
}

function enterGame(){
  tournamentWarnFired = false;
  tournamentCloseFired = false;
  document.getElementById('gate').classList.add('hidden');
  document.getElementById('app-shell').classList.remove('app-hidden');
  document.getElementById('breaking-news').classList.remove('app-hidden');
  loadState().then(function(restored){
    renderAll(); saveState();
    // Elect host — first user drives prices, others sync from Firebase
    electHost();
    setInterval(saveState, 10000);
    setInterval(syncShared, 4000);
    setInterval(checkTournamentEvents, 5000);
    checkTournamentEvents();
    window.addEventListener('resize', function(){ renderMarket(); });
    var a=document.getElementById('wallet-addr-display');
    if(a&&myWalletAddress)a.textContent=fmtWallet(myWalletAddress);
    if(!restored) showToast('Welcome, '+myUsername+'! 10,000 PvE ready.','tp-hit');
    else showToast('Welcome back, '+myUsername+'!','tp-hit');
  });
}

// ═══ SAVE / LOAD / RESET ════════════════════════════════════════════════════
async function saveState(){
  if(!myUsername)return;
  try{
    var snaps=ITEMS.map(function(i){return{id:i.id,price:i.price,trend:i.trend,volatility:i.volatility,history:i.history.slice(-MAX_HISTORY),candles:i.candles.slice(-60)};});
    var op=0;positions.forEach(function(p){op+=calcPnl(p,getItem(p.itemId).price);});
    var lk=0;positions.forEach(function(p){lk+=p.margin;});
    var wealth=wallet+lk+op;
    var state={wallet:wallet,positions:positions,tradeHistory:tradeHistory,posIdCounter:posIdCounter,
      tickCount:tickCount,chartMode:chartMode,nextShiftAt:nextShiftAt,sessionBoundaries:sessionBoundaries,
      items:snaps,savedAt:Date.now(),username:myUsername,walletAddress:myWalletAddress,
      totalWealth:wealth,totalPnl:wealth-STARTING_GP};
    // Personal state saved to localStorage (fast, no network)
    localStorage.setItem(SAVE_KEY+':'+myUsername, JSON.stringify(state));
    // Shared leaderboard + live positions via Firebase
    var userKey = safeKey(myUsername.toLowerCase());
    await fbSet('leaderboard/'+userKey, {
      username:myUsername, walletAddress:myWalletAddress,
      totalWealth:wealth, totalPnl:wealth-STARTING_GP,
      positions:positions.length, savedAt:Date.now()
    });
    var live=positions.map(function(pos){
      var item=getItem(pos.itemId);
      return{id:pos.id,username:myUsername,walletAddress:myWalletAddress,
             itemId:pos.itemId,itemName:item.name,itemIcon:item.icon,
             direction:pos.direction,leverage:pos.leverage,entryPrice:pos.entryPrice,
             currentPrice:item.price,margin:pos.margin,pnl:Math.round(calcPnl(pos,item.price)),
             sl:pos.sl,tp:pos.tp,savedAt:Date.now()};
    });
    await fbSet('live/'+userKey, live.length ? live : null);
    var ind=document.getElementById('save-indicator');
    if(ind){ind.style.color='rgba(39,174,96,.8)';ind.textContent='&#128190; Saved';setTimeout(function(){ind.style.color='rgba(201,169,110,.4)';ind.textContent='&#128190; Auto-saving';},1500);}
  }catch(e){console.warn('Save failed:',e);}
}

async function loadState(){
  if(!myUsername)return false;
  try{
    // Try localStorage first (fast)
    var raw=localStorage.getItem(SAVE_KEY+':'+myUsername);
    if(raw){
      var s=JSON.parse(raw);
      if(s&&typeof s.wallet==='number'){
        wallet=s.wallet;positions=s.positions||[];tradeHistory=s.tradeHistory||[];
        posIdCounter=s.posIdCounter||0;tickCount=s.tickCount||0;chartMode=s.chartMode||'candle';
        nextShiftAt=s.nextShiftAt||TREND_SHIFT_TICKS;sessionBoundaries=s.sessionBoundaries||[];
        if(s.walletAddress)myWalletAddress=s.walletAddress;
        if(s.items)s.items.forEach(function(sn){
          var it=getItem(sn.id);if(!it)return;
          it.price=sn.price||it.price;it.trend=sn.trend||it.trend;it.volatility=sn.volatility||it.volatility;
          if(sn.history&&sn.history.length)it.history=sn.history;
          if(sn.candles&&sn.candles.length)it.candles=sn.candles;
          lastPrices[it.id]=it.price;
        });
        return true;
      }
    }
    // No local state — check Firebase for pre-saved balance (registered on another device)
    var userKey = safeKey(myUsername.toLowerCase());
    var fbBal = await fbGet('balances/'+userKey);
    if(fbBal&&fbBal.initialized){
      wallet = typeof fbBal.balance==='number' ? fbBal.balance : STARTING_GP;
      // Save to localStorage so next load is instant
      var initState={
        wallet:wallet,positions:[],tradeHistory:[],posIdCounter:0,
        tickCount:0,chartMode:'candle',nextShiftAt:TREND_SHIFT_TICKS,
        sessionBoundaries:[],items:[],savedAt:Date.now(),
        username:myUsername,walletAddress:myWalletAddress,
        totalWealth:wallet,totalPnl:wallet-STARTING_GP
      };
      localStorage.setItem(SAVE_KEY+':'+myUsername, JSON.stringify(initState));
      return true;
    }
    return false;
  }catch(e){console.warn('loadState error:',e);return false;}
}

function resetState(){
  if(!confirm('Reset everything and start with 10,000 PvE? This cannot be undone.'))return;
  wallet=STARTING_GP;positions=[];tradeHistory=[];posIdCounter=0;
  tickCount=0;chartMode='candle';nextShiftAt=TREND_SHIFT_TICKS;
  ITEMS.forEach(function(item){
    item.price=item.basePrice||item.price;item.history=[];item.candles=[];item.candleTickBuf=[];
    lastPrices[item.id]=item.price;
    for(var i=0;i<20;i++)item.history.push(item.price);
    for(var j=0;j<15;j++)item.candles.push({o:item.price,h:item.price,l:item.price,c:item.price});
  });
  renderAll();saveState();showToast('Fresh start! 10,000 PvE loaded.','buy');
}

function scheduleSave(){clearTimeout(saveTimer);saveTimer=setTimeout(saveState,5000);}

// ═══ SHARED SYNC ════════════════════════════════════════════════════════════
async function syncShared(){
  await saveState();
  var liveEl=document.getElementById('page-livetrades');
  var lbEl=document.getElementById('page-leaderboard');
  if(liveEl&&liveEl.classList.contains('active'))await renderLiveTrades();
  if(lbEl&&lbEl.classList.contains('active'))await renderLeaderboard();
}

async function renderLiveTrades(){
  var con=document.getElementById('live-table-container'),cE=document.getElementById('live-count');
  if(!con)return;
  try{
    var allData=await fbGetAll('live');
    var all=[];
    Object.values(allData).forEach(function(arr){
      if(!Array.isArray(arr))return;
      arr.filter(function(p){return Date.now()-p.savedAt<60000;}).forEach(function(p){all.push(p);});
    });
    if(cE)cE.textContent=all.length+' position'+(all.length!==1?'s':'');
    if(!all.length){con.innerHTML='<div class="hist-empty"><div class="hist-empty-icon">&#128225;</div>No live trades right now.</div>';return;}
    all.sort(function(a,b){return Math.abs(b.pnl)-Math.abs(a.pnl);});
    var h='<div class="live-table-wrap"><table class="live-table"><thead><tr>';
    h+='<th>Trader</th><th>Item</th><th>Dir</th><th>Lev</th><th>Entry</th><th>Now</th><th>SL</th><th>TP</th><th>P&amp;L</th>';
    h+='</tr></thead><tbody>';
    all.forEach(function(pos){
      var isMe=pos.username===myUsername,pc=pos.pnl>=0?'lt-pnl-pos':'lt-pnl-neg';
      h+='<tr'+(isMe?' class="me-row"':'')+'>'+
        '<td><div class="lt-name">'+pos.username+(isMe?' <span class="lb-you-tag">you</span>':'')+'</div>'+
        '<div class="lt-wallet">'+fmtWallet(pos.walletAddress)+'</div></td>'+
        '<td>'+pos.itemIcon+' '+pos.itemName+'</td>'+
        '<td><span class="lt-badge '+pos.direction+'">'+pos.direction.toUpperCase()+'</span></td>'+
        '<td>'+pos.leverage+'x</td><td>'+fmt(pos.entryPrice)+'</td><td>'+fmt(pos.currentPrice)+'</td>'+
        '<td>'+(pos.sl!=null?'<span style="color:#ff9999">'+fmt(pos.sl)+'</span>':'<span style="color:var(--text-dim)">&mdash;</span>')+'</td>'+
        '<td>'+(pos.tp!=null?'<span style="color:#99ffcc">'+fmt(pos.tp)+'</span>':'<span style="color:var(--text-dim)">&mdash;</span>')+'</td>'+
        '<td class="'+pc+'">'+fmtPnl(pos.pnl)+' PvE</td></tr>';
    });
    h+='</tbody></table></div>';
    con.innerHTML=h;
  }catch(e){con.innerHTML='<div class="hist-empty"><div class="hist-empty-icon">&#128225;</div>Could not load live trades.</div>';console.warn(e);}
}

async function renderLeaderboard(){
  var con=document.getElementById('lb-table-container');
  if(!con)return;
  try{
    var allData=await fbGetAll('leaderboard');
    var players=Object.values(allData).filter(function(p){return p&&p.username;});
    if(!players.length){con.innerHTML='<div class="hist-empty"><div class="hist-empty-icon">&#127942;</div>No players yet.</div>';return;}
    players.sort(function(a,b){return b.totalPnl-a.totalPnl;});
    var medals=['&#129351;','&#129352;','&#129353;'];
    var h='<div class="lb-table-wrap"><table class="lb-table"><thead><tr>';
    h+='<th>#</th><th>Trader</th><th>Wallet</th><th>Open</th><th style="text-align:right">Total PvE</th><th style="text-align:right">P&amp;L</th>';
    h+='</tr></thead><tbody>';
    players.forEach(function(p,idx){
      var isMe=p.username===myUsername,rank=idx+1;
      var rs=rank<=3?medals[idx]:'#'+rank,pc=p.totalPnl>=0?'pos':'neg';
      h+='<tr'+(isMe?' class="me-row"':'')+'>'+
        '<td class="lb-rank-cell">'+rs+'</td>'+
        '<td><div class="lb-uname">'+p.username+(isMe?' <span class="lb-you-tag">you</span>':'')+'</div></td>'+
        '<td class="lb-wallet-cell">'+fmtWallet(p.walletAddress)+'</td>'+
        '<td style="text-align:center;font-size:.72rem;color:var(--text-dim)">'+(p.positions||0)+'</td>'+
        '<td class="lb-wealth-cell">'+fmt(Math.round(p.totalWealth))+' PvE</td>'+
        '<td class="lb-pnl-cell '+pc+'">'+fmtPnl(Math.round(p.totalPnl))+'</td></tr>';
    });
    h+='</tbody></table></div>';
    con.innerHTML=h;
  }catch(e){con.innerHTML='<div class="hist-empty"><div class="hist-empty-icon">&#127942;</div>Could not load leaderboard.</div>';console.warn(e);}
}

function showPage(name){
  document.querySelectorAll('.page').forEach(function(p){p.classList.remove('active');});
  document.querySelectorAll('.tab').forEach(function(t){t.classList.remove('active');});
  document.getElementById('page-'+name).classList.add('active');
  var pages=['market','portfolio','livetrades','leaderboard','history'];
  var idx=pages.indexOf(name);if(idx>=0)document.querySelectorAll('.tab')[idx].classList.add('active');
  if(name==='history')renderHistory();
  if(name==='livetrades'){saveState().then(renderLiveTrades);}
  if(name==='leaderboard'){saveState().then(renderLeaderboard);}
}

window.addEventListener('load', function(){
  // Wipe DB to clean slate on first load
  wipeDatabaseOnce();
  var savedUser   = localStorage.getItem('tt_username');
  var savedWallet = localStorage.getItem('tt_wallet');
  if(savedUser){
    myUsername      = savedUser;
    myWalletAddress = savedWallet || '';
    if(isTournamentOpen()){enterGame();return;}
    document.getElementById('cd-msg').textContent='\u2713 Welcome back, '+myUsername+'. Waiting for open\u2026';
  }
  startGateCountdown();
});
</script>
</body>
</html>
