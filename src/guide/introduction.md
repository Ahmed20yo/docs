<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>موقع ثلاثي الأبعاد مع ذكاء اصطناعي</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            height: 100%;
            font-family: 'Arial', sans-serif;
            background: black;
            color: white;
            overflow: hidden;
        }

        header {
            position: fixed;
            top: 0;
            width: 100%;
            z-index: 10;
            background: rgba(0, 0, 0, 0.8);
            padding: 1rem;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        header h1 {
            font-size: 1.8rem;
            color: #00d4ff;
        }

        header nav a {
            color: white;
            text-decoration: none;
            margin: 0 1rem;
            font-size: 1rem;
            transition: color 0.3s ease;
        }

        header nav a:hover {
            color: #00d4ff;
        }

        canvas {
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .container {
            position: relative;
            z-index: 5;
            text-align: center;
            margin-top: 8rem;
        }

        .container h1 {
            font-size: 3rem;
            margin-bottom: 1rem;
            background: linear-gradient(90deg, #00d4ff, #ff00f7);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .container p {
            font-size: 1.2rem;
            max-width: 600px;
            margin: 1rem auto;
            color: #ccc;
        }

        .btn {
            display: inline-block;
            padding: 0.8rem 2rem;
            margin-top: 1.5rem;
            font-size: 1.2rem;
            color: white;
            background: linear-gradient(90deg, #00d4ff, #ff00f7);
            border: none;
            border-radius: 30px;
            cursor: pointer;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .btn:hover {
            transform: scale(1.1);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
        }

        .news-section {
            position: relative;
            z-index: 5;
            padding: 2rem;
            background: rgba(0, 0, 0, 0.8);
            margin-top: 2rem;
            border-radius: 10px;
        }

        .news-section h2 {
            font-size: 2rem;
            margin-bottom: 1rem;
            color: #00d4ff;
        }

        .news-item {
            margin-bottom: 1.5rem;
            border-bottom: 1px solid #444;
            padding-bottom: 1rem;
        }

        .news-item h3 {
            font-size: 1.5rem;
            color: #ff00f7;
        }

        .news-item p {
            color: #ccc;
            font-size: 1rem;
        }

        .news-item a {
            color: #00d4ff;
            text-decoration: none;
        }
    </style>
</head>
<body>
    <header>
        <h1>موقع ثلاثي الأبعاد</h1>
        <nav>
            <a href="#about">عن الموقع</a>
            <a href="#features">الميزات</a>
            <a href="#contact">تواصل معنا</a>
        </nav>
    </header>

    <canvas id="bg"></canvas>

    <div class="container">
        <h1>مرحبًا بك في موقع المستقبل</h1>
        <p>هذا الموقع يعرض تصميمًا ثلاثي الأبعاد فريدًا، مع تأثيرات مبهرة وتقنيات حديثة لجذب الانتباه.</p>
        <button class="btn">استكشاف المزيد</button>
    </div>

    <div class="news-section">
        <h2>آخر الأخبار</h2>
        <div id="news-container">
            <!-- الأخبار ستُضاف هنا ديناميكيًا -->
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        // إعداد المشهد والكاميرا والرندر
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ canvas: document.querySelector('#bg') });

        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        camera.position.setZ(30);

        // إنشاء مجسم ثلاثي الأبعاد
        const geometry = new THREE.TorusGeometry(10, 3, 16, 100);
        const material = new THREE.MeshStandardMaterial({ color: 0x00d4ff });
        const torus = new THREE.Mesh(geometry, material);

        scene.add(torus);

        // إضاءة
        const pointLight = new THREE.PointLight(0xffffff);
        pointLight.position.set(20, 20, 20);
        const ambientLight = new THREE.AmbientLight(0xffffff);
        scene.add(pointLight, ambientLight);

        // نجوم عشوائية
        function addStar() {
            const starGeometry = new THREE.SphereGeometry(0.25, 24, 24);
            const starMaterial = new THREE.MeshStandardMaterial({ color: 0xffffff });
            const star = new THREE.Mesh(starGeometry, starMaterial);

            const [x, y, z] = Array(3).fill().map(() => THREE.MathUtils.randFloatSpread(100));
            star.position.set(x, y, z);
            scene.add(star);
        }

        Array(200).fill().forEach(addStar);

        // خلفية فضائية
        const spaceTexture = new THREE.TextureLoader().load('https://source.unsplash.com/random/1920x1080?space');
        scene.background = spaceTexture;

        // تدوير الشكل
        function animate() {
            requestAnimationFrame(animate);

            torus.rotation.x += 0.01;
            torus.rotation.y += 0.005;
            torus.rotation.z += 0.01;

            renderer.render(scene, camera);
        }

        animate();

        // تغيير حجم الرندر عند تغيير الشاشة
        window.addEventListener('resize', () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
        });

        // جلب الأخبار باستخدام API
        const API_KEY = 'YOUR_NEWS_API_KEY'; // أدخل مفتاح News API الخاص بك
        const newsContainer = document.getElementById('news-container');

        async function fetchNews() {
            const response = await fetch(`https://newsapi.org/v2/top-headlines?country=us&apiKey=${API_KEY}`);
            const data = await response.json();

            data.articles.slice(0, 5).forEach(article => {
                const newsItem = document.createElement('div');
                newsItem.classList.add('news-item');
                newsItem.innerHTML = `
                    <h3>${article.title}</h3>
                    <p>${article.description || 'لا يوجد وصف متاح.'}</p>
                    <a href="${article.url}" target="_blank">اقرأ المزيد</a>
                `;
                newsContainer.appendChild(newsItem);
            });
        }

        fetchNews();
    </script>
</body>
</html>
![Screenshot_20250614-195102](https://github.com/user-attachments/assets/d8adfb32-a1c3-483b-badf-34d44b6cbc61)
