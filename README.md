# week-3
Use http and user interface to convert voice to text


file:///C:/Users/WinDows/OneDrive/Desktop/Hessa/htdocs/robot_control/speech_to_text.html




http://localhost/phpmyadmin/index.php?route=/table/structure&db=speech_to_text_db&table=texts




http://localhost/my_project/create_database.php/




file:///C:/Users/WinDows/OneDrive/Desktop/Hessa/htdocs/robot_control/speech_to_text.html







<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحويل الصوت إلى نص</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f9;
        }
        .container {
            text-align: center;
            background: #fff;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            border-radius: 8px;
        }
        button {
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            background-color: #007bff;
            color: #fff;
            border-radius: 5px;
        }
        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        textarea {
            width: 100%;
            height: 150px;
            margin-top: 10px;
            padding: 10px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
            resize: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>تحويل الصوت إلى نص</h1>
        <button id="startButton">ابدأ التسجيل</button>
        <button id="stopButton" disabled>إيقاف التسجيل</button>
        <textarea id="textOutput" placeholder="النص المحول سيظهر هنا..."></textarea>
    </div>

    <script>
        const startButton = document.getElementById('startButton');
        const stopButton = document.getElementById('stopButton');
        const textOutput = document.getElementById('textOutput');

        let recognition;

        if ('webkitSpeechRecognition' in window) {
            recognition = new webkitSpeechRecognition();
            recognition.lang = 'ar-SA'; // ضبط اللغة إلى العربية
            recognition.continuous = true;
            recognition.interimResults = true;

            recognition.onstart = function() {
                startButton.disabled = true;
                stopButton.disabled = false;
            };

            recognition.onresult = function(event) {
                let interimTranscript = '';
                let finalTranscript = '';

                for (let i = 0; i < event.results.length; i++) {
                    let transcript = event.results[i][0].transcript;
                    if (event.results[i].isFinal) {
                        finalTranscript += transcript;
                    } else {
                        interimTranscript += transcript;
                    }
                }

                textOutput.value = finalTranscript + interimTranscript;
            };

            recognition.onerror = function(event) {
                console.error('Error occurred in recognition: ' + event.error);
            };

            recognition.onend = function() {
                startButton.disabled = false;
                stopButton.disabled = true;
            };
        } else {
            alert('متصفحك لا يدعم Web Speech API');
        }

        startButton.addEventListener('click', () => {
            recognition.start();
        });

        stopButton.addEventListener('click', () => {
            recognition.stop();
        });
    </script>
</body>
</html>






