
# Интернет-магазин

Этот проект представляет собой интернет-магазин, разработанный с использованием современных технологий, таких как React, Redux Toolkit, Material-UI и Vite. Проект включает в себя функциональность фильтрации, сортировки, поиска товаров, а также реализацию корзины с возможностью добавления и удаления товаров.

## Основные функции

1. **Фильтрация товаров по категориям**: Пользователи могут фильтровать товары по категориям, таким как "Электроника", "Одежда", "Книги" и "Все".
2. **Сортировка товаров**: Товары можно сортировать по цене (возрастание и убывание) и по рейтингу.
3. **Поиск товаров**: Пользователи могут искать товары по названию.
4. **Корзина**: Пользователи могут добавлять товары в корзину, удалять их и видеть общую стоимость.
5. **Темная и светлая темы**: Пользователи могут переключаться между светлой и темной темами.
6. **Локальное кэширование**: Состояние корзины сохраняется в локальном хранилище браузера.

## Технологии

- **React**: Библиотека для создания пользовательских интерфейсов.
- **Redux Toolkit**: Управление состоянием приложения.
- **Material-UI**: Библиотека компонентов для стилизации и улучшения интерфейса.
- **Vite**: Сборщик проекта для быстрой разработки.
- **Framer Motion**: Библиотека для добавления анимаций.
- **Axios**: Для работы с API.
- **React Router DOM**: Для маршрутизации в приложении.

## Установка и запуск

1. Клонируйте репозиторий:
   bash
   git clone https://github.com/SkrepLook192/pract-9101112
   cd ваш-репозиторий


   Установите зависимости**:
   bash
   npm install
   

3. Запустите mock API:
   bash
   json-server --watch public/data.json --port 3001
   

4. Запустите приложение:
   bash
   npm run dev
   

5. Откройте браузер и перейдите по адресу `http://localhost:5173`.

 Структура проекта

- **src/api/**: Функции для работы с API.
- **src/components/**: React-компоненты.
- **src/features/**: Слайсы Redux.
- **src/pages/**: Страницы приложения.
- **src/store/**: Настройка Redux store.
- **src/styles/**: Глобальные стили и темы.

Примеры кода

 Фильтрация товаров

javascript
import React from 'react';
import { Button, ButtonGroup } from '@mui/material';

const FilterPanel = ({ onFilterChange }) => {
    const categories = ['Все', 'Электроника', 'Одежда', 'Книги'];
    return (
        <ButtonGroup variant="contained" sx={{ my: 2 }}>
            {categories.map((category) => (
                <Button key={category} onClick={() => onFilterChange(category)}>
                    {category}
                </Button>
            ))}
        </ButtonGroup>
    );
};

export default FilterPanel;

### Сортировка товаров

javascript
import React from 'react';
import { FormControl, InputLabel, Select, MenuItem } from '@mui/material';

const SortPanel = ({ onSortChange }) => {
    const sortOptions = [
        { value: 'default', label: 'По умолчанию' },
        { value: 'priceAsc', label: 'По цене (возрастание)' },
        { value: 'priceDesc', label: 'По цене (убывание)' },
        { value: 'rating', label: 'По рейтингу' },
    ];

    return (
        <FormControl fullWidth sx={{ my: 2 }}>
            <InputLabel>Сортировка</InputLabel>
            <Select
                label="Сортировка"
                onChange={(e) => onSortChange(e.target.value)}
                defaultValue="default"
            >
                {sortOptions.map((option) => (
                    <MenuItem key={option.value} value={option.value}>
                        {option.label}
                    </MenuItem>
                ))}
            </Select>
        </FormControl>
    );
};

export default SortPanel;


### Корзина

javascript
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { removeFromCart, clearCart } from '../features/cartSlice';
import { Card, CardContent, Typography, Button, List, ListItem, ListItemText } from '@mui/material';
import { motion } from 'framer-motion';

const Cart = () => {
    const dispatch = useDispatch();
    const { items, total } = useSelector((state) => state.cart);

    return (
        <Card sx={{ maxWidth: 345, margin: 2 }}>
            <CardContent>
                <Typography variant="h6">Корзина</Typography>
                <List>
                    {items.map((item) => (
                        <motion.div
                            key={item.id}
                            initial={{ opacity: 0, y: 20 }}
                            animate={{ opacity: 1, y: 0 }}
                            transition={{ duration: 0.5 }}
                        >
                            <ListItem>
                                <ListItemText
                                    primary={item.title}
                                    secondary={`${item.quantity} x ${item.price}$`}
                                />
                                <Button onClick={() => dispatch(removeFromCart(item))}>Удалить</Button>
                            </ListItem>
                        </motion.div>
                    ))}
                </List>
                <Typography variant="h6">Общая стоимость: {total}$</Typography>
                <Button onClick={() => dispatch(clearCart())} variant="contained" color="error">
                    Очистить корзину
                </Button>
            </CardContent>
        </Card>
    );
};

export default Cart;
