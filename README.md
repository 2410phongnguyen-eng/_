<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Đấu Trường Sinh Học</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0B1410;
    --panel:#122019;
    --panel-2:#182B21;
    --line:rgba(61,220,151,0.22);
    --accent:#3DDC97;
    --accent-dim:#25916a;
    --amber:#F2B84B;
    --danger:#E4405A;
    --text:#EAF2EC;
    --text-dim:#93A99C;
    --radius:14px;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(circle at 15% 8%, rgba(61,220,151,0.07), transparent 40%),
      radial-gradient(circle at 85% 92%, rgba(242,184,75,0.05), transparent 45%),
      var(--bg);
    color:var(--text);
    font-family:'IBM Plex Sans', sans-serif;
    min-height:100vh;
    display:flex;
    justify-content:center;
    padding:20px 14px 40px;
  }
  body::before{
    content:"";
    position:fixed; inset:0; pointer-events:none; opacity:0.5;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='60' height='104' viewBox='0 0 60 104'%3E%3Cg fill='none' stroke='%233DDC97' stroke-opacity='0.05' stroke-width='1'%3E%3Cpath d='M30 0 L60 17.3 L60 52 L30 69.3 L0 52 L0 17.3 Z'/%3E%3Cpath d='M30 34.6 L60 51.9 L60 86.6 L30 103.9 L0 86.6 L0 51.9 Z'/%3E%3C/g%3E%3C/svg%3E");
    background-size:60px 104px;
  }
  .app{width:100%; max-width:460px; position:relative;}
  .eyebrow{
    font-family:'IBM Plex Mono', monospace;
    font-size:11px; letter-spacing:0.16em; text-transform:uppercase;
    color:var(--accent); margin:0 0 6px;
  }
  h1{
    font-family:'Space Grotesk', sans-serif;
    font-size:28px; font-weight:700; margin:0 0 4px; letter-spacing:-0.01em;
  }
  .sub{color:var(--text-dim); font-size:13.5px; margin:0 0 22px; line-height:1.5;}
  .card{
    background:linear-gradient(180deg, var(--panel-2), var(--panel));
    border:1px solid var(--line);
    border-radius:var(--radius);
    padding:18px;
    margin-bottom:16px;
    position:relative;
  }
  .card::before{
    content:"";
    position:absolute; top:0; left:18px; right:18px; height:1px;
    background:linear-gradient(90deg, transparent, var(--accent), transparent);
    opacity:0.5;
  }
  .field-label{
    font-family:'IBM Plex Mono', monospace;
    font-size:11px; letter-spacing:0.1em; text-transform:uppercase;
    color:var(--text-dim); margin-bottom:10px; display:block;
  }
  .seg{display:flex; gap:8px; flex-wrap:wrap; margin-bottom:18px;}
  .seg:last-child{margin-bottom:0;}
  .seg button{
    flex:1; min-width:90px;
    background:var(--panel); border:1px solid var(--line); color:var(--text-dim);
    font-family:'IBM Plex Sans', sans-serif; font-size:13.5px; font-weight:500;
    padding:10px 8px; border-radius:10px; cursor:pointer; transition:.15s;
  }
  .seg button.active{
    background:rgba(61,220,151,0.14); border-color:var(--accent); color:var(--accent);
  }
  .seg button:hover{border-color:var(--accent-dim);}
  .btn-main{
    width:100%; background:var(--accent); color:#06140D; border:none;
    font-family:'Space Grotesk', sans-serif; font-weight:700; font-size:16px;
    padding:15px; border-radius:12px; cursor:pointer; letter-spacing:0.01em;
    transition:.15s; box-shadow:0 6px 18px rgba(61,220,151,0.18);
  }
  .btn-main:hover{transform:translateY(-1px); box-shadow:0 8px 22px rgba(61,220,151,0.28);}
  .btn-ghost{
    width:100%; background:transparent; color:var(--text-dim); border:1px solid var(--line);
    font-family:'IBM Plex Sans'; font-size:14px; font-weight:500;
    padding:12px; border-radius:12px; cursor:pointer; margin-top:10px;
  }
  .btn-ghost:hover{color:var(--text); border-color:var(--accent-dim);}

  /* battle screen */
  .hidden{display:none !important;}
  .monster{
    display:flex; align-items:center; justify-content:space-between; margin-bottom:8px;
  }
  .monster-name{font-family:'Space Grotesk'; font-weight:600; font-size:15px;}
  .monster-name span{color:var(--text-dim); font-weight:400; font-size:12px; display:block; font-family:'IBM Plex Mono'; letter-spacing:0.08em; text-transform:uppercase; margin-top:2px;}
  .hp-value{font-family:'IBM Plex Mono'; font-size:13px; color:var(--text-dim);}
  .hp-track{
    height:14px; border-radius:7px; background:#0d1a14; border:1px solid var(--line);
    overflow:hidden; position:relative;
  }
  .hp-fill{
    height:100%; border-radius:7px; transition:width .35s ease;
    background-image:repeating-linear-gradient(115deg, rgba(255,255,255,0.16) 0 6px, transparent 6px 12px);
  }
  .hp-fill.boss{background-color:var(--danger);}
  .hp-fill.player{background-color:var(--accent);}
  .battle-row{margin-bottom:16px;}
  .timerbar{height:5px; border-radius:3px; background:#0d1a14; overflow:hidden; margin-bottom:14px;}
  .timerbar-fill{height:100%; background:var(--amber); width:100%; transition:width 0.1s linear;}
  .combo{
    font-family:'IBM Plex Mono'; font-size:12px; color:var(--amber);
    text-align:right; height:16px; margin-bottom:6px;
  }
  .qtopic{
    font-family:'IBM Plex Mono'; font-size:10.5px; letter-spacing:0.1em; text-transform:uppercase;
    color:var(--accent); margin-bottom:8px;
  }
  .qtext{font-size:16px; line-height:1.5; margin:0 0 16px; font-weight:500;}
  .choices{display:flex; flex-direction:column; gap:9px;}
  .choice{
    text-align:left; background:var(--panel); border:1px solid var(--line); color:var(--text);
    font-family:'IBM Plex Sans'; font-size:14.5px; padding:12px 14px; border-radius:10px;
    cursor:pointer; transition:.12s;
  }
  .choice:hover:not(:disabled){border-color:var(--accent-dim); background:rgba(61,220,151,0.06);}
  .choice.correct{background:rgba(61,220,151,0.18); border-color:var(--accent); color:var(--accent);}
  .choice.wrong{background:rgba(228,64,90,0.18); border-color:var(--danger); color:#ff8a9a;}
  .choice:disabled{cursor:default;}
  .float-dmg{
    position:absolute; font-family:'Space Grotesk'; font-weight:700; font-size:18px;
    pointer-events:none; animation:rise 0.9s ease forwards;
  }
  @keyframes rise{ from{opacity:1; transform:translateY(0);} to{opacity:0; transform:translateY(-34px);} }

  /* result */
  .result-title{font-family:'Space Grotesk'; font-size:24px; font-weight:700; margin:4px 0 4px;}
  .stat-row{display:flex; justify-content:space-between; padding:9px 0; border-bottom:1px solid var(--line); font-size:14px;}
  .stat-row:last-child{border-bottom:none;}
  .stat-row span:last-child{font-family:'IBM Plex Mono'; color:var(--accent);}
</style>
</head>
<body>
<div class="app">

  <!-- HOME -->
  <div id="screen-home">
    <p class="eyebrow">Ngân hàng đề · Sinh học THCS &amp; THPT</p>
    <h1>Đấu Trường Sinh Học</h1>
    <p class="sub">Mỗi trận là một bộ đề mới — lý thuyết rút từ ngân hàng câu hỏi, bài tập tính toán sinh số liệu ngẫu nhiên mỗi lần, không lặp lại y hệt.</p>

    <div class="card">
      <span class="field-label">Cấp học</span>
      <div class="seg" id="sel-level">
        <button data-v="THCS">THCS</button>
        <button data-v="THPT">THPT</button>
        <button data-v="ALL" class="active">Cả hai</button>
      </div>

      <span class="field-label">Dạng đề</span>
      <div class="seg" id="sel-mode">
        <button data-v="theory">Lý thuyết</button>
        <button data-v="calc">Tính toán</button>
        <button data-v="mixed" class="active">Hỗn hợp</button>
      </div>

      <span class="field-label">Độ khó</span>
      <div class="seg" id="sel-diff">
        <button data-v="Dễ">Dễ</button>
        <button data-v="Vừa" class="active">Vừa</button>
        <button data-v="Khó">Khó</button>
      </div>
    </div>

    <button class="btn-main" id="btn-start">Vào trận đấu</button>
    <p class="sub" style="text-align:center; margin-top:14px; font-size:12px;">14 dạng bài tính toán tự sinh số liệu · 40 câu lý thuyết theo chương trình hiện hành</p>
  </div>

  <!-- BATTLE -->
  <div id="screen-battle" class="hidden">
    <div class="card" style="position:relative; overflow:hidden;">
      <div class="battle-row">
        <div class="monster">
          <div class="monster-name" id="boss-name">Đối Thủ<span id="boss-topic">Chủ đề</span></div>
          <div class="hp-value" id="boss-hp-text">0/0</div>
        </div>
        <div class="hp-track"><div class="hp-fill boss" id="boss-hp-fill" style="width:100%;"></div></div>
      </div>
      <div class="battle-row" style="margin-bottom:0;">
        <div class="monster">
          <div class="monster-name">Bạn</div>
          <div class="hp-value" id="player-hp-text">0/0</div>
        </div>
        <div class="hp-track"><div class="hp-fill player" id="player-hp-fill" style="width:100%;"></div></div>
      </div>
    </div>

    <div class="card">
      <div class="combo" id="combo-label"></div>
      <div class="timerbar"><div class="timerbar-fill" id="timer-fill"></div></div>
      <p class="qtopic" id="q-topic">Chủ đề</p>
      <p class="qtext" id="q-text">Câu hỏi</p>
      <div class="choices" id="q-choices"></div>
    </div>
  </div>

  <!-- RESULT -->
  <div id="screen-result" class="hidden">
    <div class="card" style="text-align:center;">
      <p class="eyebrow" id="result-eyebrow">Kết thúc trận</p>
      <h2 class="result-title" id="result-title">—</h2>
    </div>
    <div class="card">
      <div class="stat-row"><span>Số câu đúng</span><span id="stat-correct">0/0</span></div>
      <div class="stat-row"><span>Combo cao nhất</span><span id="stat-combo">0</span></div>
      <div class="stat-row"><span>Cấp học</span><span id="stat-level">—</span></div>
      <div class="stat-row"><span>Độ khó</span><span id="stat-diff">—</span></div>
    </div>
    <button class="btn-main" id="btn-replay">Đấu lại · đề hoàn toàn mới</button>
    <button class="btn-ghost" id="btn-home">Về sảnh chờ</button>
  </div>

</div>

<script>
/* ============ HELPERS ============ */
function randInt(min,max){return Math.floor(Math.random()*(max-min+1))+min;}
function randEven(min,max){let v=randInt(min,max); return v%2===0?v:v+1;}
function choice(arr){return arr[randInt(0,arr.length-1)];}
function shuffle(arr){const a=[...arr]; for(let i=a.length-1;i>0;i--){const j=randInt(0,i);[a[i],a[j]]=[a[j],a[i]];} return a;}
function fmtInt(v){return Math.round(v).toLocaleString('vi-VN');}

function buildNumericChoices(correct, unit){
  correct = Math.round(correct);
  const set = new Set([correct]);
  const spread = Math.max(2, Math.round(correct*0.25));
  const generators = [
    ()=>Math.round(correct*0.5),
    ()=>Math.round(correct*2),
    ()=>Math.round(correct*1.25),
    ()=>Math.round(correct*0.75),
    ()=>Math.round(correct*1.5),
    ()=>correct+randInt(1,spread),
    ()=>Math.max(0, correct-randInt(1,spread)),
  ];
  let tries=0;
  while(set.size<4 && tries<60){
    const v = choice(generators)();
    if(v!==correct && v>=0) set.add(v);
    tries++;
  }
  while(set.size<4){ set.add(correct + set.size*7 + 3); }
  const arr = shuffle([...set]);
  return { choices: arr.map(v=>`${fmtInt(v)} ${unit}`.trim()), answerIndex: arr.indexOf(correct) };
}

/* ============ NGÂN HÀNG LÝ THUYẾT ============ */
const THEORY_BANK = [
// ---- THCS ----
{level:'THCS',topic:'Tế bào học',q:'Đơn vị cấu tạo cơ bản của cơ thể sống là gì?',choices:['Tế bào','Mô','Cơ quan','Hệ cơ quan'],correct:'Tế bào'},
{level:'THCS',topic:'Tế bào học',q:'Bào quan nào thực hiện quang hợp ở tế bào thực vật?',choices:['Ti thể','Lục lạp','Ribôxôm','Bộ máy Gôngi'],correct:'Lục lạp'},
{level:'THCS',topic:'Di truyền học',q:'Chất nào là vật chất di truyền chính ở phần lớn sinh vật?',choices:['ADN','Prôtêin','Lipit','Glucôzơ'],correct:'ADN'},
{level:'THCS',topic:'Di truyền học',q:'Ai là người đặt nền móng cho Di truyền học qua thí nghiệm trên đậu Hà Lan?',choices:['Menđen','Đacuyn','Paxtơ','Moocgan'],correct:'Menđen'},
{level:'THCS',topic:'Tế bào học',q:'Nguyên phân từ 1 tế bào mẹ tạo ra bao nhiêu tế bào con?',choices:['1','2','4','8'],correct:'2'},
{level:'THCS',topic:'Di truyền học',q:'Giảm phân từ 1 tế bào sinh dục chín (ở động vật) tạo ra tối đa bao nhiêu giao tử?',choices:['1','2','4','8'],correct:'4'},
{level:'THCS',topic:'Di truyền học',q:'Cấu trúc nào trong nhân tế bào mang gen quy định tính trạng?',choices:['Nhiễm sắc thể','Ribôxôm','Ti thể','Không bào'],correct:'Nhiễm sắc thể'},
{level:'THCS',topic:'Di truyền học',q:'Ở người, bộ nhiễm sắc thể lưỡng bội (2n) có bao nhiêu chiếc?',choices:['23','46','44','48'],correct:'46'},
{level:'THCS',topic:'Sinh lý thực vật',q:'Quá trình biến đổi CO2 và H2O thành chất hữu cơ nhờ ánh sáng gọi là gì?',choices:['Hô hấp','Quang hợp','Thoát hơi nước','Sinh sản'],correct:'Quang hợp'},
{level:'THCS',topic:'Sinh lý thực vật',q:'Hô hấp tế bào chủ yếu diễn ra ở bào quan nào?',choices:['Lục lạp','Ti thể','Nhân','Không bào'],correct:'Ti thể'},
{level:'THCS',topic:'Di truyền học',q:'Đột biến gen là gì?',choices:['Sự thay đổi số lượng nhiễm sắc thể','Biến đổi trong cấu trúc của gen','Sự trao đổi chất giữa tế bào','Sự phân chia tế bào bất thường'],correct:'Biến đổi trong cấu trúc của gen'},
{level:'THCS',topic:'Sinh thái học',q:'Quần thể sinh vật là gì?',choices:['Tập hợp cá thể cùng loài sống trong một khu vực nhất định','Tập hợp nhiều loài sinh vật','Một cá thể sinh vật','Môi trường sống của sinh vật'],correct:'Tập hợp cá thể cùng loài sống trong một khu vực nhất định'},
{level:'THCS',topic:'Sinh thái học',q:'Chuỗi thức ăn thể hiện mối quan hệ gì giữa các loài?',choices:['Quan hệ dinh dưỡng','Quan hệ cạnh tranh nơi ở','Quan hệ sinh sản','Quan hệ hỗ trợ'],correct:'Quan hệ dinh dưỡng'},
{level:'THCS',topic:'Sinh thái học',q:'Nhóm sinh vật nào có khả năng tự tổng hợp chất hữu cơ từ chất vô cơ?',choices:['Sinh vật tiêu thụ','Sinh vật phân giải','Sinh vật sản xuất','Sinh vật kí sinh'],correct:'Sinh vật sản xuất'},
{level:'THCS',topic:'Sinh lý người',q:'Hệ tuần hoàn ở người có chức năng chính là gì?',choices:['Vận chuyển máu và chất dinh dưỡng','Tiêu hóa thức ăn','Bài tiết chất thải','Điều hòa thân nhiệt'],correct:'Vận chuyển máu và chất dinh dưỡng'},
{level:'THCS',topic:'Sinh lý người',q:'Cơ quan nào ở người thực hiện trao đổi khí với môi trường?',choices:['Tim','Phổi','Gan','Thận'],correct:'Phổi'},
{level:'THCS',topic:'Tế bào học',q:'Enzim là chất gì trong cơ thể sinh vật?',choices:['Chất xúc tác sinh học có bản chất prôtêin','Chất dự trữ năng lượng','Chất vận chuyển oxi','Hoocmôn điều hòa sinh trưởng'],correct:'Chất xúc tác sinh học có bản chất prôtêin'},
{level:'THCS',topic:'Sinh sản',q:'Sinh sản vô tính khác sinh sản hữu tính chủ yếu ở điểm nào?',choices:['Không có sự kết hợp giao tử đực và cái','Luôn tạo ra cá thể khác loài','Không cần tế bào mẹ','Chỉ xảy ra ở thực vật'],correct:'Không có sự kết hợp giao tử đực và cái'},
{level:'THCS',topic:'Sinh sản',q:'Ở thực vật có hoa, thụ tinh kép tạo ra những gì?',choices:['Hai hợp tử','Hợp tử và nội nhũ','Hai nội nhũ','Hạt phấn và noãn'],correct:'Hợp tử và nội nhũ'},
{level:'THCS',topic:'Sinh thái học',q:'Hệ sinh thái bao gồm những thành phần nào?',choices:['Chỉ gồm các loài động vật','Quần xã sinh vật và môi trường sống','Chỉ gồm nhân tố vô sinh','Chỉ gồm sinh vật sản xuất'],correct:'Quần xã sinh vật và môi trường sống'},
// ---- THPT ----
{level:'THPT',topic:'Sinh học tế bào',q:'Thành phần nào tạo nên tính khảm động của màng sinh chất?',choices:['Xenlulôzơ và tinh bột','Photpholipit và prôtêin','ADN và ARN','Kitin và peptiđôglican'],correct:'Photpholipit và prôtêin'},
{level:'THPT',topic:'Sinh học tế bào',q:'Vận chuyển chất qua màng không tiêu tốn ATP gọi là gì?',choices:['Vận chuyển chủ động','Vận chuyển thụ động','Ẩm bào','Thực bào'],correct:'Vận chuyển thụ động'},
{level:'THPT',topic:'Sinh học tế bào',q:'Chu kỳ tế bào gồm những giai đoạn chính nào?',choices:['Chỉ có nguyên phân','Kỳ trung gian và quá trình phân bào','Chỉ có kỳ trung gian','Giảm phân I và giảm phân II'],correct:'Kỳ trung gian và quá trình phân bào'},
{level:'THPT',topic:'Di truyền học',q:'Hiện tượng trao đổi chéo trong giảm phân xảy ra ở kỳ nào?',choices:['Kỳ đầu I','Kỳ giữa II','Kỳ sau I','Kỳ cuối II'],correct:'Kỳ đầu I'},
{level:'THPT',topic:'Di truyền học',q:'Quy luật phân li độc lập của Menđen phản ánh điều gì?',choices:['Các cặp alen luôn di truyền cùng nhau','Các cặp alen quy định các cặp tính trạng phân li độc lập trong giảm phân','Chỉ có một cặp alen quy định một tính trạng','Gen luôn nằm trên nhiễm sắc thể giới tính'],correct:'Các cặp alen quy định các cặp tính trạng phân li độc lập trong giảm phân'},
{level:'THPT',topic:'Di truyền học',q:'Hiện tượng các gen trên cùng một nhiễm sắc thể di truyền cùng nhau gọi là gì?',choices:['Liên kết gen','Hoán vị gen','Tương tác gen','Di truyền liên kết giới tính'],correct:'Liên kết gen'},
{level:'THPT',topic:'Di truyền học',q:'Hoán vị gen xảy ra do hiện tượng nào?',choices:['Đột biến điểm','Trao đổi chéo giữa các crômatit khác nguồn gốc','Đột biến lệch bội','Đột biến đa bội'],correct:'Trao đổi chéo giữa các crômatit khác nguồn gốc'},
{level:'THPT',topic:'Di truyền học',q:'Đột biến cấu trúc nhiễm sắc thể dạng nào làm tăng số lượng gen trên nhiễm sắc thể?',choices:['Mất đoạn','Lặp đoạn','Đảo đoạn','Chuyển đoạn tương hỗ'],correct:'Lặp đoạn'},
{level:'THPT',topic:'Di truyền học',q:'Thể đột biến có bộ nhiễm sắc thể dạng 2n+1 được gọi là gì?',choices:['Thể một','Thể ba','Thể tam bội','Thể tứ bội'],correct:'Thể ba'},
{level:'THPT',topic:'Tiến hóa',q:'Nhân tố nào làm thay đổi tần số alen nhanh nhất trong quần thể có kích thước nhỏ?',choices:['Chọn lọc tự nhiên','Các yếu tố ngẫu nhiên (phiêu bạt di truyền)','Giao phối không ngẫu nhiên','Đột biến gen'],correct:'Các yếu tố ngẫu nhiên (phiêu bạt di truyền)'},
{level:'THPT',topic:'Tiến hóa',q:'Chọn lọc tự nhiên tác động trực tiếp lên cấp độ nào?',choices:['Kiểu gen','Kiểu hình','Alen riêng lẻ','Nhiễm sắc thể'],correct:'Kiểu hình'},
{level:'THPT',topic:'Tiến hóa',q:'Hình thành loài mới bằng con đường địa lí xảy ra khi nào?',choices:['Khi có cách li địa lí dẫn đến cách li sinh sản','Khi hai loài giao phối tự do với nhau','Khi không có đột biến xảy ra','Khi quần thể không thay đổi môi trường sống'],correct:'Khi có cách li địa lí dẫn đến cách li sinh sản'},
{level:'THPT',topic:'Sinh học tế bào',q:'Hô hấp hiếu khí gồm các giai đoạn chính nào?',choices:['Đường phân, chu trình Crep, chuỗi truyền êlectron','Chỉ có đường phân','Pha sáng và pha tối','Chỉ có chuỗi truyền êlectron'],correct:'Đường phân, chu trình Crep, chuỗi truyền êlectron'},
{level:'THPT',topic:'Sinh học tế bào',q:'Pha sáng của quang hợp diễn ra ở đâu?',choices:['Chất nền lục lạp (strôma)','Màng tilacôit của lục lạp','Ti thể','Nhân tế bào'],correct:'Màng tilacôit của lục lạp'},
{level:'THPT',topic:'Sinh học tế bào',q:'Pha tối (chu trình Canvin) của quang hợp diễn ra ở đâu?',choices:['Màng tilacôit','Chất nền lục lạp (strôma)','Màng ngoài ti thể','Ribôxôm'],correct:'Chất nền lục lạp (strôma)'},
{level:'THPT',topic:'Sinh thái học',q:'Mối quan hệ nào sau đây là quan hệ đối kháng trong quần xã?',choices:['Cộng sinh','Hội sinh','Cạnh tranh, kí sinh','Hợp tác'],correct:'Cạnh tranh, kí sinh'},
{level:'THPT',topic:'Sinh thái học',q:'Diễn thế sinh thái là gì?',choices:['Sự tăng số lượng cá thể trong quần thể','Quá trình biến đổi tuần tự của quần xã qua các giai đoạn','Sự di cư của một loài','Sự đột biến gen trong quần thể'],correct:'Quá trình biến đổi tuần tự của quần xã qua các giai đoạn'},
{level:'THPT',topic:'Sinh thái học',q:'Chu trình sinh địa hóa là gì?',choices:['Chu trình trao đổi vật chất vô cơ giữa các thành phần của hệ sinh thái','Chu trình chỉ xảy ra trong cơ thể sinh vật','Quá trình chọn lọc tự nhiên','Quá trình hình thành loài mới'],correct:'Chu trình trao đổi vật chất vô cơ giữa các thành phần của hệ sinh thái'},
{level:'THPT',topic:'Sinh thái học',q:'Chỉ số nào phản ánh khả năng sinh sản của một quần thể?',choices:['Mức sinh sản','Mức tử vong','Kích thước tối thiểu','Tỉ lệ giới tính'],correct:'Mức sinh sản'},
{level:'THPT',topic:'Vi sinh vật',q:'Nhóm vi sinh vật nào có khả năng cố định nitơ tự do trong không khí?',choices:['Vi khuẩn lam và vi khuẩn cố định đạm (như Rhizobium)','Nấm men','Vi khuẩn lactic','Tảo lục'],correct:'Vi khuẩn lam và vi khuẩn cố định đạm (như Rhizobium)'},
];

/* ============ BÀI TẬP TÍNH TOÁN (mỗi lần gọi sinh số liệu mới) ============ */
const CALC_TEMPLATES = [
// 1. Chiều dài gen
{level:'ALL', topic:'ADN – Chiều dài gen', gen:()=>{
  const N = randEven(1000,3000)*2;
  const correct = (N/2)*3.4;
  const b = buildNumericChoices(correct,'Å');
  return {stem:`Một gen có tổng số N = ${fmtInt(N)} nuclêôtit. Chiều dài của gen là bao nhiêu Å?`, ...b};
}},
// 2. Liên kết hidro
{level:'ALL', topic:'ADN – Liên kết hiđrô', gen:()=>{
  const N = randEven(2000,5000);
  const pctA = choice([15,20,25,30,35]);
  const A = Math.round(N*(pctA/100));
  const G = N/2 - A;
  const H = 2*A + 3*G;
  const b = buildNumericChoices(H,'liên kết');
  return {stem:`Một gen có N = ${fmtInt(N)} nuclêôtit, trong đó A chiếm ${pctA}% tổng số nuclêôtit. Số liên kết hiđrô của gen là bao nhiêu?`, ...b};
}},
// 3. Số nu loại G
{level:'ALL', topic:'ADN – Thành phần nuclêôtit', gen:()=>{
  const N = randEven(2000,5000);
  const pctA = choice([15,20,25,30,35]);
  const A = Math.round(N*(pctA/100));
  const G = N/2 - A;
  const b = buildNumericChoices(G,'nuclêôtit');
  return {stem:`Một gen có N = ${fmtInt(N)} nuclêôtit, trong đó A chiếm ${pctA}% tổng số nuclêôtit. Số nuclêôtit loại G của gen là bao nhiêu?`, ...b};
}},
// 4. Số ADN con sau k lần nhân đôi
{level:'ALL', topic:'ADN – Nhân đôi', gen:()=>{
  const k = randInt(1,5);
  const correct = Math.pow(2,k);
  const b = buildNumericChoices(correct,'phân tử');
  return {stem:`Một phân tử ADN nhân đôi liên tiếp ${k} lần. Số phân tử ADN con được tạo ra là bao nhiêu?`, ...b};
}},
// 5. Số nu môi trường cung cấp
{level:'ALL', topic:'ADN – Nhân đôi', gen:()=>{
  const N = randEven(1000,3000);
  const k = randInt(1,4);
  const correct = N*(Math.pow(2,k)-1);
  const b = buildNumericChoices(correct,'nuclêôtit');
  return {stem:`Một gen có N = ${fmtInt(N)} nuclêôtit nhân đôi liên tiếp ${k} lần. Số nuclêôtit môi trường nội bào cung cấp là bao nhiêu?`, ...b};
}},
// 6. Số cromatit kỳ giữa nguyên phân
{level:'ALL', topic:'Nguyên phân', gen:()=>{
  const n2 = choice([8,14,20,24,38,46]);
  const correct = 2*n2;
  const b = buildNumericChoices(correct,'crômatit');
  return {stem:`Một loài có bộ nhiễm sắc thể lưỡng bội 2n = ${n2}. Ở kỳ giữa của nguyên phân, số crômatit có trong tế bào là bao nhiêu?`, ...b};
}},
// 7. Số loại giao tử tối đa
{level:'THPT', topic:'Giảm phân', gen:()=>{
  const n = randInt(2,10);
  const correct = Math.pow(2,n);
  const b = buildNumericChoices(correct,'loại');
  return {stem:`Một cơ thể có ${n} cặp gen dị hợp, các cặp phân li độc lập trong giảm phân. Số loại giao tử tối đa được tạo ra là bao nhiêu?`, ...b};
}},
// 8. Số kiểu gen ở đời con
{level:'THPT', topic:'Quy luật di truyền', gen:()=>{
  const n = randInt(1,4);
  const correct = Math.pow(3,n);
  const b = buildNumericChoices(correct,'kiểu gen');
  return {stem:`Cho ${n} cặp gen dị hợp, phân li độc lập, các cá thể dị hợp giao phối với nhau. Số kiểu gen tối đa ở đời con là bao nhiêu?`, ...b};
}},
// 9. Hardy-Weinberg: tần số alen từ q^2
{level:'THPT', topic:'Di truyền học quần thể', gen:()=>{
  const q2 = choice([0.01,0.04,0.09,0.16,0.25,0.36]);
  const q = Math.sqrt(q2);
  const correct = Math.round(q*100);
  const b = buildNumericChoices(correct,'%');
  return {stem:`Một quần thể cân bằng di truyền có kiểu gen đồng hợp lặn (aa) chiếm tỉ lệ ${(q2*100).toFixed(0)}%. Tần số alen a trong quần thể là bao nhiêu %?`, ...b};
}},
// 10. Hardy-Weinberg: tỉ lệ dị hợp
{level:'THPT', topic:'Di truyền học quần thể', gen:()=>{
  const p = choice([0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9]);
  const q = +(1-p).toFixed(1);
  const correct = Math.round(2*p*q*100);
  const b = buildNumericChoices(correct,'%');
  return {stem:`Quần thể cân bằng di truyền có tần số alen A = ${p}, a = ${q}. Tỉ lệ kiểu gen dị hợp Aa trong quần thể là bao nhiêu %?`, ...b};
}},
// 11. Hô hấp - ATP
{level:'THPT', topic:'Hô hấp tế bào', gen:()=>{
  const g = randInt(2,12);
  const correct = g*36;
  const b = buildNumericChoices(correct,'ATP');
  return {stem:`Có ${g} phân tử glucôzơ được phân giải hoàn toàn qua hô hấp hiếu khí. Số ATP tối đa được tạo ra là bao nhiêu (lấy hiệu suất 36 ATP/glucôzơ)?`, ...b};
}},
// 12. Quang hợp - CO2
{level:'THPT', topic:'Quang hợp', gen:()=>{
  const g = randInt(1,10);
  const correct = g*6;
  const b = buildNumericChoices(correct,'phân tử CO2');
  return {stem:`Quá trình quang hợp tạo ra ${g} phân tử glucôzơ (C6H12O6). Số phân tử CO2 đã được sử dụng là bao nhiêu?`, ...b};
}},
// 13. Tăng trưởng quần thể
{level:'THPT', topic:'Sinh thái học', gen:()=>{
  const N0 = randInt(50,300)*10;
  const b_rate = randInt(10,30)/100;
  const d_rate = randInt(2,15)/100;
  const correct = N0*(1+b_rate-d_rate);
  const bch = buildNumericChoices(correct,'cá thể');
  return {stem:`Một quần thể có kích thước ban đầu N0 = ${fmtInt(N0)} cá thể, tỉ lệ sinh b = ${(b_rate*100).toFixed(0)}%, tỉ lệ tử d = ${(d_rate*100).toFixed(0)}% mỗi năm. Kích thước quần thể sau 1 năm là bao nhiêu cá thể?`, ...bch};
}},
// 14. Menđen phân li F2
{level:'ALL', topic:'Di truyền Menđen', gen:()=>{
  const total = choice([400,800,1200,1600,2000]);
  const correct = total/4;
  const b = buildNumericChoices(correct,'cá thể');
  return {stem:`Ở phép lai Aa × Aa (trội hoàn toàn), thu được ${fmtInt(total)} cá thể ở đời con. Số cá thể có kiểu hình lặn là bao nhiêu?`, ...b};
}},
];

/* ============ GAME STATE ============ */
const DIFF = {
  'Dễ':  {bossHP:60,  dmgCorrect:16, dmgWrong:10, time:26},
  'Vừa': {bossHP:100, dmgCorrect:18, dmgWrong:15, time:20},
  'Khó': {bossHP:140, dmgCorrect:22, dmgWrong:20, time:15},
};
let settings = {level:'ALL', mode:'mixed', diff:'Vừa'};
let state = {};
let timerHandle = null;

function initSegGroup(id, key){
  const group = document.getElementById(id);
  group.querySelectorAll('button').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      group.querySelectorAll('button').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      settings[key] = btn.dataset.v;
    });
  });
}
initSegGroup('sel-level','level');
initSegGroup('sel-mode','mode');
initSegGroup('sel-diff','diff');

function showScreen(id){
  ['screen-home','screen-battle','screen-result'].forEach(s=>{
    document.getElementById(s).classList.toggle('hidden', s!==id);
  });
}

function pickTheory(levelFilter){
  const pool = levelFilter==='ALL' ? THEORY_BANK : THEORY_BANK.filter(q=>q.level===levelFilter);
  const item = choice(pool);
  const opts = shuffle(item.choices);
  return {
    topic:item.topic,
    stem:item.q,
    choices:opts,
    answerIndex:opts.indexOf(item.correct)
  };
}
function pickCalc(levelFilter){
  const pool = levelFilter==='ALL' ? CALC_TEMPLATES : CALC_TEMPLATES.filter(t=>t.level==='ALL'||t.level===levelFilter);
  const tpl = choice(pool);
  const g = tpl.gen();
  return { topic: tpl.topic, stem: g.stem, choices: g.choices, answerIndex: g.answerIndex };
}
function nextQuestion(){
  let mode = settings.mode;
  if(mode==='mixed') mode = Math.random()<0.5 ? 'theory' : 'calc';
  return mode==='theory' ? pickTheory(settings.level) : pickCalc(settings.level);
}

function startBattle(){
  const d = DIFF[settings.diff];
  state = {
    bossHP:d.bossHP, bossMax:d.bossHP,
    playerHP:100, playerMax:100,
    streak:0, bestStreak:0,
    correctCount:0, totalCount:0,
    diff:d
  };
  showScreen('screen-battle');
  renderHP();
  loadQuestion();
}

function renderHP(){
  document.getElementById('boss-hp-fill').style.width = Math.max(0,(state.bossHP/state.bossMax*100))+'%';
  document.getElementById('player-hp-fill').style.width = Math.max(0,(state.playerHP/state.playerMax*100))+'%';
  document.getElementById('boss-hp-text').textContent = `${Math.max(0,Math.round(state.bossHP))}/${state.bossMax}`;
  document.getElementById('player-hp-text').textContent = `${Math.max(0,Math.round(state.playerHP))}/${state.playerMax}`;
}

function loadQuestion(){
  const q = nextQuestion();
  state.current = q;
  document.getElementById('boss-topic').textContent = q.topic;
  document.getElementById('q-topic').textContent = q.topic;
  document.getElementById('q-text').textContent = q.stem;
  document.getElementById('combo-label').textContent = state.streak>=2 ? `COMBO x${state.streak}` : '';

  const wrap = document.getElementById('q-choices');
  wrap.innerHTML = '';
  q.choices.forEach((c,i)=>{
    const btn = document.createElement('button');
    btn.className = 'choice';
    btn.textContent = c;
    btn.addEventListener('click', ()=>answer(i, btn));
    wrap.appendChild(btn);
  });

  startTimer(state.diff.time);
}

function startTimer(seconds){
  clearInterval(timerHandle);
  let t = seconds;
  const fill = document.getElementById('timer-fill');
  fill.style.transition = 'none';
  fill.style.width = '100%';
  requestAnimationFrame(()=>{ fill.style.transition = `width ${seconds}s linear`; fill.style.width='0%'; });
  timerHandle = setTimeout(()=>{ timeUp(); }, seconds*1000);
}
function timeUp(){
  const wrap = document.getElementById('q-choices');
  wrap.querySelectorAll('button').forEach(b=>b.disabled=true);
  applyResult(false, null);
}

function answer(i, btn){
  clearInterval(timerHandle);
  clearTimeout(timerHandle);
  const wrap = document.getElementById('q-choices');
  wrap.querySelectorAll('button').forEach(b=>b.disabled=true);
  const correct = i === state.current.answerIndex;
  btn.classList.add(correct?'correct':'wrong');
  if(!correct){
    wrap.children[state.current.answerIndex].classList.add('correct');
  }
  applyResult(correct, btn);
}

function floatDamage(text, color, anchorEl){
  const el = document.createElement('div');
  el.className = 'float-dmg';
  el.textContent = text;
  el.style.color = color;
  el.style.left = '50%';
  el.style.top = '10px';
  anchorEl.parentElement.style.position = 'relative';
  anchorEl.parentElement.appendChild(el);
  setTimeout(()=>el.remove(), 900);
}

function applyResult(correct, btn){
  state.totalCount++;
  const combo = document.getElementById('combo-label');
  if(correct){
    state.correctCount++;
    state.streak++;
    state.bestStreak = Math.max(state.bestStreak, state.streak);
    const bonus = state.streak>=2 ? Math.min(state.streak-1,5)*2 : 0;
    const dmg = state.diff.dmgCorrect + bonus;
    state.bossHP -= dmg;
    if(btn) floatDamage(`-${dmg}`, '#3DDC97', btn);
  } else {
    state.streak = 0;
    const dmg = state.diff.dmgWrong;
    state.playerHP -= dmg;
    if(btn) floatDamage(`-${dmg}`, '#E4405A', btn);
  }
  renderHP();
  combo.textContent = state.streak>=2 ? `COMBO x${state.streak}` : '';

  setTimeout(()=>{
    if(state.bossHP<=0) return endBattle(true);
    if(state.playerHP<=0) return endBattle(false);
    loadQuestion();
  }, 850);
}

function endBattle(win){
  clearInterval(timerHandle);
  clearTimeout(timerHandle);
  showScreen('screen-result');
  document.getElementById('result-eyebrow').textContent = win ? 'Chiến thắng' : 'Trận đấu kết thúc';
  document.getElementById('result-title').textContent = win ? 'Bạn đã hạ gục quái vật đề thi!' : 'Bạn đã gục ngã — luyện thêm rồi quay lại!';
  document.getElementById('stat-correct').textContent = `${state.correctCount}/${state.totalCount}`;
  document.getElementById('stat-combo').textContent = state.bestStreak;
  document.getElementById('stat-level').textContent = settings.level==='ALL' ? 'THCS & THPT' : settings.level;
  document.getElementById('stat-diff').textContent = settings.diff;
}

document.getElementById('btn-start').addEventListener('click', startBattle);
document.getElementById('btn-replay').addEventListener('click', startBattle);
document.getElementById('btn-home').addEventListener('click', ()=>showScreen('screen-home'));
</script>
</body>
</html>
