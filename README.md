
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Конфигуратор кухонной мебели</title>
  <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/babel-standalone@6.26.0/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</head>
<body class="bg-black min-h-screen flex flex-col items-center justify-center p-4">
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useEffect } = React;

    const App = () => {
      const [roomParams, setRoomParams] = useState({ length: '', width: '', height: '' });
      const [budget, setBudget] = useState('');
      const [style, setStyle] = useState('');
      const [material, setMaterial] = useState('');
      const [color, setColor] = useState('');
      const [hasIsland, setHasIsland] = useState('');
      const [results, setResults] = useState([]);
      const [savedResults, setSavedResults] = useState([]);
      const [error, setError] = useState('');

      const kitchenDatabase = [
        { id: 1, name: "Классика Белая", length: 3.0, width: 2.0, height: 2.2, price: 150000, style: "Классический", material: "Дерево", color: "Белый", hasIsland: false, description: "Деревянная отделка, подходит для средних кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 2, name: "Модерн Серый", length: 2.5, width: 1.8, height: 2.0, price: 120000, style: "Модерн", material: "МДФ", color: "Серый", hasIsland: false, description: "Минимализм, идеален для небольших помещений.", image: "https://images.unsplash.com/photo-1588850561407-ed78c282e89b?q=80&w=800&auto=format&fit=crop" },
        { id: 3, name: "Лофт Металл", length: 3.5, width: 2.5, height: 2.4, price: 200000, style: "Лофт", material: "Металл", color: "Чёрный", hasIsland: true, description: "Металлические элементы, для просторных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 4, name: "Сканди Светлый", length: 2.8, width: 2.0, height: 2.1, price: 130000, style: "Скандинавский", material: "Дерево", color: "Белый", hasIsland: false, description: "Светлые тона, уютный дизайн.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 5, name: "Премиум Мрамор", length: 4.0, width: 3.0, height: 2.5, price: 300000, style: "Премиум", material: "Мрамор", color: "Белый", hasIsland: true, description: "Мраморная отделка, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 6, name: "Рустик Дуб", length: 3.2, width: 2.2, height: 2.3, price: 180000, style: "Рустик", material: "Дуб", color: "Коричневый", hasIsland: false, description: "Дубовая текстура, для теплых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 7, name: "Хай-тек Черный", length: 2.7, width: 1.9, height: 2.1, price: 140000, style: "Хай-тек", material: "МДФ", color: "Чёрный", hasIsland: false, description: "Глянцевый черный, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 8, name: "Прованс Пастель", length: 3.1, width: 2.1, height: 2.2, price: 160000, style: "Прованс", material: "Дерево", color: "Пастель", hasIsland: false, description: "Пастельные тона, для романтичных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 9, name: "Минимал Синий", length: 2.6, width: 1.7, height: 2.0, price: 110000, style: "Минимализм", material: "МДФ", color: "Синий", hasIsland: false, description: "Синий акцент, компактный дизайн.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 10, name: "Эко Дерево", length: 3.4, width: 2.3, height: 2.4, price: 220000, style: "Эко", material: "Дерево", color: "Коричневый", hasIsland: true, description: "Естественные материалы, экологичный выбор.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 11, name: "Классика Кремовая", length: 3.3, width: 2.1, height: 2.3, price: 170000, style: "Классический", material: "Дерево", color: "Кремовый", hasIsland: false, description: "Кремовые оттенки, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 12, name: "Модерн Белый", length: 2.9, width: 2.0, height: 2.2, price: 125000, style: "Модерн", material: "МДФ", color: "Белый", hasIsland: false, description: "Белый минимализм, для светлых кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 13, name: "Лофт Бетон", length: 3.6, width: 2.6, height: 2.5, price: 210000, style: "Лофт", material: "Бетон", color: "Серый", hasIsland: true, description: "Бетонная отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 14, name: "Сканди Серый", length: 2.7, width: 1.9, height: 2.1, price: 135000, style: "Скандинавский", material: "Дерево", color: "Серый", hasIsland: false, description: "Серые акценты, для скандинавских интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 15, name: "Премиум Черный", length: 4.2, width: 3.2, height: 2.6, price: 320000, style: "Премиум", material: "Мрамор", color: "Чёрный", hasIsland: true, description: "Чёрный мрамор, для роскошных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 16, name: "Рустик Орех", length: 3.1, width: 2.0, height: 2.2, price: 175000, style: "Рустик", material: "Орех", color: "Коричневый", hasIsland: false, description: "Ореховая отделка, для деревенского стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 17, name: "Хай-тек Белый", length: 2.8, width: 2.0, height: 2.1, price: 145000, style: "Хай-тек", material: "МДФ", color: "Белый", hasIsland: true, description: "Глянцевый белый, для современных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 18, name: "Прованс Белый", length: 3.0, width: 2.2, height: 2.3, price: 165000, style: "Прованс", material: "Дерево", color: "Белый", hasIsland: false, description: "Белый прованс, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 19, name: "Минимал Белый", length: 2.5, width: 1.8, height: 2.0, price: 115000, style: "Минимализм", material: "МДФ", color: "Белый", hasIsland: false, description: "Белый минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 20, name: "Эко Белый", length: 3.3, width: 2.2, height: 2.4, price: 230000, style: "Эко", material: "Дерево", color: "Белый", hasIsland: true, description: "Экологичный белый дизайн, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 21, name: "Классика Золотая", length: 3.5, width: 2.4, height: 2.3, price: 190000, style: "Классический", material: "Дерево", color: "Золотой", hasIsland: false, description: "Золотая отделка, для роскошных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 22, name: "Модерн Чёрный", length: 2.6, width: 1.9, height: 2.1, price: 130000, style: "Модерн", material: "МДФ", color: "Чёрный", hasIsland: false, description: "Чёрный модерн, для стильных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 23, name: "Лофт Кирпич", length: 3.7, width: 2.7, height: 2.5, price: 220000, style: "Лофт", material: "Кирпич", color: "Красный", hasIsland: true, description: "Кирпичная отделка, для лофт-стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 24, name: "Сканди Зелёный", length: 2.9, width: 2.1, height: 2.2, price: 140000, style: "Скандинавский", material: "Дерево", color: "Зелёный", hasIsland: false, description: "Зелёные акценты, для свежих интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 25, name: "Премиум Золотой", length: 4.5, width: 3.5, height: 2.7, price: 350000, style: "Премиум", material: "Мрамор", color: "Золотой", hasIsland: true, description: "Золотой мрамор, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 26, name: "Рустик Белый", length: 3.0, width: 2.1, height: 2.2, price: 170000, style: "Рустик", material: "Дерево", color: "Белый", hasIsland: false, description: "Белый рустик, для деревенского стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 27, name: "Хай-тек Серый", length: 2.9, width: 2.0, height: 2.1, price: 150000, style: "Хай-тек", material: "МДФ", color: "Серый", hasIsland: true, description: "Глянцевый серый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 28, name: "Прованс Лаванда", length: 3.2, width: 2.3, height: 2.3, price: 175000, style: "Прованс", material: "Дерево", color: "Лавандовый", hasIsland: false, description: "Лавандовые тона, для романтичных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 29, name: "Минимал Чёрный", length: 2.4, width: 1.7, height: 2.0, price: 105000, style: "Минимализм", material: "МДФ", color: "Чёрный", hasIsland: false, description: "Чёрный минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 30, name: "Эко Серый", length: 3.5, width: 2.4, height: 2.5, price: 240000, style: "Эко", material: "Дерево", color: "Серый", hasIsland: true, description: "Экологичный серый дизайн, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 31, name: "Классика Темный Дуб", length: 3.6, width: 2.3, height: 2.3, price: 185000, style: "Классический", material: "Дуб", color: "Тёмно-коричневый", hasIsland: false, description: "Тёмный дуб, для классических интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 32, name: "Модерн Зелёный", length: 2.7, width: 1.9, height: 2.1, price: 135000, style: "Модерн", material: "МДФ", color: "Зелёный", hasIsland: false, description: "Зелёный модерн, для свежих кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 33, name: "Лофт Сталь", length: 3.8, width: 2.6, height: 2.5, price: 225000, style: "Лофт", material: "Сталь", color: "Серебристый", hasIsland: true, description: "Стальная отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 34, name: "Сканди Голубой", length: 2.9, width: 2.0, height: 2.2, price: 145000, style: "Скандинавский", material: "Дерево", color: "Голубой", hasIsland: false, description: "Голубые акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 35, name: "Премиум Серебро", length: 4.3, width: 3.3, height: 2.6, price: 330000, style: "Премиум", material: "Металл", color: "Серебристый", hasIsland: true, description: "Серебряная отделка, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 36, name: "Рустик Кедр", length: 3.2, width: 2.1, height: 2.3, price: 190000, style: "Рустик", material: "Кедр", color: "Коричневый", hasIsland: false, description: "Кедровая текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 37, name: "Хай-тек Золотой", length: 2.8, width: 2.0, height: 2.1, price: 160000, style: "Хай-тек", material: "МДФ", color: "Золотой", hasIsland: true, description: "Золотой глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 38, name: "Прованс Розовый", length: 3.1, width: 2.2, height: 2.3, price: 180000, style: "Прованс", material: "Дерево", color: "Розовый", hasIsland: false, description: "Розовые тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 39, name: "Минимал Золотой", length: 2.5, width: 1.8, height: 2.0, price: 120000, style: "Минимализм", material: "МДФ", color: "Золотой", hasIsland: false, description: "Золотой минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 40, name: "Эко Коричневый", length: 3.4, width: 2.3, height: 2.4, price: 250000, style: "Эко", material: "Дерево", color: "Коричневый", hasIsland: true, description: "Экологичный коричневый, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 41, name: "Классика Чёрный", length: 3.7, width: 2.4, height: 2.3, price: 195000, style: "Классический", material: "Дерево", color: "Чёрный", hasIsland: false, description: "Чёрная классика, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 42, name: "Модерн Красный", length: 2.6, width: 1.9, height: 2.1, price: 140000, style: "Модерн", material: "МДФ", color: "Красный", hasIsland: false, description: "Красный модерн, для ярких кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 43, name: "Лофт Дерево", length: 3.9, width: 2.8, height: 2.5, price: 230000, style: "Лофт", material: "Дерево", color: "Коричневый", hasIsland: true, description: "Деревянный лофт, для просторных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 44, name: "Сканди Жёлтый", length: 2.8, width: 2.0, height: 2.2, price: 150000, style: "Скандинавский", material: "Дерево", color: "Жёлтый", hasIsland: false, description: "Жёлтые акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 45, name: "Премиум Бронза", length: 4.4, width: 3.4, height: 2.6, price: 340000, style: "Премиум", material: "Металл", color: "Бронзовый", hasIsland: true, description: "Бронзовая отделка, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 46, name: "Рустик Сосна", length: 3.3, width: 2.2, height: 2.3, price: 200000, style: "Рустик", material: "Сосна", color: "Коричневый", hasIsland: false, description: "Сосновая текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 47, name: "Хай-тек Синий", length: 2.9, width: 2.1, height: 2.1, price: 155000, style: "Хай-тек", material: "МДФ", color: "Синий", hasIsland: true, description: "Синий глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 48, name: "Прованс Голубой", length: 3.2, width: 2.3, height: 2.3, price: 185000, style: "Прованс", material: "Дерево", color: "Голубой", hasIsland: false, description: "Голубые тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 49, name: "Минимал Красный", length: 2.5, width: 1.8, height: 2.0, price: 125000, style: "Минимализм", material: "МДФ", color: "Красный", hasIsland: false, description: "Красный минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 50, name: "Эко Зелёный", length: 3.6, width: 2.4, height: 2.5, price: 260000, style: "Эко", material: "Дерево", color: "Зелёный", hasIsland: true, description: "Экологичный зелёный, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 51, name: "Классика Серебро", length: 3.8, width: 2.5, height: 2.4, price: 210000, style: "Классический", material: "Металл", color: "Серебристый", hasIsland: false, description: "Серебряная классика, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 52, name: "Модерн Жёлтый", length: 2.7, width: 1.9, height: 2.1, price: 145000, style: "Модерн", material: "МДФ", color: "Жёлтый", hasIsland: false, description: "Жёлтый модерн, для ярких кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 53, name: "Лофт Камень", length: 4.0, width: 2.9, height: 2.6, price: 250000, style: "Лофт", material: "Камень", color: "Серый", hasIsland: true, description: "Каменная отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 54, name: "Сканди Оранжевый", length: 2.8, width: 2.0, height: 2.2, price: 155000, style: "Скандинавский", material: "Дерево", color: "Оранжевый", hasIsland: false, description: "Оранжевые акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 55, name: "Премиум Алый", length: 4.6, width: 3.6, height: 2.7, price: 360000, style: "Премиум", material: "Мрамор", color: "Алый", hasIsland: true, description: "Алый мрамор, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 56, name: "Рустик Бук", length: 3.4, width: 2.2, height: 2.3, price: 205000, style: "Рустик", material: "Бук", color: "Коричневый", hasIsland: false, description: "Буковая текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 57, name: "Хай-тек Оранжевый", length: 3.0, width: 2.1, height: 2.1, price: 165000, style: "Хай-тек", material: "МДФ", color: "Оранжевый", hasIsland: true, description: "Оранжевый глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 58, name: "Прованс Фиолетовый", length: 3.3, width: 2.4, height: 2.3, price: 190000, style: "Прованс", material: "Дерево", color: "Фиолетовый", hasIsland: false, description: "Фиолетовые тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 59, name: "Минимал Фиолетовый", length: 2.6, width: 1.8, height: 2.0, price: 130000, style: "Минимализм", material: "МДФ", color: "Фиолетовый", hasIsland: false, description: "Фиолетовый минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 60, name: "Эко Оливковый", length: 3.7, width: 2.5, height: 2.5, price: 270000, style: "Эко", material: "Дерево", color: "Оливковый", hasIsland: true, description: "Экологичный оливковый, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 61, name: "Классика Бронза", length: 3.9, width: 2.6, height: 2.4, price: 215000, style: "Классический", material: "Металл", color: "Бронзовый", hasIsland: false, description: "Бронзовая классика, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 62, name: "Модерн Фиолетовый", length: 2.8, width: 2.0, height: 2.1, price: 150000, style: "Модерн", material: "МДФ", color: "Фиолетовый", hasIsland: false, description: "Фиолетовый модерн, для стильных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 63, name: "Лофт Стекло", length: 4.1, width: 3.0, height: 2.6, price: 260000, style: "Лофт", material: "Стекло", color: "Прозрачный", hasIsland: true, description: "Стеклянная отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 64, name: "Сканди Красный", length: 2.9, width: 2.1, height: 2.2, price: 160000, style: "Скандинавский", material: "Дерево", color: "Красный", hasIsland: false, description: "Красные акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 65, name: "Премиум Изумруд", length: 4.7, width: 3.7, height: 2.7, price: 370000, style: "Премиум", material: "Мрамор", color: "Изумрудный", hasIsland: true, description: "Изумрудный мрамор, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 66, name: "Рустик Ясень", length: 3.5, width: 2.3, height: 2.3, price: 210000, style: "Рустик", material: "Ясень", color: "Коричневый", hasIsland: false, description: "Ясеневая текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 67, name: "Хай-тек Розовый", length: 3.1, width: 2.2, height: 2.1, price: 170000, style: "Хай-тек", material: "МДФ", color: "Розовый", hasIsland: true, description: "Розовый глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 68, name: "Прованс Оранжевый", length: 3.4, width: 2.5, height: 2.3, price: 195000, style: "Прованс", material: "Дерево", color: "Оранжевый", hasIsland: false, description: "Оранжевые тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 69, name: "Минимал Зелёный", length: 2.7, width: 1.9, height: 2.0, price: 135000, style: "Минимализм", material: "МДФ", color: "Зелёный", hasIsland: false, description: "Зелёный минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 70, name: "Эко Синий", length: 3.8, width: 2.6, height: 2.5, price: 280000, style: "Эко", material: "Дерево", color: "Синий", hasIsland: true, description: "Экологичный синий, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 71, name: "Классика Бордо", length: 3.6, width: 2.4, height: 2.3, price: 220000, style: "Классический", material: "Дерево", color: "Бордовый", hasIsland: false, description: "Бордовый классический стиль, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 72, name: "Модерн Бирюзовый", length: 2.8, width: 2.0, height: 2.1, price: 155000, style: "Модерн", material: "МДФ", color: "Бирюзовый", hasIsland: false, description: "Бирюзовый модерн, для стильных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 73, name: "Лофт Гранит", length: 4.2, width: 3.1, height: 2.6, price: 270000, style: "Лофт", material: "Гранит", color: "Серый", hasIsland: true, description: "Гранитная отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 74, name: "Сканди Фиолетовый", length: 3.0, width: 2.2, height: 2.2, price: 165000, style: "Скандинавский", material: "Дерево", color: "Фиолетовый", hasIsland: false, description: "Фиолетовые акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 75, name: "Премиум Сапфир", length: 4.8, width: 3.8, height: 2.7, price: 380000, style: "Премиум", material: "Мрамор", color: "Сапфировый", hasIsland: true, description: "Сапфировый мрамор, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 76, name: "Рустик Клён", length: 3.6, width: 2.3, height: 2.3, price: 215000, style: "Рустик", material: "Клён", color: "Коричневый", hasIsland: false, description: "Клёновая текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 77, name: "Хай-тек Бирюзовый", length: 3.2, width: 2.2, height: 2.1, price: 175000, style: "Хай-тек", material: "МДФ", color: "Бирюзовый", hasIsland: true, description: "Бирюзовый глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 78, name: "Прованс Жёлтый", length: 3.5, width: 2.5, height: 2.3, price: 200000, style: "Прованс", material: "Дерево", color: "Жёлтый", hasIsland: false, description: "Жёлтые тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 79, name: "Минимал Бордо", length: 2.8, width: 1.9, height: 2.0, price: 140000, style: "Минимализм", material: "МДФ", color: "Бордовый", hasIsland: false, description: "Бордовый минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 80, name: "Эко Бордо", length: 3.9, width: 2.7, height: 2.5, price: 290000, style: "Эко", material: "Дерево", color: "Бордовый", hasIsland: true, description: "Экологичный бордовый, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 81, name: "Классика Оливковый", length: 4.0, width: 2.7, height: 2.4, price: 230000, style: "Классический", material: "Дерево", color: "Оливковый", hasIsland: false, description: "Оливковый классический стиль, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 82, name: "Модерн Сапфировый", length: 2.9, width: 2.1, height: 2.1, price: 160000, style: "Модерн", material: "МДФ", color: "Сапфировый", hasIsland: false, description: "Сапфировый модерн, для стильных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 83, name: "Лофт Медь", length: 4.3, width: 3.2, height: 2.6, price: 280000, style: "Лофт", material: "Медь", color: "Медный", hasIsland: true, description: "Медная отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 84, name: "Сканди Бордо", length: 3.1, width: 2.2, height: 2.2, price: 170000, style: "Скандинавский", material: "Дерево", color: "Бордовый", hasIsland: false, description: "Бордовые акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 85, name: "Премиум Оникс", length: 4.9, width: 3.9, height: 2.7, price: 390000, style: "Премиум", material: "Мрамор", color: "Чёрный", hasIsland: true, description: "Ониксовая отделка, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 86, name: "Рустик Вишня", length: 3.7, width: 2.4, height: 2.3, price: 220000, style: "Рустик", material: "Вишня", color: "Коричневый", hasIsland: false, description: "Вишнёвая текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 87, name: "Хай-тек Сапфировый", length: 3.3, width: 2.3, height: 2.1, price: 180000, style: "Хай-тек", material: "МДФ", color: "Сапфировый", hasIsland: true, description: "Сапфировый глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 88, name: "Прованс Бирюзовый", length: 3.6, width: 2.6, height: 2.3, price: 205000, style: "Прованс", material: "Дерево", color: "Бирюзовый", hasIsland: false, description: "Бирюзовые тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 89, name: "Минимал Оливковый", length: 2.9, width: 2.0, height: 2.0, price: 145000, style: "Минимализм", material: "MDF", color: "Оливковый", hasIsland: false, description: "Оливковый минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 90, name: "Эко Жёлтый", length: 4.0, width: 2.8, height: 2.5, price: 300000, style: "Эко", material: "Дерево", color: "Жёлтый", hasIsland: true, description: "Экологичный жёлтый, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 91, name: "Классика Индиго", length: 4.1, width: 2.8, height: 2.4, price: 240000, style: "Классический", material: "Дерево", color: "Индиго", hasIsland: false, description: "Индиго классический стиль, для элегантных интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 92, name: "Модерн Лайм", length: 3.0, width: 2.2, height: 2.1, price: 165000, style: "Модерн", material: "MDF", color: "Лаймовый", hasIsland: false, description: "Лаймовый модерн, для ярких кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 93, name: "Лофт Оникс", length: 4.4, width: 3.3, height: 2.6, price: 290000, style: "Лофт", material: "Оникс", color: "Чёрный", hasIsland: true, description: "Ониксовая отделка, для индустриального стиля.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 94, name: "Сканди Индиго", length: 3.2, width: 2.3, height: 2.2, price: 175000, style: "Скандинавский", material: "Дерево", color: "Индиго", hasIsland: false, description: "Индиго акценты, для светлых интерьеров.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 95, name: "Премиум Золотистый", length: 5.0, width: 4.0, height: 2.8, price: 400000, style: "Премиум", material: "Мрамор", color: "Золотистый", hasIsland: true, description: "Золотистый мрамор, для элитных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 96, name: "Рустик Лиственница", length: 3.8, width: 2.5, height: 2.3, price: 225000, style: "Рустик", material: "Лиственница", color: "Коричневый", hasIsland: false, description: "Лиственничная текстура, для деревенских кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 97, name: "Хай-тек Лайм", length: 3.4, width: 2.4, height: 2.1, price: 185000, style: "Хай-тек", material: "MDF", color: "Лаймовый", hasIsland: true, description: "Лаймовый глянцевый, для современных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 98, name: "Прованс Индиго", length: 3.7, width: 2.7, height: 2.3, price: 210000, style: "Прованс", material: "Дерево", color: "Индиго", hasIsland: false, description: "Индиго тона, для романтичных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 99, name: "Минимал Сапфировый", length: 3.0, width: 2.1, height: 2.0, price: 150000, style: "Минимализм", material: "MDF", color: "Сапфировый", hasIsland: false, description: "Сапфировый минимализм, для компактных кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
        { id: 100, name: "Эко Лайм", length: 4.1, width: 2.9, height: 2.5, price: 310000, style: "Эко", material: "Дерево", color: "Лаймовый", hasIsland: true, description: "Экологичный лаймовый, для больших кухонь.", image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=800&auto=format&fit=crop" },
      ];

      const styles = ['Классический', 'Модерн', 'Лофт', 'Скандинавский', 'Премиум', 'Рустик', 'Хай-тек', 'Прованс', 'Минимализм', 'Эко'];
      const materials = ['Дерево', 'MDF', 'Мрамор', 'Металл', 'Бетон', 'Орех', 'Кирпич', 'Сталь', 'Камень', 'Стекло', 'Дуб', 'Кедр', 'Сосна', 'Бук', 'Ясень', 'Гранит', 'Медь', 'Оникс', 'Вишня', 'Клён', 'Лиственница'];
      const colors = ['Белый', 'Серый', 'Чёрный', 'Коричневый', 'Пастель', 'Синий', 'Золотой', 'Кремовый', 'Красный', 'Зелёный', 'Лавандовый', 'Голубой', 'Розовый', 'Жёлтый', 'Оранжевый', 'Фиолетовый', 'Серебристый', 'Бронзовый', 'Алый', 'Изумрудный', 'Тёмно-коричневый', 'Прозрачный', 'Бордовый', 'Бирюзовый', 'Сапфировый', 'Оливковый', 'Индиго', 'Лаймовый', 'Медный', 'Золотистый'];

      useEffect(() => {
        const saved = localStorage.getItem('savedKitchenResults');
        if (saved) setSavedResults(JSON.parse(saved));
      }, []);

      const handleInputChange = (e, field) => {
        const value = e.target.value;
        if (field === 'budget') setBudget(value);
        else if (field === 'style') setStyle(value);
        else if (field === 'material') setMaterial(value);
        else if (field === 'color') setColor(value);
        else if (field === 'hasIsland') setHasIsland(value === 'yes');
        else setRoomParams({ ...roomParams, [field]: value });
        setError('');
      };

      const findBestOptions = () => {
        const { length, width, height } = roomParams;
        const budgetValue = parseFloat(budget);

        if (!length || !width || !height || !budget) {
          setError('Пожалуйста, заполните все поля размеров и бюджета.');
          setResults([]);
          return;
        }

        if (isNaN(length) || isNaN(width) || isNaN(height) || isNaN(budgetValue) || length <= 0 || width <= 0 || height <= 0 || budgetValue <= 0) {
          setError('Введите корректные числовые значения больше 0.');
          setResults([]);
          return;
        }

        const tolerance = 0.1;
        let filteredOptions = kitchenDatabase.map(option => {
          const lengthFit = option.length <= parseFloat(length) * (1 + tolerance);
          const widthFit = option.width <= parseFloat(width) * (1 + tolerance);
          const heightFit = option.height <= parseFloat(height) * (1 + tolerance);
          const priceFit = option.price <= budgetValue * (1 + tolerance);

          let priority = 1.0;
          if (lengthFit) priority *= 1.0; else priority *= 0.8;
          if (widthFit) priority *= 1.0; else priority *= 0.8;
          if (heightFit) priority *= 1.0; else priority *= 0.8;
          if (priceFit) priority *= 1.0; else priority *= 0.9;

          return { ...option, priority };
        }).filter(option => {
          return option.length <= parseFloat(length) * (1 + tolerance) &&
                 option.width <= parseFloat(width) * (1 + tolerance) &&
                 option.height <= parseFloat(height) * (1 + tolerance) &&
                 option.price <= budgetValue * (1 + tolerance);
        });

        if (style) filteredOptions = filteredOptions.filter(option => option.style === style);
        if (material) filteredOptions = filteredOptions.filter(option => option.material === material);
        if (color) filteredOptions = filteredOptions.filter(option => option.color === color);
        if (hasIsland !== '') filteredOptions = filteredOptions.filter(option => option.hasIsland === hasIsland);

        const sortedOptions = filteredOptions.sort((a, b) => b.priority - a.priority || b.price - a.price).slice(0, 3);
        if (sortedOptions.length === 0) {
          setError('Не найдено подходящих вариантов для заданных параметров.');
          setResults([]);
        } else {
          setResults(sortedOptions);
          setError('');
        }
      };

      const saveResults = () => {
        if (results.length > 0) {
          const newSaved = [...savedResults, { id: Date.now(), data: results, params: { ...roomParams, budget, style, material, color, hasIsland }, timestamp: new Date().toLocaleString() }];
          setSavedResults(newSaved);
          localStorage.setItem('savedKitchenResults', JSON.stringify(newSaved));
          alert('Результаты сохранены!');
        }
      };

      const exportToPDF = () => {
        if (results.length > 0) {
          const { jsPDF } = window.jspdf;
          const doc = new jsPDF();
          doc.setFontSize(16);
          doc.text('Рекомендуемые варианты кухонной мебели', 10, 10);
          doc.setFontSize(12);
          doc.text(`Параметры комнаты: Длина ${roomParams.length}m, Ширина ${roomParams.width}m, Высота ${roomParams.height}m, Бюджет: ${budget} руб.`, 10, 20);
          if (style) doc.text(`Стиль: ${style}`, 10, 30);
          if (material) doc.text(`Материал: ${material}`, 10, 40);
          if (color) doc.text(`Цвет: ${color}`, 10, 50);
          doc.text(`Остров: ${hasIsland ? 'Да' : 'Нет'}`, 10, 60);

          let yOffset = 70;
          results.forEach((option, index) => {
            doc.text(`${index + 1}. ${option.name}`, 10, yOffset);
            doc.text(`Цена: ${option.price} руб.`, 10, yOffset + 10);
            doc.text(`Размеры: ${option.length} x ${option.width} x ${option.height} м`, 10, yOffset + 20);
            doc.text(`Стиль: ${option.style}, Материал: ${option.material}, Цвет: ${option.color}`, 10, yOffset + 30);
            doc.text(`Остров: ${option.hasIsland ? 'Да' : 'Нет'}`, 10, yOffset + 40);
            doc.text(`Описание: ${option.description}`, 10, yOffset + 50);
            yOffset += 60;
            if (yOffset > 260) {
              doc.addPage();
              yOffset = 10;
            }
          });

          doc.save(`kitchen_config_${new Date().toISOString().split('T')[0]}.pdf`);
          alert('PDF-файл сохранён!');
        }
      };

      return (
        <div className="max-w-5xl w-full bg-gray-900 shadow-lg rounded-xl p-6 border border-gray-700">
          <h1 className="text-3xl font-bold text-center text-white mb-6">Конфигуратор кухонной мебели</h1>
          
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
            <div>
              <label className="block text-gray-300 mb-2">Длина помещения (м):</label>
              <input
                type="number"
                value={roomParams.length}
                onChange={(e) => handleInputChange(e, 'length')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Например, 3.5"
                step="0.1"
              />
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Ширина помещения (м):</label>
              <input
                type="number"
                value={roomParams.width}
                onChange={(e) => handleInputChange(e, 'width')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Например, 2.0"
                step="0.1"
              />
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Высота помещения (м):</label>
              <input
                type="number"
                value={roomParams.height}
                onChange={(e) => handleInputChange(e, 'height')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Например, 2.4"
                step="0.1"
              />
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Бюджет (руб):</label>
              <input
                type="number"
                value={budget}
                onChange={(e) => handleInputChange(e, 'budget')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Например, 150000"
                step="1000"
              />
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Стиль:</label>
              <select
                value={style}
                onChange={(e) => handleInputChange(e, 'style')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
              >
                <option value="">Выберите стиль</option>
                {styles.map(s => <option key={s} value={s}>{s}</option>)}
              </select>
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Материал:</label>
              <select
                value={material}
                onChange={(e) => handleInputChange(e, 'material')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
              >
                <option value="">Выберите материал</option>
                {materials.map(m => <option key={m} value={m}>{m}</option>)}
              </select>
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Цвет:</label>
              <select
                value={color}
                onChange={(e) => handleInputChange(e, 'color')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
              >
                <option value="">Выберите цвет</option>
                {colors.map(c => <option key={c} value={c}>{c}</option>)}
              </select>
            </div>
            <div>
              <label className="block text-gray-300 mb-2">Наличие острова:</label>
              <select
                value={hasIsland ? 'yes' : 'no'}
                onChange={(e) => handleInputChange(e, 'hasIsland')}
                className="w-full p-2 border rounded bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
              >
                <option value="">Не важно</option>
                <option value="yes">Да</option>
                <option value="no">Нет</option>
              </select>
            </div>
          </div>

          <div className="text-center mb-6 space-x-4">
            <button
              onClick={findBestOptions}
              className="bg-blue-600 text-white px-6 py-2 rounded hover:bg-blue-700 transition duration-200"
            >
              Подобрать варианты
            </button>
            <button
              onClick={saveResults}
              disabled={results.length === 0}
              className="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700 transition duration-200 disabled:bg-gray-400"
            >
              Сохранить результаты
            </button>
            <button
              onClick={exportToPDF}
              disabled={results.length === 0}
              className="bg-purple-600 text-white px-6 py-2 rounded hover:bg-purple-700 transition duration-200 disabled:bg-gray-400"
            >
              Экспорт в PDF
            </button>
          </div>

          {error && (
            <div className="text-red-400 text-center mb-6">{error}</div>
          )}

          {results.length > 0 && (
            <div>
              <h2 className="text-2xl font-semibold text-center text-white mb-4">Рекомендуемые варианты:</h2>
              <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                {results.map(option => (
                  <div key={option.id} className="bg-gray-800 p-4 rounded-lg shadow">
                    <img src={option.image} alt={option.name} className="w-full h-48 object-cover rounded-lg mb-4 border border-gray-600" />
                    <h3 className="text-xl font-bold text-white">{option.name}</h3>
                    <p className="text-gray-300">Цена: {option.price} руб.</p>
                    <p className="text-gray-300">Размеры: {option.length} x {option.width} x {option.height} м</p>
                    <p className="text-gray-300">Стиль: {option.style}</p>
                    <p className="text-gray-300">Материал: {option.material}</p>
                    <p className="text-gray-300">Цвет: {option.color}</p>
                    <p className="text-gray-300">Остров: {option.hasIsland ? 'Да' : 'Нет'}</p>
                    <p className="text-gray-300 mt-2">{option.description}</p>
                  </div>
                ))}
              </div>
            </div>
          )}

          {savedResults.length > 0 && (
            <div className="mt-6">
              <h2 className="text-2xl font-semibold text-center text-white mb-4">Сохранённые результаты:</h2>
              <ul className="list-disc pl-5">
                {savedResults.map((saved, index) => (
                  <li key={saved.id} className="mb-2 text-gray-300">
                    {`Параметры: Длина ${saved.params.length}m, Ширина ${saved.params.width}m, Высота ${saved.params.height}m, Бюджет ${saved.params.budget} руб., Стиль: ${saved.params.style || 'Не указано'}, Материал: ${saved.params.material || 'Не указано'}, Цвет: ${saved.params.color || 'Не указано'}, Остров: ${saved.params.hasIsland ? 'Да' : 'Нет'} — ${saved.timestamp}`}
                  </li>
                ))}
              </ul>
            </div>
          )}
        </div>
      );
    };

    ReactDOM.render(<App />, document.getElementById('root'));
  </script>
</body>
