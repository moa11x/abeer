<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="utf-8">
    <title>AR Experience by Abeer</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
    <script src="https://raw.githack.com/fcor/aframe-gestures/master/dist/gestures.js"></script>

    <style>
        /* تصميم واجهة البداية الخاصة بـ عبير */
        #overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, #1a1a1a 0%, #000000 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            color: white;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            text-align: center;
        }

        .brand-name {
            font-size: 40px;
            font-weight: bold;
            letter-spacing: 8px;
            color: #ff0055;
            margin: 0;
            text-shadow: 0 0 15px rgba(255, 0, 85, 0.5);
        }

        .sub-text {
            font-size: 14px;
            color: #888;
            margin-top: 5px;
            margin-bottom: 40px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        h1 { font-weight: 300; margin-bottom: 10px; }

        button {
            padding: 15px 50px;
            font-size: 18px;
            cursor: pointer;
            border-radius: 30px;
            border: 2px solid #ff0055;
            background-color: transparent;
            color: white;
            transition: all 0.3s ease;
            box-shadow: 0 0 10px rgba(255, 0, 85, 0.2);
        }

        button:hover {
            background-color: #ff0055;
            box-shadow: 0 0 20px rgba(255, 0, 85, 0.6);
        }

        #instructions {
            margin-top: 25px;
            font-size: 13px;
            color: #666;
            direction: rtl;
        }
    </style>
</head>

<body style="margin: 0; overflow: hidden;">

    <div id="overlay">
        <p class="brand-name">ABEER</p>
        <p class="sub-text">Interactive AR Experience</p>
        
        <h1>عرض الواقع المعزز</h1>
        <p style="direction: rtl;">جاهزة لاستعراض الشكل؟</p>
        
        <button onclick="startAR()">ابدأ العرض الآن</button>
        
        <div id="instructions">
            بعد الضغط، وجه الكاميرا لمساحة مفتوحة<br>
            استخدمي أصابعك لتدوير وتكبير المخروط
        </div>
    </div>

    <a-scene 
        arjs="sourceType: webcam; debugUIEnabled: false;" 
        embedded 
        renderer="logarithmicDepthBuffer: true;"
        vr-mode-ui="enabled: false"
        gesture-detector 
        id="scene">

        <a-entity light="type: ambient; intensity: 0.7;"></a-entity>
        <a-entity light="type: directional; intensity: 0.5;" position="1 1 1"></a-entity>

        <a-marker preset="hiro" raycaster="objects: .clickable" emitevents="true" cursor="fuse: false; rayOrigin: mouse;">
            
            <a-cone 
                id="abeer-cone"
                class="clickable"
                position="0 0.5 0" 
                radius-bottom="0.4" 
                radius-top="0" 
                height="0.8" 
                material="color: #ff0055; metalness: 0.5; roughness: 0.2; opacity: 0.9;"
                gesture-handler="minScale: 0.2; maxScale: 5;">
            </a-cone>

        </a-marker>

        <a-entity camera></a-entity>
    </a-scene>

    <script>
        function startAR() {
            // إخفاء الواجهة عند النقر
            document.getElementById('overlay').style.display = 'none';
        }

        // ملاحظة للمستخدمة عبير: 
        // الكود يستخدم ماركر "Hiro". عند فتح الكاميرا، وجهيها لصورة Hiro Marker 
        // وستجدين المخروط الفوشيا يظهر ويمكنك تدويره بأصابعك.
    </script>
</body>
</html>
