
# Система видео аналитики для управления очередью людей

## Содержание

- [Введение](#введение)
- [Проблема](#проблема)
- [Анализ области](#анализ-области)
- [Описание предлагаемого решения](#описание-предлагаемого-решения)
	- [Первая система](#первая-система)
	- [Вторая система](#первая-система)
- [Высокоуровневый план реализации решения](#высокоуровневый-план-реализации-решения) 
- [Описание отдельных шагов плана](#описание-отдельных-шагов-плана)
	- [Формирование датасета](#формирование-датасета)
	- [Обучение модели](#обучение-модели)
	- [Тестирование модели](#тестирование-модели)
	- [Создание алгоритма для первой системы](#создание-алгоритма-для-первой-системы)
	- [Создание алгоритма для второй системы](#создание-алгоритма-для-второй-системы)
- [Макет интерфейса приложения](#макет-интерфейса-приложения)


## Введение

В ресторане "Вега", расположенном на территории университета "Сириус", систематически возникают проблемы с длинными очередями в периоды часов пика. Это, в свою очередь, приводит к значительным задержкам для студентов, преподавателей и посетителей заведения. Такие задержки могут серьезно нарушить распорядок дня, уменьшая доступное время на академическую деятельность, исследования и личные встречи.
Кроме того, длительное ожидание в очередях может способствовать усилению стресса среди учащихся и преподавательского состава, что негативно сказывается на общей атмосфере в учебном заведении. В условиях, когда время является ценным ресурсом, необходимость тратить его на ожидание в очереди в ресторане воспринимается как неоправданное ухудшение качества образовательного процесса и быта университетского сообщества.


## Проблема
 
Проблема неэффективного расходования времени в ресторане "Вега" университета "Сириус" обусловлена большим количеством посетителей, особенно в периоды пиковой загруженности, когда число людей в очереди превышает 50 человек. В связи с увеличением потока клиентов, время ожидания в очереди на кассе ресторана может возрасти до 20-30 минут, что вызывает значительный дискомфорт и неприятные эмоции у посетителей. Чтобы обеспечить комфортное и приятное посещение ресторана, необходимо эффективно управлять данной ситуацией.


## Анализ области

| Критерий | YOLO | SSD | Faster R-CNN | Mask R-CNN|
| :------: | :--: | :-: | :----------: | :--------:|
|**Скорость**|Очень высокая|Высокая|Средняя|Низкая|
|**Точность**|Средняя|Средняя|Высокая|Высокая|
|**Обнаружение**|Объекты и их местоположение в одном проходе |Объекты и их местоположение в одном проходе|Обнаружение объектов и их границ|Обнаружение объектов, их границ и сегментация|
|**Использует**|Сверточные нейронные сети (CNN)|Сверточные нейронные сети (CNN)|Сверточные нейронные сети (CNN)|Сверточные нейронные сети (CNN)|
|**Применение**|Реальное время и задачи, где важна скорость обнаружения| Обнаружение в реальном времени и высокая скорость|Основные задачи компьютерного зрения, где важна точность|Сегментация и обнаружение объектов в изображениях|

Также мы проанализировали какой минимальный набор технических средств для системы видеоанализа понадобится и выяснили, что нам понадобится:
 1. Камеры наблюдения:
 
	 Для обработки полученного видеопотока и запуска моделей машинного обучения необходимо наличие серверного оборудования
 
 2. Серверное оборудование:
 
	Для обучения и выполнения моделей машинного обучения также требуются вычислительные ресурсы. Это может включать как локальные компьютеры с высокой производительностью, так и облачные вычислительные ресурсы
 
 3. Вычислительные ресурсы:
    
	 Для обучения и выполнения моделей машинного обучения также требуются вычислительные ресурсы. Это может включать как локальные компьютеры с высокой производительностью, так и облачные вычислительные ресурсы


# Описание предлагаемого решения

Мы планируем создать две взаимодополняющие системы, которые существенно улучшат уровень обслуживания клиентов и уменьшат время ожидания на кассе. Первая система будет в режиме реального времени предоставлять актуальную информацию о загруженности заведения, что позволит посетителям рационально планировать свое время и избегать очередей. Вторая система оптимизирует распределение посетителей по очередям, учитывая количество свободных мест. В совокупности, эти системы предоставят посетителям полезную информацию и помогут им сделать более осознанный выбор, сокращая тем самым время ожидания.


## Первая система

Первая система автоматически подсчитывает количество посетителей в ресторане и на этой основе определяет уровень его загруженности. Это крайне важно для потенциальных посетителей заведения, позволяя им оценить текущую заполненность заведения перед принятием решения о посещении.
При создании данной системы мы будем использовать видеокамеру, размещенную на входе и выходе ресторана, для непрерывного считывания видеопотока в реальном времени. Это позволит непрерывно отслеживать количество людей, входящих и выходящих из заведения, и определять текущую загруженность ресторана.


## Вторая система

Вторая система автоматически отслеживает людей, находящихся в очереди, и вычисляет процент загруженности каждой из очередей. Затем система визуализирует данные, чтобы показать посетителям, какая очередь является наиболее выгодной с точки зрения времени ожидания.
Для этой системы мы установим камеры над очередями для записи видеопотока. Система будет использовать маски для выделения областей с очередями на видеокадрах и подсчета людей. Она также определит менее загруженные очереди и предложит посетителям присоединиться к ним.


## Высокоуровневый план реализации решения

1. **Формирование датасета**: 

	Использование общедоступных датасетов (например, [COCO](https://docs.ultralytics.com/datasets/detect/coco/)). Также возможно создание собственного путем сбора изображений людей и использования инструментов для разметки данных ([Roboflow](https://app.roboflow.com)).

2. **Обучение модели**: 
	
	Использование архитектуры YOLOv8n для обучения модели обнаружения людей на выбранном датасете

3. **Тестирование модели**: 
	
	Запуск обученной модели на видеопотоке для оценки ее производительности и точности
	
4. **Создание алгоритма для первой системы**:
   
	Алгоритм должен подсчитывать количество посетителей, входящих и выходящих из заведения

5. **Создание алгоритма для второй системы**:
   
	Алгоритм будет отслеживать людей в очередях и определять загруженность каждой из них


# Описание отдельных шагов плана

Мы планируем использовать Python, потому что это предпочтительный язык программирования для многих нейронных сетей. Python обладает поддержкой TensorFlow, PyTorch и имеет читаемый синтаксис. Также он широко используется в сообществе разработчиков.

Для создания интерфейса мы воспользуемся языком разметки и стилей HTML, CSS, а также языком программирования JavaScript.

Для задач по обучению модели планируется использовать API Ultralytics, которые по умолчанию поставляются с пакетом YOLOv8. API основано на фреймворке PyTorch, который часто используется для создания нейронных сетей.


## Формирование датасета
Для обучения модели для детекции мы использовали 67 тысяч извлечённых изображений с людьми из набора данных [COCO2017](https://cocodataset.org#home).

Из них 64 тысячи изображений составили обучающий набор, 3 тысячи - валидационный.

Извлеченные данные были преобразованы в формат, совместимый с архитектурой YOLO, с помощью API Ultralytics. Также были вырезаны все классы, кроме "person".

![labels](https://github.com/pocketgodru/SiriusAI-QA/assets/126785140/e4424b41-90b4-4c49-93ab-0486df77f077)

> Общее количество людей в обучающем наборе данных


## Обучение модели
Обучение модели проводилось в течение 65 эпох. К концу обучения на валидационном датасете были достигнуты следующие результаты:

-   **Точность (Precision):** 0.82
-   **Полнота (Recall):** 0.65
-   **Средняя точность (mAP@0.5):** 0.76
-   **Средняя точность (mAP@0.5:0.95):** 0.53
 
![img](https://github.com/pocketgodru/SiriusAI-QA/assets/126785140/8f6ac154-edc2-4595-8d52-7f8137ca0df7)


## Тестирование модели

Сейчас мы находимся в процессе тестирования обученной модели на видеопотоке с целью оценки её производительности и точности. Наша цель - достичь максимальной скорости работы и точности выявления объектов на видео.

| Модель обученная нами | Предобученная YOLOv8n |
| :-------------------: | :-------------------: |
|![img](https://github.com/pocketgodru/SiriusAI-QA/assets/126785140/04ff6d1c-38d7-4411-b6cb-354b92dcad86)| ![img](https://github.com/pocketgodru/SiriusAI-QA/assets/126785140/9e240755-effd-4ea4-bcb7-0ecafd9a2961) |

Как можно заметить - наша модель лучше детектирует людей при одинаковой скорости работы.


## Создание алгоритма для первой системы

Пример работы первой системы:

![Запись экрана 2024-02-24 в 11 17 04 (1)](https://github.com/pocketgodru/SiriusAI-QA/assets/126785140/0a04ca48-a61f-4ee3-b148-a986d8b05acc)

## Создание алгоритма для второй системы

> В процессе реализации

# Макет интерфейса приложения

 > Добавить фотку

В интерфейсе имеется:

- Число людей в ресторане

- Общая нагрузка

- Цветовая визуализация загруженности очереди. При нажатии высвечивается время ожидания и количество людей, стоящих в ней
