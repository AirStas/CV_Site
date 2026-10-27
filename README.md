# Сайт-портфолио Станислава Холькина

Одностраничный сайт-резюме: светлая и тёмная тема (по умолчанию как в системе, выбор запоминается), адаптив под телефон, анимации при прокрутке, кнопка скачивания PDF-резюме.

Стек: чистые HTML/CSS/JS без сборки, раздаёт nginx в Docker.

## Структура

```
.
├── site/                  # всё, что видит посетитель
│   ├── index.html         # точка входа и весь контент
│   ├── styles.css         # стили, цвета тем в начале файла
│   ├── script.js          # переключатель темы, анимации, подсветка меню
│   └── assets/
│       ├── photo.jpg      # фото для первого экрана
│       ├── uav.jpg        # рендер БПЛА (обрезанный)
│       ├── bmstu.jpg      # кампус МГТУ, Wikimedia Commons, CC0
│       ├── zvirt.svg      # логотип zVirt с сайта orionsoft.ru
│       └── Kholkin_Stanislav_CV.pdf
├── nginx.conf             # gzip, кеширование, security-заголовки
├── Dockerfile
├── docker-compose.yml
└── Резюме/                # исходники резюме, в образ не попадают
```

## Запуск локально

```sh
docker compose up -d --build
```

Сайт: http://localhost:8080. Другой порт: `PORT=3000 docker compose up -d --build`.

Остановить: `docker compose down`.

Без Docker: `python3 -m http.server 8080 --directory site`.

## Деплой на VPS

```sh
# на сервере
git clone <репозиторий> portfolio && cd portfolio   # или scp -r
PORT=80 docker compose up -d --build
```

Для HTTPS и домена `stanislavkholkin.ru` удобнее всего поставить перед контейнером Caddy или nginx с certbot и проксировать на порт контейнера.

## Как править

- Тексты лежат в `site/index.html`, разбиты по секциям с комментариями (`HERO`, `ABOUT`, `SKILLS`, …).
- Акцентный цвет задаёт `--accent` в начале `styles.css`: отдельно для светлой и тёмной темы.
- Новое резюме: замените `site/assets/Kholkin_Stanislav_CV.pdf`.
- Картинки лежат в `site/assets/`: файл можно заменить, сохранив имя.
- После правок пересоберите образ: `docker compose up -d --build`.
