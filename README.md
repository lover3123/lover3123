<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Profile Terminal</title>
    <!-- 
    Chosen Palette: Hacker Terminal (Dark BG/Neon Green)
    Application Structure Plan: A single-page app with tabbed navigation (Home, Skills, Projects, Stats). This structure logically organizes the static profile information from the Markdown report into interactive sections, making it easier to explore than a single long file. The 'Home' tab acts as a landing page with the bio, 'Skills' isolates badges and technologies, 'Projects' details active work, and 'Stats' provides a dynamic visualization of the user's activity (mocked with Chart.js). This is more usable as it groups related content.
    Visualization & Content Choices: Report Info: Bio/Skills -> Goal: Inform -> Presentation: HTML text/list -> Interaction: Clickable tabs to reveal. Report Info: Badges -> Goal: Inform -> Presentation: Image placeholders. Report Info: 'Stats Graph' placeholder -> Goal: Visualize Change/Activity -> Viz: Bar Chart (Chart.js/Canvas) -> Interaction: Chart.js tooltips -> Justification: Replaces a static placeholder with a live, interactive chart, fulfilling the implied need for data visualization.
    CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. 
    -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@400;700&display=swap');
        
        body {
            font-family: 'Roboto Mono', monospace;
            background-color: #0a0a0a;
            color: #00ff00;
        }

        .terminal-border {
            border: 2px solid #008800;
            box-shadow: 0 0 15px #00ff0030;
        }

        .terminal-header {
            color: #00ff00;
            text-shadow: 0 0 10px #00ff00;
            font-size: 1.5em;
            line-height: 1.1;
            padding: 10px;
            border: 1px dashed #00ff0044;
            background-color: #0a0a0a;
        }

        .nav-tab {
            @apply px-4 py-2 font-bold uppercase tracking-wider transition-all duration-200 cursor-pointer border-b-2 border-transparent;
        }

        .nav-tab:hover {
            background-color: #00ff0015;
            color: #fff;
        }

        .nav-tab.active {
            color: #fff;
            border-color: #00ff00;
            background-color: #00ff0020;
        }

        .content-section {
            @apply p-6 bg-dark-bg border border-neon-green/20 rounded-md;
        }

        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
    </style>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'dark-bg': '#0a0a0a',
                        'terminal-bg': '#1a1a1a',
                        'neon-green': '#00ff00',
                        'neon-red': '#ff4d4d',
                        'neon-link': '#4CAF50',
                    },
                    fontFamily: {
                        mono: ['Roboto Mono', 'monospace'],
                    },
                }
            }
        }
    </script>
</head>
<body class="bg-dark-bg min-h-screen p-4 md:p-8">

    <div class="max-w-5xl mx-auto terminal-border rounded-lg">
        
        <header class="p-4 text-center">
            <pre class="terminal-header whitespace-pre-wrap">
   _.-'          '._
 .'  _..: .:'  '.._  '.
/  ,' _/ \ \_ ',  \
|  | (o)   (o) |  |    
|  \  ( .-. )  /  |    :: ANALYZING DIGITAL FINGERPRINT ::
\   '._'-'_.'  /    // ACCESS LEVEL: ROOT
 '-._  `"`  _.-'    &gt; IDENTITY: A N O N
     `'-.,.-'`
            </pre>
        </header>

        <div class="bg-terminal-bg p-2 border-t-2 border-b-2 border-neon-green/50">
            <nav id="main-nav" class="flex flex-wrap justify-center space-x-2 md:space-x-4">
                <button data-target="home" class="nav-tab active">Home</button>
                <button data-target="skills" class="nav-tab">Skills_&_Badges</button>
                <button data-target="projects" class="nav-tab">Projects</button>
                <button data-target="stats" class="nav-tab">Stats</button>
            </nav>
        </div>

        <main class="p-4 md:p-8">
            
            <section id="home" class="app-section space-y-8">
                <div class="intro-text">
                    <p class="text-lg">Welcome to the interactive terminal. This application provides a dynamic view of the data extracted from the static profile manifest. Use the navigation tabs to explore different data sections.</p>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="border-l-4 border-neon-link pl-4">
                        <p class="text-sm text-neon-green/80">EXECUTE COMMAND</p>
                        <p class="text-xl">
                            <a href="https://github.com/your-username" target="_blank" class="text-neon-link hover:text-white transition-colors duration-200">$ cd /user/your-username</a>
                        </p>
                    </div>
                    
                    <div class="border-l-4 border-neon-red pl-4">
                        <p class="text-sm text-neon-red">ACTIVE PROJECT</p>
                        <h3 class="text-xl text-white">Project: Firefly (In Dev)</h3>
                    </div>
                </div>

                <div>
                    <h2 class="text-2xl text-neon-green mb-3">[LOG_ENTRY: BIO]</h2>
                    <div class="bg-terminal-bg p-4 border border-neon-green/30 rounded-md">
                        <p class="text-white leading-relaxed">
                            &gt; **LOG:** Connection established. I specialize in computational architecture, focusing on **TypeScript, Rust, and Go**. This profile serves as a manifest of open-source contributions to low-latency network applications and cryptographic libraries. Objective: optimize, secure, and deploy.
                        </p>
                    </div>
                </div>
            </section>

            <section id="skills" class="app-section space-y-8 hidden">
                <h2 class="text-2xl text-neon-green mb-3">[SYSTEM_BADGES]</h2>
                <div class="intro-text">
                    <p class="text-lg">This section displays validated credentials and primary skillsets identified from the profile manifest. Badges are visual representations of status and achievements.</p>
                </div>
                <div class="flex flex-wrap justify-center gap-4 p-4 bg-terminal-bg rounded-md border border-neon-green/30">
                    <img src="https://placehold.co/120x30/1a1a1a/00ff00?text=STATUS" alt="System Status Badge" class="rounded"/>
                    <img src="https://placehold.co/120x30/1a1a1a/00ff00?text=SKILL_1" alt="Skill Badge 1" class="rounded"/>
                    <img src="https://placehold.co/120x30/1a1a1a/00ff00?text=SKILL_2" alt="Skill Badge 2" class="rounded"/>
                    <img src="https://placehold.co/120x30/1a1a1a/00ff00?text=STATS" alt="GitHub Stats Badge" class="rounded"/>
                </div>

                <h2 class="text-2xl text-neon-green mb-3">[PRIMARY_SKILLSET]</h2>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <div class="p-4 bg-terminal-bg text-center rounded-md border border-neon-green/30 hover:bg-neon-green/10 transition-colors">
                        <h3 class="text-2xl text-white font-bold">TypeScript</h3>
                        <p class="text-neon-green/80">Web & App Architecture</p>
                    </div>
                    <div class="p-4 bg-terminal-bg text-center rounded-md border border-neon-green/30 hover:bg-neon-green/10 transition-colors">
                        <h3 class="text-2xl text-white font-bold">Rust</h3>
                        <p class="text-neon-green/80">Systems & Performance</p>
                    </div>
                    <div class="p-4 bg-terminal-bg text-center rounded-md border border-neon-green/30 hover:bg-neon-green/10 transition-colors">
                        <h3 class="text-2xl text-white font-bold">Go</h3>
                        <p class="text-neon-green/80">Network & Infra</p>
                    </div>
                </div>
            </section>

            <section id="projects" class="app-section space-y-8 hidden">
                <h2 class="text-2xl text-neon-green mb-3">[ACTIVE_PROJECTS_MANIFEST]</h2>
                <div class="intro-text">
                    <p class="text-lg">Current development focus. This section lists and describes projects currently in active development or under primary maintenance.</p>
                </div>
                <div class="bg-terminal-bg p-6 rounded-md border border-neon-green/30">
                    <h3 class="text-3xl text-white mb-2">Project: Firefly</h3>
                    <p class="text-neon-red text-lg mb-4">(In Development)</p>
                    <p class="text-white/90 leading-relaxed">
                        A placeholder description for "Project: Firefly". This initiative focuses on developing a lightweight, high-performance distributed logging system using Rust for the core agent and Go for the aggregation API. The goal is to achieve sub-millisecond log ingestion over low-bandwidth networks.
                    </p>
                    <div class="mt-4 flex space-x-2">
                        <span class="text-xs font-bold bg-neon-green text-dark-bg px-2 py-1 rounded">Rust</span>
                        <span class="text-xs font-bold bg-neon-green text-dark-bg px-2 py-1 rounded">Go</span>
                        <span class="text-xs font-bold bg-neon-green text-dark-bg px-2 py-1 rounded">Networking</span>
                    </div>
                </div>
            </section>
            
            <section id="stats" class="app-section space-y-8 hidden">
                <h2 class="text-2xl text-neon-green mb-3">[CODE_ACTIVITY_REPORT.log]</h2>
                <div class="intro-text">
                    <p class="text-lg">This visualization provides a dynamic representation of code activity and language focus, replacing the static graph placeholder from the manifest. This chart is generated client-side using Chart.js.</p>
                </div>
                <div class="bg-terminal-bg p-4 rounded-md border border-neon-green/30">
                    <div class="chart-container">
                        <canvas id="statsChart"></canvas>
                    </div>
                </div>
            </section>

        </main>
        
        <footer class="p-4 border-t-2 border-neon-green/50 text-center text-neon-green/70 text-sm">
            <p>&gt; Connection secure. Session active. | STATUS: OK</p>
        </footer>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const sections = document.querySelectorAll('.app-section');
            const navButtons = document.querySelectorAll('.nav-tab');

            function showSection(targetId) {
                sections.forEach(section => {
                    if (section.id === targetId) {
                        section.classList.remove('hidden');
                    } else {
                        section.classList.add('hidden');
                    }
                });
                
                navButtons.forEach(button => {
                    if (button.dataset.target === targetId) {
                        button.classList.add('active');
                    } else {
                        button.classList.remove('active');
                    }
                });
            }

            navButtons.forEach(button => {
                button.addEventListener('click', () => {
                    showSection(button.dataset.target);
                });
            });

            function createStatsChart() {
                const ctx = document.getElementById('statsChart');
                if (!ctx) return;

                Chart.defaults.font.family = "'Roboto Mono', monospace";
                Chart.defaults.color = '#00ff00';

                new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: ['TypeScript', 'Rust', 'Go', 'Shell', 'Python', 'Markdown'],
                        datasets: [{
                            label: 'Focus (Commits)',
                            data: [45, 30, 25, 15, 10, 5],
                            backgroundColor: '#00ff0020',
                            borderColor: '#00ff00',
                            borderWidth: 2,
                            hoverBackgroundColor: '#00ff0040',
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                backgroundColor: '#0a0a0a',
                                borderColor: '#00ff00',
                                borderWidth: 1,
                                titleFont: { weight: 'bold' },
                                bodyFont: { weight: 'normal' },
                                displayColors: false,
                            }
                        },
                        scales: {
                            y: {
                                beginAtZero: true,
                                grid: {
                                    color: '#00ff0030',
                                    borderColor: '#00ff00'
                                },
                                ticks: {
                                    color: '#00ff00',
                                }
                            },
                            x: {
                                grid: {
                                    display: false
                                },
                                ticks: {
                                    color: '#00ff00',
                                }
                            }
                        }
                    }
                });
            }

            showSection('home');
            createStatsChart();
        });
    </script>

</body>
</html>
