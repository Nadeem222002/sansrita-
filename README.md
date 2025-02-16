<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Sansrita!</title>
    <style>
        body {
            text-align: center;
            background: linear-gradient(135deg, #ff6b6b, #ffa502);
            color: #fff;
            font-family: 'Comic Sans MS', cursive;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }
        h1 {
            font-size: 3em;
            margin: 20px 0;
            animation: heartbeat 2s infinite;
        }
        @keyframes heartbeat {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        p {
            font-size: 1.5em;
            margin: 10px 0;
        }
        .candle {
            width: 20px;
            height: 100px;
            background: orange;
            margin: 20px auto;
            border-radius: 5px;
            position: relative;
            animation: flicker 2s infinite;
        }
        .flame {
            width: 10px;
            height: 20px;
            background: yellow;
            border-radius: 50%;
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            animation: flameFlicker 1s infinite;
        }
        @keyframes flameFlicker {
            0% { transform: translateX(-50%) scale(1); }
            50% { transform: translateX(-50%) scale(1.2); }
            100% { transform: translateX(-50%) scale(1); }
        }
        #blowMsg {
            margin-top: 20px;
            font-size: 1.2em;
        }
    </style>
</head>
<body>
    <h1>🎂 Happy Birthday, Sansrita! 🎉</h1>
    <div class="candle">
        <div class="flame"></div>
    </div>
    <p id="blowMsg">Blow out the candle to start the surprise! 🎤</p>

    <audio id="birthdaySong" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" preload="auto"></audio>

    <script>
        function listenForBlow() {
            navigator.mediaDevices.getUserMedia({ audio: true }).then(function (stream) {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const source = audioContext.createMediaStreamSource(stream);
                const analyser = audioContext.createAnalyser();
                source.connect(analyser);
                analyser.fftSize = 256;

                const bufferLength = analyser.frequencyBinCount;
                const dataArray = new Uint8Array(bufferLength);

                function detectBlow() {
                    analyser.getByteFrequencyData(dataArray);
                    let sum = dataArray.reduce((a, b) => a + b, 0);

                    if (sum > 5000) {
                        blowOutCandle();
                    } else {
                        requestAnimationFrame(detectBlow);
                    }
                }
                detectBlow();
            }).catch(function (err) {
                alert('Microphone access is required to blow out the candle!');
            });
        }

        function blowOutCandle() {
            document.querySelector('.flame').style.display = 'none';
            document.getElementById('blowMsg').innerText = '🎉 Candle blown! Happy Birthday, Sansrita! 🎂';
            document.getElementById('birthdaySong').play();

            setTimeout(() => {
                alert('🎈 Happy Birthday, Sansrita! May your day be as amazing as you are! 🎊');
            }, 2000);
        }

        listenForBlow();
    </script>
</body>
</html>
