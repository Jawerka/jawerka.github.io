+++
date = '2024-10-19T03:49:14+03:00'
draft = false
title = 'Hugo Install'
+++

# Подготовка к установке

Для начала необходимо установить Git, подходящий для вашей операционной системы. Скачайте его с официального сайта:  
[https://git-scm.com/downloads](https://git-scm.com/downloads)

Устанавливайте Git, не изменяя стандартные настройки, предложенные установщиком, за исключением одного пункта:
- Если у вас уже установлен Notepad++, выберите его в разделе "Choosing the default editor used by Git".
- В противном случае рекомендуется выбрать редактор **Nano**.

---

# Установка Hugo

## Для Windows

#### Перейдите на страницу последнего релиза
Зайдите на страницу с последней версией Hugo: [latest release](https://github.com/gohugoio/hugo/releases/latest). Здесь вы найдете исходный код и скомпилированные файлы.

- Пролистайте страницу до раздела **Assets**.
- Выберите архив для вашей операционной системы и архитектуры.

#### Скачайте нужный архив
- Рекомендуется скачать версию **extended**.
- Убедитесь, что архив подходит для вашей операционной системы (Windows) и архитектуры процессора (x86 или x64).
- Скачайте архив.

#### Извлеките содержимое архива
После загрузки распакуйте архив, и переместите его в удобный для вас каталог.

#### Добавьте путь к файлу `hugo` в переменную окружения PATH
Чтобы операционная система могла найти исполняемый файл, добавьте его в переменную окружения `PATH`.

- Откройте _Панель управления → Система → Дополнительные параметры системы → Переменные среды_.
- В разделе **Системные переменные** найдите переменную `Path` и добавьте путь к файлу.
	*Например `C:\hugo-extended\`

### Для Linux

Установите Hugo через менеджер пакетов:
```bash
sudo apt-get install hugo
```

### Для macOS

Установите Hugo через Homebrew:
```bash
brew install hugo
```

### Проверьте успешность установки
Откройте терминал:
- Для Windows: Shift + ПКМ в нужной папке и выберите "Запуск PowerShell".

Выполните команду:
```bash
hugo version
```

Если вы видите версию программы, установка прошла успешно.

---

# Создание нового проекта Hugo

Создайте новую папку для сайта:
```bash
hugo new site my-hugo-site
```

Замените `my-hugo-site` на имя вашего проекта. Hugo создаст структуру проекта с папками, такими как `content`, `themes`, `layouts` и т.д.

---

# Установка темы PaperMod

**PaperMod** — минималистичная и удобная тема для Hugo. Чтобы установить ее, выполните следующие шаги:

Перейдите в папку проекта:
```bash
cd my-hugo-site
```

И инициализируйте git-репозиторий:
```bash
git init
```

Добавьте тему как submodule:
```bash
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

---

# Настройка файла конфигурации `hugo.toml`

Файл `hugo.toml` управляет основными настройками сайта. Пример базовой конфигурации для темы PaperMod:
```toml
baseURL = "https://<ваш-логин>.github.io/"
title = "Мой Hugo Сайт"
theme = "PaperMod"
languageCode = "ru-ru"

[params]
  author = "Ваше имя"
  description = "Описание сайта"
  ShowReadingTime = true
  defaultTheme = "auto"
```

### Основные параметры:
- **author**: Имя автора.
- **description**: Краткое описание.
- **ShowReadingTime**: Показывать время чтения (true/false).
- **defaultTheme**: Тема сайта (`dark`, `light`, `auto`).

---

# Создание контента

Для создания первого поста выполните команду:
```bash
hugo new posts/welcome.md
```

Это создаст файл `welcome.md` в папке `content/posts`. Откройте его и добавьте текст между знаками `+++`:
```markdown
+++
date = '2024-10-19T03:49:14+03:00'
draft = false
title = 'Приветственный пост'
+++

Добро пожаловать на мой сайт, созданный с Hugo!
```

Убедитесь, что параметр `draft = false`, чтобы пост был доступен для просмотра.

---

# Локальная сборка и проверка сайта

Для запуска сервера разработки выполните команду:
```bash
hugo server
```

Перейдите по адресу [http://localhost:1313/](http://localhost:1313/), чтобы увидеть сайт.

Остановите сервер с помощью комбинации `Ctrl + C`.

---

# Публикация на GitHub Pages

- Зарегистрируйтесь на [github.com](https://github.com/), если ещё не сделали этого.
- Создайте новый репозиторий с именем `<username>.github.io`, где `username` — ваш ник на GitHub.
- Подтвердите публикацию на GitHub Pages. В настройках репозитория: Settings -> Раздел Options -> Секция GitHub Pages.

Добавьте удалённый репозиторий:
```bash
git remote add origin https://github.com/<username>/<username>.github.io.git
```

Загрузите файлы:
```bash
git add .
git commit -m "Initial commit"
git push origin main
```

---

### Настройка GitHub Actions

Создайте файл `.github\workflows\gh-pages.yml`:
```bash
cmd /c "mkdir .github\workflows && cd .github\workflows && echo. > gh-pages.yml"
```

Внесите в файл следующие настройки:
```yaml
name: Deploy Hugo site to GitHub Pages
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'

      - name: Build the website
        run: hugo --minify

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

Добавьте файл в git и отправьте изменения:
```bash
git add .github/workflows/gh-pages.yml
git commit -m "Add GitHub Actions"
git push origin main
```

---

# Ожидание публикации

GitHub Pages соберет сайт и опубликует его через несколько минут. Ваш сайт будет доступен по адресу `https://<ваш-логин>.github.io/`.

Теперь ваш сайт с темой **PaperMod** опубликован и готов к использованию!