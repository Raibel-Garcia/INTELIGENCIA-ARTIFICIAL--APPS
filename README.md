<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Social Engagement Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 1.5rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }

        .tab-active {
            border-bottom: 3px solid #764ba2;
            color: #764ba2;
        }

        .input-group focus-within label {
            color: #764ba2;
        }

        .result-animate {
            animation: slideUp 0.5s ease-out;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="p-4 md:p-8 flex items-center justify-center">

    <div class="glass-card w-full max-w-2xl p-6 md:p-10">
        <header class="text-center mb-8">
            <h1 class="text-3xl font-700 text-gray-800 mb-2">?? Calculadora de Engagement</h1>
            <p class="text-gray-500">Mide el impacto de tu contenido en redes sociales</p>
        </header>

        <!-- Tabs -->
        <div class="flex border-b border-gray-200 mb-8">
            <button onclick="switchTab('post')" id="tab-post" class="flex-1 py-3 font-semibold text-gray-400 transition-all tab-active">Individual Post</button>
            <button onclick="switchTab('profile')" id="tab-profile" class="flex-1 py-3 font-semibold text-gray-400 transition-all">Perfil Completo</button>
        </div>

        <!-- Form Section -->
        <div id="calculator-form" class="space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="input-group">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Me gusta (Likes)</label>
                    <input type="number" id="likes" placeholder="0" class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:ring-2 focus:ring-purple-500 focus:border-transparent outline-none transition-all">
                </div>
                <div class="input-group">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Comentarios</label>
                    <input type="number" id="comments" placeholder="0" class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:ring-2 focus:ring-purple-500 focus:border-transparent outline-none transition-all">
                </div>
                <div class="input-group">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Guardados / Compartidos</label>
                    <input type="number" id="others" placeholder="0" class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:ring-2 focus:ring-purple-500 focus:border-transparent outline-none transition-all">
                </div>
                <div class="input-group">
                    <label id="reach-label" class="block text-sm font-medium text-gray-700 mb-1">Alcance o Seguidores</label>
                    <input type="number" id="reach" placeholder="0" class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:ring-2 focus:ring-purple-500 focus:border-transparent outline-none transition-all">
                </div>
            </div>

            <button onclick="calculateEngagement()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 rounded-xl shadow-lg transform transition hover:-translate-y-1 active:scale-95">
                Calcular Tasa de Engagement
            </button>
        </div>

        <!-- Results Section -->
        <div id="result-container" class="hidden mt-10 p-6 rounded-2xl bg-gray-50 border border-gray-100 result-animate">
            <div class="text-center">
                <span class="text-gray-500 uppercase tracking-widest text-xs font-bold">Tu Resultado</span>
                <div id="engagement-value" class="text-5xl font-extrabold text-indigo-600 my-2">0%</div>
                <div id="engagement-level" class="inline-block px-4 py-1 rounded-full text-sm font-semibold mt-2"></div>
                <p id="engagement-desc" class="text-gray-600 mt-4 text-sm"></p>
            </div>
        </div>

        <footer class="mt-8 text-center text-xs text-gray-400">
            * El engagement se calcula sumando interacciones y dividiéndolas por el alcance/seguidores.
        </footer>
    </div>

    <script>
        let currentMode = 'post';

        function switchTab(mode) {
            currentMode = mode;
            const tabPost = document.getElementById('tab-post');
            const tabProfile = document.getElementById('tab-profile');
            const reachLabel = document.getElementById('reach-label');
            const resultContainer = document.getElementById('result-container');

            resultContainer.classList.add('hidden');

            if (mode === 'post') {
                tabPost.classList.add('tab-active');
                tabProfile.classList.remove('tab-active');
                reachLabel.innerText = 'Alcance del Post';
            } else {
                tabProfile.classList.add('tab-active');
                tabPost.classList.remove('tab-active');
                reachLabel.innerText = 'Número de Seguidores';
            }
        }

        function calculateEngagement() {
            const likes = parseFloat(document.getElementById('likes').value) || 0;
            const comments = parseFloat(document.getElementById('comments').value) || 0;
            const others = parseFloat(document.getElementById('others').value) || 0;
            const reach = parseFloat(document.getElementById('reach').value) || 0;

            if (reach <= 0) {
                alert("Por favor, ingresa un valor válido para el alcance o seguidores.");
                return;
            }

            const totalInteractions = likes + comments + others;
            const engagementRate = (totalInteractions / reach) * 100;

            displayResults(engagementRate);
        }

        function displayResults(rate) {
            const container = document.getElementById('result-container');
            const valueDisplay = document.getElementById('engagement-value');
            const levelDisplay = document.getElementById('engagement-level');
            const descDisplay = document.getElementById('engagement-desc');

            container.classList.remove('hidden');
            valueDisplay.innerText = rate.toFixed(2) + '%';

            let level = "";
            let colorClass = "";
            let description = "";

            if (rate < 1) {
                level = "Bajo";
                colorClass = "bg-red-100 text-red-600";
                description = "Tu contenido no está llegando o interesando lo suficiente. Prueba nuevos formatos.";
            } else if (rate >= 1 && rate < 3.5) {
                level = "Promedio";
                colorClass = "bg-blue-100 text-blue-600";
                description = "Estás en el estándar de la industria. Sigue interactuando con tu audiencia.";
            } else if (rate >= 3.5 && rate < 6) {
                level = "Alto";
                colorClass = "bg-green-100 text-green-600";
                description = "¡Excelente! Tu comunidad es muy activa y valora mucho lo que compartes.";
            } else {
                level = "Viral / Muy Alto";
                colorClass = "bg-purple-100 text-purple-600 font-bold";
                description = "¡Impresionante! Tienes una conexión excepcional con tu audiencia.";
            }

            levelDisplay.innerText = level;
            levelDisplay.className = `inline-block px-4 py-1 rounded-full text-sm font-semibold mt-2 ${colorClass}`;
            descDisplay.innerText = description;
        }
    </script>
</body>
</html>
