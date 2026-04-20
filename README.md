<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>تفاعل واقع معزز - مخروط</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
    <script src="https://raw.githack.com/donmccurdy/aframe-extras/master/dist/aframe-extras.min.js"></script>
    <script src="https://raw.githack.com/fcor/aframe-physics-system/v4.0.1/dist/aframe-physics-system.min.js"></script>
    
    <style>
        /* تصميم واجهة البداية */
        #overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 9999; color: white; font-family: sans-serif; text-align: center;
        }
        button {
            padding: 15px 40px; font-size: 22px; cursor: pointer;
            border-radius: 50px; border: none; background-color: #ff0055;
            color: white; box-shadow: 0 4px 15px rgba(255,0,85,0.4);
            margin-top: 20px; transition: 0.3s;
        }
        button:active { transform: scale(0.95); }
        #instructions { margin-top: 20px; font-size: 14px; color: #ccc; max-width: 80%; }
    </style>
</head>

<body style="margin: 0; overflow: hidden;">

    <div id="overlay">
        <h1>الواقع المعزز جاهز</h1>
        <p>اضغط الزر، ثم وجه الكاميرا نحو مكان مستوٍ.</p>
        <button onclick="startAR()">ابدأ العرض</button>
        <div id="instructions">
            (استخدم إصبعين للتدوير والتكبير)
        </div>
    </div>

    <a-scene 
        embedded 
        arjs="sourceType: webcam; debugUIEnabled: false; detectionMode: mono_and_matrix; matrixCodeType: 3x3;"
        vr-mode-ui="enabled: false"
        gesture-detector> <a-entity light="type: ambient; color: #BBB"></a-entity>
        <a-entity light="type: directional; color: #FFF; intensity: 0.6" position="-0.5 1 1"></a-entity>

        <a-entity camera look-controls raycaster="objects: .clickable" cursor="fuse: false; rayOrigin: mouse;"></a-entity>

        <a-entity 
            id="cone-container"
            ar-markerless="anchoring: plane;" 
            position="0 0 -2"  class="clickable"
            gesture-handler> <a-cone 
                id="myCone"
                radius-bottom="0.3" 
                radius-top="0" 
                height="0.7" 
                material="color: #ff0055; roughness: 0.1; metalness: 0.5; opacity: 0.9; transparent: true"
                animation="property: rotation; to: 0 360 0; loop: true; dur: 10000; eased: linear">
            </a-cone>
        </a-entity>

    </a-scene>

    <script>
        function startAR() {
            // إخفاء واجهة البداية
            document.getElementById('overlay').style.display = 'none';
        }

        // تسجيل مركّبة (Component) مخصصة للتحكم باللمس (Rotate and Scale)
        AFRAME.registerComponent('gesture-handler', {
            schema: {
                enabled: {default: true},
                rotationFactor: {default: 5},
                minScale: {default: 0.3},
                maxScale: {default: 3},
            },

            init: function () {
                this.handleOnInput = this.handleOnInput.bind(this);
                this.handleOnPinch = this.handleOnPinch.bind(this);
                this.isPinching = false;
            },

            play: function () {
                window.addEventListener('onefingermove', this.handleOnInput);
                window.addEventListener('twofingermove', this.handleOnPinch);
            },

            pause: function () {
                window.removeEventListener('onefingermove', this.handleOnInput);
                window.removeEventListener('twofingermove', this.handleOnPinch);
            },

            handleOnInput: function (event) {
                if (!this.data.enabled || this.isPinching) { return; }
                const yRotation = event.detail.positionChange.x * this.data.rotationFactor;
                const xRotation = event.detail.positionChange.y * this.data.rotationFactor;
                this.el.object3D.rotation.y += yRotation;
                this.el.object3D.rotation.x += xRotation;
            },

            handleOnPinch: function (event) {
                if (!this.data.enabled) { return; }
                this.isPinching = true;
                const scaleFactor = event.detail.spreadChange;
                const currentScale = this.el.getAttribute('scale');
                const newScale = {
                    x: currentScale.x + scaleFactor,
                    y: currentScale.y + scaleFactor,
                    z: currentScale.z + scaleFactor,
                };

                // تحديد حدود التكبير والتصغير
                newScale.x = Math.min(Math.max(newScale.x, this.data.minScale), this.data.maxScale);
                newScale.y = Math.min(Math.max(newScale.y, this.data.minScale), this.data.maxScale);
                newScale.z = Math.min(Math.max(newScale.z, this.data.minScale), this.data.maxScale);

                this.el.setAttribute('scale', newScale);
                
                // إعادة تعيين حالة القرص بعد فترة قصيرة
                clearTimeout(this.pinchTimeout);
                this.pinchTimeout = setTimeout(() => { this.isPinching = false; }, 50);
            },
        });
    </script>
</body>
</html>
