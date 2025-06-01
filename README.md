<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Конфигуратор мебельной фурнитуры</title>
  <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/client.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/babel-standalone@6.26.0/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.68/pdfmake.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.68/vfs_fonts.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/i18next@23.11.5/dist/umd/i18next.min.js"></script>
  <style>
    body.dark-theme {
      background: linear-gradient(135deg, #0d1b2a, #1b263b);
      color: #e0e1dd;
    }
    body.light-theme {
      background: linear-gradient(135deg, #E8F5E9, #C8E6C9);
      color: #333;
    }
    .container-overlay, .welcome-container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 1.5rem;
      padding: 2rem;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8);
      backdrop-filter: blur(10px);
      max-width: 100rem;
      width: 100%;
      margin: 2rem auto;
      transition: all 0.5s ease;
    }
    .light-theme .container-overlay, .light-theme .welcome-container {
      background: rgba(255, 255, 255, 0.8);
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
    }
    .welcome-container {
      max-width: 45rem;
      text-align: center;
      animation: fadeInSlide 1s ease-out;
    }
    .light-theme .welcome-container p {
      color: #000000;
    }
    .configurator-container {
      margin-top: 8rem;
      animation: fadeInUp 1s ease-out;
      width: 100%;
      min-height: 60rem;
    }
    .edge-animation {
      position: fixed;
      top: 0;
      bottom: 0;
      width: 60px;
      background: linear-gradient(90deg, rgba(0, 255, 150, 0.3), transparent);
      animation: edgePulse 4s ease-in-out infinite;
      pointer-events: none;
    }
    .edge-animation.left { left: 0; }
    .edge-animation.right { right: 0; background: linear-gradient(270deg, rgba(138, 43, 226, 0.3), transparent); }
    .navbar {
      position: fixed;
      top: 0;
      width: 100%;
      background: linear-gradient(90deg, #0d1b2a, #415a77);
      padding: 1.5rem 3rem;
      z-index: 20;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
      overflow-x: auto;
      white-space: nowrap;
    }
    .light-theme .navbar {
      background: linear-gradient(90deg, #2E7D32, #43A047);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    }
    .navbar a, .navbar button {
      font-size: 1rem;
      white-space: nowrap;
    }
    .price-inputs-container {
      display: flex;
      flex-direction: row;
      gap: 2rem;
      align-items: center;
      justify-content: center;
      margin-bottom: 2rem;
      width: 100%;
    }
    .price-inputs-container div {
      flex: 1;
      max-width: 200px;
    }
    .tab-buttons {
      display: flex;
      gap: 0.5rem;
      align-items: center;
      justify-content: center;
      padding: 1rem 0;
      margin-top: 1rem;
      overflow-x: auto;
      white-space: nowrap;
    }
    .tab-button, .gradient-button {
      background: linear-gradient(45deg, #00ff96, #8a2be2);
      background-size: 200% 200%;
      animation: pulse 2s infinite;
      border-radius: 0.75rem;
      padding: 0.75rem 1.5rem;
      cursor: pointer;
      transition: all 0.3s ease;
      font-weight: 600;
      border: 2px solid transparent;
      text-align: center;
    }
    .tab-button.active {
      transform: scale(1.05);
      border-color: #00ff96;
    }
    .tab-button span, .gradient-button span {
      color: white;
      white-space: nowrap;
    }
    .tab-content {
      margin-top: 18rem;
    }
    .tooltip .tooltip-text {
      visibility: hidden;
      width: 220px;
      background: rgba(0, 0, 0, 0.7);
      color: #d1e8e2;
      text-align: center;
      border-radius: 0.5rem;
      padding: 0.5rem;
      position: absolute;
      z-index: 1;
      left: -240px;
      top: 50%;
      transform: translateY(-50%);
      opacity: 0;
      transition: opacity 0.3s ease;
    }
    .light-theme .tooltip .tooltip-text {
      background: rgba(255, 255, 255, 0.9);
      color: #333;
    }
    .tooltip:hover .tooltip-text { visibility: visible; opacity: 0.95; }
    @keyframes fadeInSlide { 0% { opacity: 0; transform: translateY(20px); } 100% { opacity: 1; transform: translateY(0); } }
    @keyframes fadeInUp { 0% { opacity: 0; transform: translateY(30px); } 100% { opacity: 1; transform: translateY(0); } }
    @keyframes slideUp { 0% { opacity: 0; transform: translateY(50px); } 100% { opacity: 1; transform: translateY(0); } }
    @keyframes tourFadeIn { 0% { opacity: 0; transform: scale(0.9); } 100% { opacity: 1; transform: scale(1); } }
    @keyframes edgePulse { 0% { opacity: 0.4; } 50% { opacity: 0.9; } 100% { opacity: 0.4; } }
    @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }
    @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    @media (max-width: 640px) {
      .container-overlay, .welcome-container { padding: 1rem; margin: 3rem 0.5rem; }
      .welcome-container { margin: 5rem 0.5rem; }
      h1 { font-size: 1.8rem; }
      h2 { font-size: 1.3rem; }
      .grid-cols-3 { grid-template-columns: 1fr; }
      .edge-animation { display: none; }
      .navbar { padding: 0.75rem 1rem; }
      .navbar a, .navbar button { font-size: 0.85rem; padding: 0.5rem; }
      .price-inputs-container { flex-direction: column; align-items: center; }
      .price-inputs-container div { max-width: 100%; }
      .tab-buttons { flex-direction: row; width: 100%; margin-top: 1rem; padding: 0.5rem 0; overflow-x: auto; }
      .tab-button { min-width: 100px; text-align: center; padding: 0.5rem 1rem; font-size: 0.85rem; }
      .tab-content { margin-top: 12rem; }
      select, input { font-size: 1rem; padding: 0.75rem; width: 100%; }
      button { font-size: 1rem; padding: 0.75rem; width: 100%; }
      .configurator-container { margin-top: 6rem; }
    }
    .about-container {
      background: linear-gradient(135deg, rgba(0, 255, 150, 0.2), rgba(0, 0, 0, 0.9));
      border-radius: 1.5rem;
      padding: 2.5rem;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8);
      text-align: center;
      max-width: 55rem;
      margin: 7rem auto;
      animation: fadeInSlide 1s ease-out;
    }
    .light-theme .about-container {
      background: linear-gradient(135deg, rgba(0, 255, 150, 0.1), rgba(255, 255, 255, 0.9));
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
    }
    .about-container h2 { font-size: 2.2rem; color: #00ff96; margin-bottom: 1.5rem; }
    .light-theme .about-container h2 { color: #8a2be2; }
    .about-container p { font-size: 1.2rem; color: #d1e8e2; line-height: 1.7; }
    .light-theme .about-container p { color: #333; }
    .promotion-card {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 1rem;
      padding: 1.5rem 9rem;
      margin: 1rem 0;
      width: 100%;
      max-width: 270rem;
      transition: transform 0.4s ease, box-shadow 0.4s ease;
      animation: slideUp 0.8s ease-out;
    }
    .light-theme .promotion-card {
      background: rgba(255, 255, 255, 0.8);
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    }
    .light-theme .promotion-card p {
      color: #000000;
    }
    .promotion-card:hover { transform: translateY(-10px) rotate(2deg); box-shadow: 0 15px 30px rgba(0, 255, 150, 0.3); }
    .tour-container {
      max-width: 90rem;
      margin: 7rem auto;
      text-align: center;
      opacity: 0;
      animation: tourFadeIn 1s ease-out forwards;
      animation-delay: 0.2s;
    }
    .tour-container iframe {
      width: 100%;
      height: 450px;
      border-radius: 1.5rem;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8);
      transition: transform 0.5s ease;
    }
    .light-theme .tour-container iframe {
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
    }
    .tour-container:hover iframe { transform: scale(1.02); }
    @media (max-width: 640px) {
      .tour-container { margin: 5rem 0.5rem; }
      .tour-container iframe { height: 300px; }
      .promotion-card { padding: 1rem 3rem; max-width: 90rem; }
    }
    .sort-dropdown {
      position: relative;
      display: inline-block;
    }
    .sort-dropdown button {
      background: linear-gradient(45deg, #00ff96, #8a2be2);
      color: white;
      padding: 0.75rem 1.5rem;
      border-radius: 0.75rem;
      cursor: pointer;
      transition: all 0.3s ease;
      animation: pulse 2s infinite;
    }
    .sort-dropdown:hover .dropdown-content {
      display: block;
    }
    .dropdown-content {
      display: none;
      position: absolute;
      background: rgba(0, 0, 0, 0.7);
      min-width: 160px;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
      z-index: 1;
      border-radius: 0.5rem;
    }
    .light-theme .dropdown-content {
      background: rgba(255, 255, 255, 0.9);
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
    }
    .dropdown-content a {
      color: #d1e8e2;
      padding: 0.5rem 1rem;
      text-decoration: none;
      display: block;
    }
    .light-theme .dropdown-content a {
      color: #333;
    }
    .dropdown-content a:hover { background: #00ff96; color: #1b263b; }
    .light-theme .dropdown-content a:hover { background: #8a2be2; color: #fff; }
    .configurator-label {
      font-size: 1.5rem;
      font-weight: 600;
      background: linear-gradient(90deg, #00ff96, #8a2be2);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }
    .light-theme .configurator-label {
      background: linear-gradient(90deg, #00ff96, #8a2be2);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }
    .fittings-image {
      height: 200px;
      object-fit: cover;
      object-position: center;
      cursor: pointer;
    }
    .light-theme .bg-gray-100 {
      background-color: #DCFCE7;
    }
    .dark-theme .price-text {
      color: #A855F7;
    }
    .light-theme .card-description p {
      color: #FFFFFF !important;
    }
    .light-theme .card-description h3 {
      color: #FFFFFF !important;
    }
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
    .modal img {
      max-width: 90%;
      max-height: 90%;
      border-radius: 1rem;
    }
    .modal-close {
      position: absolute;
      top: 20px;
      right: 20px;
      color: white;
      font-size: 2rem;
      cursor: pointer;
    }
    .loader {
      border: 4px solid #f3f3f3;
      border-top: 4px solid #00ff96;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      animation: spin 1s linear infinite;
      margin: 0 auto;
    }
    .calculator-container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 1rem;
      padding: 1.5rem;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
      max-width: 60rem;
      margin: 2rem auto;
    }
    .light-theme .calculator-container {
      background: rgba(255, 255, 255, 0.8);
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    }
    .database-window {
      background: rgba(0, 0, 0, 0.7);
      padding: 1rem;
      border-radius: 0.5rem;
      margin-top: 1rem;
      max-height: 20rem;
      overflow-y: auto;
    }
    .light-theme .database-window {
      background: rgba(255, 255, 255, 0.9);
    }
    .database-item {
      padding: 0.5rem;
      border-bottom: 1px solid #444;
    }
    .light-theme .database-item {
      border-bottom: 1px solid #ccc;
    }
  </style>
</head>
<body className="flex flex-col min-h-screen p-4 dark-theme">
  <div className="edge-animation left"></div>
  <div className="edge-animation right"></div>
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useEffect } = React;
    const { createRoot } = ReactDOM;

    i18next.init({
      lng: 'ru',
      resources: {
        ru: {
          translation: {
            "welcome_title": "Добро пожаловать!",
            "welcome_text": "Откройте мир мебельной фурнитуры с нашим удобным конфигуратором.",
            "go_to_configurator": "Начать",
            "configurator_title": "Конфигуратор мебельной фурнитуры",
            "type_label": "Тип фурнитуры",
            "type_placeholder": "Выберите тип",
            "type_tooltip": "Выберите тип, например, петли или ручки.",
            "brand_label": "Бренд",
            "brand_placeholder": "Выберите бренд",
            "brand_tooltip": "Выберите бренд, например, GTV или BOYARD.",
            "specific_option_label": "Опции",
            "specific_option_placeholder": "Выберите опцию",
            "specific_option_tooltip": "Уточните характеристики, например, доводчик.",
            "price_range_from": "Цена от (руб.)",
            "price_range_to": "Цена до (руб.)",
            "find_options": "Найти",
            "results_title": "Найденные варианты:",
            "tab_results": "Результаты",
            "tab_saved": "Сохранённые",
            "tab_compare": "Сравнение",
            "tab_history": "История поиска",
            "tab_calculator": "Калькулятор",
            "save": "Сохранить",
            "compare": "Сравнить",
            "remove_compare": "Убрать из сравнения",
            "saved_results_title": "Сохранённые варианты:",
            "compare_title": "Сравнение товаров:",
            "history_title": "История поиска:",
            "export_selected_pdf": "Экспорт в PDF",
            "export_all_pdf": "Экспорт в PDF",
            "no_match": "Совпадений нет. Показываем похожие.",
            "price_disclaimer": "*Цена может варьироваться",
            "about_title": "О нас",
            "about_text": "Мы представляем свой конфигуратор, который может помочь вам с первичными видами и типами определенной мебельной фурнитуры, поможем узнать среднюю цену, анализируя разные сайты поставщиков и продавцов. Наша цель — упростить ваш выбор, предоставляя актуальную информацию и удобный интерфейс для поиска идеального решения.",
            "nav_home": "Главная",
            "nav_configurator": "Конфигуратор",
            "nav_about": "О нас",
            "nav_warehouse": "Виртуальный тур",
            "nav_promotions": "Акции",
            "nav_contacts": "Контакты",
            "lang_ru": "Русский",
            "lang_en": "English",
            "theme_dark": "Темная тема",
            "theme_light": "Светлая тема",
            "name_label": "Наименование",
            "type": "Петля",
            "subtype_label": "Подтип",
            "option": "Опция",
            "brand": "Бренд",
            "price": "Цена",
            "description": "Описание",
            "promotions_title": "Акции и скидки",
            "promotion_gtv_discount": "Скидка 20% на GTV до 2025-06-15",
            "promotion_free_delivery": "Бесплатная доставка от 7000 руб. до 2025-06-20",
            "warehouse_title": "Виртуальный тур по складу",
            "warehouse_text": "Погрузитесь в мир нашей фурнитуры!",
            "contacts_title": "Свяжитесь с нами",
            "contacts_text": "Оставьте заявку для консультации.",
            "sort_asc": "По возрастанию",
            "sort_desc": "По убыванию",
            "sort_label": "Сортировать",
            "search_label": "Поиск по названию",
            "clear_filters": "Очистить фильтры",
            "calculator_title": "Калькулятор цен",
            "calculator_input_label": "Введите сумму заказа:",
            "calculator_percent_5": "+5%",
            "calculator_percent_10": "+10%",
            "calculator_percent_15": "+15%",
            "calculator_percent_20": "+20%",
            "calculator_undo": "Отмена",
            "calculator_save": "Сохранить",
            "calculator_saved_title": "Сохраненные суммы",
            "calculator_no_saved": "Нет сохраненных сумм.",
            "calculator_error": "Пожалуйста, введите корректную сумму.",
            "calculator_select_percent": "Выберите процент наценки.",
            "calculator_final_price": "Итоговая цена с наценкой ({percentages}%):",
            "database_title": "База данных фурнитуры"
          }
        },
        en: {
          translation: {
            "welcome_title": "Welcome!",
            "welcome_text": "Discover the world of furniture fittings with our configurator.",
            "go_to_configurator": "Start",
            "configurator_title": "Furniture Fittings Configurator",
            "type_label": "Fitting Type",
            "type_placeholder": "Select type",
            "type_tooltip": "Select a type, e.g., hinges or handles.",
            "brand_label": "Brand",
            "brand_placeholder": "Select brand",
            "brand_tooltip": "Select a brand, e.g., GTV or BOYARD.",
            "specific_option_label": "Options",
            "specific_option_placeholder": "Select option",
            "specific_option_tooltip": "Specify features, e.g., soft-close.",
            "price_range_from": "Price from (RUB)",
            "price_range_to": "Price to (RUB)",
            "find_options": "Find",
            "results_title": "Found Options:",
            "tab_results": "Results",
            "tab_saved": "Saved",
            "tab_compare": "Compare",
            "tab_history": "Search History",
            "tab_calculator": "Calculator",
            "save": "Save",
            "compare": "Compare",
            "remove_compare": "Remove from Compare",
            "saved_results_title": "Saved Options:",
            "compare_title": "Compare Items:",
            "history_title": "Search History:",
            "export_selected_pdf": "Export to PDF",
            "export_all_pdf": "Export to PDF",
            "no_match": "No matches. Showing similar.",
            "price_disclaimer": "*Price may vary",
            "about_title": "About Us",
            "about_text": "We present our configurator, which can help you with primary types and categories of specific furniture fittings, helping you determine average prices by analyzing various supplier and seller websites. Our goal is to simplify your choice by providing up-to-date information and a user-friendly interface to find the perfect solution.",
            "nav_home": "Home",
            "nav_configurator": "Configurator",
            "nav_about": "About Us",
            "nav_warehouse": "Virtual Tour",
            "nav_promotions": "Promotions",
            "nav_contacts": "Contacts",
            "lang_ru": "Русский",
            "lang_en": "English",
            "theme_dark": "Dark Theme",
            "theme_light": "Light Theme",
            "name_label": "Name",
            "type": "Hinge",
            "subtype_label": "Subtype",
            "option": "Option",
            "brand": "Brand",
            "price": "Price",
            "description": "Description",
            "promotions_title": "Promotions and Discounts",
            "promotion_gtv_discount": "20% off GTV until 2025-06-15",
            "promotion_free_delivery": "Free delivery over 7000 RUB until 2025-06-20",
            "warehouse_title": "Virtual Warehouse Tour",
            "warehouse_text": "Dive into our fittings world!",
            "contacts_title": "Contact Us",
            "contacts_text": "Leave a request for consultation.",
            "sort_asc": "Ascending",
            "sort_desc": "Descending",
            "sort_label": "Sort",
            "search_label": "Search by name",
            "clear_filters": "Clear Filters",
            "calculator_title": "Price Calculator",
            "calculator_input_label": "Enter order amount:",
            "calculator_percent_5": "+5%",
            "calculator_percent_10": "+10%",
            "calculator_percent_15": "+15%",
            "calculator_percent_20": "+20%",
            "calculator_undo": "Undo",
            "calculator_save": "Save",
            "calculator_saved_title": "Saved Amounts",
            "calculator_no_saved": "No saved amounts.",
            "calculator_error": "Please enter a valid amount.",
            "calculator_select_percent": "Select a markup percentage.",
            "calculator_final_price": "Final price with markup ({percentages}%):",
            "database_title": "Furniture Database"
          }
        }
      }
    });

    const Navbar = ({ setCurrentPage, onLanguageChange, toggleTheme, currentTheme }) => {
      const changeLanguage = (lng) => {
        i18next.changeLanguage(lng);
        onLanguageChange(lng);
        document.querySelectorAll('[data-i18n]').forEach(elem => {
          elem.textContent = i18next.t(elem.getAttribute('data-i18n'));
        });
      };

      return (
        React.createElement('nav', { className: 'navbar flex gap-4' },
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('welcome'),
            className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
            'data-i18n': 'nav_home'
          }, i18next.t('nav_home')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('configurator'),
            className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
            'data-i18n': 'nav_configurator'
          }, i18next.t('nav_configurator')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('about'),
            className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
            'data-i18n': 'nav_about'
          }, i18next.t('nav_about')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('warehouse'),
            className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
            'data-i18n': 'nav_warehouse'
          }, i18next.t('nav_warehouse')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('promotions'),
            className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
            'data-i18n': 'nav_promotions'
          }, i18next.t('nav_promotions')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('contacts'),
            className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
            'data-i18n': 'nav_contacts'
          }, i18next.t('nav_contacts')),
          React.createElement('div', { className: 'flex gap-2' },
            React.createElement('button', {
              onClick: () => changeLanguage('ru'),
              className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
              'data-i18n': 'lang_ru'
            }, i18next.t('lang_ru')),
            React.createElement('button', {
              onClick: () => changeLanguage('en'),
              className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
              'data-i18n': 'lang_en'
            }, i18next.t('lang_en')),
            React.createElement('button', {
              onClick: toggleTheme,
              className: 'text-white hover:text-green-400 transition duration-300 light-theme:text-black light-theme:hover:text-purple-600',
              'data-i18n': currentTheme === 'dark' ? 'theme_light' : 'theme_dark'
            }, i18next.t(currentTheme === 'dark' ? 'theme_light' : 'theme_dark'))
          )
        )
      );
    };

    const WelcomePage = ({ onNavigate }) => {
      return (
        React.createElement('div', { className: 'welcome-container' },
          React.createElement('h1', {
            className: 'text-5xl font-bold mb-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
            'data-i18n': 'welcome_title'
          }, i18next.t('welcome_title')),
          React.createElement('p', { className: 'text-gray-300 mb-10 light-theme:text-gray-600', 'data-i18n': 'welcome_text' }, i18next.t('welcome_text')),
          React.createElement('button', {
            onClick: onNavigate,
            className: 'gradient-button'
          },
            React.createElement('span', { 'data-i18n': 'go_to_configurator' }, i18next.t('go_to_configurator'))
          )
        )
      );
    };

    const Modal = ({ imageSrc, onClose }) => {
      return (
        React.createElement('div', { className: 'modal' },
          React.createElement('span', { className: 'modal-close', onClick: onClose }, '×'),
          React.createElement('img', { src: imageSrc, alt: 'Preview' })
        )
      );
    };

    const ConfiguratorPage = ({ language }) => {
      const [type, setType] = useState('');
      const [specificOption, setSpecificOption] = useState('');
      const [brand, setBrand] = useState('');
      const [priceRange, setPriceRange] = useState({ min: '', max: '' });
      const [results, setResults] = useState([]);
      const [savedResults, setSavedResults] = useState([]);
      const [compareItems, setCompareItems] = useState([]);
      const [searchQuery, setSearchQuery] = useState('');
      const [searchHistory, setSearchHistory] = useState([]);
      const [activeTab, setActiveTab] = useState('results');
      const [error, setError] = useState('');
      const [sortOrder, setSortOrder] = useState('none');
      const [currentPage, setCurrentPage] = useState(1);
      const [isLoading, setIsLoading] = useState(false);
      const [modalImage, setModalImage] = useState(null);

      const itemsPerPage = 6;

      const fittingsDatabase = [
        { id: 1, name: "Петля угловая стандартная", type: "Петля", subtype: "Угловая", mechanism: "Без доводчика", specificOption: "Стандартная", brand: "GTV", price: 500, description: "Надёжная угловая петля для шкафов.", image: "https://avatars.mds.yandex.net/i?id=6c6f936f9f225749da710eacb5cccc32426fde80-8132087-images-thumbs&n=13" },
        { id: 2, name: "Петля накладная с доводчиком", type: "Петля", subtype: "Накладная", mechanism: "С доводчиком", specificOption: "С доводчиком", brand: "BOYARD", price: 800, description: "Накладная петля с плавным закрыванием.", image: "https://www.boyard.biz/thumbs/original/products/h301a020930/36e376c9887d25fa4c777f18305719e2.jpg" },
        { id: 3, name: "Петля врезная push-to-open", type: "Петля", subtype: "Врезная", mechanism: "Push-to-open", specificOption: "Push-to-open", brand: "DECOLINE", price: 1200, description: "Врезная петля с push-механизмом.", image: "https://gratis72.ru/upload/iblock/493/tehvfjk7lpyrz06nq7qb77sv6lj4drk1/08558114_fe74_11ec_b964_00155d936a00_5a75ba8f_fe7c_11ec_b964_00155d936a00.png" },
        { id: 4, name: "Направляющая шариковая стандартная", type: "Направляющая", subtype: "Шариковая", mechanism: "Без доводчика", specificOption: "Стандартная", brand: "MF", price: 1500, description: "Шариковая направляющая для ящиков.", image: "https://avatars.mds.yandex.net/i?id=f5e609961ac0460c78fd86fec8d1fcbfe10626fd-5217037-images-thumbs&n=13" },
        { id: 5, name: "Направляющая роликовая с доводчиком", type: "Направляющая", subtype: "Роликовая", mechanism: "С доводчиком", specificOption: "С доводчиком", brand: "GTV", price: 1800, description: "Роликовая направляющая с плавным ходом.", image: "https://cdn1.ozone.ru/s3/multimedia-b/6042782627.jpg" },
        { id: 6, name: "Направляющая скрытого монтажа push-to-open", type: "Направляющая", subtype: "Скрытого монтажа", mechanism: "Push-to-open", specificOption: "Push-to-open", brand: "BOYARD", price: 2000, description: "Скрытая направляющая для ящиков.", image: "https://fantorg-irk.ru/upload/iblock/025/0252e19ba37270bfe55e603074ce18b0.jpg" },
        { id: 7, name: "Ручка круглая классическая", type: "Ручка", subtype: "Круглая", mechanism: "Без доводчика", specificOption: "Классическая", brand: "DECOLINE", price: 300, description: "Классическая круглая ручка.", image: "https://avatars.mds.yandex.net/i?id=0162c28c527b7eb6053a30b1b042ed7b_l-5233220-images-thumbs&n=13" },
        { id: 8, name: "Ручка длинная современная", type: "Ручка", subtype: "Длинная", mechanism: "Без доводчика", specificOption: "Рейлинг", brand: "MF", price: 400, description: "Современная длинная ручка-рейлинг.", image: "https://avatars.mds.yandex.net/i?id=2b0da45dcaa2a51019f95bb92fa43d4d_l-5859311-images-thumbs&n=13" },
        { id: 9, name: "Ручка квадратная push-to-open", type: "Ручка", subtype: "Квадратная", mechanism: "Push-to-open", specificOption: "Скоба", brand: "GTV", price: 600, description: "Квадратная ручка-скоба с push-механизмом.", image: "https://lavrmf.ru/upload/iblock/dc4/xu9l839o92jykgv8odqy33wq4ugeucfn/KHettikh-mekhanizm-Push-to-open-magnet-d-petel-pod-prikruchivanie-dlinnyy-khod-belyy-25sht-9089591-3.jpg" },
        { id: 10, name: "Крепление угловое базовое", type: "Крепление", subtype: "Угловое", mechanism: "Без доводчика", specificOption: "Базовое", brand: "BOYARD", price: 400, description: "Базовое угловое крепление.", image: "https://avatars.mds.yandex.net/i?id=e7033f6d7e9ef1a87d8b637230e2c403_l-3986726-images-thumbs&n=13" },
      ];

      const specificOptions = {
        "Петля": ["Стандартная", "С доводчиком", "Push-to-open", "Усиленная", "Скрытая", "Декоративная", "Компактная", "Двойная", "Мини", "Премиум"],
        "Направляющая": ["Стандартная", "С доводчиком", "Push-to-open", "Лёгкая", "Усиленная", "Декоративная", "Мягкая", "Двойная", "Компактная"],
        "Ручка": ["Классическая", "Рейлинг", "Скоба", "Винтажная", "Современная", "Дизайнерская", "Минималистичная", "Эргономичная"],
        "Крепление": ["Базовое", "Регулируемое", "Улучшенное", "Компактное", "Стандартное", "Усиленное", "Универсальное", "Потолочное"]
      };

      const findBestOptions = async () => {
        setIsLoading(true);
        if (!fittingsDatabase || fittingsDatabase.length === 0) {
          setError(i18next.t('no_match'));
          setResults([]);
          setIsLoading(false);
          return;
        }

        const filteredFittings = fittingsDatabase.filter(fitting => {
          const typeMatch = !type || fitting.type === type;
          const specificOptionMatch = !specificOption || fitting.specificOption === specificOption;
          const brandMatch = !brand || fitting.brand === brand;
          const priceMinMatch = !priceRange.min || fitting.price >= Number(priceRange.min);
          const priceMaxMatch = !priceRange.max || fitting.price <= Number(priceRange.max);
          const searchMatch = !searchQuery || fitting.name.toLowerCase().includes(searchQuery.toLowerCase()) || fitting.description.toLowerCase().includes(searchQuery.toLowerCase());

          return typeMatch && specificOptionMatch && brandMatch && priceMinMatch && priceMaxMatch && searchMatch;
        });

        let finalFittings = filteredFittings;
        if (filteredFittings.length === 0) {
          setError(i18next.t('no_match'));
          finalFittings = type ? fittingsDatabase.filter(f => f.type === type) : fittingsDatabase.slice(0, 1);
        } else {
          setError('');
        }

        setResults(finalFittings);
        if (searchQuery) {
          setSearchHistory(prev => [...new Set([searchQuery, ...prev])].slice(0, 10));
        }
        setIsLoading(false);
      };

      const saveToPDF = (resultsToExport) => {
        try {
          const docDefinition = {
            content: [
              { text: i18next.t('export_all_pdf'), style: 'header' },
              ...resultsToExport.map(fitting => ({
                text: [
                  `${i18next.t('name_label')}: ${fitting.name}\n`,
                  `${i18next.t('type')}: ${i18next.t(fitting.type)}\n`,
                  `${i18next.t('subtype_label')}: ${i18next.t(fitting.subtype)}\n`,
                  `${i18next.t('option')}: ${i18next.t(fitting.specificOption)}\n`,
                  `${i18next.t('brand')}: ${i18next.t(fitting.brand)}\n`,
                  `${i18next.t('price')}: ${fitting.price} руб.\n`,
                  `${i18next.t('description')}: ${fitting.description}\n\n`
                ]
              }))
            ],
            styles: {
              header: { fontSize: 18, bold: true, margin: [0, 0, 0, 10], color: '#000000' }
            }
          };
          pdfMake.createPdf(docDefinition).download('fittings_results.pdf');
        } catch (err) {
          setError('Ошибка при экспорте в PDF');
        }
      };

      const saveResult = (fitting) => {
        if (!savedResults.some(item => item.id === fitting.id)) {
          setSavedResults([...savedResults, fitting]);
        }
      };

      const addToCompare = (fitting) => {
        if (compareItems.length < 3 && !compareItems.some(item => item.id === fitting.id)) {
          setCompareItems([...compareItems, fitting]);
        }
      };

      const removeFromCompare = (fittingId) => {
        setCompareItems(compareItems.filter(item => item.id !== fittingId));
      };

      const sortResults = (order) => {
        setSortOrder(order);
        let sorted = [...results];
        if (order === 'asc') {
          sorted.sort((a, b) => a.price - b.price);
        } else if (order === 'desc') {
          sorted.sort((a, b) => b.price - a.price);
        }
        setResults(sorted);
      };

      const clearFilters = () => {
        setType('');
        setSpecificOption('');
        setBrand('');
        setPriceRange({ min: '', max: '' });
        setSearchQuery('');
        setCurrentPage(1);
      };

      const handlePriceRangeChange = (e) => {
        const { name, value } = e.target;
        const newValue = value === '' ? '' : Math.max(0, Number(value));
        setPriceRange(prev => ({ ...prev, [name]: newValue }));
      };

      useEffect(() => {
        findBestOptions();
      }, [language, type, specificOption, brand, priceRange, searchQuery]);

      const paginatedResults = results.slice((currentPage - 1) * itemsPerPage, currentPage * itemsPerPage);
      const totalPages = Math.ceil(results.length / itemsPerPage);

      return (
        React.createElement('div', { className: 'container-overlay configurator-container' },
          modalImage && React.createElement(Modal, {
            imageSrc: modalImage,
            onClose: () => setModalImage(null)
          }),
          React.createElement('h1', {
            className: 'text-5xl font-bold text-center mb-12 mt-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
            'data-i18n': 'configurator_title'
          }, i18next.t('configurator_title')),
          React.createElement('div', { className: 'flex flex-col md:flex-row justify-center items-center gap-6 mb-10' },
            React.createElement('div', { className: 'w-full md:w-1/4 tooltip' },
              React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'type_label' }, i18next.t('type_label')),
              React.createElement('span', { className: 'tooltip-text', 'data-i18n': 'type_tooltip' }, i18next.t('type_tooltip')),
              React.createElement('select', {
                value: type,
                onChange: (e) => { setType(e.target.value); setSpecificOption(''); },
                className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300 light-theme:bg-white light-theme:text-black light-theme:border-gray-300'
              },
                React.createElement('option', { value: '', 'data-i18n': 'type_placeholder' }, i18next.t('type_placeholder')),
                [...new Set(fittingsDatabase.map(f => f.type))].map(t =>
                  React.createElement('option', { key: t, value: t }, i18next.t(t))
                )
              )
            ),
            React.createElement('div', { className: 'w-full md:w-1/4 tooltip' },
              React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'brand_label' }, i18next.t('brand_label')),
              React.createElement('span', { className: 'tooltip-text', 'data-i18n': 'brand_tooltip' }, i18next.t('brand_tooltip')),
              React.createElement('select', {
                value: brand,
                onChange: (e) => setBrand(e.target.value),
                className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300 light-theme:bg-white light-theme:text-black light-theme:border-gray-300'
              },
                React.createElement('option', { value: '', 'data-i18n': 'brand_placeholder' }, i18next.t('brand_placeholder')),
                ["GTV", "BOYARD", "DECOLINE", "MF"].map(b =>
                  React.createElement('option', { key: b, value: b }, i18next.t(b))
                )
              )
            )
          ),
          type && React.createElement('div', { className: 'mb-8 text-center tooltip' },
            React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'specific_option_label' }, i18next.t('specific_option_label')),
            React.createElement('span', { className: 'tooltip-text', 'data-i18n': 'specific_option_tooltip' }, i18next.t('specific_option_tooltip')),
            React.createElement('select', {
              value: specificOption,
              onChange: (e) => setSpecificOption(e.target.value),
              className: 'mt-2 block w-1/2 mx-auto rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300 light-theme:bg-white light-theme:text-black light-theme:border-gray-300'
            },
              React.createElement('option', { value: '', 'data-i18n': 'specific_option_placeholder' }, i18next.t('specific_option_placeholder')),
              specificOptions[type].map(option =>
                React.createElement('option', { key: option, value: option }, i18next.t(option))
              )
            )
          ),
          React.createElement('div', { className: 'price-inputs-container' },
            React.createElement('div', { className: 'flex flex-col md:flex-row gap-6 justify-center' },
              React.createElement('div', null,
                React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'price_range_from' }, i18next.t('price_range_from')),
                React.createElement('input', {
                  type: 'number',
                  name: 'min',
                  value: priceRange.min,
                  onChange: handlePriceRangeChange,
                  className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300 text-center light-theme:bg-white light-theme:text-black light-theme:border-gray-300'
                })
              ),
              React.createElement('div', null,
                React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'price_range_to' }, i18next.t('price_range_to')),
                React.createElement('input', {
                  type: 'number',
                  name: 'max',
                  value: priceRange.max,
                  onChange: handlePriceRangeChange,
                  className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300 text-center light-theme:bg-white light-theme:text-black light-theme:border-gray-300'
                })
              )
            )
          ),
          React.createElement('div', { className: 'flex flex-col md:flex-row gap-4 justify-center mb-6' },
            React.createElement('div', { className: 'w-full md:w-1/4' },
              React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'search_label' }, i18next.t('search_label')),
              React.createElement('input', {
                type: 'text',
                value: searchQuery,
                onChange: (e) => setSearchQuery(e.target.value),
                className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300 light-theme:bg-white light-theme:text-black light-theme:border-gray-300'
              })
            )
          ),
          React.createElement('div', { className: 'text-center mb-6' },
            React.createElement('button', {
              onClick: findBestOptions,
              className: 'gradient-button mr-4'
            },
              React.createElement('span', { 'data-i18n': 'find_options' }, i18next.t('find_options'))
            ),
            React.createElement('button', {
              onClick: clearFilters,
              className: 'gradient-button'
            },
              React.createElement('span', { 'data-i18n': 'clear_filters' }, i18next.t('clear_filters'))
            )
          ),
          React.createElement('div', { className: 'tab-buttons' },
            React.createElement('button', {
              onClick: () => setActiveTab('results'),
              className: `tab-button ${activeTab === 'results' ? 'active' : ''}`
            },
              React.createElement('span', { 'data-i18n': 'tab_results' }, i18next.t('tab_results'))
            ),
            React.createElement('button', {
              onClick: () => setActiveTab('saved'),
              className: `tab-button ${activeTab === 'saved' ? 'active' : ''}`
            },
              React.createElement('span', { 'data-i18n': 'tab_saved' }, i18next.t('tab_saved'))
            ),
            React.createElement('button', {
              onClick: () => setActiveTab('compare'),
              className: `tab-button ${activeTab === 'compare' ? 'active' : ''}`
            },
              React.createElement('span', { 'data-i18n': 'tab_compare' }, i18next.t('tab_compare'))
            ),
            React.createElement('button', {
              onClick: () => setActiveTab('history'),
              className: `tab-button ${activeTab === 'history' ? 'active' : ''}`
            },
              React.createElement('span', { 'data-i18n': 'tab_history' }, i18next.t('tab_history'))
            ),
            React.createElement('button', {
              onClick: () => setActiveTab('calculator'),
              className: `tab-button ${activeTab === 'calculator' ? 'active' : ''}`
            },
              React.createElement('span', { 'data-i18n': 'tab_calculator' }, i18next.t('tab_calculator'))
            )
          ),
          React.createElement('div', { className: 'tab-content' },
            activeTab === 'results' && (
              React.createElement('div', null,
                error && React.createElement('p', { className: 'text-red-400 text-center mb-4' }, error),
                React.createElement('h2', {
                  className: 'text-3xl font-bold text-center mb-6 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                  'data-i18n': 'results_title'
                }, i18next.t('results_title')),
                React.createElement('div', { className: 'sort-dropdown mb-4' },
                  React.createElement('button', {
                    onClick: () => sortResults('asc'),
                    className: 'gradient-button'
                  },
                    React.createElement('span', { 'data-i18n': 'sort_asc' }, i18next.t('sort_asc'))
                  ),
                  React.createElement('button', {
                    onClick: () => sortResults('desc'),
                    className: 'gradient-button ml-2'
                  },
                    React.createElement('span', { 'data-i18n': 'sort_desc' }, i18next.t('sort_desc'))
                  )
                ),
                isLoading ? React.createElement('div', { className: 'loader' }) : (
                  React.createElement('div', null,
                    React.createElement('div', { className: 'grid grid-cols-1 md:grid-cols-3 gap-6' },
                      paginatedResults.map(fitting => (
                        React.createElement('div', { key: fitting.id, className: 'p-4 bg-gray-800 rounded-lg shadow-lg light-theme:bg-gray-100 card-description' },
                          React.createElement('img', {
                            src: fitting.image,
                            alt: fitting.name,
                            className: 'fittings-image w-full mb-4 rounded',
                            onClick: () => setModalImage(fitting.image)
                          }),
                          React.createElement('h3', { className: 'text-xl font-semibold mb-2' }, fitting.name),
                          React.createElement('p', null, `${i18next.t('type')}: ${i18next.t(fitting.type)}`),
                          React.createElement('p', null, `${i18next.t('subtype_label')}: ${i18next.t(fitting.subtype)}`),
                          React.createElement('p', null, `${i18next.t('option')}: ${i18next.t(fitting.specificOption)}`),
                          React.createElement('p', null, `${i18nnext.t('brand')}: ${i18next.t(fitting.brand)}`),
                          React.createElement('p', null, `${i18next.t('description')}: ${fitting.description}`),
                          React.createElement('p', { className: 'price-text' }, `${i18next.t('price')}: ${fitting.price} руб.`),
                          React.createElement('p', null, i18next.t('price_disclaimer')),
                          React.createElement('button', {
                            onClick: () => saveResult(fitting),
                            className: 'gradient-button mt-2'
                          },
                            React.createElement('span', { 'data-i18n': 'save' }, i18next.t('save'))
                          ),
                          React.createElement('button', {
                            onClick: () => compareItems.some(item => item.id === fitting.id) ? removeFromCompare(fitting.id) : addToCompare(fitting),
                            className: 'gradient-button mt-2 ml-2'
                          },
                            React.createElement('span', { 'data-i18n': compareItems.some(item => item.id === fitting.id) ? 'remove_compare' : 'compare' }, i18next.t(compareItems.some(item => item.id === fitting.id) ? 'remove_compare' : 'compare'))
                          )
                        )
                      ))
                    ),
                    React.createElement('div', { className: 'flex justify-center mt-6 gap-4' },
                      React.createElement('button', {
                        onClick: () => setCurrentPage(prev => Math.max(prev - 1, 1)),
                        disabled: currentPage === 1,
                        className: 'gradient-button'
                      }, 'Предыдущая'),
                      React.createElement('span', { className: 'text-white' }, `Страница ${currentPage} из ${totalPages}`),
                      React.createElement('button', {
                        onClick: () => setCurrentPage(prev => Math.min(prev + 1, totalPages)),
                        disabled: currentPage === totalPages,
                        className: 'gradient-button'
                      }, 'Следующая')
                    )
                  )
                ),
                results.length > 0 && React.createElement('button', {
                  onClick: () => saveToPDF(results),
                  className: 'gradient-button mt-6'
                },
                  React.createElement('span', { 'data-i18n': 'export_selected_pdf' }, i18next.t('export_selected_pdf'))
                )
              )
            ),
            activeTab === 'saved' && (
              React.createElement('div', null,
                React.createElement('h2', {
                  className: 'text-3xl font-bold text-center mb-6 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                  'data-i18n': 'saved_results_title'
                }, i18next.t('saved_results_title')),
                React.createElement('div', { className: 'grid grid-cols-1 md:grid-cols-3 gap-6' },
                  savedResults.map(fitting => (
                    React.createElement('div', { key: fitting.id, className: 'p-4 bg-gray-800 rounded-lg shadow-lg light-theme:bg-gray-100 card-description' },
                      React.createElement('img', {
                        src: fitting.image,
                        alt: fitting.name,
                        className: 'fittings-image w-full mb-4 rounded',
                        onClick: () => setModalImage(fitting.image)
                      }),
                      React.createElement('h3', { className: 'text-xl font-semibold mb-2' }, fitting.name),
                      React.createElement('p', null, `${i18next.t('type')}: ${i18next.t(fitting.type)}`),
                      React.createElement('p', null, `${i18next.t('subtype_label')}: ${i18next.t(fitting.subtype)}`),
                      React.createElement('p', null, `${i18next.t('option')}: ${i18next.t(fitting.specificOption)}`),
                      React.createElement('p', null, `${i18next.t('brand')}: ${i18next.t(fitting.brand)}`),
                      React.createElement('p', null, `${i18next.t('description')}: ${fitting.description}`),
                      React.createElement('p', { className: 'price-text' }, `${i18next.t('price')}: ${fitting.price} руб.`),
                      React.createElement('p', null, i18next.t('price_disclaimer')),
                      React.createElement('button', {
                        onClick: () => saveResult(fitting),
                        className: 'gradient-button mt-2'
                      },
                        React.createElement('span', { 'data-i18n': 'save' }, i18next.t('save'))
                      )
                    )
                  ))
                ),
                savedResults.length > 0 && React.createElement('button', {
                  onClick: () => saveToPDF(savedResults),
                  className: 'gradient-button mt-6'
                },
                  React.createElement('span', { 'data-i18n': 'export_all_pdf' }, i18next.t('export_all_pdf'))
                )
              )
            ),
            activeTab === 'compare' && (
              React.createElement('div', null,
                React.createElement('h2', {
                  className: 'text-3xl font-bold text-center mb-6 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                  'data-i18n': 'compare_title'
                }, i18next.t('compare_title')),
                React.createElement('div', { className: 'grid grid-cols-1 md:grid-cols-3 gap-6' },
                  compareItems.map(fitting => (
                    React.createElement('div', { key: fitting.id, className: 'p-4 bg-gray-800 rounded-lg shadow-lg light-theme:bg-gray-100 card-description' },
                      React.createElement('img', {
                        src: fitting.image,
                        alt: fitting.name,
                        className: 'fittings-image w-full mb-4 rounded',
                        onClick: () => setModalImage(fitting.image)
                      }),
                      React.createElement('h3', { className: 'text-xl font-semibold mb-2' }, fitting.name),
                      React.createElement('p', null, `${i18next.t('type')}: ${i18next.t(fitting.type)}`),
                      React.createElement('p', null, `${i18next.t('subtype_label')}: ${i18next.t(fitting.subtype)}`),
                      React.createElement('p', null, `${i18next.t('option')}: ${i18next.t(fitting.specificOption)}`),
                      React.createElement('p', null, `${i18next.t('brand')}: ${i18next.t(fitting.brand)}`),
                      React.createElement('p', null, `${i18next.t('description')}: ${fitting.description}`),
                      React.createElement('p', { className: 'price-text' }, `${i18next.t('price')}: ${fitting.price} руб.`),
                      React.createElement('p', null, i18next.t('price_disclaimer')),
                      React.createElement('button', {
                        onClick: () => removeFromCompare(fitting.id),
                        className: 'gradient-button mt-2'
                      },
                        React.createElement('span', { 'data-i18n': 'remove_compare' }, i18next.t('remove_compare'))
                      )
                    )
                  ))
                )
              )
            ),
            activeTab === 'history' && (
              React.createElement('div', null,
                React.createElement('h2', {
                  className: 'text-3xl font-bold text-center mb-6 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                  'data-i18n': 'history_title'
                }, i18next.t('history_title')),
                React.createElement('ul', { className: 'list-disc pl-5' },
                  searchHistory.map((query, index) => (
                    React.createElement('li', { key: index, className: 'text-white mb-2' }, query)
                  ))
                )
              )
            ),
            activeTab === 'calculator' && (
              React.createElement('div', { className: 'calculator-container' },
                React.createElement('h2', {
                  className: 'text-3xl font-bold text-center mb-6 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                  'data-i18n': 'calculator_title'
                }, i18next.t('calculator_title')),
                React.createElement('div', { className: 'flex flex-col md:flex-row gap-4' },
                  React.createElement('div', { className: 'flex-1' },
                    React.createElement('div', { className: 'mb-4' },
                      React.createElement('label', { className: 'configurator-label block text-center', 'data-i18n': 'calculator_input_label' }, i18next.t('calculator_input_label')),
                      React.createElement('input', {
                        type: 'number',
                        id: 'calcInputPrice',
                        min: '0',
                        className: 'mt-2 block w-full p-3 border border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500 text-lg bg-gray-800 text-white light-theme:bg-white light-theme:text-black light-theme:border-gray-300',
                        placeholder: "Сумма в рублях",
                        step: "0.01"
                      })
                    ),
                    React.createElement('div', { id: 'calcResult', className: 'mt-4' })
                  ),
                  React.createElement('div', { className: 'flex flex-col gap-2' },
                    React.createElement('button', {
                      id: 'calcPercent5Btn',
                      className: 'gradient-button text-white p-3 rounded-md text-lg',
                      'data-percent': '5'
                    }, React.createElement('span', { 'data-i18n': 'calculator_percent_5' }, i18next.t('calculator_percent_5'))),
                    React.createElement('button', {
                      id: 'calcPercent10Btn',
                      className: 'gradient-button text-white p-3 rounded-md text-lg',
                      'data-percent': '10'
                    }, React.createElement('span', { 'data-i18n': 'calculator_percent_10' }, i18next.t('calculator_percent_10'))),
                    React.createElement('button', {
                      id: 'calcPercent15Btn',
                      className: 'gradient-button text-white p-3 rounded-md text-lg',
                      'data-percent': '15'
                    }, React.createElement('span', { 'data-i18n': 'calculator_percent_15' }, i18next.t('calculator_percent_15'))),
                    React.createElement('button', {
                      id: 'calcPercent20Btn',
                      className: 'gradient-button text-white p-3 rounded-md text-lg',
                      'data-percent': '20'
                    }, React.createElement('span', { 'data-i18n': 'calculator_percent_20' }, i18next.t('calculator_percent_20'))),
                    React.createElement('button', {
                      id: 'calcUndoBtn',
                      className: 'gradient-button text-white p-3 rounded-md text-lg'
                    }, React.createElement('span', { 'data-i18n': 'calculator_undo' }, i18next.t('calculator_undo'))),
                    React.createElement('button', {
                      id: 'calcSaveBtn',
                      className: 'gradient-button text-white p-3 rounded-md text-lg'
                    }, React.createElement('span', { 'data-i18n': 'calculator_save' }, i18next.t('calculator_save')))
                  )
                ),
                React.createElement('div', { className: 'mt-4' },
                  React.createElement('h2', {
                    className: 'text-2xl font-bold text-center mb-4 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                    'data-i18n': 'calculator_saved_title'
                  }, i18next.t('calculator_saved_title')),
                  React.createElement('div', { id: 'calcSavedResults', className: 'bg-gray-800 p-4 rounded-md shadow-sm light-theme:bg-gray-100' })
                ),
                React.createElement('div', { className: 'mt-6' },
                  React.createElement('h3', {
                    className: 'text-xl font-bold text-center mb-2 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
                    'data-i18n': 'database_title'
                  }, i18next.t('database_title')),
                  React.createElement('div', { className: 'database-window' },
                    fittingsDatabase.map(fitting => (
                      React.createElement('div', { key: fitting.id, className: 'database-item text-white light-theme:text-black' },
                        `${fitting.name}: ${fitting.price} руб.`
                      )
                    ))
                  )
                )
              )
            )
          )
        )
      );
    };

    const CalculatorPage = () => {
      const [basePrice, setBasePrice] = useState('');
      const [appliedPercentages, setAppliedPercentages] = useState([]);
      const [savedResults, setSavedResults] = useState([]);

      const updateResults = () => {
        const price = parseFloat(basePrice);
        const resultDiv = document.getElementById('calcResult');
        resultDiv.innerHTML = '';

        if (isNaN(price) || price <= 0) {
          resultDiv.innerHTML = '<p class="text-red-400 text-center text-lg">' + i18next.t('calculator_error') + '</p>';
          return;
        }

        if (appliedPercentages.length === 0) {
          resultDiv.innerHTML = '<p class="text-gray-400 text-center text-lg">' + i18next.t('calculator_select_percent') + '</p>';
          return;
        }

        let finalPrice = price;
        appliedPercentages.forEach(percentage => {
          finalPrice *= (1 + percentage / 100);
        });

        resultDiv.innerHTML = `
          <div class="bg-gray-600 p-4 rounded-md shadow-sm">
           <p class="text-lg font-medium text-gray-200">Итоговая цена с наценкой (${appliedPercentages.join(', ')}%):</p>
          <p class="text-2xl font-bold text-white">${finalPrice.toFixed(2)} руб.</p>
        </div>
      `;
    }

    function updateSavedResults() {
      if (savedResults.length === 0) {
        savedResultsDiv.innerHTML = '<p class="text-gray-400 text-center text-lg">Нет сохраненных сумм.</p>';
        return;
      }

      savedResultsDiv.innerHTML = savedResults.map((result, index) => `
        <div class="bg-gray-600 p-3 rounded-md shadow-sm mb-2">
          <p class="text-sm font-medium text-gray-200">Сохранение #${index + 1} (${result.percentages.join(', ')}%):</p>
          <p class="text-lg font-bold text-white">${result.price.toFixed(2)} руб.</p>
        </div>
      `).join('');
    }

    buttons.forEach(button => {
      button.addEventListener('click', () => {
        const percentage = parseFloat(button.getAttribute('data-percent'));
        appliedPercentages.push(percentage);
        updateResults();
      });
    });

    undoBtn.addEventListener('click', () => {
      appliedPercentages.pop();
      updateResults();
    });

    saveBtn.addEventListener('click', () => {
      const basePrice = parseFloat(inputPrice.value);
      if (isNaN(basePrice) || basePrice <= 0 || appliedPercentages.length === 0) {
        return;
      }

      let finalPrice = basePrice;
      appliedPercentages.forEach(percentage => {
        finalPrice *= (1 + percentage / 100);
      });

      savedResults.push({
        price: finalPrice,
        percentages: [...appliedPercentages]
      });
      updateSavedResults();
    });

    inputPrice.addEventListener('input', () => {
      updateResults();
    });

    inputPrice.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        updateResults();
      }
    });
  </script>
</body>
</html>
