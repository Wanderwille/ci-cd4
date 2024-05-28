# Домашнее задание к занятию 10 «Jenkins» - Подус Сергей

## Подготовка к выполнению

1. Создать два VM: для jenkins-master и jenkins-agent.
2. Установить Jenkins при помощи playbook.
3. Запустить и проверить работоспособность.
4. Сделать первоначальную настройку.

## Основная часть

1. Сделать Freestyle Job, который будет запускать `molecule test` из любого вашего репозитория с ролью.
2. Сделать Declarative Pipeline Job, который будет запускать `molecule test` из любого вашего репозитория с ролью.
3. Перенести Declarative Pipeline в репозиторий в файл `Jenkinsfile`.
4. Создать Multibranch Pipeline на запуск `Jenkinsfile` из репозитория.
5. Создать Scripted Pipeline, наполнить его скриптом из [pipeline](./pipeline).
6. Внести необходимые изменения, чтобы Pipeline запускал `ansible-playbook` без флагов `--check --diff`, если не установлен параметр при запуске джобы (prod_run = True). По умолчанию параметр имеет значение False и запускает прогон с флагами `--check --diff`.
7. Проверить работоспособность, исправить ошибки, исправленный Pipeline вложить в репозиторий в файл `ScriptedJenkinsfile`.
8. Отправить ссылку на репозиторий с ролью и Declarative Pipeline и Scripted Pipeline.
9. Сопроводите процесс настройки скриншотами для каждого пункта задания!!


# Ответ:

Подготовил jenkins и Агента:

![Скриншот 1](https://github.com/Wanderwille/scrinshot/blob/scrin2/Настройка%20jenkins.png)

## Основная часть

1. Создал Freestyle Job из репозитория с ролью:

![Скриншот 2](https://github.com/Wanderwille/scrinshot/blob/scrin2/1%20задание.png)

![Скриншот 3](https://github.com/Wanderwille/scrinshot/blob/scrin2/успешно-1задание.png)

![Скриншот 4](https://github.com/Wanderwille/scrinshot/blob/scrin2/шаг%20сборки.png)

2. Создал Declarative Pipeline Job из репозитория с ролью:

![Скриншот 5](https://github.com/Wanderwille/scrinshot/blob/scrin2/задание%202.png)

Скрипт который выполнялся

```
pipeline {
    agent any

    stages {
        stage('GIT checkout') {
            steps {
                echo 'Get from GIT repository'
                git credentialsId: 'git_url',
                url: 'https://github.com/Wanderwille/vector-role',
                branch: 'main'
            }
        }
        stage('preparation') {
            steps {
                echo 'Start preparation'
                sh 'pip3 install -r tox-requirements.txt'
                sh 'pip install "molecule[lint]"'
                sh 'pip install "molecule[docker,lint]"'
            }
        }
        stage('Start molecule test') {
            steps {
                echo 'Run molecule test'
                sh 'molecule test'
            }
        }
    }
}
```

Сборка прошла с успехом

![Скриншот 6](https://github.com/Wanderwille/scrinshot/blob/scrin2/задание2%20успех.png)

3. Перенес в jenkinsfile

4. Создал Multibranch Pipeline:

![Скриншот 7](https://github.com/Wanderwille/scrinshot/blob/scrin2/zadanie4.png)

![Скриншот 8](https://github.com/Wanderwille/scrinshot/blob/scrin2/zadanie4-succus.png)


5. Создал Scripted Pipeline и наполнил его скриптом

![Скриншот 9](https://github.com/Wanderwille/scrinshot/blob/scrin2/zadanie5.png)

6. Внес изменения в скрипт, запуск с параметрами

![Скриншот 10](https://github.com/Wanderwille/scrinshot/blob/scrin2/не%20прод%206%20задание.png)

--check-diff

![Скриншот 11](https://github.com/Wanderwille/scrinshot/blob/scrin2/запуск%20--check.png)

8. Ссылка на role и скрипт:

Role: https://github.com/Wanderwille/vector-role

Скрипты:

1. https://github.com/Wanderwille/ci-cd4/blob/main/SRC/Jenkinsfile

2. https://github.com/Wanderwille/ci-cd4/blob/main/SRC/ScriptedJenkinsfile

