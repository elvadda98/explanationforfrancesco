<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Flashcards</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
        }
        
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
            font-size: 2.2em;
        }
        
        .flashcard-container {
            perspective: 1000px;
            margin-bottom: 30px;
        }
        
        .flashcard {
            width: 100%;
            height: 300px;
            position: relative;
            transform-style: preserve-3d;
            transition: all 0.5s ease;
            cursor: pointer;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        
        .flashcard.flipped {
            transform: rotateY(180deg);
        }
        
        .flashcard-front, .flashcard-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
        }
        
        .flashcard-front {
            background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
            color: white;
        }
        
        .flashcard-back {
            background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%);
            color: white;
            transform: rotateY(180deg);
        }
        
        .word {
            font-size: 3em;
            font-weight: bold;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }
        
        .example {
            font-size: 1.3em;
            margin-bottom: 15px;
            line-height: 1.5;
        }
        
        .definition {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 20px;
        }
        
        .image-icon {
            font-size: 4em;
            margin-bottom: 20px;
        }
        
        .navigation {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        
        .nav-btn {
            padding: 12px 25px;
            border: none;
            border-radius: 30px;
            background: linear-gradient(135deg, #ff758c 0%, #ff7eb3 100%);
            color: white;
            font-weight: bold;
            cursor: pointer;
            font-size: 1em;
            box-shadow: 0 5px 15px rgba(255,117,140,0.3);
            transition: all 0.3s ease;
        }
        
        .nav-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 7px 20px rgba(255,117,140,0.4);
        }
        
        .nav-btn:disabled {
            background: #cccccc;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .progress {
            text-align: center;
            margin-top: 20px;
            font-size: 1.1em;
            color: #2c3e50;
            font-weight: bold;
        }
        
        @media (max-width: 600px) {
            .word {
                font-size: 2em;
            }
            
            .definition {
                font-size: 1.2em;
            }
            
            .example {
                font-size: 1em;
            }
            
            .flashcard {
                height: 250px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📚 Vocabulary Flashcards</h1>
        
        <div class="progress">Card <span id="current-card">1</span> of <span id="total-cards">10</span></div>
        
        <div class="flashcard-container">
            <div class="flashcard" id="flashcard" onclick="flipCard()">
                <div class="flashcard-front">
                    <div class="word" id="word">some</div>
                    <div class="example" id="example">"Would you like <u>some</u> water?"</div>
                    <div class="image-icon">🔤</div>
                    <p>Click to flip</p>
                </div>
                <div class="flashcard-back">
                    <div class="definition" id="definition">An unspecified amount or number</div>
                    <div class="example" id="back-example">"I have <u>some</u> apples in my bag."</div>
                    <div class="image-icon">💡</div>
                    <p>Click to flip back</p>
                </div>
            </div>
        </div>
        
        <div class="navigation">
            <button class="nav-btn" id="prev-btn" onclick="prevCard()" disabled>← Previous</button>
            <button class="nav-btn" id="next-btn" onclick="nextCard()">Next →</button>
        </div>
    </div>

    <script>
        const flashcards = [
            {
                word: "some",
                definition: "An unspecified amount or number",
                example: "Would you like some water?",
                backExample: "I have some apples in my bag."
            },
            {
                word: "shy",
                definition: "Nervous or uncomfortable with other people",
                example: "The new student is too shy to speak.",
                backExample: "She was shy as a child but is more confident now."
            },
            {
                word: "mainly",
                definition: "Mostly or usually",
                example: "I eat mainly vegetables for lunch.",
                backExample: "This book is mainly about animals."
            },
            {
                word: "meet",
                definition: "To come together with someone",
                example: "Let's meet at the café at 3pm.",
                backExample: "I'll meet you after school tomorrow."
            },
            {
                word: "going well",
                definition: "Progressing successfully",
                example: "How is your project going? - It's going well!",
                backExample: "Everything was going well until it started raining."
            },
            {
                word: "between",
                definition: "In the space separating two things",
                example: "The cat is sitting between the two chairs.",
                backExample: "There's a small table between the sofa and the wall."
            },
            {
                word: "raw",
                definition: "Not cooked",
                example: "Sushi often has raw fish in it.",
                backExample: "Be careful not to eat raw meat."
            },
            {
                word: "without",
                definition: "Not having something",
                example: "I drink tea without sugar.",
                backExample: "She left without saying goodbye."
            },
            {
                word: "average",
                definition: "Typical or normal, not special",
                example: "His grades are average in the class.",
                backExample: "The average temperature in July is 25°C."
            },
            {
                word: "quiet",
                definition: "Making little or no noise",
                example: "Please be quiet in the library.",
                backExample: "It's very quiet early in the morning."
            }
        ];

        let currentCardIndex = 0;
        const totalCards = flashcards.length;
        const flashcard = document.getElementById('flashcard');
        
        document.getElementById('total-cards').textContent = totalCards;
        
        function updateCard() {
            const card = flashcards[currentCardIndex];
            document.getElementById('word').textContent = card.word;
            document.getElementById('example').innerHTML = `"${card.example.replace(card.word, `<u>${card.word}</u>`)}"`;
            document.getElementById('definition').textContent = card.definition;
            document.getElementById('back-example').innerHTML = `"${card.backExample.replace(card.word, `<u>${card.word}</u>`)}"`;
            document.getElementById('current-card').textContent = currentCardIndex + 1;
            
            // Reset card to front when changing
            if (flashcard.classList.contains('flipped')) {
                flashcard.classList.remove('flipped');
            }
            
            // Update button states
            document.getElementById('prev-btn').disabled = currentCardIndex === 0;
            document.getElementById('next-btn').disabled = currentCardIndex === totalCards - 1;
        }
        
        function flipCard() {
            flashcard.classList.toggle('flipped');
        }
        
        function nextCard() {
            if (currentCardIndex < totalCards - 1) {
                currentCardIndex++;
                updateCard();
            }
        }
        
        function prevCard() {
            if (currentCardIndex > 0) {
                currentCardIndex--;
                updateCard();
            }
        }
        
        // Initialize first card
        updateCard();
        
        // Keyboard navigation
        document.addEventListener('keydown', function(event) {
            if (event.key === 'ArrowRight') {
                nextCard();
            } else if (event.key === 'ArrowLeft') {
                prevCard();
            } else if (event.key === ' ' || event.key === 'Enter') {
                flipCard();
            }
        });
    </script>
</body>
</html>
