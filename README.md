<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DIVANEASY - Бескаркасные диваны</title>
  <link href="https://fonts.googleapis.com/css2?family=Montaga&display=swap" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: 'Montaga', serif;
      overflow-x: hidden;
      background: #F6D6D9;
      color: #000000;
      line-height: 1.4;
    }
    section {
      background: #F8CBC6;
      width: 100%;
      padding: 20px 0;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 10px;
    }
    .hero {
      background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://media.discordapp.net/attachments/1241693361279340545/1380973354244378644/assets_task_01jx5r1285e1892tcgm4pfpdwn_1749318498_img_0.webp?ex=6845d328&is=684481a8&hm=cacc5e5cc8ec7400fa5ed9f6fb8ec18f372da76f70d61ae70d52685dafd5a92a');
      background-size: cover;
      background-position: center;
      min-height: 50vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #FFFFFF;
      text-align: center;
      opacity: 0;
      transition: opacity 1.5s ease-in-out;
    }
    .hero.loaded {
      opacity: 1;
    }
    .color-option {
      width: 25px;
      height: 25px;
      border-radius: 50%;
      cursor: pointer;
      border: 2px solid transparent;
      transition: border-color 0.3s;
    }
    .color-option.selected {
      border-color: #000000;
    }
    .color-option.bg-yellow-500 { background-color: #eab308; }
    .color-option.bg-gray-500 { background-color: #6b7280; }
    .color-option.bg-blue-500 { background-color: #3b82f6; }
    .sofa-image {
      width: 100%;
      height: 150px;
      object-fit: cover;
      border-radius: 8px;
      margin-bottom: 10px;
      transition: opacity 0.5s ease-in-out;
    }
    .fade-in {
      opacity: 0;
      transform: translateY(20px);
    }
    .fade-in.visible {
      opacity: 1;
      transform: translateY(0);
      transition: opacity 0.6s ease, transform 0.6s ease;
    }
    .info-box {
      background: #FFF5E1;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      text-align: center;
      transition: transform 0.3s ease;
    }
    .info-box:hover {
      transform: scale(1.03);
    }
    nav a {
      color: #4A504D;
      text-decoration: none;
      padding: 8px;
      font-size: 14px;
      display: block;
    }
    nav a:hover {
      color: #E9B7C2;
    }
    .btn-primary {
      background: #FAB1AA;
      color: #FFFFFF;
      padding: 10px 15px;
      border-radius: 25px;
      text-decoration: none;
      display: inline-block;
      font-size: 14px;
    }
    .btn-primary:hover {
      background: #FBC8C5;
    }
    header {
      position: fixed;
      top: 0;
      width: 100%;
      background: #F5A9A9;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
      z-index: 40;
      padding: 10px 0;
    }
    header .container {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    nav {
      display: none;
      flex-direction: column;
      width: 100%;
    }
    .menu-toggle {
      display: none;
      font-size: 24px;
      background: none;
      border: none;
      color: #4A504D;
      cursor: pointer;
    }
    .grid-1-3, .grid-1-2, .grid-1-4, .staggered-grid {
      display: grid;
      gap: 15px;
    }
    h1 {
      font-size: 1.5rem;
      font-weight: 700;
      color: #4A504D;
    }
    h2 {
      font-size: 1.6rem;
      font-weight: 700;
      margin-bottom: 20px;
      color: #000000;
      text-align: center;
    }
    h3 {
      font-size: 1rem;
      font-weight: 600;
      margin-bottom: 10px;
      color: #000000;
    }
    p {
      font-size: 14px;
      color: #000000;
    }
    ul {
      list-style: disc;
      padding-left: 15px;
      color: #000000;
    }
    .accordion {
      background: #FFF5E1;
      padding: 12px;
      border-radius: 8px;
      margin-bottom: 10px;
      cursor: pointer;
    }
    .accordion h3 {
      margin-bottom: 8px;
    }
    .accordion-content {
      display: none;
      padding-top: 8px;
    }
    .accordion.active .accordion-content {
      display: block;
    }
    @media (max-width: 768px) {
      .hero {
        min-height: 40vh;
      }
      header .container {
        flex-direction: column;
        padding: 10px;
      }
      .menu-toggle {
        display: block;
      }
      nav {
        display: none;
        position: absolute;
        top: 60px;
        left: 0;
        background: #F5A9A9;
        width: 100%;
        padding: 10px 0;
      }
      nav.active {
        display: flex;
      }
      .grid-1-3, .grid-1-2, .grid-1-4, .staggered-grid {
        grid-template-columns: 1fr;
      }
      .info-box {
        padding: 10px;
      }
      .buy-button {
        width: 100%;
        margin-top: 10px;
      }
      .order-form {
        width: 90%;
        left: 5%;
        transform: none;
        padding: 15px;
      }
      #production {
        flex-direction: column;
      }
      #production img {
        max-width: 100%;
        margin-bottom: 15px;
      }
      #production div {
        max-width: 100%;
      }
      h1 {
        font-size: 1.2rem;
      }
      h2 {
        font-size: 1.4rem;
      }
      h3 {
        font-size: 0.9rem;
      }
      p, ul {
        font-size: 12px;
      }
      .back-to-top {
        bottom: 10px;
        right: 10px;
        padding: 8px 12px;
        font-size: 12px;
      }
    }
    @media (min-width: 769px) {
      .menu-toggle {
        display: none;
      }
      nav {
        display: flex;
        flex-direction: row;
        gap: 15px;
      }
      h1 {
        font-size: 1.875rem;
      }
      h2 {
        font-size: 2.25rem;
      }
      #production {
        flex-direction: row;
        align-items: center;
      }
    }
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.9);
      z-index: 1001;
      justify-content: center;
      align-items: center;
    }
    .modal-content {
      max-width: 90%;
      max-height: 90%;
    }
    .close {
      color: #FFFFFF;
      position: absolute;
      top: 15px;
      right: 20px;
      font-size: 24px;
      cursor: pointer;
    }
    .back-to-top {
      position: fixed;
      bottom: 15px;
      right: 15px;
      background: #FAB1AA;
      color: #FFFFFF;
      padding: 10px 15px;
      border-radius: 25px;
      text-decoration: none;
      display: none;
      z-index: 50;
    }
    .back-to-top.visible {
      display: block;
    }
  </style>
</head>
<body>
  <header>
    <div class="container">
      <h1>DIVANEASY</h1>
      <button class="menu-toggle" onclick="toggleMenu()">☰</button>
      <nav id="main-nav">
        <a href="#hero">Главная</a>
        <a href="#catalog">Каталог</a>
        <a href="#delivery">Доставка</a>
        <a href="#payment">Оплата</a>
        <a href="#about">О компании</a>
        <a href="#contact">Контакты</a>
      </nav>
    </div>
  </header>

  <section class="hero" id="hero">
    <div class="container">
      <h2 class="fade-in">DIVANEASY</h2>
      <p style="margin-bottom: 15px;" class="fade-in">Бескаркасные диваны для вашего уюта и стиля</p>
      <p style="max-width: 100%; font-size: 14px;" class="fade-in">Создавайте уют в своем доме с нашими стильными и комфортными модными диванами.</p>
      <a href="#catalog" class="btn-primary fade-in">Купить сейчас</a>
    </div>
  </section>

  <section id="gallery">
    <div class="container">
      <h2 class="fade-in">Галерея наших диванов</h2>
      <div class="grid-1-4">
        <img src="https://cdn.discordapp.com/attachments/1241693361279340545/1380973354772598876/ChatGPT_Image_5_._2025_._20_17_48.png?ex=6845d329&is=684481a9&hm=1b512366cb070c19a066260e6a9573a6d350ce77d293a0bff0fcd73f1aaf36ad&" alt="Галерея 1" class="fade-in" onclick="openModal(this.src)">
        <img src="https://cdn.discordapp.com/attachments/1241693361279340545/1380973503989158110/20250606_0126_Brown_Sofa_Placement_remix_01jx134jx8er0v9y9rkyab921m.png?ex=6845d34c&is=684481cc&hm=ed5bbf88a4151bd351b20c1f193ddc4edf81799a3e1206b00f364a262ccaefb7&" alt="Галерея 2" class="fade-in" onclick="openModal(this.src)">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s" alt="Галерея 3" class="fade-in" onclick="openModal(this.src)">
        <img src="https://fayni-mebli.com/content/goods/61647/picture-61647.jpg" alt="Галерея 4" class="fade-in" onclick="openModal(this.src)">
        <img src="https://tia-sport.com/image/cache/data/beskarkasnaya-mebel/sm-0804/sm-0804-1000x1000.JPG" alt="Галерея 5" class="fade-in" onclick="openModal(this.src)">
      </div>
    </div>
  </section>

  <section id="advantages">
    <div class="container">
      <h2 class="fade-in">Почему выбирают DIVANEASY?</h2>
      <div class="grid-1-3">
        <div class="info-box fade-in">
          <img src="https://via.placeholder.com/80?text=Комфорт" alt="Комфорт" style="margin-bottom: 10px;">
          <h3>Максимальный комфорт</h3>
          <p>Наши диваны адаптируются к вашему телу.</p>
        </div>
        <div class="info-box fade-in">
          <img src="https://via.placeholder.com/80?text=Дизайн" alt="Дизайн" style="margin-bottom: 10px;">
          <h3>Стильный дизайн</h3>
          <p>Яркие цвета для любого интерьера.</p>
        </div>
        <div class="info-box fade-in">
          <img src="https://via.placeholder.com/80?text=Качество" alt="Качество" style="margin-bottom: 10px;">
          <h3>Высокое качество</h3>
          <p>Экологичные и долговечные материалы.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="catalog">
    <div class="container">
      <h2 class="fade-in">Наши диваны</h2>
      <div class="grid-1-3">
        <div class="info-box">
          <img src="https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg" alt="Диван Комфорт" class="sofa-image" id="sofa-image-1">
          <h3>Модель "Комфорт"</h3>
          <p>Мягкий и уютный бескаркасный диван.</p>
          <div style="margin: 10px 0;">
            <p style="font-weight: 600;">Цвет:</p>
            <div style="display: flex; justify-content: center; gap: 6px;">
              <div class="color-option bg-yellow-500" onclick="changeSofaImage('sofa-image-1', 'https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg', this)"></div>
              <div class="color-option bg-gray-500" onclick="changeSofaImage('sofa-image-1', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s', this)"></div>
              <div class="color-option bg-blue-500" onclick="changeSofaImage('sofa-image-1', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s', this)"></div>
            </div>
          </div>
          <p><strong>Цена:</strong> 25 000 ₽</p>
          <button class="buy-button" onclick="toggleOrderForm(this.nextElementSibling)">Купить</button>
          <div class="order-form">
            <input type="text" placeholder="Имя" style="width: 100%; padding: 8px; margin-bottom: 8px;">
            <input type="email" placeholder="Email" style="width: 100%; padding: 8px; margin-bottom: 8px;">
            <textarea placeholder="Комментарий" style="width: 100%; padding: 8px; margin-bottom: 8px;" rows="2"></textarea>
            <button class="btn-primary" style="width: 100%;">Отправить</button>
          </div>
        </div>
        <div class="info-box">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s" alt="Диван Релакс" class="sofa-image" id="sofa-image-2">
          <h3>Модель "Релакс"</h3>
          <p>Идеальный выбор для отдыха.</p>
          <div style="margin: 10px 0;">
            <p style="font-weight: 600;">Цвет:</p>
            <div style="display: flex; justify-content: center; gap: 6px;">
              <div class="color-option bg-gray-500" onclick="changeSofaImage('sofa-image-2', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s', this)"></div>
              <div class="color-option bg-yellow-500" onclick="changeSofaImage('sofa-image-2', 'https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg', this)"></div>
              <div class="color-option bg-blue-500" onclick="changeSofaImage('sofa-image-2', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s', this)"></div>
            </div>
          </div>
          <p><strong>Цена:</strong> 30 000 ₽</p>
          <button class="buy-button" onclick="toggleOrderForm(this.nextElementSibling)">Купить</button>
          <div class="order-form">
            <input type="text" placeholder="Имя" style="width: 100%; padding: 8px; margin-bottom: 8px;">
            <input type="email" placeholder="Email" style="width: 100%; padding: 8px; margin-bottom: 8px;">
            <textarea placeholder="Комментарий" style="width: 100%; padding: 8px; margin-bottom: 8px;" rows="2"></textarea>
            <button class="btn-primary" style="width: 100%;">Отправить</button>
          </div>
        </div>
        <div class="info-box">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s" alt="Диван Сириус" class="sofa-image" id="sofa-image-3">
          <h3>Модель "Сириус"</h3>
          <p>Яркий и современный диван.</p>
          <div style="margin: 10px 0;">
            <p style="font-weight: 600;">Цвет:</p>
            <div style="display: flex; justify-content: center; gap: 6px;">
              <div class="color-option bg-blue-500" onclick="changeSofaImage('sofa-image-3', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s', this)"></div>
              <div class="color-option bg-yellow-500" onclick="changeSofaImage('sofa-image-3', 'https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg', this)"></div>
              <div class="color-option bg-gray-500" onclick="changeSofaImage('sofa-image-3', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s', this)"></div>
            </div>
          </div>
          <p><strong>Цена:</strong> 28 000 ₽</p>
          <button class="buy-button" onclick="toggleOrderForm(this.nextElementSibling)">Купить</button>
          <div class="order-form">
            <input type="text" placeholder="Имя" style="width: 100%; padding: 8px; margin-bottom: 8px;">
            <input type="email" placeholder="Email" style="width: 100%; padding: 8px; margin-bottom: 8px;">
            <textarea placeholder="Комментарий" style="width: 100%; padding: 8px; margin-bottom: 8px;" rows="2"></textarea>
            <button class="btn-primary" style="width: 100%;">Отправить</button>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section id="production">
    <div class="container">
      <h2 class="fade-in">Наше производство</h2>
      <div style="display: flex; flex-direction: column; gap: 15px;">
        <img src="https://via.placeholder.com/600x400?text=Производство" alt="Производство" style="width: 100%; border-radius: 8px;" class="fade-in">
        <div class="fade-in">
          <p>Мы гордимся нашим производством, где каждый диван создается с вниманием к деталям. Используем только сертифицированные материалы.</p>
          <p>Наши мастера сочетают технологии и ручную работу для уникальности.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="how-to-choose">
    <div class="container">
      <h2 class="fade-in">Как выбрать бескаркасный диван?</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <h3>1. Размеры</h3>
          <p>Подберите под ваше пространство.</p>
        </div>
        <div class="info-box fade-in">
          <h3>2. Материал</h3>
          <p>Экокожа, ткань или велюр.</p>
        </div>
        <div class="info-box fade-in">
          <h3>3. Цвет</h3>
          <p>Синий, желтый или серый.</p>
        </div>
        <div class="info-box fade-in">
          <h3>4. Наполнитель</h3>
          <p>ППУ и синтепон для комфорта.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="about">
    <div class="container">
      <h2 class="fade-in">О компании</h2>
      <div class="grid-1-2">
        <div class="info-box fade-in">
          <p>DIVANEASY создает стильную мебель с 2020 года для вашего уюта.</p>
        </div>
        <div class="info-box fade-in">
          <p>Качество, доступность и индивидуальный подход — наша миссия.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="faq">
    <div class="container">
      <h2 class="fade-in">Часто задаваемые вопросы</h2>
      <div class="staggered-grid">
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>1. Доставка?</h3>
          <div class="accordion-content">От 3 до 7 дней.</div>
        </div>
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>2. Возврат?</h3>
          <div class="accordion-content">14 дней при сохранении вида.</div>
        </div>
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>3. Гарантия?</h3>
          <div class="accordion-content">12 месяцев.</div>
        </div>
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>4. Индивидуальный дизайн?</h3>
          <div class="accordion-content">Свяжитесь с нами.</div>
        </div>
      </div>
    </div>
  </section>

  <section id="reviews">
    <div class="container">
      <h2 class="fade-in">Отзывы</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <p>"Комфорт — лучшее для гостиной!"</p>
          <p>Анна, Москва</p>
        </div>
        <div class="info-box fade-in">
          <p>"Релакс — отличное качество."</p>
          <p>Михаил, Химки</p>
        </div>
        <div class="info-box fade-in">
          <p>"Сириус — изюминка комнаты."</p>
          <p>Екатерина, Зеленоград</p>
        </div>
      </div>
    </div>
  </section>

  <section id="values">
    <div class="container">
      <h2 class="fade-in">Ценности</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <h3>Экологичность</h3>
          <p>Только экологичные материалы.</p>
        </div>
        <div class="info-box fade-in">
          <h3>Индивидуальность</h3>
          <p>Диваны под ваши пожелания.</p>
        </div>
        <div class="info-box fade-in">
          <h3>Доступность</h3>
          <p>Качество по низким ценам.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="promotions">
    <div class="container">
      <h2 class="fade-in">Акции</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <h3>Скидка 10%</h3>
          <p>На первый заказ.</p>
        </div>
        <div class="info-box fade-in">
          <h3>Бесплатная доставка</h3>
          <p>От 50 000 ₽.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="comparison">
    <div class="container">
      <h2 class="fade-in">Сравнение</h2>
      <div class="grid-1-3">
        <div class="info-box fade-in">
          <h3>Комфорт</h3>
          <p>25 000 ₽</p>
          <p>180x90x80 см</p>
          <p>Экокожа</p>
        </div>
        <div class="info-box fade-in">
          <h3>Релакс</h3>
          <p>30 000 ₽</p>
          <p>200x100x85 см</p>
          <p>Ткань</p>
        </div>
        <div class="info-box fade-in">
          <h3>Сириус</h3>
          <p>28 000 ₽</p>
          <p>190x95x80 см</p>
          <p>Велюр</p>
        </div>
      </div>
    </div>
  </section>

  <section id="delivery">
    <div class="container">
      <h2 class="fade-in">Доставка</h2>
      <div class="grid-1-2">
        <div class="info-box fade-in">
          <p>От 3 до 7 дней. Бесплатно от 50 000 ₽.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="payment">
    <div class="container">
      <h2 class="fade-in">Оплата</h2>
      <div class="grid-1-2">
        <div class="info-box fade-in">
          <p>Карта, наличные или рассрочка.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="contact">
    <div class="container">
      <h2 class="fade-in">Контакты</h2>
      <div style="max-width: 100%; margin: 0 auto;">
        <div style="margin-bottom: 10px;" class="fade-in">
          <label for="name" style="display: block; color: #4A504D;">Имя</label>
          <input type="text" id="name" style="width: 100%; padding: 8px; border: 1px solid #D9D9D9; border-radius: 4px;">
        </div>
        <div style="margin-bottom: 10px;" class="fade-in">
          <label for="email" style="display: block; color: #4A504D;">Email</label>
          <input type="email" id="email" style="width: 100%; padding: 8px; border: 1px solid #D9D9D9; border-radius: 4px;">
        </div>
        <div style="margin-bottom: 10px;" class="fade-in">
          <label for="message" style="display: block; color: #4A504D;">Сообщение</label>
          <textarea id="message" style="width: 100%; padding: 8px; border: 1px solid #D9D9D9; border-radius: 4px;" rows="3"></textarea>
        </div>
        <button class="btn-primary fade-in" onclick="submitForm()">Отправить</button>
      </div>
    </div>
  </section>

  <footer style="background: #FDC4BD; color: #4A504D; padding: 15px 0; text-align: center;">
    <div class="container">
      <p>© 2025 DIVANEASY. Все права защищены.</p>
      <p>info@divaneasy.ru | +7 (495) 123-45-67</p>
    </div>
  </footer>

  <a href="#hero" class="back-to-top" id="backToTop">Наверх</a>

  <div class="modal" id="modal">
    <span class="close" onclick="closeModal()">×</span>
    <img class="modal-content" id="modal-image">
  </div>

  <script>
    gsap.registerPlugin(ScrollTrigger);

    document.addEventListener('DOMContentLoaded', () => {
      const hero = document.getElementById('hero');
      const img = new Image();
      img.src = 'https://media.discordapp.net/attachments/1241693361279340545/1380973354244378644/assets_task_01jx5r1285e1892tcgm4pfpdwn_1749318498_img_0.webp?ex=6845d328&is=684481a8&hm=cacc5e5cc8ec7400fa5ed9f6fb8ec18f372da76f70d61ae70d52685dafd5a92a';
      img.onload = () => {
        hero.classList.add('loaded');
        gsap.to('.fade-in', { opacity: 1, y: 0, duration: 0.8, stagger: 0.2 });
      };
      setTimeout(() => {
        if (!hero.classList.contains('loaded')) {
          hero.classList.add('loaded');
          gsap.to('.fade-in', { opacity: 1, y: 0, duration: 0.8, stagger: 0.2 });
        }
      }, 1500);
    });

    gsap.utils.toArray(".fade-in").forEach((element) => {
      ScrollTrigger.create({
        trigger: element,
        start: "top 80%",
        onEnter: () => gsap.to(element, { opacity: 1, y: 0, duration: 0.6 }),
        onEnterBack: () => gsap.to(element, { opacity: 1, y: 0, duration: 0.6 }),
        toggleActions: "play none none reset",
      });
    });

    function changeSofaImage(imageId, newSrc, element) {
      const img = document.getElementById(imageId);
      img.style.opacity = 0;
      setTimeout(() => {
        img.src = newSrc;
        img.style.opacity = 1;
      }, 400);
      document.querySelectorAll('.color-option').forEach(opt => opt.classList.remove('selected'));
      element.classList.add('selected');
    }

    function submitForm() {
      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const message = document.getElementById('message').value;
      if (name && email && message) {
        alert('Сообщение отправлено!');
        document.getElementById('name').value = '';
        document.getElementById('email').value = '';
        document.getElementById('message').value = '';
      } else {
        alert('Заполните все поля.');
      }
    }

    window.addEventListener('scroll', () => {
      const backToTop = document.getElementById('backToTop');
      if (window.pageYOffset > 200) backToTop.classList.add('visible');
      else backToTop.classList.remove('visible');
    });

    function openModal(src) {
      const modal = document.getElementById('modal');
      const modalImage = document.getElementById('modal-image');
      modalImage.src = src;
      modal.style.display = 'flex';
    }

    function closeModal() {
      document.getElementById('modal').style.display = 'none';
    }

    function toggleOrderForm(form) {
      form.classList.toggle('active');
    }

    function toggleMenu() {
      const nav = document.getElementById('main-nav');
      nav.classList.toggle('active');
    }

    document.querySelectorAll('.buy-button').forEach(button => {
      button.addEventListener('click', (e) => {
        const form = e.target.nextElementSibling;
        form.classList.toggle('active');
      });
    });

    document.addEventListener('click', (e) => {
      const forms = document.querySelectorAll('.order-form');
      forms.forEach(form => {
        if (!form.contains(e.target) && !e.target.classList.contains('buy-button')) {
          form.classList.remove('active');
        }
      });
    });
  </script>
</body>
</html>
