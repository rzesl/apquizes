<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AP Biology: Water & Hydrogen Bonding Quiz</title>
    <style>
        :root {
            --primary: #2980b9;
            --secondary: #3498db;
            --correct: #27ae60;
            --incorrect: #c0392b;
            --bg: #f0f4f8;
            --text: #2c3e50;
        }
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }
        .container {
            max-width: 800px;
            width: 100%;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: var(--primary);
            border-bottom: 2px solid #eee;
            padding-bottom: 10px;
        }
        .progress-bar {
            height: 10px;
            background: #eee;
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }
        #progress {
            height: 100%;
            background: var(--secondary);
            width: 0%;
            transition: width 0.3s;
        }
        .question-box {
            display: none;
        }
        .question-box.active {
            display: block;
        }
        .question-text {
            font-size: 1.2em;
            font-weight: 600;
            margin-bottom: 20px;
        }
        .options {
            display: grid;
            gap: 10px;
        }
        .option {
            padding: 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .option:hover {
            background: #f9f9f9;
            border-color: var(--secondary);
        }
        .option.correct {
            background: #d4edda;
            border-color: var(--correct);
            color: #155724;
        }
        .option.incorrect {
            background: #f8d7da;
            border-color: var(--incorrect);
            color: #721c24;
        }
        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 8px;
            display: none;
            line-height: 1.5;
        }
        .controls {
            margin-top: 30px;
            display: flex;
            justify-content: space-between;
        }
        button {
            padding: 10px 25px;
            border: none;
            border-radius: 5px;
            background: var(--primary);
            color: white;
            cursor: pointer;
            font-weight: bold;
        }
        button:disabled {
            background: #ccc;
        }
        #results {
            display: none;
            text-align: center;
        }
        .score-circle {
            font-size: 3em;
            font-weight: bold;
            color: var(--primary);
            margin: 20px 0;
        }
    </style>
</head>
<body>

<div class="container">
    <div id="quiz-ui">
        <h1>AP Biology: Properties of Water</h1>
        <div class="progress-bar"><div id="progress"></div></div>
        <div id="question-container"></div>
        <div class="controls">
            <button id="next-btn" onclick="nextQuestion()" disabled>Next Question</button>
        </div>
    </div>

    <div id="results">
        <h1>Quiz Complete!</h1>
        <div class="score-circle" id="final-score"></div>
        <p id="commentary"></p>
        <button onclick="location.reload()">Restart Quiz</button>
    </div>
</div>

<script>
const quizData = [
    {
        q: "Which property of water is most directly responsible for the ability of sweat to lower body temperature?",
        a: ["High specific heat", "High heat of vaporization", "Cohesion", "Adhesion"],
        correct: 1,
        exp: "High heat of vaporization means water requires significant energy to transform from liquid to gas. As sweat evaporates from the skin, it absorbs and carries away a large amount of body heat."
    },
    {
        q: "The partial negative charge at one end of a water molecule is attracted to the partial positive charge of another water molecule. What is this attraction called?",
        a: ["A covalent bond", "An ionic bond", "A hydrogen bond", "A van der Waals interaction"],
        correct: 2,
        exp: "Hydrogen bonds occur between the highly electronegative oxygen of one water molecule and the electropositive hydrogen of another."
    },
    {
        q: "Why does ice float in liquid water?",
        a: ["The crystalline lattice of ice causes it to be more dense than liquid water.", "Hydrogen bonds stabilize and keep the molecules of ice farther apart than the water molecules of liquid water.", "The ionic bonds between the molecules in ice prevent the ice from sinking.", "The high surface tension of liquid water keeps the ice on top."],
        correct: 1,
        exp: "In ice, each molecule is hydrogen-bonded to four neighbors in a rigid 3D lattice, pushing the molecules further apart and decreasing density."
    },
    {
        q: "Water's high specific heat is a consequence of which of the following?",
        a: ["Absorption and release of heat when hydrogen bonds break and form", "The high density of liquid water", "The small size of the water molecule", "The low volatility of water"],
        correct: 0,
        exp: "Heat must be absorbed to break hydrogen bonds before the molecules can move faster (increasing temp), and heat is released when bonds form."
    },
    {
        q: "What is the maximum number of hydrogen bonds a single water molecule can form with neighboring water molecules?",
        a: ["2", "3", "4", "6"],
        correct: 2,
        exp: "A water molecule has two hydrogen atoms (donors) and two lone pairs on oxygen (acceptors), allowing for 4 hydrogen bonds."
    },
    {
        q: "Which of the following effects is produced by the high surface tension of water?",
        a: ["Lakes don't freeze solid in winter.", "A water strider can walk across the surface of a small pond.", "Organisms resist temperature changes.", "Water can move up the xylem of a tree."],
        correct: 1,
        exp: "Surface tension is a measure of how difficult it is to stretch or break the surface of a liquid, caused by the collective strength of hydrogen bonds."
    },
    {
        q: "A dietitian recommends a patient drink more water because it is the 'universal solvent.' This property is primarily due to water's:",
        a: ["High specific heat", "Polarity", "Neutral pH", "Hydrogen bonding with nonpolar solutes"],
        correct: 1,
        exp: "Water's polarity allows it to form hydration shells around ions and polar molecules, effectively dissolving them."
    },
    {
        q: "Which of the following is an example of adhesion?",
        a: ["Water molecules sticking to each other", "Water molecules sticking to the cellulose walls of plant cells", "Water forming a bead on a waxed car", "Surface tension"],
        correct: 1,
        exp: "Adhesion is the clinging of one substance to another. In plants, water adheres to cell walls via hydrogen bonding, helping counter gravity."
    },
    {
        q: "In a single water molecule, the bond between the oxygen and hydrogen atoms is a(n):",
        a: ["Hydrogen bond", "Nonpolar covalent bond", "Polar covalent bond", "Ionic bond"],
        correct: 2,
        exp: "Intramolecular bonds (within the molecule) are polar covalent because oxygen is significantly more electronegative than hydrogen."
    },
    {
        q: "If the temperature of a lake rises from 20°C to 22°C, what is happening to the hydrogen bonds?",
        a: ["They are forming more frequently.", "They are breaking more frequently.", "They are becoming stronger.", "They are being replaced by covalent bonds."],
        correct: 1,
        exp: "Increasing temperature increases molecular kinetic energy, causing hydrogen bonds to break more often than they form."
    },
    {
        q: "The pH of a solution is determined by the concentration of which ion, often resulting from water dissociation?",
        a: ["O2-", "OH-", "H+", "H2"],
        correct: 2,
        exp: "pH is the negative log of the hydrogen ion (H+) concentration. These ions are produced when a water molecule dissociates."
    },
    {
        q: "In the xylem of a tall tree, water column breakage (cavitation) is prevented primarily by:",
        a: ["Adhesion", "Cohesion", "Evaporation", "Active transport"],
        correct: 1,
        exp: "Cohesion (water molecules sticking to each other) ensures that a continuous pull is transmitted from the leaves down to the roots."
    },
    {
        q: "Which of the following best describes a hydrophobic substance?",
        a: ["A substance that has an affinity for water", "A nonpolar substance that repels water", "An ionic substance that dissolves in water", "A polar substance that forms hydrogen bonds"],
        correct: 1,
        exp: "Hydrophobic substances are nonpolar and cannot form hydrogen bonds with water, thus they do not dissolve."
    },
    {
        q: "Which of the following would be the most likely result if water's hydrogen bonds were significantly weaker?",
        a: ["Water would be a liquid at a wider range of temperatures.", "Water would evaporate more easily.", "Ice would be more dense than liquid water.", "Plants would grow taller."],
        correct: 1,
        exp: "Weaker hydrogen bonds would mean less energy is required to move molecules into the gas phase, increasing volatility/evaporation."
    },
    {
        q: "How does water protect aquatic life in cold climates during winter?",
        a: ["By contracting when it freezes", "By insulating the water below with a layer of floating ice", "By sinking when it reaches 0°C", "By increasing its kinetic energy as it freezes"],
        correct: 1,
        exp: "Floating ice acts as an insulator, preventing the water beneath from freezing solid and allowing life to survive."
    },
    {
        q: "The bond angle in a water molecule is approximately 104.5°. This V-shape is crucial because it:",
        a: ["Makes the molecule nonpolar", "Allows the oxygen to be positive", "Ensures the molecule is polar with distinct poles", "Prevents hydrogen bonding"],
        correct: 2,
        exp: "The asymmetrical shape and polar bonds ensure the molecule has a dipole moment (polarity)."
    },
    {
        q: "When sodium chloride (NaCl) dissolves in water, the oxygen atoms of the water molecules wrap around:",
        a: ["The Sodium ions (Na+)", "The Chloride ions (Cl-)", "Both ions equally", "Neither, they face away from the ions"],
        correct: 0,
        exp: "Oxygen has a partial negative charge, so it is attracted to and surrounds the positive Sodium (Na+) ions."
    },
    {
        q: "Thermal energy is defined as:",
        a: ["The average kinetic energy of molecules", "The total kinetic energy associated with the random motion of atoms", "The measure of temperature in Celsius", "The heat required to raise 1g of water by 1 degree"],
        correct: 1,
        exp: "Thermal energy is total kinetic energy (volume-dependent), whereas temperature is average kinetic energy (volume-independent)."
    },
    {
        q: "Which property allows coastal regions to have more moderate climates than inland regions?",
        a: ["High surface tension", "High specific heat", "Adhesion", "Low density of ice"],
        correct: 1,
        exp: "The ocean absorbs heat during the day/summer and releases it during the night/winter, moderating the air temperature."
    },
    {
        q: "Evaporative cooling is most effective when the relative humidity is:",
        a: ["High", "Low", "100%", "Fluctuating"],
        correct: 1,
        exp: "Low humidity allows for faster evaporation, as the air is not yet saturated with water vapor."
    }
];

let currentIdx = 0;
let score = 0;
let answered = false;

function initQuiz() {
    const container = document.getElementById('question-container');
    quizData.forEach((data, i) => {
        const div = document.createElement('div');
        div.className = `question-box ${i === 0 ? 'active' : ''}`;
        div.id = `qbox-${i}`;
        
        div.innerHTML = `
            <div class="question-text">${i+1}. ${data.q}</div>
            <div class="options">
                ${data.a.map((opt, optIdx) => `
                    <div class="option" onclick="checkAnswer(${i}, ${optIdx})" id="opt-${i}-${optIdx}">${opt}</div>
                `).join('')}
            </div>
            <div class="feedback" id="feedback-${i}"></div>
        `;
        container.appendChild(div);
    });
    updateProgress();
}

function checkAnswer(qIdx, optIdx) {
    if (answered) return;
    answered = true;
    
    const data = quizData[qIdx];
    const feedback = document.getElementById(`feedback-${qIdx}`);
    const nextBtn = document.getElementById('next-btn');
    
    const options = document.querySelectorAll(`#qbox-${qIdx} .option`);
    
    if (optIdx === data.correct) {
        options[optIdx].classList.add('correct');
        feedback.innerHTML = "<strong>Correct!</strong> " + data.exp;
        feedback.style.background = "#d4edda";
        feedback.style.color = "#155724";
        score++;
    } else {
        options[optIdx].classList.add('incorrect');
        options[data.correct].classList.add('correct');
        feedback.innerHTML = "<strong>Incorrect.</strong> " + data.exp;
        feedback.style.background = "#f8d7da";
        feedback.style.color = "#721c24";
    }
    
    feedback.style.display = "block";
    nextBtn.disabled = false;
}

function nextQuestion() {
    const currentBox = document.getElementById(`qbox-${currentIdx}`);
    currentBox.classList.remove('active');
    
    currentIdx++;
    answered = false;
    document.getElementById('next-btn').disabled = true;
    
    if (currentIdx < quizData.length) {
        document.getElementById(`qbox-${currentIdx}`).classList.add('active');
        updateProgress();
    } else {
        showResults();
    }
}

function updateProgress() {
    const progress = (currentIdx / quizData.length) * 100;
    document.getElementById('progress').style.width = progress + "%";
}

function showResults() {
    document.getElementById('quiz-ui').style.display = 'none';
    const results = document.getElementById('results');
    results.style.display = 'block';
    
    const finalScore = document.getElementById('final-score');
    finalScore.innerText = `${score} / ${quizData.length}`;
    
    const commentary = document.getElementById('commentary');
    const percent = (score / quizData.length) * 100;
    
    if (percent >= 90) commentary.innerText = "Excellent! You have a master-level understanding of water's properties.";
    else if (percent >= 70) commentary.innerText = "Great job! You have a solid grasp of the material.";
    else if (percent >= 50) commentary.innerText = "Good effort, but review the mechanics of hydrogen bonding.";
    else commentary.innerText = "Keep studying! Focus on how polarity leads to emergent properties.";
}

initQuiz();
</script>

</body>
</html>
