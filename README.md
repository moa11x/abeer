<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>عرض واقع معزز</title>
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
    <script src="https://raw.githack.com/donmccurdy/aframe-extras/master/dist/aframe-extras.min.js"></script>
    
    <style>
        /* تصميم زر البداية */
        #overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            display: flex; justify-content: center; align-items: center;
            z-index: 9999;
        }
        button {
            padding: 15px 30px;
            font-size: 20px;
            cursor: pointer;
            border-radius: 10px;
            border: none;
            background-color: #007bff;
            color: white;
        }
    </style>
</head>

<body style="margin: 0; overflow: hidden;">

    <div id="overlay">
        <button onclick="startAR()">ابدأ العرض</button>
    </div>

    <a-scene embedded arjs="sourceType: webcam; debugUIEnabled: false;" vr-mode-ui="enabled: false">
        
        <a-marker-camera preset="hiro">
            <a-box id="box" position="0 0.5 0" material="color: orange;" 
                   animation="property: rotation; to: 0 360 0; loop: true; dur: 5000">
            </a-box>
        </a-marker-camera>

    </a-scene>

    <script>
        function startAR() {
            // إخفاء الزر عند الضغط
            document.getElementById('overlay').style.display = 'none';
            // تشغيل الصوت أو الكاميرا إذا لزم الأمر في بعض المتصفحات
        }

        // كود بسيط للتحكم بالحركة عبر اللمس (اختياري)
        let box = document.querySelector('#box');
        window.addEventListener('touchstart', (e) => {
            // هنا تقدر تضيف منطق لتحريك المربع باللمس
            box.setAttribute('material', 'color', 'red'); // مثال: يتغير لونه عند اللمس
        });
    </script>
</body>
</html>
