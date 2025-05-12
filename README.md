OCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>For Anjali 💕</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #fff0f5;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      overflow: hidden;
    }

    .scene {
      width: 90%;
      max-width: 400px;
      height: 250px;
      perspective: 1000px;
      position: relative;
    }

    .card {
      width: 100%;
      height: 100%;
      position: relative;
      transform-style: preserve-3d;
      transition: transform 0.8s;
      cursor: pointer;
    }

    .card.flipped {
      transform: rotateY(180deg);
    }

    .card-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      background: #fff;
      border-radius: 16px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 20px;
      font-size: 1.1em;
      line-height: 1.5em;
      text-align: center;
    }

    .card-front {
      background-color: #ff69b4;
      color: white;
      font-weight: bold;
      font-size: 1.2em;
    }

    .card-back {
      transform: rotateY(180deg);
      background-color: #fff;
      color: #333;
    }

    .hint {
      position: absolute;
      bottom: 40px;
      font-size: 0.9em;
      color: #888;
      text-align: center;
      width: 100%;
    }

    .hearts {
      position: absolute;
      width: 100%;
      height: 100%;
      pointer-events: none;
      overflow: hidden;
    }

    .heart {
      position: absolute;
      width: 20px;
      height: 20px;
      background: red;
      transform: rotate(45deg);
      animation: float 1s ease-out forwards;
    }

    .heart::before,
    .heart::after {
      content: '';
      position: absolute;
      width: 20px;
      height: 20px;
      background: red;
      border-radius: 50%;
    }

    .heart::before {
      top: -10px;
      left: 0;
    }

    .heart::after {
      left: -10px;
      top: 0;
    }

    @keyframes float {
      0% {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
      100% {
        opacity: 0;
        transform: translateY(-100px) scale(1.5);
      }
    }
  </style>
</head>
<body>

  <div class="scene" onclick="flipCard(event)">
    <div class="card" id="card">
      <div class="card-face card-front">
        <span id="frontLine">Loading...</span>
      </div>
      <div class="card-face card-back" id="quoteBack">
        <!-- Quote will go here -->
      </div>
    </div>
    <div class="hearts" id="hearts"></div>
  </div>

  <div class="hint">Tap the card to flip & see a new quote</div>

  <audio id="flipSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_b7d8bcba87.mp3?filename=flip-2-6416.mp3" preload="auto"></audio>
  <audio id="bgMusic" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_e5a6525b3b.mp3?filename=romantic-background-music-11392.mp3" autoplay loop></audio>

  <script>
    const quotes = [
      "Every day you show the world what it means to be strong and gentle at once.",
      "Your heart is the kind that heals, even in its quietest moments.",
      "Your love is not just felt, it is lived in every action you take.",
      "You deserve every ounce of happiness that comes your way and more.",
      "You have a way of making others feel seen, even when they don’t speak.",
      "Your presence is a gift the world could never take for granted.",
      "Every challenge you face becomes a testament to your strength.",
      "The love you give the world is immeasurable, and yet, it always feels infinite.",
      "You have a way of making everyone around you feel valued, heard, and understood.",
      "You don’t just shine; you make everyone around you feel like they belong to your light.",
      "You have the rarest gift—caring without limits, loving without conditions.",
      "There’s a quiet power in your grace that makes you unforgettable.",
      "The way you hold space for others makes them feel like they matter, always.",
      "Your heart is full of compassion, and yet, you always put others first.",
      "You make life feel worth living just by being in it.",
      "You deserve to be seen, just as much as you see others.",
      "There’s a magic in the way you bring calm to chaos.",
      "What you do changes lives, even when you don’t realize it.",
      "You bring warmth to every room, even in your quiet moments.",
      "Your strength is like the foundation of a home—steady, reliable, and always there.",
      "You are loved more than words could ever express.",
      "Every gesture you make speaks louder than any words could ever say.",
      "Your kindness is a gift that keeps giving, to everyone you meet.",
      "You deserve to be cherished as much as you cherish the world.",
      "You are a treasure, even in your humblest moments.",
      "The world is a better place because you are in it.",
      "You are worthy of every dream you chase and every goal you reach.",
      "There is nothing about you that isn't worth celebrating.",
      "In your eyes, the world is a little softer, a little kinder.",
      "You make people believe in the goodness of humanity.",
      "Even in your quiet, you leave a mark that words can’t capture.",
      "Your love makes everyone feel like they are enough, just as they are.",
      "You have a rare way of turning every moment into something meaningful.",
      "You deserve the world, because the world is a better place with you in it.",
      "You have a heart so big, it could fill the whole world.",
      "What you do is important, but who you are is extraordinary.",
      "You make kindness look effortless, but it’s the most powerful thing about you.",
      "Every day you become more of the person everyone needs in their life.",
      "The love you give isn’t just given—it’s felt, deeply and beautifully.",
      "In a world that often overlooks, you always see the best in others.",
      "You deserve to be celebrated, not just today, but every day.",
      "You have a way of making others feel like they matter, simply by being you.",
      "Your worth isn’t measured in accolades, but in the lives you touch.",
      "Your presence is a reminder that the world is still full of goodness.",
      "You deserve every piece of love that comes your way—and so much more.",
      "You are a light, and the world needs you to shine as brightly as you do.",
      "In your care, everyone feels seen, valued, and cherished.",
      "Your love is a constant reminder that there is beauty in everything.",
      "You give others strength, even when you don’t realize your own.",
      "You make every moment feel like it’s full of possibility.",
      "You’re deserving of everything you dream, and everything you deserve.",
      "The world needs more souls like you—steady, compassionate, and beautifully you."
    ];

    const flirtyLines = [
      "Anjali, this card’s got ✨rizz✨... but not as much as you.",
      "Anjali, if this card had game, it’d still be losing to you.",
      "Lowkey? This quote’s cute. Highkey? You’re cuter.",
      "Anjali, this flip ain't half as dramatic as your smile.",
      "Swipe right on this card... just like I did on you.",
      "Anjali, if charm were currency, you'd be a billionaire.",
      "This card’s lucky—Anjali just tapped it.",
      "Flipping cards > flipping feelings, but you make both look easy.",
      "Anjali, this card’s smooth... but you? Smoother.",
      "If blushing was a sport, I’d be in the Olympics every time you smile.",
      "Anjali, even my WiFi connects slower than I connected with you.",
      "Your smile could outshine my entire explore page.",
      "Anjali, I’d swipe up on your stories just to say hi.",
      "If love was a trend, you'd be the one setting it.",
      "Anjali, you’re the whole vibe. This card’s just catching up.",
      "You make my heart skip like a laggy livestream.",
      "Anjali, you’re not just a 10. You’re the whole algorithm.",
      "This card flips once. I’ve been flipping for you daily.",
      "You’re the caption I could never top.",
      "Anjali, my heart has zero chill when it’s you.",
      "Netflix who? I'd rather binge-watch you smile.",
      "If I had a dollar for every time I thought of you, I'd be rich and still simping.",
      "Anjali, you could ghost me and I'd still say “wyd?”",
      "You’re the main character. I’m just here catching feelings.",
      "Anjali, if looks could trend, you’d break the internet.",
      "You’re the type of girl that makes me want to text first.",
      "Just like this card, you always keep me guessing.",
      "You're not even WiFi but I feel a strong connection.",
      "Anjali, your laugh > every playlist I’ve ever made.",
      "You’re the update my heart didn’t know it needed.",
      "This card’s cute, but you make everything aesthetic.",
      "If I was a filter, I’d still choose to be seen by you.",
      "Anjali, your energy is full send material.",
      "You’re the “you up?” at 2am and the “good morning :)” at 9.",
      "You’ve got me soft like a lo-fi beat at midnight.",
      "Anjali, I’d repost you on my private story and main.",
      "You’re the plot twist my heart didn’t see coming.",
      "This isn’t a crush, it’s a full-blown rom-com.",
      "Anjali, my screen time’s gone up just thinking of you.",
      "Even if I get zero likes, I’d still double-tap on you.",
      "You walked in like a vibe and left me in my feels.",
      "Anjali, your name’s the only notification I want.",
      "I had a thought... and it was you. Again.",
      "Anjali, you're not just the moment—you’re the whole season.",
      "This card flips once. My thoughts about you? On loop.",
      "You’re not a filter but you light up everything.",
      "I’d match your energy if I wasn’t already busy falling for it.",
      "If texting you was cardio, I’d be shredded.",
      "Anjali, your energy’s got me pressing 'send' way too fast.",
      "You're the dream I didn’t know I was awake for.",
      "Even my for you page wants me to simp for you.",
      "Anjali, this card’s romantic, but you're iconic."
    ];

    let flipped = false;

    function flipCard(event) {
      const card = document.getElementById("card");
      const back = document.getElementById("quoteBack");
      const front = document.getElementById("frontLine");
      const sound = document.getElementById("flipSound");

      if (!flipped) {
        const randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
        back.textContent = randomQuote;
        card.classList.add("flipped");
        flipped = true;
      } else {
        const randomLine = flirtyLines[Math.floor(Math.random() * flirtyLines.length)];
        front.textContent = randomLine;
        card.classList.remove("flipped");
        flipped = false;
      }

      sound.currentTime = 0;
      sound.play();
      burstHearts(event.clientX, event.clientY);
    }

    function burstHearts(x, y) {
      const container = document.getElementById("hearts");
      for (let i = 0; i < 6; i++) {
        const heart = document.createElement("div");
        heart.className = "heart";
        heart.style.left = `${Math.random() * 100}%`;
        heart.style.top = "100%";
        heart.style.animationDuration = `${0.8 + Math.random()}s`;
        container.appendChild(heart);
        setTimeout(() => container.removeChild(heart), 1000);
      }
    }

    window.onload = () => {
      const front = document.getElementById("frontLine");
      front.textContent = flirtyLines[Math.floor(Math.random() * flirtyLines.length)];
    };
  </script>

</body>
</html>
