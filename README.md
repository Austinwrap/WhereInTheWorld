<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Where in the World is Morgan Sampson?</title>
    <style>
        body {
            background: linear-gradient(45deg, #1a1a1a, #2a004a, #4a0080);
            color: #fff;
            font-family: 'Arial', sans-serif;
            text-align: center;
            margin: 0;
            padding: 10px;
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }
        #game-container {
            max-width: 100%;
            width: 100%;
            padding: 10px;
            box-sizing: border-box;
            position: relative;
            z-index: 2;
        }
        h1 {
            color: #ff00ff;
            font-size: 24px;
            text-shadow: 0 0 15px #ff00ff, 0 0 25px #00ffff;
            animation: neon 0.8s ease-in-out infinite alternate;
            margin: 0;
        }
        p#intro, p#question {
            font-size: 16px;
            color: #00ffff;
            text-shadow: 0 0 5px #00ffff;
        }
        button {
            background: #ff00ff;
            color: #fff;
            border: 2px solid #00ffff;
            padding: 15px;
            margin: 10px 0;
            font-size: 18px;
            width: 90%;
            max-width: 300px;
            border-radius: 10px;
            box-shadow: 0 0 15px #ff00ff;
            cursor: pointer;
            transition: transform 0.2s, background 0.2s;
        }
        button:hover, button:active {
            background: #00ffff;
            color: #000;
            transform: scale(1.05);
        }
        #result-popup {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            z-index: 10;
            padding: 20px;
            box-sizing: border-box;
            overflow-y: auto;
        }
        #result-title {
            font-size: 36px;
            margin: 10px 0;
            line-height: 1.2;
        }
        .win {
            color: #00ff00;
            text-shadow: 0 0 30px #00ff00, 0 0 40px #ff00ff, 0 0 50px #ffff00;
            animation: buzz 0.08s infinite, scale 0.3s infinite alternate;
        }
        .lose {
            color: #ff0000;
            text-shadow: 0 0 15px #ff0000;
            animation: vibrate 0.3s infinite;
        }
        #confetti {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 11;
        }
        .confetti-piece {
            position: absolute;
            width: 15px;
            height: 15px;
            background: #ff00ff;
            animation: fall 2s linear infinite;
        }
        #result-text, #fun-facts, #review, #reward {
            font-size: 16px;
            color: #fff;
            text-shadow: 0 0 5px #00ffff;
        }
        #globe {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 40px;
            height: 40px;
            background: radial-gradient(circle, #00ffff 20%, #000 70%);
            border-radius: 50%;
            box-shadow: 0 0 10px #00ffff;
            animation: spin 4s linear infinite;
            z-index: 1;
        }
        @keyframes neon {
            from { text-shadow: 0 0 10px #ff00ff, 0 0 20px #00ffff; }
            to { text-shadow: 0 0 25px #ff00ff, 0 0 50px #00ffff; }
        }
        @keyframes buzz {
            0% { transform: translate(3px, -3px); }
            50% { transform: translate(-3px, 3px); }
            100% { transform: translate(3px, -3px); }
        }
        @keyframes scale {
            from { transform: scale(1); }
            to { transform: scale(1.3); }
        }
        @keyframes vibrate {
            0% { transform: translate(0); }
            20% { transform: translate(-2px, 2px); }
            40% { transform: translate(2px, -2px); }
            60% { transform: translate(-2px, -2px); }
            80% { transform: translate(2px, 2px); }
            100% { transform: translate(0); }
        }
        @keyframes fall {
            0% { transform: translateY(-100vh) rotate(0deg); }
            100% { transform: translateY(100vh) rotate(360deg); }
        }
        @keyframes spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        @media (max-width: 768px) {
            h1 { font-size: 20px; }
            button { font-size: 16px; padding: 12px; }
            #result-title { font-size: 28px; }
            #globe { width: 30px; height: 30px; }
        }
    </style>
</head>
<body>
    <div id="game-container">
        <h1>Where in the World is Morgan Sampson?</h1>
        <p id="intro">Morgan’s a party queen on the run! Catch her in 5 tries!</p>
        <p id="question"></p>
        <div id="options"></div>
        <div id="result-popup">
            <div id="confetti"></div>
            <h2 id="result-title"></h2>
            <p id="result-text"></p>
            <p id="fun-facts"></p>
            <p id="review"></p>
            <p id="reward"></p>
            <button id="next-button" onclick="nextRound()">Find Her Again!</button>
        </div>
    </div>
    <div id="globe"></div>

    <script>
        const places = [
            "Amsterdam, Netherlands", "Ibiza, Spain", "Las Vegas, USA", "Bangkok, Thailand", "Woodstock, USA",
            "Haight-Ashbury, USA", "Glastonbury, UK", "Berlin, Germany", "New Orleans, USA", "Goa, India",
            "Roskilde, Denmark", "Pattaya, Thailand", "Marrakech, Morocco", "San Francisco, USA", "Tokyo, Japan",
            "Christiania, Denmark", "Mykonos, Greece", "Memphis, USA", "London, UK", "Rio de Janeiro, Brazil",
            "Key West, USA", "Bali, Indonesia", "Austin, USA", "Kingston, Jamaica", "Brighton, UK",
            "Venice Beach, USA", "Tijuana, Mexico", "Mumbai, India", "Palm Springs, USA", "Moscow, Russia",
            "Athens, Greece", "Cairo, Egypt", "Havana, Cuba", "Seattle, USA", "Prague, Czech Republic",
            "Reykjavik, Iceland", "Monaco", "Shanghai, China", "Cape Town, South Africa", "Bonnaroo, USA",
            "Burning Man, USA", "Katmandu, Nepal", "Dublin, Ireland", "Copenhagen, Denmark", "Miami, USA",
            "Barcelona, Spain", "Phuket, Thailand", "Manchester, UK", "Montreal, Canada", "Salvador, Brazil",
            "Detroit, USA", "Lisbon, Portugal", "Belfast, Ireland", "Tel Aviv, Israel", "Santo Domingo, Dominican Republic",
            "Glasgow, Scotland", "Hamburg, Germany", "La Paz, Bolivia", "Nairobi, Kenya", "Buenos Aires, Argentina",
            "Chiang Mai, Thailand", "Leeds, UK", "Orlando, USA", "Accra, Ghana", "Sturgis, USA",
            "Sao Paulo, Brazil", "Kyoto, Japan", "Lagos, Nigeria", "Liverpool, UK", "Boulder, USA",
            "Porto, Portugal", "Jakarta, Indonesia", "Sofia, Bulgaria", "Naples, Italy", "Halifax, Canada",
            "Cartagena, Colombia", "Lima, Peru", "Birmingham, UK", "Tbilisi, Georgia", "San Juan, Puerto Rico",
            "Oaxaca, Mexico", "Cardiff, Wales", "Valencia, Spain", "Belgrade, Serbia", "Quito, Ecuador",
            "Adelaide, Australia", "Oslo, Norway", "Santiago, Chile", "Brisbane, Australia", "Hanoi, Vietnam",
            "Riga, Latvia", "Zagreb, Croatia", "Lviv, Ukraine", "Asheville, USA", "Sheffield, UK",
            "Montevideo, Uruguay", "Anchorage, USA", "Skopje, North Macedonia", "Darwin, Australia", "Ulaanbaatar, Mongolia"
        ];

        const funFacts = {
            "Amsterdam, Netherlands": ["Weed’s her jam!", "She rides bikes drunk!"],
            "Ibiza, Spain": ["Raves all night!", "She’s the hippie queen!"],
            "Las Vegas, USA": ["Slots and shots!", "Morgan married a stranger!"],
            "Bangkok, Thailand": ["Neon chaos queen!", "Spicy food’s her fuel!"],
            "Woodstock, USA": ["Mud-dancing diva!", "She’s stuck in ‘69!"]
        };

        let morganLocation = places[Math.floor(Math.random() * places.length)];
        let questionCount = 0;
        let score = 0;
        let responses = [];
        let gameCount = 0;
        let allGames = [];

        function startGame() {
            questionCount = 0;
            score = 0;
            responses = [];
            morganLocation = places[Math.floor(Math.random() * places.length)];
            document.getElementById('result-popup').style.display = 'none';
            document.getElementById('intro').style.display = 'block';
            document.getElementById('question').style.display = 'block';
            document.getElementById('options').style.display = 'block';
            askQuestion();
        }

        function askQuestion() {
            if (questionCount < 5) {
                questionCount++;
                document.getElementById('question').textContent = `Q${questionCount}: Where’s Morgan shredding it?`;
                const options = getRandomOptions();
                const optionsDiv = document.getElementById('options');
                optionsDiv.innerHTML = '';
                options.forEach(opt => {
                    const btn = document.createElement('button');
                    btn.textContent = opt;
                    btn.onclick = () => checkAnswer(opt);
                    optionsDiv.appendChild(btn);
                });
            } else {
                endRound(false);
            }
        }

        function getRandomOptions() {
            let opts = [];
            while (opts.length < 4) {
                const place = places[Math.floor(Math.random() * places.length)];
                if (!opts.includes(place)) opts.push(place);
            }
            if (!opts.includes(morganLocation)) {
                opts[Math.floor(Math.random() * 4)] = morganLocation;
            }
            return opts;
        }

        function checkAnswer(guess) {
            responses.push({ question: questionCount, guess: guess, correct: guess === morganLocation });
            if (guess === morganLocation) {
                score += 20;
                playBuzz(true);
                endRound(true);
            } else {
                playBuzz(false);
                alert("Nope, she’s still raging somewhere else!");
                askQuestion();
            }
        }

        function endRound(isWin) {
            const popup = document.getElementById('result-popup');
            const title = document.getElementById('result-title');
            const text = document.getElementById('result-text');
            const facts = document.getElementById('fun-facts');
            const review = document.getElementById('review');
            const reward = document.getElementById('reward');
            const confetti = document.getElementById('confetti');
            const nextButton = document.getElementById('next-button');

            allGames.push({ round: gameCount + 1, responses: [...responses], score: score, location: morganLocation });

            if (isWin) {
                title.textContent = "YES, YOU FOUND MORGAN!\nBUT SHE’S OFF PARTYING AGAIN!";
                title.className = 'win';
                text.textContent = `She was WILD in ${morganLocation}!`;
                reward.textContent = "Reward: A glitter bomb worth 100 points!";
                confetti.innerHTML = '';
                for (let i = 0; i < 100; i++) {
                    const piece = document.createElement('div');
                    piece.className = 'confetti-piece';
                    piece.style.left = Math.random() * 100 + 'vw';
                    piece.style.background = ['#ff00ff', '#00ffff', '#00ff00', '#ffff00', '#ff9900'][Math.floor(Math.random() * 5)];
                    piece.style.animationDelay = Math.random() * 1.5 + 's';
                    piece.style.width = Math.random() * 15 + 5 + 'px';
                    piece.style.height = piece.style.width;
                    confetti.appendChild(piece);
                }
                nextButton.style.display = 'block';
            } else {
                title.textContent = "MORGAN OUTPARTIED YOU!";
                title.className = 'lose';
                text.textContent = `She was rocking ${morganLocation}!`;
                reward.textContent = "Reward: A soggy glowstick worth 10 points.";
                confetti.innerHTML = '';
                nextButton.style.display = 'block';
            }

            facts.textContent = "Morgan’s Chaos:\n" + 
                (funFacts[morganLocation] || ["She’s a total party freak!", "Rock ‘n’ roll is her soul!"]).join('\n');

            review.innerHTML = "This Round:<br>" + responses.map(r => 
                `Q${r.question}: ${r.guess} - ${r.correct ? '<span style="color: #00ff00">BOOM!</span>' : '<span style="color: #ff0000">NOPE!</span>'}`
            ).join('<br>') + `<br>Score: ${score}/100`;

            popup.style.display = 'block';
            gameCount++;

            if (gameCount === 3) {
                setTimeout(showSuperReview, 3000);
            }
        }

        function nextRound() {
            if (gameCount < 3) {
                startGame();
            }
        }

        function showSuperReview() {
            const popup = document.getElementById('result-popup');
            const title = document.getElementById('result-title');
            const text = document.getElementById('result-text');
            const facts = document.getElementById('fun-facts');
            const review = document.getElementById('review');
            const reward = document.getElementById('reward');
            const nextButton = document.getElementById('next-button');

            title.textContent = "MORGAN’S THREE-ROUND RAGER!";
            title.className = 'win';
            text.textContent = "Here’s your epic party chase recap!";
            facts.textContent = "";
            review.innerHTML = allGames.map(game => 
                `<br>Round ${game.round} - ${game.location}:<br>` + 
                game.responses.map(r => 
                    `Q${r.question}: ${r.guess} - ${r.correct ? '<span style="color: #00ff00">BOOM!</span>' : '<span style="color: #ff0000">NOPE!</span>'}`
                ).join('<br>') + `<br>Score: ${game.score}/100`
            ).join('<br>');
            reward.textContent = `Total Loot: ${allGames.reduce((sum, g) => sum + (g.score >= 20 ? 100 : 10), 0)} points!`;
            nextButton.textContent = "Start Over!";
            nextButton.onclick = () => { gameCount = 0; allGames = []; startGame(); };
            popup.style.display = 'block';
        }

        function playBuzz(win = true) {
            const audio = new Audio('data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEARKwAAIhYAQACABAAZGF0YQAAAAA=');
            audio.play();
        }

        window.onload = startGame;
    </script>
</body>
</html>
