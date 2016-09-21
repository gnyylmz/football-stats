# Здравейте приятелчета!

Football stats е система, която ще помогне при анализирането на футболните мачове. Крайният резултат, ще бъде анализиране на позициите на играчите, топката и разиграните ситуации и изготвяне на heatmap-ове и статистики спрямо тези данни.

# Как да стартирам демото?

1. Инсталирате си Python и PIP (ако не върви с него)
2. `pip install липсваща_библиотека`
3. Инсталирате си OpenCV (за Windows: http://docs.opencv.org/3.1.0/d5/de5/tutorial_py_setup_in_windows.html, за Mac много подобен процес, питайте Цецо при проблеми ;) )
4. стартирате demo-то с `python motion_detector_improved.py --video път_към_видео_файл` или `python motion_detector_improved.py` ще тръгне с видео от камерата ви
5. Кликвате 4 пъти на екрана очертавайки игралното поле
6. Profit

# Sprint 1:
- Първи спринт започна на 13.09.2016 с екип (по произволен, латинско-азбучен ред): Цецо (ceco@devlabs.bg), Радо (rado@devlabs.bg), Веселин (veselin@devlabs.bg) в development и Гого (goran@devlabs.bg) в съпорт. Изключително сме доволни от проявеният интерес от останалите ни колеги, интересните дискусии и спонтанните брейнсторминг сесии, които спринта инициира. Kudos, както се казва по български.

## Video Обобщение

Summary на информацията за спринта и представяне на демото можете да видите във видеото, което заснехме за вас с много любов <3

https://www.youtube.com/watch?v=R9vkffBp5Qc

## Процеса на работа

### Ден първи

Имахме почти изчистена идеята за продукта и трябваше да ошлайфаме добре детайлите. След кратка среща набелязахме конкретните цели, към които може да се стреми проекта:
- Heatmaps за активността на играчите
- Snapshot-и на моментни ситуации
- Статистически репорти за скорост, ускорение, сърдечен ритъм, успеваемост на удърите, пасове, ball possession, out/corner/throw-ins и др.

След кратка дискусия стигнахме до заключението, че за scope първи е трябва да си поставим постижими цели, които да дадат старт на идеята или да валидират нейната изпълнимост.

Sprint 1 scope:
Извличане на информация за позицията на играчите по игралното поле и нейната визуализация, изготвяне на proof-of-concept демо с code examples.

След известно количество брейнсторминг набелязахме потенциалните кандидат-технологии, които може да се използват за определяне на позицията на играчите.
- GPS
- Image processing / Computer vision
- IR Detection
- 3D Reconstruction
- Bluetooth/Wifi signal strength triangulation
- Ultrasonic sensors
- LAZERS!

Разпределихме задачите за research и остатъка от ден 1ви, както и част от ден 2ри мина в research/assessment на използваемостта на тези технологии за нашия проект.

### Ден втори

След research-а обобщихме екипно резултата за research-а по всяка една технология, набелязахме за тях плюсове и минуси, както и набелязахме кандидат-технологията, която искаме да използваме за проекта.
- GPS - не достатъчна прецизност на GPS-ите в достъпните потребителски устройства (4-8м грешка, 1hz сигнали), Има евентуални възможности за решения чрез по-скъпо оборудване (по-корави GPS-и, с по-големи антени), които теоретично биха минимизирали грешката от 4 до 1м, както и инсталиране на RTK станция (https://en.wikipedia.org/wiki/Real_Time_Kinematic) - теоретично с "up to centimetre-level accuracy", прекалено скъпи и енергоемки решения за масовия потребител.
- CV - TODO
- IR - за да работят (дори и неточно) са нужни множество светлинни източници по играчите, дори и тогава не е сигурно. Възможно е да помогне при идентифициране на играчите, като всеки играч "излъчва/свети" в различен infrared диапазон и за всеки играч се прилага различен филтър върху камерата. За ограниченото време в първи спринт преценихме, че трудно ще докараме работещ прототип и идеята на този етап отпадна.
- 3D - 3D reconstruction накратко представлява взимане на stereo image (образ от две камери) и изготвяне на т.нар. depth map от тях. Сравняват се пикселите от единия образ с пикселите от другия образ, комбинирано с намиране на контури, се определя колко далеч от камерата се намира дадения обект. Процеса е сравнително податлив на грешки и неточни данни. Първата стъпка е изготвяне на point cloud, в който точките от камерата са позиционирани в 3D Пространството, той се преобразува в 3D модели като един вид се "свържат точките". За да се достигне прецизност тук е необходимо това да се изпълни върху повече от два кадъра за даден момент (т.е. да имаме повече от две камери на полето), като колкото повече камери има толкова по-точен може да бъде алгоритъма. Целият процес застъпва до голяма степен похвати от computer vision метода, като би бил по-неефективен за голям размер игрище и ресурсоемък (необходимост от много повече камери и непрекъснато обработване на кадрите от тях), като motion tracking-а би бил по-ефективен за нашия use case. Ако пресъздаваме 3D обекти например, този подход би бил по-подходящ. (напр. за 3D скенер) 
- BT - не достатъчна прецизност на bluetooth устройствата, дори и с по-скъпа апаратура. Нужни са десетки bluetooth трансмитери дори и за прост detection на един обект. Твърде много (и скъпо) оборудване за масовия потребител.
- US - По принцип е достъпна и сравнително евтина технология за измерване на разстояние/засичане на обекти, но навсякъде се говори за обхват до 6-10 метра в най-добрия случай (което не е достатъчно дори и за малко игрище). Също така, не е много подходящо в шумна среда (феновете обикновено са шумни, някои дори ползват вувузели ;), която може да води до изкривявания в сигналите.
- LZ - яко е и може много прецизно да измери разстоянието до обект, дори и да е много далече, НО изисква изключително точно насочване. Това общо взето означава, че трябва да има по един лазер за всеки играч и трябва да се комбинира с технология (механика), която постоянно да следи играча и да насочва лазера към него. Друг вариант е няколко лазера, да се разположат на определени места покарай терена и да "сканират" (чрез постоянно въртене около оста си) заобикалящите ги обекти. Звучи интересно, но доста тегаво щеше да е да докараме нещо дори близко до proof-of-concept за 3-4 дни, което все пак беше една от целите на спринта. А и мисля, че такава технология би имала много по-добро приложение в друг вид проект, например self-driving колата.

В крайна сметка избрахме да се фокусираме върху computer vision и image processing с OpenCV.

### Ден трети

Набелязахме потенциалните направления от computer vision, които трябва да разучим за да успеем да извадим data за позицията на играчите по футболното игрище. Разделихме ги на 3 основни групи:
- Player detection
- Player identification
- Player camera to field coordinates projection

Отново разделихме задачите и започнахме главно изучаване на предлаганите от библиотеката възможности за CV и запознаване с Python (езика, с който избрахме да цъкаме на OpenCV, никой от нас нямаше опит с Python, поехме този challenge).

### Ден четвърти

Разполагахме с някакъв background и working examples по OpenCV, заснехме няколко sample видеа отзад в #лятнатаГрадина (Изтегли видео от демото тук: https://drive.google.com/file/d/0Bw_RSgw8AS5IUWV0QTRxdXBmUFE/view?usp=sharing ) и започнахме да подготвяме working prototype за идеята върху тях. 
Набелязахме нещата, които можем да използваме:
- motion detection алгоритми - за да знаем къде има нещо по игрището
- text/letter recognition или object recognition за разпознаване на въпросното от по-горе "нещо" кой играч всъщност е
- Няколко метода предлагани от библиотеката за изготвяне на translation matrix за коригиране на изкривяванията на камерата и преобразуване на координатите на играчите от видео образа към виртуалното 2D игрище.

Набелязахме и доста corner cases, който _няма_ да предвиждаме в demo-то за Sprint 1: 
Multi user - след разпознаване на играчите трябва да ги следим и да "знаем" играч къде се намира. Съответно когато играч е пред/зад друг играч трябва да се разпознаят няколко отделни обекта. Един възможен начин за справяне с тази ситуация е наличие на няколко камери - взаимодействие/синхронизация между тях за разпознаване на максимален брой обекти в сцената, както и коригиране (average) на position данните от едната камера с данните от другата. Възможно е и да се разпишат алгоритми за намиране на "last known position" на играчите, която да се използва за estimate-ване на текущата позиция на играч в кадрите, където камерата не го вижда, спрямо последните карди в които го е виждала, и/или бъдещи кадри, в които ще го види. Разпознаване на топки, съдии и други обекти по полето, които не са ни важни.

### Ден пети

Последния ден от спринта. Моментът на истината. В началото на деня повечето компоненти от работещото демо (ограничено за един играч в полето #foreveralone и една снимаща го камера (без кожен диван)) бяха почти работещи. В по-реалните тестове открихме, че letter recognition-а не работи добре ако нямаме адски много samples, с които да обучим системата да намира цифри (разбирайте цифрата 9 снимана под всякакви възможни ъгли), изпробвахме object tracking предлаган от библиотеката, но там резултатите бяха неточни и още по-неточни ако има много детайли в картината. На този етап имаше нужда от повече research за други начини (или доусъвършенстване на тези) за разпознаване на играчите в игрището и във финалното демо тази функционалност не е представена. Доусъвършенствахме тук-там алгоритмите - running average на координатите и error correction за false-positive detection, поизпипахме визуално демото (да чертае позицията на играча, да не забиваме координатите на игрището и т.н.) и по план искахме да изготвим това обобщение + презентация.

### Обобщение от Спринт 1
В крайна сметка успяхме да докараме работещ прототип, включващ следене и heatmap на играч върху игрището. Чрез motion tracking намерихме обекта, транслирахме координатите му върху игралното поле и изчертахме движенията му. Прецизността на намиране на точно местоположение в демо видеото, което използвахме, беше в рамките на сантиметри. За разпознаване на обекта успяхме да си извадим няколко извода - вграденият object detection не работи достатъчно добре дори и след малко донастройване, разпознаването на цифри в полето има по-голям потенциал, но изисква наличие на много sample снимки (на цифрите), които да обучат системата за да разпознава добре числата.

### Ами сега?
Непременно първата стъпка в следващия спринт трябва да бъде идентифициране на разпознатия обект - кой играч е, от кой отбор е и т.н., като тук има доста challenge и все още неизследвани опции. Процеса по разпознаване на играчите е своя собствена наука и евентуален следващ спринт би валидирал посоки, в които може да продължи той. След постигане на резултати там следва работа по различните алгоритми - разпознаване на повече от един играч в полето и предвиждане на местоположението на играчите когато то не е налично, обработка и синхронизация на кадрите от няколко камери, разпознаване на топка, голи фенове и други обекти по игрището и изготвяне на алгоритми по разпознаване на различните интеракции между играчите - пасове, удъри, хвърляния, мятания, както и разпознаване на събития по терена - голове, молове, тъчове, мъчове, ъглови удъри, фалове и други.

Вратата тук остава широко отворена за спринт 2, с доста неизследвани територии и challenges.