# Solar-System-Simulation-with-Scientific-References
Bridging my physics background with a passion for coding, this simulation makes celestial mechanics visual and intuitive. It's an educational tool designed for high school and undergraduate students, aiming to make complex science engaging for the next generation of innovators. This is my contribution to the STEM community.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Solar System Simulation with Scientific References</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Fira+Code:wght@400&display=swap" rel="stylesheet">
    <!-- KaTeX for rendering mathematical equations -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" xintegrity="sha384-n8MVd4RsNIU0KOVEMVIhbCwHMAq2hIoZ4C0eRMLRTgMfxC0EFe14GEs2elGpc2J4" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" xintegrity="sha384-XjKyOOlGwcjNTAIQHIpgOno0Hl1YQqzUOEleOLALmuqehneUG+vnGctmUb0ZY0l8" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" xintegrity="sha384-+VBxd3r6XgURycqtZ117nYw44OOcIax56Z4dCRWbxyPt0Koah1uHoK0o4+/RRE05" crossorigin="anonymous"></script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
            overflow: hidden;
        }
        canvas {
            display: block;
            background-color: #00001a;
            width: 100vw;
            height: 100vh;
        }
        .controls {
            position: absolute;
            top: 1rem;
            left: 1rem;
            background-color: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(8px);
            padding: 1.25rem;
            border-radius: 0.5rem;
            color: white;
            max-width: 380px;
            max-height: calc(100vh - 2rem);
            overflow-y: auto;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        .controls h1 {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: #FBBF24;
        }
        .controls details {
            border-bottom: 1px solid #374151;
            margin-bottom: 0.75rem;
        }
        .controls summary {
            font-size: 1rem;
            font-weight: 600;
            padding: 0.5rem 0;
            cursor: pointer;
            list-style: none;
        }
        .controls summary::-webkit-details-marker {
            display: none;
        }
        .controls summary::before {
            content: '► ';
            font-size: 0.8em;
            margin-right: 0.5rem;
            transition: transform 0.2s;
        }
        .controls details[open] summary::before {
            transform: rotate(90deg);
        }
        .content-box {
            padding: 0 0 0.75rem 0.5rem;
        }
        .content-box p, .content-box .equation {
            font-size: 0.875rem;
            margin-bottom: 0.5rem;
            line-height: 1.5;
            color: #D1D5DB;
        }
        .content-box .equation {
            text-align: center;
            padding: 0.5rem 0;
            background-color: rgba(0,0,0,0.2);
            border-radius: 0.25rem;
        }
        .controls button {
            width: 100%;
            padding: 0.5rem;
            border-radius: 0.375rem;
            background-color: #4f46e5;
            color: white;
            font-weight: 500;
            border: none;
            cursor: pointer;
            transition: background-color 0.2s;
            margin-top: 1rem;
        }
        .controls button:hover {
            background-color: #4338ca;
        }
        .katex { font-size: 1.1em !important; }
        #relative-speeds-list { list-style: none; padding: 0; font-size: 0.8rem; }
        #relative-speeds-list li { display: flex; justify-content: space-between; padding: 0.1rem 0; }
        #relative-speeds-list .planet-name { font-weight: 500; }
        #relative-speeds-list .speed-value { font-family: 'Fira Code', monospace; color: #A5B4FC; }
        .content-box a { color: #60A5FA; text-decoration: underline; }
        .watermark {
            position: absolute;
            bottom: 1rem;
            right: 1rem;
            font-size: 0.875rem;
            color: rgba(255, 255, 255, 0.2);
            pointer-events: none;
            z-index: 100;
        }
    </style>
</head>
<body class="bg-gray-900">
    <canvas id="simulationCanvas"></canvas>
    <div class="controls">
        <h1>Solar System Dynamics</h1>
        
        <details>
            <summary>Live Data</summary>
            <div class="content-box">
                 <p><b>Relative Speed (to Earth)</b></p>
                 <ul id="relative-speeds-list"></ul>
            </div>
        </details>

        <details>
            <summary>Governing Principles</summary>
            <div class="content-box">
                <p><b>Newton's Law of Universal Gravitation:</b> The force between any two bodies is proportional to the product of their masses and inversely proportional to the square of the distance between them.</p>
                <div class="equation">$$F = G \frac{m_1 m_2}{r^2}$$</div>
                <p><b>Newton's Second Law:</b> The acceleration of a body is directly proportional to the net force acting on it and inversely proportional to its mass.</p>
                <div class="equation">$$\vec{a} = \frac{\vec{F}_{net}}{m}$$</div>
            </div>
        </details>

        <details>
            <summary>Computational Method</summary>
            <div class="content-box">
                <p>The simulation uses the <b>Semi-Implicit Euler</b> method to numerically integrate the equations of motion over discrete time steps ($\Delta t$).</p>
                <p>1. Update Velocity:</p>
                <div class="equation">$$\vec{v}(t + \Delta t) = \vec{v}(t) + \vec{a}(t) \Delta t$$</div>
                <p>2. Update Position:</p>
                <div class="equation">$$\vec{x}(t + \Delta t) = \vec{x}(t) + \vec{v}(t + \Delta t) \Delta t$$</div>
            </div>
        </details>
        
        <details>
            <summary>Kepler's Laws Visualization</summary>
            <div class="content-box">
                <p><b>1st Law:</b> Planets move in elliptical orbits. The long trails illustrate these non-circular paths.</p>
                <p><b>2nd Law:</b> Planets move fastest when closest to the Sun. The yellow vector indicates velocity—notice its length change.</p>
                <p><b>3rd Law:</b> Inner planets orbit much faster than outer planets, a direct result of the force equation.</p>
            </div>
        </details>

        <details>
            <summary>Data Source & Notes</summary>
            <div class="content-box">
                <p>The physical parameters for mass and distance in this simulation are scaled for visual stability and are not scientifically precise. For canonical astronomical data, refer to:</p>
                <p><a href="https://nssdc.gsfc.nasa.gov/planetary/factsheet/" target="_blank">NASA NSSDCA - Planetary Fact Sheet</a></p>
            </div>
        </details>

        <button id="resetButton">Reset Simulation</button>
    </div>
    <div class="watermark">
        Developed & Designed by Danish Khan
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
             renderMathInElement(document.body, {
                delimiters: [
                    {left: '$$', right: '$$', display: true},
                    {left: '$', right: '$', display: false}
                ]
            });
        });

        const canvas = document.getElementById('simulationCanvas');
        const ctx = canvas.getContext('2d');
        const resetButton = document.getElementById('resetButton');
        const speedsList = document.getElementById('relative-speeds-list');

        let bodies = [];
        let animationFrameId;

        // --- Configuration ---
        const G = 2; 
        const TIME_STEP = 0.015;
        const TRAIL_LENGTH = 800;
        const VELOCITY_VECTOR_SCALE = 4;

        // --- Utility Functions ---
        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }

        // --- Body Class ---
        class Body {
            constructor(name, x, y, vx, vy, mass, color) {
                this.name = name;
                this.x = x;
                this.y = y;
                this.vx = vx;
                this.vy = vy;
                this.mass = mass;
                this.radius = Math.max(2, Math.pow(mass, 1/3) / 2);
                this.color = color;
                this.trail = [];
            }

            updatePosition() {
                this.x += this.vx * TIME_STEP;
                this.y += this.vy * TIME_STEP;
                this.trail.push({ x: this.x, y: this.y });
                if (this.trail.length > TRAIL_LENGTH) {
                    this.trail.shift();
                }
            }

            draw() {
                // Draw trail
                if (this.trail.length > 1) {
                    ctx.beginPath();
                    ctx.moveTo(this.trail[0].x, this.trail[0].y);
                    for (let i = 1; i < this.trail.length; i++) {
                        const opacity = i / this.trail.length;
                        ctx.strokeStyle = `rgba(${hexToRgb(this.color)}, ${opacity * 0.6})`;
                        ctx.lineWidth = 1;
                        ctx.lineTo(this.trail[i].x, this.trail[i].y);
                    }
                    ctx.stroke();
                }

                // Draw body
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fillStyle = this.color;
                ctx.fill();
                ctx.shadowColor = this.color;
                ctx.shadowBlur = 15;

                // Draw label
                ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                ctx.font = '12px Inter';
                ctx.fillText(this.name, this.x + this.radius + 5, this.y);

                // Draw velocity vector
                if (this.name !== 'Sun') {
                    ctx.beginPath();
                    ctx.moveTo(this.x, this.y);
                    ctx.lineTo(this.x + this.vx * VELOCITY_VECTOR_SCALE, this.y + this.vy * VELOCITY_VECTOR_SCALE);
                    ctx.strokeStyle = 'rgba(255, 255, 0, 0.7)';
                    ctx.lineWidth = 2;
                    ctx.stroke();
                }

                // Draw Saturn's rings
                if (this.name === 'Saturn') {
                    ctx.beginPath();
                    ctx.strokeStyle = 'rgba(240, 230, 140, 0.7)';
                    ctx.lineWidth = this.radius * 0.4;
                    ctx.arc(this.x, this.y, this.radius * 1.8, 0, Math.PI * 2);
                    ctx.stroke();
                }
            }
        }
        
        function hexToRgb(hex) {
            let r = 0, g = 0, b = 0;
            if (hex.length == 7) {
                r = "0x" + hex[1] + hex[2];
                g = "0x" + hex[3] + hex[4];
                b = "0x" + hex[5] + hex[6];
            }
            return `${+r},${+g},${+b}`;
        }

        // --- Simulation Logic ---
        function initializeBodies() {
            bodies = [];
            const centerX = canvas.width / 2;
            const centerY = canvas.height / 2;
            const maxDist = Math.min(centerX, centerY) * 0.95;

            const sun = { name: 'Sun', mass: 20000, color: '#FFD700' };
            const planets = [
                { name: 'Mercury', distScale: 0.15, mass: 10, color: '#A9A9A9' },
                { name: 'Venus', distScale: 0.25, mass: 80, color: '#FFA500' },
                { name: 'Earth', distScale: 0.35, mass: 100, color: '#4682B4' },
                { name: 'Mars', distScale: 0.5, mass: 20, color: '#FF6347' },
                { name: 'Jupiter', distScale: 0.7, mass: 2000, color: '#D2B48C' },
                { name: 'Saturn', distScale: 0.85, mass: 1000, color: '#F0E68C' },
                { name: 'Uranus', distScale: 0.95, mass: 500, color: '#ADD8E6' },
                { name: 'Neptune', distScale: 1.0, mass: 600, color: '#4169E1' }
            ];

            const sunBody = new Body(sun.name, centerX, centerY, 0, 0, sun.mass, sun.color);
            bodies.push(sunBody);

            planets.forEach((p, i) => {
                const dist = p.distScale * maxDist;
                // Use a fixed, staggered angle for deterministic starting positions
                const angle = (i / planets.length) * (2 * Math.PI);
                const x = centerX + dist * Math.cos(angle);
                const y = centerY + dist * Math.sin(angle);
                const orbitalVelocity = Math.sqrt(G * sunBody.mass / dist);
                const vx = orbitalVelocity * -Math.sin(angle);
                const vy = orbitalVelocity * Math.cos(angle);
                bodies.push(new Body(p.name, x, y, vx, vy, p.mass, p.color));
            });
        }

        function updateForces() {
            // This function is now a Sun-centered model, not a full N-body simulation.
            // Only the Sun's gravity acts on the planets.
            const sun = bodies[0];

            for (let i = 1; i < bodies.length; i++) {
                const planet = bodies[i];
                
                const dx = sun.x - planet.x;
                const dy = sun.y - planet.y;
                const distSq = dx * dx + dy * dy;
                
                // Softening factor to prevent instability at close distances
                const softening = 100; 
                const dist = Math.sqrt(distSq + softening);
                
                // Newton's Law of Universal Gravitation
                const force = (G * sun.mass * planet.mass) / (dist * dist);
                
                // Decompose force into x and y components
                const forceX = force * (dx / dist);
                const forceY = force * (dy / dist);
                
                // Update planet's velocity based on this force
                planet.vx += (forceX / planet.mass) * TIME_STEP;
                planet.vy += (forceY / planet.mass) * TIME_STEP;
            }
            // The Sun's velocity is not updated, so it remains stationary.
        }

        function updateUI() {
            const earth = bodies.find(b => b.name === 'Earth');
            if (!earth) return;

            speedsList.innerHTML = ''; // Clear previous list

            bodies.forEach(body => {
                if (body.name === 'Sun' || body.name === 'Earth') return;
                
                const dvx = body.vx - earth.vx;
                const dvy = body.vy - earth.vy;
                const relSpeed = Math.sqrt(dvx*dvx + dvy*dvy);

                const li = document.createElement('li');
                li.innerHTML = `<span class="planet-name">${body.name}</span> <span class="speed-value">${(relSpeed * 10).toFixed(1)} km/s</span>`;
                speedsList.appendChild(li);
            });
        }

        // --- Animation Loop ---
        function animate() {
            updateForces();
            bodies.forEach(body => body.updatePosition());
            
            ctx.shadowBlur = 0;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            bodies.forEach(body => body.draw());
            
            updateUI(); // Update the relative speeds panel
            
            animationFrameId = requestAnimationFrame(animate);
        }
        
        function startSimulation() {
            if (animationFrameId) {
                cancelAnimationFrame(animationFrameId);
            }
            resizeCanvas();
            initializeBodies();
            animate();
        }

        // --- Event Listeners ---
        window.addEventListener('resize', () => {
            clearTimeout(window.resizedFinished);
            window.resizedFinished = setTimeout(startSimulation, 250);
        });
        resetButton.addEventListener('click', startSimulation);

        // --- Initial Start ---
        startSimulation();
    </script>
</body>
</html>
