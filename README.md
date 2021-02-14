# Домашнее задание к занятию «Выстраивание процесса непрерывной интеграции (CI): Github Actions. Покрытие кода с JaCoCo, статический анализ кода: CheckStyle, SpotBugs»
В качестве результата пришлите ссылки на ваши GitHub-проекты в личном кабинете студента на сайте netology.ru.

Все задачи этого занятия нужно делать в разных репозиториях.

**Важно:** если у вас что-то не получилось, то оформляйте Issue по установленным правилам.

Вы можете делать все задачи этого занятия в одном репозитории (если делаете их последовательно).

**Как сдавать задачи**
Инициализируйте на своём компьютере пустой Git-репозиторий
Добавьте в него готовый файл .gitignore
Добавьте в этот же каталог необходимые файлы
Сделайте необходимые коммиты
Создайте публичный репозиторий на GitHub и свяжите свой локальный репозиторий с удалённым
Сделайте пуш (удостоверьтесь, что ваш код появился на GitHub)
Ссылку на ваш проект отправьте в личном кабинете на сайте netology.ru
Задачи, отмеченные, как необязательные, можно не сдавать, это не повлияет на получение зачета (в этом ДЗ все задачи являются обязательными)

## Задача №1 - Синдром 100%
**Легенда**
Вы попали в команду максималистов, которые хотят, чтобы те авто-тесты, которые вы пишете, покрывали код на 100%.

Но вот незадача:

Непонятно, что такое 100%
Непонятно, как это сделать
Вспоминаем: покрытием кода у нас занимается JaCoCo, но он просто "сигнализирует" о том, что конкретно пошло не так.

Большинство подобных плагинов помимо целей отчётности (report) содержат ещё цель check, которая обрушает сборку, если не выполнены определённые проверки.

**Что вам нужно:**

Изучить документацию на плагин (а конкретно на цель check)
Внедрить эту цель в фазу verify (обратите внимание, что эта цель итак публикуется в эту фазу)
Настроить правила по покрытию на 100% (при этом нужно изучить разницу между счётчиками INSTRUCTION, LINE, BRANCH, COMPLEXITY)
Выбрать один из счётчиков и добиться 100% покрытия тестами
Важно: использовать можно только один из следующих:

INSTRUCTION
LINE
BRANCH
По шагам:

Скачиваете проект с лекции
Изучаете документацию, экспериментируете со счётчиками (INSTRUCTION, LINE, BRANCH, COMPLEXITY)
Выбираете один из счётчиков (на ваше усмотрение из первых трёх: INSTRUCTION, LINE, BRANCH)
Запускаете, удостоверяетесь, что сборка падает, делаете push в GitHub (так, чтобы в логах осталось, что сборка падала в CI)
Дописываете тесты, чтобы обеспечить 100% по выбранному счётчику, делаете push в GitHub (так, чтобы в логах осталось, что сборка зелёная в CI)
**Итого:** у вас должен быть репозиторий на GitHub, в котором расположен ваш Java-код + файлик README.md, в котором кратко описано, какой из счётчиков за что отвечает.

**Подсказка №1** Не всегда все плагины хорошо документированы. Достаточно часто плагин просто запускает какой-то инструмент. И именно в документации самого инструмента раскрываются значения параметров.

Также и с JaCoCo. Вы можете найти описание счётчиков и их назначения на странице документации самого инструмента JaCoCo (не плагина).

**Подсказка №2 (пример настройки)** 
<executions>
  ...
  <execution>
    <id>check</id>
    <goals>
      <goal>check</goal>
    </goals>
  </execution>
</executions>
<configuration>
  <rules>
    <rule>
      <limits>
        <limit>
          <counter>ВАШ COUNTER</counter>
          <value>COVEREDRATIO</value>
          <minimum>100%</minimum>
        </limit>
      </limits>
    </rule>
  </rules>
</configuration>

**Важно:** если вы используете Lombok, то покрытие сгенерированных им методов (например, getter/setter'ы тоже учитываются). Это не всегда бывает нужно.

Варианта тут два:

Вы читаете документацию на excludes, которая позволяет вам исключать целые классы из рассмотрения (не нав вариант)
Игнорируете сгенерированные Lombok'ом методы (для этого создайте файл lombok.config в корне проекта со следующим содержимым: lombok.addLombokGeneratedAnnotation = true и не забудьте сделать mvn clean)


## Счетчики 
**Instruction:** Предоставляет информацию о том, какое количество кода было выполнено или пропущено. 
**Branches:** Предоставляет информацию о том, сколько операторов *if* и *switch* в методе и сколько из них были выполнены. 
**Lines:** Предоставляет информацию о построчном исполнении кода.
