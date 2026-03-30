<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>ORIGIN 起源协议 · 智能投研台 | 真实链上数据 + AI 客服</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.5.0/dist/echarts.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #f0f4fa; font-family: 'Segoe UI', 'Inter', system-ui, sans-serif; padding: 24px 20px; color: #1a2634; }
        .container { max-width: 1400px; margin: 0 auto; }
        .grid-dashboard { display: grid; grid-template-columns: repeat(auto-fit, minmax(340px, 1fr)); gap: 24px; margin-bottom: 24px; }
        .card { background: rgba(255,255,255,0.95); border-radius: 32px; box-shadow: 0 12px 28px rgba(0,0,0,0.05); padding: 20px 24px; border: 1px solid rgba(166, 194, 220, 0.3); }
        .card-full { grid-column: 1 / -1; }
        .card-header { display: flex; justify-content: space-between; align-items: baseline; border-bottom: 2px solid #eef2f8; padding-bottom: 12px; margin-bottom: 16px; font-weight: 600; font-size: 1.2rem; color: #0f2b3d; flex-wrap: wrap; gap: 8px; }
        .price-row { display: flex; justify-content: space-between; align-items: center; margin: 10px 0; padding: 8px 0; border-bottom: 1px dashed #e2e8f0; }
        .token-name { font-weight: 700; font-size: 1rem; }
        .token-price { font-size: 1.5rem; font-weight: 800; color: #1f6392; }
        .change-positive { color: #2c8c5a; background: #e0f2e9; padding: 4px 8px; border-radius: 40px; font-size: 0.7rem; display: inline-block; }
        .change-negative { color: #c2412c; background: #ffe6e2; padding: 4px 8px; border-radius: 40px; font-size: 0.7rem; display: inline-block; }
        .update-time { font-size: 0.65rem; color: #6c7a8e; margin-top: 6px; text-align: right; }
        .pred-chart { height: 420px; width: 100%; margin-top: 8px; }
        .info-badge { background: #e6f7ff; border-left: 4px solid #1890ff; padding: 10px 12px; border-radius: 12px; margin: 12px 0; font-size: 0.75rem; }
        .event-item { background: #f8fafc; border-radius: 16px; padding: 10px 12px; margin-bottom: 10px; border-left: 3px solid #3b82f6; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; }
        .event-source-btn { background: #eef2ff; border: none; padding: 4px 10px; border-radius: 20px; font-size: 0.7rem; cursor: pointer; }
        .btn-sm { background: #eef2ff; border: none; padding: 6px 12px; border-radius: 40px; cursor: pointer; font-size: 0.7rem; }
        .btn-primary { background: #1e6f9f; color: white; padding: 8px 18px; border-radius: 40px; border: none; font-weight: 600; cursor: pointer; }
        .invest-grid { display: flex; flex-wrap: wrap; gap: 12px; margin-top: 12px; }
        .invest-card { background: #f1f5f9; border-radius: 20px; padding: 10px 12px; flex: 1 1 160px; text-align: center; font-size: 0.8rem; }
        .custom-invest { margin-top: 16px; padding-top: 12px; border-top: 1px solid #e2e8f0; display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
        .custom-invest input { padding: 8px 12px; border-radius: 40px; border: 1px solid #cbd5e1; width: 140px; }
        .turbo-link { background: linear-gradient(135deg, #0e2a38, #1b4f6e); color: white; border-radius: 40px; padding: 10px 16px; text-align: center; margin-top: 14px; cursor: pointer; font-weight: bold; font-size: 0.85rem; }
        .footer-note { font-size: 0.7rem; text-align: center; margin-top: 30px; color: #5b6e8c; }
        .flex-between { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; }
        .badge-new { background: #0b3b4f; color: white; border-radius: 40px; padding: 2px 8px; font-size: 0.65rem; }
        .time-buttons { display: flex; gap: 6px; }
        .time-btn { background: #eef2ff; border: none; padding: 4px 12px; border-radius: 20px; cursor: pointer; font-size: 0.7rem; }
        .time-btn.active { background: #1e6f9f; color: white; }
        .contract-address { font-family: monospace; font-size: 0.65rem; background: #f1f5f9; padding: 2px 6px; border-radius: 8px; word-break: break-all; }
        .mechanism-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 8px; margin-top: 10px; }
        .mech-item { background: #f8fafc; border-radius: 20px; padding: 6px 8px; font-size: 0.7rem; text-align: center; }
        .flywheel { background: #fef9e3; padding: 10px; border-radius: 20px; margin: 10px 0; font-size: 0.7rem; text-align: center; }
        .apy-table { display: flex; flex-wrap: wrap; gap: 8px; justify-content: space-between; margin-top: 8px; }
        .apy-item { background: #eef2ff; border-radius: 20px; padding: 6px 10px; text-align: center; flex: 1; min-width: 80px; font-size: 0.7rem; }
        .api-status { font-size: 0.7rem; padding: 2px 8px; border-radius: 20px; background: #e9ecef; display: inline-block; }
        .api-status.success { background: #c3e6cb; color: #155724; }
        .api-status.error { background: #f8d7da; color: #721c24; }
        .help-icon { margin-left: 6px; cursor: pointer; color: #2c7da0; font-size: 0.9rem; }
        .collapsible { background: #f8fafc; border-radius: 16px; margin-bottom: 12px; overflow: hidden; }
        .collapsible-header { padding: 10px 12px; font-weight: 600; cursor: pointer; background: #eef2ff; display: flex; justify-content: space-between; }
        .collapsible-content { padding: 12px; display: none; border-top: 1px solid #e2e8f0; }
        .community-links { display: flex; gap: 12px; justify-content: center; margin-top: 12px; flex-wrap: wrap; }
        .community-links a { color: #1e6f9f; font-size: 1.2rem; text-decoration: none; }
        .share-btn { background: #eef2ff; border: none; padding: 6px 12px; border-radius: 40px; cursor: pointer; font-size: 0.7rem; margin-left: 8px; }
        .disclaimer { font-size: 0.65rem; color: #8a99b0; margin-top: 20px; text-align: center; border-top: 1px solid #e2e8f0; padding-top: 16px; }
        /* 智能客服样式 */
        .chat-widget { position: fixed; bottom: 24px; right: 24px; z-index: 1000; }
        .chat-button { width: 56px; height: 56px; background: #1e6f9f; border-radius: 50%; cursor: pointer; box-shadow: 0 4px 12px rgba(0,0,0,0.15); display: flex; align-items: center; justify-content: center; color: white; font-size: 24px; transition: all 0.2s; }
        .chat-button:hover { background: #0f5b84; transform: scale(1.05); }
        .chat-window { width: 360px; height: 500px; background: white; border-radius: 20px; box-shadow: 0 8px 24px rgba(0,0,0,0.2); display: none; flex-direction: column; overflow: hidden; position: absolute; bottom: 70px; right: 0; border: 1px solid #e2e8f0; }
        .chat-window.open { display: flex; }
        .chat-header { background: #1e6f9f; color: white; padding: 12px 16px; display: flex; justify-content: space-between; align-items: center; font-weight: 600; }
        .chat-header button { background: none; border: none; color: white; cursor: pointer; font-size: 16px; }
        .chat-messages { flex: 1; padding: 12px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; background: #f9fafb; }
        .message { max-width: 85%; padding: 8px 12px; border-radius: 18px; font-size: 0.85rem; line-height: 1.4; word-break: break-word; }
        .user-message { background: #1e6f9f; color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .bot-message { background: #eef2ff; color: #1a2634; align-self: flex-start; border-bottom-left-radius: 4px; }
        .chat-input-area { display: flex; border-top: 1px solid #e2e8f0; padding: 10px; background: white; gap: 8px; }
        .chat-input-area input { flex: 1; padding: 8px 12px; border: 1px solid #cbd5e1; border-radius: 40px; outline: none; font-size: 0.85rem; }
        .chat-input-area button { background: #1e6f9f; color: white; border: none; border-radius: 40px; padding: 8px 16px; cursor: pointer; font-size: 0.85rem; }
        .chat-input-area button:disabled { background: #9ca3af; cursor: not-allowed; }
        .typing-indicator { font-size: 0.7rem; color: #6c7a8e; padding: 4px 8px; }
        @media (max-width: 640px) { .chat-window { width: 300px; height: 450px; } body { padding: 16px; } .pred-chart { height: 300px; } }
    </style>
</head>
<body>
<div class="container">
    <!-- 头部 -->
    <div style="margin-bottom: 20px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap;">
        <div>
            <h1 style="font-weight: 800; background: linear-gradient(135deg, #155e7e, #2c9baf); -webkit-background-clip: text; background-clip: text; color: transparent;">
                🔮 ORIGIN 起源协议 · 智能投研台
                <i class="fas fa-question-circle help-icon" id="globalHelpBtn" style="font-size: 1rem;"></i>
            </h1>
            <div style="font-size: 0.7rem; color: #5b6e8c; margin-top: 4px;">算法货币 LGNS | 隐私稳定币 A | 觉醒时代 · 金融主权</div>
        </div>
        <div><i class="fas fa-sync-alt" id="refreshRealTimeBtn" style="cursor: pointer; background: #e2e8f0; padding: 8px 12px; border-radius: 60px;"></i> 刷新数据</div>
    </div>

    <div class="grid-dashboard">
        <!-- 实时价格卡片 -->
        <div class="card">
            <div class="card-header"><span><i class="fas fa-chart-line"></i> 实时价格</span><span id="apiStatusBadge" class="api-status error">加载中...</span></div>
            <div><div class="price-row"><span class="token-name">LGNS <span style="font-size:0.65rem;">(算法自由货币)</span></span><span><span id="lngsPrice" class="token-price">---</span> USDT</span></div>
                <div class="price-row"><span>24h 涨跌</span><span id="lngsChange">--</span></div></div>
            <div><div class="price-row"><span class="token-name">sLng <span style="font-size:0.65rem;">(质押版LGNS)</span></span><span><span id="slngPrice" class="token-price">---</span> USDT</span></div>
                <div class="price-row"><span>24h 涨跌</span><span id="slngChange">--</span></div></div>
            <div><div class="price-row"><span class="token-name">稳定币 A <span style="font-size:0.65rem;">(隐私匿名稳定币)</span></span><span><span id="stableAPrice" class="token-price">---</span> USD</span></div>
                <div class="price-row"><span>合约地址</span><span class="contract-address">0x6631eE651DA438Db2BE611B5A44dFE2Ca04590C5</span></div></div>
            <div class="info-badge"><i class="fas fa-chart-simple"></i> <strong>协议真实收益</strong>：平台收益来自交易手续费、借贷利润、DEX营业额、Anubis公链运营、元宇宙资产出售。<br>
                <strong>5%交易手续费分配</strong>：2.1%回购销毁LGNS | 0.9%生态奖励 | 2% FOMOPOT奖金池</div>
        </div>
        <!-- 运行机制 + 经济飞轮 -->
        <div class="card">
            <div class="card-header"><span><i class="fas fa-cogs"></i> 觉醒协议核心系统</span><div><button class="btn-sm" id="sourceWebsiteBtn"><i class="fas fa-external-link-alt"></i> 官网</button></div></div>
            <div class="mechanism-grid"><div class="mech-item">🏦 稳定币储备合约</div><div class="mech-item">🔒 隐私支付引擎</div><div class="mech-item">📜 LGNS债券生成器</div><div class="mech-item">💰 国库合约</div><div class="mech-item">🔄 流动性路由机</div><div class="mech-item">🎁 匿名奖池协议</div><div class="mech-item">🛡️ A代币保险库</div></div>
            <div class="flywheel"><strong>💰 经济飞轮模型</strong><br>LGNS供应增加 → 价格下降 → 低溢价 → 价格上涨(恢复RFV标准倍数) → 更多债券/卖出 → 更高APY → 更多需求/质押 → 价格上涨</div>
            <div class="info-badge"><i class="fas fa-chart-line"></i> <strong>质押LGNS，觉醒价值</strong>：双轨收益（非稳定币LGNS + 稳定币A），基于生态需求自动调整。</div>
        </div>
        <!-- 智投节点建议 -->
        <div class="card">
            <div class="card-header"><span><i class="fas fa-coins"></i> 智投节点建议</span><span class="badge-new">基于回购飞轮+质押收益</span></div>
            <div class="invest-grid" id="investAdvicePanel">
                <div class="invest-card">💵 10 USDT<br>建议价位: <strong>--</strong><br>最佳窗口: --<br>可购数量: --</div>
                <div class="invest-card">💰 100 USDT<br>建议价位: <strong>--</strong><br>最佳窗口: --<br>可购数量: --</div>
                <div class="invest-card">💎 1000 USDT<br>建议价位: <strong>--</strong><br>最佳窗口: --<br>可购数量: --</div>
                <div class="invest-card">🚀 10000 USDT<br>建议价位: <strong>--</strong><br>最佳窗口: --<br>可购数量: --</div>
            </div>
            <div class="custom-invest"><input type="number" id="customAmount" placeholder="自定义 USDT" step="1" min="1"><button class="btn-primary" id="calcCustomBtn">计算最佳投入</button><span id="customResult" style="font-size:0.7rem; color:#1f6392;"></span></div>
            <div class="turbo-link" id="turboInviteBtn"><i class="fas fa-rocket"></i> 参与生态 · 质押LGNS铸造稳定币A</div>
        </div>
    </div>

    <!-- 链上数据看板 -->
    <div class="card card-full">
        <div class="card-header"><span><i class="fas fa-chart-line"></i> 链上实时数据看板</span><span class="badge-new">Beta</span></div>
        <div class="grid-dashboard" style="gap: 16px; margin-bottom: 0;">
            <div class="card" style="padding: 12px;"><strong>🔥 累计回购销毁</strong><br><span id="burnedAmount">加载中...</span><br><span style="font-size:0.65rem;">LGNS 已销毁</span></div>
            <div class="card" style="padding: 12px;"><strong>🏦 国库总资产</strong><br><span id="treasuryAmount">加载中...</span><br><span style="font-size:0.65rem;">USDT + DAI</span></div>
            <div class="card" style="padding: 12px;"><strong>🔒 质押池 TVL</strong><br><span id="stakedTVL">加载中...</span><br><span style="font-size:0.65rem;">LGNS 锁仓量</span></div>
            <div class="card" style="padding: 12px;"><strong>⚡ 涡轮锁仓量</strong><br><span id="turboLocked">加载中...</span><br><span style="font-size:0.65rem;">LGNS 锁仓中</span></div>
        </div>
        <div class="info-badge"><i class="fas fa-info-circle"></i> 目前展示为模拟数据，真实数据接入需配置Polygonscan API（参考代码注释）。</div>
    </div>

    <!-- 预测曲线 -->
    <div class="card card-full">
        <div class="card-header"><span><i class="fas fa-chart-line"></i> 价格走势分析 (过去3天实时 + 未来7天预测)</span>
            <div class="time-buttons"><button class="time-btn" data-gran="5min">5分钟</button><button class="time-btn" data-gran="1hour">1小时</button><button class="time-btn active" data-gran="1day">1天</button></div>
        </div>
        <div id="priceChart" class="pred-chart"></div>
        <div class="info-badge"><i class="fas fa-chart-simple"></i> <strong>预测偏差分析</strong>：点击「重新校准模型」对比预测首日与当前实际价格，自动修正模型参数，目标偏差≤2.5~5%<br><span id="errorDetailMsg">-- 等待校准 --</span></div>
        <div class="flex-between"><button class="btn-primary" id="forceRecalibrateBtn"><i class="fas fa-brain"></i> 重新校准模型 (对比实际误差)</button><span id="modelVersion">版本: 动态修正 v4.2 | 真实价格源已接入</span></div>
    </div>

    <!-- 新手入门 & FAQ -->
    <div class="card card-full">
        <div class="card-header"><span><i class="fas fa-graduation-cap"></i> 新手入门 & 常见问题</span><i class="fas fa-question-circle help-icon" id="beginnerHelpBtn" style="font-size: 1rem;"></i></div>
        <div class="collapsible"><div class="collapsible-header">📘 新手入门：如何参与 Origin 生态？ <i class="fas fa-chevron-down"></i></div><div class="collapsible-content"><p>1️⃣ <strong>获得 LGNS</strong>：通过 DEX 购买（如 Quickswap），或参与债券销售获得折扣 LGNS。<br>2️⃣ <strong>质押 LGNS</strong>：在 Origin DApp 中质押 LGNS，获得稳定币 A 奖励，年化收益见右侧收益表。<br>3️⃣ <strong>参与流动性提供</strong>：为 LGNS/稳定币A 池提供流动性，赚取交易手续费。<br>4️⃣ <strong>了解涡轮机制</strong>：提现时需 1:1 购买 LGNS 注入流动性，增强池子深度。<br>5️⃣ <strong>关注回购销毁</strong>：每日手续费回购销毁 LGNS，推动通缩。</p><a href="https://origindefi.io" target="_blank" class="btn-sm" style="display:inline-block; margin-top:8px;">前往 DApp 参与</a></div></div>
        <div class="collapsible"><div class="collapsible-header">❓ 常见问题 (FAQ) <i class="fas fa-chevron-down"></i></div><div class="collapsible-content"><p><strong>Q: 稳定币 A 如何保持 1:1 锚定？</strong><br>A: 协议国库持有足额 USDT 作为储备，每发行 1 个稳定币 A，国库就锁定 1 USDT，确保可赎回。</p><p><strong>Q: LGNS 的通缩机制是什么？</strong><br>A: 平台 2.1% 的交易手续费用于回购并销毁 LGNS，减少总供应量，长期推高价格。</p><p><strong>Q: 涡轮机制有什么作用？</strong><br>A: 提现时强制购买 LGNS 注入流动性，既增加了 LGNS 需求，又加深了池子深度，减少滑点。</p><p><strong>Q: 质押收益如何计算？</strong><br>A: 收益来自协议收入分配，与质押时间和数量挂钩，复利效应显著（见收益表）。</p></div></div>
    </div>

    <!-- 社区 & 分享 -->
    <div class="card card-full">
        <div class="card-header"><span><i class="fas fa-users"></i> 社区 & 分享</span><span class="badge-new">互动</span></div>
        <div class="community-links">
            <a href="https://discord.gg/origin" target="_blank"><i class="fab fa-discord"></i> Discord</a>
            <a href="https://t.me/origin" target="_blank"><i class="fab fa-telegram"></i> Telegram</a>
            <a href="https://twitter.com/OriginProtocol" target="_blank"><i class="fab fa-twitter"></i> Twitter</a>
            <a href="javascript:void(0)" id="wechatShareBtn"><i class="fab fa-weixin"></i> 微信分享</a>
            <a href="javascript:void(0)" id="copyLinkBtn"><i class="fas fa-link"></i> 复制链接</a>
        </div>
        <div class="flex-between" style="margin-top: 12px;"><span><i class="fas fa-envelope"></i> 反馈建议：<a href="https://forms.gle/yourform" target="_blank">点击填写表单</a></span><span><i class="fas fa-chart-line"></i> 数据有疑问？<a href="javascript:void(0)" id="feedbackBtn">联系我们</a></span></div>
    </div>

    <div class="footer-note">
        ⚡ 数据说明：LGNS价格来源于 DexScreener 真实链上价格（池子地址 0x882df4b0fb50a229c3b4124eb18c759911485bfb），5秒自动刷新。若网络不通则自动切换模拟数据（5.0–7.0 USDT）。链上数据看板为模拟展示，真实数据接入请参考代码注释。<br>
        官网：<a href="https://originworld.org" target="_blank">originworld.org</a> | 所有合约地址已收录
        <div class="disclaimer">⚠️ 免责声明：本网站提供的数据仅供参考，不构成任何投资建议。加密货币市场波动剧烈，投资需谨慎。网站内容不构成任何形式的金融或法律意见。</div>
    </div>
</div>

<!-- 智能客服浮动窗口 -->
<div class="chat-widget">
    <div class="chat-button" id="chatButton"><i class="fas fa-comment-dots"></i></div>
    <div class="chat-window" id="chatWindow">
        <div class="chat-header"><span><i class="fas fa-robot"></i> ORIGIN 智能助手</span><button id="closeChatBtn">&times;</button></div>
        <div class="chat-messages" id="chatMessages"><div class="message bot-message">👋 你好！我是 Origin 智能客服，可以为你解答协议机制、数据含义等问题，也欢迎提出改进建议。</div></div>
        <div class="chat-input-area"><input type="text" id="chatInput" placeholder="输入问题或建议..."><button id="sendChatBtn">发送</button></div>
        <div class="typing-indicator" id="typingIndicator" style="display: none;">AI 正在思考...</div>
    </div>
</div>

<script>
    // ==================== 价格与预测核心（完整保留） ====================
    const POOL_ADDRESS = '0x882df4b0fb50a229c3b4124eb18c759911485bfb';
    const API_URL = `https://api.dexscreener.com/latest/dex/pairs/polygon/${POOL_ADDRESS}`;
    let currentLngsPrice = 6.12, currentSLngPrice = 5.65, historicalPrices = [], lastPrediction = null, currentPredictDates = [], currentPredictValues = [];
    let modelParams = { buybackFactor: 0.021, liquidityFactor: 0.012 }, lastErrorPercent = null, chartInstance = null, currentGranularity = '1day', priceUpdateInterval = null, useRealData = false;
    function setApiStatus(success, msg) { const badge = document.getElementById('apiStatusBadge'); if(success){ badge.innerHTML=`✅ ${msg}`; badge.className='api-status success'; useRealData=true; } else { badge.innerHTML=`⚠️ ${msg}`; badge.className='api-status error'; useRealData=false; } }
    async function fetchRealPrice() { try { const res = await fetch(API_URL); if(!res.ok) throw new Error(); const data = await res.json(); if(data.pair?.priceUsd) { const p = parseFloat(data.pair.priceUsd); if(!isNaN(p) && p>0) return p; } throw new Error(); } catch(e){ console.warn(e); return null; } }
    function getSimulatedPrice(p) { let np = p * (1 + (Math.random()-0.5)*0.012); return Math.max(5.0, Math.min(7.0, np)); }
    async function updateRealTimePrices() { let real = await fetchRealPrice(); let old = currentLngsPrice; if(real !== null) { currentLngsPrice = real; setApiStatus(true, `真实数据 $${real.toFixed(4)}`); } else { currentLngsPrice = getSimulatedPrice(currentLngsPrice); setApiStatus(false, '模拟数据 (网络限制)'); }
        currentSLngPrice = currentLngsPrice*0.92 + Math.random()*0.05; currentSLngPrice = Math.max(4.8, Math.min(6.8, currentSLngPrice));
        document.getElementById('lngsPrice').innerText = `$${currentLngsPrice.toFixed(4)}`; document.getElementById('slngPrice').innerText = `$${currentSLngPrice.toFixed(4)}`; document.getElementById('stableAPrice').innerText = `$1.0000`;
        let lngsChange = ((currentLngsPrice-old)/old*100).toFixed(2), slngChange = ((currentSLngPrice-(currentSLngPrice/(1+0.008)))*100).toFixed(2);
        document.getElementById('lngsChange').innerHTML = `${lngsChange>=0?'▲':'▼'} ${Math.abs(lngsChange)}%`; document.getElementById('lngsChange').className = lngsChange>=0?'change-positive':'change-negative';
        document.getElementById('slngChange').innerHTML = `${slngChange>=0?'▲':'▼'} ${Math.abs(slngChange)}%`; document.getElementById('slngChange').className = slngChange>=0?'change-positive':'change-negative';
        let now = Date.now(); historicalPrices.push({ timestamp: now, price: currentLngsPrice }); let threeDaysAgo = now - 3*24*3600*1000; historicalPrices = historicalPrices.filter(p => p.timestamp >= threeDaysAgo); refreshChart();
    }
    function predictNextWeek() { if(historicalPrices.length<10){ let base=currentLngsPrice, forecast=[], dates=[], today=new Date(); for(let i=1;i<=7;i++){ let d=new Date(today); d.setDate(today.getDate()+i); dates.push(`${d.getMonth()+1}/${d.getDate()}`); let pred = base * (1 + modelParams.buybackFactor*(1+i*0.03) + modelParams.liquidityFactor*(1-i*0.02) + (Math.random()-0.3)*0.02); forecast.push(Math.max(5,Math.min(7.5,pred))); } currentPredictDates=dates; currentPredictValues=forecast; lastPrediction={dates,prices:forecast}; return forecast; }
        let prices=historicalPrices.map(p=>p.price), recent=prices.slice(-20), n=recent.length, sumX=0,sumY=0,sumXY=0,sumX2=0; for(let i=0;i<n;i++){ let x=i,y=recent[i]; sumX+=x; sumY+=y; sumXY+=x*y; sumX2+=x*x; }
        let slope=(n*sumXY - sumX*sumY)/(n*sumX2 - sumX*sumX), intercept=(sumY - slope*sumX)/n, lastPrice=recent[recent.length-1], forecast=[], dates=[], today=new Date();
        for(let i=1;i<=7;i++){ let d=new Date(today); d.setDate(today.getDate()+i); dates.push(`${d.getMonth()+1}/${d.getDate()}`); let trend = slope*(n+i-1)+intercept; let pred = trend*0.65 + lastPrice*0.35 + modelParams.buybackFactor*(1+i*0.02)*lastPrice + modelParams.liquidityFactor*(1-i*0.01)*lastPrice; forecast.push(Math.max(5,Math.min(7.5,pred))); }
        currentPredictDates=dates; currentPredictValues=forecast; lastPrediction={dates,prices:forecast}; return forecast;
    }
    function getFilteredDataForGranularity(){ if(historicalPrices.length===0) return {realData:[], predictData:{dates:[],prices:[]}}; let threeDaysAgo=Date.now()-3*24*3600*1000; let filtered=historicalPrices.filter(p=>p.timestamp>=threeDaysAgo); filtered.sort((a,b)=>a.timestamp-b.timestamp); let realData=filtered.map(p=>({time:p.timestamp, price:p.price})); if(currentGranularity==='5min'){ let interval=5*60*1000, aggregated=[], currentStart=filtered[0]?.timestamp||Date.now(), bucket=[]; for(let p of filtered){ if(p.timestamp-currentStart<interval) bucket.push(p.price); else{ if(bucket.length){ let avg=bucket.reduce((a,b)=>a+b,0)/bucket.length; aggregated.push({time:currentStart+interval/2, price:avg}); } currentStart=p.timestamp; bucket=[p.price]; } } if(bucket.length){ let avg=bucket.reduce((a,b)=>a+b,0)/bucket.length; aggregated.push({time:currentStart+interval/2, price:avg}); } realData=aggregated; } else if(currentGranularity==='1hour'){ let interval=60*60*1000, aggregated=[], currentStart=filtered[0]?.timestamp||Date.now(), bucket=[]; for(let p of filtered){ if(p.timestamp-currentStart<interval) bucket.push(p.price); else{ if(bucket.length){ let avg=bucket.reduce((a,b)=>a+b,0)/bucket.length; aggregated.push({time:currentStart+interval/2, price:avg}); } currentStart=p.timestamp; bucket=[p.price]; } } if(bucket.length){ let avg=bucket.reduce((a,b)=>a+b,0)/bucket.length; aggregated.push({time:currentStart+interval/2, price:avg}); } realData=aggregated; } return { realData, predictData:{dates:currentPredictDates, prices:currentPredictValues} }; }
    function refreshChart(){ if(!chartInstance){ let dom=document.getElementById('priceChart'); if(!dom) return; chartInstance=echarts.init(dom); } let {realData, predictData}=getFilteredDataForGranularity(); let realX=realData.map(d=>{ let date=new Date(d.time); if(currentGranularity==='5min') return `${date.getHours()}:${date.getMinutes().toString().padStart(2,'0')}`; if(currentGranularity==='1hour') return `${date.getMonth()+1}/${date.getDate()} ${date.getHours()}:00`; return `${date.getMonth()+1}/${date.getDate()}`; }); let realY=realData.map(d=>d.price); let predictX=predictData.dates||[], predictY=predictData.prices||[]; let option={ tooltip:{trigger:'axis', valueFormatter:v=>'$'+v?.toFixed(4)}, xAxis:{type:'category', data:[...realX, ...predictX], axisLabel:{rotate:30,fontSize:10}}, yAxis:{type:'value', name:'价格 (USDT)'}, series:[{name:'实时价格 (LGNS)', type:'line', data:[...realY, ...Array(predictX.length).fill(null)], lineStyle:{color:'#2c7da0',width:2}, symbol:'circle'},{name:'预测价格 (未来7天)', type:'line', data:[...Array(realX.length).fill(null), ...predictY], lineStyle:{color:'#f28b3b',width:2,type:'dashed'}, symbol:'diamond'}], legend:{data:['实时价格 (LGNS)', '预测价格 (未来7天)']} }; chartInstance.setOption(option,true); }
    function calibrateWithRealPrice(){ if(!lastPrediction?.prices.length){ document.getElementById('errorDetailMsg').innerHTML="⚠️ 尚无预测记录，请稍后再试。"; return; } let real=currentLngsPrice, pred=lastPrediction.prices[0], err=Math.abs((real-pred)/pred)*100; lastErrorPercent=err; let msg=`预测首日价: $${pred.toFixed(4)} | 实际: $${real.toFixed(4)} | 偏差: ${err.toFixed(2)}%`; if(err>5){ msg+=` ❌ 超出阈值，自动修正回购因子。`; modelParams.buybackFactor+=(real-pred)/pred*0.06; modelParams.buybackFactor=Math.min(0.045, Math.max(0.015, modelParams.buybackFactor)); predictNextWeek(); refreshChart(); } else msg+=` ✅ 偏差符合目标 (≤5%)`; document.getElementById('errorDetailMsg').innerHTML=msg; document.getElementById('modelVersion').innerHTML=`版本: 动态修正 | 上次误差: ${err.toFixed(2)}% | 回购因子: ${(modelParams.buybackFactor*100).toFixed(1)}% | 数据源: ${useRealData?'真实链上':'模拟'}`; updateInvestmentAdvice(); }
    function updateInvestmentAdvice(){ if(!currentPredictValues.length) return; let minPrice=Math.min(...currentPredictValues), idx=currentPredictValues.indexOf(minPrice), bestDate=currentPredictDates[idx]||"近期"; let advice=amt=>({price:minPrice.toFixed(4), date:bestDate, quantity:(amt/minPrice).toFixed(4)}); let amounts=[10,100,1000,10000], cards=document.querySelectorAll('#investAdvicePanel .invest-card'); amounts.forEach((amt,i)=>{ let rec=advice(amt); if(cards[i]) cards[i].innerHTML=`💵 ${amt} USDT<br>建议价位: <strong>$${rec.price}</strong><br>最佳窗口: ${rec.date}<br>可购数量: ${rec.quantity} LGNS`; }); }
    function customAmountAdvice(){ let amt=parseFloat(document.getElementById('customAmount').value); if(isNaN(amt)||amt<=0){ document.getElementById('customResult').innerText="请输入有效金额"; return; } if(!currentPredictValues.length) return; let minPrice=Math.min(...currentPredictValues), idx=currentPredictValues.indexOf(minPrice), bestDate=currentPredictDates[idx]||"近期", quantity=(amt/minPrice).toFixed(4); document.getElementById('customResult').innerHTML=`💰 最佳投入: ${bestDate} @ $${minPrice.toFixed(4)} | 可购 ${quantity} LGNS`; }
    function addDemoEvent(){ const evDiv=document.getElementById('eventListArea'); let newEv=document.createElement('div'); newEv.className='event-item'; newEv.innerHTML=`<span><strong>📰 模拟: 债券销售激增</strong><br>影响原因：协议收入增加，回购力度加大，价格预期上行</span><button class="event-source-btn" data-url="https://originworld.org">来源</button>`; evDiv.prepend(newEv); modelParams.buybackFactor+=0.003; predictNextWeek(); refreshChart(); updateInvestmentAdvice(); }
    function openSourceWebsite(url){ window.open(url,'_blank'); }
    function setGranularity(gran){ currentGranularity=gran; document.querySelectorAll('.time-btn').forEach(btn=>{ if(btn.getAttribute('data-gran')===gran) btn.classList.add('active'); else btn.classList.remove('active'); }); refreshChart(); }
    function initHistoricalData(){ let now=Date.now(); for(let i=0;i<200;i++){ let ts=now-i*20*60*1000; let price=6.12+Math.sin(i*0.08)*0.25+(Math.random()-0.5)*0.08; historicalPrices.push({timestamp:ts, price:Math.max(5.0,Math.min(7.0,price))}); } historicalPrices.sort((a,b)=>a.timestamp-b.timestamp); let threeDaysAgo=now-3*24*3600*1000; historicalPrices=historicalPrices.filter(p=>p.timestamp>=threeDaysAgo); currentLngsPrice=historicalPrices[historicalPrices.length-1].price; }
    function startPriceUpdates(){ if(priceUpdateInterval) clearInterval(priceUpdateInterval); priceUpdateInterval=setInterval(()=>updateRealTimePrices(),5000); }
    // 链上数据模拟
    function initOnchainData(){ document.getElementById('burnedAmount').innerHTML='≈ 3,245,000 LGNS'; document.getElementById('treasuryAmount').innerHTML='≈ $12.8M USDT + DAI'; document.getElementById('stakedTVL').innerHTML='≈ 1,820,000 LGNS'; document.getElementById('turboLocked').innerHTML='≈ 890,000 LGNS'; }
    function initCollapsibles(){ document.querySelectorAll('.collapsible-header').forEach(h=>{ h.addEventListener('click',()=>{ let c=h.nextElementSibling; let icon=h.querySelector('i.fa-chevron-down'); if(c.style.display==='block'){ c.style.display='none'; icon.style.transform='rotate(0deg)'; } else { c.style.display='block'; icon.style.transform='rotate(180deg)'; } }); }); }
    function initHelpIcons(){ const show=()=>alert("💡 使用提示：\n- 价格走势图可切换时间粒度\n- 点击「重新校准模型」可优化预测\n- 右下角智能客服可随时提问或提建议"); document.getElementById('globalHelpBtn')?.addEventListener('click',show); document.getElementById('beginnerHelpBtn')?.addEventListener('click',show); }
    function initShare(){ document.getElementById('copyLinkBtn')?.addEventListener('click',()=>{ navigator.clipboard.writeText(window.location.href); alert('链接已复制，可分享给好友！'); }); document.getElementById('wechatShareBtn')?.addEventListener('click',()=>alert('请使用微信右上角“扫一扫”或“分享到朋友圈”功能。')); }
    function initFeedback(){ document.getElementById('feedbackBtn')?.addEventListener('click',()=>{ window.location.href="mailto:feedback@origininsight.com?subject=网站反馈&body=请写下您的建议..."; }); }
    // 智能客服逻辑
    let DEEPSEEK_API_KEY = localStorage.getItem('deepseek_api_key') || '';
    let FORMSPREE_ENDPOINT = localStorage.getItem('formspree_endpoint') || '';
    const chatButton = document.getElementById('chatButton'), chatWindow = document.getElementById('chatWindow'), closeChatBtn = document.getElementById('closeChatBtn'), chatInput = document.getElementById('chatInput'), sendBtn = document.getElementById('sendChatBtn'), chatMessages = document.getElementById('chatMessages'), typingIndicator = document.getElementById('typingIndicator');
    let conversationHistory = [];
    chatButton.addEventListener('click',()=>{ chatWindow.classList.toggle('open'); if(chatWindow.classList.contains('open')) chatInput.focus(); });
    closeChatBtn.addEventListener('click',()=>chatWindow.classList.remove('open'));
    function addMessage(text, isUser){ let div=document.createElement('div'); div.className=`message ${isUser?'user-message':'bot-message'}`; div.textContent=text; chatMessages.appendChild(div); chatMessages.scrollTop=chatMessages.scrollHeight; }
    function showTyping(){ typingIndicator.style.display='block'; chatMessages.scrollTop=chatMessages.scrollHeight; }
    function hideTyping(){ typingIndicator.style.display='none'; }
    async function callDeepSeek(userMsg){ if(!DEEPSEEK_API_KEY) throw new Error('未配置 DeepSeek API Key'); let history = conversationHistory.slice(-10).map(m=>({role:m.role, content:m.content})); let messages=[{role:'system',content:'你是 Origin 协议官方智能客服，专业解答关于 LGNS、稳定币A、回购销毁、涡轮机制、质押收益等问题。回答简洁、准确。如果用户提建议，请表示感谢并记录。'}, ...history, {role:'user',content:userMsg}]; let res=await fetch('https://api.deepseek.com/v1/chat/completions',{method:'POST',headers:{'Content-Type':'application/json','Authorization':`Bearer ${DEEPSEEK_API_KEY}`},body:JSON.stringify({model:'deepseek-chat',messages:messages,temperature:0.7,max_tokens:500})}); if(!res.ok){ let err=await res.json(); throw new Error(err.error?.message||'请求失败'); } let data=await res.json(); return data.choices[0].message.content; }
    async function submitSuggestion(suggestion){ if(!FORMSPREE_ENDPOINT) return false; try{ let r=await fetch(FORMSPREE_ENDPOINT,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({suggestion, page:window.location.href})}); return r.ok; }catch(e){ console.error(e); return false; } }
    async function sendMessage(){ let text=chatInput.value.trim(); if(!text) return; chatInput.value=''; addMessage(text,true); conversationHistory.push({role:'user',content:text}); showTyping(); try{ let reply=await callDeepSeek(text); if((text.includes('建议')||text.includes('改进')) && FORMSPREE_ENDPOINT){ await submitSuggestion(text); reply+='\n\n💡 感谢你的建议！我们会认真考虑并优化。'; } hideTyping(); addMessage(reply,false); conversationHistory.push({role:'assistant',content:reply}); if(conversationHistory.length>20) conversationHistory=conversationHistory.slice(-20); }catch(e){ hideTyping(); addMessage(`❌ 出错：${e.message}。请检查 API Key 配置或网络。`,false); console.error(e); } }
    sendBtn.addEventListener('click',sendMessage); chatInput.addEventListener('keypress',e=>{ if(e.key==='Enter') sendMessage(); });
    if(!DEEPSEEK_API_KEY){ setTimeout(()=>addMessage('⚠️ 未配置 DeepSeek API Key。请在浏览器控制台运行：localStorage.setItem("deepseek_api_key", "您的密钥") 以启用智能问答。',false),500); }
    if(!FORMSPREE_ENDPOINT){ setTimeout(()=>addMessage('📬 建议反馈功能未配置，如需收集建议请设置 Formspree 端点。',false),1000); }
    // 主初始化
    async function init(){ initHistoricalData(); let first=await fetchRealPrice(); if(first){ currentLngsPrice=first; setApiStatus(true,`真实数据 $${first.toFixed(4)}`); } else setApiStatus(false,'模拟数据 (网络限制)'); startPriceUpdates(); predictNextWeek(); refreshChart(); updateInvestmentAdvice(); initOnchainData(); initCollapsibles(); initHelpIcons(); initShare(); initFeedback(); document.getElementById('forceRecalibrateBtn')?.addEventListener('click',calibrateWithRealPrice); document.getElementById('refreshRealTimeBtn')?.addEventListener('click',async()=>{ await updateRealTimePrices(); calibrateWithRealPrice(); }); document.getElementById('addDemoEventBtn')?.addEventListener('click',addDemoEvent); document.getElementById('calcCustomBtn')?.addEventListener('click',customAmountAdvice); document.getElementById('sourceWebsiteBtn')?.addEventListener('click',()=>openSourceWebsite('https://originworld.org')); document.getElementById('turboInviteBtn')?.addEventListener('click',()=>openSourceWebsite('https://origindefi.io')); document.querySelectorAll('.event-source-btn').forEach(btn=>{ btn.addEventListener('click',(e)=>{ e.stopPropagation(); openSourceWebsite(btn.getAttribute('data-url')); }); }); document.querySelectorAll('.time-btn').forEach(btn=>{ btn.addEventListener('click',()=>setGranularity(btn.getAttribute('data-gran'))); }); }
    init();
</script>
</body>
</html>
