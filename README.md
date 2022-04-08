# Auto-ambient Doom

A hightly customizable (G-/Q-/L-)ZDoom modification which lets you to dispose ambient sounds/actors to the walls.

Гибко настраиваемая модификация для (G-/Q-/L-)ZDoom, позволяющая автоматически располагать звуки или объекты окружения на стены уровней.

- [English](#en-sector)
  - [Description](#en-description)
  - [Options](#en-settings)
  - [Configuration file](#en-config-file)
  - [Credits](#en-credits)

- [Русский](#ru-sector)
  - [Описание](#ru-description)
  - [Настройки](#ru-settings)
  - [Файл конфигурации](#ru-config-file)
  - [Авторы и благодарности](#ru-credits)




<a name="en-sector"></a>

# English

<a name="en-description"></a>

## Description

The essence of the modification is simple. For actors (in-game objects in Doom), sounds are added [elementarily](https://zdoom.org/wiki/A_PlaySound); there are also [built-in mechanisms for flats](https://zdoom.org/wiki/SNDSEQ). And nothing for walls. I mean, absolutely nothing. No one attempt in the whole Internet.

So, Auto-ambient Doom is a modification which automatically places specified actors (by default an ambient sounds like lumeniscent lamps buzzing or ducts)  to the walls textures.


**Minimal required GZDoom engine version is v3.3.0**. Maximal isn't regulated.

Compatibility with all the modifications I have tested is not violated.


When the game starts, the modification reads the last specified configuration text file (see [configuration section](#en-config-file)) and handles it, parsing settings. It uses them every time a new level is loaded: in fact, it goes through the entire map, creating environment objects on the walls where necessary.

A release usually comes in two flavors: "`AAmbient_vXYZ.pk3`" includes by default some ambient objects and prewritten configuration file, and "`AAmbient_vXYZ_core_only.pk3`" is a pure project core.

##### Launching

You may drag Auto-ambient modification file(s) to the `gzdoom.exe` (`lzdoom.exe`, `qzdoom.exe`) on Windows or to the `gzdoom` on Linux/MacOS.

Also you may use a console command on Windows:

`gzdoom.exe -file AAmbient_v*.pk3`, where wildcard (asterisk) is a downloaded modification version;

Or launch under a Linux/MacOS terminal:

`gzdoom -file AAmbient_v*.pk3` with the same note as above.





<a name="en-settings"></a>

## Options

Located in "Options"→"Auto-ambient control".

[WIP section]


<a name="en-config-file"></a>

## Configuration file

[WIP section]


### Configuration file example

Compiled from the default `doom2.cfg` and files included to it.

```
# Special sound actors (not just looped): =====================================

soundactor snd_acid {
	sound AA/Acid/loopA
	mode random 20 175
	volume 0.4
	attn 3.8
}

soundactor default {
	mode fixed 35
	volume 0.4
	attn 5.0
}

# Both has mode "fixed, 35" and volume/attenuation of "0.4"/"5.0".
soundactor snd_comp1 {
	sound AA/Computers/Beep
}
soundactor snd_comp2 {
	sound AA/Computers/Boop
}


# Groups: =====================================================================

group default {
	mindistance 128
}

group Acid {
	# Minimal distance is 128 (from a default definition bit above).
	soundactor snd_acid
}

group Blazing {
	# Minimal distance is 128.
	loopedsound AA/Blazing/loop1 0.65 4.0
	loopedsound AA/Blazing/loop2 0.65 4.0
	loopedsound AA/Blazing/loop3 0.5 3.8
}

group Computers {
	# Minimal distance still 128.
	soundactor snd_comp1 snd_comp2
}

group Tech {
	mindistance 160 # Distance overridden, so here it equals to 160.
	loopedsound AA/Tech/loopA 0.7 3.5
	loopedsound AA/Tech/loop1 0.7 3.5
	loopedsound AA/Tech/loop2 0.7 3.5
	loopedsound AA/Tech/loop3 1.0 3.5
	loopedsound AA/Tech/loop4 0.3 4.5
}


# Textures parameters: ========================================================

textureparam somewhereAtTopHalf {
	# To spawn somewhere in the top half.
	start -50 -75
	random -48 -24.5
}

textureparam default {
		start -50 -50
		random -50 -50
	}
	textureparam somewhere100 {}
	textureparam somewhere50 {
		chance 0.5
	}

	# No similar indentation is neccesary... But with it, the default 
	#blocks logic is easier to perceive.
	textureparam default {
		random 0.0
}

textureparam "Cement Tech" {
	# Just specialized parameters for the "CEMENT[24]" texture.
	start -57 -70
	random -17 -26
	chance 0.67 # Sometimes (about a third of the time) nothing will appear.
}

textureparam verticalSplit {
	# Dividing in two vertically.
	# 25% start + 50% offset: objects on the X axis will be on 25% and 
	#on 75% from texture height.
	start -50 -25
	offset -100 -50
	random -10 -10
}


# Textures: ===================================================================

texture BRICKLIT BSTONE3 {
	# Oil bowl at the top of the texture (50% by X, 75% by Y):
	group Blazing -50 -75
}

texture CEMENT[24] {
	group Tech "Cement Tech" # Wires.
	belowdistchance 0.8 0.8 # Let it sparkle nearby!
}

texture COMPSTA[12] {
	# It is possible to use more than one groups for the texture:

	group Computers somewhereAtTopHalf
	group Tech somewhereAtTopHalf
}

texture SFALL[1234] {
	group Acid somewhere100
	group Acid somewhere50
	group Blazing somewhere50
}

texture SILVER3 {
	# Sometimes it is more convenient to use a regular rather than a 
	#percentage offset.
	group Tech 19 32
	group Computers 38 96
}

texture SPACEW3 {
	group Computers verticalSplit
}
```



<a name="en-credits"></a>

## Credits

Idea, core code, default content: Morthimer McMare.

Special thanks to:

- Renaul Damek for the moral support;
- YURA_111 for the video and bug reportings (commit dc41772).
- endoomer for the overview;
- ika707 for the bug reportings (commit 5560bf7).



---

<a name="ru-sector"></a>

# Русский

<a name="ru-description"></a>

## Описание

 Суть модификации проста. Для акторов (внутриигровых объектов в Doom) звуки добавляются [элементарно](https://zdoom.org/wiki/A_PlaySound). Для полов/потолков также существуют [встроенные механизмы](https://zdoom.org/wiki/SNDSEQ)... А для текстур на стенах — ничего. В смысле, вообще ничего — ни единой попытки на просторах всего Интернета.

 Итак. Auto-ambient Doom — модификация, позволяющая в автоматическом порядке разместить указанные объекты (по умолчанию — звуки окружения вроде жужжания люминесцентных ламп или вентиляции) на текстуры стен.


 **Минимальная версия движка — GZDoom v3.3.0**. Максимальная не регламентирована.

 Совместимость со всеми проверенными мной модификациями не нарушается.


 При запуске игры модификация считывает последний указанный текстовый файл конфигурации ([см.](#ru-config-file)) и обрабатывает его, извлекая настройки. Использует она их при каждой загрузке нового уровня: собственно, проходится по всей карте, создавая на стенах там, где необходимо, объекты окружения.

 Обычно релиз поставляется в двух вариациях: "`AAmbient_vXYZ.pk3`" по умолчанию включает в себя какие-либо объекты и уже настроенный конфигурационный файл, тогда как "`AAmbient_vXYZ_core_only.pk3`" является чистым ядром проекта.

##### Запуск

 Для запуска под Windows Вы можете мышкой перетащить файл на `gzdoom.exe` (`lzdoom.exe`, `qzdoom.exe`). Для запуска из-под командной строки необходимо воспользоваться командой (подставьте скачанную версию Auto-ambient на место шаблона):

`gzdoom.exe -file AAmbient_v*.pk3`

 Для Linux и MacOS запуск производится практически также.
 Отличия от Windows: `gzdoom` или иной движок семьи ZDoom не обязан иметь расширения, но должен иметь бит разрешения запуска; ну и среда называется терминалом, а не командной строкой. Всё.




<a name="ru-settings"></a>

## Настройки

Открываются в меню "Options"→"Auto-ambient control".

#### "Modification enabled"

 Отвечает за то, загружается ли модификация вообще. Полезна, например, если кто-нибудь включил эту модификацию в свою сборку, а она Вам не нужна.

 Доступные значения: "Yes" ("Да")/"No" ("Нет").

#### "Show automatic actors"/"Hide automatic actors"

 Вспомогательные функции, пригождающиеся при картостроении. Позволяют отобразить/скрыть на уровне все автоматические объекты, созданные модификацией.

#### "Show actors at start"

 Также вспомогательная функция для отладки, позволяет сразу после загрузки уровня отобразить все автоматические объекты.

 Доступные значения: "Yes" ("Да")/"No" ("Нет").

#### "Reserved channels for other sounds"

 Сколько звуковых каналов резервировать под все другие звуки мира (напрямую коррелирует с "Options"→"Sound options"→"Sound channels").

 Доступные значения слайдера: от 24 до 128 звуков, по умолчанию — 32.

#### "Log level"

 Уровень журналирования (уровень лога) действий модификации.

 Доступные значения:
- "Emergency only" ("Только аварийные ситуации"). Записывает только критические нарушения в работе модификации. Использовать не советую, но мало ли, какие ситуации бывают...
- "Main" ("Основной", стоит по умолчанию). Чуть более подробный вывод. Вполне подходит для обычной игры.
- "Detailed" ("Детализированный"). Крайне удобен для отладки во время написания своего файла конфигурации.
- "Debug" ("Режим отладки"). Помимо всего предыдущего выводит также много внутренней информации мода. **Если Вы встретили баг — лучше всего передавать его на рассмотрение вместе с выводом этого уровня журналирования!**

#### "Show parsed data on load"

 Показывать или нет полученные из файлов конфигурации данные при загрузке игры. Может быть полезно для отладки собственных модификаций.

 Для работы необходим уровень журналирования "Detailed" или "Debug".

 Доступные значения: "Yes" ("Да")/"No" ("Нет"). По умолчанию — "Нет".

#### "Destroy auto-ambient actors"

 Удаляет с карты все автоматически созданные объекты Auto-Ambient.


---

<a name="ru-config-file"></a>

## Файл конфигурации

Считывается последний конфигурационный lump вида "`aambient*`", на месте астериска — любые символы (например, расширение). Регистр названия не важен. В этом файле автоматически распознаётся тип переносов строк (для *nix, DOS/Windows, MacOS, DOS/Windows reversed), поэтому мод даже в этом не зависит от используемой ОС.

Однострочный комментарий начинается с символа "#".

Содержимое файла также регистронезависимо (в итоге приводится к строчному виду).

#### Глобальные ключевые слова

Эти кейворды используются вне каких-либо блоков.

"`include <lumppath>`" подключает дополнительный файл по пути `<lumppath>`, если файл не найден — прерывает парсинг с ошибкой.

"`includeoptional <lumppath>`" также подключает дополнительный файл по пути `<lumppath>`, но при его отсутствии дальнейшее распознавание не срывается.

_"`includeprev`" (неотлаженная, используйте на свой страх и риск!)_. Добавляет файл с таким же названием из предыдущего загруженного архива, если он там существует. Если нет — ничего страшного не происходит, команда игнорируется.



"`mapsdefaulttextures allow|on|deny|off <mapname1> [<mapname2> [...]]`" разрешает (`allow`/`on`) или запрещает (`deny`/`off`) использовать стандартные текстуры для всех карт с указанными названиями `<mapname*>`. При повторной встрече конструкции записывается последнее значение.

"`maphashesdefaulttextures allow|on|deny|off <mapname1> [<mapname2> [...]]`" — аналогично предыдущему, но работает с контрольными суммами карт.

#### Объявление автоматических звуковых авторов

Синтаксис со всеми возможными комбинациями слов:

```
soundactor <soundactor_name>|default {
	sound <alias>
	mode looped
	mode fixed <tics>
	mode random <mintics> <maxtics>
	volume <volume>
	attn <attenuation>
}
```

"`<soundactor_name>`" — название определения для звукового актора, используется в группах.

Ключевое слово "`default`" в заголовке означает, что до следующего переопределения умолчаний параметры `mode`, `volume` и `attn` во всех блоках такого типа изначально будут присвоены указанным здесь значениям.



"`sound <alias>`" определяет, какой звук из `SNDINFO` будет проигрываться.

"`mode looped|fixed <tics>|random <mintics> <maxtics>`" — способ проигрывания звука:
- "`looped`" отвечает за запуск простого зацикленного звука. В начале предусмотрена случайная задержка от нуля до пяти тактов.
- "`fixed <tics>`" проигрывает указанный звук раз в `<tics>` тактов (1/35 секунды). Первый звук проигрывается с задержкой от нуля до `<tics>` тактов.
- "`random <mintics> <maxtics>`" проигрывает указанный звук, после каждого воспроизведения устанавливая новую задержку от `<mintics>` до `<maxtics>`.

"`volume <volume>`" устанавливает громкость звуку; значения должны лежать в пределах `[0.0; 1.0]`. Перемножается с `$volume` в `SNDINFO`.

"`attn <attenuation>`" устанавливает коэффициет затухания звуку при удалении игрока от него (`0.0` — не затухает вовсе, `1.0` — нормальное значение, более единицы — затухает быстрее). Перемножается с одноимённым параметром в `SNDINFO`.

#### Объявление групп

Синтаксис со всеми возможными комбинациями слов:

```
group <group_name>|default {
	mindistance <value>
	loopedsound <alias> [<volume> [<attenuation>]]
	soundactor <soundactor1> [<soundactor2> [...]]
	actor <class1> [<class2> [...]]
}
```

"`<group_name>`" — название определения для групп, используется в текстурах.

Ключевое слово "`default`" в заголовке означает, что до следующего переопределения умолчаний параметр `mindistance` (и только он!) во всех блоках такого типа изначально будет присвоен указанному здесь значению.



"`mindistance`" определяет, насколько близко акторы этой группы могут появиться друг рядом с другом.

"`loopedsound <alias> [<volume> [<attenuation>]]`" объявляет простой звуковой актор со звуком `<alias>`, громкостью `<volume>` (по умолчанию — `1.0`) и коэффициентом затухания `<attenuation>` (по умолчанию также `1.0`).

"`soundactor <soundactor1> [<soundactor2> [...]]`" добавляет в группу ранее созданного(-ых) звукового(-ых) актора(-ов) (см. "Объявление автоматических звуковых авторов"). Если какого-то объявления не встречалось до этого, прерывает парсинг.

"`actor <class1> [<class2> [...]]`" добавляет в группу любые классы акторов наравне со специальными. ~~Имп в стенке — чем не развлечение?~~

#### Объявление параметров соединения групп с текстурами

Синтаксис со всеми возможными комбинациями слов:

```
textureparam <texparam_name>|default {
	start <x> <y>
	offset <x> <y>
	random <x> <y>
	chance <chance>
}
```

"`<texparam_name>`" — название определения для параметров, используется в текстурах.

Ключевое слово "`default`" в заголовке означает, что до следующего переопределения умолчаний все параметры во всех блоках такого типа изначально будут присвоены указанным здесь значениям.



_Далее считается, что координата "0"/"0" — левый-нижний угол текстуры, а сама она располагается в первом квадранте декартовой плоскости (орт — один пиксель). Процент смещения по текстуре задаётся отрицательными значениями, то есть запись "-50.0" — это "50%"._

"`start <x> <y>`" задаёт начальную позицию для спауна. По умолчанию — "`-50 -50`" (центр текстуры).

"`offset <x> <y>`" изменяет то, насколько спаун-позиция смещается по текстуре. По умолчанию — "`-100 -100`" (смещение на 100%/100%).

"`random <x> [<y>]`" добавляет отклонение к точке появления. Если задан только один параметр, то он применяется и для X, и для Y. По умолчанию смещения нет.

"`chance <chance>`" позволяет указать шанс на появление объекта; значения должны лежать в пределах `[0.0; 1.0]`. По умолчанию — "`1.0`", то есть актор появляется всегда. Применяется после всех статических фильтров, но до проверки на слишком малое расстояние до предыдущих объектов.

#### Объявление текстур

Синтаксис со всеми возможными комбинациями слов:

```
texture default|<texture_name1> [<texture_name2> [...]] {
	group <group_name> <texparam_name>
	group <group_name> <x> <y>
	belowdistchance <onsameline> <onotherlines>

	setflag <flagname>
	+f <flagname>
	resetflag <flagname>
	-f <flagname>

	onmaps <mapname1> [<mapname2> [...]]
	onmaphashes <maphash1> [<maphash2> [...]]
}
```

"`<texture_name*>`" — названия текстур, для которых нужно применить этот блок; если их не существует, то парсер прерывает работу. Реализована обработка простого регулярного выражения: каждый символ, который находится внутри квадратных скобок, будет подставлен в свою текстуру. Например, "`GSTFONT[123]`" превратится в три текстуры "`GSTFONT1`", "`GSTFONT2`" и "`GSTFONT3`", а "`TEST[AB][4Y]`" — в "`TESTA4`", "`TESTAY`", "`TESTB4`" и "`TESTBY`".

Ключевое слово "`default`" в заголовке означает, что до следующего переопределения умолчаний параметры `belowdistchance`, флаги текстур, группы, карты и хэш-суммы карт во всех блоках такого типа изначально будет присвоены указанным здесь значениям; притом группы, карты и хэш-суммы карт сначала удаляются, если в них впоследствии было указано хоть что-то.



"`group <group_name> <texparam_name>`" добавляет группу `<group_name>` с параметрами привязки `<texparam_name>`.

"`group <group_name> <x> <y>`" добавляет группу `<group_name>` по смещению `<x> <y>` (см. примечение к "Объявлению параметров соединения групп с текстурами").

"`belowdistchance <onsameline> <onotherlines>`" позволяет чуть более филигранно управлять возможностью создания акторов, даже если проверка расстояния между ними не уложилась в параметр группы `mindistance`. Первый аргумент отвечает за шанс такого появления на этой же линии, второй — за шанс появления на любой другой; оба значения должны находиться в диапазоне `[0.0; 1.0]`. По умолчанию — "`0.0 0.0`" (всё-таки не создавать).

"`setflag <flagname>`" или "`+f <flagname>`" устанавливает флаг текстуры:
- "`handlecommonlines`" отвечает за обработку обычных линий (`action = 0`; установлен по умолчанию);
- "`handleactionlines`" отвечает за обработку линий с действием (`action ≠ 0`; установлен по умолчанию);
- "`samelinedistmultipl`" или "`selfdistmultipl`" меняет смысл первого агрумента для `belowdistchance`. С этим флагом на шанс в основном влияет расстояние до предыдущих объектов (чем ближе — тем меньше вероятность его появления).
- "`otherlinesdistmultipl`" или "`othersdistmultipl`" аналогичным образом меняет смысл второго агрумента для `belowdistchance`.
- "`alllinesdistmultipl`" или "`usedistmultipl`" устанавливает сразу и `samelinedistmultipl`, и `otherlinesdistmultipl`.

"`resetflag <flagname>`" или "`-f <flagname>`" снимает подобный флаг.

"`onmaps <mapname1> [<mapname2> [...]]`" привязывает определение текстуры к названию(-ям) уровня(-ей). Такие привязанные текстуры имеют более высокий приоритет, чем те, что присутствуют на всех уровнях: если найдётся и текстура, привязанная к карте, и глобальная, то вторая будет просто проигнорирована.

"`onmaphashes <maphash1> [<maphash2> [...]]`" — аналогично предыдущему, но для контрольных сумм карт.



### Пример конфигурационного файла

Скомпилирован из стандартного `doom2.cfg` и подключённых к нему файлов.

```
# Специальные звуковые акторы (не просто зацикленные): ========================

soundactor snd_acid {
	sound AA/Acid/loopA
	mode random 20 175
	volume 0.4
	attn 3.8
}

soundactor default {
	mode fixed 35
	volume 0.4
	attn 5.0
}

# В обоих: режим — "fixed, 35", громкость/затухание — 0.4/5.0.
soundactor snd_comp1 {
	sound AA/Computers/Beep
}
soundactor snd_comp2 {
	sound AA/Computers/Boop
}


# Группы: =====================================================================

group default {
	mindistance 128
}

group Acid {
	# Минимальная дистанция — 128 (с определения умолчаний чуть выше).
	soundactor snd_acid
}

group Blazing {
	# Минимальная дистанция — 128.
	loopedsound AA/Blazing/loop1 0.65 4.0
	loopedsound AA/Blazing/loop2 0.65 4.0
	loopedsound AA/Blazing/loop3 0.5 3.8
}

group Computers {
	# Минимальная дистанция — 128.
	soundactor snd_comp1 snd_comp2
}

group Tech {
	mindistance 160 # Дистанция переопределена, тут она равна 160.
	loopedsound AA/Tech/loopA 0.7 3.5
	loopedsound AA/Tech/loop1 0.7 3.5
	loopedsound AA/Tech/loop2 0.7 3.5
	loopedsound AA/Tech/loop3 1.0 3.5
	loopedsound AA/Tech/loop4 0.3 4.5
}


# Параметры текстур: ==========================================================

textureparam somewhereAtTopHalf {
	# Появляться где-то в верхней половине текстуры.
	start -50 -75
	random -48 -24.5
}

textureparam default {
		start -50 -50
		random -50 -50
	}
	textureparam somewhere100 {}
	textureparam somewhere50 {
		chance 0.5
	}

	# Подобный отступ не необходим... Но с ним проще воспринимается
	#логика default-блоков.
	textureparam default {
		random 0.0
}

textureparam "Cement Tech" {
	# Просто специализированные параметры под реалии текстуры "CEMENT[24]":
	start -57 -70
	random -17 -26
	chance 0.67 # Иногда (примерно в трети случаев) ничего не появится.
}

textureparam verticalSplit {
	# Деление надвое по вертикали.
	#25% старт + 50% смещение: объекты по X будут на 25% и на 75% от текстуры.
	start -50 -25
	offset -100 -50
	random -10 -10
}


# Текстуры: ===================================================================

texture BRICKLIT BSTONE3 {
	# Масляная плошка в верхней части текстуры (50% по X, 75% по Y):
	group Blazing -50 -75
}

texture CEMENT[24] {
	group Tech "Cement Tech" # Переплетение проводов.
	belowdistchance 0.8 0.8 # Пусть и рядом искрит!
}

texture COMPSTA[12] {
	# Можно использовать более одной группы для текстуры:

	group Computers somewhereAtTopHalf
	group Tech somewhereAtTopHalf
}

texture SFALL[1234] {
	group Acid somewhere100
	group Acid somewhere50
	group Blazing somewhere50
}

texture SILVER3 {
	# Иногда удобнее использовать обычное, а не процентное смещение.
	group Tech 19 32
	group Computers 38 96
}

texture SPACEW3 {
	group Computers verticalSplit
}
```


<a name="ru-credits"></a>

## Авторы и благодарности

Идея, код ядра, содержимое по умолчанию: Morthimer McMare.

Отдельное спасибо:

- Renaul Damek за моральную поддержку;
- YURA_111 за запись видео и сообщение об ошибке (коммит dc41772);
- endoomer за обзор;
- ika707 за сообщение об ошибке (коммит 5560bf7).

