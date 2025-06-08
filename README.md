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
    }
    section {
      background: #F8CBC6;
      width: 100%;
      padding: 32px 0; /* Уменьшен для мобильных */
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 16px;
    }
    .hero {
      background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://media.discordapp.net/attachments/1241693361279340545/1380973354244378644/assets_task_01jx5r1285e1892tcgm4pfpdwn_1749318498_img_0.webp?ex=6845d328&is=684481a8&hm=cacc5e5cc8ec7400fa5ed9f6fb8ec18f372da76f70d61ae70d52685dafd5a92a');
      background-size: cover;
      background-position: center;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #FFFFFF;
      text-align: center;
      opacity: 0;
      transition: opacity 1.5s ease-in-out;
    }
    .hero.loaded {
      opacity: 1;
    }
    .color-option {
      width: 30px;
      height: 30px;
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
      transition: opacity 0.5s ease-in-out;
    }
    .fade-in {
      opacity: 0;
      transform: translateY(50px);
    }
    .fade-in.visible {
      opacity: 1;
      transform: translateY(0);
      transition: opacity 0.8s ease, transform 0.8s ease;
    }
    .info-box {
      background: #FFF5E1;
      padding: 16px; /* Уменьшен для мобильных */
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      text-align: center;
      transition: transform 0.3s ease, background 0.3s ease;
    }
    .info-box:hover {
      transform: scale(1.05);
      background: #FFE8CC;
    }
    nav a {
      transition: color 0.3s ease;
      color: #4A504D;
      text-decoration: none;
      padding: 5px 10px;
      font-size: 14px; /* Уменьшен для мобильных */
    }
    nav a:hover {
      color: #E9B7C2;
      transform: translateY(-2px);
    }
    .btn-primary {
      background: #FAB1AA;
      color: #FFFFFF;
      padding: 8px 16px; /* Уменьшен для мобильных */
      border-radius: 50px;
      text-decoration: none;
      display: inline-block;
      font-size: 14px; /* Уменьшен для мобильных */
    }
    .btn-primary:hover {
      background: #FBC8C5;
    }
    header {
      position: fixed;
      top: 0;
      width: 100%;
      background: #F5A9A9;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      z-index: 40;
      padding: 8px 0; /* Уменьшен для мобильных */
    }
    header .container {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    nav {
      display: flex;
      gap: 8px; /* Уменьшен для мобильных */
    }
    .grid-1-3 {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px; /* Уменьшен для мобильных */
    }
    .grid-1-2 {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px; /* Уменьшен для мобильных */
    }
    .grid-1-4 {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 8px; /* Уменьшен для мобильных */
    }
    .staggered-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 24px 12px; /* Уменьшен для мобильных */
    }
    h1 {
      font-size: 1.5rem; /* Уменьшен для мобильных */
      font-weight: 700;
      color: #4A504D;
    }
    h2 {
      font-size: 1.75rem; /* Уменьшен для мобильных */
      font-weight: 700;
      margin-bottom: 24px; /* Уменьшен для мобильных */
      color: #000000;
      text-align: center;
    }
    h3 {
      font-size: 1rem; /* Уменьшен для мобильных */
      font-weight: 600;
      margin-bottom: 12px; /* Уменьшен для мобильных */
      color: #000000;
    }
    p {
      color: #000000;
      font-size: 14px; /* Уменьшен для мобильных */
    }
    ul {
      list-style: disc;
      padding-left: 16px; /* Уменьшен для мобильных */
      color: #000000;
    }
    .accordion {
      background: #FFF5E1;
      padding: 16px;
      border-radius: 10px;
      margin-bottom: 12px;
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
        min-height: 50vh; /* Уменьшен для мобильных */
      }
      header .container {
        flex-direction: column;
        padding: 8px;
      }
      nav {
        flex-direction: column;
        align-items: center;
        gap: 8px;
        margin-top: 8px;
      }
      .grid-1-3, .grid-1-2, .grid-1-4, .staggered-grid {
        grid-template-columns: 1fr;
      }
      .info-box {
        padding: 12px;
      }
      .buy-button {
        width: 100%;
        margin-top: 8px;
      }
      .order-form {
        width: 90%;
        left: 5%;
        transform: none;
      }
      #production {
        flex-direction: column;
      }
      #production img {
        max-width: 100%;
      }
      #production div {
        max-width: 100%;
      }
      h1 {
        font-size: 1.25rem;
      }
      h2 {
        font-size: 1.5rem;
      }
      h3 {
        font-size: 0.875rem;
      }
      p, ul {
        font-size: 12px;
      }
      .container {
        padding: 0 8px;
      }
      .back-to-top {
        bottom: 10px;
        right: 10px;
        padding: 8px 12px;
        font-size: 12px;
      }
    }
    @media (min-width: 769px) {
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
      top: 20px;
      right: 30px;
      font-size: 30px;
      cursor: pointer;
    }
    .buy-button {
      background: #FAB1AA;
      color: #FFFFFF;
      padding: 10px 20px;
      border: none;
      border-radius: 50px;
      cursor: pointer;
      margin-top: 15px;
      transition: background 0.3s;
    }
    .buy-button:hover {
      background: #FBC8C5;
    }
    .order-form {
      display: none;
      position: absolute;
      background: #FFF5E1;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      z-index: 100;
      width: 300px;
      top: 100%;
      left: 50%;
      transform: translateX(-50%);
    }
    .order-form.active {
      display: block;
    }
    .back-to-top {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #FAB1AA;
      color: #FFFFFF;
      padding: 10px 20px;
      border-radius: 50px;
      text-decoration: none;
      display: none;
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
      <nav>
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
      <p style="font-size: 1.5rem; margin-bottom: 24px;" class="fade-in">Бескаркасные диваны для вашего уюта и стиля</p>
      <p style="font-size: 1rem; max-width: 32rem; margin: 0 auto;" class="fade-in">Создавайте уют в своем доме с нашими стильными и комфортными модными диванами.</p>
      <a href="#catalog" class="btn-primary fade-in">Купить сейчас</a>
    </div>
  </section>

  <section id="gallery">
    <div class="container">
      <h2 class="fade-in">Галерея наших диванов</h2>
      <div class="grid-1-4">
        <img src="https://cdn.discordapp.com/attachments/1241693361279340545/1380973354772598876/ChatGPT_Image_5_._2025_._20_17_48.png?ex=6845d329&is=684481a9&hm=1b512366cb070c19a066260e6a9573a6d350ce77d293a0bff0fcd73f1aaf36ad&" alt="Галерея 1" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; cursor: pointer;" onclick="openModal(this.src)" class="fade-in">
        <img src="https://cdn.discordapp.com/attachments/1241693361279340545/1380973503989158110/20250606_0126_Brown_Sofa_Placement_remix_01jx134jx8er0v9y9rkyab921m.png?ex=6845d34c&is=684481cc&hm=ed5bbf88a4151bd351b20c1f193ddc4edf81799a3e1206b00f364a262ccaefb7&" alt="Галерея 2" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; cursor: pointer;" onclick="openModal(this.src)" class="fade-in">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s" alt="Галерея 3" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; cursor: pointer;" onclick="openModal(this.src)" class="fade-in">
        <img src="https://fayni-mebli.com/content/goods/61647/picture-61647.jpg" alt="Галерея 4" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; cursor: pointer;" onclick="openModal(this.src)" class="fade-in">
        <img src="https://tia-sport.com/image/cache/data/beskarkasnaya-mebel/sm-0804/sm-0804-1000x1000.JPG" alt="Галерея 5" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; cursor: pointer;" onclick="openModal(this.src)" class="fade-in">
      </div>
    </div>
  </section>

  <section id="advantages">
    <div class="container">
      <h2 class="fade-in">Почему выбирают DIVANEASY?</h2>
      <div class="grid-1-3">
        <div class="info-box fade-in">
          <img src="https://via.placeholder.com/100?text=Комфорт" alt="Комфорт" style="display: block; margin: 0 auto 16px;">
          <h3>Максимальный комфорт</h3>
          <p>Наши диваны адаптируются к вашему телу, обеспечивая непревзойденный комфорт.</p>
        </div>
        <div class="info-box fade-in">
          <img src="https://via.placeholder.com/100?text=Дизайн" alt="Дизайн" style="display: block; margin: 0 auto 16px;">
          <h3>Стильный дизайн</h3>
          <p>Современные формы и яркие цвета для любого интерьера.</p>
        </div>
        <div class="info-box fade-in">
          <img src="https://via.placeholder.com/100?text=Качество" alt="Качество" style="display: block; margin: 0 auto 16px;">
          <h3>Высокое качество</h3>
          <p>Используем только экологичные и долговечные материалы.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="catalog">
    <div class="container">
      <h2 class="fade-in">Наши диваны</h2>
      <div class="grid-1-3">
        <div class="info-box card">
          <img src="https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg" alt="Диван Комфорт" class="sofa-image" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; margin-bottom: 16px;" id="sofa-image-1">
          <h3>Модель "Комфорт"</h3>
          <p style="margin-bottom: 16px;">Мягкий и уютный бескаркасный диван для вашего дома.</p>
          <div style="margin-bottom: 16px;">
            <p style="font-weight: 600;">Выберите цвет:</p>
            <div style="display: flex; justify-content: center; gap: 8px;">
              <div class="color-option bg-yellow-500" onclick="changeSofaImage('sofa-image-1', 'https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg', this)"></div>
              <div class="color-option bg-gray-500" onclick="changeSofaImage('sofa-image-1', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s', this)"></div>
              <div class="color-option bg-blue-500" onclick="changeSofaImage('sofa-image-1', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s', this)"></div>
            </div>
          </div>
          <p><strong>Характеристики:</strong></p>
          <ul style="max-width: 16rem; margin: 0 auto; text-align: left;">
            <li>Размеры: 180x90x80 см</li>
            <li>Материал: Экокожа</li>
            <li>Наполнитель: ППУ высокой плотности</li>
          </ul>
          <p style="margin-top: 16px;"><strong>Цена:</strong> 25 000 ₽</p>
          <button class="buy-button" onclick="toggleOrderForm(this.nextElementSibling)">Купить</button>
          <div class="order-form">
            <input type="text" placeholder="Имя" style="width: 100%; padding: 8px; margin-bottom: 10px;">
            <input type="email" placeholder="Email" style="width: 100%; padding: 8px; margin-bottom: 10px;">
            <textarea placeholder="Комментарий" style="width: 100%; padding: 8px; margin-bottom: 10px;" rows="3"></textarea>
            <button onclick="submitOrder(this.parentElement)" class="btn-primary" style="width: 100%;">Отправить заявку</button>
          </div>
        </div>
        <div class="info-box card">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s" alt="Диван Релакс" class="sofa-image" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; margin-bottom: 16px;" id="sofa-image-2">
          <h3>Модель "Релакс"</h3>
          <p style="margin-bottom: 16px;">Идеальный выбор для отдыха и расслабления.</p>
          <div style="margin-bottom: 16px;">
            <p style="font-weight: 600;">Выберите цвет:</p>
            <div style="display: flex; justify-content: center; gap: 8px;">
              <div class="color-option bg-gray-500" onclick="changeSofaImage('sofa-image-2', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s', this)"></div>
              <div class="color-option bg-yellow-500" onclick="changeSofaImage('sofa-image-2', 'https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg', this)"></div>
              <div class="color-option bg-blue-500" onclick="changeSofaImage('sofa-image-2', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s', this)"></div>
            </div>
          </div>
          <p><strong>Характеристики:</strong></p>
          <ul style="max-width: 16rem; margin: 0 auto; text-align: left;">
            <li>Размеры: 200x100x85 см</li>
            <li>Материал: Ткань</li>
            <li>Наполнитель: Синтепон</li>
          </ul>
          <p style="margin-top: 16px;"><strong>Цена:</strong> 30 000 ₽</p>
          <button class="buy-button" onclick="toggleOrderForm(this.nextElementSibling)">Купить</button>
          <div class="order-form">
            <input type="text" placeholder="Имя" style="width: 100%; padding: 8px; margin-bottom: 10px;">
            <input type="email" placeholder="Email" style="width: 100%; padding: 8px; margin-bottom: 10px;">
            <textarea placeholder="Комментарий" style="width: 100%; padding: 8px; margin-bottom: 10px;" rows="3"></textarea>
            <button onclick="submitOrder(this.parentElement)" class="btn-primary" style="width: 100%;">Отправить заявку</button>
          </div>
        </div>
        <div class="info-box card">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s" alt="Диван Сириус" class="sofa-image" style="width: 100%; height: 192px; object-fit: cover; border-radius: 8px; margin-bottom: 16px;" id="sofa-image-3">
          <h3>Модель "Сириус"</h3>
          <p style="margin-bottom: 16px;">Яркий и современный диван для вашего пространства.</p>
          <div style="margin-bottom: 16px;">
            <p style="font-weight: 600;">Выберите цвет:</p>
            <div style="display: flex; justify-content: center; gap: 8px;">
              <div class="color-option bg-blue-500" onclick="changeSofaImage('sofa-image-3', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSHVppv5ya8INk0uwl7iB3JCUkGTANvAlC1vA&s', this)"></div>
              <div class="color-option bg-yellow-500" onclick="changeSofaImage('sofa-image-3', 'https://www.restmebel.ru/upload/iblock/04a/04ae9911648e1ed02420b4fec4667485.jpg', this)"></div>
              <div class="color-option bg-gray-500" onclick="changeSofaImage('sofa-image-3', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXan-5GDRK435d5Fg7B6IZ3_-7LTpx8sBm5g&s', this)"></div>
            </div>
          </div>
          <p><strong>Характеристики:</strong></p>
          <ul style="max-width: 16rem; margin: 0 auto; text-align: left;">
            <li>Размеры: 190x95x80 см</li>
            <li>Материал: Велюр</li>
            <li>Наполнитель: ППУ и синтепон</li>
          </ul>
          <p style="margin-top: 16px;"><strong>Цена:</strong> 28 000 ₽</p>
          <button class="buy-button" onclick="toggleOrderForm(this.nextElementSibling)">Купить</button>
          <div class="order-form">
            <input type="text" placeholder="Имя" style="width: 100%; padding: 8px; margin-bottom: 10px;">
            <input type="email" placeholder="Email" style="width: 100%; padding: 8px; margin-bottom: 10px;">
            <textarea placeholder="Комментарий" style="width: 100%; padding: 8px; margin-bottom: 10px;" rows="3"></textarea>
            <button onclick="submitOrder(this.parentElement)" class="btn-primary" style="width: 100%;">Отправить заявку</button>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section id="production">
    <div class="container">
      <h2 class="fade-in">Наше производство</h2>
      <div style="display: flex; flex-direction: column; gap: 32px;" class="md:flex-row md:items-center">
        <img src="https://via.placeholder.com/600x400?text=Производство" alt="Производство" style="width: 100%; max-width: 50%; border-radius: 8px;" class="fade-in">
        <div style="width: 100%; max-width: 50%;" class="fade-in">
          <p style="font-size: 1.125rem; margin-bottom: 16px; color: #4A504D;">Мы гордимся нашим производством, где каждый диван создается с вниманием к деталям. Используем только сертифицированные материалы, чтобы обеспечить долговечность и безопасность.</p>
          <p style="font-size: 1.125rem; color: #4A504D;">Наши мастера сочетают современные технологии и ручную работу, чтобы каждый диван был не только удобным, но и уникальным.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="how-to-choose">
    <div class="container">
      <h2 class="fade-in">Как выбрать бескаркасный диван?</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <h3>1. Определите размеры</h3>
          <p>Убедитесь, что диван подходит для вашего пространства.</p>
        </div>
        <div class="info-box fade-in">
          <h3>2. Выберите материал</h3>
          <p>Экокожа, ткань или велюр — выбирайте по предпочтениям.</p>
        </div>
        <div class="info-box fade-in">
          <h3>3. Подберите цвет</h3>
          <p>Гармонируйте с интерьером: синий, желтый или серый.</p>
        </div>
        <div class="info-box fade-in">
          <h3>4. Проверьте наполнитель</h3>
          <p>ППУ и синтепон для комфорта и долговечности.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="about">
    <div class="container">
      <h2 class="fade-in">О компании</h2>
      <div class="grid-1-2">
        <div class="info-box fade-in">
          <p style="margin-bottom: 16px;">DIVANEASY — это команда профессионалов, создающих комфортную и стильную мебель с 2020 года. Мы стремимся сделать ваш дом уютнее!</p>
        </div>
        <div class="info-box fade-in">
          <p style="margin-bottom: 16px;">Наша миссия — сочетание качества, доступности и индивидуального подхода к каждому клиенту.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="faq">
    <div class="container">
      <h2 class="fade-in">Часто задаваемые вопросы</h2>
      <div class="staggered-grid">
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>1. Как долго доставляют диван?</h3>
          <div class="accordion-content">Доставка занимает от 3 до 7 рабочих дней.</div>
        </div>
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>2. Можно ли вернуть диван?</h3>
          <div class="accordion-content">Да, в течение 14 дней при сохранении вида.</div>
        </div>
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>3. Есть ли гарантия?</h3>
          <div class="accordion-content">Гарантия 12 месяцев на все изделия.</div>
        </div>
        <div class="accordion fade-in" onclick="this.classList.toggle('active')">
          <h3>4. Индивидуальный дизайн?</h3>
          <div class="accordion-content">Свяжитесь для обсуждения.</div>
        </div>
      </div>
    </div>
  </section>

  <section id="reviews">
    <div class="container">
      <h2 class="fade-in">Отзывы наших клиентов</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <p style="margin-bottom: 16px;">"Диван 'Комфорт' — лучшее решение для гостиной!"</p>
          <p style="font-weight: 600;">Анна, Москва</p>
        </div>
        <div class="info-box fade-in">
          <p style="margin-bottom: 16px;">"Релакс превзошёл ожидания по качеству."</p>
          <p style="font-weight: 600;">Михаил, Химки</p>
        </div>
        <div class="info-box fade-in">
          <p style="margin-bottom: 16px;">"Сириус в синем — изюминка комнаты."</p>
          <p style="font-weight: 600;">Екатерина, Зеленоград</p>
        </div>
      </div>
    </div>
  </section>

  <section id="values">
    <div class="container">
      <h2 class="fade-in">Наши ценности</h2>
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
          <p>Качество по доступным ценам.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="promotions">
    <div class="container">
      <h2 class="fade-in">Акции и скидки</h2>
      <div class="staggered-grid">
        <div class="info-box fade-in">
          <h3>Скидка 10%</h3>
          <p>На первый заказ при регистрации.</p>
        </div>
        <div class="info-box fade-in">
          <h3>Бесплатная доставка</h3>
          <p>При заказе от 50 000 ₽.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="comparison">
    <div class="container">
      <h2 class="fade-in">Сравнение моделей</h2>
      <div class="grid-1-3">
        <div class="info-box fade-in">
          <h3>Комфорт</h3>
          <p>Цена: 25 000 ₽</p>
          <p>Размеры: 180x90x80 см</p>
          <p>Материал: Экокожа</p>
        </div>
        <div class="info-box fade-in">
          <h3>Релакс</h3>
          <p>Цена: 30 000 ₽</p>
          <p>Размеры: 200x100x85 см</p>
          <p>Материал: Ткань</p>
        </div>
        <div class="info-box fade-in">
          <h3>Сириус</h3>
          <p>Цена: 28 000 ₽</p>
          <p>Размеры: 190x95x80 см</p>
          <p>Материал: Велюр</p>
        </div>
      </div>
    </div>
  </section>

  <section id="delivery">
    <div class="container">
      <h2 class="fade-in">Доставка</h2>
      <div class="grid-1-2">
        <div class="info-box fade-in">
          <p>Доставка по России от 3 до 7 дней. Бесплатно при заказе от 50 000 ₽.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="payment">
    <div class="container">
      <h2 class="fade-in">Оплата</h2>
      <div class="grid-1-2">
        <div class="info-box fade-in">
          <p>Онлайн-оплата картой, наличные при доставке или рассрочка.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="contact">
    <div class="container">
      <h2 class="fade-in">Контакты</h2>
      <div style="max-width: 600px; margin: 0 auto;">
        <div style="margin-bottom: 16px;" class="fade-in">
          <label for="name" style="display: block; color: #4A504D;">Имя</label>
          <input type="text" id="name" style="width: 100%; padding: 8px; border: 1px solid #D9D9D9; border-radius: 4px; background: #FFFFFF; color: #000000;" required>
        </div>
        <div style="margin-bottom: 16px;" class="fade-in">
          <label for="email" style="display: block; color: #4A504D;">Email</label>
          <input type="email" id="email" style="width: 100%; padding: 8px; border: 1px solid #D9D9D9; border-radius: 4px; background: #FFFFFF; color: #000000;" required>
        </div>
        <div style="margin-bottom: 16px;" class="fade-in">
          <label for="message" style="display: block; color: #4A504D;">Сообщение</label>
          <textarea id="message" style="width: 100%; padding: 8px; border: 1px solid #D9D9D9; border-radius: 4px; resize: vertical; background: #FFFFFF; color: #000000;" rows="4" required></textarea>
        </div>
        <button type="submit" onclick="submitForm()" class="btn-primary fade-in">Отправить</button>
      </div>
    </div>
  </section>

  <footer style="background: #FDC4BD; color: #4A504D; padding: 16px 0;">
    <div class="container" style="text-align: center;">
      <p class="fade-in">© 2025 DIVANEASY. Все права защищены.</p>
      <p class="fade-in">info@divaneasy.ru | +7 (495) 123-45-67</p>
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
        gsap.to('.fade-in', { opacity: 1, y: 0, duration: 1, stagger: 0.2 });
      };
      setTimeout(() => {
        if (!hero.classList.contains('loaded')) {
          hero.classList.add('loaded');
          gsap.to('.fade-in', { opacity: 1, y: 0, duration: 1, stagger: 0.2 });
        }
      }, 1500);
    });

    gsap.utils.toArray(".fade-in").forEach((element) => {
      ScrollTrigger.create({
        trigger: element,
        start: "top 80%",
        onEnter: () => gsap.fromTo(element, { opacity: 0, y: 50 }, { opacity: 1, y: 0, duration: 0.8, ease: "power2.out" }),
        onEnterBack: () => gsap.fromTo(element, { opacity: 0, y: 50 }, { opacity: 1, y: 0, duration: 0.8, ease: "power2.out" }),
        toggleActions: "play none none reset",
      });
    });

    function changeSofaImage(imageId, newSrc, element) {
      const img = document.getElementById(imageId);
      img.style.opacity = 0;
      setTimeout(() => {
        img.src = newSrc;
        img.style.opacity = 1;
      }, 500);
      document.querySelectorAll('.color-option').forEach(opt => opt.classList.remove('selected'));
      element.classList.add('selected');
    }

    function submitForm() {
      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const message = document.getElementById('message').value;
      if (name && email && message) {
        alert('Сообщение отправлено! Мы свяжемся с вами скоро.');
        document.getElementById('name').value = '';
        document.getElementById('email').value = '';
        document.getElementById('message').value = '';
      } else {
        alert('Пожалуйста, заполните все поля.');
      }
    }

    window.addEventListener('scroll', () => {
      const backToTop = document.getElementById('backToTop');
      if (window.pageYOffset > 300) {
        backToTop.classList.add('visible');
      } else {
        backToTop.classList.remove('visible');
      }
    });

    function openModal(src) {
      const modal = document.getElementById('modal');
      const modalImage = document.getElementById('modal-image');
      modalImage.src = src;
      modal.style.display = 'flex';
    }

    function closeModal() {
      const modal = document.getElementById('modal');
      modal.style.display = 'none';
    }

    function toggleOrderForm(form) {
      form.classList.toggle('active');
    }

    function submitOrder(form) {
      const name = form.querySelector('input[type="text"]').value;
      const email = form.querySelector('input[type="email"]').value;
      const comment = form.querySelector('textarea').value;
      if (name && email && comment) {
        alert('Заявка отправлена! Мы свяжемся с вами скоро.');
        form.classList.remove('active');
        form.querySelector('input[type="text"]').value = '';
        form.querySelector('input[type="email"]').value = '';
        form.querySelector('textarea').value = '';
      } else {
        alert('Пожалуйста, заполните все поля.');
      }
    }

    document.addEventListener('click', (e) => {
      const forms = document.querySelectorAll('.order-form');
      forms.forEach(form => {
        if (!form.closest('.info-box').contains(e.target) && !e.target.classList.contains('buy-button')) {
          form.classList.remove('active');
        }
      });
    });
  </script>
</body>
</html>
