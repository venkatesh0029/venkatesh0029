<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Venkatesh — Animated Profile</title>
  <meta name="description" content="Animated profile page / GitHub profile companion for venkatesh0029" />
  <style>
    /* Reset */
    :root{
      --bg:#071028; --card:#071833; --accent:#00f7ff; --accent2:#8b5cf6;
      --glass: rgba(255,255,255,0.04);
      --glass-strong: rgba(255,255,255,0.06);
      --text:#e6eef8;
    }
    *{box-sizing:border-box}
    html,body,#app{height:100%;}
    body{
      margin:0;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,"Helvetica Neue",Arial;
      background: radial-gradient(1200px 600px at 10% 10%, rgba(11,22,40,0.6), transparent), radial-gradient(800px 400px at 90% 90%, rgba(50,10,70,0.15), transparent), var(--bg);
      color:var(--text); -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
      overflow:hidden;
    }

    /* Particle canvas covers background */
    #particles{position:fixed;inset:0;z-index:0}

    /* Container */
    .wrap{position:relative;z-index:5;max-width:1100px;margin:48px auto;padding:32px;display:grid;grid-template-columns:360px 1fr;gap:28px;align-items:start}

    /* 3D Profile card */
    .card{min-height:420px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:20px;padding:22px;backdrop-filter: blur(8px);border:1px solid var(--glass-strong);box-shadow:0 10px 30px rgba(2,6,23,0.6);position:relative;overflow:visible;transform-style:preserve-3d;transition:transform .12s ease}

    .profile-3d{perspective:1200px}
    .inner-card{border-radius:16px;padding:18px;background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);height:100%;display:flex;flex-direction:column;align-items:center;justify-content:flex-start;gap:14px;transform-style:preserve-3d}

    .avatar{
      width:160px;height:160px;border-radius:22px;display:block;overflow:hidden;border:3px solid rgba(255,255,255,0.06);box-shadow:0 8px 30px rgba(11,20,50,0.5);
      transform:translateZ(40px);
      background: linear-gradient(135deg,var(--accent),var(--accent2));
      display:flex;align-items:center;justify-content:center;color:#021124;font-weight:700;font-size:28px
    }

    .name{font-size:22px;font-weight:700;margin-top:6px}
    .role{font-size:13px;color:#bfcfe0}
    .bio{font-size:13px;text-align:center;color:#cfe8ff;margin-top:6px}

    .skills{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin-top:8px}
    .chip{padding:8px 12px;border-radius:999px;font-size:12px;background:linear-gradient(90deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.03)}

    /* glow border accent */
    .glow-border{position:absolute;inset:0;border-radius:20px;pointer-events:none;mix-blend-mode:screen}
    .glow-border:before{content:'';position:absolute;inset:-1px;border-radius:21px;background:linear-gradient(90deg, rgba(0,247,255,0.12), rgba(139,92,246,0.12));filter:blur(18px);opacity:0.9}

    /* Buttons */
    .cta{display:flex;gap:10px;margin-top:12px}
    .btn{padding:10px 14px;border-radius:12px;background:linear-gradient(90deg,var(--accent),var(--accent2));color:#021124;font-weight:700;border:none;cursor:pointer;box-shadow:0 8px 30px rgba(139,92,246,0.12);transition:transform .12s ease}
    .btn.secondary{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--text)}
    .btn:active{transform:translateY(1px) scale(.995)}

    /* Right column */
    .main-panel{min-height:420px;padding:20px;border-radius:18px;background:linear-gradient(180deg, rgba(255,255,255,0.015), transparent);border:1px solid var(--glass);box-shadow:0 12px 40px rgba(2,6,23,0.5);position:relative}

    /* Animated banner (SVG) */
    .banner{height:140px;border-radius:12px;overflow:hidden;margin-bottom:12px}
    .banner svg{width:100%;height:100%;display:block}

    /* Project grid */
    .projects{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px}
    .project{height:140px;border-radius:12px;position:relative;overflow:hidden;border:1px solid rgba(255,255,255,0.03);background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.00));padding:12px;display:flex;flex-direction:column;justify-content:space-between}

    .thumb{height:72px;border-radius:10px;overflow:hidden;display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.02);}
    .thumb img{width:100%;height:100%;object-fit:cover;transform-origin:center;transition:transform .6s cubic-bezier(.2,.9,.2,1)}
    .project:hover .thumb img{transform:scale(1.08) rotate(-1deg)}

    .proj-meta{display:flex;align-items:center;justify-content:space-between}
    .proj-title{font-weight:700}
    .proj-tags{display:flex;gap:6px}
    .tag{font-size:11px;padding:6px 8px;border-radius:8px;background:rgba(255,255,255,0.02)}

    /* animated badges */
    .badge{display:inline-block;padding:6px 10px;border-radius:999px;background:linear-gradient(90deg,var(--accent),var(--accent2));color:#021124;font-weight:700;box-shadow:0 6px 20px rgba(11,24,40,0.5);transform:translateY(0);animation:float 3s ease-in-out infinite}
    @keyframes float{0%{transform:translateY(0)}50%{transform:translateY(-6px)}100%{transform:translateY(0)}}

    /* footer connectors */
    .connect{display:flex;gap:12px;align-items:center;flex-wrap:wrap;margin-top:14px}
    .icon-btn{width:44px;height:44px;border-radius:10px;display:grid;place-items:center;background:linear-gradient(90deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.03)}

    /* Responsive */
    @media (max-width:920px){.wrap{grid-template-columns:1fr;max-width:920px;padding:18px} .banner{height:120px}}

    /* tiny helper */
    a{text-decoration:none;color:inherit}

  </style>
</head>
<body>
  <canvas id="particles"></canvas>
  <div id="app">
    <div class="wrap">
      <div class="card profile-3d" id="card">
        <div class="glow-border"></div>
        <div class="inner-card" id="inner">
          <div class="avatar" id="avatar">VB</div>
          <div class="name">Venkatesh (venkatesh0029)</div>
          <div class="role">2nd Year · Full Stack · Blockchain & DApps</div>
          <div class="bio">Building beautiful products with Java, MERN, and blockchain. Volunteering @ Hyma Hospital.</div>
          <div class="skills" aria-hidden>
            <span class="chip">React</span><span class="chip">Java</span><span class="chip">Spring</span><span class="chip">Node</span><span class="chip">MongoDB</span><span class="chip">Docker</span>
          </div>
          <div class="cta">
            <a class="btn" href="#projects">View Projects</a>
            <a class="btn secondary" href="#contact">Contact</a>
          </div>
        </div>
      </div>

      <div class="main-panel">
        <!-- Animated SVG Banner (AI-aesthetic simulated) -->
        <div class="banner" role="img" aria-label="Aesthetic header">
          <!-- Inline SVG with animated gradients + shapes to emulate AI-generated header -->
          <svg viewBox="0 0 1200 300" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <linearGradient id="g1" x1="0" x2="1">
                <stop offset="0%" stop-color="#00F7FF" stop-opacity="0.95">
                  <animate attributeName="stop-opacity" values="0.95;0.6;0.95" dur="6s" repeatCount="indefinite"/>
                </stop>
                <stop offset="100%" stop-color="#8B5CF6" stop-opacity="0.9">
                  <animate attributeName="stop-opacity" values="0.9;0.5;0.9" dur="6s" repeatCount="indefinite"/>
                </stop>
              </linearGradient>
              <filter id="grain">
                <feTurbulence baseFrequency="0.8" numOctaves="2" stitchTiles="stitch" result="t"/>
                <feColorMatrix type="saturate" values="0"/>
                <feBlend in="SourceGraphic"/>
              </filter>
            </defs>
            <rect width="1200" height="300" fill="url(#g1)" />
            <!-- animated blob shapes -->
            <g opacity="0.9">
              <path fill="#021124" opacity="0.12">
                <animate attributeName="d" dur="8s" repeatCount="indefinite" values="M0,60 C200,120 400,0 600,60 C800,120 1000,0 1200,60 L1200,300 L0,300 Z;M0,40 C220,80 420,140 600,40 C780,-20 980,80 1200,40 L1200,300 L0,300 Z;M0,60 C200,120 400,0 600,60 C800,120 1000,0 1200,60 L1200,300 L0,300 Z"/>
              </path>
            </g>
            <g transform="translate(60,40)">
              <g transform="translate(0,0)">
                <circle cx="60" cy="60" r="36" fill="#021124" opacity="0.08">
                  <animate attributeName="cx" dur="10s" values="60;160;60" repeatCount="indefinite"/>
                </circle>
                <circle cx="240" cy="40" r="48" fill="#021124" opacity="0.06">
                  <animate attributeName="cx" dur="12s" values="240;420;240" repeatCount="indefinite"/>
                </circle>
              </g>
            </g>
            <text x="48" y="190" fill="rgba(255,255,255,0.95)" font-size="38" font-weight="700" font-family="Inter, sans-serif">Venkatesh — Full Stack · Blockchain</text>
            <text x="48" y="225" fill="rgba(255,255,255,0.65)" font-size="14">Building beautiful UIs, secure backends, and decentralized apps.</text>
          </svg>
        </div>

        <!-- Projects section -->
        <h3 id="projects">Projects</h3>
        <div class="projects">
          <div class="project">
            <div class="thumb"><img src="https://images.unsplash.com/photo-1526378725246-9a6afc61fbae?q=80&w=800&auto=format&fit=crop&ixlib=rb-4.0.3&s=placeholder" alt="project-1"/></div>
            <div class="proj-meta"><div class="proj-title">E-Commerce (Amazon-style)</div><div class="proj-tags"><span class="tag">React</span><span class="tag">MySQL</span></div></div>
          </div>

          <div class="project">
            <div class="thumb"><img src="https://images.unsplash.com/photo-1522071820081-009f0129c71c?q=80&w=800&auto=format&fit=crop&ixlib=rb-4.0.3&s=placeholder" alt="project-2"/></div>
            <div class="proj-meta"><div class="proj-title">Secure Genius — Password Analyzer</div><div class="proj-tags"><span class="tag">Java</span><span class="tag">Security</span></div></div>
          </div>

          <div class="project">
            <div class="thumb"><img src="https://images.unsplash.com/photo-1545239351-1141bd82e8a6?q=80&w=800&auto=format&fit=crop&ixlib=rb-4.0.3&s=placeholder" alt="project-3"/></div>
            <div class="proj-meta"><div class="proj-title">AI Agents with n8n</div><div class="proj-tags"><span class="tag">n8n</span><span class="tag">Automation</span></div></div>
          </div>

          <div class="project">
            <div class="thumb"><img src="https://images.unsplash.com/photo-1555066931-4365d14bab8c?q=80&w=800&auto=format&fit=crop&ixlib=rb-4.0.3&s=placeholder" alt="project-4"/></div>
            <div class="proj-meta"><div class="proj-title">Spotify Clone</div><div class="proj-tags"><span class="tag">Full-Stack</span><span class="tag">Animations</span></div></div>
          </div>
        </div>

        <div style="height:12px"></div>
        <!-- Buttons for projects (CTA) -->
        <div style="display:flex;gap:10px;flex-wrap:wrap">
          <a class="btn" href="#" aria-label="Open Project 1">Open Project 1</a>
          <a class="btn" href="#" aria-label="Open Project 2">Open Project 2</a>
          <a class="btn secondary" href="#" aria-label="Explore All Repos">Explore Repos</a>
        </div>

        <!-- Contact / Connect -->
        <div id="contact" style="margin-top:18px;display:flex;align-items:center;justify-content:space-between;gap:16px">
          <div>
            <div style="font-weight:700">Connect with me</div>
            <div style="color:#bcd8ff;margin-top:6px">LinkedIn · Twitter · Portfolio · Email</div>
            <div class="connect" style="margin-top:10px">
              <a class="icon-btn" href="#" title="LinkedIn">in</a>
              <a class="icon-btn" href="#" title="Twitter">tw</a>
              <a class="icon-btn" href="#" title="Portfolio">pf</a>
            </div>
          </div>
          <div style="text-align:right">
            <div style="font-weight:700">Open to:</div>
            <div style="color:#bcd8ff;margin-top:6px">Internships · Open-source · Mentorship</div>
            <div style="margin-top:10px"><span class="badge">Open to collaborate</span></div>
          </div>
        </div>

      </div>
    </div>
  </div>

  <script>
    /* Simple particle system (canvas) */
    const canvas = document.getElementById('particles');
    const ctx = canvas.getContext('2d');
    let w = canvas.width = innerWidth;
    let h = canvas.height = innerHeight;
    const particles = [];
    const TAU = Math.PI * 2;

    function rand(a,b){return Math.random()*(b-a)+a}
    function resize(){w=canvas.width=innerWidth;h=canvas.height=innerHeight}
    addEventListener('resize',resize);

    class P{constructor(){this.reset()}reset(){this.x=rand(0,w);this.y=rand(0,h);this.vx=rand(-0.2,0.2);this.vy=rand(-0.2,0.2);this.r=rand(0.6,2.8);this.life=rand(60,360)}step(){this.x+=this.vx;this.y+=this.vy;this.life--; if(this.x<0||this.x>w||this.y<0||this.y>h){if(Math.random()<0.02) this.reset();}}draw(){ctx.beginPath();ctx.globalAlpha=0.7;ctx.fillStyle='rgba(200,240,255,0.14)';ctx.arc(this.x,this.y,this.r,0,TAU);ctx.fill()}}

    for(let i=0;i<120;i++)particles.push(new P());

    function loop(){ctx.clearRect(0,0,w,h);
      // faint radial gradient
      const g = ctx.createRadialGradient(w*0.2,h*0.2,10,w*0.5,h*0.5,Math.max(w,h));
      g.addColorStop(0,'rgba(0,0,0,0.0)'); g.addColorStop(1,'rgba(0,0,0,0.08)');
      ctx.fillStyle=g; ctx.fillRect(0,0,w,h);

      // draw particles
      for(const p of particles){p.step();p.draw();}

      // links
      for(let i=0;i<particles.length;i++){
        for(let j=i+1;j<i+4 && j<particles.length;j++){
          const a=particles[i], b=particles[j];
          const dx=a.x-b.x, dy=a.y-b.y; const d=dx*dx+dy*dy;
          if(d<12000){ctx.beginPath();ctx.globalAlpha=0.06;ctx.strokeStyle='rgba(140,100,255,0.12)';ctx.moveTo(a.x,a.y);ctx.lineTo(b.x,b.y);ctx.stroke()}
        }
      }
      requestAnimationFrame(loop);
    }
    loop();

    /* 3D hover tilt for profile card */
    const card = document.getElementById('card');
    const inner = document.getElementById('inner');
    const avatar = document.getElementById('avatar');
    let bounds = card.getBoundingClientRect();
    function onMove(e){const x = (e.clientX - bounds.left) / bounds.width - 0.5; const y = (e.clientY - bounds.top) / bounds.height - 0.5; const rx = -y * 10; const ry = x * 12; card.style.transform = `rotateX(${rx}deg) rotateY(${ry}deg)`; inner.style.transform = `translateZ(20px)`; avatar.style.transform = `translateZ(48px) scale(1.02)`}
    function onLeave(){card.style.transform='rotateX(0deg) rotateY(0deg)'; inner.style.transform='translateZ(0)'; avatar.style.transform='translateZ(40px)'}
    card.addEventListener('mousemove',onMove); card.addEventListener('mouseleave',onLeave); addEventListener('resize',()=>bounds=card.getBoundingClientRect());

    /* small accessibility: keyboard focus for buttons */
    document.querySelectorAll('.btn').forEach(b=>{b.addEventListener('focus',()=>b.style.boxShadow='0 12px 40px rgba(0,247,255,0.12)'); b.addEventListener('blur',()=>b.style.boxShadow='')});

    /* Lazy replace placeholder images with repo thumbnails - user to replace with own repo thumbnails */
    // NOTE: The 'AI-generated' header is an SVG above; if you want a real AI image, replace the header <svg> with an <img> pointing to your generated image.
  </script>
</body>
</html>
