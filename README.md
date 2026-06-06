<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>clipAI</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#0A0A0F;--panel:#090910;--card:rgba(255,255,255,0.025);
  --accent:#534AB7;--ah:#7F77DD;--al:#CECBF6;
  --txt:#fff;--muted:rgba(255,255,255,0.4);
  --border:rgba(255,255,255,0.07);--dash:rgba(127,119,221,0.35);
  --ff:'Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;
}
html,body{height:100%}
body{font-family:var(--ff);background:var(--bg);color:var(--txt);display:flex;flex-direction:column}

/* NAV */
nav{position:fixed;top:0;left:0;right:0;height:56px;display:flex;align-items:center;
  justify-content:space-between;padding:0 24px;border-bottom:1px solid var(--border);
  background:var(--bg);z-index:100}
.logo{font-size:18px;font-weight:600;letter-spacing:-.5px;user-select:none}
.logo .c{color:#fff}.logo .a{color:var(--al)}
.nav-btns{display:flex;gap:9px}
.bg{background:transparent;border:1px solid var(--border);color:rgba(255,255,255,.7);
  padding:7px 15px;border-radius:8px;font:500 13px/1 var(--ff);cursor:pointer;
  transition:border-color .2s,color .2s}
.bg:hover{border-color:rgba(255,255,255,.2);color:#fff}
.bs{background:var(--accent);border:none;color:#fff;padding:7px 15px;
  border-radius:8px;font:500 13px/1 var(--ff);cursor:pointer;transition:background .2s}
.bs:hover{background:var(--ah)}

/* LAYOUT */
.main{display:flex;margin-top:56px;height:calc(100vh - 56px)}
.lp{flex:2;min-width:0;padding:28px;border-right:1px solid var(--border);overflow-y:auto}
.rp{width:310px;flex-shrink:0;background:var(--panel);padding:22px;
  display:flex;flex-direction:column;gap:18px;overflow-y:auto}

/* LABELS */
.lbl{font-size:10px;font-weight:500;letter-spacing:.12em;text-transform:uppercase;
  color:var(--muted);margin-bottom:11px}

/* UPLOAD ZONE */
.uz{border:1.5px dashed var(--dash);border-radius:16px;padding:52px 24px;
  display:flex;flex-direction:column;align-items:center;gap:6px;cursor:pointer;
  transition:border-color .25s,background .25s;background:rgba(83,74,183,.015);
  margin-bottom:22px;text-align:center}
.uz:hover,.uz.dov{border-color:rgba(127,119,221,.8);background:rgba(83,74,183,.05)}
.uicon{width:52px;height:52px;border-radius:50%;background:rgba(83,74,183,.13);
  display:flex;align-items:center;justify-content:center;margin-bottom:8px}
.ut{font-size:15px;font-weight:500}
.us{font-size:13px;color:var(--muted)}
.uh{font-size:11px;color:rgba(255,255,255,.2);margin-top:5px;letter-spacing:.03em}

/* VIDEO PREVIEW */
.vp{display:none;position:relative;width:100%;aspect-ratio:16/9;border-radius:14px;
  overflow:hidden;margin-bottom:22px;background:rgba(255,255,255,.03);
  border:1px solid var(--border)}
.vp video{width:100%;height:100%;object-fit:cover;display:none}
.vph{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
  font-size:13px;color:rgba(255,255,255,.2)}
.vb{position:absolute;font-size:11px;font-weight:500;padding:4px 9px;border-radius:6px;line-height:1.3}
.vbf{top:10px;left:10px;background:rgba(83,74,183,.8);color:var(--al);backdrop-filter:blur(6px)}
.vbd{bottom:10px;right:10px;background:rgba(0,0,0,.65);color:rgba(255,255,255,.85);backdrop-filter:blur(6px)}

/* OPTION CARDS */
.oc{border:1px solid var(--border);border-radius:12px;background:var(--card);
  margin-bottom:10px;cursor:pointer;transition:border-color .2s}
.oc:hover:not(.on){border-color:rgba(255,255,255,.12)}
.oc.on{border-color:rgba(83,74,183,.6)}
.oh{display:flex;align-items:flex-start;gap:13px;padding:15px 16px}
.cb{width:18px;height:18px;border:1.5px solid rgba(255,255,255,.18);border-radius:5px;
  display:flex;align-items:center;justify-content:center;flex-shrink:0;margin-top:2px;
  transition:background .15s,border-color .15s}
.oc.on .cb{background:var(--accent);border-color:var(--accent)}
.cb svg{opacity:0;transition:opacity .1s}
.oc.on .cb svg{opacity:1}
.oi h3{font-size:14px;font-weight:500;margin-bottom:3px}
.oi p{font-size:12px;color:var(--muted);line-height:1.5}
.ob{display:none;padding:0 16px 16px;border-top:1px solid var(--border)}
.oc.on .ob{display:block}

/* AUDIO TOGGLE */
.atog{display:flex;background:rgba(255,255,255,.035);border-radius:8px;
  padding:3px;gap:3px;margin:13px 0 15px}
.atb{flex:1;padding:7px 8px;border:none;background:transparent;color:var(--muted);
  font:500 12px/1 var(--ff);cursor:pointer;border-radius:6px;transition:all .2s;text-align:center}
.atb.on{background:var(--accent);color:#fff}

/* CHIPS */
.clbl{font-size:11px;color:rgba(255,255,255,.3);margin-bottom:8px;letter-spacing:.03em}
.crow{display:flex;gap:7px;flex-wrap:wrap}
.chip{padding:6px 13px;border-radius:20px;border:1px solid rgba(255,255,255,.1);
  background:rgba(255,255,255,.03);color:rgba(255,255,255,.45);font-size:12px;
  cursor:pointer;transition:all .15s;font-family:var(--ff)}
.chip:hover:not(.dis){border-color:rgba(255,255,255,.2);color:rgba(255,255,255,.8)}
.chip.on{background:rgba(83,74,183,.2);border-color:var(--accent);color:var(--al)}
.chip.dis{opacity:.3;cursor:default}

/* VIDEO GRID */
.vgrid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin:13px 0 11px}
.vc{border:1px solid rgba(255,255,255,.07);border-radius:10px;overflow:hidden;
  cursor:pointer;transition:border-color .2s}
.vc:hover{border-color:rgba(255,255,255,.15)}
.vc.on{border-color:var(--accent)}
.vt{aspect-ratio:16/9;position:relative;display:flex;align-items:center;justify-content:center}
.vt-chill{background:linear-gradient(135deg,#0d1520 0%,#121e2e 50%,#0f1c28 100%)}
.vt-asmr{background:linear-gradient(135deg,#1a1020 0%,#201530 50%,#1a1028 100%)}
.pb{width:30px;height:30px;background:rgba(255,255,255,.08);border-radius:50%;
  display:flex;align-items:center;justify-content:center}
.tbb{position:absolute;font-size:10px;font-weight:600;padding:3px 6px;border-radius:4px}
.tbl{top:5px;left:5px;background:rgba(255,255,255,.1);color:rgba(255,255,255,.6)}
.tba{top:5px;left:5px;background:var(--accent);color:#fff}
.tbd{bottom:5px;right:5px;background:rgba(0,0,0,.6);color:rgba(255,255,255,.7)}
.tck{position:absolute;top:5px;right:5px;width:18px;height:18px;background:var(--accent);
  border-radius:50%;display:none;align-items:center;justify-content:center}
.vc.on .tck{display:flex}
.vci{padding:8px 10px 9px}
.vcn{font-size:12px;font-weight:500;margin-bottom:2px}
.vcd{font-size:11px;color:var(--muted)}

/* NOTES */
.note{border-radius:8px;padding:9px 11px;font-size:12px;line-height:1.55;
  display:flex;gap:9px;align-items:flex-start;margin-bottom:7px}
.ni{flex-shrink:0;margin-top:1px}
.np{background:rgba(83,74,183,.1);border:1px solid rgba(83,74,183,.25);color:var(--al)}
.nd{background:rgba(255,255,255,.03);border:1px solid var(--border);color:var(--muted)}

/* RIGHT PANEL */
.tr{background:var(--card);border:1px solid var(--border);border-radius:10px;
  padding:13px 14px;display:flex;align-items:center;justify-content:space-between;gap:12px}
.ti h4{font-size:14px;font-weight:500;margin-bottom:2px}
.ti p{font-size:12px;color:var(--muted)}
.tsw{width:40px;height:22px;background:var(--accent);border-radius:11px;
  position:relative;cursor:pointer;flex-shrink:0;transition:background .2s}
.tsw.off{background:rgba(255,255,255,.1)}
.tk{position:absolute;top:2px;width:18px;height:18px;background:#fff;border-radius:50%;
  transition:left .2s;left:20px}
.tsw.off .tk{left:2px}

.cpw{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:10px}
.cpr{background:#000;border-radius:8px;aspect-ratio:9/5;display:flex;
  align-items:flex-end;justify-content:center;padding-bottom:11px}
.cpt{background:rgba(0,0,0,.7);padding:5px 11px;border-radius:6px;
  font-size:12px;font-weight:500}
.cpt .hl{color:var(--al)}
.spills{display:flex;gap:6px;flex-wrap:wrap;margin-top:9px}
.sp{padding:5px 11px;border-radius:20px;border:1px solid rgba(255,255,255,.08);
  background:transparent;color:var(--muted);font:400 11px/1 var(--ff);cursor:pointer;transition:all .15s}
.sp:hover{border-color:rgba(255,255,255,.15);color:rgba(255,255,255,.7)}
.sp.on{background:rgba(83,74,183,.2);border-color:var(--accent);color:var(--al)}

.obox{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:0 14px}
.or{display:flex;justify-content:space-between;align-items:center;
  padding:11px 0;border-bottom:1px solid var(--border)}
.or:last-child{border-bottom:none}
.ol{font-size:13px;color:var(--muted)}
.ov{display:flex;align-items:center;gap:4px;font-size:13px;font-weight:500;
  cursor:pointer;color:rgba(255,255,255,.85)}

.spacer{flex:1;min-height:8px}

/* PROGRESS */
.prog{display:flex;align-items:center;justify-content:center;gap:4px;margin-bottom:10px}
.pd{width:6px;height:6px;border-radius:50%;background:rgba(255,255,255,.12);transition:all .3s}
.pd.done{background:var(--accent)}
.pd.act{background:rgba(255,255,255,.85);width:8px;height:8px}
.pl{height:1px;background:rgba(255,255,255,.07);flex:1;max-width:26px;transition:background .3s}
.pl.done{background:rgba(83,74,183,.45)}

/* CREATE BUTTON */
.bcr{width:100%;padding:13px;background:var(--accent);border:none;border-radius:10px;
  color:#fff;font:600 14px/1 var(--ff);cursor:pointer;display:flex;align-items:center;
  justify-content:center;gap:8px;transition:background .2s,transform .1s}
.bcr:hover{background:var(--ah)}
.bcr:active{transform:scale(.98)}

/* OVERLAY */
.ov-wrap{display:none;position:fixed;inset:0;background:rgba(10,10,15,.93);
  z-index:999;flex-direction:column;align-items:center;justify-content:center;gap:14px}
.ov-wrap.show{display:flex}
.spin{width:44px;height:44px;border:2.5px solid rgba(255,255,255,.07);
  border-top-color:var(--accent);border-radius:50%;animation:sp .7s linear infinite}
@keyframes sp{to{transform:rotate(360deg)}}
.ovt{font-size:18px;font-weight:500}
.ovs{font-size:13px;color:var(--muted)}
.ovact{display:flex;flex-direction:column;align-items:center;gap:8px;margin-top:4px}
.bdl{padding:12px 32px;background:var(--accent);border:none;border-radius:10px;
  color:#fff;font:600 14px/1 var(--ff);cursor:pointer;transition:background .2s}
.bdl:hover{background:var(--ah)}
.odis{font-size:12px;color:rgba(255,255,255,.25);cursor:pointer;background:none;
  border:none;font-family:var(--ff)}
.odis:hover{color:rgba(255,255,255,.5)}

::-webkit-scrollbar{width:5px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:rgba(255,255,255,.07);border-radius:3px}

@media(max-width:700px){
  .main{flex-direction:column;height:auto}
  .lp{border-right:none;border-bottom:1px solid var(--border)}
  .rp{width:100%}
  .prog{display:none}
}
</style>
</head>
<body>

<nav>
  <div class="logo"><span class="c">clip</span><span class="a">AI</span></div>
  <div class="nav-btns">
    <button class="bg">My videos</button>
    <button class="bs">Sign up</button>
  </div>
</nav>

<div class="main">
  <!-- LEFT PANEL -->
  <div class="lp">
    <p class="lbl">Upload</p>

    <div class="uz" id="uz" onclick="document.getElementById('fin').click()">
      <div class="uicon">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#CECBF6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="16 16 12 12 8 16"/><line x1="12" y1="12" x2="12" y2="21"/>
          <path d="M20.39 18.39A5 5 0 0018 9h-1.26A8 8 0 103 16.3"/>
        </svg>
      </div>
      <p class="ut">Drop your video here</p>
      <p class="us">or click to browse your files</p>
      <p class="uh">MP4 · MOV · AVI · up to 2GB</p>
    </div>

    <div class="vp" id="vp">
      <div class="vph" id="vph">Preview will appear here</div>
      <video id="vel" muted playsinline></video>
      <div class="vb vbf" id="vbf"></div>
      <div class="vb vbd" id="vbd"></div>
    </div>

    <p class="lbl">Editing options</p>

    <!-- Card 1: Audio only -->
    <div class="oc" id="c1">
      <div class="oh" onclick="tog('c1')">
        <div class="cb"><svg width="10" height="10" viewBox="0 0 10 10" fill="none"><polyline points="1.5,5 4,7.5 8.5,2.5" stroke="white" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="oi"><h3>Audio only</h3><p>Remove visuals, keep your audio — pick a new background</p></div>
      </div>
      <div class="ob" onclick="event.stopPropagation()">
        <p class="clbl" style="margin-top:13px">Audio source</p>
        <div class="atog">
          <button class="atb on" id="ab1" onclick="sAudio(1)">Keep original audio only</button>
          <button class="atb" id="ab2" onclick="sAudio(2)">Add another video on top</button>
        </div>
        <p class="clbl">Visual background</p>
        <div class="crow" id="bgc">
          <div class="chip on" onclick="sChip(this,'bgc')">Dark gradient</div>
          <div class="chip" onclick="sChip(this,'bgc')">Minimal white</div>
          <div class="chip" onclick="sChip(this,'bgc')">Soft blur</div>
          <div class="chip dis">+ coming soon</div>
        </div>
      </div>
    </div>

    <!-- Card 2: Bottom video -->
    <div class="oc" id="c2">
      <div class="oh" onclick="tog('c2')">
        <div class="cb"><svg width="10" height="10" viewBox="0 0 10 10" fill="none"><polyline points="1.5,5 4,7.5 8.5,2.5" stroke="white" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="oi"><h3>Bottom video</h3><p>Add a looping clip below your video for vertical content</p></div>
      </div>
      <div class="ob" onclick="event.stopPropagation()">
        <div class="vgrid">
          <div class="vc" id="vc1" onclick="sVid('vc1')">
            <div class="vt vt-chill">
              <div class="pb"><svg width="11" height="11" viewBox="0 0 11 11" fill="white"><polygon points="2,1 10,5.5 2,10"/></svg></div>
              <div class="tbb tbl">looping</div>
              <div class="tck"><svg width="9" height="9" viewBox="0 0 9 9" fill="none"><polyline points="1.5,4.5 3.5,6.5 7.5,2" stroke="white" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
            </div>
            <div class="vci"><div class="vcn">Chill vibes</div><div class="vcd">Looping visual · no audio</div></div>
          </div>
          <div class="vc" id="vc2" onclick="sVid('vc2')">
            <div class="vt vt-asmr">
              <div class="pb"><svg width="11" height="11" viewBox="0 0 11 11" fill="white"><polygon points="2,1 10,5.5 2,10"/></svg></div>
              <div class="tbb tba">ASMR cycle</div>
              <div class="tbb tbd">5s each</div>
              <div class="tck"><svg width="9" height="9" viewBox="0 0 9 9" fill="none"><polyline points="1.5,4.5 3.5,6.5 7.5,2" stroke="white" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
            </div>
            <div class="vci"><div class="vcn">ASMR mix</div><div class="vcd">Auto-cycles every 5s · no audio</div></div>
          </div>
        </div>
        <div class="note np" id="an" style="display:none">
          <svg class="ni" width="14" height="14" viewBox="0 0 16 16" fill="none"><circle cx="8" cy="8" r="6.5" stroke="#CECBF6" stroke-width="1.4"/><line x1="8" y1="5.5" x2="8" y2="8.5" stroke="#CECBF6" stroke-width="1.5" stroke-linecap="round"/><circle cx="8" cy="10.5" r=".75" fill="#CECBF6"/></svg>
          ASMR cycle on: each scene plays for 5 seconds then cuts to the next automatically, looping for the full length of your video.
        </div>
        <div class="note nd">
          <svg class="ni" width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M7 4L4 7H1.5v2H4l3 4V4z" stroke="rgba(255,255,255,.35)" stroke-width="1.3" fill="none" stroke-linejoin="round"/><line x1="13.5" y1="6" x2="10" y2="9.5" stroke="rgba(255,255,255,.35)" stroke-width="1.3" stroke-linecap="round"/><line x1="10" y1="6" x2="13.5" y2="9.5" stroke="rgba(255,255,255,.35)" stroke-width="1.3" stroke-linecap="round"/></svg>
          Bottom video audio is always muted — only your uploaded video's audio plays.
        </div>
      </div>
    </div>

    <div style="height:24px"></div>
  </div>

  <!-- RIGHT PANEL -->
  <div class="rp">
    <div>
      <p class="lbl">Captions</p>
      <div class="tr">
        <div class="ti"><h4>Auto captions</h4><p>AI transcribes your audio</p></div>
        <div class="tsw" id="ct" onclick="togCap()"><div class="tk"></div></div>
      </div>
    </div>

    <div id="css">
      <p class="lbl">Caption style</p>
      <div class="cpw">
        <div class="cpr">
          <div class="cpt">this is how your <span class="hl">captions</span> look</div>
        </div>
        <div class="spills" id="sp">
          <div class="sp on" onclick="sSp(this)">Default</div>
          <div class="sp" onclick="sSp(this)">Bold highlight</div>
          <div class="sp" onclick="sSp(this)">Minimal</div>
          <div class="sp" onclick="sSp(this)">Big &amp; center</div>
        </div>
      </div>
    </div>

    <div>
      <p class="lbl">Output settings</p>
      <div class="obox">
        <div class="or"><span class="ol">Format</span><span class="ov">MP4 <svg width="10" height="10" viewBox="0 0 10 10" fill="none"><polyline points="2,3.5 5,6.5 8,3.5" stroke="rgba(255,255,255,.35)" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/></svg></span></div>
        <div class="or"><span class="ol">Resolution</span><span class="ov">1080p <svg width="10" height="10" viewBox="0 0 10 10" fill="none"><polyline points="2,3.5 5,6.5 8,3.5" stroke="rgba(255,255,255,.35)" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/></svg></span></div>
        <div class="or"><span class="ol">Aspect ratio</span><span class="ov">9:16 <svg width="10" height="10" viewBox="0 0 10 10" fill="none"><polyline points="2,3.5 5,6.5 8,3.5" stroke="rgba(255,255,255,.35)" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/></svg></span></div>
        <div class="or"><span class="ol">Quality</span><span class="ov">High <svg width="10" height="10" viewBox="0 0 10 10" fill="none"><polyline points="2,3.5 5,6.5 8,3.5" stroke="rgba(255,255,255,.35)" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/></svg></span></div>
      </div>
    </div>

    <div class="spacer"></div>

    <div>
      <div class="prog" id="prog">
        <div class="pd act" id="pd1"></div>
        <div class="pl" id="pl1"></div>
        <div class="pd" id="pd2"></div>
        <div class="pl" id="pl2"></div>
        <div class="pd" id="pd3"></div>
        <div class="pl" id="pl3"></div>
        <div class="pd" id="pd4"></div>
      </div>
      <button class="bcr" onclick="startCreate()">
        <svg width="13" height="13" viewBox="0 0 13 13" fill="white"><polygon points="2.5,1.5 11.5,6.5 2.5,11.5"/></svg>
        Create video
      </button>
    </div>
  </div>
</div>

<!-- Processing overlay -->
<div class="ov-wrap" id="ovw">
  <div id="ovsp" class="spin"></div>
  <div id="ovck" style="font-size:36px;display:none">✅</div>
  <p class="ovt" id="ovt">Processing your video...</p>
  <p class="ovs" id="ovs">Extracting audio from your clip</p>
  <div class="ovact" id="ova" style="display:none">
    <button class="bdl" onclick="closeOv()">⬇&nbsp; Download video</button>
    <button class="odis" onclick="closeOv()">dismiss</button>
  </div>
</div>

<input type="file" id="fin" accept="video/*" style="display:none">

<script>
const $ = id => document.getElementById(id);

// FILE UPLOAD
$('fin').addEventListener('change', e => { if(e.target.files[0]) loadVid(e.target.files[0]) });
const uz = $('uz');
uz.addEventListener('dragover', e => { e.preventDefault(); uz.classList.add('dov') });
['dragleave','dragend'].forEach(ev => uz.addEventListener(ev, () => uz.classList.remove('dov')));
uz.addEventListener('drop', e => {
  e.preventDefault(); uz.classList.remove('dov');
  if(e.dataTransfer.files[0]) loadVid(e.dataTransfer.files[0]);
});

function loadVid(f) {
  const url = URL.createObjectURL(f);
  const vel = $('vel'), vp = $('vp');
  const fn = f.name;
  $('vbf').textContent = fn.length > 22 ? fn.slice(0,19)+'…' : fn;
  uz.style.display = 'none';
  vp.style.display = 'block';
  vel.src = url; vel.style.display = 'block';
  $('vph').style.display = 'none';
  vel.addEventListener('loadedmetadata', function h(){
    const s = Math.round(vel.duration), m = Math.floor(s/60), sec = s%60;
    $('vbd').textContent = `${m}:${sec.toString().padStart(2,'0')}`;
    vel.removeEventListener('loadedmetadata',h);
  });
  vel.currentTime = 0.1;
  setProg(2);
}

function tog(id) {
  $(id).classList.toggle('on');
  const a = document.querySelectorAll('.oc.on').length;
  setProg(a > 0 ? 3 : 2);
}

function sAudio(n) {
  $('ab1').classList.toggle('on', n===1);
  $('ab2').classList.toggle('on', n===2);
}

function sChip(el, grp) {
  if(el.classList.contains('dis')) return;
  el.closest('.crow').querySelectorAll('.chip').forEach(c => c.classList.remove('on'));
  el.classList.add('on');
}

function sVid(id) {
  document.querySelectorAll('.vc').forEach(c => c.classList.remove('on'));
  $(id).classList.add('on');
  $('an').style.display = id==='vc2' ? 'flex' : 'none';
}

let capOn = true;
function togCap() {
  capOn = !capOn;
  $('ct').classList.toggle('off', !capOn);
  const s = $('css');
  s.style.opacity = capOn ? '1' : '0.35';
  s.style.pointerEvents = capOn ? '' : 'none';
}

function sSp(el) {
  $('sp').querySelectorAll('.sp').forEach(p => p.classList.remove('on'));
  el.classList.add('on');
}

function setProg(step) {
  for(let i=1;i<=4;i++){
    const d=$(`pd${i}`); d.className='pd';
    if(i<step) d.classList.add('done');
    else if(i===step) d.classList.add('act');
  }
  for(let i=1;i<=3;i++){
    const l=$(`pl${i}`); l.className='pl';
    if(i<step) l.classList.add('done');
  }
}

const steps = [
  ['Processing your video...','Extracting audio from your clip'],
  ['Generating captions...','AI is transcribing your audio with Whisper'],
  ['Compositing layers...','Combining visuals, audio and settings'],
  ['Finalizing export...','Encoding your finished video at 1080p']
];

function startCreate() {
  const ow = $('ovw');
  ow.classList.add('show');
  $('ovsp').style.display='block';
  $('ovck').style.display='none';
  $('ova').style.display='none';
  let i=0; setStep(0); setProg(4);
  const iv = setInterval(() => {
    i++;
    if(i < steps.length) { setStep(i); }
    else {
      clearInterval(iv);
      $('ovsp').style.display='none';
      $('ovck').style.display='block';
      $('ovt').textContent='Your video is ready!';
      $('ovs').textContent='Download it below';
      $('ova').style.display='flex';
    }
  }, 1800);
}

function setStep(i) {
  $('ovt').textContent = steps[i][0];
  $('ovs').textContent = steps[i][1];
}

function closeOv() { $('ovw').classList.remove('show') }

setProg(1);
</script>
</body>
</html>
