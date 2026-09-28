# Trivia-secundaria
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AprendeQuiz - Trivia Interactiva de Secundaria</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Canvas Confetti Library -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&family=Nunito:wght@400;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        fredoka: ['Fredoka', 'sans-serif'],
                        sans: ['Nunito', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            purple: '#6C5CE7',
                            pink: '#FD79A8',
                            blue: '#0984E3',
                            green: '#00B894',
                            yellow: '#FDCB6E',
                            orange: '#E17055',
                            dark: '#2D3436'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Nunito', sans-serif;
            background: linear-gradient(135deg, #a8c0ff 0%, #3f2b96 100%);
            min-height: 100vh;
        }
        .btn-bounce {
            transition: transform 0.15s cubic-bezier(0.175, 0.885, 0.32, 1.275), box-shadow 0.15s ease;
        }
        .btn-bounce:hover {
            transform: translateY(-4px) scale(1.02);
        }
        .btn-bounce:active {
            transform: translateY(1px) scale(0.98);
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.93);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-8px); }
            40%, 80% { transform: translateX(8px); }
        }
        .animate-shake {
            animation: shake 0.4s ease-in-out;
        }
        @keyframes pop {
            0% { transform: scale(0.9); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop {
            animation: pop 0.3s ease-out forwards;
        }
    </style>
</head>
<body class="text-gray-800 min-h-screen flex flex-col justify-between p-3 md:p-6">

    <!-- Header / Navbar -->
    <header class="max-w-4xl mx-auto w-full flex items-center justify-between glass-card p-4 rounded-2xl shadow-lg mb-4">
        <div class="flex items-center space-x-3 cursor-pointer" onclick="goHome()">
            <div class="bg-gradient-to-tr from-brand-purple to-brand-pink p-3 rounded-2xl text-white shadow-md">
                <i class="fa-solid fa-graduation-cap text-2xl"></i>
            </div>
            <div>
                <h1 class="font-fredoka text-2xl md:text-3xl font-bold text-gray-800 tracking-wide leading-none">Aprende<span class="text-brand-purple">Quiz</span></h1>
                <p class="text-xs text-gray-500 font-semibold">1º y 2º de Secundaria</p>
            </div>
        </div>
        
        <div id="header-stats" class="hidden flex items-center space-x-3">
            <div class="flex items-center space-x-1 bg-amber-100 text-amber-700 px-3 py-1.5 rounded-full font-bold text-sm shadow-inner">
                <i class="fa-solid fa-fire text-amber-500 animate-pulse"></i>
                <span id="streak-counter">0</span>
            </div>
            <div class="flex items-center space-x-1 bg-purple-100 text-brand-purple px-3 py-1.5 rounded-full font-bold text-sm shadow-inner">
                <i class="fa-solid fa-star text-yellow-500"></i>
                <span id="score-counter">0</span>
            </div>
        </div>
    </header>

    <!-- Contenedor Principal de Modulos -->
    <main class="max-w-4xl mx-auto w-full flex-grow flex flex-col justify-center my-2">

        <!-- 1. PANTALLA DE INICIO / SELECCIÓN -->
        <section id="screen-setup" class="glass-card rounded-3xl p-6 md:p-8 shadow-2xl animate-pop">
            <div class="text-center mb-8">
                <span class="bg-purple-100 text-brand-purple text-xs md:text-sm font-bold px-4 py-1.5 rounded-full uppercase tracking-wider">¡Desafía tus conocimientos!</span>
                <h2 class="font-fredoka text-3xl md:text-4xl font-bold text-gray-800 mt-3">¿Qué quieres aprender hoy?</h2>
                <p class="text-gray-600 text-sm md:text-base mt-1">Selecciona tu grado, materia y modo de juego para comenzar.</p>
            </div>

            <!-- Paso 1: Selección de Grado -->
            <div class="mb-6">
                <label class="block text-gray-700 font-bold mb-3 text-sm md:text-base flex items-center">
                    <span class="w-6 h-6 rounded-full bg-brand-purple text-white text-xs flex items-center justify-center mr-2">1</span>
                    Selecciona tu Grado Escolar:
                </label>
                <div class="grid grid-cols-2 gap-4">
                    <button type="button" onclick="selectGrade(1)" id="btn-grade-1" class="btn-bounce grade-option p-4 rounded-2xl border-2 border-purple-200 bg-white hover:border-brand-purple text-center focus:outline-none shadow-sm flex flex-col items-center">
                        <i class="fa-solid fa-1 text-2xl text-brand-purple mb-1"></i>
                        <span class="font-bold text-gray-800 text-base">1er Grado</span>
                        <span class="text-xs text-gray-500">12 - 13 años</span>
                    </button>
                    <button type="button" onclick="selectGrade(2)" id="btn-grade-2" class="btn-bounce grade-option p-4 rounded-2xl border-2 border-purple-200 bg-white hover:border-brand-purple text-center focus:outline-none shadow-sm flex flex-col items-center">
                        <i class="fa-solid fa-2 text-2xl text-brand-purple mb-1"></i>
                        <span class="font-bold text-gray-800 text-base">2do Grado</span>
                        <span class="text-xs text-gray-500">13 - 14 años</span>
                    </button>
                </div>
            </div>

            <!-- Paso 2: Selección de Materia -->
            <div class="mb-6">
                <label class="block text-gray-700 font-bold mb-3 text-sm md:text-base flex items-center">
                    <span class="w-6 h-6 rounded-full bg-brand-purple text-white text-xs flex items-center justify-center mr-2">2</span>
                    Selecciona la Materia:
                </label>
                <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                    <button type="button" onclick="selectSubject('math')" id="btn-sub-math" class="btn-bounce subject-option p-3 rounded-2xl border-2 border-gray-100 bg-white hover:border-blue-400 flex flex-col items-center text-center">
                        <div class="w-10 h-10 rounded-xl bg-blue-100 text-brand-blue flex items-center justify-center text-xl mb-2">
                            <i class="fa-solid fa-calculator"></i>
                        </div>
                        <span class="font-bold text-xs md:text-sm text-gray-700">Matemáticas</span>
                    </button>

                    <button type="button" onclick="selectSubject('spanish')" id="btn-sub-spanish" class="btn-bounce subject-option p-3 rounded-2xl border-2 border-gray-100 bg-white hover:border-pink-400 flex flex-col items-center text-center">
                        <div class="w-10 h-10 rounded-xl bg-pink-100 text-brand-pink flex items-center justify-center text-xl mb-2">
                            <i class="fa-solid fa-book-open"></i>
                        </div>
                        <span class="font-bold text-xs md:text-sm text-gray-700">Español</span>
                    </button>

                    <button type="button" onclick="selectSubject('science')" id="btn-sub-science" class="btn-bounce subject-option p-3 rounded-2xl border-2 border-gray-100 bg-white hover:border-green-400 flex flex-col items-center text-center">
                        <div class="w-10 h-10 rounded-xl bg-emerald-100 text-brand-green flex items-center justify-center text-xl mb-2">
                            <i class="fa-solid fa-flask"></i>
                        </div>
                        <span class="font-bold text-xs md:text-sm text-gray-700">Ciencias</span>
                    </button>

                    <button type="button" onclick="selectSubject('history')" id="btn-sub-history" class="btn-bounce subject-option p-3 rounded-2xl border-2 border-gray-100 bg-white hover:border-orange-400 flex flex-col items-center text-center">
                        <div class="w-10 h-10 rounded-xl bg-orange-100 text-brand-orange flex items-center justify-center text-xl mb-2">
                            <i class="fa-solid fa-earth-americas"></i>
                        </div>
                        <span class="font-bold text-xs md:text-sm text-gray-700">Historia y Geografía</span>
                    </button>
                </div>
            </div>

            <!-- Paso 3: Modo de Juego -->
            <div class="mb-8">
                <label class="block text-gray-700 font-bold mb-3 text-sm md:text-base flex items-center">
                    <span class="w-6 h-6 rounded-full bg-brand-purple text-white text-xs flex items-center justify-center mr-2">3</span>
                    Modo de Juego:
                </label>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <button type="button" onclick="selectMode('practice')" id="btn-mode-practice" class="btn-bounce mode-option p-4 rounded-2xl border-2 border-gray-100 bg-white hover:border-purple-400 flex items-center space-x-3 text-left">
                        <div class="w-12 h-12 rounded-xl bg-purple-100 text-brand-purple flex items-center justify-center text-xl shrink-0">
                            <i class="fa-solid fa-brain"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-gray-800 text-sm">Modo Práctica</h4>
                            <p class="text-xs text-gray-500">A tu ritmo con explicaciones inmediatas tras responder.</p>
                        </div>
                    </button>

                    <button type="button" onclick="selectMode('timed')" id="btn-mode-timed" class="btn-bounce mode-option p-4 rounded-2xl border-2 border-gray-100 bg-white hover:border-purple-400 flex items-center space-x-3 text-left">
                        <div class="w-12 h-12 rounded-xl bg-amber-100 text-amber-600 flex items-center justify-center text-xl shrink-0">
                            <i class="fa-solid fa-stopwatch"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-gray-800 text-sm">Contra Reloj (60s)</h4>
                            <p class="text-xs text-gray-500">¡Responde la mayor cantidad de preguntas en 1 minuto!</p>
                        </div>
                    </button>
                </div>
            </div>

            <!-- Botón de Inicio -->
            <button id="btn-start-game" onclick="startGame()" disabled class="w-full bg-gray-300 text-gray-500 font-fredoka text-xl py-4 rounded-2xl shadow-lg font-bold tracking-wide transition duration-200 cursor-not-allowed">
                ¡Selecciona las opciones para jugar!
            </button>
        </section>

        <!-- 2. PANTALLA DEL QUIZ / PREGUNTAS -->
        <section id="screen-quiz" class="hidden glass-card rounded-3xl p-5 md:p-8 shadow-2xl relative overflow-hidden animate-pop">
            <!-- Barra superior del Quiz -->
            <div class="flex items-center justify-between mb-4 pb-3 border-b border-gray-100">
                <div class="flex items-center space-x-2">
                    <span id="badge-grade" class="bg-purple-100 text-brand-purple font-bold text-xs px-3 py-1 rounded-full">1º Sec</span>
                    <span id="badge-subject" class="bg-blue-100 text-brand-blue font-bold text-xs px-3 py-1 rounded-full">Matemáticas</span>
                </div>
                
                <!-- Indicador de Tiempo (Modo Reloj) o Contador de Preguntas -->
                <div id="quiz-progress-badge" class="flex items-center font-bold text-sm text-gray-600">
                    <i class="fa-regular fa-compass mr-1 text-brand-purple"></i>
                    <span id="question-tracker">Pregunta 1 de 5</span>
                </div>
                
                <div id="timer-badge" class="hidden flex items-center bg-red-100 text-red-600 px-3 py-1 rounded-full font-bold text-sm">
                    <i class="fa-solid fa-clock mr-1.5 animate-bounce"></i>
                    <span id="timer-count">60s</span>
                </div>
            </div>

            <!-- Barra de Progreso Visual -->
            <div id="progress-bar-container" class="w-full bg-gray-200 rounded-full h-2.5 mb-6 overflow-hidden">
                <div id="progress-bar-fill" class="bg-gradient-to-r from-brand-purple to-brand-pink h-2.5 rounded-full transition-all duration-300" style="width: 0%"></div>
            </div>

            <!-- Pregunta -->
            <div class="mb-6">
                <h3 id="question-text" class="font-fredoka text-xl md:text-2xl font-bold text-gray-800 leading-snug">
                    ¿Cargando pregunta...
                </h3>
            </div>

            <!-- Opciones de Respuesta -->
            <div id="options-container" class="grid grid-cols-1 gap-3 mb-6">
                <!-- Se generan dinámicamente -->
            </div>

            <!-- Retroalimentación / Explicación (Modo Práctica) -->
            <div id="explanation-box" class="hidden p-4 rounded-2xl mb-6 text-sm transition-all animate-pop">
                <div class="flex items-start space-x-3">
                    <div id="explanation-icon" class="text-xl mt-0.5"></div>
                    <div>
                        <h5 id="explanation-title" class="font-bold text-base mb-1"></h5>
                        <p id="explanation-text" class="text-gray-700 leading-relaxed"></p>
                    </div>
                </div>
            </div>

            <!-- Botón Siguiente -->
            <div class="flex justify-end">
                <button id="btn-next-question" onclick="nextQuestion()" class="hidden bg-brand-purple hover:bg-purple-700 text-white font-bold py-3 px-8 rounded-xl shadow-md btn-bounce items-center space-x-2">
                    <span>Siguiente</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </section>

        <!-- 3. PANTALLA DE RESULTADOS -->
        <section id="screen-results" class="hidden glass-card rounded-3xl p-6 md:p-8 shadow-2xl text-center animate-pop">
            <div class="w-20 h-20 bg-yellow-100 text-yellow-500 rounded-full flex items-center justify-center mx-auto text-4xl mb-4 shadow-inner">
                <i id="result-trophy-icon" class="fa-solid fa-trophy"></i>
            </div>

            <h2 id="result-title" class="font-fredoka text-3xl md:text-4xl font-bold text-gray-800">¡Buen trabajo!</h2>
            <p id="result-subtitle" class="text-gray-600 mt-1 mb-6">Has completado el cuestionario con éxito.</p>

            <!-- Card de Calificación -->
            <div class="bg-gradient-to-r from-purple-50 to-pink-50 rounded-2xl p-6 mb-6 max-w-md mx-auto border border-purple-100">
                <div class="text-xs uppercase font-bold tracking-wider text-gray-400 mb-1">Puntuación Final</div>
                <div id="result-score" class="font-fredoka text-5xl font-extrabold text-brand-purple">0 pts</div>
                
                <div class="grid grid-cols-3 gap-2 mt-6 pt-4 border-t border-purple-100/60">
                    <div>
                        <span class="block text-xs text-gray-500">Correctas</span>
                        <span id="result-correct" class="font-bold text-lg text-emerald-600">0</span>
                    </div>
                    <div>
                        <span class="block text-xs text-gray-500">Incorrectas</span>
                        <span id="result-incorrect" class="font-bold text-lg text-rose-500">0</span>
                    </div>
                    <div>
                        <span class="block text-xs text-gray-500">Precisión</span>
                        <span id="result-accuracy" class="font-bold text-lg text-brand-blue">0%</span>
                    </div>
                </div>
            </div>

            <!-- Desglose / Repaso de Preguntas -->
            <div class="text-left mb-6">
                <h4 class="font-bold text-gray-700 text-base mb-3 flex items-center">
                    <i class="fa-solid fa-list-check text-brand-purple mr-2"></i> Desglose de respuestas:
                </h4>
                <div id="review-list" class="space-y-3 max-h-60 overflow-y-auto pr-1">
                    <!-- Respuestas renderizadas por JS -->
                </div>
            </div>

            <!-- Acciones -->
            <div class="flex flex-col sm:flex-row gap-3 justify-center">
                <button onclick="restartQuiz()" class="btn-bounce bg-brand-purple hover:bg-purple-700 text-white font-bold py-3.5 px-6 rounded-2xl shadow-lg flex items-center justify-center space-x-2">
                    <i class="fa-solid fa-rotate-right"></i>
                    <span>Volver a Intentar</span>
                </button>
                <button onclick="goHome()" class="btn-bounce bg-white border border-gray-200 text-gray-700 hover:bg-gray-50 font-bold py-3.5 px-6 rounded-2xl shadow-sm flex items-center justify-center space-x-2">
                    <i class="fa-solid fa-house"></i>
                    <span>Elegir Otra Materia</span>
                </button>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="max-w-4xl mx-auto w-full text-center py-3 text-xs text-white/80 font-semibold">
        AprendeQuiz &copy; Educativo - Diseñado para 1º y 2º de Educación Secundaria
    </footer>

    <!-- Lógica JavaScript -->
    <script>
        // BANCO DE PREGUNTAS DETALLADO
        const questionBank = {
            // ================= 1ER GRADO =================
            1: {
                math: [
                    {
                        question: "¿Cuál es el resultado de resolver la siguiente operación de números enteros? (-8) + (+12) - (-3)",
                        options: ["+7", "+1", "+17", "-1"],
                        answer: 0,
                        explanation: "Siguiendo la regla de signos: -8 + 12 = 4. Luego, restar un negativo equivale a sumar: 4 - (-3) = 4 + 3 = 7."
                    },
                    {
                        question: "¿Cuál es el valor equivalente en fracción de la cifra decimal periódica o finita 0.75?",
                        options: ["1/2", "3/4", "2/3", "4/5"],
                        answer: 1,
                        explanation: "0.75 se lee como 75 centésimas (75/100). Al simplificar dividiendo entre 25, obtenemos 3/4."
                    },
                    {
                        question: "Si el 20% de los 35 alumnos de un grupo de 1º de secundaria obtuvo excelencia, ¿cuántos alumnos son?",
                        options: ["5 alumnos", "7 alumnos", "10 alumnos", "12 alumnos"],
                        answer: 1,
                        explanation: "Para calcular el 20% de 35, multiplicas 35 por 0.20 (o 35 × 20 / 100), lo que da exactamente 7 alumnos."
                    },
                    {
                        question: "¿Cuál es el perímetro de un terreno rectangular que mide 12 metros de largo y 8 metros de ancho?",
                        options: ["96 metros", "20 metros", "40 metros", "48 metros"],
                        answer: 2,
                        explanation: "El perímetro es la suma de todos sus lados: P = 2(largo) + 2(ancho) = 2(12) + 2(8) = 24 + 16 = 40 metros."
                    },
                    {
                        question: "Resuelve la ecuación lineal simple: 2x + 5 = 15. ¿Cuánto vale 'x'?",
                        options: ["x = 5", "x = 10", "x = 4", "x = 6"],
                        answer: 0,
                        explanation: "Restamos 5 a ambos lados: 2x = 10. Luego dividimos entre 2: x = 10 / 2 = 5."
                    },
                    {
                        question: "¿Cómo se llama el triángulo que tiene sus tres lados con diferentes longitudes?",
                        options: ["Equilátero", "Isósceles", "Escaleno", "Rectángulo"],
                        answer: 2,
                        explanation: "El triángulo Escaleno se caracteriza por tener sus tres lados y tres ángulos de diferente medida."
                    }
                ],
                spanish: [
                    {
                        question: "¿Qué tipo de texto tiene como propósito principal relatar acontecimientos reales o ficticios sucedidos en un tiempo y lugar?",
                        options: ["Texto Narrativo", "Texto Expositivo", "Texto Argumentativo", "Texto Instructivo"],
                        answer: 0,
                        explanation: "El texto narrativo cuenta historias reales o ficticias en las que intervienen personajes."
                    },
                    {
                        question: "Identifica cuál de las siguientes palabras es aguda y lleva tilde por regla ortográfica:",
                        options: ["Árbol", "Canción", "Música", "Lápiz"],
                        answer: 1,
                        explanation: "'Canción' es una palabra aguda porque su sílaba tónica es la última y lleva tilde al terminar en la letra 'n'."
                    },
                    {
                        question: "En la frase 'El sol brillaba como un disco de oro', ¿qué figura literaria se está utilizando?",
                        options: ["Metáfora", "Símil o Comparación", "Personificación", "Hipérbole"],
                        answer: 1,
                        explanation: "Es un Símil porque compara dos elementos directamente usando la palabra nexo 'como'."
                    },
                    {
                        question: "¿Cuál de las siguientes opciones contiene un sinónimo adecuado para la palabra 'Abundante'?",
                        options: ["Escaso", "Copioso", "Pequeño", "Deficiente"],
                        answer: 1,
                        explanation: "'Copioso' significa abundante o numeroso."
                    },
                    {
                        question: "En un reglamento escolar, ¿en qué modo verbal suelen redactarse las normas (ej. 'Respetar', 'Entregar')?",
                        options: ["Subjuntivo", "Infinitivo", "Imperativo", "Indicativo"],
                        answer: 1,
                        explanation: "Los verbos en Infinitivo (terminados en -ar, -er, -ir) son muy comunes en reglamentos para indicar acciones generales."
                    }
                ],
                science: [
                    {
                        question: "¿Cuál es la unidad fundamental, estructural y funcional de todos los seres vivos?",
                        options: ["El tejido", "El órgano", "La célula", "La molécula"],
                        answer: 2,
                        explanation: "La célula es la unidad más pequeña y básica capaz de realizar todas las funciones vitales."
                    },
                    {
                        question: "¿Qué organelo celular contiene el material genético (ADN) y dirige las actividades celulares?",
                        options: ["Mitocondria", "Núcleo", "Ribosoma", "Membrana"],
                        answer: 1,
                        explanation: "El núcleo alberga el ADN y coordina las funciones de la célula, actuando como su centro de control."
                    },
                    {
                        question: "Proceso biológico mediante el cual las plantas verdes transforman la luz solar en alimento (glucosa):",
                        options: ["Respiración", "Fotosíntesis", "Fermentación", "Combustión"],
                        answer: 1,
                        explanation: "La fotosíntesis permite a las autótrofas (plantas) sintetizar materia orgánica a partir de luz, CO2 y agua."
                    },
                    {
                        question: "¿Qué término describe la gran variedad de especies de seres vivos presentes en el planeta?",
                        options: ["Ecosistema", "Biodiversidad", "Bioma", "Comunidad"],
                        answer: 1,
                        explanation: "Biodiversidad se refiere a la pluralidad y diversidad biológica de formas de vida en la Tierra."
                    },
                    {
                        question: "¿Cuál es el estado de la materia donde las partículas están muy unidas y mantienen forma y volumen constante?",
                        options: ["Líquido", "Gaseoso", "Sólido", "Plasma"],
                        answer: 2,
                        explanation: "En el estado sólido, las fuerzas de atracción entre partículas son fuertes, dándole forma y volumen fijos."
                    }
                ],
                history: [
                    {
                        question: "¿Cuál de las siguientes civilizaciones agrícolas de la antigüedad floreció entre los ríos Tigris y Éufrates?",
                        options: ["Egipto", "Mesopotamia", "China", "Grecia"],
                        answer: 1,
                        explanation: "Mesopotamia significa 'tierra entre ríos' y floreció justamente junto al Tigris y Éufrates."
                    },
                    {
                        question: "¿Cuál es el océano más grande de la Tierra?",
                        options: ["Océano Atlántico", "Océano Índico", "Océano Pacífico", "Océano Ártico"],
                        answer: 2,
                        explanation: "El Océano Pacífico cubre más de un tercio de la superficie terrestre y es el de mayor extensión."
                    },
                    {
                        question: "¿En qué continente se localiza la cordillera de los Andes?",
                        options: ["Europa", "América", "Asia", "África"],
                        answer: 1,
                        explanation: "Los Andes recorren la parte occidental de América del Sur."
                    },
                    {
                        question: "¿Qué evento histórico del siglo XVIII promovió ideas de libertad, igualdad y fraternidad en Europa?",
                        options: ["La Revolución Industrial", "La Revolución Francesa", "El Descubrimiento de América", "La Guerra Fría"],
                        answer: 1,
                        explanation: "La Revolución Francesa (1789) popularizó estos ideales democráticos e impactó al mundo entero."
                    },
                    {
                        question: "¿Cuál es la línea imaginaria que divide verticalmente a la Tierra en hemisferio Oriental y Occidental?",
                        options: ["Ecuador", "Trópico de Cáncer", "Meridiano de Greenwich", "Círculo Polar"],
                        answer: 2,
                        explanation: "El Meridiano de Greenwich (Meridiano 0°) divide al planeta en hemisferios Este y Oeste."
                    }
                ]
            },
            // ================= 2DO GRADO =================
            2: {
                math: [
                    {
                        question: "¿Cuál es el resultado de simplificar la expresión algebraica: 4x + 3y - 2x + 5y?",
                        options: ["2x + 8y", "6x + 8y", "2x + 2y", "8xy"],
                        answer: 0,
                        explanation: "Agrupamos términos semejantes: (4x - 2x) + (3y + 5y) = 2x + 8y."
                    },
                    {
                        question: "Según el Teorema de Pitágoras (a² + b² = c²), si los catetos de un triángulo rectángulo miden 3 cm y 4 cm, ¿cuánto mide la hipotenusa?",
                        options: ["5 cm", "7 cm", "25 cm", "6 cm"],
                        answer: 0,
                        explanation: "c² = 3² + 4² = 9 + 16 = 25. La raíz cuadrada de 25 es 5 cm."
                    },
                    {
                        question: "¿Cuál es la solución para la variable 'x' en la ecuación 3x - 9 = 12?",
                        options: ["x = 3", "x = 7", "x = 5", "x = 21"],
                        answer: 1,
                        explanation: "3x = 12 + 9 => 3x = 21 => x = 21 / 3 = 7."
                    },
                    {
                        question: "¿Cuál es el área de un círculo cuyo radio mide 5 cm? (Usa π ≈ 3.14)",
                        options: ["31.4 cm²", "78.5 cm²", "15.7 cm²", "50 cm²"],
                        answer: 1,
                        explanation: "Fórmula de área del círculo: A = π × r² = 3.14 × (5)² = 3.14 × 25 = 78.5 cm²."
                    },
                    {
                        question: "Si lanzas un dado regular de 6 caras, ¿cuál es la probabilidad de obtener un número par?",
                        options: ["1/6", "1/2 (50%)", "1/3", "2/3"],
                        answer: 1,
                        explanation: "Hay 3 números pares (2, 4, 6) de un total de 6 resultados posibles: 3/6 = 1/2 o 50%."
                    }
                ],
                spanish: [
                    {
                        question: "¿En qué consisten los textos argumentativos?",
                        options: [
                            "En describir personajes de forma detallada.",
                            "En defender una postura o tesis mediante razones y argumentos.",
                            "En enumerar instrucciones para armar un objeto.",
                            "En recopilar fechas históricas cronológicamente."
                        ],
                        answer: 1,
                        explanation: "El objetivo del texto argumentativo es persuadir o convencer al lector demostrando la validez de una tesis."
                    },
                    {
                        question: "¿Qué figura literaria consiste en exagerar una realidad de forma aumentada o disminuida? (Ej. 'Tengo un millón de cosas que hacer')",
                        options: ["Metáfora", "Hipérbole", "Anáfora", "Onomatopeya"],
                        answer: 1,
                        explanation: "La Hipérbole es una figura retórica que consiste en una exageración intencionada."
                    },
                    {
                        question: "¿Cuál es la forma verbal en participio de la palabra 'escribir'?",
                        options: ["Escribido", "Escribiendo", "Escrito", "Escribiera"],
                        answer: 2,
                        explanation: "El verbo escribir es irregular en participio: la forma correcta es 'escrito'."
                    },
                    {
                        question: "¿Qué es una biografía?",
                        options: [
                            "Un relato inventado sobre seres mitológicos.",
                            "La historia redactada de la vida de una persona escrita por otra.",
                            "Un poema de versos libres de amor.",
                            "Un reporte de investigación de experimentos."
                        ],
                        answer: 1,
                        explanation: "La biografía es el texto periodístico o literario que narra la vida real de una persona."
                    }
                ],
                science: [
                    {
                        question: "Según la Segunda Ley de Newton, la fuerza (F) aplicada a un objeto es igual a:",
                        options: ["Masa ÷ Aceleración", "Masa × Aceleración", "Velocidad × Tiempo", "Trabajo ÷ Distancia"],
                        answer: 1,
                        explanation: "La fórmula fundamental de la dinámica es F = m × a (Fuerza es igual a masa por aceleración)."
                    },
                    {
                        question: "¿Qué propiedad de la materia mide la cantidad de masa contenida en un determinado volumen (D = m/V)?",
                        options: ["Peso", "Presión", "Densidad", "Temperatura"],
                        answer: 2,
                        explanation: "La Densidad es una propiedad intensiva calculada como el cociente entre la masa y el volumen ocupado."
                    },
                    {
                        question: "Proceso físico en el cual una sustancia pasa del estado líquido al estado gaseoso al hervir:",
                        options: ["Condensación", "Evaporación / Vaporización", "Solidificación", "Sublimación"],
                        answer: 1,
                        explanation: "La vaporización o evaporación ocurre cuando las partículas ganan energía térmica y pasan de líquido a gas."
                    },
                    {
                        question: "¿Qué tipo de energía posee un objeto debido a su movimiento?",
                        options: ["Energía Potencial", "Energía Cinética", "Energía Térmica", "Energía Química"],
                        answer: 1,
                        explanation: "La Energía Cinética es la asociada al movimiento de los cuerpos."
                    }
                ],
                history: [
                    {
                        question: "¿Qué movimiento cultural e intelectual del siglo XVIII promovió el uso de la razón frente a la ignorancia y el absolutismo?",
                        options: ["El Renacimiento", "La Ilustración", "El Romanticismo", "El Feudalismo"],
                        answer: 1,
                        explanation: "La Ilustración ('Siglo de las Luces') situó la razón como la vía principal para el progreso de la humanidad."
                    },
                    {
                        question: "¿En qué siglo ocurrió la Independencia de la mayoría de las naciones hispanoamericanas?",
                        options: ["Siglo XVII (1600s)", "Siglo XIX (1800s)", "Siglo XX (1900s)", "Siglo XV (1400s)"],
                        answer: 1,
                        explanation: "Las guerras de independencia en Iberoamérica se desarrollaron principalmente entre 1808 y 1826 (Siglo XIX)."
                    },
                    {
                        question: "¿Cuál de los siguientes es un recurso natural no renovable?",
                        options: ["Energía Solar", "Petróleo", "Viento (Eólica)", "Agua"],
                        answer: 1,
                        explanation: "El petróleo es un hidrocarburo fósil cuyas reservas son finitas y tardan millones de años en formarse."
                    },
                    {
                        question: "¿Qué escala mide la magnitud o energía liberada por un sismo o terremoto?",
                        options: ["Escala Celsius", "Escala de Richter", "Escala de Beaufort", "Escala de Fujita"],
                        answer: 1,
                        explanation: "La escala sismológica de Richter se utiliza para evaluar la energía liberada en el foco sísmico."
                    }
                ]
            }
        };

        // ESTADO DE LA APLICACIÓN
        let appState = {
            grade: null,
            subject: null,
            mode: null,
            currentQuestionIndex: 0,
            score: 0,
            streak: 0,
            correctCount: 0,
            incorrectCount: 0,
            questions: [],
            userAnswers: [], // Para guardar historial
            timer: null,
            timeLeft: 60
        };

        // Nombres de materias para UI
        const subjectNames = {
            math: 'Matemáticas',
            spanish: 'Español',
            science: 'Ciencias',
            history: 'Historia y Geografía'
        };

        // SELECCIÓN DE OPCIONES EN SETUP
        function selectGrade(g) {
            appState.grade = g;
            document.querySelectorAll('.grade-option').forEach(el => el.classList.remove('border-brand-purple', 'bg-purple-50'));
            const selectedBtn = document.getElementById(`btn-grade-${g}`);
            selectedBtn.classList.add('border-brand-purple', 'bg-purple-50');
            checkReadyToStart();
        }

        function selectSubject(s) {
            appState.subject = s;
            document.querySelectorAll('.subject-option').forEach(el => el.classList.remove('border-brand-purple', 'bg-purple-50', 'border-blue-400', 'border-pink-400', 'border-green-400', 'border-orange-400'));
            const selectedBtn = document.getElementById(`btn-sub-${s}`);
            selectedBtn.classList.add('border-brand-purple', 'bg-purple-50');
            checkReadyToStart();
        }

        function selectMode(m) {
            appState.mode = m;
            document.querySelectorAll('.mode-option').forEach(el => el.classList.remove('border-brand-purple', 'bg-purple-50'));
            const selectedBtn = document.getElementById(`btn-mode-${m}`);
            selectedBtn.classList.add('border-brand-purple', 'bg-purple-50');
            checkReadyToStart();
        }

        function checkReadyToStart() {
            const btn = document.getElementById('btn-start-game');
            if (appState.grade && appState.subject && appState.mode) {
                btn.disabled = false;
                btn.classList.remove('bg-gray-300', 'text-gray-500', 'cursor-not-allowed');
                btn.classList.add('bg-gradient-to-r', 'from-brand-purple', 'to-brand-pink', 'text-white', 'hover:opacity-95', 'btn-bounce', 'cursor-pointer');
                btn.textContent = "¡Comenzar Desafío!";
            }
        }

        // INICIAR EL JUEGO
        function startGame() {
            // Cargar preguntas de la BD
            const pool = questionBank[appState.grade][appState.subject];
            // Clonar y desordenar (shuffle)
            appState.questions = [...pool].sort(() => Math.random() - 0.5);
            appState.currentQuestionIndex = 0;
            appState.score = 0;
            appState.streak = 0;
            appState.correctCount = 0;
            appState.incorrectCount = 0;
            appState.userAnswers = [];
            appState.timeLeft = 60;

            // Actualizar interfaz
            document.getElementById('screen-setup').classList.add('hidden');
            document.getElementById('screen-quiz').classList.remove('hidden');
            document.getElementById('header-stats').classList.remove('hidden');

            document.getElementById('badge-grade').textContent = `${appState.grade}º Sec`;
            document.getElementById('badge-subject').textContent = subjectNames[appState.subject];

            if (appState.mode === 'timed') {
                document.getElementById('timer-badge').classList.remove('hidden');
                document.getElementById('quiz-progress-badge').classList.add('hidden');
                startTimer();
            } else {
                document.getElementById('timer-badge').classList.add('hidden');
                document.getElementById('quiz-progress-badge').classList.remove('hidden');
            }

            updateHeaderStats();
            renderQuestion();
        }

        // TIMER PARA MODO RELOJ
        function startTimer() {
            clearInterval(appState.timer);
            appState.timer = setInterval(() => {
                appState.timeLeft--;
                document.getElementById('timer-count').textContent = `${appState.timeLeft}s`;
                if (appState.timeLeft <= 0) {
                    clearInterval(appState.timer);
                    endQuiz();
                }
            }, 1000);
        }

        // RENDERIZAR PREGUNTA ACTUAL
        function renderQuestion() {
            const q = appState.questions[appState.currentQuestionIndex];
            if (!q) {
                endQuiz();
                return;
            }

            // Ocultar explicaciones y botón siguiente
            document.getElementById('explanation-box').classList.add('hidden');
            document.getElementById('btn-next-question').classList.add('hidden');

            // Actualizar pregunta
            document.getElementById('question-text').textContent = q.question;

            // Actualizar contador y barra de progreso
            const totalQ = appState.questions.length;
            document.getElementById('question-tracker').textContent = `Pregunta ${appState.currentQuestionIndex + 1} de ${totalQ}`;
            const progressPct = ((appState.currentQuestionIndex) / totalQ) * 100;
            document.getElementById('progress-bar-fill').style.width = `${progressPct}%`;

            // Renderizar Opciones
            const container = document.getElementById('options-container');
            container.innerHTML = '';

            q.options.forEach((opt, idx) => {
                const btn = document.createElement('button');
                btn.type = 'button';
                btn.className = `option-btn w-full text-left p-4 rounded-2xl border-2 border-gray-200 bg-white hover:border-purple-300 hover:bg-purple-50/50 font-bold text-gray-700 transition duration-150 flex items-center justify-between group`;
                btn.onclick = () => handleAnswerSelect(idx);
                btn.innerHTML = `
                    <span class="flex items-center">
                        <span class="w-8 h-8 rounded-xl bg-gray-100 text-gray-600 font-extrabold flex items-center justify-center mr-3 group-hover:bg-brand-purple group-hover:text-white transition-colors">${String.fromCharCode(65 + idx)}</span>
                        <span>${opt}</span>
                    </span>
                    <i class="fa-regular fa-circle text-gray-300 text-xl group-hover:text-brand-purple"></i>
                `;
                container.appendChild(btn);
            });
        }

        // EVALUACIÓN DE RESPUESTA
        function handleAnswerSelect(selectedIndex) {
            const q = appState.questions[appState.currentQuestionIndex];
            const isCorrect = selectedIndex === q.answer;

            // Deshabilitar todos los botones de opciones
            const buttons = document.querySelectorAll('.option-btn');
            buttons.forEach(btn => btn.disabled = true);

            // Registrar respuesta
            appState.userAnswers.push({
                question: q.question,
                selected: q.options[selectedIndex],
                correct: q.options[q.answer],
                isCorrect: isCorrect,
                explanation: q.explanation
            });

            // Feedback visual en el botón seleccionado
            const selectedBtn = buttons[selectedIndex];
            if (isCorrect) {
                appState.score += 100 + (appState.streak * 10);
                appState.streak++;
                appState.correctCount++;
                selectedBtn.classList.remove('border-gray-200', 'bg-white');
                selectedBtn.classList.add('border-emerald-500', 'bg-emerald-50', 'text-emerald-800');
                selectedBtn.querySelector('i').className = 'fa-solid fa-circle-check text-emerald-500 text-xl';
                
                // Efecto confeti
                triggerConfetti();
            } else {
                appState.streak = 0;
                appState.incorrectCount++;
                selectedBtn.classList.remove('border-gray-200', 'bg-white');
                selectedBtn.classList.add('border-rose-500', 'bg-rose-50', 'text-rose-800', 'animate-shake');
                selectedBtn.querySelector('i').className = 'fa-solid fa-circle-xmark text-rose-500 text-xl';

                // Mostrar la correcta
                const correctBtn = buttons[q.answer];
                correctBtn.classList.remove('border-gray-200');
                correctBtn.classList.add('border-emerald-500', 'bg-emerald-50/60');
            }

            updateHeaderStats();

            // Lógica según el modo
            if (appState.mode === 'practice') {
                showExplanation(isCorrect, q.explanation);
                document.getElementById('btn-next-question').classList.remove('hidden');
                document.getElementById('btn-next-question').classList.add('flex');
            } else {
                // Modo Reloj pasa automáticamente tras un breve delay
                setTimeout(() => {
                    appState.currentQuestionIndex++;
                    if (appState.currentQuestionIndex < appState.questions.length) {
                        renderQuestion();
                    } else {
                        endQuiz();
                    }
                }, 800);
            }
        }

        function showExplanation(isCorrect, text) {
            const box = document.getElementById('explanation-box');
            const icon = document.getElementById('explanation-icon');
            const title = document.getElementById('explanation-title');
            const explanationText = document.getElementById('explanation-text');

            box.classList.remove('hidden', 'bg-emerald-50', 'border-emerald-200', 'bg-rose-50', 'border-rose-200');
            
            if (isCorrect) {
                box.classList.add('bg-emerald-50', 'border', 'border-emerald-200', 'text-emerald-900');
                icon.innerHTML = '<i class="fa-solid fa-lightbulb text-emerald-600"></i>';
                title.textContent = "¡Respuesta Correcta!";
            } else {
                box.classList.add('bg-rose-50', 'border', 'border-rose-200', 'text-rose-900');
                icon.innerHTML = '<i class="fa-solid fa-circle-info text-rose-600"></i>';
                title.textContent = "Respuesta Incorrecta";
            }

            explanationText.textContent = text;
        }

        function nextQuestion() {
            appState.currentQuestionIndex++;
            if (appState.currentQuestionIndex < appState.questions.length) {
                renderQuestion();
            } else {
                endQuiz();
            }
        }

        function updateHeaderStats() {
            document.getElementById('score-counter').textContent = appState.score;
            document.getElementById('streak-counter').textContent = appState.streak;
        }

        function triggerConfetti() {
            if (typeof confetti === 'function') {
                confetti({
                    particleCount: 40,
                    spread: 60,
                    origin: { y: 0.8 }
                });
            }
        }

        // FINALIZACIÓN DEL QUIZ
        function endQuiz() {
            clearInterval(appState.timer);
            document.getElementById('screen-quiz').classList.add('hidden');
            document.getElementById('screen-results').classList.remove('hidden');

            const total = appState.correctCount + appState.incorrectCount;
            const accuracy = total > 0 ? Math.round((appState.correctCount / total) * 100) : 0;

            document.getElementById('result-score').textContent = `${appState.score} pts`;
            document.getElementById('result-correct').textContent = appState.correctCount;
            document.getElementById('result-incorrect').textContent = appState.incorrectCount;
            document.getElementById('result-accuracy').textContent = `${accuracy}%`;

            // Personalizar títulos
            const trophyIcon = document.getElementById('result-trophy-icon');
            const resultTitle = document.getElementById('result-title');
            
            if (accuracy >= 80) {
                resultTitle.textContent = "¡Excelente Trabajo! 🌟";
                trophyIcon.className = "fa-solid fa-trophy text-yellow-500 animate-bounce";
                triggerConfetti();
            } else if (accuracy >= 50) {
                resultTitle.textContent = "¡Buen Intento! 👍";
                trophyIcon.className = "fa-solid fa-award text-blue-500";
            } else {
                resultTitle.textContent = "¡Sigue Practicando! 💪";
                trophyIcon.className = "fa-solid fa-book-open-reader text-purple-500";
            }

            // Renderizar repaso de preguntas
            const reviewList = document.getElementById('review-list');
            reviewList.innerHTML = '';

            appState.userAnswers.forEach((ans, idx) => {
                const item = document.createElement('div');
                item.className = `p-3 rounded-xl border text-xs md:text-sm ${ans.isCorrect ? 'bg-emerald-50/50 border-emerald-200' : 'bg-rose-50/50 border-rose-200'}`;
                item.innerHTML = `
                    <div class="font-bold text-gray-800 mb-1">${idx + 1}. ${ans.question}</div>
                    <div class="flex items-center space-x-2">
                        <span class="${ans.isCorrect ? 'text-emerald-700' : 'text-rose-700'} font-semibold">
                            ${ans.isCorrect ? '<i class="fa-solid fa-check mr-1"></i> Correcto' : '<i class="fa-solid fa-xmark mr-1"></i> Tu respuesta: ' + ans.selected}
                        </span>
                    </div>
                    ${!ans.isCorrect ? `<div class="text-gray-600 mt-1"><strong>Respuesta correcta:</strong> ${ans.correct}</div>` : ''}
                    <div class="text-gray-500 italic text-xs mt-1 border-t border-gray-200/50 pt-1">${ans.explanation}</div>
                `;
                reviewList.appendChild(item);
            });
        }

        // NAVEGACIÓN Y REINICIO
        function goHome() {
            clearInterval(appState.timer);
            document.getElementById('screen-results').classList.add('hidden');
            document.getElementById('screen-quiz').classList.add('hidden');
            document.getElementById('header-stats').classList.add('hidden');
            document.getElementById('screen-setup').classList.remove('hidden');
        }

        function restartQuiz() {
            document.getElementById('screen-results').classList.add('hidden');
            startGame();
        }
    </script>
</body>
</html>