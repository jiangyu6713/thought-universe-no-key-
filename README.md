[index.html](https://github.com/user-attachments/files/25841155/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Thought Universe · The Universe Within (禅意气泡)</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400&family=Lato:wght@300;400&family=Montserrat:wght@500&family=Roboto+Mono&display=swap" rel="stylesheet">
    <style>
        /* 样式基于 V1.3，修改滑块按钮和 AI 弹窗 */
        body { margin:0; overflow:hidden; font-family:'Helvetica Neue','Arial',sans-serif; font-weight:300; background:#000; }
        #bg-canvas { display:block; position:fixed; top:0; left:0; width:100%; height:100%; z-index:1; pointer-events:auto; }
        .ui-layer { position:absolute; top:0; left:0; width:100%; height:100%; z-index:10; pointer-events:none; }
        .ui-layer > * { pointer-events:auto; }
        .button-fold { position:absolute; top:24px; left:24px; width:200px; perspective:600px; z-index:30; }
        .fold-btn { width:100%; background:rgba(255,255,255,0.1); backdrop-filter:blur(12px); border:1px solid rgba(255,255,255,0.2); border-radius:0; padding:10px 16px; font-size:14px; font-weight:500; font-family:'Montserrat',sans-serif; color:rgba(255,255,255,0.8); cursor:pointer; text-transform:uppercase; letter-spacing:1px; transition:background 0.2s; text-align:center; }
        .fold-btn:hover { background:rgba(255,255,255,0.2); }
        .record-panel { position:absolute; top:100%; left:0; width:100%; max-height:0; overflow:hidden; transition:max-height 0.3s ease; background:transparent; z-index:40; filter:drop-shadow(0 8px 12px rgba(0,0,0,0.3)); pointer-events:none; }
        .button-fold:hover .record-panel { max-height:300px; overflow-y:auto; pointer-events:auto; transition:max-height 0.3s ease; }
        .record-panel::-webkit-scrollbar { width:4px; }
        .record-panel::-webkit-scrollbar-track { background:transparent; }
        .record-panel::-webkit-scrollbar-thumb { background:rgba(255,255,255,0.2); border-radius:0; }
        .record-panel::-webkit-scrollbar-thumb:hover { background:rgba(255,255,255,0.3); }
        .record-item { display:flex; align-items:center; justify-content:space-between; background:rgba(30,30,40,0.7); backdrop-filter:blur(20px); border:1px solid rgba(255,255,255,0.15); border-top:2px solid rgba(255,255,255,0.2); padding:8px 12px; font-size:13px; color:white; transition:transform 0.3s ease-out, opacity 0.3s ease-out, border-top-color 0.3s ease-out, background 0.2s; transform-origin:top; transform:rotateX(90deg); opacity:0; cursor:pointer; border-bottom:none; position:relative; z-index:1; box-shadow:inset 0 1px 3px rgba(0,0,0,0.2); }
        .record-item:last-child { border-bottom:1px solid rgba(255,255,255,0.15); }
        .record-item:hover { background:rgba(50,50,65,0.8); border-color:rgba(255,255,255,0.3); }
        .button-fold:hover .record-item { transform:rotateX(0); opacity:1; border-top-color:transparent; }
        .record-time { flex:1; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; margin-right:8px; font-family:'Inter',sans-serif; font-weight:300; }
        .delete-record { width:20px; height:20px; background:rgba(255,255,255,0.1); border:1px solid rgba(255,255,255,0.2); border-radius:0; color:rgba(255,255,255,0.6); font-size:14px; line-height:1; display:flex; align-items:center; justify-content:center; cursor:pointer; transition:background 0.2s, color 0.2s; margin-left:4px; user-select:none; }
        .delete-record:hover { background:rgba(255,80,80,0.3); color:white; border-color:rgba(255,100,100,0.5); }
        .empty-record { background:rgba(30,30,40,0.7); backdrop-filter:blur(20px); border:1px solid rgba(255,255,255,0.15); padding:16px; text-align:center; color:rgba(255,255,255,0.4); font-style:italic; transform:rotateX(0); opacity:1; font-family:'Inter',sans-serif; }
        #record-count { position:absolute; bottom:24px; left:24px; color:rgba(255,255,255,0.3); background:rgba(255,255,255,0.03); backdrop-filter:blur(8px); padding:6px 16px; border:1px solid rgba(255,255,255,0.1); border-radius:40px; font-size:12px; font-weight:300; letter-spacing:1px; z-index:20; pointer-events:none; font-family:'Inter',sans-serif; }
        #control-toggler { position:absolute; bottom:32px; right:32px; width:44px; height:44px; border-radius:0; background:transparent; border:1px solid rgba(255,255,255,0.2); backdrop-filter:blur(8px); display:flex; align-items:center; justify-content:center; cursor:pointer; z-index:30; color:rgba(255,255,255,0.4); box-shadow:0 0 15px rgba(0,0,0,0.2); transition:all 0.2s ease; }
        #control-toggler::after { content:''; width:6px; height:6px; border-radius:0; background:rgba(255,255,255,0.4); transition:background 0.2s; }
        #control-toggler:hover { border-color:rgba(255,255,255,0.5); }
        #control-toggler:hover::after { background:rgba(255,255,255,0.8); }
        #control-panel { position:absolute; bottom:100px; right:32px; width:260px; background:rgba(255,255,255,0.05); backdrop-filter:blur(20px); border:1px solid rgba(255,255,255,0.1); box-shadow:0 10px 30px rgba(0,0,0,0.3); padding:24px 20px; color:rgba(255,255,255,0.6); transition:opacity 0.25s ease, transform 0.3s ease; z-index:40; transform-origin:bottom right; pointer-events:auto; font-size:12px; letter-spacing:0.5px; border-radius:0; }
        #control-panel.hidden { opacity:0; transform:scale(0.8); pointer-events:none; }
        .panel-title { text-align:center; margin-bottom:24px; color:rgba(255,255,255,0.3); font-size:11px; text-transform:uppercase; letter-spacing:3px; border-bottom:1px solid rgba(255,255,255,0.1); padding-bottom:16px; font-family:'Montserrat',sans-serif; font-weight:500; }
        .slider-group { margin-bottom:28px; }
        .slider-group label { display:flex; justify-content:space-between; margin-bottom:10px; opacity:0.5; font-size:11px; text-transform:uppercase; letter-spacing:1.5px; color:rgba(255,255,255,0.5); font-weight:500; padding:0 2px; font-family:'Montserrat',sans-serif; }
        .slider-group input[type=range] { width:100%; height:1px; background:rgba(255,255,255,0.2); border:none; outline:none; -webkit-appearance:none; appearance:none; cursor:pointer; }
        /* 滑块按钮调小并半透明 */
        .slider-group input[type=range]::-webkit-slider-thumb {
            -webkit-appearance:none;
            width:8px;
            height:8px;
            border-radius:0;
            background:rgba(255,255,255,0.3);
            box-shadow:0 0 5px rgba(255,255,255,0.2);
            margin-top:-3.5px; /* 调整垂直居中 */
        }
        .slider-group .value-badge { font-size:11px; color:rgba(255,255,255,0.6); background:transparent; padding:2px 0; font-family:'Inter',sans-serif; }
        .audio-control { display:flex; align-items:center; justify-content:space-between; margin-top:10px; padding-top:15px; border-top:1px solid rgba(255,255,255,0.1); }
        .audio-btn { background:transparent; border:1px solid rgba(255,255,255,0.3); border-radius:0; width:32px; height:32px; display:flex; align-items:center; justify-content:center; cursor:pointer; font-size:16px; color:rgba(255,255,255,0.6); transition:all 0.2s; }
        .audio-btn:hover { background:rgba(255,255,255,0.1); border-color:rgba(255,255,255,0.6); }
        .audio-label { font-size:11px; text-transform:uppercase; letter-spacing:1px; color:rgba(255,255,255,0.4); }
        .audio-upload { margin-top:8px; display:flex; align-items:center; justify-content:center; }
        .audio-upload label { background:transparent; border:1px solid rgba(255,255,255,0.3); border-radius:0; padding:4px 12px; font-size:11px; color:rgba(255,255,255,0.6); cursor:pointer; transition:background 0.2s; text-transform:uppercase; letter-spacing:1px; width:100%; text-align:center; }
        .audio-upload label:hover { background:rgba(255,255,255,0.1); border-color:rgba(255,255,255,0.6); }
        .audio-upload input { display:none; }
        .hint { text-align:center; font-size:10px; opacity:0.3; margin-top:20px; letter-spacing:1px; border-top:1px solid rgba(255,255,255,0.1); padding-top:18px; font-family:'Inter',sans-serif; }
        #floating-window { position:absolute; top:50%; left:50%; transform:translate(-50%,-50%); width:450px; min-width:300px; background:transparent; backdrop-filter:none; border:1px solid rgba(255,255,255,0.3); border-radius:0; box-shadow:none; color:white; z-index:50; display:flex; flex-direction:column; resize:none; overflow:hidden; pointer-events:auto; }
        .title-bar { height:28px; background:transparent; border-bottom:1px solid rgba(255,255,255,0.2); display:flex; align-items:center; padding:0 12px; cursor:grab; user-select:none; font-size:12px; color:rgba(255,255,255,0.7); letter-spacing:0.5px; }
        .title-bar:active { cursor:grabbing; }
        .timestamp-display::before { content:'●'; margin-right:8px; opacity:0.6; font-size:8px; color:rgba(255,255,255,0.8); }
        .timestamp-display { flex:1; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; font-family:'Roboto Mono',monospace; font-size:12px; }
        .window-content { flex:1; padding:20px; display:flex; flex-direction:column; }
        .window-content textarea { width:100%; background:rgba(0,0,0,0.3); border:none; padding:14px; color:white; font-family:'Lato',sans-serif; font-size:16px; font-weight:300; line-height:1.6; resize:vertical; min-height:100px; outline:none; margin-bottom:16px; box-sizing:border-box; box-shadow:inset 0 0 0 1px rgba(255,255,255,0.1); }
        .window-content textarea::placeholder { color:rgba(255,255,255,0.4); font-style:italic; }
        .window-footer { display:flex; justify-content:flex-end; }
        .window-footer button { background:transparent; border:1px solid rgba(255,255,255,0.4); border-radius:0; padding:8px 22px; font-size:13px; color:rgba(255,255,255,0.9); cursor:pointer; transition:background 0.2s, border-color 0.2s; text-transform:uppercase; letter-spacing:2px; font-weight:500; font-family:'Montserrat',sans-serif; }
        .window-footer button:hover { background:rgba(255,255,255,0.1); border-color:rgba(255,255,255,0.7); }
        .resize-handle { position:absolute; bottom:0; right:0; width:20px; height:20px; cursor:nwse-resize; background:radial-gradient(circle, rgba(255,255,255,0.3) 1px, transparent 1px); background-size:6px 6px; z-index:60; }
        /* AI 回复气泡 - 极简禅意风格 */
        #ai-popup {
            position:absolute;
            top:120px;
            left:50%;
            transform:translateX(-50%);
            width:400px;
            max-width:80vw;
            background:rgba(20,20,20,0.7);
            backdrop-filter:blur(24px);
            border:1px solid rgba(255,255,255,0.1);
            padding:24px;
            color:white;
            box-shadow:0 20px 40px rgba(0,0,0,0.6);
            z-index:80;
            transition:opacity 0.2s ease;
            pointer-events:none;
            text-align:center;
            font-family:'Inter',sans-serif;
            font-size:16px;
            font-weight:300;
            line-height:1.5;
            border-radius:0; /* 直角 */
        }
        #ai-popup.hidden { opacity:0; pointer-events:none; }
        #clear-btn { position:absolute; top:24px; right:24px; width:44px; height:44px; border-radius:0; background:transparent; border:1px solid rgba(255,255,255,0.2); backdrop-filter:blur(8px); display:flex; align-items:center; justify-content:center; cursor:pointer; z-index:30; color:rgba(255,255,255,0.4); transition:all 0.2s ease; font-size:20px; line-height:1; }
        #clear-btn:hover { border-color:rgba(255,100,100,0.8); color:rgba(255,160,160,0.9); background:rgba(255,80,80,0.1); }
        #clear-btn::after { content:'⌫'; }
    </style>
    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.128.0/build/three.module.js"
            }
        }
    </script>
</head>
<body>
    <canvas id="bg-canvas"></canvas>
    <div class="ui-layer">
        <div class="button-fold">
            <button class="fold-btn">PLANET GALLERY</button>
            <div class="record-panel" id="record-panel"><div class="empty-record">No records</div></div>
        </div>
        <div id="record-count">0 entries</div>
        <div id="control-toggler" class="clickable" title="Adjust universe parameters"></div>
        <div id="control-panel" class="hidden">
            <div class="panel-title">— UNIVERSE PARAMETERS —</div>
            <div class="slider-group">
                <label><span>Hue Shift</span> <span id="hue-val" class="value-badge">0.00</span></label>
                <input type="range" id="hue-slider" min="-1.0" max="1.0" step="0.01" value="0.0">
            </div>
            <div class="slider-group">
                <label><span>Speed</span> <span id="speed-val" class="value-badge">1.00</span></label>
                <input type="range" id="speed-slider" min="0.0" max="2.5" step="0.01" value="1.0">
            </div>
            <div class="slider-group">
                <label><span>Intensity</span> <span id="intensity-val" class="value-badge">0.30</span></label>
                <input type="range" id="intensity-slider" min="0.1" max="0.5" step="0.01" value="0.3">
            </div>
            <div class="audio-control">
                <span class="audio-label">Meditation Music</span>
                <div class="audio-btn" id="audio-toggle">🔇</div>
            </div>
            <div class="audio-upload">
                <label for="audio-upload">Upload Music</label>
                <input type="file" id="audio-upload" accept="audio/*">
            </div>
            <div class="hint">Intensity controls visual brightness</div>
        </div>
        <div id="floating-window" class="clickable">
            <div class="title-bar" id="title-bar">
                <span class="timestamp-display" id="timestamp-display">Space Museum</span>
            </div>
            <div class="window-content">
                <textarea id="thought-input" placeholder="Write your thoughts..."></textarea>
                <div class="window-footer">
                    <button id="save-btn">SAVE</button>
                </div>
            </div>
            <div class="resize-handle" id="resize-handle"></div>
        </div>
        <div id="ai-popup" class="hidden">✨</div>
        <div id="clear-btn" class="clickable" title="Clear all entries"></div>
    </div>

    <script type="module">
        import * as THREE from 'three';

        // --- 初始化渲染器、场景、相机 ---
        const canvas = document.getElementById('bg-canvas');
        const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: false, powerPreference: "high-performance" });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.setClearColor(0x000000);

        const scene = new THREE.Scene();
        const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0.1, 10);
        camera.position.z = 1;

        // --- Uniforms ---
        const bgUniforms = {
            u_time: { value: 0 },
            u_resolution: { value: new THREE.Vector2(window.innerWidth, window.innerHeight) },
            u_mouse: { value: new THREE.Vector2(0.5, 0.5) },
            u_hueShift: { value: 0.0 },
            u_speed: { value: 1.0 },
            u_intensity: { value: 0.3 },
            u_fft: { value: 0.3 }
        };

        const vertexShader = `varying vec2 vUv; void main() { vUv = uv; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }`;

        // 片段着色器（无暗角版）
        const fragmentShader = `
            precision highp float;
            varying vec2 vUv;
            uniform vec2 u_resolution;
            uniform float u_time;
            uniform vec2 u_mouse;
            uniform float u_hueShift;
            uniform float u_speed;
            uniform float u_intensity;
            uniform float u_fft;

            #define S(a, b, t) smoothstep(a, b, t)
            #define NUM_LAYERS 4.

            float N21(vec2 p) {
                vec3 a = fract(vec3(p.xyx) * vec3(213.897, 653.453, 253.098));
                a += dot(a, a.yzx + 79.76);
                return fract((a.x + a.y) * a.z);
            }

            vec2 GetPos(vec2 id, vec2 offs, float t) {
                float n = N21(id+offs);
                float n1 = fract(n*10.);
                float n2 = fract(n*100.);
                float a = t+n;
                return offs + vec2(sin(a*n1), cos(a*n2))*.4;
            }

            float df_line( in vec2 a, in vec2 b, in vec2 p) {
                vec2 pa = p - a, ba = b - a;
                float h = clamp(dot(pa,ba) / dot(ba,ba), 0., 1.);
                return length(pa - ba * h);
            }

            float line(vec2 a, vec2 b, vec2 uv) {
                float r1 = .04;
                float r2 = .01;
                float d = df_line(a, b, uv);
                float d2 = length(a-b);
                float fade = S(1.5, .5, d2);
                fade += S(.05, .02, abs(d2-.75));
                return S(r1, r2, d)*fade;
            }

            float NetLayer(vec2 st, float n, float t) {
                vec2 id = floor(st)+n;
                st = fract(st)-.5;
                vec2 p[9];
                int i=0;
                for(float y=-1.; y<=1.; y++) {
                    for(float x=-1.; x<=1.; x++) {
                        p[i++] = GetPos(id, vec2(x,y), t);
                    }
                }
                float m = 0.;
                float sparkle = 0.;
                for(int i=0; i<9; i++) {
                    m += line(p[4], p[i], st);
                    float d = length(st-p[i]);
                    float s = (.005/(d*d));
                    s *= S(1., .7, d);
                    float pulse = sin((fract(p[i].x)+fract(p[i].y)+t)*5.)*.4+.6;
                    pulse = pow(pulse, 20.);
                    s *= pulse;
                    sparkle += s;
                }
                m += line(p[1], p[3], st);
                m += line(p[1], p[5], st);
                m += line(p[7], p[5], st);
                m += line(p[7], p[3], st);
                float sPhase = (sin(t+n)+sin(t*.1))*.25+.5;
                sPhase += pow(sin(t*.1)*.5+.5, 50.)*5.;
                m += sparkle * sPhase;
                return m;
            }

            vec3 hueShift(vec3 color, float shift) {
                const vec3 k = vec3(0.57735, 0.57735, 0.57735);
                float cosAngle = cos(shift);
                return color * cosAngle + cross(k, color) * sin(shift) + k * dot(k, color) * (1.0 - cosAngle);
            }

            void main() {
                vec2 uv = (gl_FragCoord.xy - u_resolution.xy * 0.5) / u_resolution.y;
                vec2 mouse = u_mouse - 0.5;
                float t = u_time * u_speed * 0.1;
                float s = sin(t);
                float c = cos(t);
                mat2 rot = mat2(c, -s, s, c);
                vec2 st = uv * rot;  
                mouse *= rot * 2.0;
                
                float m = 0.0;
                for(float i=0.0; i<1.0; i+=1.0/NUM_LAYERS) {
                    float z = fract(t+i);
                    float size = mix(15.0, 1.0, z);
                    float fade = S(0.0, 0.6, z) * S(1.0, 0.8, z);
                    m += fade * NetLayer(st * size - mouse * z, i, u_time);
                }
                
                float fft = u_fft;
                float glow = -uv.y * fft * 2.0;
                vec3 baseCol = vec3(s, cos(t*0.4), -sin(t*0.24)) * 0.4 + 0.6;
                vec3 col = baseCol * m;
                col += baseCol * glow;
                col = hueShift(col, u_hueShift * 6.28318);
                col *= u_intensity;
                gl_FragColor = vec4(col, 1.0);
            }
        `;

        const bgGeometry = new THREE.PlaneGeometry(2, 2);
        const bgMaterial = new THREE.ShaderMaterial({ 
            vertexShader, 
            fragmentShader, 
            uniforms: bgUniforms 
        });
        const backgroundMesh = new THREE.Mesh(bgGeometry, bgMaterial);
        scene.add(backgroundMesh);

        // 环境光（保留）
        const ambientLight = new THREE.AmbientLight(0x404060);
        scene.add(ambientLight);
        const dirLight = new THREE.DirectionalLight(0xffffff, 1);
        dirLight.position.set(1, 2, 1);
        scene.add(dirLight);
        const backLight = new THREE.PointLight(0x446688, 0.5);
        backLight.position.set(-1, -1, -1);
        scene.add(backLight);

        // --- 鼠标移动监听 ---
        window.addEventListener('mousemove', (e) => {
            const x = e.clientX / window.innerWidth;
            const y = 1.0 - (e.clientY / window.innerHeight);
            bgUniforms.u_mouse.value.set(x, y);
        });

        window.addEventListener('resize', () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            bgUniforms.u_resolution.value.set(window.innerWidth, window.innerHeight);
        });

        // --- 历史记录管理 ---
        let thoughts = [];
        const STORAGE_KEY = 'cosmic_thoughts';
        let currentLoadedTimestamp = null;

        function loadThoughts() {
            const stored = localStorage.getItem(STORAGE_KEY);
            if (stored) { try { thoughts = JSON.parse(stored); } catch { thoughts = []; } } else { thoughts = []; }
            updateHistoryUI();
        }
        function saveThoughts() { localStorage.setItem(STORAGE_KEY, JSON.stringify(thoughts)); updateHistoryUI(); }
        function addThought(text) {
            if (!text.trim()) return;
            thoughts.unshift({ text: text.trim(), timestamp: Date.now() });
            if (thoughts.length > 30) thoughts.pop();
            saveThoughts();
            document.getElementById('thought-input').value = '';
            setTitleBarToNew();
        }
        function deleteThought(index) {
            if (index >= 0 && index < thoughts.length) {
                thoughts.splice(index, 1);
                saveThoughts();
                if (currentLoadedTimestamp && thoughts.find(t => t.timestamp === currentLoadedTimestamp) === undefined) {
                    setTitleBarToNew(); currentLoadedTimestamp = null;
                }
            }
        }
        function clearAllThoughts() {
            thoughts = [];
            localStorage.removeItem(STORAGE_KEY);
            updateHistoryUI();
            setTitleBarToNew();
            currentLoadedTimestamp = null;
        }
        function formatTimestamp(ts) {
            const d = new Date(ts);
            return `${d.getFullYear()}.${(d.getMonth()+1).toString().padStart(2,'0')}.${d.getDate().toString().padStart(2,'0')} ${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}`;
        }
        function setTitleBarToTimestamp(ts) { document.getElementById('timestamp-display').innerText = formatTimestamp(ts); }
        function setTitleBarToNew() { document.getElementById('timestamp-display').innerText = 'Space Museum'; }

        // --- API 配置（已删除密钥，AI将返回模拟错误信息）---
        const DASHSCOPE_API_KEY = ''; // 密钥已移除，请自行填入有效密钥
        const DASHSCOPE_BASE_URL = 'https://dashscope.aliyuncs.com/compatible-mode/v1';
        const MODEL_NAME = 'qwen-plus';

        async function generateAIResponse(userText) {
            if (!userText.trim()) return "✨ I'm here to listen...";
            try {
                const response = await fetch(`${DASHSCOPE_BASE_URL}/chat/completions`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${DASHSCOPE_API_KEY}`
                    },
                    body: JSON.stringify({
                        model: MODEL_NAME,
                        messages: [
                            { role: 'system', content: '你是一个温柔、有智慧的倾听者。你不急于给出建议，而是通过提问引导用户自我探索。用1-2句话回应，语气温暖但克制，像一位安静陪伴的朋友。' },
                            { role: 'user', content: userText }
                        ],
                        temperature: 0.8,
                        max_tokens: 150
                    })
                });
                if (!response.ok) {
                    const errorText = await response.text();
                    throw new Error(`HTTP ${response.status}: ${errorText}`);
                }
                const data = await response.json();
                return data.choices[0].message.content;
            } catch (error) {
                console.error('AI生成失败:', error);
                return `✨ 宇宙暂时安静 (错误: ${error.message})`;
            }
        }

        let aiPopupTimer = null;
        const aiPopup = document.getElementById('ai-popup');
        function showAIResponse(text) {
            if (aiPopupTimer) clearTimeout(aiPopupTimer);
            aiPopup.innerText = text;
            aiPopup.classList.remove('hidden');
            aiPopupTimer = setTimeout(() => aiPopup.classList.add('hidden'), 4000);
        }
        async function updateAIResponse(text) {
            const response = await generateAIResponse(text);
            showAIResponse(response);
        }

        // --- 历史 UI 更新 ---
        function updateHistoryUI() {
            const panel = document.getElementById('record-panel');
            const countSpan = document.getElementById('record-count');
            countSpan.innerText = `${thoughts.length} entries`;
            if (thoughts.length === 0) { panel.innerHTML = '<div class="empty-record">No records</div>'; return; }
            let html = '';
            thoughts.forEach((thought, index) => {
                html += `<div class="record-item" data-index="${index}"><span class="record-time">${formatTimestamp(thought.timestamp)}</span><span class="delete-record" data-index="${index}" title="Delete">✕</span></div>`;
            });
            panel.innerHTML = html;
            const items = document.querySelectorAll('.record-item');
            items.forEach((item, idx) => {
                item.style.transitionDelay = idx * 0.1 + 's';
                item.addEventListener('click', (e) => {
                    if (e.target.classList.contains('delete-record')) return;
                    const index = item.dataset.index;
                    if (index !== undefined) {
                        const thought = thoughts[parseInt(index)];
                        if (thought) {
                            document.getElementById('thought-input').value = thought.text;
                            setTitleBarToTimestamp(thought.timestamp);
                            currentLoadedTimestamp = thought.timestamp;
                            updateAIResponse(thought.text);
                        }
                    }
                });
            });
            document.querySelectorAll('.delete-record').forEach(btn => {
                btn.addEventListener('click', (e) => { e.stopPropagation(); const index = btn.dataset.index; if (index !== undefined) deleteThought(parseInt(index)); });
            });
        }

        loadThoughts();

        const saveBtn = document.getElementById('save-btn');
        const thoughtInput = document.getElementById('thought-input');
        const clearBtn = document.getElementById('clear-btn');

        saveBtn.addEventListener('click', () => {
            const text = thoughtInput.value.trim();
            if (text === '') { alert('Please enter something'); return; }
            addThought(text);
            updateAIResponse(text);
        });
        thoughtInput.addEventListener('keydown', (e) => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); saveBtn.click(); } });
        clearBtn.addEventListener('click', () => { if (confirm('Clear all entries?')) clearAllThoughts(); });

        // --- 滑块控制（Intensity 范围 0.1-0.5）---
        const hueSlider = document.getElementById('hue-slider');
        const hueVal = document.getElementById('hue-val');
        hueSlider.addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            hueVal.innerText = val.toFixed(2);
            bgUniforms.u_hueShift.value = val;
        });

        const speedSlider = document.getElementById('speed-slider');
        const speedVal = document.getElementById('speed-val');
        speedSlider.addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            speedVal.innerText = val.toFixed(2);
            bgUniforms.u_speed.value = val;
        });

        const intensitySlider = document.getElementById('intensity-slider');
        const intensityVal = document.getElementById('intensity-val');
        intensitySlider.addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            intensityVal.innerText = val.toFixed(2);
            bgUniforms.u_intensity.value = val;
            bgUniforms.u_fft.value = val;
        });

        // 控制面板显示/隐藏
        const toggler = document.getElementById('control-toggler');
        const panel = document.getElementById('control-panel');
        toggler.addEventListener('click', () => {
            panel.classList.toggle('hidden');
        });

        // --- 窗口拖拽与缩放 ---
        const floatingWindow = document.getElementById('floating-window');
        const titleBar = document.getElementById('title-bar');
        const resizeHandle = document.getElementById('resize-handle');

        let isDragging = false;
        let dragOffsetX = 0, dragOffsetY = 0;

        titleBar.addEventListener('mousedown', (e) => {
            isDragging = true;
            const rect = floatingWindow.getBoundingClientRect();
            dragOffsetX = e.clientX - rect.left;
            dragOffsetY = e.clientY - rect.top;
            floatingWindow.style.cursor = 'grabbing';
            e.preventDefault();
        });

        window.addEventListener('mousemove', (e) => {
            if (!isDragging) return;
            let newLeft = e.clientX - dragOffsetX;
            let newTop = e.clientY - dragOffsetY;
            newLeft = Math.max(0, Math.min(window.innerWidth - floatingWindow.offsetWidth, newLeft));
            newTop = Math.max(0, Math.min(window.innerHeight - floatingWindow.offsetHeight, newTop));
            floatingWindow.style.left = newLeft + 'px';
            floatingWindow.style.top = newTop + 'px';
            floatingWindow.style.transform = 'none';
        });

        window.addEventListener('mouseup', () => {
            if (isDragging) {
                isDragging = false;
                floatingWindow.style.cursor = 'default';
            }
        });

        let isResizing = false;
        let startWidth, startHeight, startX, startY;

        resizeHandle.addEventListener('mousedown', (e) => {
            isResizing = true;
            startWidth = floatingWindow.offsetWidth;
            startHeight = floatingWindow.offsetHeight;
            startX = e.clientX;
            startY = e.clientY;
            e.preventDefault();
            e.stopPropagation();
        });

        window.addEventListener('mousemove', (e) => {
            if (!isResizing) return;
            const dx = e.clientX - startX;
            const dy = e.clientY - startY;
            let newWidth = startWidth + dx;
            let newHeight = startHeight + dy;
            newWidth = Math.max(300, newWidth);
            newHeight = Math.max(150, newHeight);
            floatingWindow.style.width = newWidth + 'px';
            floatingWindow.style.height = newHeight + 'px';
            floatingWindow.style.transform = 'none';
        });

        window.addEventListener('mouseup', () => {
            isResizing = false;
        });

        // --- 动画循环 ---
        let clock = new THREE.Clock();
        function animate() {
            requestAnimationFrame(animate);
            bgUniforms.u_time.value += clock.getDelta();
            renderer.render(scene, camera);
        }
        animate();

        // 初始化分辨率
        bgUniforms.u_resolution.value.set(window.innerWidth, window.innerHeight);
    </script>
</body>
</html>
