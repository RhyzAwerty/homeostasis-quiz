<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Homeostasis Quiz</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
            color: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }
        
        .container {
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
            width: 100%;
            max-width: 600px;
            margin-top: 20px;
        }
        
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 30px;
        }
        
        .quiz-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            font-weight: bold;
        }
        
        .timer {
            color: #e74c3c;
        }
        
        .question-container {
            margin-bottom: 20px;
        }
        
        .question {
            font-size: 1.2rem;
            margin-bottom: 15px;
            font-weight: bold;
        }
        
        .options {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .option {
            padding: 10px 15px;
            background-color: #ecf0f1;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        
        .option:hover {
            background-color: #d6eaf8;
        }
        
        .option.selected {
            background-color: #3498db;
            color: white;
        }
        
        .option.correct {
            background-color: #2ecc71;
            color: white;
        }
        
        .option.incorrect {
            background-color: #e74c3c;
            color: white;
        }
        
        .btn {
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            transition: background-color 0.3s;
            margin-top: 10px;
        }
        
        .btn:hover {
            background-color: #2980b9;
        }
        
        .btn:disabled {
            background-color: #bdc3c7;
            cursor: not-allowed;
        }
        
        .results {
            display: none;
            margin-top: 20px;
        }
        
        .score {
            font-size: 1.5rem;
            font-weight: bold;
            text-align: center;
            margin-bottom: 20px;
        }
        
        .result-item {
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
        }
        
        .result-item.correct {
            background-color: #d5f5e3;
        }
        
        .result-item.incorrect {
            background-color: #fadbd8;
        }
        
        .time-up {
            color: #e74c3c;
            font-weight: bold;
            text-align: center;
            margin-bottom: 20px;
            display: none;
        }
        
        @media (max-width: 500px) {
            .container {
                padding: 15px;
            }
            
            .question {
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Homeostasis Quiz</h1>
        
        <div id="start-screen">
            <p>Test your knowledge of homeostasis with this 10-question quiz. You have 1 minute to complete it!</p>
            <button id="start-btn" class="btn">Start Quiz</button>
        </div>
        
        <div id="quiz-screen" style="display: none;">
            <div class="quiz-header">
                <div>Question <span id="question-number">1</span>/10</div>
                <div class="timer">Time: <span id="time">01:00</span></div>
            </div>
            
            <div class="question-container">
                <div class="question" id="question-text"></div>
                <div class="options" id="options-container"></div>
            </div>
            
            <button id="next-btn" class="btn" disabled>Next Question</button>
            
            <div class="time-up" id="time-up-message">Time's up! Quiz completed.</div>
        </div>
        
        <div id="results-screen" class="results">
            <div class="score">Your Score: <span id="final-score">0</span>/10</div>
            
            <div id="results-container"></div>
            
            <button id="restart-btn" class="btn">Restart Quiz</button>
        </div>
    </div>

    <script>
        // Quiz questions
        const questions = [
            {
                question: "What is the normal body temperature of a human?",
                options: ["36°C", "37°C", "38°C", "39°C"],
                answer: 1,
                explanation: "The normal human body temperature is approximately 37°C (98.6°F)."
            },
            {
                question: "What is the meaning of 'ENDO' from the word ENDOTHERM?",
                options: ["Internal", "External", "Temperature", "Hypothalamus"],
                answer: 0,
                explanation: "'Endo' means internal, referring to how endotherms generate heat internally."
            },
            {
                question: "What is Homeostasis?",
                options: [
                    "Process of digestion", 
                    "Breaking down of Food", 
                    "Maintenance of Internal balance in the body", 
                    "Movement of blood"
                ],
                answer: 2,
                explanation: "Homeostasis is the process by which organisms maintain a relatively stable internal environment."
            },
            {
                question: "What organ helps regulate body temperature in the human body?",
                options: ["Liver", "Brain", "Heart", "Lungs"],
                answer: 1,
                explanation: "The brain, specifically the hypothalamus, helps regulate body temperature."
            },
            {
                question: "What is the term for the process of maintaining a salt and water balance in the body?",
                options: [
                    "Thermoregulation", 
                    "Metabolism", 
                    "Homeostasis", 
                    "Osmoregulation"
                ],
                answer: 3,
                explanation: "Osmoregulation is the process of maintaining salt and water balance across membranes."
            },
            {
                question: "Which body systems are mainly responsible for maintaining homeostasis?",
                options: [
                    "Circulatory and muscular systems", 
                    "Digestive and excretory systems", 
                    "Nervous and endocrine systems", 
                    "Respiratory and skeletal systems"
                ],
                answer: 2,
                explanation: "The nervous and endocrine systems work together to detect changes and respond to maintain homeostasis."
            },
            {
                question: "Which type of animal is classified as endothermic?",
                options: ["Birds", "Fish", "Reptiles", "Amphibians"],
                answer: 0,
                explanation: "Birds are endothermic (warm-blooded), meaning they can regulate their body temperature internally."
            },
            {
                question: "In a situation where a person is dehydrated, what role does the hypothalamus play in restoring homeostasis?",
                options: [
                    "It ignores the signals and maintains normal function", 
                    "It sends signals to the kidneys to excrete more water", 
                    "It triggers thirst and releases antidiuretic hormone (ADH) to conserve water", 
                    "It increases the metabolic rate to produce more heat"
                ],
                answer: 2,
                explanation: "The hypothalamus detects dehydration and triggers thirst while releasing ADH to reduce water loss."
            },
            {
                question: "In a scenario where blood glucose levels rise significantly after a meal, what feedback mechanism is activated to restore homeostasis?",
                options: [
                    "Positive feedback to increase glucose levels", 
                    "Negative feedback to decrease glucose levels through insulin release", 
                    "No feedback mechanism is activated", 
                    "Positive feedback to decrease insulin levels"
                ],
                answer: 1,
                explanation: "Rising blood glucose triggers negative feedback through insulin release to lower glucose levels."
            },
            {
                question: "If a cell is placed in a hypotonic solution, what will happen to the cell?",
                options: [
                    "The cell will shrink because water moves out", 
                    "The cell will remain unchanged", 
                    "The cell will swell and may burst because water moves in", 
                    "The cell will die immediately"
                ],
                answer: 2,
                explanation: "In a hypotonic solution, water moves into the cell, causing it to swell and potentially burst."
            }
        ];

        // DOM elements
        const startScreen = document.getElementById('start-screen');
        const quizScreen = document.getElementById('quiz-screen');
        const resultsScreen = document.getElementById('results-screen');
        const startBtn = document.getElementById('start-btn');
        const nextBtn = document.getElementById('next-btn');
        const restartBtn = document.getElementById('restart-btn');
        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const questionNumber = document.getElementById('question-number');
        const timeDisplay = document.getElementById('time');
        const finalScore = document.getElementById('final-score');
        const resultsContainer = document.getElementById('results-container');
        const timeUpMessage = document.getElementById('time-up-message');

        // Quiz variables
        let currentQuestionIndex = 0;
        let score = 0;
        let timer;
        let timeLeft = 60; // 1 minute
        let shuffledQuestions = [];
        let userAnswers = [];

        // Start the quiz
        startBtn.addEventListener('click', startQuiz);
        restartBtn.addEventListener('click', startQuiz);

        // Next question button
        nextBtn.addEventListener('click', () => {
            currentQuestionIndex++;
            if (currentQuestionIndex < shuffledQuestions.length) {
                showQuestion();
            } else {
                endQuiz();
            }
        });

        function startQuiz() {
            // Reset variables
            score = 0;
            currentQuestionIndex = 0;
            timeLeft = 60;
            userAnswers = [];
            
            // Shuffle questions
            shuffledQuestions = [...questions].sort(() => Math.random() - 0.5);
            
            // Show quiz screen
            startScreen.style.display = 'none';
            resultsScreen.style.display = 'none';
            quizScreen.style.display = 'block';
            timeUpMessage.style.display = 'none';
            
            // Start timer
            startTimer();
            
            // Show first question
            showQuestion();
        }

        function startTimer() {
            clearInterval(timer);
            updateTimerDisplay();
            
            timer = setInterval(() => {
                timeLeft--;
                updateTimerDisplay();
                
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    timeUpMessage.style.display = 'block';
                    nextBtn.disabled = true;
                    setTimeout(endQuiz, 1500);
                }
            }, 1000);
        }

        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            timeDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function showQuestion() {
            const question = shuffledQuestions[currentQuestionIndex];
            questionText.textContent = question.question;
            questionNumber.textContent = currentQuestionIndex + 1;
            
            // Clear previous options
            optionsContainer.innerHTML = '';
            
            // Create new options
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.classList.add('option');
                optionElement.textContent = option;
                optionElement.dataset.index = index;
                
                optionElement.addEventListener('click', () => selectOption(optionElement, index));
                
                optionsContainer.appendChild(optionElement);
            });
            
            // Reset next button
            nextBtn.disabled = true;
        }

        function selectOption(optionElement, optionIndex) {
            // Remove selected class from all options
            document.querySelectorAll('.option').forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Add selected class to clicked option
            optionElement.classList.add('selected');
            
            // Enable next button
            nextBtn.disabled = false;
            
            // Store user's answer
            userAnswers[currentQuestionIndex] = optionIndex;
        }

        function endQuiz() {
            clearInterval(timer);
            
            // Calculate score
            score = 0;
            shuffledQuestions.forEach((question, index) => {
                if (userAnswers[index] === question.answer) {
                    score++;
                }
            });
            
            // Display results
            finalScore.textContent = score;
            resultsContainer.innerHTML = '';
            
            shuffledQuestions.forEach((question, index) => {
                const resultItem = document.createElement('div');
                const isCorrect = userAnswers[index] === question.answer;
                
                resultItem.classList.add('result-item');
                resultItem.classList.add(isCorrect ? 'correct' : 'incorrect');
                
                const userAnswer = userAnswers[index] !== undefined ? 
                    question.options[userAnswers[index]] : 'Not answered';
                const correctAnswer = question.options[question.answer];
                
                resultItem.innerHTML = `
                    <p><strong>Question ${index + 1}:</strong> ${question.question}</p>
                    <p><strong>Your answer:</strong> ${userAnswer}</p>
                    <p><strong>Correct answer:</strong> ${correctAnswer}</p>
                    <p><em>${question.explanation}</em></p>
                `;
                
                resultsContainer.appendChild(resultItem);
            });
            
            // Show results screen
            quizScreen.style.display = 'none';
            resultsScreen.style.display = 'block';
        }
    </script>
</body>
</html>
