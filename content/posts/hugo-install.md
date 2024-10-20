+++
date = '2024-10-20T09:30:00+03:00'
draft = false
title = 'Hugo Install'
+++

# Подготовка к установке

Для начала необходимо установить Git, подходящий для вашей операционной системы. Скачайте его с официального сайта: [https://git-scm.com/downloads](https://git-scm.com/downloads)

Устанавливайте Git, не изменяя стандартные настройки, предложенные установщиком, за исключением одного пункта:
- Если у вас уже установлен Notepad++, выберите его в разделе "Choosing the default editor used by Git".
- В противном случае рекомендуется выбрать редактор **Nano**.

---

# Установка Hugo

## Для Windows

### Перейдите на страницу последнего релиза

Зайдите на страницу с последней версией Hugo: [latest release](https://github.com/gohugoio/hugo/releases/latest). Здесь вы найдете исходный код и скомпилированные файлы.

- Пролистайте страницу до раздела **Assets**.
- Выберите архив для вашей операционной системы и архитектуры.

### Скачайте нужный архив

- Рекомендуется скачать версию **extended**.
- Убедитесь, что архив подходит для вашей операционной системы (Windows) и архитектуры процессора (x86 или x64).
- Скачайте архив.

### Извлеките содержимое архива

После загрузки распакуйте архив и переместите его в удобный для вас каталог.

### Добавьте путь к файлу `hugo` в переменную окружения PATH

Чтобы операционная система могла найти исполняемый файл, добавьте его в переменную окружения `PATH`:

- Откройте _Панель управления → Система → Дополнительные параметры системы → Переменные среды_.
- В разделе **Системные переменные** найдите переменную `Path` и добавьте путь к файлу.
    - Например, `C:\hugo-extended\`

---

## Для Linux

Установите Hugo через менеджер пакетов:

```bash
sudo apt-get install hugo
```

---

## Для macOS

Установите Hugo через Homebrew:

```bash
brew install hugo
```

---

## Проверьте успешность установки

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

**PaperMod** — минималистичная и удобная тема для Hugo. Чтобы установить её, выполните следующие шаги:

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
- **description**: Краткое описание сайта.
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

Добро пожаловать на мой сайт, созданный в Hugo!
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

- Зарегистрируйтесь на [github.com](https://github.com/), если ещё не сделали этого.
- Создайте новый репозиторий с именем `<username>.github.io`, где `<username>` — ваш ник на GitHub.

### Настройка разрешений по умолчанию для деплоя

По умолчанию, при создании нового репозитория в вашем личном аккаунте, **GITHUB_TOKEN** имеет доступ только для чтения к содержимому. Для того чтобы экшен мог добавить ветку **gh-pages** в репозиторий, необходимо дать права на чтение и запись.

- Перейдите на главную страницу созданного репозитория.
- Под именем репозитория нажмите на **Settings**. (Если вкладка **Settings** не отображается, выберите её из выпадающего меню.)
- В левом боковом меню выберите **Actions**, затем нажмите **General**.
- В разделе **Workflow permissions** выберите **Read and write access**.
- Нажмите **Save**, чтобы применить настройки.

---

Добавьте удалённый репозиторий:

```bash
git remote add origin https://github.com/<username>/<username>.github.io.git

git branch -M main
```

Загрузите файлы:

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

---

# Настройка GitHub Actions

Создайте файл `.github\workflows\gh-pages.yml`:

```bash
cmd /c "mkdir .github\workflows && cd .github\workflows && echo. > gh-pages.yml"
```

Внесите в файл следующие настройки:

```yaml
name: GitHub Pages

on:
  push: 
    branches:
      - main
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    permissions:
      contents: write
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: '0.119.0'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
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

## Настройка ветки для отображения сайта

Для первой публикации необходимо настроить ветку для отображения сайта:

- Перейдите в ваш репозиторий.
- Далее **Settings**.
- Слева выберите **Pages**.
- В разделе **Build and deployment - Branch** раскройте выпадающее меню.
- Выберите **gh-pages**, правее должно быть указано **/ (root)**.
- Нажмите **Save**.

Эту настройку необходимо выполнить только один раз.

---

# Ожидание публикации

GitHub Pages соберёт сайт и опубликует его через несколько минут. Ваш сайт будет доступен по адресу `https://<username>.github.io/`.

Дальнейшие публикации или изменения можно производить через выполнение команд из раздела [[Создание контента]] и отправку коммита в репозиторий с помощью команд:

```bash
git add .
git commit -m "Update content"
git push origin main
```

Страницы будут собраны автоматически и обновятся через несколько минут.