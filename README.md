# Отчет по лабораторной работе lab04

## Выполнение работы

### Подготовка репозитория и окружения
Был создан новый публичный репозиторий с названием lab04. В терминале виртуальной машины файлы проекта из предыдущей лабораторной работы были скопированы в новый каталог. Также был добавлен файл `.gitignore` для исключения локального кэша сборки, который мог нарушить работу удаленных серверов:

* Инициализация и копирование файлов проекта в новый рабочий каталог lab04:
```bash
cd ~/x0mwsdc/workspace/projects
cp -r lab03 lab04
cd lab04
```

* Настройка связи с новым удаленным репозиторием lab04:
```bash
git remote remove origin
git remote add origin [https://github.com/x0mwsdc/lab04.git](https://github.com/x0mwsdc/lab04.git)
```

* Создание файла .gitignore для предотвращения отслеживания временных файлов компиляции:
```bash
echo "build/" > .gitignore
```

### Настройка непрерывной интеграции (CI)
Для автоматизации сборки и проверки корректности проекта при каждом обновлении кода был настроен инструмент GitHub Actions. Вместо устаревшего Travis CI в корне репозитория была создана служебная директория с конфигурационным файлом `cmake.yml`, описывающим правила развертывания окружения Ubuntu и компиляции через CMake:

* Создание структуры каталогов для GitHub Actions и файла конфигурации сборки:
```bash
mkdir -p .github/workflows
cat > .github/workflows/cmake.yml <<EOF
name: CMake Build

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Configure CMake
      run: cmake -B build -S .

    - name: Build project
      run: cmake --build build
EOF
```

### Публикация и проверка результатов
Все подготовленные файлы конфигурации автоматизации, исходный код и файл описания проекта были проиндексированы в Git, закоммичены и отправлены в удаленный репозиторий:

* Добавление созданных файлов конфигурации CI и исключений в индекс Git:
```bash
git add .gitignore .github/workflows/cmake.yml
```

* Фиксация изменений в репозитории:
```bash
git commit -m"remove local build cache and add gitignore"
```

* Отправка изменений в ветку main на GitHub с прохождением авторизации:
```bash
git push origin main
```

Проверка Github actions прошла успешно.
