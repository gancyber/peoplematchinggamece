# peoplematchinggamece
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Matching</title>
    <style>
        body {
            background-color: lightblue;
            text-align: center;
            font-family: Arial, sans-serif;
        }
        h1 {
            font-size: 36px;
        }
        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .row {
            display: flex;
            justify-content: center;
            margin: 10px 0;
        }
        .image-box img {
            width: 100px;
            height: 100px;
            cursor: pointer;
            border: 3px solid black;
            border-radius: 10px;
            padding: 5px;
            background-color: white;
        }
        .word-box {
            width: 120px;
            height: 50px;
            background: rgba(0, 128, 0, 0.7);
            color: white;
            font-size: 18px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            border-radius: 10px;
            box-shadow: 2px 2px 5px gray;
        }
        .shake {
            animation: shake 0.5s;
        }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25%, 75% { transform: translateX(-5px); }
            50% { transform: translateX(5px); }
        }
        .hidden {
            display: none;
        }
        .result {
            font-size: 32px;
            font-weight: bold;
            color: gold;
        }
        .trophy {
            width: 100px;
            height: 100px;
        }
        .time {
            font-size: 24px;
            color: darkblue;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1 id="title">Matching</h1>
    <div class="game-container">
        <div class="row" id="images"></div>
        <div class="row" id="words"></div>
    </div>
    <div id="result" class="hidden">
        <div class="result">Excellent! Very Good!</div>
        <img src="trophy.png" class="trophy" alt="Trophy">
        <div id="time-display" class="time"></div>
    </div>
    
    <script>
        const words = ["teacher", "student", "friend", "police", "engineer", "pilot", "soldier", "doctor"];
        let selectedImage = null;
        let selectedWord = null;
        let startTime = Date.now();

        function createGameElements() {
            const imageContainer = document.getElementById("images");
            const wordContainer = document.getElementById("words");
            
            words.forEach(word => {
                let img = document.createElement("img");
                img.src = word + ".png";
                img.dataset.word = word;
                img.classList.add("image-box");
                img.onclick = () => selectImage(img);
                imageContainer.appendChild(img);

                let wordBox = document.createElement("div");
                wordBox.textContent = word;
                wordBox.dataset.word = word;
                wordBox.classList.add("word-box");
                wordBox.onclick = () => selectWord(wordBox);
                wordContainer.appendChild(wordBox);
            });
        }
        
        function selectImage(img) {
            if (selectedImage) return;
            selectedImage = img;
            img.style.border = "3px solid red";
        }
        
        function selectWord(wordBox) {
            if (!selectedImage) return;
            selectedWord = wordBox;
            
            if (selectedImage.dataset.word === selectedWord.dataset.word) {
                correctMatch();
            } else {
                incorrectMatch();
            }
        }

        function correctMatch() {
            playSound("correct.mp3");
            selectedImage.remove();
            selectedWord.remove();
            selectedImage = null;
            selectedWord = null;
            checkWin();
        }

        function incorrectMatch() {
            playSound("wrong.mp3");
            selectedImage.classList.add("shake");
            selectedWord.classList.add("shake");
            setTimeout(() => {
                selectedImage.classList.remove("shake");
                selectedWord.classList.remove("shake");
                selectedImage.style.border = "3px solid black";
                selectedImage = null;
                selectedWord = null;
            }, 1000);
        }
        
        function checkWin() {
            if (document.getElementById("images").children.length === 0) {
                let endTime = Date.now();
                let elapsedTime = ((endTime - startTime) / 1000).toFixed(2);
                document.getElementById("title").classList.add("hidden");
                document.getElementById("result").classList.remove("hidden");
                document.getElementById("time-display").textContent = `Time taken: ${elapsedTime} seconds`;
                playSound("win.mp3");
            }
        }
        
        function playSound(filename) {
            let audio = new Audio(filename);
            audio.play();
        }

        createGameElements();
    </script>
</body>
</html>
