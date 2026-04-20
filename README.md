<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Abeer AR Project</title>
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/mind-ar@1.1.4/dist/mindar-image-aframe.prod.js"></script>
    
    <style>
        #overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #000; display: flex; flex-direction: column;
            justify-content: center; align-items: center; z-index: 999;
            color: white; font-family: sans-serif;
        }
        .abeer-text { font-size: 45px; color: #ff0055; letter-spacing: 5px; font-weight: bold; }
        button {
            margin-top: 20px; padding: 15px 40px; font-size: 18px;
            background: #ff0055; color: white; border: none; border-radius: 50px;
        }
    </style>
</head>
<body>

    <div id="overlay">
        <div class="abeer-text">ABEER</div>
        <p>تصميم واقع معزز تفاعلي</p>
        <button id="startButton">ابدأ العرض</button>
    </div>

    <a-scene mindar-image="imageTargetSrc: https://cdn.jsdelivr.net/npm/mind-ar@1.1.4/examples/image-tracking/assets/card-example/card.mind;" 
             embedded color-space="sRGB" renderer="colorManagement: true, physicallyCorrectLights" 
             vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
        
        <a-camera position="0 0 0" look-controls="enabled: false"></a-camera>

        <a-entity id="target" position="0 0 -1">
            <a-cone 
                id="cone"
                radius-bottom="0.2" 
                radius-top="0" 
                height="0.5" 
                material="color: #ff0055; metalness: 0.6; roughness: 0.2"
                animation="property: rotation; to: 0 360 0; loop: true; dur: 5000">
            </a-cone>
        </a-entity>
    </a-scene>

    <script>
        const startButton = document.getElementById('startButton');
        const overlay = document.getElementById('overlay');
        const cone = document.getElementById('cone');

        startButton.addEventListener('click', () => {
            overlay.style.display = 'none';
            // تشغيل الصوت أو الحساسات إذا لزم الأمر
        });

        // البرمجة الخاصة بالتحكم باللمس (التحريك بالأصابع)
        let isDragging = false;
        let previousMousePosition = { x: 0, y: 0 };

        window.addEventListener('touchstart', (e) => {
            isDragging = true;
        });

        window.addEventListener('touchend', (e) => {
            isDragging = false;
        });

        window.addEventListener('touchmove', (e) => {
            if (isDragging) {
                let deltaX = e.touches[0].pageX - previousMousePosition.x;
                let deltaY = e.touches[0].pageY - previousMousePosition.y;

                // تحريك المخروط بناءً على حركة الأصبع
                cone.object3D.rotation.y += deltaX * 0.01;
                cone.object3D.rotation.x += deltaY * 0.01;
            }
            previousMousePosition = {
                x: e.touches[0].pageX,
                y: e.touches[0].pageY
            };
        });
    </script>
</body>
</html>
