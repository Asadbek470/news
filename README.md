# news
...
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Новостной портал с синхронизацией</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.6;
        }
        
        header {
            background: linear-gradient(135deg, #2c3e50, #1a2530);
            color: white;
            padding: 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
            flex-wrap: wrap;
        }
        
        .logo {
            font-size: 1.8rem;
            font-weight: bold;
            display: flex;
            align-items: center;
        }
        
        .logo i {
            margin-right: 10px;
            color: #3498db;
        }
        
        nav ul {
            display: flex;
            list-style: none;
            flex-wrap: wrap;
        }
        
        nav ul li {
            margin: 0 10px;
        }
        
        nav ul li a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            transition: color 0.3s;
        }
        
        nav ul li a:hover {
            color: #3498db;
        }
        
        .auth-buttons {
            display: flex;
            gap: 15px;
        }
        
        .btn {
            padding: 8px 20px;
            border-radius: 4px;
            border: none;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        .btn-login {
            background-color: #3498db;
            color: white;
        }
        
        .btn-register {
            background-color: #e74c3c;
            color: white;
        }
        
        .btn-sync {
            background-color: #9b59b6;
            color: white;
            margin-left: 10px;
        }
        
        .btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .sync-info {
            background-color: #f1c40f;
            color: #333;
            padding: 10px;
            border-radius: 4px;
            margin: 15px 0;
            text-align: center;
            display: none;
        }
        
        .news-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 25px;
            margin-top: 30px;
        }
        
        .news-card {
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s;
        }
        
        .news-card:hover {
            transform: translateY(-5px);
        }
        
        .news-image {
            width: 100%;
            height: 200px;
            object-fit: cover;
        }
        
        .news-content {
            padding: 20px;
        }
        
        .news-title {
            font-size: 1.3rem;
            margin-bottom: 10px;
            color: #2c3e50;
        }
        
        .news-excerpt {
            color: #7f8c8d;
            margin-bottom: 15px;
        }
        
        .news-date {
            color: #95a5a6;
            font-size: 0.9rem;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 100;
            justify-content: center;
            align-items: center;
        }
        
        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            width: 400px;
            max-width: 90%;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
            max-height: 90vh;
            overflow-y: auto;
        }
        
        .modal h2 {
            margin-bottom: 20px;
            color: #2c3e50;
            text-align: center;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }
        
        .form-group input, .form-group textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
        }
        
        .form-group textarea {
            min-height: 100px;
            resize: vertical;
        }
        
        .admin-panel {
            display: none;
            background-color: #2c3e50;
            color: white;
            padding: 15px;
            margin-top: 20px;
            border-radius: 8px;
        }
        
        .admin-panel h2 {
            margin-bottom: 20px;
            text-align: center;
        }
        
        .admin-actions {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }
        
        .admin-action {
            background-color: #34495e;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        
        .admin-action:hover {
            background-color: #3498db;
        }
        
        footer {
            background-color: #2c3e50;
            color: white;
            text-align: center;
            padding: 20px;
            margin-top: 50px;
        }
        
        .add-content-modal {
            display: none;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            background-color: #2ecc71;
            color: white;
            border-radius: 5px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
            opacity: 0;
            transition: opacity 0.3s;
            z-index: 1000;
        }
        
        .notification.show {
            opacity: 1;
        }
        
        .notification.error {
            background-color: #e74c3c;
        }
        
        @media (max-width: 768px) {
            nav ul {
                display: none;
            }
            
            .news-container {
                grid-template-columns: 1fr;
            }
            
            header {
                flex-direction: column;
                gap: 15px;
            }
            
            .auth-buttons {
                width: 100%;
                justify-content: center;
            }
        }
        
        .server-status {
            display: flex;
            align-items: center;
            margin-top: 10px;
            color: #7f8c8d;
            font-size: 0.9rem;
        }
        
        .status-dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            margin-right: 8px;
        }
        
        .status-online {
            background-color: #2ecc71;
        }
        
        .status-offline {
            background-color: #e74c3c;
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">
            <i>📰</i> Новостной портал
        </div>
        <nav>
            <ul>
                <li><a href="#">Главная</a></li>
                <li><a href="#">Политика</a></li>
                <li><a href="#">Экономика</a></li>
                <li><a href="#">Технологии</a></li>
                <li><a href="#">Спорт</a></li>
            </ul>
        </nav>
        <div class="auth-buttons">
            <button class="btn btn-login" onclick="openModal('login')">Вход</button>
            <button class="btn btn-register" onclick="openModal('register')">Регистрация</button>
            <button class="btn btn-sync" onclick="syncNews()">Синхронизировать</button>
        </div>
    </header>

    <div class="container">
        <div class="sync-info" id="syncInfo">
            <strong>Внимание:</strong> Это демонстрационная версия. В реальном приложении новости будут синхронизироваться между устройствами через сервер.
        </div>
        
        <h1>Последние новости</h1>
        <div class="server-status">
            <div class="status-dot status-online"></div>
            <span>Сервер: онлайн (localStorage)</span>
        </div>
        
        <div id="newsContainer" class="news-container">
            <!-- Новости будут добавляться здесь -->
        </div>
        
        <div id="adminPanel" class="admin-panel">
            <h2>Панель администратора</h2>
            <div class="admin-actions">
                <div class="admin-action" onclick="openAddContentModal('news')">Добавить новость</div>
                <div class="admin-action" onclick="openAddContentModal('image')">Добавить изображение</div>
                <div class="admin-action" onclick="openAddContentModal('video')">Добавить видео</div>
                <div class="admin-action" onclick="openAddContentModal('repost')">Сделать репост</div>
            </div>
        </div>
    </div>

    <!-- Модальное окно входа -->
    <div id="loginModal" class="modal">
        <div class="modal-content">
            <h2>Вход в систему</h2>
            <div class="form-group">
                <label for="loginUsername">Имя пользователя</label>
                <input type="text" id="loginUsername" placeholder="Введите имя пользователя">
            </div>
            <div class="form-group">
                <label for="loginPassword">Пароль</label>
                <input type="password" id="loginPassword" placeholder="Введите пароль">
            </div>
            <button class="btn btn-login" style="width: 100%" onclick="login()">Войти</button>
            <p style="text-align: center; margin-top: 15px;">
                <a href="#" onclick="alert('Функция в разработке')">Забыли пароль?</a>
            </p>
        </div>
    </div>

    <!-- Модальное окно регистрации -->
    <div id="registerModal" class="modal">
        <div class="modal-content">
            <h2>Регистрация</h2>
            <div class="form-group">
                <label for="registerUsername">Имя пользователя</label>
                <input type="text" id="registerUsername" placeholder="Придумайте имя пользователя">
            </div>
            <div class="form-group">
                <label for="registerEmail">Email</label>
                <input type="email" id="registerEmail" placeholder="Введите ваш email">
            </div>
            <div class="form-group">
                <label for="registerPassword">Пароль</label>
                <input type="password" id="registerPassword" placeholder="Придумайте пароль">
            </div>
            <div class="form-group">
                <label for="registerPasswordConfirm">Подтверждение пароля</label>
                <input type="password" id="registerPasswordConfirm" placeholder="Повторите пароль">
            </div>
            <button class="btn btn-register" style="width: 100%" onclick="register()">Зарегистрироваться</button>
        </div>
    </div>

    <!-- Модальное окно добавления контента -->
    <div id="addContentModal" class="modal add-content-modal">
        <div class="modal-content">
            <h2 id="addContentTitle">Добавить контент</h2>
            <div id="addNewsForm" style="display: none;">
                <div class="form-group">
                    <label for="newsTitle">Заголовок новости</label>
                    <input type="text" id="newsTitle" placeholder="Введите заголовок">
                </div>
                <div class="form-group">
                    <label for="newsContent">Содержание новости</label>
                    <textarea id="newsContent" placeholder="Введите текст новости"></textarea>
                </div>
                <div class="form-group">
                    <label for="newsImage">URL изображения (необязательно)</label>
                    <input type="text" id="newsImage" placeholder="https://example.com/image.jpg">
                </div>
                <button class="btn btn-login" style="width: 100%" onclick="addNews()">Добавить новость</button>
            </div>
            
            <div id="addImageForm" style="display: none;">
                <div class="form-group">
                    <label for="imageUrl">URL изображения</label>
                    <input type="text" id="imageUrl" placeholder="https://example.com/image.jpg">
                </div>
                <div class="form-group">
                    <label for="imageDescription">Описание изображения</label>
                    <input type="text" id="imageDescription" placeholder="Описание изображения">
                </div>
                <button class="btn btn-login" style="width: 100%" onclick="addImage()">Добавить изображение</button>
            </div>
            
            <div id="addVideoForm" style="display: none;">
                <div class="form-group">
                    <label for="videoUrl">URL видео (YouTube или другой источник)</label>
                    <input type="text" id="videoUrl" placeholder="https://youtube.com/embed/abc123">
                </div>
                <div class="form-group">
                    <label for="videoTitle">Название видео</label>
                    <input type="text" id="videoTitle" placeholder="Название видео">
                </div>
                <button class="btn btn-login" style="width: 100%" onclick="addVideo()">Добавить видео</button>
            </div>
            
            <div id="addRepostForm" style="display: none;">
                <div class="form-group">
                    <label for="repostUrl">URL источника</label>
                    <input type="text" id="repostUrl" placeholder="https://example.com/news">
                </div>
                <div class="form-group">
                    <label for="repostTitle">Заголовок репоста</label>
                    <input type="text" id="repostTitle" placeholder="Заголовок">
                </div>
                <div class="form-group">
                    <label for="repostDescription">Описание</label>
                    <textarea id="repostDescription" placeholder="Краткое описание"></textarea>
                </div>
                <button class="btn btn-login" style="width: 100%" onclick="addRepost()">Сделать репост</button>
            </div>
        </div>
    </div>

    <div id="notification" class="notification"></div>

    <footer>
        <p>© 2023 Новостной портал. Все права защищены.</p>
        <p>Это демонстрационная версия. В реальном приложении данные синхронизируются между устройствами через сервер.</p>
    </footer>

    <script>
        // Имитация сервера через localStorage
        const NEWS_STORAGE_KEY = 'news_portal_data';
        
        // Загрузка новостей из localStorage
        function loadNewsFromStorage() {
            const newsJSON = localStorage.getItem(NEWS_STORAGE_KEY);
            return newsJSON ? JSON.parse(newsJSON) : [];
        }
        
        // Сохранение новостей в localStorage
        function saveNewsToStorage(news) {
            localStorage.setItem(NEWS_STORAGE_KEY, JSON.stringify(news));
        }
        
        // Пример начальных новостей
        const initialNews = [
            {
                id: 1,
                title: "Важное событие в мире технологий",
                content: "Крупная IT-компания представила революционный продукт, который изменит будущее цифровых технологий.",
                image: "https://placehold.co/600x400/3498db/ffffff?text=Новость+1",
                date: "15 октября 2023",
                type: "news"
            },
            {
                id: 2,
                title: "Изменения в экономической политике",
                content: "Правительство объявило о новых мерах поддержки бизнеса в условиях текущей экономической ситуации.",
                image: "https://placehold.co/600x400/e74c3c/ffffff?text=Новость+2",
                date: "14 октября 2023",
                type: "news"
            },
            {
                id: 3,
                title: "Спортивные достижения",
                content: "Национальная сборная одержала победу в международных соревнованиях, установив новый рекорд.",
                image: "https://placehold.co/600x400/2ecc71/ffffff?text=Новость+3",
                date: "13 октября 2023",
                type: "news"
            }
        ];
        
        // Загрузка начальных новостей
        document.addEventListener('DOMContentLoaded', function() {
            // Показать информацию о синхронизации
            document.getElementById('syncInfo').style.display = 'block';
            
            let news = loadNewsFromStorage();
            
            // Если новостей нет, сохраняем начальные
            if (news.length === 0) {
                news = initialNews;
                saveNewsToStorage(news);
            }
            
            renderNews(news);
        });
        
        // Отображение новостей
        function renderNews(newsArray) {
            const newsContainer = document.getElementById('newsContainer');
            newsContainer.innerHTML = '';
            
            // Сортируем новости по дате (сначала новые)
            const sortedNews = [...newsArray].sort((a, b) => b.id - a.id);
            
            sortedNews.forEach(news => {
                newsContainer.appendChild(createNewsCard(news));
            });
        }
        
        // Функции для модальных окон
        function openModal(type) {
            if (type === 'login') {
                document.getElementById('loginModal').style.display = 'flex';
            } else if (type === 'register') {
                document.getElementById('registerModal').style.display = 'flex';
            }
        }
        
        function openAddContentModal(type) {
            // Скрыть все формы
            document.getElementById('addNewsForm').style.display = 'none';
            document.getElementById('addImageForm').style.display = 'none';
            document.getElementById('addVideoForm').style.display = 'none';
            document.getElementById('addRepostForm').style.display = 'none';
            
            // Показать нужную форму
            if (type === 'news') {
                document.getElementById('addContentTitle').textContent = 'Добавить новость';
                document.getElementById('addNewsForm').style.display = 'block';
            } else if (type === 'image') {
                document.getElementById('addContentTitle').textContent = 'Добавить изображение';
                document.getElementById('addImageForm').style.display = 'block';
            } else if (type === 'video') {
                document.getElementById('addContentTitle').textContent = 'Добавить видео';
                document.getElementById('addVideoForm').style.display = 'block';
            } else if (type === 'repost') {
                document.getElementById('addContentTitle').textContent = 'Сделать репост';
                document.getElementById('addRepostForm').style.display = 'block';
            }
            
            document.getElementById('addContentModal').style.display = 'flex';
        }
        
        // Закрытие модальных окон при клике вне их области
        window.onclick = function(event) {
            if (event.target.classList.contains('modal')) {
                event.target.style.display = 'none';
            }
        }
        
        // Функция входа
        function login() {
            const username = document.getElementById('loginUsername').value;
            const password = document.getElementById('loginPassword').value;
            
            if (username === 'admin' && password === 'admin') {
                showNotification('Вход выполнен как администратор');
                document.getElementById('adminPanel').style.display = 'block';
                document.getElementById('loginModal').style.display = 'none';
            } else if (username && password) {
                showNotification('Вход выполнен как пользователь');
                document.getElementById('loginModal').style.display = 'none';
            } else {
                showNotification('Заполните все поля', true);
            }
        }
        
        // Функция регистрации
        function register() {
            const username = document.getElementById('registerUsername').value;
            const email = document.getElementById('registerEmail').value;
            const password = document.getElementById('registerPassword').value;
            const passwordConfirm = document.getElementById('registerPasswordConfirm').value;
            
            if (!username || !email || !password || !passwordConfirm) {
                showNotification('Заполните все поля', true);
                return;
            }
            
            if (password !== passwordConfirm) {
                showNotification('Пароли не совпадают', true);
                return;
            }
            
            showNotification('Регистрация успешно завершена!');
            document.getElementById('registerModal').style.display = 'none';
        }
        
        // Функция добавления новости
        function addNews() {
            const title = document.getElementById('newsTitle').value;
            const content = document.getElementById('newsContent').value;
            const imageUrl = document.getElementById('newsImage').value || 'https://placehold.co/600x400/3498db/ffffff?text=Новость';
            
            if (!title || !content) {
                showNotification('Заполните заголовок и содержание новости', true);
                return;
            }
            
            const news = loadNewsFromStorage();
            const newId = Math.max(...news.map(item => item.id), 0) + 1;
            
            const newsItem = {
                id: newId,
                title,
                content,
                image: imageUrl,
                date: new Date().toLocaleDateString('ru-RU', { day: 'numeric', month: 'long', year: 'numeric' }),
                type: 'news'
            };
            
            news.push(newsItem);
            saveNewsToStorage(news);
            renderNews(news);
            
            document.getElementById('addContentModal').style.display = 'none';
            showNotification('Новость успешно добавлена!');
            
            // Очистка формы
            document.getElementById('newsTitle').value = '';
            document.getElementById('newsContent').value = '';
            document.getElementById('newsImage').value = '';
        }
        
        // Функция добавления изображения
        function addImage() {
            const url = document.getElementById('imageUrl').value;
            const description = document.getElementById('imageDescription').value;
            
            if (!url || !description) {
                showNotification('Заполните URL и описание изображения', true);
                return;
            }
            
            const news = loadNewsFromStorage();
            const newId = Math.max(...news.map(item => item.id), 0) + 1;
            
            const imageItem = {
                id: newId,
                title: description,
                content: 'Новое изображение в галерее',
                image: url,
                date: new Date().toLocaleDateString('ru-RU', { day: 'numeric', month: 'long', year: 'numeric' }),
                type: 'image'
            };
            
            news.push(imageItem);
            saveNewsToStorage(news);
            renderNews(news);
            
            document.getElementById('addContentModal').style.display = 'none';
            showNotification('Изображение успешно добавлено!');
            
            // Очистка формы
            document.getElementById('imageUrl').value = '';
            document.getElementById('imageDescription').value = '';
        }
        
        // Функция добавления видео
        function addVideo() {
            const url = document.getElementById('videoUrl').value;
            const title = document.getElementById('videoTitle').value;
            
            if (!url || !title) {
                showNotification('Заполните URL и название видео', true);
                return;
            }
            
            const news = loadNewsFromStorage();
            const newId = Math.max(...news.map(item => item.id), 0) + 1;
            
            const videoItem = {
                id: newId,
                title,
                content: 'Новое видео',
                video: url,
                date: new Date().toLocaleDateString('ru-RU', { day: 'numeric', month: 'long', year: 'numeric' }),
                type: 'video'
            };
            
            news.push(videoItem);
            saveNewsToStorage(news);
            renderNews(news);
            
            document.getElementById('addContentModal').style.display = 'none';
            showNotification('Видео успешно добавлено!');
            
            // Очистка формы
            document.getElementById('videoUrl').value = '';
            document.getElementById('videoTitle').value = '';
        }
        
        // Функция добавления репоста
        function addRepost() {
            const url = document.getElementById('repostUrl').value;
            const title = document.getElementById('repostTitle').value;
            const description = document.getElementById('repostDescription').value;
            
            if (!url || !title || !description) {
                showNotification('Заполните все поля для репоста', true);
                return;
            }
            
            const news = loadNewsFromStorage();
            const newId = Math.max(...news.map(item => item.id), 0) + 1;
            
            const repostItem = {
                id: newId,
                title,
                content: description,
                url,
                date: new Date().toLocaleDateString('ru-RU', { day: 'numeric', month: 'long', year: 'numeric' }),
                type: 'repost'
            };
            
            news.push(repostItem);
            saveNewsToStorage(news);
            renderNews(news);
            
            document.getElementById('addContentModal').style.display = 'none';
            showNotification('Репост успешно добавлен!');
            
            // Очистка формы
            document.getElementById('repostUrl').value = '';
            document.getElementById('repostTitle').value = '';
            document.getElementById('repostDescription').value = '';
        }
        
        // Создание карточки новости
        function createNewsCard(item) {
            const card = document.createElement('div');
            card.className = 'news-card';
            
            let contentHTML = '';
            
            if (item.type === 'video') {
                contentHTML = `
                    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden;">
                        <iframe src="${item.video}" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;" allowfullscreen></iframe>
                    </div>
                    <div class="news-content">
                        <h2 class="news-title">${item.title}</h2>
                        <p class="news-excerpt">${item.content}</p>
                        <p class="news-date">${item.date}</p>
                    </div>
                `;
            } else if (item.type === 'repost') {
                contentHTML = `
                    <img src="https://placehold.co/600x400/9b59b6/ffffff?text=Репост" alt="Репост" class="news-image">
                    <div class="news-content">
                        <h2 class="news-title">${item.title}</h2>
                        <p class="news-excerpt">${item.content}</p>
                        <p class="news-date">${item.date}</p>
                        <p><a href="${item.url}" target="_blank">Читать оригинал</a></p>
                    </div>
                `;
            } else {
                contentHTML = `
                    <img src="${item.image}" alt="${item.title}" class="news-image">
                    <div class="news-content">
                        <h2 class="news-title">${item.title}</h2>
                        <p class="news-excerpt">${item.content}</p>
                        <p class="news-date">${item.date}</p>
                    </div>
                `;
            }
            
            card.innerHTML = contentHTML;
            return card;
        }
        
        // Функция показа уведомлений
        function showNotification(message, isError = false) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.style.backgroundColor = isError ? '#e74c3c' : '#2ecc71';
            if (isError) {
                notification.classList.add('error');
            } else {
                notification.classList.remove('error');
            }
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }
        
        // Функция синхронизации (имитация)
        function syncNews() {
            showNotification('Синхронизация с сервером...');
            
            // Имитация задержки сети
            setTimeout(() => {
                const news = loadNewsFromStorage();
                renderNews(news);
                showNotification('Данные синхронизированы!');
            }, 1500);
        }
        
        // Секретный доступ по вводу "/admin"
        let inputBuffer = '';
        document.addEventListener('keydown', function(event) {
            // Добавляем символ в буфер
            if (event.key.length === 1) {
                inputBuffer += event.key;
            }
            
            // Проверяем, не содержится ли в буфере "/admin"
            if (inputBuffer.includes('/admin')) {
                document.getElementById('adminPanel').style.display = 'block';
                showNotification('Режим администратора активирован');
                inputBuffer = ''; // Сбрасываем буфер
            }
            
            // Ограничиваем длину буфера
            if (inputBuffer.length > 10) {
                inputBuffer = inputBuffer.substring(1);
            }
        });
    </script>
</body>
</html>
