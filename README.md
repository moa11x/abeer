<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Abeer AR Experience</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
    <script src="https://raw.githack.com/fcor/aframe-gestures/master/dist/gestures.js"></script>

    <style>
        #overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, #1a1a1a 0%, #000000 100%);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 9999; color: white; font-family: sans-serif; text-align: center;
        }
        .brand-name { font-size: 40px; font-weight: bold; letter-spacing: 8px; color: #ff0055; margin: 0; }
        button {
            padding: 15px 40px; font-size: 18px; cursor: pointer; border-radius: 30px;
            border: 2px solid #ff0055; background: transparent; color: white; margin-top: 20px;
        }
        /* تنبيه للمستخدم */
        #ar-instruction {
            position: fixed; bottom: 20px; width: 100%; text-align: center;
            color: white; z-index: 100; display: none; font-family: sans-serif;
            text-shadow: 1px 1px 2px black;
        }
    </style>
</head>

<body style="margin: 0; overflow: hidden;">

    <div id="overlay">
        <p class="brand-name">ABEER</p>
        <p style="color: #888;">Interactive AR Cone</p>
        <button onclick="startAR()">ابدأ العرض</button>
    </div>

    <div id="ar-instruction">وجهي الكاميرا حولك وسيظهر المخروط</div>

    <a-scene 
        embedded 
        arjs="sourceType: webcam; debugUIEnabled: false; trackingMethod: best;"
        vr-mode-ui="enabled: false"
        gesture-detector>

        <a-entity light="type: ambient; intensity: 0.8;"></a-entity>
        
        <a-entity 
            id="cone-wrapper" 
            position="0 0 -5" 
            gesture-handler>
            <a-cone 
                class="clickable"
                radius-bottom="0.5" 
                radius-top="0" 
                height="1.2" 
                material="color: #ff0055; emissive: #330011;"
                animation="property: rotation; to: 0 360 0; loop: true; dur: 8000">
            </a-cone>
        </a-entity>

        <a-entity camera></a-entity>
    </a-scene>

    <script>
        function startAR() {
            document.getElementById('overlay').style.display = 'none';
            document.getElementById('ar-instruction').style.display = 'block';
            
            // طلب إذن الوصول للحساسات (مهم جداً للأيفون)
            if (typeof DeviceMotionEvent !== 'undefined' && typeof DeviceMotionEvent.requestPermission === 'function') {
                DeviceMotionEvent.requestPermission()
                    .then(response => {
                        if (response == 'granted') {
                            console.log("Permission granted");
                        }
                    })
                    .catch(console.error);
            }
        }
    </script>
</body>
</html>
