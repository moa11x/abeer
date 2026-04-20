<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>تجربة الواقع المعزز</title>
  <script type="module" src="https://unpkg.com/@google/model-viewer/dist/model-viewer.min.js"></script>
  <style>
    body {
      font-family: Arial;
      text-align: center;
      margin-top: 50px;
    }
    button {
      padding: 15px 25px;
      font-size: 18px;
      background-color: #007aff;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
    }
    model-viewer {
      width: 100%;
      height: 400px;
      display: none;
      margin-top: 20px;
    }
  </style>
</head>

<body>

<h2>تجربة الواقع المعزز</h2>

<button onclick="startAR()">ابدأ التجربة</button>

<model-viewer 
  id="viewer"
  src="cone.glb"
  ios-src="cone.usdz"
  ar
  ar-modes="scene-viewer quick-look webxr"
  camera-controls
  auto-rotate>
</model-viewer>

<script>
function startAR() {
  const viewer = document.getElementById("viewer");
  viewer.style.display = "block";
  viewer.activateAR();
}
</script>

</body>
</html>
